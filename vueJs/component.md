# Component

- [Component](#component)
  - [Component Names](#component-names)
  - [Global Registration](#global-registration)
  - [Local Registration](#local-registration)
  - [Props](#props)
    - [two way to define](#two-way-to-define)
    - [one way flow](#one-way-flow)
    - [Prop Validation](#prop-validation)
  - [slot](#slot)
    - [Compilation Scope](#compilation-scope)
    - [Fallback Content](#fallback-content)
    - [Named Slots](#named-slots)
    - [Scoped Slots](#scoped-slots)


Here’s an example of a Vue component:

```
// Define a new component called button-counter
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```

> Note: since we are using `Vue.component(..)` to define and register the component so it actually
> becomes the `global` component, which may not be ideal generally and will increase the bundle size.

>Another approach would be define a `plain javascript object` and then use `components` props to register it. that way component will local to that specific component hence no large bundle size.

```
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

> Since we are using `data attribute as function and not as object` due to this fact every object has its own state so you can actually use same objects multiple time without any worry.


Components can be reused as many times as you want:

```
<div id="components-demo">
  <button-counter></button-counter>
  <button-counter></button-counter>
  <button-counter></button-counter>
</div>
```

Notice that when clicking on the buttons, each one maintains its own, separate count. That’s because each time you use a component, a new instance of it is created.

## Component Names
When registering a component, it will always be given a name. For example, in the global registration we’ve seen so far:

```
Vue.component('my-component-name', { /* ... */ })
```

or

```
Vue.component('MyComponentName', { /* ... */ })
```

## Global Registration
So far, we’ve only created components using Vue.component:
```


Vue.component('my-component-name', {
  // ... options ...
})
```

These components are `globally registered`. That means they can be used in the template of any root Vue instance (new Vue) created after registration. For example:

## Local Registration

Global registration often isn’t ideal. For example, if you’re using a build system like Webpack, globally registering all components means that even if you stop using a component, it could still be included in your final build. This unnecessarily increases the amount of JavaScript your users have to download.

In these cases, you can define your components as plain JavaScript objects:

```
var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
```

Then define the components you’d like to use in a `components` option:

```
new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
```
>Note that locally registered components are not also available in subcomponents. For example, if you wanted ComponentA to be available in ComponentB, you’d have to use:

```
var ComponentA = { /* ... */ }

var ComponentB = {
  components: {
    'component-a': ComponentA
  },
  // ...
}
```

Or if you’re using ES2015 modules, such as through Babel and Webpack, that might look more like:

```
import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  },
  // ...
}
```


## Props

### two way to define

```
    props: ['title', 'likes', 'isPublished', 'commentIds', 'author']

//or 

    props: {
    title: String,
    likes: Number,
    isPublished: Boolean,
    commentIds: Array,
    author: Object,
    callback: Function,
    contactsPromise: Promise // or any other constructor
    }

    //or even you can have


```

### one way flow
All props form a one-way-down binding between the child property and the parent one: when the parent property updates, it will flow down to the child, but not the other way around. 

In addition, every time the parent component is updated, all props in the child component will be refreshed with the latest value. This means you should not attempt to mutate a prop inside a child component. If you do, Vue will warn you in the console.

### Prop Validation

Components can specify requirements for their props, such as the types you’ve already seen. If a requirement isn’t met, Vue will warn you in the browser’s JavaScript console. This is especially useful when developing a component that’s intended to be used by others.

To specify prop validations, you can provide an object with validation requirements to the value of props, instead of an array of strings. For example:

```
Vue.component('my-component', {
  props: {
    // Basic type check (`null` and `undefined` values will pass any type validation)
    propA: Number,
    // Multiple possible types
    propB: [String, Number],
    // Required string
    propC: {
      type: String,
      required: true
    },
    // Number with a default value
    propD: {
      type: Number,
      default: 100
    },
    // Object with a default value
    propE: {
      type: Object,
      // Object or array defaults must be returned from
      // a factory function
      default: function () {
        return { message: 'hello' }
      }
    },
    // Custom validator function
    propF: {
      validator: function (value) {
        // The value must match one of these strings
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

## slot

Vue implements a content distribution API inspired by the Web Components spec draft, using the `<slot>` element to serve as distribution outlets for content.

This allows you to compose components like this:

> If `<navigation-link>`‘s template did not contain a `<slot>` element, any content provided between its opening and closing tag would be discarded.

```
<navigation-link url="/profile">
  Your Profile
</navigation-link>
```

Then in the template for `<navigation-link>`, you might have:

```
    <a
    v-bind:href="url"
    class="nav-link"
    >
    <slot></slot>
    </a>
```
When the component renders, `<slot></slot>` will be replaced by “Your Profile”. Slots can contain any template code, including HTML:


```
    <navigation-link url="/profile">
    <!-- Add a Font Awesome icon -->
    <span class="fa fa-user"></span>
    Your Profile
    </navigation-link>

// Or even other components:

    <navigation-link url="/profile">
    <!-- Use a component to add an icon -->
    <font-awesome-icon name="user"></font-awesome-icon>
    Your Profile
    </navigation-link>

```


### Compilation Scope
When you want to use data inside a slot, such as in:

> Everything in the parent template is compiled in parent scope; everything in the child template is compiled in the child scope.


```
<navigation-link url="/profile">
  Logged in as {{ user.name }}
</navigation-link>
```

That slot has access to the same instance properties (i.e. the same “scope”) as the rest of the template. The slot does not have access to `<navigation-link>`‘s scope. For example, trying to access url would not work:


### Fallback Content

```
<button type="submit">
  <slot>Submit</slot> //here Submit is fallback content when child component do not pass any content
</button>
```

### Named Slots
There are times when it’s useful to have multiple slots. For example, in a `<base-layout>` component with the following template:

>A `<slot>` outlet without name implicitly has the name “default”.



```
<div class="container">
  <header>
    <!-- We want header content here -->
  </header>
  <main>
    <!-- We want main content here -->
  </main>
  <footer>
    <!-- We want footer content here -->
  </footer>
</div>
  ```

For these cases, the `<slot>` element has a special attribute, name, which can be used to define additional slots:

```
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

To provide content to named slots, we can use the v-slot directive on a `<template>`, providing the name of the slot as v-slot‘s argument:

```
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>

```

Now everything inside the `<template>` elements will be passed to the corresponding slots. Any content not wrapped in a `<template>` using v-slot is assumed to be for the default slot.

### Scoped Slots
Sometimes, it’s useful for slot content to have access to data only available in the child component. For example, imagine a `<current-user>` component with the following template:

```
<span>
  <slot>{{ user.lastName }}</slot>
</span>

```
We might want to replace this fallback content to display the user’s first name, instead of last, like this:

```
<current-user>
  {{ user.firstName }}
</current-user>
```

That won’t work, however, because only the `<current-user>` component has access to the user and the content we’re providing is rendered in the parent.

To make user available to the slot content in the parent, we can bind `user` as an attribute to the `<slot>` element:

```
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```