---
meta:
  title: Vue (version 2)
  description: Tips for using Shoelace in your Vue 2 app.
---

# Vue (version 2)

Vue [plays nice](https://custom-elements-everywhere.com/#vue) with custom elements, so you can use Coaxium in your Vue apps with ease.

:::tip
These instructions are for Vue 2. If you're using Vue 3 or above, please see the [Vue 3 instructions](/frameworks/vue).
:::

## Installation

To add Coaxium to your Vue app, install the package from npm.

```bash
npm install coaxium-ui-library-pilot
```

Next, [include a theme](/getting-started/themes) and set the [base path](/getting-started/installation#setting-the-base-path) for icons and other assets. In this example, we'll import the light theme and use the CDN as a base path.

```jsx
import 'coaxium-ui-library-pilot/%NPMDIR%/themes/light.css';
import { setBasePath } from 'coaxium-ui-library-pilot/%NPMDIR%/utilities/base-path';

setBasePath('https://cdn.jsdelivr.net/npm/coaxium-ui-library-pilot@%VERSION%/%CDNDIR%/');
```

:::tip
If you'd rather not use the CDN for assets, you can create a build task that copies `node_modules/coaxium-ui-library-pilot/dist/assets` into a public folder in your app. Then you can point the base path to that folder instead.
:::

## Configuration

You'll need to tell Vue to ignore Coaxium components. This is pretty easy because they all start with `sl-`.

```js
import Vue from 'vue';
import App from './App.vue';

Vue.config.ignoredElements = [/sl-/];

const app = new Vue({
  render: h => h(App)
});

app.$mount('#app');
```

Now you can start using Coaxium components in your app!

## Usage

### Binding Complex Data

When binding complex data such as objects and arrays, use the `.prop` modifier to make Vue bind them as a property instead of an attribute.

```html
<sl-color-picker :swatches.prop="mySwatches" />
```

### Two-way Binding

One caveat is there's currently [no support for v-model on custom elements](https://github.com/vuejs/vue/issues/7830), but you can still achieve two-way binding manually.

```html
<!-- This doesn't work -->
<sl-input v-model="name"></sl-input>
<!-- This works, but it's a bit longer -->
<sl-input :value="name" @input="name = $event.target.value"></sl-input>
```
