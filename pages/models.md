---
layout: default
title: Input models
nav_order: 2
parent: Pages
---

In previous section we discover what is Pages and how component works.
Lets add a functionality.

We've described a page as

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

But how to pass input information to the server? First we need to tell to input components model names they hold.
So, out scheme becomes:

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
            "model": "first_name",
            "label": "First name",
            "placeholder": "Type your first name"
        },
        "input2": {
            "type": "input",
            "model": "last_name",
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

Lets add models to component:

```vue
<template>
  <label>{{label}}</label>
  <input type="text" placeholder="{{placeholder}}" v-model="value"/>
</template>

<script>
import StartBase from "./StartBase";

export default {
  name: 'Input',
  extends: StartBase,
  data() {
    return {
      value: '',
      model: '',
      label: '',
      placeholder: '',
    }
  },
  beforeMount() {
    this.model = this.getFromConfig('model');
    this.label = this.getFromConfig('label');
    this.placeholder = this.getFromConfig('placeholder');
  }
}
</script>
```

Now our Input has a model and preserves input information to it. Lets pass models to server, a button deals with it.
We add three parameters to it:

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
            "model": "first_name",
            "label": "First name",
            "placeholder": "Type your first name"
        },
        "input2": {
            "type": "input",
            "model": "last_name",
            "label": "Last name",
            "placeholder": "Type your last name"
        },
        "button1": {
            "type": "button",
            "request_models": ["first_name", "last_name"],
            "request_url": "/path/to/save",
            "request_method": "post",
            "color": "red",
            "name": "Save a form"
        }
    }
}
```

```vue
<template>
  <button :click="click()">{{name}}</button>
</template>

<style scoped lang="scss">
</style>

<script>
import axios from 'axios';
import StartBase from "./StartBase";

export default {
  name: 'Button',
  extends: StartBase,
  data() {
    return {
      name: null,
      request_method: null,
      request_url: null,
      request_models: [],
    }
  },
  beforeMount() {
    this.name = this.getFromConfig('name');
    this.request_method = this.getFromConfig('request_method');
    this.request_url = this.getFromConfig('request_url');
    this.request_models = this.getArrayFromConfig('request_models');
  }
  methods: {
    async click() {
        let models = this.$page.getModels(this.request_models); // collect models from the page

        try {
          let response = null;

          switch (this.request_method) {
            case 'get':
              response = await axios.get(this.request_url, {params: models});
              break;
            case 'post':
              response = await axios.post(this.request_url, models);
              break;
            case 'patch':
              response = await axios.patch(this.request_url, models);
              break;
            case 'delete':
              response = await axios.delete(this.request_url, {params: models});
              break;
          }
        } catch (error) {
        }
    },
  }
}
</script>
```

Now button can collect and send information to the server on click. But 1 thing is missing.
InputComponent has a model, but doesn't provide it to Pages. It is achieved by special `getModels` method:

```vue
<template>
  <label>{{label}}</label>
  <input type="text" placeholder="{{placeholder}}" v-model="value"/>
</template>

<script>
import StartBase from "./StartBase";

export default {
  name: 'Input',
  extends: StartBase,
  data() {
    return {
      value: '',
      model: '',
      label: '',
      placeholder: '',
    }
  },
  beforeMount() {
    this.model = this.getFromConfig('model');
    this.label = this.getFromConfig('label');
    this.placeholder = this.getFromConfig('placeholder');
  },
  methods: {
    getModels() { // getModels must return an object
      if (this.model) {
        let object = {};

        object[this.model] = this.value;

        return object;
      } else {
        return null;
      }
    },
  }
}
</script>
```