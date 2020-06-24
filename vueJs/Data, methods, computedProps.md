# data,props , computed, methods, watch

- [data,props , computed, methods, watch](#dataprops--computed-methods-watch)
  - [data](#data)
  - [props](#props)
  - [computed](#computed)
  - [methods](#methods)
  - [watch](#watch)

## data

Type: `Object | Function`

Restriction: `Only accepts Function when used in a component definition.`

The data object for the Vue instance. Vue will recursively convert its properties into `getter/setters` to make it **“reactive”**. **The object must be plain:** `native objects such as browser API objects and prototype properties are ignored.` A rule of thumb is that data should just be data - it is not recommended to observe objects with their own stateful behavior.

> After the instance is created, the original data object can be accessed as `vm.$data`. The Vue instance also proxies all the properties found on the data object, so vm.a will be equivalent to `vm.$data.a`.

Properties that start with `_` or `$` will not be proxied on the Vue instance because they may conflict with Vue’s internal properties and API methods. You will have to access them as `vm.$data._property`.

```
var data = { a: 1 }

// direct instance creation
var vm = new Vue({
  data: data
})
vm.a // => 1
vm.$data === data // => true

// must use function when in Vue.extend()
var Component = Vue.extend({
  data: function () {
    return { a: 1 }
  }
})
```

> Note that if you use an arrow function with the data property, this won’t be the component’s instance, but you can still access the instance as the function’s first argument:

## props
Type: `Array<string> | Object`

A list/hash of attributes that are exposed to **accept data from the parent component**. It has an Array-based simple syntax and an alternative Object-based syntax that allows advanced configurations such as type checking, custom validation and default values.

With Object-based syntax, you can use following options:

*    `type`: can be one of the following native constructors: `String`, `Number`, `Boolean`, `Array`, `Object`, `Date`, `Function`, Symbol, any custom constructor function or an array of those. Will check if a prop has a given type, and will throw a warning if it doesn’t. More information on prop types.
*    default: `any`
    Specifies a default value for the prop. If the prop is not passed, this value will be used instead. Object or array defaults must be returned from a factory function.
*    `required`: Boolean
    Defines if the prop is required. In a non-production environment, a console warning will be thrown if this value is truthy and the prop is not passed.
*    `validator`: Function
    Custom validator function that takes the prop value as the sole argument. In a non-production environment, a console warning will be thrown if this function returns a falsy value (i.e. the validation fails). You can read more about prop validation here.


```
// simple syntax
Vue.component('props-demo-simple', {
  props: ['size', 'myMessage']
})

// object syntax with validation
Vue.component('props-demo-advanced', {
  props: {
    // type check
    height: Number,
    // type check plus other validations
    age: {
      type: Number,
      default: 0,
      required: true,
      validator: function (value) {
        return value >= 0
      }
    }
  }
})
```


## computed

Computed properties to be mixed into the Vue instance. All getters and setters have their this context automatically bound to the Vue instance.

> Note that if you use an arrow function with a computed property, this won’t be the `component’s instance`, but you can still access the instance as the function’s first argument:

```computed: {
  aDouble: vm => vm.a * 2
}
```

Computed properties are cached, and only re-computed on reactive dependency changes. Note that if a certain dependency is out of the instance’s scope (i.e. not reactive), the computed property will not be updated.

```
var vm = new Vue({
  data: { a: 1 },
  computed: {
    // get only
    aDouble: function () {
      return this.a * 2
    },
    // both get and set
    aPlus: {
      get: function () {
        return this.a + 1
      },
      set: function (v) {
        this.a = v - 1
      }
    }
  }
})
vm.aPlus   // => 2
vm.aPlus = 3
vm.a       // => 2
vm.aDouble // => 4

```

## methods

```
var vm = new Vue({
  data: { a: 1 },
  methods: {
    plus: function () {
      this.a++
    }
  }
})
vm.plus()
vm.a // 2
```


## watch
Type: `{ [key: string]: string | Function | Object | Array}`

An object where keys are expressions to watch and values are the corresponding callbacks. The value can also be a string of a method name, or an Object that contains additional options. The Vue instance will call `$watch()` for each entry in the object at instantiation.

```
var vm = new Vue({
  data: {
    a: 1,
    b: 2,
    c: 3,
    d: 4,
    e: {
      f: {
        g: 5
      }
    }
  },
  watch: {
    a: function (val, oldVal) {
      console.log('new: %s, old: %s', val, oldVal)
    },
    // string method name
    b: 'someMethod',
    // the callback will be called whenever any of the watched object properties change regardless of their nested depth
    c: {
      handler: function (val, oldVal) { /* ... */ },
      deep: true
    },
    // the callback will be called immediately after the start of the observation
    d: {
      handler: 'someMethod',
      immediate: true
    },
    // you can pass array of callbacks, they will be called one-by-one
    e: [
      'handle1',
      function handle2 (val, oldVal) { /* ... */ },
      {
        handler: function handle3 (val, oldVal) { /* ... */ },
        /* ... */
      }
    ],
    // watch vm.e.f's value: {g: 5}
    'e.f': function (val, oldVal) { /* ... */ }
  }
})
vm.a = 2 // => new: 2, old: 1
```

