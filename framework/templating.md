---
layout: default
title: Templating
nav_order: 5
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
{% raw %}
{% extends "base" %}

{% include "header" %}
{% endraw %}
```

should be converted to 

```twig
{% raw %}
{% extends tpl(app.module, 'base') %}

{% include tpl(app.module, 'header') %}
{% endraw %}
```

### Twig helpers

#### Request

This helper renders content of the controller. Optionally, you can cache it.

```twig
{% raw %}
{{ request('my_module', 'snippet', 'someBlock', ['foo', 'bar']) }}
{% endraw %}
``` 

will call controller `SnippetController`, action `snippet` in `my_module` module with arguments `$foo, $bar` and echo'es its content.
If you want to cache it call:

```twig
{% raw %}
{{ request('my_module', 'snippet', 'someBlock', ['foo', 'bar'], 'cache_key', 3600) }}
{% endraw %}
``` 

### Translating

To translate call:

```twig
{% raw %}
{{ t('translation_key') }}
{% endraw %}
```

To translate countable words:

```twig
{% raw %}
{{ tc('translation_key', 10) }}
{% endraw %}
```

### WIP: Plates