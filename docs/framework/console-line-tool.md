# Console Line Tool

> https://github.com/antidot-framework/antidot-cli

The CLI component is available in `bin/console` file, you can watch listed commands executing it in a terminal

````bash
bin/console 
````

![Command list](/../images/cli-list.png)

## Show Container Command

Shows all configured dependencies in a table.

```bash
bin/console config:show:container
```
![Show container command](/../images/cli-show-container.png)

## Clear Config Cache

Removes all files at configured cache directory, by default in keys 'config_cache_path' and '''cli_config_cache_path'.

```bash
bin/console config:clear-cache
```
![Clear Config Cache command](/../images/clear-config-cache.png)

## Development mode

Enables and disables development mode.

### Enable Development mode

```bash
bin/console config:development-mode
```
![Enable Development Mode command](/../images/enable-development-mode.png)

### Disable Development mode

```bash
bin/console config:development-mode --disable=true
```
![Disable Development Mode command](/../images/disable-development-mode.png)

## Maker Commands

It has a set of useful CLI commands to facilitate the creation of different types of classes. 

### Make PSR-7 Request Handler

Generates an instance of Psr\Http\Server\RequestHandlerInterface and allocates in its place on the filesystem 
respecting the given full qualified classname 

```bash
bin/console make:request-handler 'App\Application\Http\Handler\TestRequestHandler'
```
![Make Request Handler command](/../images/make-request-handler.png)

### Make PSR-15 Middleware

Generates an instance of Psr\Http\Server\MiddlewareInterface and allocates in its place on the filesystem 
respecting the given full qualified classname 

```bash
bin/console make:middleware 'App\Application\Http\Middleware\TestMiddleware'
```
![Make Middleware command](/../images/make-middleware.png)

### Make Invokable Factory

Generates an invokable factory and allocates in its place on the filesystem respecting the given full qualified classname 

```bash
bin/console make:factory 'App\Container\SomeFactory'
```
![Make Factory command](/../images/make-factory.png)

### Make PSR-14 Compatible Event

Generates an instance of Psr\EventDispatcher\StoppableEventInterface and allocates in its place on the filesystem 
respecting the given full qualified classname 

```bash
bin/console make:event 'App\Domain\Event\SomethingOccurred'
```
![Make Event command](/../images/make-event.png)

### Make Event Listener

Generates an event listener class for given event name, and allocates in its place on the filesystem 
respecting the given full qualified classname 

```bash
bin/console make:event-listener 'App\Application\Service\SomeEventListener' 'App\Domain\Event\SomethingOccurred'
```
![Make Event Listener command](/../images/make-event-listener.png)

### Make Symfony Console Command

Generates a command class compatible with Symfony console, and allocates in its place on the filesystem 
respecting the given full qualified classname 

```bash
bin/console make:event-listener 'App\Application\Service\SomeEventListener' 'App\Domain\Event\SomethingOccurred'
```
![Make Console Command command](/../images/make-console-command.png)

## Custom Console Command

The Console tool, built in on top of Symfony Console Component, to create new command you need to extend from Symfony 
Console Command Class

````php
<?php

declare(strict_types=1);

namespace Some\Namespace;

use Symfony\Component\Console\Command\Command;
use Symfony\Component\Console\Input\InputInterface;
use Symfony\Component\Console\Output\OutputInterface;

final class SomeCommand extends Command
{
    public const NAME = 'some:command:name';

    protected function configure()
    {
        $this->setName(self::NAME)
            ->setDescription('Show all available services inner container.');
    }

    protected function execute(InputInterface $input, OutputInterface $output)
    {
        // do some stuff here.
    }
}
````

## Config

<!-- tabs:start -->

### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
services:
  App\Application\EventListener\SomeCommand:
  tags:
    - { name: 'console.command', command: 'some:command:name' }
````

### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
console:
  commands:
    'some:command:name': App\Application\EventListener\SomeCommand
dependencies:
  invokables:
    App\Application\EventListener\SomeCommand: App\Application\EventListener\SomeCommand
    
````

### ** Symfony style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'services' => [
        App\Application\EventListener\SomeCommand::class => [
          'class' => App\Application\EventListener\SomeCommand::class,
          'tags' => [
            [
                'name' => 'console.command', 
                'command' => App\Application\EventListener\SomeCommand::NAME
            ]    
          ]
        ]  
    ]
];
````

### ** Zend style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'console' => [
        'commands' => [
            App\Application\EventListener\SomeCommand::NAME =>
              App\Application\EventListener\SomeCommand::class
        ]
    ],
    'dependencies' => [
        'invokables' => [
            App\Application\EventListener\SomeCommand::class => 
              App\Application\EventListener\SomeCommand::clsss    
        ]    
    ]
];
````

<!-- tabs:end -->
