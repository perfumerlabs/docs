---
layout: default
title: Validation cycle
nav_order: 2
parent: Perfumer
---

Perfumer constructs validation cycle in each generated method.
Initially, validation is positive.
As long as state is positive method execution lasts from the beginning to the end.

Annotations like @ErrorMessage change validation state to negative.
They have key property "unless" or "if" with the name of variable which is checked.
If this condition is met, everything after the line where that variable is initialised, will not be executed.
Instead @ErrorMessage annotations will return some value which is passed to method return statement.

```php
interface SampleContract
{
    /**
     * @SetFromField(name="param1")
     * @SetFromField(name="param2")
     *
     * @Sum(a="param1", b="param2", out="sum")
     *
     * If param1 is false or null, @Sum annotation will not be executed, even if @ErrorMessage is declared after @Sum
     * @ErrorMessage(message="Param1 shold not be empty", unless="param1")
     */
    public function get();
}
```

Once validation state has changed from positive to negative, it can not be reverted.