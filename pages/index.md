---
layout: default
title: Pages
nav_order: 4
has_children: true
---

`Perfumer Pages` is json-based protocol that describes, how API can construct UI pages.
Currently, Pages has implementation on VueJS 2.

To understand how Pages works, suppose we have a couple of VueJS components: InputComponent (draws input[type=text]) and ButtonComponent (draws button).
These components somehow receive parameters. For example, InputComponent can have `label` and `placeholder` parameters,
ButtonComponent can have `color` and `name` (text on the button) parameter.

Then, if we want to output a page which has 2 inputs and a button, we can describe this page with following json:

```json
{
    "build": [
        "input1",
        "input2",
        "button1"
    ],
    "components": {
        "input1": {
            "type": "input",
            "label": "First name",
            "placeholder": "Type your first name"
        },
        "input2": {
            "type": "input",
            "label": "Last name",
            "placeholder": "Type your last name"
        },
        "button1": {
            "type": "button",
            "color": "red",
            "name": "Save a form"
        }
    }
}
```

where:

- `components` section is a registry of blocks to render
- `build` is an order of blocks in the page
- `input1`, `input2`, `button1` - are names of specific blocks
- `type` is a name of component to use
- `label`, `placeholder`, `color`, `name` are parameters to pass to components

So, in Pages you don't design your pages with html or js templating system.
Instead, you prepare components, then call backend to receive a page `scheme` in json format above and render that json `scheme` in browser with special engine.

### Flow

Flow of Pages rendering is following:

1. This is a classic SPA. So, when browser loads first page, initially an empty page is loaded with all CSS and JS in it.
2. Then Pages engine is being loaded.
3. Pages call to server for scheme for homepage.
4. Server returns scheme.
5. Pages parses scheme, looks for block components in its list of registered components, and if they are, renders blocks with order provided in `build` section.
6. When user clicks on the link, Pages opens new page through `vue-router`.
7. Then Pages get link path, send it to server, receive new `scheme` of the page, render it, and so on.

### Component

Components in Pages must follow some conventions.

For example, InputComponent can be as following:

```vue
<template>
  <label>{{label}}</label>
  <input type="text" placeholder="{{placeholder}}"/>
</template>

<script>
import StartBase from "./StartBase";

export default {
  name: 'Input',
  extends: StartBase, // every component must extend StartBase
  data() {
    return {
      label: '',
      placeholder: '',
    }
  },
  beforeMount() {
    this.label = this.getFromConfig('label'); // get "label" from block parameter in scheme
    this.placeholder = this.getFromConfig('placeholder'); // get "placeholder" from block parameter in scheme
  }
}
</script>
```

ButtonComponent can be as following:

```vue
<template>
  <button>{{name}}</button>
</template>

<script>
import StartBase from "./StartBase";

export default {
  name: 'Input',
  extends: StartBase,
  data() {
    return {
      name: '',
    }
  },
  beforeMount() {
    this.name = this.getFromConfig('name'); // get "name" from block parameter in scheme
  }
}
</script>
```