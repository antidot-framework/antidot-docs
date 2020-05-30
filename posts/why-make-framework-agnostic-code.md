# Why make Framework agnostic code

First of all, in my opinion, using a framework on your production applications is a good choice(this is not an article against any framework or framework user).

As every open source solution, using a framework isn't a silver bullet, it has benefits and some cadences. 
In this post we going to view how we can take profit of good ones and how we can avoid the bad things.

I start listing some good things that I will expect from a professional PHP Framework:

* Routing system
* HTTP Request response Lifecycle
* Centralized, environment friendly config system
* Dependency injection system(maybe autowired)
* Log system
* Console Line Tool
* Good open source library integration

These are very good starting point, a lot of effort and working hours made for us. Tested and maintained solutions that we don't have to worry about.
In addition, they often also have their own implementations of key parts of our application that we may not be interested in sticking to the rules and timing of the framework, 
such as controllers, requests, responses, even models, events and so on. 

Coupling our business logic or application code to some framework can lead in a several mistakes in a long time term project:

* App deprecation locked to the framework's roadmap timing and lifecycle
* Spend hours updating code parts
* Add bugs updating code parts

These issues are that we have to have in mind to avoiding it, 
thank to [PHP-FIG](https://www.php-fig.org/) we have different community accepted standards to write all integration pieces between framework code and project code,
and the most popular frameworks adopted it in part or can be managed to adopt it with a little workaround. 

Either we can apply some techniques to crate framework agnostic code

## Decoupled Controllers

When you can, do not extend your controller from the framework base controller, if you can't, 
make sure the base controller still uses PSR-7 Request Interface as entry parameter and PSR Response Interfaces as output parameter.  
Frameworks like Laminas, Mezzio, Slim or Antidot have out of the bo support for PSR-7(the standard accepted for the request lifecycle) also, 
we have packages to allow this in all popular frameworks as [symfony/psr-http-message-bridge](https://github.com/symfony/psr-http-message-bridge) for Symfony 
and Larabel or [charlesportwoodii/yii2-psr7-bridge](https://github.com/charlesportwoodii/yii2-psr7-bridge) for Yii.., 

Try not using framework magic for tasks like parameter autoload, validation or security, else make sure you can use these king of magic out of the framework as standalone component.

As a rule, independently o the framework you use or no, do not write business logic inside controllers, 
it's only responsibility should be getting the request pass it to some service and then prepare a response

## Application Events

Event object must be simple DTOs which do not extend from any framework abstract class, this way we can use different event dispatcher implementation according to our needs.

Do not use the framework default event listeners make your own describing your use cases.

## Decoupled Model

Model objects like entities, value objects or aggregates, should not extend or have code related to any framework, orm other external library, 
their responsibility is to describe our business logic in the clearest way, you can take profit from repository pattern to separate domain logic from storage. 

## Decoupled Infrastructure

Apply dependency inversion pattern with all your infrastructure pieces, 
understanding as infrastructure everything related to database, cache, session, 3rd party integrations, filesystem, mailing concrete implementations
