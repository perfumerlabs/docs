---
layout: default
title: Controllers
nav_order: 1
parent: Perfumer
---

The main usage of Perfumer in Framework is to generate controllers. 
Perfumer has a number of annotations to generate different boilerplate code.

### Controller lifecycle

Almost every controller has the same lifecycle:

1. Set initial variables from request/session/config/etc.
2. Validate request variables.
3. Return error message if validation fails.
4. Call business logic if validation succeed.
5. Return content if needed.

### Basic example

```php
interface SampleContract
{
    /**
     * Get "a" field from request body and set it to new variable $a
     * @SetFromIntField(name="a", value="a")
     *
     * Same with "b"
     * @SetFromIntField(name="b", value="b")
     * 
     * We can skip setting "value" property, if variable name is the same as field name
     * @SetFromIntField(name="c")
     *
     * Check if "a" is a positive integer, return bool to new variable $a_valid
     * @PositiveInteger(v="a", out="a_valid")
     *
     * Same with "b" and "c"
     * @PositiveInteger(v="b", out="b_valid")
     * @PositiveInteger(v="c", out="c_valid")
     *
     * Do logic
     * @Sum(out="x")
     * @Multiply(a="x", b="c", out="y")
     *
     * Return content to client
     * @Content(name="result", value="y")
     *
     * Return error messages if "a_valid", "b_valid" or "c_valid" fail
     * @ErrorMessage(message="Must be positive integer", unless="a_valid")
     * 
     * This code will be executed right after "b_valid" initialized.
     * So we don't need to care about where ErrorMessage annotation declared.
     * @ErrorMessage(message="Must be positive integer", unless="b_valid")
     *
     * @ErrorMessage(message="Must be positive integer", unless="c_valid")
     */
    public function get();
}

class SampleContractContext
{
    public function positiveInteger($v)
    {
        return is_int($v) && $v > 0;
    }
}
```