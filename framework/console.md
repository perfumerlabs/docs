---
layout: default
title: Console Commands
nav_order: 13
parent: Framework
---

### Console Commands

Console commands by default are situated at `src/MyPackage/Command/` directory.
To call command in console provide 3 parameters: front controller, module name, controller name.
For example:

```bash
php cli/dev my_module do/something
```

will execute `src/MyPackage/Command/Do/SomethingCommand`.

By default, request goes to `action()` action in commands. Though it configurable.

```php
class MyCommand extends PlainController
{
    public function action()
    {
        // write staff here
    }
}
```

### Parameters

Bash supports providing options and arguments to console commands.
All of them can be obtained on command.

```bash
php cli/dev my_module make -s 123 --long 456 -- foo bar
```

There are 1 short option (`s`), 1 long option (`long`) and 2 arguments.

```php
class MakeCommand extends PlainController
{
    public function action()
    {
        // Getting options
        $s = $this->o('s');
        $long = $this->o('long');

        // Getting arguments
        $foo = $this->a(0);
        $bar = $this->a(1);
    }
}
```

Common practice is to have 1 short and 1 long option for the same parameter.

```php
class MakeCommand extends PlainController
{
    public function action()
    {
        // Getting either short or long option
        $p = $this->o('p', 'param', 'default_value');

        // Third argument is a default value if no option was provided, null by default
    }
}
```

### Command as controller

Commands in Framework are just subtype of controllers, so all controller possibilities can be used in commands.

```php
class MakeCommand extends PlainController
{
    public function action()
    {
        $p = $this->o('p', 'param');

        $response = $this->getProxy()->execute('snippet_module', 'some/staff', 'someBlock', [$p]);

        echo $response->getContent();
    }
}
```