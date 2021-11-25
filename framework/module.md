---
layout: default
title: Module
nav_order: 140
parent: Framework
---

### WIP: Module

Module is a bunch of controllers with same settings.
You can define different view, auth, router, templating and even container for each module.

### Default modules

Initially, there are 2 predefined modules: `src/MyPackage/Controller/` and `src/MyPackage/Command/` directories.

### Define new module

Add new class to `src/MyPackage/Module/` directory like following:

```php
use Perfumer\Framework\Controller\Module;

class MyPackage extends Module
{
    // Name of the module
    public $name = 'my_module';

    // Router name in service definitions
    public $router = 'my_module_router';

    // Request name in service definitions
    public $request = 'my_module_request';

    // Response name in service definitions
    public $response = 'my_module_response';

    // container name in service definitions
    public $container = 'my_module_container';

    // list custom components
    public $components = [
        'auth' => 'my_module_auth',
        'view' => 'my_module_view'
    ];
}
```

