---
layout: default
title: View
nav_order: 4
parent: Framework
---

### View

View is a layer which deals with response rendering.
Fast example:

```php
class MyController extends PlainController
{
    protected function get()
    {
        $view = $this->getView();

        $view->setTemplate('my_tpl');

        $view->addVars([
            'var1' => 'foo',
            'var2' => 'bar',
        ]);

        $content = $view->render();
    }
}
```

HINT. If controller extends TemplateController, it is not needed to call `render()` method as it will be called explicitly.

### Variable groups

Sometimes it is needed to add an array-like variable, then somehow change particular field in that array and reassign array to the view.
In this case View suggests convenient group API.

```php
class MyController extends PlainController
{
    protected function get()
    {
        $view = $this->getView();

        // Assign variable "var1"
        $view->addVars([
            'var1' => 'val1',
        ]);

        // View vars become:
        // $vars = [
        //  'var1' => 'val1'
        //];

        // Add a group
        $view->addGroup('group1');

        // Now View vars become:
        // $vars = [
        //  'var1' => 'val1',
        //  'group1' => []
        //];

        // Assign variable to the group1
        $view->addVar('var2', 'val2', 'group1');

        // Now View vars become:
        // $vars = [
        //  'var1' => 'val1',
        //  'group1' => [
        //      'var2' => 'val2'
        //  ]
        //];

        // Lets go deeper
        $view->addGroup('group2', 'group1');

        // Now View vars become:
        // $vars = [
        //  'var1' => 'val1',
        //  'group1' => [
        //      'var2' => 'val2'
        //      'group2' => []
        //  ]
        //];

        // Assign variable to the group2
        $view->addVar('var3', 'val3', 'group2');

        // Now View vars become:
        // $vars = [
        //  'var1' => 'val1',
        //  'group1' => [
        //      'var2' => 'val2'
        //      'group2' => [
        //          'var3' => 'val3'
        //      ]
        //  ]
        //];
    }
}
```

### Handle vars in view

There are various methods to handle variables:

```php
interface ViewInterface
{
    /**
     * gets a variable by name
     */
    public function getVar($name, $group = null);

    /**
     * gets the group of variables
     */
    public function getVars($group = null);

    /**
     * adds a variable
     */
    public function addVar($name, $value, $group = null);

    /**
     * add an array of variables
     */
    public function addVars(array $vars, $group = null);

    /**
     * check if variable exists
     */
    public function hasVar($name, $group = null);

    /**
     * check if group exists
     */
    public function hasVars($group = null);

    /**
     * deletes a variable
     */
    public function deleteVar($name, $group = null);

    /**
     * deletes a group
     */
    public function deleteVars($group = null);
}
```

### Serialize view 

SerializeView differs from TemplateView with that SerializeView does not have a template,
and `render()` method returns serialized content.