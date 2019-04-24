Antidot Framework
=================

This framework is based on concepts and components of other open source software, especially [Zend Request Handler Runner](https://docs.zendframework.com/zend-httphandlerrunner/), [Zend Expressive](https://docs.zendframework.com/zend-expressive/) and [Zend Stratigillity](https://docs.zendframework.com/zend-stratigility/).

## Key Features

* **Preconfigured Coding Style**: [Psr-1](https://www.php-fig.org/psr/psr-1) and [Psr-2](https://www.php-fig.org/psr/psr-2) code sniffer to help to respect standard
* **Logger**: [Psr-3](https://www.php-fig.org/psr/psr-3) implementation by [wshafer/monolog](https://github.com/wshafer/psr11-monolog)
* **Auto-loading**: [Psr-4](https://www.php-fig.org/psr/psr-4) Namespaces auto-loading
* **Request Response Lifecycle**: [Psr-7](https://www.php-fig.org/psr/psr-7) Request and responses using [Zend Diactoros](https://docs.zendframework.com/zend-diactoros/)
* **Auto-wired Dependency injection**: [Psr-11](https://www.php-fig.org/psr/psr-11) Container implemented by [Aura/Di](https://github.com/auraphp/Aura.Di)
* **Event Dispatcher**: [Psr-14](https://www.php-fig.org/psr/psr-14) Event dispatching system
* **Request pipeline**: [Psr-15](https://www.php-fig.org/psr/psr-15) Request handler and Middleware
* ***Pipeline based router**: Intuitive to use route system on top of [Aura/Router](https://github.com/auraphp/Aura.Router)
* **Different Config Translators**: [Zend config](https://github.com/zendframework/zend-config) style or [Symfony](https://symfony.com/doc/current/best_practices/configuration.html) style
* **Cli**: Ready to use Console Line Tool by [Symfony Console Tool]()

## Getting Started

### Installation

> https://github.com/antidot-framework/antidot-starter

Using [composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos) package manager

````bash
composer create-project antidot-fw/antidot-framework-starter:dev-master project-name
````

### File Structure

````
bin/console
config/config.php
config/config-cli.php
config/container.php
config/container-cli.php
config/autoload/dependencies.prod.yaml
public/index.php
router/middleware.php
router/routes.php
src/
test/
var/cache/*.php
var/log/*.log
.gitignore
composer.json
grumphp.yml
phpcs.xml.dist
phpunit.xml.dist
README.md

````

## Running Application 

> https://github.com/antidot-framework/antidot-framework

### Running PHP build in web server

PHP provides us a built-in web server which is cool for development purposes

````bash
php -S 127.0.0.1 -t public
````
![running built in server](/images/php-built-in-server.jpg)

## Routing

The routing system gives two levels of pipelines, [Global Pipeline](/#Global-Pipeline), and [Routed Pipeline](/#Routed-Pipeline)

### Global Pipeline

Consist in a middleware collection that encapsulates complete lifecycle from the begin of the HTTP Request to the end of the HTTP Response

This image from [Slim Framework](http://www.slimframework.com/docs/v3/concepts/middleware.html) documentation describes perfectly the Middleware pattern

![Middleware](/images/middleware.png)

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

### Routed Pipeline

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

## Dependency Injection

> https://github.com/antidot-framework/antidot-symfony-config-translator

Auto wired dependency injection system, the only thing you have to do is declare dependencies in `config/autoload/` directory

### Auto-wired Invokables

<!-- tabs:start -->

#### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
services:
  Full\Qualified\ClassName:  

````

#### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
dependencies:
  invokables:
    - Full\Qualified\ClassName: Full\Qualified\ClassName

````

#### ** Symfony style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'services' => [
        Full\Qualified\ClassName::class => [
            'class' => Full\Qualified\ClassName::class
        ],
    ]
];
````

#### ** Zend style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'dependencies' => [
        'invokables' => [
            Full\Qualified\ClassName::class => Full\Qualified\ClassName::class,
        ]
    ]
];
````

<!-- tabs:end -->

### Factory Classes

<!-- tabs:start -->

#### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
services:
  Full\Qualified\ClassName:  
    factory: [Full\Qualified\ClassNameFactory, '__invoke']

````

#### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
dependencies:
  factories:
    - Full\Qualified\ClassName: Full\Qualified\ClassNameFactory

````

#### ** Symfony style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'services' => [
        Full\Qualified\ClassName::class => [
            'factory' => [Full\Qualified\ClassNameFactory::class, '__invoke']
        ],
    ]
];
````

#### ** Zend style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'dependencies' => [
        'factories' => [
            Full\Qualified\ClassName::class => Full\Qualified\ClassNameFactory::class,
        ]
    ]
];
````

<!-- tabs:end -->

### Complex classes with arguments

<!-- tabs:start -->

#### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
services:
  Full\Qualified\ClassName:  
    arguments:
      $foo: '@SomeService'
      $bar: '%config.some_parameter%'

````

#### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
dependencies:
    conditionals:
      Full\Qualified\ClassName:
        class: Full\Qualified\ClassName
        arguments: 
          foo: SomeService
          bar: 'Some.String'

````

#### ** Symfony style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'services' => [
        Full\Qualified\ClassName::class => [
            'arguments' => [
                '$foo' => Full\Qualified\ClassNameFactory::class,
                '$bar' => 'some.string',
            ]
        ],
    ]
];
````

#### ** Zend style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'dependencies' => [
        'conditionals' => [
            Full\Qualified\ClassName::class => [
                'class' => Full\Qualified\ClassName::class,
                'arguments' => [
                    '$foo' => Full\Qualified\ClassNameFactory::class,
                    '$bar' => 'some.string',
                ]
            ],
        ]
    ]
];
````

<!-- tabs:end -->

### Implementing Interfaces

<!-- tabs:start -->

#### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
services:
  Full\Qualified\ClassNameInterface:
    class: Full\Qualified\ClassName
  # Or using aliases
  Full\Qualified\AnotherClassNameInterface:
    alias: 'service.name'
````

#### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
dependencies:
  invokables:
    Full\Qualified\ClassNameInterface: Full\Qualified\ClassName
  # Or using aliases
  aliases:
    Full\Qualified\AnotherClassNameInterface: 'service.name'
````

#### ** Symfony style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'services' => [
        Full\Qualified\ClassNameInterface::class => [
            'class' => Full\Qualified\ClassName::class
        ],
        // or using alias
        Full\Qualified\ClassNameInterface::class => [
            'alias' => 'service.name'
        ],
    ]
];
````

#### ** Zend style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'dependencies' => [
        'invokables' => [
            Full\Qualified\ClassNameInterface::class => Full\Qualified\ClassName::class
        ],
        // or using alias
        'aliases' => [
            Full\Qualified\ClassNameInterface::class => 'service.name'
        ],
    ]
];
````

<!-- tabs:end -->

### Declaring services

<!-- tabs:start -->

#### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
services:
  service.name:
    class: Full\Qualified\ClassName

````

#### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
dependencies:
  services: 
    service.name: Full\Qualified\ClassName

````

#### ** Symfony style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'services' => [
        'some.service' => [
            'class' => Full\Qualified\ClassName::class
        ],
    ]
];
````

#### ** Zend style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'dependencies' => [
        'services' => [
            'some.service' => Full\Qualified\ClassName::class,
        ]
    ]
];
````

<!-- tabs:end -->

## Config

The configuration system uses Zend Config Aggregator strategy, this allows us to create complex configs and dependencies depending on the environment, by default it loads first PHP config files and then it parses YAML files merging all in a unique array of data who is available inside the container keyed by `config`

````php
<?php

/** @var Psr\ContainerInterface $config */
$config = $container->get('config');
````


<!-- tabs:start -->

#### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
parameters:
  some.key: Some value
  other.key: 
    array: of values
    array1: 
      item1: value
      item2: value

````

#### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
some.key: Some value
other.key: 
  array: of values
  array1: 
    item1: value
    item2: value

````

#### ** Symfony style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'parameters' => [
        'some.key' => 'Some value',
        'other.key' => [
            'array' => 'of values',
            'array1' => [
                'item1' => 'value',
                'item2' => 'value',
            ]
        ]
    ]
];
````

#### ** Zend style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'some.key' => 'Some value',
    'other.key' => [
        'array' => 'of values',
        'array1' => [
            'item1' => 'value',
            'item2' => 'value',
        ]
    ]
];
````

<!-- tabs:end -->

## Logger

> https://github.com/antidot-framework/antidot-logger

[Psr 3 Logger](https://github.com/php-fig/logger) implementation by Monolog.

The logger is available as `Psr\Log\LoggerInterface` you can inject in your services constructors out of the box.

### Config

<!-- tabs:start -->

#### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
parameters:
  monolog:
    handlers:
      default:
        type: 'stream'
        options:
          stream: 'var/log/%date%-default.log'
          level: 400
````

#### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
monolog:
  handlers:
    default:
      type: 'stream'
      options:
        stream: 'var/log/%date%-default.log'
        level: 400
````

#### ** Symfony style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

use Monolog\Logger;

return [
    'parameters' => [
        'monolog' => [
            'handlers' => [
                'default' => [
                    'type' => 'stream',
                    'options' => [
                        'stream' => 'var/log/%date%-default.log',
                        'level' => Logger::DEBUG,
                    ]
                ]    
            ]
        ]
    ]
];
````

#### ** Zend style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

use Monolog\Logger;

return [
    'monolog' => [
        'handlers' => [
            'default' => [
                'type' => 'stream',
                'options' => [
                    'stream' => 'var/log/%date%-default.log',
                    'level' => Logger::DEBUG,
                ]
            ]    
        ]
    ]
];
````

<!-- tabs:end -->

See [wshafer/monolog]() for full options and handlers support.

### Usage

For example the usage of the Logger inner Request Handler

````php
<?php
// src/Application/Http/Handler/HomePageRequestHandler.php

declare(strict_types=1);

namespace App\Application\Http\Handler;

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Psr\Log\LoggerInterface;
use Zend\Diactoros\Response\HtmlResponse;

class HomePageRequestHandler implements RequestHandlerInterface
{
    /** @var LoggerInterface */
    private $logger;

    public function __construct(LoggerInterface $logger)
    {
        $this->logger = $logger;
    }

    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        $this->logger->debug('Something happened to be logged.');

        return new HtmlResponse('<html><body><h1>Hello World!!!!</h1></body></html>');
    }
}
````

## Event System

> https://github.com/antidot-framework/antidot-event-dispatcher

[Psr 14 Event dispatcher](https://github.com/php-fig/event-dispatcher) implementation.

The event dispatcher is available as `Psr\EventDispatcher\EventDispatcherInterface` you can inject in your services constructors out of the box.

### Config

<!-- tabs:start -->

#### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
parameters:
  app-events:
    event-listeners:
      # Full\Name\To\SomeEvent or some.event.identifier
      some-event:
        - 'App\Application\EventListener\SomeEventListener'
        - 'App\Application\EventListener\SomeOtherEventListener'
````

#### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
app-events:
  event-listeners:
    # Full\Name\To\SomeEvent or some.event.identifier
    some-event:
      - 'App\Application\EventListener\SomeEventListener'
      - 'App\Application\EventListener\SomeOtherEventListener'
````

#### ** Symfony style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'parameters' => [
        'app-events' => [
            'event-listeners' => [
                // Full\Name\To\SomeEvent::class or some.event.identifier
                'some-event' => [
                    App\Application\EventListener\SomeEventListener::class,
                    App\Application\EventListener\SomeOtherEventListener::class,
                ]    
            ]
        ]
    ]
];
````

#### ** Zend style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'app-events' => [
        'event-listeners' => [
            // Full\Name\To\SomeEvent::class or some.event.identifier
            'some-event' => [
                App\Application\EventListener\SomeEventListener::class,
                App\Application\EventListener\SomeOtherEventListener::class,
            ]    
        ]
    ]
];
````

<!-- tabs:end -->

### Usage

For example the usage of Event Dispatcher inner Request Handler

````php
<?php
// src/Application/Http/Handler/HomePageRequestHandler.php

declare(strict_types=1);

namespace App\Application\Http\Handler;

use App\Application\Event\SomeEvent;
use Psr\EventDispatcher\EventDispatcherInterface;
use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Zend\Diactoros\Response\HtmlResponse;

class HomePageRequestHandler implements RequestHandlerInterface
{
    /** @var EventDispatcherInterface */
    private $eventDispatcher;

    public function __construct(EventDispatcherInterface $eventDispatcher)
    {
        $this->eventDispatcher = $eventDispatcher;
    }

    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        $this->eventDispatcher->dispatch(SomeEvent::occur());

        return new HtmlResponse('<html><body><h1>Hello World!!!!</h1></body></html>');
    }
}
````

Implementation of `Psr\EventDispatcher\EventDispatcherInterface` is injected via constructor to the `HomePageRequestHandler` class, then, when class handles request it will dispatch given event, who is handled by all configured event listener since event propagation is not stopped.

The Listener class should be `callable` and it should be properly configured in [Dependency Injection](/#Dependency-Injection)

````php
<?php
// src/Application/EventListener/SomeEventListener.php

declare(strict_types=1);

namespace App\Application\EventListener;

use App\Application\Event\SomeEvent;

class SomeEventListener
{
    public function __invoke(SomeEvent $event): void
    {
        // do your stuff with event data here
    }
}
````

The event should be an implementation of `Antidot\Event\EventInterface` or `Psr\EventDispatcher\StoppableEventInterface`

````php
<?php
// src/Application/Event/SomeEvent.php

declare(strict_types=1);

namespace App\Application\Event;

use Antidot\Event\Event;

final class SomeEvent extends Event
{
    private function __construct(string $name)
    {
        $this->name = $name;
        $this->stopped = false;

    }

    public static function occur(): self
    {
        return new self('some.event');
    }
}
````

## Console Line Tool

> https://github.com/antidot-framework/antidot-cli

The CLI component is available in `bin/console` file, you can watch listed commands executing it in a terminal

````bash
bin/console 
````

![Command list](/images/cli-list.jpg)

### Show Container Command

````php
bin console config:show:container
````
![Show container command](/images/cli-show-container.jpg)

###### Disclaimer: 

* This framework is created for educational purposes. The full or partial use of this software is the responsibility of the user.
