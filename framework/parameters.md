---
layout: default
title: Parameters
nav_order: 7
parent: Framework
---

### Parameters

Parameters are written in separate config files, usually it is `MyPackage/Resource/config/resources.php`,
though it depends from particular project.

Fast example:

```php
<?php

return [
    'my_group' => [
        'my_param1' => true,
        'my_param2' => 111,
        'my_param3' => 'foobar',
    ]
];
```

Parameter must be part of some group.

### How to get parameter in controller

```php
class MyController extends LayoutController
{
    public function get()
    {
        // Getting parameter "my_param1" of "my_group" group
        $my_param1 = $this->getContainer()->getParam('my_group/my_param1');
    }
}
```

### How to use parameter in service definition

```php
<?php

return [
    'my_service' => [
        'class' => 'MyPackage\\Service\\MyClass',
        'arguments' => ['@my_group/my_param1']
    ]
];
```