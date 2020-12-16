---
layout: default
title: Generator
nav_order: 1
has_children: true
---

This is generator of PHP classes.

### How it works

Perfumer based on Doctrine Annotations library.
When you want to generate some class you create interface and put some annotations to it.
Perfumer creates a class implementing that interface and generates some piece of code for each annotation declared.

### Concept

Mainly, Generator is used in Framework to generate controllers (although it can generate any class). 

Every controller has the same lifecycle:

1. Set initial variables from request/session/config/etc.
2. Validate initial variables.
3. Return error message if validation fails.
4. Call business logic if validation succeed.
5. Return content if needed.

Code for steps 1,2,3 and 5 almost the same from case to case.
Perfumer tries to automate this boilerplate code writing by providing a number of different annotations.
Of course, you can write your own annotations dealing with complicated setting of variables or validation.


Step 4 is what application is, so Perfumer can not help with that :)
But Perfumer can generate annotations for each method of each business logic class, which can be used to call that method in controller class.

### Fast example

Consider, we want to write some controller that gets a number of request variables, validates them,
do some math operations and return content to response.

We have a class with math functions. This is our "business logic" class in terms of application.

```php
class Math
{
    public function sum($a, $b)
    {
        return $a + $b;
    }

    public function multiply($a, $b)
    {
        return $a * $b;
    }
}
```

Suppose, we configured Perfumer properly and got 2 annotations for "sum" and "multiply" methods.

Lets create controller. Annotations must be written to abstract method. So, controller has to be interface or abstract class.

```php
interface MyController
{
    /**
     * Get "a" field from request body, cast it to int and set it to new variable $a
     * @SetFromIntField(name="a", value="a")
     *
     * Same with "b" and "c"
     * @SetFromIntField(name="b", value="b")
     * @SetFromIntField(name="c", value="c")
     *
     * Check if "a" is a positive integer, return bool to new variable $a_valid
     * @PositiveInteger(v="a", out="a_valid")
     *
     * Same with "b" and "c"
     * @PositiveInteger(v="b", out="b_valid")
     * @PositiveInteger(v="c", out="c_valid")
     *
     * Do business logic
     * @Sum(a="a", b="b", out="x")
     * @Multiply(a="x", b="c", out="y")
     *
     * Return $y as content to client with the key "result"
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
```

Without all comments:

```php
interface MyController
{
    /**
     * @SetFromIntField(name="a", value="a")
     * @SetFromIntField(name="b", value="b")
     * @SetFromIntField(name="c", value="c")
     *
     * @PositiveInteger(v="a", out="a_valid")
     * @PositiveInteger(v="b", out="b_valid")
     * @PositiveInteger(v="c", out="c_valid")
     *
     * @Sum(a="a", b="b", out="x")
     * @Multiply(a="x", b="c", out="y")
     *
     * @Content(name="result", value="y")
     *
     * @ErrorMessage(message="Must be positive integer", unless="a_valid")
     * @ErrorMessage(message="Must be positive integer", unless="b_valid")
     * @ErrorMessage(message="Must be positive integer", unless="c_valid")
     */
    public function get();
}
```

More improvements:

1. @SetFromIntField(name="a", value="a") can be replaced with just @SetFromIntField(name="a")
2. @Sum(a="a", b="b", out="x") can be replaced just with @Sum(out="x")

Final version

```php
interface MyController
{
    /**
     * @SetFromIntField(name="a")
     * @SetFromIntField(name="b")
     * @SetFromIntField(name="c")
     *
     * @PositiveInteger(v="a", out="a_valid")
     * @PositiveInteger(v="b", out="b_valid")
     * @PositiveInteger(v="c", out="c_valid")
     *
     * @Sum(out="x")
     * @Multiply(a="x", b="c", out="y")
     *
     * @Content(name="result", value="y")
     *
     * @ErrorMessage(message="Must be positive integer", unless="a_valid")
     * @ErrorMessage(message="Must be positive integer", unless="b_valid")
     * @ErrorMessage(message="Must be positive integer", unless="c_valid")
     */
    public function get();
}
```

### Terms

The interface which is generated is called Contract (MyController in example).

The class which methods we call by annotations is called Context (Math in example).