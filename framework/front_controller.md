---
layout: default
title: Front Controller
nav_order: 120
parent: Framework
---

### Front Controller

Front Controller is a php file which is set to nginx host config file as `index` file.
Usually it is situated in the `web/` directory.
Fast example:

```php
<?php

require __DIR__ . '/../vendor/autoload.php';

$application = new \Project\Application();
$application->setEnv('http');
$application->setBuildType('dev');
$application->setFlavor('my_flavor');
$application->run();
```

Front controller is used to set up `env`, `build_type` and `flavor` application parameters (see Application chapter to learn their meanings) and to run the application.

Usually there are 3 front controllers for http env: `prod.php`, `stage.php`, `dev.php`, one for each development environments.

### Console front Controller

Console front Controller are situated in the `cli/` directory.