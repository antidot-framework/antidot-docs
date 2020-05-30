# Console Line Tool

> https://github.com/antidot-framework/antidot-cli

The CLI component is available in `bin/console` file, you can watch listed commands executing it in a terminal

````bash
bin/console 
````

![Command list](/../images/cli-list.jpg)

## Show Container Command

````php
bin console config:show:container
````
![Show container command](/../images/cli-show-container.jpg)

## Clear Config Cache

````php
bin console config:clear-cache
````

## Custom Console Command

Console tool is built in on top of Symfony Console Component, to createnew command you need to extend from Symfony 
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
