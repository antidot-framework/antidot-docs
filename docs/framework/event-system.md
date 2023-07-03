# Event System

> https://github.com/antidot-framework/antidot-event-dispatcher

[Psr 14 Event dispatcher](https://github.com/php-fig/event-dispatcher) implementation.

The event dispatcher is available as `Psr\EventDispatcher\EventDispatcherInterface` you can inject in your services constructors out of the box.

## Config

````php
<?php
// config/autoload/dependencies.{prod,dev,local}.php

declare(strict_types=1);

return [
    'app-events' => [
        'event-listeners' => [
            // Full\Name\To\SomeEvent::class or some.event.identifier
            'some-event' => [
                App\Application\EventListener\SomeEventListener::class,
                App\Application\EventListener\SomeOtherEventListener::class,
            ]    
        ]
    ]
];
````

## Usage

For example the usage of Event Dispatcher inner Request Handler

````php
<?php
// src/Application/Http/Handler/HomePageRequestHandler.php

declare(strict_types=1);

namespace App\Application\Http\Handler;

use App\Application\Event\SomeEvent;
use Psr\EventDispatcher\EventDispatcherInterface;
use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Zend\Diactoros\Response\HtmlResponse;

class HomePageRequestHandler implements RequestHandlerInterface
{
    /** @var EventDispatcherInterface */
    private $eventDispatcher;

    public function __construct(EventDispatcherInterface $eventDispatcher)
    {
        $this->eventDispatcher = $eventDispatcher;
    }

    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        $this->eventDispatcher->dispatch(SomeEvent::occur());

        return new HtmlResponse('<html><body><h1>Hello World!!!!</h1></body></html>');
    }
}
````

Implementation of `Psr\EventDispatcher\EventDispatcherInterface` is injected via constructor to the `HomePageRequestHandler` class, then, when class handles request it will dispatch given event, who is handled by all configured event listener since event propagation is not stopped.

The Listener class should be `callable` and it should be properly configured in [Dependency Injection](/#Dependency-Injection)

````php
<?php
// src/Application/EventListener/SomeEventListener.php

declare(strict_types=1);

namespace App\Application\EventListener;

use App\Application\Event\SomeEvent;

class SomeEventListener
{
    public function __invoke(SomeEvent $event): void
    {
        // do your stuff with event data here
    }
}
````

The event should be an implementation of `Antidot\Event\EventInterface` or `Psr\EventDispatcher\StoppableEventInterface`

````php
<?php
// src/Application/Event/SomeEvent.php

declare(strict_types=1);

namespace App\Application\Event;

use Antidot\Event\Event;

final class SomeEvent extends Event
{
    private function __construct(string $name)
    {
        $this->name = $name;
        $this->stopped = false;

    }

    public static function occur(): self
    {
        return new self('some.event');
    }
}
````
