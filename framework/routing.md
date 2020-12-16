---
layout: default
title: Routing
nav_order: 3
parent: Framework
---

Routing consists of 2 stages: host routing and url routing.
Host routing is dealt by Gateway.
URL routing is dealt by 1 of 2 provided routers.

### Gateway

Gateway is a special class which handles host routing.
Its configuration is very similar to Application's one.
Gateway's purpose is to define module to proxy request.
Fast example:

```php
use Perfumer\Framework\Gateway\CompositeGateway;

class Gateway extends CompositeGateway
{
    protected function configure(): void
    {
        // Pass "example.local" host in "dev" environment to "controller" module
        $this->addBundle('controller', 'example.local', null,    'http', 'dev');

        // Pass "example.test" host in "test" environment to "controller" module
        $this->addBundle('controller', 'example.test',  null,    'http', 'test');

        // Pass "example.com" host in "prod" environment to "controller" module
        $this->addBundle('controller', 'example.com',   null,    'http', 'prod');

        // Pass "example.local/ajax/" urls in "dev" environment to "ajax" module
        $this->addBundle('ajax',       'example.local', '/ajax', 'http', 'dev');
    }
}
```

Actually, the last rule should be at the top as first acceptable rule will be proxied to.

### Router

At the second stage request is handled by router. Router defines controller by URL.
Framework suggests 2 routers: simple canonical router and FastRouter from [https://github.com/nikic/FastRoute](https://github.com/nikic/FastRoute).

##### Default canonical router

This router suggests simple canonical rule: routes like `/my/awesome/page` go to `My\Awesome\PageController`.
The benefits of such router is that you don't have to write route rules.
It will be useful in admin interfaces or other sites which doesn't require beautiful routes.

How to get request variables:

```php
class PageController extends TemplateController
{
    protected function get()
    {
        // get "name" field either from query string or from body
        $name = $this->f('name');
    }
}
```

Additionally, there is possibility to add numeric value to the end of the URL.
For example, `/my/awesome/page/100`.
It is useful to pass some ID-like value, for instance.
This URL will also goes to `My\Awesome\PageController`, but with additional request variable:

```php
class PageController extends TemplateController
{
    protected function get()
    {
        // get numeric value from the end of URL
        $id = $this->i();
    }
}
```

##### FastRoute router

This is open-source router from [https://github.com/nikic/FastRoute](https://github.com/nikic/FastRoute).
Generally, route rules are situated at `src/MyPackage/Resource/config/router.php`

Routes can be added like the following:

```php
<?php

return [
    'fast_router' => [
        'shared' => true,
        'init' => function (\Perfumer\Component\Container\Container $container) {
            return \FastRoute\simpleDispatcher(function (\FastRoute\RouteCollector $r) {
                $r->addRoute('GET', '/hello/{name}', 'hello.get');
               
            });
        }
    ],
];
```

- 1 argument is request method
- 2 argument is URL pattern (go to github documentation to learn more options)
- 3 argument is resource-action string. "hello.get" means that this string goes to `HelloController` and `get()` action.

How to get request variables:

```php
class PageController extends TemplateController
{
    protected function get()
    {
        // get "name" field either from query string or from body or from pattern
        $name = $this->f('name');
    }
}
```