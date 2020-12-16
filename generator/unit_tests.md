---
layout: default
title: Unit tests
nav_order: 6
parent: Generator
---

## Unit tests generating

Methods of Context are subject to test. Perfumer auto-generates all boilerplate code for unit test (based on phpunit).

To generate test add @Test annotation to desirable method.

```php
use Perfumerlabs\Perfumer\ContextAnnotation\Test;

class Math
{
    /**
     * @Test
     */
    public function sum($a, $b)
    {
        return $a + $b;
    }
}
```

After generating Perfumer creates new file in "tests" directory.

```php
class MathTest extends \Generated\Tests\Context\MathTest
{
    public function sumDataProvider()
    {
        throw new \Exception('Method "sumDataProvider" is not implemented yet.');
    }
}
```

Method "sumDataProvider" is a data provider method in terms of phpunit.
Read more about data providers [here](https://phpunit.readthedocs.io/en/8.4/writing-tests-for-phpunit.html#data-providers).

First several arguments (in the example - two) are method arguments, the last argument is what method must return.

After implementing data provider we can get the code like this.

```php
class MathTest extends \Generated\Tests\Context\MathTest
{
    public function sumDataProvider()
    {
        return [
            [1, 2, 3],
            [2, 2, 4],
        ];
    }
}
```

The test is done!

### Overriding assertion function

By default Perfumer applies "assertEquals" assertion to test. 
If another assertion function is needed you have to override "assertTest{method_name}" test method.

```php
class MathTest extends \Generated\Tests\Context\MathTest
{
    public function sumDataProvider()
    {
        return [
            [1, 2, 3],
            [2, 2, 4],
        ];
    }
    
    protected function assertTestSum($expected, $result)
    {
        // Override this assertion if needed
        $this->assertEquals($expected, $result);
    }
}
```