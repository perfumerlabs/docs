---
layout: default
title: Dependency Injection
nav_order: 75
parent: Framework
---

Framework is hardly relies on dependency injecting.
You should not instantiate class objects by yourself.
Instead register you service definitions to container and get instances from it.

Framework has own Container implementation based on `PSR-11` standard.

```php
<?php

class MyController extends PlainController
{
    protected function get()
    {
        // get instance of service
        $my_service = $this->s('my_service');
    }
}
```