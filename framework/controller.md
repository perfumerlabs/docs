---
layout: default
title: Controller
nav_order: 2
parent: Framework
---

Controller is the part of project where a request handling starts from.

### Actions

Action is a method which handles particular request.
Normally, actions are named with the names of request methods.
For example, `GET` request goes to `get()` action, `POST` request goes to `post()` action.
Though this behaviour is configurable.

### Before/After

If you want to write some logic which must be executed before all actions in the controller, use `before()` method.

```php
class MyController extends PlainController
{
    protected function before(): void
    {
        // execute parent "before" method first
        parent::before();

        // this code will be executed before "get" action
    }

    protected function get()
    {

    }
}
```

The same is with the code after the action:

```php
class MyController extends PlainController
{
    protected function after(): void
    {
        // this code will be executed after "get" action

        // execute parent "after" method at last
        parent::after();
    }

    protected function get()
    {

    }
}
```

### Request parameters

To get request parameters use special `f` function:

```php
class MyController extends PlainController
{
    // actually, "f" is a shortcut, which is situated in this trait
    use DefaultRouterControllerHelpers;

    protected function get()
    {
        $name = $this->f('name');
    }
}
```

More about request parameters can be found in Routing chapter.

### Template controller

Template controllers are used when it is needed to render html or other text content.

```php
class MyController extends TemplateController
{
    protected function get()
    {
        $name = $this->f('name');
    
        // pass "name" variable to template
        // more about View options is in View chapter
        $this->getView()->addVar('name', $name);
    }
}
```

You don't need to set template name in action as controller automatically set a template by following convention.
The name of template would be `<templates-dir>/<controller-name>/<action-name>`.
For example, template name for `MyPageController` would be `<templates-dir>/my-page/get.twig`.
 
 ### Serialize controller
 
 Serialize controller is used to render content as serialized string.
 For example, to json in ajax requests.
 
 ```php
 class MyController extends SerializeController
 {
     protected function get()
     {
         $name = $this->f('name');
     
         $this->getView()->addVar('name', $name);
     }
 }
 ```

As the default serialization is JSON this controller will output `{"name": "<name>"}`.

### Status controller

Status controller is the child of SerializeController.
It sets some predefined structure to json-content.
Add `StatusViewControllerHelpers` to add some useful methods also to controller.

StatusView defines 4 fields in response: `status`, `message`, `errors`, `content`.

- `status` - is a `boolean` which tells the result of request in common, i.e. if it `false` it means that request was insuccessful generally.
- `message` - is a `string` which brings a text information about the request. For example, it can be "You successfully done this action".
- `errors` - is an `object` which contains error messages about some fields. For example, it can be used to return form fields error messages.
- `content` - is a `mixed` value. It can any data here.

Example:

```json
{
  "status": true,
  "message": "You successfully created an object",
  "content": {
    "object_fields": {
      "id": 1,
      "name": "Awesome object"
    }
  }
}
```

If `StatusViewControllerHelpers` trait is added there are some useful methods to handle `StatusView` content:

```php
class MyController extends StatusController
{
    protected function get()
    {
        // Sets message and status to true
        $this->setSuccessMessage('You are succeed');

        // Sets message and status to false, exits action form here
        $this->setErrorMessageAndExit('You failed');

        // Sets content
        $this->setContent(['foo' => 'bar']);
    
        // Adds an error to errors array
        $this->addError('name', 'This field is required');

        // and more
    }
}
```

### Plain Controller

Plain controller is the simplest one. It outputs nothing.
For example, it is used in controllers returning files for downloading.

### Requesting in controllers

Inside each controller you can request to another controller.

##### Execute controller

This is used if you want to execute another controller and receive it content.
Pass 3 parameters: controller name, action and action arguments.

```php
class SnippetController extends TemplateController
{
    // This controller renders some html snippet
    protected function someBlock($foo, $bar)
    {
        $this->getView()->addVars([
            'foo' => $foo,
            'bar' => $bar,
        ]);
    }
}

class MyController extends TemplateController
{
    protected function get()
    {
        $response = $this->execute('snippet', 'someBlock', ['foo', 'bar']);
    
        // get html content
        $snippet = $response->getContent();

        $this->getView()->addVars([
            'snippet' => $snippet,
        ]);
    }
}
```

##### Forwarding

This feature is used when it is needed to "redirect" internally to another controller

```php
class SnippetController extends TemplateController
{
    // This controller renders some html page
    protected function somePage($foo, $bar)
    {
        $this->getView()->addVars([
            'foo' => $foo,
            'bar' => $bar,
        ]);
    }
}

class MyController extends TemplateController
{
    protected function get()
    {
        $this->forward('snippet', 'somePage', ['foo', 'bar']);

        // code after "forward" will not be executed
    }
}
```

##### Deferring

This feature is based on `fastcgi_finish_request` PHP function.
After this function executed PHP returns content to web server (and it returns it to browser).
So any code after `fastcgi_finish_request` does not affect client performance.
`defer` registers controller and execute it after response sent to client.
This feature is used to do some long lasting work like email sending, network requests, etc.

```php
class MyController extends TemplateController
{
    protected function get()
    {
        $this->defer('deferred', 'hardWork', ['foo', 'bar']);
    }
}

class DeferredController extends PlainController
{
    protected function hardWork($foo, $bar)
    {
        // do hard work like email sending
    }
}
```