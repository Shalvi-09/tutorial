# Vue Instance

- [Vue Instance](#vue-instance)
  - [data and methods](#data-and-methods)
  - [Options / Composition](#options--composition)
    - [parent](#parent)
    - [mixins](#mixins)
    - [extends](#extends)
  - [Life cycle](#life-cycle)
  - [Instance properties](#instance-properties)
    - [**`vm.$data`**](#vmdata)
    - [**`vm.$props`**](#vmprops)
    - [**`vm.$el`**](#vmel)
    - [**`vm.$options`**](#vmoptions)
    - [**`vm.$parent`**](#vmparent)
    - [**`vm.$root`**](#vmroot)
    - [**`vm.$refs`**](#vmrefs)
    - [**`vm.$isServer`**](#vmisserver)
  - [Instance Methods / Events](#instance-methods--events)
    - [**`vm.$on( event, callback )`**](#vmon-event-callback-)
    - [**`vm.$once( event, callback )`**](#vmonce-event-callback-)
    - [**`vm.$off( [event, callback] )`**](#vmoff-event-callback-)
    - [**`vm.$emit( eventName, […args] )`**](#vmemit-eventname-args-)
  - [Instance Methods / Lifecycle](#instance-methods--lifecycle)
    - [**`vm.$mount( [elementOrSelector] )`**](#vmmount-elementorselector-)
    - [**`vm.$forceUpdate()`**](#vmforceupdate)
    - [**`vm.$nextTick( [callback] )`**](#vmnexttick-callback-)
    - [**`vm.$destroy()`**](#vmdestroy)

Every Vue application starts by creating a new Vue instance with the `Vue function`:

```
var vm = new Vue({
  // options
})

```

Although not strictly associated with the MVVM pattern, Vue’s design was partly inspired by it. As a convention, we often use the variable vm (short for ViewModel) to refer to our Vue instance.


## data and methods

When a Vue instance is created, it adds all the properties found in its data object to `Vue’s reactivity system`. When the values of those properties change, the view will “react”, updating to match the new values.

```
// Our data object
var data = { a: 1 }

// The object is added to a Vue instance
var vm = new Vue({
  data: data
})

// Getting the property on the instance
// returns the one from the original data
vm.a == data.a // => true

// Setting the property on the instance
// also affects the original data
vm.a = 2
data.a // => 2

// ... and vice-versa
data.a = 3
vm.a // => 3

```

When this data changes, the view will re-render. 

**It should be noted that properties in data are only reactive `if they existed when the instance was created`. That means if you add a new property, like:**

```
vm.b = 'hi'
```

**`Then changes to b will not trigger any view updates.`** If you know you’ll need a property later, but it starts out empty or non-existent, you’ll need to set some initial value. For example:


The only exception to this being the use of **`Object.freeze()`**, which prevents existing properties from being changed, which also means the reactivity system can’t track changes.

```
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})

```

## Options / Composition
### parent

Specify the parent instance for the instance to be created. **Establishes a parent-child relationship between the two.** The parent will be accessible as `this.$parent` for the child, and the child will be pushed into the parent’s `$children` array.

> Use `$parent` and `$children` sparingly - they mostly serve as an escape-hatch. Prefer using props and events for parent-child communication.

**Type: `Vue instance`**


### mixins

The mixins option accepts an array of mixin objects. These mixin objects can contain instance options like normal instance objects, and they will be merged against the eventual options using the same option merging logic in Vue.extend(). e.g. If your mixin contains a created hook and the component itself also has one, both functions will be called.

Mixin hooks are called in the order they are provided, and called before the component’s own hooks.

**Type: `Array<Object>`**


```
var mixin = {
  created: function () { console.log(1) }
}
var vm = new Vue({
  created: function () { console.log(2) },
  mixins: [mixin]
})
// => 1
// => 2
```

### extends

Allows declaratively extending another component (could be either a plain options object or a constructor) without having to use `Vue.extend`. This is primarily intended to make it easier to extend between single file components.

This is similar to mixins.

```
var CompA = { ... }

// extend CompA without having to call `Vue.extend` on either
var CompB = {
  extends: CompA,
  ...
}

```


**Type: `Object | Function`**



## Life cycle

Each Vue instance goes through a series of initialization steps when it’s created - for example, it needs to set up data observation, compile the template, mount the instance to the DOM, and update the DOM when data changes. Along the way, it also runs functions called lifecycle hooks, giving users the opportunity to add their own code at specific stages.

* **`beforeCReate()`**
* **`created()`**
* **`beforeMount()`**
* **`mounted()`**
* **`beforeUpdate()`**
* **`updated()`**
* **`beforeDestroy()`**
* **`destroyed()`**

![](img/lifecycle.png)



## Instance properties
### **`vm.$data`**
The data object that the Vue instance is observing. The Vue instance proxies access to the properties on its data object.

**type: Object**

### **`vm.$props`**
An object representing the current props a component has received. The Vue instance proxies access to the properties on its props object.

**Type: Object**

### **`vm.$el`**
The root DOM element that the Vue instance is managing.

**Type: Element**

**Read only**

### **`vm.$options`**
The instantiation options used for the current Vue instance. This is useful when you want to include custom properties in the options:

**Type: Object**

**Read only**

```
new Vue({
  customOption: 'foo',
  created: function () {
    console.log(this.$options.customOption) // => 'foo'
  }
})
```

### **`vm.$parent`**
The parent instance, if the current instance has one.

**Type: Vue instance**

**Read only**

### **`vm.$root`**
The root Vue instance of the current component tree. If the current instance has no parents this value will be itself.

**Type: Vue instance**

**Read only**

### **`vm.$refs`**
An object of DOM elements and component instances, registered with `ref` attributes.

### **`vm.$isServer`**

Whether the current Vue instance is running on the server.

**Type: boolean**

## Instance Methods / Events

### **`vm.$on( event, callback )`**
Listen for a custom event on the current vm. Events can be triggered by `vm.$emit`. The callback will receive all the additional arguments passed into these event-triggering methods.

```
vm.$on('test', function (msg) {
  console.log(msg)
})
vm.$emit('test', 'hi')
// => "hi"
```

### **`vm.$once( event, callback )`**
Listen for a custom event, but only once. The listener will be removed once it triggers for the first time.

### **`vm.$off( [event, callback] )`**

Remove custom event listener(s).

*    If no arguments are provided, remove all event listeners;

*    If only the event is provided, remove all listeners for that event;

*   If both event and callback are given, remove the listener for that specific callback only.

### **`vm.$emit( eventName, […args] )`**

Trigger an event on the current instance. Any additional arguments will be passed into the listener’s callback function.

## Instance Methods / Lifecycle

### **`vm.$mount( [elementOrSelector] )`**

If a Vue instance didn’t receive the el option at instantiation, it will be in “unmounted” state, without an associated DOM element. vm.$mount() can be used to manually start the mounting of an unmounted Vue instance.

If elementOrSelector argument is not provided, the template will be rendered as an off-document element, and you will have to use native DOM API to insert it into the document yourself.

The method returns the instance itself so you can chain other instance methods after it.

```
var MyComponent = Vue.extend({
  template: '<div>Hello!</div>'
})

// create and mount to #app (will replace #app)
new MyComponent().$mount('#app')

// the above is the same as:
new MyComponent({ el: '#app' })

// or, render off-document and append afterwards:
var component = new MyComponent().$mount()
document.getElementById('app').appendChild(component.$el)

```

### **`vm.$forceUpdate()`**
Force the Vue instance to re-render. Note it does not affect all child components, only the instance itself and child components with inserted slot content.

### **`vm.$nextTick( [callback] )`**

Defer the callback to be executed after the next DOM update cycle. Use it immediately after you’ve changed some data to wait for the DOM update. This is the same as the global `Vue.nextTick`, except that the callback’s `this` context is automatically bound to the instance calling `this` method.

```
new Vue({
  // ...
  methods: {
    // ...
    example: function () {
      // modify data
      this.message = 'changed'
      // DOM is not updated yet
      this.$nextTick(function () {
        // DOM is now updated
        // `this` is bound to the current instance
        this.doSomethingElse()
      })
    }
  }
})
```

### **`vm.$destroy()`**

Completely destroy a vm. Clean up its connections with other existing vms, unbind all its directives, turn off all event listeners.

Triggers the `beforeDestroy` and `destroyed` hooks.







