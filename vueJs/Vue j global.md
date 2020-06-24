# Globally available instance methods

## Vue.nextTick( [callback, context] )
Defer the callback to be executed after the next DOM update cycle. Use it immediately after you’ve changed some data to wait for the DOM update.

```
// modify data
vm.msg = 'Hello'
// DOM not updated yet
Vue.nextTick(function () {
  // DOM updated
})

// usage as a promise (2.1.0+, see note below)
Vue.nextTick()
  .then(function () {
    // DOM updated
  })
  ```

  ## Vue.set( target, propertyName/index, value )

  Arguments:

*    {Object | Array} target
*    {string | number} propertyName/index
*    {any} value

Returns: the set value.

Adds a property to a reactive object, ensuring the new property is also reactive, so triggers view updates. This must be used to add new properties to reactive objects, as Vue cannot detect normal property additions (e.g. this.myObject.newProperty = 'hi').

## Vue.delete( target, propertyName/index )

Delete a property on an object. If the object is reactive, ensure the deletion triggers view updates. This is primarily used to get around the limitation that Vue cannot detect property deletions, but you should rarely need to use it.


## Vue.directive( id, [definition] )


## Vue.filter( id, [definition] )


## Vue.component( id, [definition] )

## Vue.use( plugin )

Arguments:

*    {Object | Function} plugin

Install a Vue.js plugin. If the plugin is an Object, it must expose an install method. If it is a function itself, it will be treated as the install method. The install method will be called with Vue as the argument.

This method has to be called before calling new Vue()

When this method is called on the same plugin multiple times, the plugin will be installed only once.

## Vue.mixin( mixin )

Arguments:

*    {Object} mixin

Apply a mixin globally, which affects every Vue instance created afterwards. This can be used by plugin authors to inject custom behavior into components. Not recommended in application code.




