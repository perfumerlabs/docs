---
layout: default
title: Setters in context
nav_order: 2
parent: Perfumer
---

Setters may be declared in context method.

```php
class MyContext
{
    public function doSomething(MyDomain $domain, $arg1, $arg2)
    {
        // do stuff
    }
}
```

We know that MyDomain instance never changes, so we can declare @SetFromDomain here

```php
class MyContext
{
    /**
     * @SetFromDomain(name="domain", value="my_domain")
     */
    public function doSomething(MyDomain $domain, $arg1, $arg2)
    {
        // do stuff
    }
}
```

Perfumer will auto-include this annotation to all contracts where this method is used.