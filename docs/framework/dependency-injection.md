# Dependency Injection

> https://github.com/antidot-framework/antidot-symfony-config-translator

Auto wired dependency injection system, the only thing you have to do is declare dependencies in `config/autoload/` directory

## Auto-wired Invokables

<!-- tabs:start -->

### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
services:
  Full\Qualified\ClassName:  

````

### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
dependencies:
  invokables:
    - Full\Qualified\ClassName: Full\Qualified\ClassName

````

### ** Symfony style php **

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

### ** Zend style php **

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

## Factory Classes

<!-- tabs:start -->

### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
services:
  Full\Qualified\ClassName:  
    factory: [Full\Qualified\ClassNameFactory, '__invoke']

````

### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
dependencies:
  factories:
    - Full\Qualified\ClassName: Full\Qualified\ClassNameFactory

````

### ** Symfony style php **

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

### ** Zend style php **

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

## Complex classes with arguments

<!-- tabs:start -->

### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
services:
  Full\Qualified\ClassName:  
    arguments:
      $foo: '@SomeService'
      $bar: '%config.some_parameter%'

````

### ** Zend style yaml **

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

### ** Symfony style php **

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

### ** Zend style php **

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

## Implementing Interfaces

<!-- tabs:start -->

### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
services:
  Full\Qualified\ClassNameInterface:
    class: Full\Qualified\ClassName
  # Or using aliases
  Full\Qualified\AnotherClassNameInterface:
    alias: 'service.name'
````

### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
dependencies:
  invokables:
    Full\Qualified\ClassNameInterface: Full\Qualified\ClassName
  # Or using aliases
  aliases:
    Full\Qualified\AnotherClassNameInterface: 'service.name'
````

### ** Symfony style php **

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

### ** Zend style php **

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

## Declaring services

<!-- tabs:start -->

#### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
services:
  service.name:
    class: Full\Qualified\ClassName

````

### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
dependencies:
  services: 
    service.name: Full\Qualified\ClassName

````

### ** Symfony style php **

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

### ** Zend style php **

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

## Parameter Config

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
