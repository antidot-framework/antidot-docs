# Routing

The routing system gives two levels of pipelines, [Global Pipeline](/framework/#Global-Pipeline), and [Routed Pipeline](/framework/#Routed-Pipeline)

## Global Pipeline

Consist in a middleware collection that encapsulates complete lifecycle from the begin of the HTTP Request to the end of the HTTP Response

This image from [Slim Framework](http://www.slimframework.com/docs/v3/concepts/middleware.html) documentation describes perfectly the Middleware pattern

![Middleware](/../images/middleware.png)

The config for the Global Pipeline is placed at `router/middleware.php` file.

````php
<?php
// router/middleware.php

declare(strict_types=1);

use Antidot\Application\Http\Application;
use Antidot\Application\Http\Middleware\RouteDispatcherMiddleware;
use Antidot\Application\Http\Middleware\RouteNotFoundMiddleware;
use Antidot\Logger\Application\Http\Middleware\ExceptionLoggerMiddleware;
use Antidot\Logger\Application\Http\Middleware\RequestLoggerMiddleware;

return static function (Application $app) : void {
    $app->pipe(ExceptionLoggerMiddleware::class);
    $app->pipe(RequestLoggerMiddleware::class);
    // Your Server Request Middleware could gone here.
    // $app->pipe(MyCustomMiddlewareMiddleware::class);
    $app->pipe(RouteDispatcherMiddleware::class);
    $app->pipe(RouteNotFoundMiddleware::class);
};
````

You can add PSR-15 middleware implementations made by the community or made by you. See a list off community [PSR-15 Middleware implementations](https://github.com/middlewares/psr15-middlewares) or make a search in [Packagist](https://packagist.org/?query=psr-15)

````php
<?php
// src/Application/Http/Middleware/SomeMiddleware.php

declare(strict_types=1);

namespace App\Application\Http\Middleware;

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\MiddlewareInterface;
use Psr\Http\Server\RequestHandlerInterface;

class SomeMiddleware implements MiddlewareInterface
{
    public function process(ServerRequestInterface $request, RequestHandlerInterface $handler): ResponseInterface
    {
        return $handler->handle($request);
    }
}
````

## Routed Pipeline

The **Routed Pipeline** allows us to add specific pipelines to certain routes that will run inside the global pipeline

The configuration file for Routed Pipeline can be found at `router/routes.php`

````php
<?php
// router/routes.php

declare(strict_types=1);

use Antidot\Application\Http\Application;
use App\Application\Http\Handler\HomePageRequestHandler;
use App\Application\Http\Middleware\SomeMiddleware;
use Psr\Container\ContainerInterface;

/**
 * Setup routes with a single request method:
 *
 * $app->get('/', App\Handler\HomePageHandler::class, 'home');
 * $app->post('/album', App\Handler\AlbumCreateHandler::class, 'album.create');
 * $app->put('/album/:id', App\Handler\AlbumUpdateHandler::class, 'album.put');
 * $app->patch('/album/:id', App\Handler\AlbumUpdateHandler::class, 'album.patch');
 * $app->delete('/album/:id', App\Handler\AlbumDeleteHandler::class, 'album.delete');
 *
 * Or with multiple request methods:
 *
 * $app->route('/contact', App\Handler\ContactHandler::class, ['GET', 'POST', ...], 'contact');
 */
return static function (Application $app, ContainerInterface $container) : void {
    $app->get('/', [SomeMiddleware::class, HomePageRequestHandler::class], 'home');
};

````

The Request Handler must be an implementation of `Psr\RequestHandlerInterface`

````php
<?php

declare(strict_types=1);

namespace App\Application\Http\Handler;

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Zend\Diactoros\Response\HtmlResponse;

class SomeRequestHandler implements RequestHandlerInterface
{
    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        return new HtmlResponse('<html><body><h1>Hello World!!!</h1></body></html>');
    }
}
````

> The middleware instances and request handler should be registered as services with [dependency injection](/#Dependency-Injection) component
