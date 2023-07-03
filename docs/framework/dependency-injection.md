# Dependency Injection

> https://github.com/antidot-framework/antidot-symfony-config-translator

Auto wired dependency injection system, the only thing you have to do is declare dependencies in `config/autoload/` directory

## Auto-wired Invokables

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'services' => [
        Full\Qualified\ClassName::class => Full\Qualified\ClassName::class,
    ]
];
````            Full\Qualified\ClassName::class => Full\Qualified\ClassName::class,


## Factory Classes

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'factories' => [
        Full\Qualified\ClassName::class => Full\Qualified\ClassNameFactory::class,
    ]
];
````

## Classes with arguments

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'services' => [
        Full\Qualified\ClassName::class => [
            'class' => Full\Qualified\ClassName::class,
            'arguments' => [
                '$foo' => Full\Qualified\ClassNameFactory::class,
                '$bar' => 'some.string',
            ],
        ]
    ]
];
````

## Implementing Interfaces

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'services' => [
        Full\Qualified\ClassNameInterface::class => Full\Qualified\ClassName::class
    ],
    // or using alias
    'aliases' => [
        Full\Qualified\ClassNameInterface::class => 'service.name'
    ],
];
````

## Declaring services

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'services' => [
        'some.service' => Full\Qualified\ClassName::class,
    ]
];
````

## Parameter Config

The configuration system uses Zend Config Aggregator strategy, this allows us to create complex configs and dependencies depending on the environment, by default it loads first PHP config files and then it parses YAML files merging all in a unique array of data who is available inside the container keyed by `config`

````php
<?php

/** @var Psr\ContainerInterface $config */
$config = $container->get('config');
````

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
