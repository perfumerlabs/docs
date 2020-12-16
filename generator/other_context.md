---
layout: default
title: Other contexts
nav_order: 4
parent: Generator
---

In this chapter we consider special types of contexts.

### Vars to array formatter

Sometimes we want to collect some variables to assoc array. If we write regular Context, we can do it like this:

```php
class MyFormatter
{
    public function format($a, $b, $c): array
    {
        return [
            'a' => $a,
            'b' => $b,
            'c' => $c,
        ];
    }
}
```

To automate this work Perfumer suggests @VarsToAssocArray annotation:

```php
/**
 * @AnnotationExtends(class="Perfumerlabs\PerfumerFrameworkPack\Context\VarsToAssocArray")
 */
interface MyFormatter
{
    public function format($a, $b, $c): array;
}
```

After regenerating Perfumer will generate array by itself.