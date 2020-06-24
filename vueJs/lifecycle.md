# Lifecycle Hooks

- [Lifecycle Hooks](#lifecycle-hooks)
  - [beforeCreate](#beforecreate)
  - [created](#created)
  - [beforeMount](#beforemount)
  - [mounted](#mounted)
  - [beforeUpdate](#beforeupdate)
  - [updated](#updated)
  - [beforeDestroy](#beforedestroy)
  - [destroyed](#destroyed)
  - [errorCaptured](#errorcaptured)


> All lifecycle hooks automatically have their `this` context bound to the instance, so that you can access data, computed properties, and methods. This means you should not use an arrow function to define a lifecycle method `(e.g. created: () => this.fetchTodos())`. The reason is arrow functions bind the parent context, so this will not be the Vue instance as you expect and this.fetchTodos will be undefined.

## beforeCreate

Called synchronously immediately after the instance has been initialized, before data observation and event/watcher setup.

**Type: `Function`**

## created

Called synchronously after the instance is created. At this stage, the instance has finished processing the options which means the following have been set up: data observation, computed properties, methods, watch/event callbacks. However, the mounting phase has not been started, and the $el property will not be available yet.

**Type: `Function`**

## beforeMount

Called right before the mounting begins: the render function is about to be called for the first time.
 > This hook is not called during server-side rendering.

 ## mounted

 Called after the instance has been mounted, where `el` is replaced by the newly created `vm.$el`. If the root instance is mounted to an in-document element, `vm.$el` will also be in-document when mounted is called.

 > **This hook is not called during server-side rendering.**

> Note that mounted does not guarantee that all child components have also been mounted. If you want to wait until the entire view has been rendered, you can use `vm.$nextTick` inside of mounted:
```
mounted: function () {
  this.$nextTick(function () {
    // Code that will run only after the
    // entire view has been rendered
  })
}
```

## beforeUpdate

Called when data changes, before the DOM is patched. This is a good place to access the existing DOM before an update, e.g. to remove manually added event listeners.

> This hook is not called during server-side rendering, because only the initial render is performed server-side.

## updated

Called after a data change causes the virtual DOM to be re-rendered and patched.

The component’s DOM will have been updated when this hook is called, so you can perform DOM-dependent operations here. However, in most cases you should avoid changing state inside the hook. To react to state changes, it’s usually better to use a `computed property` or `watcher` instead.

> This hook is not called during server-side rendering.

> Note that updated does not guarantee that all child components have also been re-rendered. If you want to wait until the entire view has been re-rendered, you can use `vm.$nextTick` inside of updated:

```
updated: function () {
  this.$nextTick(function () {
    // Code that will run only after the
    // entire view has been re-rendered
  })
}
```

## beforeDestroy

Called right before a Vue instance is destroyed. At this stage the instance is still fully functional.

> This hook is not called during server-side rendering.

## destroyed

Called after a Vue instance has been destroyed. When this hook is called, all directives of the Vue instance have been unbound, all event listeners have been removed, and all child Vue instances have also been destroyed.

> This hook is not called during server-side rendering.

## errorCaptured
> New in 2.5.0+

`Type: (err: Error, vm: Component, info: string) => ?boolean
`

Called when an error from any descendent component is captured. The hook receives three arguments: the error, the component instance that triggered the error, and a string containing information on where the error was captured. The hook can return false to stop the error from propagating further.

Error Propagation Rules

*    By default, all errors are still sent to the global config.errorHandler if it is defined, so that these errors can still be reported to an analytics service in a single place.


*    If multiple errorCaptured hooks exist on a component’s inheritance chain or parent chain, all of them will be invoked on the same error.

*    If the errorCaptured hook itself throws an error, both this error and the original captured error are sent to the global config.errorHandler.

*    An errorCaptured hook can return false to prevent the error from propagating further. This is essentially saying “this error has been handled and should be ignored.” It will prevent any additional errorCaptured hooks or the global config.errorHandler from being invoked for this error.


