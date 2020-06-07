# Templating

> https://github.com/antidot-framework/template-renderer

By default the Antidot framework doesn't have any template engine, you can install it as a separate component.

Choose a template engine from the different available implementations

## PUG Template Renderer

Use [PHP Pug as Template engine](https://github.com/pug-php/pug) 

> [Pug Template renderer](https://github.com/antidot-framework/phug-template-renderer)

## Twig Template Renderer

Coming soon.

Use [Twig as Template engine](https://twig.symfony.com/) 

> [Twig Template renderer](https://github.com/antidot-framework/twig-template-renderer)

## Usage

Inject `Antidot\Render\TemplateRenderer` interface in your request handler constructor, then render given template with given parameters.

```php
<?php

declare(strict_types=1);

use Antidot\Render\TemplateRenderer;
use Laminas\Diactoros\Response\HtmlResponse;
use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;

class SomeRequestHandler implements RequestHandlerInterface
{
    private TemplateRenderer $template;

    public function __construct(TemplateRenderer $template)
    {
        $this->template = $template;
    }

    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        return new HtmlResponse(
            // template is in templates/app/index.html.{pug,twig}, depending on template renderer implementation used.
            $this->template->render('app/index.html', [
                'foo' => 'bar',
                'bar' => [1, 2, 3],
            ])
        );
    }
}


```

## Config

<!-- tabs:start -->

### **Symfony Style yaml**

```yaml
parameters:
    templates:
        extension: pug # pug|twig
    template:
        pretty: true # only pug
        expressionLanguage: js # only pug
        pugjs: false # only pug
        localsJsonFile: false # only pug
        cache: var/cache/pug
        template_path: templates/
        globals:
            title: Antidot Framework
        filters: []
        keywords: []
        helpers: []
        default_params: => []
```

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
    ],
];
```

### **Zend style yaml**

```yaml
templates:
    extension: pug
template:
    pretty: true
    expressionLanguage: js
    pugjs: false
    localsJsonFile: false
    cache: var/cache/pug
    template_path: templates/
    globals:
        title: Antidot Framework
    filters: []
    keywords: []
    helpers: []
    default_params: => []
```

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

