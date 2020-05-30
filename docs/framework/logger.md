## Logger

> https://github.com/antidot-framework/antidot-logger

[Psr 3 Logger](https://github.com/php-fig/logger) implementation by Monolog.

The logger is available as `Psr\Log\LoggerInterface` you can inject in your services constructors out of the box.

## Config

<!-- tabs:start -->

### ** Symfony style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
parameters:
  monolog:
    handlers:
      default:
        type: 'stream'
        options:
          stream: 'var/log/%date%-default.log'
          level: 400
````

### ** Zend style yaml **

````yaml
# config/autoload/dependencies.{prod,local,dev}.yaml
monolog:
  handlers:
    default:
      type: 'stream'
      options:
        stream: 'var/log/%date%-default.log'
        level: 400
````

### ** Symfony style php **

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

use Monolog\Logger;

return [
    'parameters' => [
        'monolog' => [
            'handlers' => [
                'default' => [
                    'type' => 'stream',
                    'options' => [
                        'stream' => 'var/log/%date%-default.log',
                        'level' => Logger::DEBUG,
                    ]
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

use Monolog\Logger;

return [
    'monolog' => [
        'handlers' => [
            'default' => [
                'type' => 'stream',
                'options' => [
                    'stream' => 'var/log/%date%-default.log',
                    'level' => Logger::DEBUG,
                ]
            ]    
        ]
    ]
];
````

<!-- tabs:end -->

See [wshafer/monolog]() for full options and handlers support.

## Usage

For example the usage of the Logger inner Request Handler

````php
<?php
// src/Application/Http/Handler/HomePageRequestHandler.php

declare(strict_types=1);

namespace App\Application\Http\Handler;

use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Psr\Log\LoggerInterface;
use Zend\Diactoros\Response\HtmlResponse;

class HomePageRequestHandler implements RequestHandlerInterface
{
    /** @var LoggerInterface */
    private $logger;

    public function __construct(LoggerInterface $logger)
    {
        $this->logger = $logger;
    }

    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        $this->logger->debug('Something happened to be logged.');

        return new HtmlResponse('<html><body><h1>Hello World!!!!</h1></body></html>');
    }
}
````
