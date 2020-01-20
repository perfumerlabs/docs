---
layout: default
title: Authentication
nav_order: 10
parent: Framework
---

### Authentication

Authentication of Framework is built on own library.
This library doesn't use PHP Session API.
The reasons are:

- Too mush unsafe parameters to mind of.
- No possibility to work with multiple sessions in one request
- No possibility to handle session transport. Only cookie is supported.

### WIP: Registration and login

Currently, there is no built-in tools.
You should do the work by PHP Password API.

### Authenticating

To get the user ID or user model call:

```php
class MyController extends TemplateController
{
    protected function get()
    {
        $user_id = $this->getUserId();

        $user = $this->getUser();
    }
}
```