---
layout: default
title: Templating
nav_order: 11
parent: Framework
---

### Templating

Framework supports 2 types of templating systems at the moment: [Twig](https://twig.symfony.com/) and [Plates](https://platesphp.com/).

### Twig

This is the main templating system.
Generally there is no much differences to [official guide](https://twig.symfony.com/) except a couple moments.

As all controllers and templates belong to some module you must specify module name in tags which deal with file system such as `extends` or `include`.

For example:

```twig
{% extends "base" %}

{% include "header" %}
```

should be converted to 

```twig
{% extends tpl(app.module, 'base') %}

{% include tpl(app.module, 'header') %}
```

### Twig helpers

#### Request

This helper renders content of the controller. Optionally, you can cache it.

```twig
{{ request('my_module', 'snippet', 'someBlock', ['foo', 'bar']) }}
``` 

will call controller `SnippetController`, action `snippet` in `my_module` module with arguments `$foo, $bar` and echo'es its content.
If you want to cache it call:

```twig
{{ request('my_module', 'snippet', 'someBlock', ['foo', 'bar'], 'cache_key', 3600) }}
``` 

### Translating

To translate call:

```twig
{{ t('translation_key') }}
```

To translate countable words:

```twig
{{ tc('translation_key', 10) }}
```

### WIP: Plates