---
layout: default
title: Caching
nav_order: 10
parent: Framework
---

### Caching

Framework uses [Stash Cache](https://github.com/tedious/Stash) library to provide caching possibilities.

[Stash Cache](https://github.com/tedious/Stash) features:

- Handling [dog-pile effect](https://en.wikipedia.org/wiki/Cache_stampede)
- PSR-6 support
- Multiple drivers

Fast example:

```php
class MyController extends TemplateController
{
    protected function get()
    {
        $cache = $this->s('cache');
        
        $item = $cache->getItem('my_cache_key');

        if ($item->isHit()) {
            $content = $item->get();
        } else {
            $item->lock();

            $content = $this->getMyContentToCache();

            $item->set($content);
            $item->expiresAfter(3600);
            $item->save();
        }

        // use $content further
    }
}
```

Go to [Stash Cache](https://github.com/tedious/Stash) to learn more features.