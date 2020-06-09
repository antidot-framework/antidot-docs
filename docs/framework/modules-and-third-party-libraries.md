# Modules and third-party libraries

## Modules

To enable a module you need to tell to `composer.json` file where to autoload module's namespace. Suppose that your 
new module will reside in `src/Mymodule` directory, and the module test at `test/MyModule`. 

```json
{
  ...,
  "autoload": {
    "psr-4": {
      "App\\": "src/App/",
      "MyModule\\": "src/MyModule/"
    }
  },
  "autoload-dev": {
    "psr-4": {
      "AppTest\\": "test/App/",
      "MyModuleTest\\": "test/MyModule/"
    }
  },
  ...
}
```

# Third Party libraries

Antidot framework is an open-source ecosystem friendly, and it allows easily to manage third-party libraries or packages 
making use of the [Laminas Config Provider](https://docs.laminas.dev/laminas-config-aggregator/config-providers/) component.

 With Composer, you can install any package available in [Packagist](https://packagist.org/), then if you want to have it available in the DI container, 
 you need to configure wanted dependency in a config file following [the config instructions](/framework/dependency-injection.md).
 
 When you want to add complex objects to the DI container you can take advantage of using factory classes, these classes
 must be invokables with a PSR-11 container interface as first parameter, and as recommendation,  
 it doesn't have to have any dependency in its constructor.

For example integrating [Doctrine DBAL](https://www.doctrine-project.org/projects/doctrine-dbal/en/2.10/index.html) connection for MySQL:

Install it by composer

```bash
composer require doctrine/dbal
```

Create a factory class for example in `App\Container` namespace.

```php
<?php
# src/Container/DbalConnectionFactory

declare(strict_types=1);

namespace App\Container;

use Doctrine\DBAL\Configuration;
use Doctrine\DBAL\Connection;
use Doctrine\DBAL\DriverManager;
use Psr\Container\ContainerInterface;

class DoctrineDBALFactory
{
    public function __invoke(ContainerInterface $container): Connection
    {
        $config = $container->get('config')['dbal']['connection'];

        return DriverManager::getConnection($config, new Configuration());
    }
}
```

Register factory in the [Dependency Injection Container](/framework/dependency-injection.md) and add config parameter to it.

```yaml
# See dependency injection container config docs to more info about service 
# configuration https://antidotfw.io/framework/dependency-injection.md.
parameters:
  dbal:
    connection:
      dbname: db_name
      user: db_user
      password: db_secret
      host: localhost
      port: 3306
      driver: pdo_mysql
      options:
        # this enables PDO to throw exceptions
        3: 2

services:
    Doctrine\DBAL\Connection:
      factory: App\Container\DoctrineDBALFactory
```
