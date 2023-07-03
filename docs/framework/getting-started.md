# Getting Started

## Installation

To start using the Antidot PHP Framework, you'll need to install it in your project. 
The recommended method is to use Composer, the popular dependency manager for PHP.
Make sure you have Composer installed before proceeding with the following steps.

### Step 1: Create a New Project

First, open your terminal or command prompt and navigate to the directory where you want 
to create your new Antidot project. Then, run the following command:

```shell
composer create-project antidot-fw/antidot-framework-starter myproject
```

This command will create a new directory named `myproject` and install the Antidot PHP Framework skeleton application inside it.

### Step 2: Configuration

After the project is created, navigate to the `myproject` directory:

```shell
cd myproject
```

Next, you'll need to configure your application. Copy the example configuration file:

```shell
bin/console config:development-mode
```

This command creates a local configuration file where you can customize the settings specific to your environment.

### Step 3: Serve the Application

To run the Antidot PHP Framework application, you can use PHP's built-in web server. In the terminal, run the following command from the project's root directory:

```shell
XDEBUG_MODE=off bin/console serve # runs Async server with XDebug disabled for better performance
php -S 127.0.0.1:3000 -t public # runs Sync server nice for testing
```

This command starts the development server and serves your application on `http://localhost:3000`.

### Step 4: Verify Installation

Open your web browser and visit `http://localhost:3000`. If everything is set up correctly, you should see a "Welcome to Antidot!" message.

Congratulations! You have successfully installed the Antidot PHP Framework.

Certainly! Here's an example of a "Project Setup" section for the Antidot PHP Framework "Getting Started" guide:

---

## Project Setup

When working with the Antidot PHP Framework, it's important to set up your project properly to follow the
recommended conventions and ensure a well-organized codebase. Here are the steps to set up your Antidot 
project:

### Project Structure

The Antidot PHP Framework follows a convention-based project structure that promotes modularity and 
separation of concerns. Here's an example of a typical project structure:

```
myproject/
├── config/
│   ├── services/
│   │   ├── dependencies.dev.php.dist
│   │   └── dependencies.prod.php
│   ├── cli-config.php
│   ├── cli-container.php
│   ├── config.php
│   └── container.php
├── src/
│   ├── Command/
|   |   └── SomeCommandExample.php
│   ├── Event/
|   |   └── SomeEvent.php
│   ├── EventListener/
|   |   └── SomeEventListener.php
│   └── Handler/
│       ├── HomePage.php
|       └── SomeMiddleware.php
├── public/
│   └── index.php
├── router/
│   ├── middleware.php
│   └── routes.php
├── tests/
├── var/
│   └── cache/
├── vendor/
└── composer.json
```

- The `config` directory contains various configuration files for your application, such as 
environment-specific settings, application bootstrap, middleware pipeline, and route definitions.

- The `src` directory is where you'll place your application's source code. It typically contains
different directories for handling requests, defining middleware, configuring dependencies, and more.

- The `public` directory is the web server's document root. It contains the entry point of your 
application, such as `index.php`, and any publicly accessible assets.

- The `router` directory is where you store your application's routes and middlewares.

- The `tests` directory contains unit tests and functional tests for your application. It's a good 
practice to write tests to ensure the stability and correctness of your code.

### Configuration Files

The `config` directory contains several important configuration files:

- `config/services/dependencies.dev.php`: This file holds local environment-specific configuration settings.
You can modify this file based on your development environment.

- `config/services/dependencies.prod.php`: This file holds production environment-specific configuration 
settings. You can modify this file based on your development environment.

- `config/[cli-]config.php`: This file is responsible for bootstrapping your application and 
configuring various Config Providers and project config.

- `config/[cli-]container.php`: This file is responsible for bootstrapping your application container.

- `router/middleware.php`: This file defines the middleware pipeline for your application. Middleware 
components are executed in a specific order, allowing you to handle request and response processing.

- `router/routes.php`: This file contains the route definitions for your application. You can define 
routes and map them to controller actions.

### Additional Setup

Depending on your project requirements, you may need to perform additional setup steps, such as 
configuring a database connection, setting up caching, or integrating other external libraries.
The specific steps for these configurations may vary based on your needs.

---

## Hello World Example

Let's get started with a simple "Hello World" example to demonstrate the basic usage of the Antidot
PHP Framework.

### Step 1: Create a Route

In the `router/routes.php` file, let's define a route that maps the root URL (`/`) to a controller
action. Open the file and add the following code:

```php
use Antidot\Framework\Application;
use Psr\Container\ContainerInterface;
use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;

return static function (Application $app, ContainerInterface $container) : void {
    $app->get('/', [\App\Handler\HomePage::class], 'home');
};
```

This code sets up a route that matches the root URL (`/`) and returns a simple HTML response with the 
text "Hello, World!".

### Step 2: Create a Controller

Next, let's create the controller that will handle the request and generate the response. In the
`src/Handler/HomeHandler.php` file, add the following code:

```php
namespace App\Handler;

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Nyholm\Psr7\Response;

class HomePage implements RequestHandlerInterface
{
    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        return new Response(200, ['Content-Type' => 'text/plain'], 'Hello, World!');
    }
}
```

### Step 3: Register the Controller

In the `config/application.php` file, we need to register the controller as a service. Open the file
and modify the code as follows:

```php
use App\Handler\HomePage;

return [
    // ...

    'services' => [
        HomePage::class => HomePage::class,
    ],

    // ...
];
```

This code registers the `HomePage` controller as a service that can be resolved and used in our routes.

### Step 4: Serve the Application

To see the "Hello World" example in action, start the development server from the project's root directory:

```shell
php -S localhost:8000 -t public
```

Open your web browser and visit `http://localhost:8000`. You should see the "Hello, World!" message 
displayed on the page.

Congratulations! You've successfully created a simple "Hello World" example using the Antidot PHP
Framework.

---

## Configuration

The Antidot PHP Framework provides a flexible configuration system that allows you to customize various 
aspects of your application. Configuration files are typically located in the `config` directory. Let's 
explore the main configuration files used in an Antidot project:

### `config/container.php`

The `config/container.php` file is responsible for configuring your application's dependency injection 
container. This file sets up the container and defines the services and dependencies your application 
needs. You can use the container to define factories, invokables, or other service definitions.

### `config/config.php`

The `config/config.php` file is responsible for configuring your application's overall configuration. It 
sets up the config providers and config files in a single configuration array.

### `router/middleware.php`

The `router/middleware.php` file defines the middleware pipeline for your application. It specifies 
the order in which middleware components are executed for each incoming request. You can customize 
the pipeline by adding or removing middleware components to fulfill your application's requirements.

### Environment-Specific Configuration

Antidot supports environment-specific configuration files that allow you to customize settings based 
on the environment in which your application is running. By default, the framework includes the following 
environment-specific files:

- `config/services/[SOME_FILE_NAME].prod.php`: This file holds global configuration settings. You can use this
file to define global settings that should be used in production.

- `config/services/[SOME_FILE_NAME].dev.php`: This file contains environment-specific configuration settings 
for the development environment. You can adjust this file to customize the behavior of your application 
during development.

To use environment-specific configuration files, you can remove the `.dist` suffix from the file names. 
For example, rename `config/services/dependencies.dev.php.dist` to `config/services/dependencies.dev.php`. 
Antidot will automatically load the corresponding configuration files, overriding the global configuration.

### Accessing Configuration Settings

You can access configuration settings throughout your application using the `config` service provided by the dependency injection container. Here's an example of how you can access a configuration setting in a controller:

```php
// Factory
use Psr\Container\ContainerInterface;

class MyControllerFactory
{
    private $config;

    public function __incoke(ContainerInterface $container): MyController
    {        
        return new MyController($container->get('config')['debug']);
    }
}

// Controller or Any other Service
use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Nyholm\Psr7\Response;

class MyController {
    public function __construct(
        private readonly bool $debug
    ) {
    }

    public function myAction(ServerRequestInterface $request): ResponseInterface
    {
        // Access a configuration setting
        $mySetting = $this->debug;

        // ...
    }
}
```

In the above example, the controller receives the `$config` array in its constructor, allowing you to 
access any configuration setting you need.

### Additional Configuration

Depending on your application's requirements, you may need to configure additional components, such as 
databases, logging, caching, or external services. The specific configuration steps for these components 
may vary based on the libraries or services you are integrating into your Antidot project. Consult the 
relevant documentation for those components to ensure proper configuration.

---

Apologies for the confusion. Here's an updated version of the "Dependency Injection" section for the Antidot PHP Framework based on the old Antidot framework documentation:

---

## Dependency Injection

The Antidot PHP Framework leverages the powerful [PSR-11](https://www.php-fig.org/psr/psr-11/) container 
interface to provide a robust dependency injection mechanism. The framework uses its own container 
implementation called Antidot Container.

### Container Configuration

The container configuration is defined in the `config/container.php` file. This file sets up the container
and defines the services and dependencies your application needs. You can use the container to define 
factories, invokables, or other service definitions.

For example, in the `config/container.php` file, you can find code similar to the following:

```php
<?php

// Load configuration
use Antidot\Container\Builder;

$config = require __DIR__ . '/../config/config.php';

// Build container
return Builder::build($config, true);
```

This code sets up the container using the `Container` class provided by the Antidot Container package. 
It creates and configures the container based on the specified configuration.

### Using Dependency Injection in your services

To use dependency injection in your services, you can type-hint the required dependencies in the 
class constructor or action method. The container will automatically resolve and inject the 
dependencies for you.

Here's an example of a controller using dependency injection:

```php
<?php

declare(strict_types=1);

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;

class MyController
{
    private $someService;

    public function __construct(SomeServiceInterface $someService)
    {
        $this->someService = $someService;
    }

    public function myAction(ServerRequestInterface $request): ResponseInterface
    {
        // Use the injected service
        $result = $this->someService->doSomething();

        // ...
    }
}
```

In the above example, the `MyController` constructor accepts a `SomeServiceInterface` dependency. 
The container will automatically resolve the `SomeService` implementation and inject it into the controller
when it is instantiated.

Let's add the service definition in `config/services/dependencies.prod.php` file:

```php
<?php

return [
    'services' => [
        MyController::class => MyController::class,
        SomeServiceInterface::class => SomeServiceInterface::class,
    ]
];
```

### Customizing the Container

You can customize the container configuration by modifying the `config/config.php` file. This allows
you to define additional services, configure aliases, or customize the container's behavior to fit your
application's needs.

### Advanced Dependency Injection

The Antidot PHP Framework also provides support for advanced dependency injection features, such as 
autowiring, factories, and delegators. These features allow you to configure more complex dependencies 
and handle specific use cases. You can refer to the Antidot framework documentation for more information 
on using advanced dependency injection features.

---

## Middleware

Middleware plays a crucial role in the Antidot PHP Framework. It allows you to intercept and modify the 
HTTP request and response objects as they pass through the application's middleware stack. You can use 
middleware to perform tasks such as authentication, input validation, logging, and more.

### Middleware Pipeline

The Antidot PHP Framework uses a middleware pipeline to handle incoming HTTP requests. The pipeline 
consists of a sequence of middleware layers that are executed in the order they are added to the pipeline.

The pipeline configuration is defined in the `config/pipeline.php` file. This file specifies the middleware 
layers and their order. Each middleware layer can perform specific tasks and modify the request and response 
objects before passing them to the next layer.

Here's an example of a `router/middleware.php` file:

```php
<?php

declare(strict_types=1);

use Antidot\Framework\Application;
use Antidot\Framework\Middleware\ErrorMiddleware;
use Antidot\Framework\Middleware\RouteDispatcherMiddleware;
use Antidot\Framework\Middleware\RouteNotFoundMiddleware;
use Antidot\Logger\Application\Http\Middleware\ExceptionLoggerMiddleware;
use Antidot\Logger\Application\Http\Middleware\RequestLoggerMiddleware;

return static function (Application $app) : void {
    $app->pipe(ErrorMiddleware::class);
    $app->pipe(ExceptionLoggerMiddleware::class);
    $app->pipe(RequestLoggerMiddleware::class);
    $app->pipe(RouteDispatcherMiddleware::class);
    $app->pipe(RouteNotFoundMiddleware::class);
};
```

In the above example, we see the pipeline configuration. The `$app->pipe()` method is used to add middleware 
layers to the pipeline. In this example, we have added the following middleware layers:

- `ErrorMiddleware`: This middleware handles error responses for exceptions thrown during request processing.
- `ExceptionLoggerMiddleware`: This middleware handles exceptions thrown during request processing and logs the 
error using psr logger interface.
- `RequestLoggerMiddleware`: This middleware gets a log from requests info, its useful for development purposes.
- `RouteDispatcherMiddleware`: This middleware dispatches the matched route's handler.
- `RouteNotFoundMiddleware`: This middleware handles requests for routes that do not exist.

You can add your own custom middleware to the pipeline by using the `$app->pipe()` method and specifying the
middleware class.

### Creating Custom Middleware

To create custom middleware, you need to create a PHP class that implements the 
`Psr\Http\Server\MiddlewareInterface` interface. This interface defines a single method `process()` 
that accepts a `Psr\Http\Message\ServerRequestInterface` object and a `Psr\Http\Server\RequestHandlerInterface`
object.

Here's an example of a custom middleware class:

```php
<?php

declare(strict_types=1);

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\MiddlewareInterface;
use Psr\Http\Server\RequestHandlerInterface;

class CustomMiddleware implements MiddlewareInterface
{
    public function process(ServerRequestInterface $request, RequestHandlerInterface $handler): ResponseInterface
    {
        // Perform custom processing on the request object
        // ...

        $response = $handler->handle($request);

        // Perform custom processing on the response object
        // ...

        return $response;
    }
}
```

In the above example, the `CustomMiddleware` class implements the `process()` method, where you can perform custom processing on the request and response objects. You can also invoke the next middleware layer by calling the `handle()` method on the `$handler` object.

### Adding Custom Middleware

To add your custom middleware to the pipeline, you can use the `$app->pipe()` method in the 
`router/middleware.php` file, as shown in the previous example. Simply specify the class name of your
middleware as the argument to the `$app->pipe()` method.

```php
$app->pipe(CustomMiddleware::class);
```

Make sure to add your custom middleware in the desired order within the pipeline to ensure the
proper flow of request and response objects.

---


## Routing and Controllers

The Antidot PHP Framework provides a powerful routing system that allows you to define routes and map them 
to specific controllers. This makes it easy to handle different HTTP requests and route them to the 
appropriate controller for processing.

### Route Configuration

The route configuration is defined in the `router/routes.php` file. This file contains a series of route 
definitions using the `Application` class.

Here's an example of a `router/routes.php` file:

```php
<?php

declare(strict_types=1);

use Antidot\Framework\Application;
use App\Application\Http\Handler\HomePage;
use Psr\Container\ContainerInterface;

/**
 * Setup routes with a single request method:
 *
 * $app->get('/', [App\Handler\HomePageHandler::class], 'home');
 * $app->post('/album', [App\Handler\AlbumCreateHandler::class], 'album.create');
 * $app->put('/album/:id', [App\Handler\AlbumUpdateHandler::class], 'album.put');
 * $app->patch('/album/:id', [App\Handler\AlbumUpdateHandler::class], 'album.patch');
 * $app->delete('/album/:id', [App\Handler\AlbumDeleteHandler::class], 'album.delete');
 */
return static function (Application $app, ContainerInterface $container) : void {
    $app->get('/', [HomePage::class], 'home');
};
```

In the above example, we define a route for the root URL ("/") using the `get()` method of the
`Application` class. The `get()` method accepts the URL pattern, an array of controller classes, and a 
route name. In this case, we specify the `HomePage` controller class as the handler for the route.

### Creating Controllers

To handle requests, you need to create controller classes that implement the `RequestHandlerInterface`.
This interface defines a `handle()` method that accepts a `ServerRequestInterface` and returns a 
`ResponseInterface`.

Here's an example of a controller class:

```php
<?php

declare(strict_types=1);

namespace App\Application\Http\Handler;

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;

class HomePage implements RequestHandlerInterface
{
    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        // Handle the request and generate a response
        // ...

        // Return the response
    }
}
```

In the above example, the `HomePage` controller class implements the `RequestHandlerInterface` and provides
the implementation for the `handle()` method. Inside the `handle()` method, you can handle the request,
perform any necessary processing, and generate a response.

### Dependency Injection in Controllers

You can use dependency injection in your controllers to access other services or dependencies. Simply 
type-hint the required dependencies in the constructor, declare it in the dependencies file and the 
Antidot container will automatically resolve and inject them for you.

Here's an example of a controller using dependency injection:

```php
<?php

declare(strict_types=1);

namespace App\Handler;

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Some\Service\Dependency;

class HomePage implements RequestHandlerInterface
{
    public function __construct(
        private Dependency $dependency
    ) {
    }

    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        // Use the injected dependency
        $result = $this->dependency->doSomething();

        // Handle the request and generate a response
        // ...

        // Return the response
    }
}
```

In the above example, the `HomePage` controller class accepts a `Dependency` dependency through its 
constructor. The Antidot container will automatically resolve the `Dependency` class and inject it into
the controller when it is instantiated.

Let's add the service definition in `config/services/dependencies.prod.php` file:

```php
<?php

return [
    'services' => [
        HomePage::class => HomePage::class,
        Dependency::class => Dependency::class,
    ]
];
```

---

## Error Handling

The Antidot PHP Framework provides a flexible and customizable error handling mechanism that allows you to 
handle and respond to errors and exceptions in a consistent way.

### Error Middleware

The error handling is done using error middleware, which is responsible for catching exceptions and 
generating appropriate error responses. Error middleware is added to the application pipeline, ensuring 
that any exceptions thrown during the request handling process are properly handled.

To add error middleware, you can modify your `router/middleware.php` file. Here's an example of adding error
middleware to the pipeline:

```php
<?php

declare(strict_types=1);

use Antidot\Framework\Middleware\ErrorMiddleware;

return static function (Application $app, ContainerInterface $container, Pipeline $pipeline) : void {
    // Add error middleware to the pipeline
    $pipeline->pipe(ErrorMiddleware::class);

    // Add more middleware to the pipeline
    // ...
};
```

In the above example, the `ErrorMiddleware` is added to the pipeline. This middleware catches exceptions
and generates appropriate error responses based on the exception type.

It prints different error representation depending on you dependencies and debug mode.

* With Debug mode On The error will be printed in json format.
* With Debug mode On, and `franzl/whoops-middleware` installed as dev dependency you will take whoops
handler for developer friendly error display. 
* With Debug mode disabled, the errors will be hidden from user perspective.

### Custom Error Handling

You can customize the error handling by creating your own error middleware and adding it to the pipeline.
This allows you to define specific error handling logic based on your application's requirements.

To create a custom error middleware, you need to implement the `Psr\Http\Server\MiddlewareInterface`.
Here's an example:

```php
<?php

declare(strict_types=1);

namespace App\Middleware;

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\MiddlewareInterface;
use Psr\Http\Server\RequestHandlerInterface;

class CustomErrorMiddleware implements MiddlewareInterface
{
    public function process(ServerRequestInterface $request, RequestHandlerInterface $handler): ResponseInterface
    {
        try {
            // Process the request
            return $handler->handle($request);
        } catch (\Throwable $e) {
            // Handle the exception and generate an error response
            $response = new Response();
            // Set appropriate status code, headers, and body for the error response
            $response->getBody()->write('An error occurred');

            return $response->withStatus(500);
        }
    }
}
```

In the above example, the `CustomErrorMiddleware` implements the `MiddlewareInterface` and provides custom 
error handling logic inside the `process()` method. When an exception is thrown during the request handling
process, the `process()` method catches the exception, generates an error response, and returns it.

To add your custom error middleware to the pipeline, you can modify the `router/middleware.php` file:

```php
<?php

declare(strict_types=1);

use App\Middleware\CustomErrorMiddleware;

return static function (Application $app, ContainerInterface $container, Pipeline $pipeline) : void {
    // Add your custom error middleware to the pipeline
    $pipeline->pipe(CustomErrorMiddleware::class);

    // Add more middleware to the pipeline
    // ...
};
```

In the above example, the `CustomErrorMiddleware` is added to the pipeline, ensuring that it is executed 
when an error occurs.

### Error Views

In addition to generating error responses, you may also want to customize the error views or templates
displayed to the user. The Antidot PHP Framework allows you to define error views based on the HTTP 
status code or the exception type.

To define an error view, you can use a templating engine or a view renderer of your choice. Create 
the necessary views for different error scenarios, such as 404 (Not Found) or 500 (Internal Server Error),
and configure the framework to use them.

The exact process for defining and configuring error views depends on the

specific templating engine or view renderer you are using. Please refer to the documentation of your
chosen template engine or view renderer for more details.
