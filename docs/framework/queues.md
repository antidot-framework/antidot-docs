# Queues

> [Message Queue](https://github.com/antidot-framework/message-queue)

## Install

```bash
composer require antidot-fw/message-queue
```

Install a transport for the queues:

* [Null Queue](https://queue.antidotfw.io/#/?id=null-queue)
* [Filesystem Queue](https://queue.antidotfw.io/#/?id=filesystem-queue)
* [DBAL Queue](https://queue.antidotfw.io/#/?id=dbal-queue)
* [Redis Queue](https://queue.antidotfw.io/#/?id=redis-queue)
* [Beanstalk](https://queue.antidotfw.io/#/?id=beanstalk-queue)
* [Amazon SQS](https://queue.antidotfw.io/#/?id=amazon-sqs-queue)

Allow using a job queues out of the box with different queue systems.

[See full documentation](https://queue.antidotfw.io) for transport specific configs.

## Quick start

Once you have installed and configured some transport type following specific config section. You can inject the `Antidot\Queue\Producer` class
inside your classes, for example in a Request Handler.

```php
<?php

declare(strict_types=1);

namespace App\Application\Http\Handler;

use Antidot\Queue\Job;
use Antidot\Queue\Producer;
use Psr\Http\Message\ResponseInterface;
use Psr\Http\Message\ServerRequestInterface;
use Psr\Http\Server\RequestHandlerInterface;
use Laminas\Diactoros\Response\JsonResponse;

class HomePage implements RequestHandlerInterface
{
    private Producer $producer;

    public function __construct(Producer $producer)
    {
        $this->producer = $producer;
    }

    public function handle(ServerRequestInterface $request): ResponseInterface
    {
        // Send String Job of type "some_message_type" to "default" queue.
        $job1 = Job::create('default', 'some_message_type', 'Hello world!!');
        $this->producer->enqueue($job1);
        
        // Send Array Job of type "other_message_type" to "other_queue" queue.
        $job2 = Job::create('other_queue', 'other_message_type', ['greet' => 'Hello world!!']);
        $this->producer->enqueue($job2);


        return new JsonResponse([
            'docs' => 'https://queue.antidotfw.io',
            'Message' => 'Welcome to Antidot Framework Starter'
        ]);
    }
}
``` 

Create a Job Handler Action

```php
<?php

declare(strict_types=1);

namespace App;

use Antidot\Queue\JobPayload;

class SomeMessageTypeAction
{
    public function __invoke(JobPayload $payload): void
    {
        // do some stuff with the job payload here.
    }
}
```

```yaml
services:
  some_action_service:
    class: App\SomeMessageTypeAction
parameters:
  queues:
    contexts:
      default:
        message_types:
          # message_type: action_service
          some_message_type: some_action_service
          other_message_type: some_action_service
```

then start listening a queue:

```bash
bin/console queue:start queue_name
```
