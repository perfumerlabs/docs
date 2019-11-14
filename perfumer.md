Perfumer is generator of PHP classes.

### Basic example

We have a class with math functions.

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

We want to make a class with method sumThenMultiply():

```php
interface MyClass
{
    /**
     * This method gets the result of ($m + $n) * $k
     *
     * Get the sum of $n and $m, return result to new variable $x
     * @Math\Sum(a="n", b="m", out="x")
     *
     * Gets the multiplication of $x and $k, return result to new variable $y
     * @Math\Multiply(a="x", b="k", out="y")
     *
     * Method return statement
     * @Out(name="y")
     */
    public function sumThenMultiply(int $m, int $n, int $k): int;
}
```

The interface which is generated is called Contract (MyClass in example).
The class which contains methods for generating is called Context (Math in example).

### Autowire variables

There is no need to specify properties of annotations, if contract method scope has variables with the same name as context method has.

```php
interface MyClass
{
    /**
     * Skip 'a="a", b="b"' declaration 
     * @Math\Sum(out="x")
     *
     * @Math\Multiply(a="x", b="c", out="y")
     * @Out(name="y")
     */
    public function sumThenMultiply(int $a, int $b, int $c): int;
}
```