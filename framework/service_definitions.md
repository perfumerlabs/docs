---
layout: default
title: Container Service Definitions
nav_order: 8
parent: Framework
---

### How to write Container service definitions

Service definition in Framework is a PHP array that describes instantiation of some class. They are written in separate config files, usually it is `MyPackage/Resource/config/services.php`,
though it depends from particular project.

Fast example:

```php
<?php

return [
    'my_service' => [
        // It means that this object exists in 1 copy only through request
        'shared' => true,
        
        // Name of the class
        'class' => 'MyPackage\\Service\\MyClass',

        // Arguments to inject to constructor in the order specified
        // Argument with "#" means that this is instance of the service "my_other_service"
        'arguments' => ['#my_other_service']
    ]
];
```

The above example is the equivalent of `new MyClass(new MyOtherClass())` in pure PHP.

### Types of Arguments

1. Starts with "#" means that this is another service.
1. Starts with "@" means that this is parameter.
1. Starts with "*" means that this is resource (bunch of parameters with same group name)
1. Starts with "$" means that this is dynamic injection (see below).
1. Any primitive types.

### Other options

###### Init

Sometimes instantiation of the service is complicated, so can not be done with just simple `new` call.
Use `init` option to provide callable that must return instance of the service.

```php
<?php

return [
    'my_service' => [
        'init' => function(Container $container) {
            // You can get other staff from $container
            $my_dep = $container->get('my_dep');

            $my_class = new MyClass();
            $my_class->setMyDep($my_dep);

            return $my_class;
        }
    ]
];
```

###### After

Sometimes it is needed to do some actions right after instantiation.

```php
<?php

return [
    'my_service' => [
        'class' => 'MyPackage\\Service\\MyClass',
        'after' => function(Container $container, MyClass $my_class) {
            $my_dep = $container->get('my_dep');

            $my_class->setMyDep($my_dep);

            return $my_class;
        }
    ]
];
```

###### Instantiating from static method

Sometimes instantiating of the class is hidden to static method. For example, `MyClass::makeInstance()`.
In this case keyword `static` will do what is needed.

```php
<?php

return [
    'my_service' => [
        'class' => 'MyPackage\\Service\\MyClass',
        'static' => 'makeInstance'
    ]
];
```

### How to call service in controller

```php
class MyClass extends LayoutController
{
    public function get()
    {
        // Getting instance of "my_service" service
        $my_instance = $this->s('my_service');
    }
}
```

### Dynamic injection

Sometimes it is needed to inject dependencies to service definition in runtime.
In this case write definition like following:

```php
<?php

return [
    'my_service' => [
        'class' => 'MyPackage\\Service\\MyClass',
        'arguments' => ['$1', '$2']
    ]
];
```

And then call it in controller:

```php
class MyController extends LayoutController
{
    public function get()
    {
        $my_instance = $this->s('my_service', ['first_dep', 'second_dep']);
    }
}
```

### Reference

- shared [bool=false] - Whether service is "singleton".
- class [string] - Full name of the class.
- arguments [array] - list of arguments to inject into constructor.
- init [callable] - custom instantiation function.
- after [callable] - Callable that will be called after instantiation.
- static [string] - Static function that is called to instantiate object.