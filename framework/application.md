---
layout: default
title: Application
nav_order: 6
parent: Framework
---

### Application

Application class is situated at `src/Application.php`.
This is a central place to configure project. Fast example:

```php
class Application extends \Perfumer\Framework\Application\Application
{
    protected function configure(): void
    {
        // These files should always be included as they contain base framework configuration
        $this->addDefinitions(__DIR__ . '/../vendor/perfumer/framework/src/Package/Framework/Resource/config/services.php');
        $this->addResources(__DIR__ . '/../vendor/perfumer/framework/src/Package/Framework/Resource/config/resources.php');

        // Custom services
        $this->addDefinitions(__DIR__ . '/MyModule/Resource/config/services.php');

        // Custom resources
        $this->addResources(__DIR__ . '/MyModule/Resource/config/resources.php');

        // Built-in modules
        $this->addModule(new HttpModule(),    'http');
        $this->addModule(new ConsoleModule(), 'cli');

        // Custom modules
        $this->addModule(new MyHttpModule(),    'http');
        $this->addModule(new MyConsoleModule(), 'cli');
    }
}
```

##### addDefinitions

Adds file with service definitions.

##### addResources

Adds file with resources.

##### addModule

Adds Module (bunch of controllers).

### Env, Build Type and Flavor

Every method (`addDefinitions`, `addResources`, `addModule`) contain 3 arguments at the end: `env`, `build_type`, `flavor`.
Content of methods `addDefinitions`, `addResources`, `addModule` is applied to request if and only if all three of them are matched. 
`null` means any value.

##### env

Means PHP environment (PHP SAPI). Can be `http` or `cli`.

##### build_type

Means variant of development environment. Can be `dev`, `test`, `stage` or `prod`.

##### flavor

Means variant of project audience or project part. Can be any value.

### Application in Controller

In order to call application in controller:

```php
class MyController extends LayoutController
{
    public function get()
    {
        if ($this->getApplication()->getBuildType() === 'dev') {
            // do staff in development environment
        }
    }
}
```  
