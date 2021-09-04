---
layout: default
title: Scheme Parameters
nav_order: 4
parent: Pages
---

Pages scheme has multiple built-in parameters.

### Redirect

```json
{
    "redirect": "/new/page/path"
}
```

If this scheme is returned Pages will redirect location to "/new/page/path" through vue-router.

### Href

```json
{
    "href": "/new/page/path"
}
```

If this scheme is returned Pages will redirect location to "/new/page/path" through `location.replace()`.

### Secured

```json
{
    "secured": true
}
```

If this scheme is returned Pages will redirect to login page.

### Content URL

```json
{
    "content_url": "/ajax/some/content",
    ... blocks and components ...
}
```

If `content_url` is provided, then Pages on page load will do ajax request to this location,
and apply received content to components.

### Content models

```json
{
    "content_models": "/ajax/some/content",
    ... blocks and components ...
}
```

If `content_models` is provided, then Pages on page load will do ajax request to this location,
and apply received models to input components.