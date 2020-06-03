# Templating

> https://github.com/antidot-framework/template-renderer

By default the Antidot framework doesn't have any template engine, you can install it as a separate component.

Choose a template engine from the different available implementations

> [Pug Template renderer](https://github.com/antidot-framework/phug-template-renderer)

## Config

<!-- tabs:start -->

### **Symfony Style yaml**

Coming soon

### **Symfony php**

```php
<?php

declare(strict_types=1);

$config = [
    'parameters' => [
        'templates' => [
            'extensino' => 'pug',
         ],
        'template' => [
            'pretty' => true,
            'expressionLanguage' => 'js',
            'pugjs' => false,ee
            'localsJsonFile' => false,
            'cache' => 'var/cache/pug',
            'template_path' => 'templates/',
            'globals' => [
                'title' => 'Antidot Framework'
            ],
            'filters' => [],
            'keywords' => [],
            'helpers' => [],
            'default_params' => [],
        ],
    ],
];
```

### **Zend style yaml**

Coming soon

### **Zend Style php**

```php
<?php

declare(strict_types=1);

$config = [
    'templates' => [
        'extensino' => 'pug',
     ],
    'template' => [
        'pretty' => true,
        'expressionLanguage' => 'js',
        'pugjs' => false,
        'localsJsonFile' => false,
        'cache' => 'var/cache/pug',
        'template_path' => 'templates/',
        'globals' => [
            'title' => 'Antidot Framework'
        ],
        'filters' => [],
        'keywords' => [],
        'helpers' => [],
        'default_params' => [],
    ],
];
```

<!-- tabs:end -->

## Usage
