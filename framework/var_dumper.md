---
layout: default
title: VarDumper
nav_order: 14
parent: Framework
---

### VarDumper

Since 0.13 version Framework supports [Symfony VarDumper](https://symfony.com/doc/current/components/var_dumper.html) component.

To enable it, first, require "symfony/var_dumper" to composer.json.
After that `dump()` function is already included and can be used.

If you want to enable [var-dump-server](https://symfony.com/doc/current/components/var_dumper.html#the-dump-server), you have to turn on `var_dumper/remote` parameter in env.php (or any other resources file):

```php
'var_dumper' => [
    'remote' => true,
]
```

#### Redefine VarDumper server URL

Framework predefine server url as mentioned in Symfony docs to `tcp://127.0.0.1:9912`.
If you want to change it redefine next parameter:

```php
'var_dumper' => [
    'server' => 'tcp://my-new-host:123',
]
```