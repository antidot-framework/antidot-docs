# Antidot Framework

## Key Features

The Antidot PHP Framework offers a range of powerful features that make it a versatile choice for 
building web applications. Some of the key features include:

### Dependency Injection Container

Antidot utilizes the Antidot Container, which is a powerful dependency injection container. 
It provides a way to manage and configure dependencies within your application, promoting 
modularity and testability. With the container, you can easily define and resolve dependencies
across your application.

### Middleware Stack

Antidot supports a flexible middleware stack, allowing you to define and execute middleware 
components in a specific order. Middleware provides a convenient way to intercept and modify 
HTTP requests and responses, enabling functionalities such as authentication, logging, error 
handling, and more. Antidot makes it easy to integrate and chain multiple middleware components 
to process requests and generate responses.

### Routing and Controllers

Antidot provides a robust routing system based on the popular nikic/fast-route package. With 
Antidot's fluent interface, you can define routes and bind them to controllers or request 
handlers. This allows you to map incoming HTTP requests to specific actions in your application.
Controllers or request handlers implement the `Psr\Http\Server\RequestHandlerInterface`,
enabling you to handle requests and return responses in a standardized way.

### Error Handling

Antidot offers comprehensive error handling capabilities. You can configure error handlers to
catch and process exceptions thrown during the execution of your application. This allows you 
to handle errors gracefully and provide appropriate responses to clients. Antidot supports 
custom error handlers, allowing you to define how different types of errors are handled based
on your application's requirements.

### Testing

Antidot emphasizes the importance of testing by providing built-in testing tools and utilities.
You can write unit tests for your application's components, including middleware, controllers, 
and services. Antidot integrates well with popular testing frameworks like PHPUnit, allowing 
you to easily set up and execute tests to verify the correctness of your code.

### ReactPHP Server with Synchronous API

One of the standout features of Antidot is its integration with ReactPHP, a powerful event-driven,
non-blocking I/O library for PHP. Antidot leverages ReactPHP to provide a high-performance,
asynchronous web server. Additionally, Antidot extends ReactPHP to support a synchronous API,
allowing developers to write code in a synchronous style without dealing with promises.
This feature provides a familiar programming experience while taking advantage of ReactPHP's 
performance benefits.

### And More...

Antidot PHP Framework also offers a range of other features, including environment-specific 
configuration, PSR-7 integration, PSR-15 middleware compatibility, extensibility through service
providers, and a vibrant community that actively contributes to the framework's development
and improvement.

---

## Quick Start

> Se the [Getting Started Guide](/framework/getting-started)

## Special thanks & Sponsors

* **JetBrains:** Thanks for supporting us with the All Products Pack License for Open Source

<a href="https://www.jetbrains.com/?from=antidot-framework" target="_blank">
    <img alt="JetBrains" src="/images/jetbrains-variant-4.png" style="width:263px !important;height:147px !important"/>
</a>

<iframe src="https://github.com/sponsors/kpicaza/card" title="Sponsor kpicaza" height="225" width="600" style="border: 0;"></iframe>

###### Disclaimer: 

* This framework is based on concepts and components of other open source software, especially [Laminas Request Handler Runner](https://docs.laminas.dev/laminas-httphandlerrunner/), [Mezzio](https://docs.mezzio.dev/mezzio/) and [Laminas API tools legacy Zend Stratigillity](https://api-tools.getlaminas.org/).
* The full or partial use of this software is the responsibility of the user.
