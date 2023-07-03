# Templating

> https://github.com/antidot-framework/template-renderer

By default the Antidot framework doesn't have any template engine, you can install it as a separate component.

Choose a template engine from the different available implementations

## PUG Template Renderer

Use [PHP Pug as Template engine](https://github.com/pug-php/pug) 

> [Pug Template renderer](https://github.com/antidot-framework/phug-template-renderer)

```bash
composer require antidot-fw/phug-template-renderer
```

## Twig Template Renderer

Use [Twig as Template engine](https://twig.symfony.com/) 

> [Twig Template renderer](https://github.com/antidot-framework/twig-template-renderer)

```bash
composer require antidot-fw/twig-template-renderer
```

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

```php
<?php

declare(strict_types=1);

$config = [
    'templates' => [
        'extensino' => 'pug', // only pug
     ],
    'template' => [
        'debug' => false, // only twig
        'file_extension' => 'twig', // only twig
        'charset' => 'utf-8', // only twig
        'auto_reload' => false, // only twig
        'autoescape' => 'html', // only twig
        'strict_variables' => true, // only twig
        'pretty' => true, // only pug
        'expressionLanguage' => 'js', // only pug
        'pugjs' => false, // only pug
        'localsJsonFile' => false, // only pug
        'cache' => 'var/cache/pug',
        'template_path' => 'templates/',
        'globals' => [
            'title' => 'Antidot Framework'
        ],
        'filters' => [
            // 'name' => PHPCallableClass::class,
            // 'some_function' => 'php_some_function,
        ],
        'extensions' => [ // only twig
            // EtensionClassName::class,
        ],
        'functions' => [ // only twig
            // 'name' => PHPCallableClass::class,
            // 'some_function' => 'php_some_function,
        ],
        'keywords' => [], // only pug
        'helpers' => [], // only pug
        'default_params' => [], // only pug
    ]
];
```
