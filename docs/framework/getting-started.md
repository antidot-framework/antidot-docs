# Getting Started

## Installation

### Standard Version

> https://github.com/antidot-framework/antidot-starter

Using [composer](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos) package manager

````bash
composer create-project antidot-fw/antidot-framework-starter project-name
````

### ReactPHP Version

> https://github.com/antidot-framework/antidot-starter

````bash
composer create-project antidot-fw/reactive-starter project-name
````

## File Structure

````
bin/console
config/config.php
config/config-cli.php
config/container.php
config/container-cli.php
config/autoload/dependencies.prod.yaml
public/index.php
router/middleware.php
router/routes.php
src/
test/
var/cache/*.php
var/log/*.log
.gitignore
composer.json
grumphp.yml
phpcs.xml.dist
phpunit.xml.dist
README.md

````
