# Unit Testing
>Vue CLI has built-in options for unit testing with `Jest` or `Mocha` that works out of the box. We also have the `official Vue Test Utils` which provides more detailed guidance for custom setups.

## Simple Assertions

You don’t have to do anything special in your components to make them testable. Export the raw options:



```
<template>
  <span>{{ message }}</span>
</template>

<script>
  export default {
    data () {
      return {
        message: 'hello!'
      }
    },
    created () {
      this.message = 'bye!'
    }
  }
</script>
```

Then import the component along with `Vue Test Utils`, and you can make many common assertions (here we are using Jest-style expect assertions just as an example):

```
// Import `shallowMount` from Vue Test Utils and the component being tested
import { shallowMount } from '@vue/test-utils'
import MyComponent from './MyComponent.vue'

// Mount the component
const wrapper = shallowMount(MyComponent)

// Here are some Jest tests, though you can
// use any test runner/assertion library combo you prefer
describe('MyComponent', () => {
  // Inspect the raw component options
  it('has a created hook', () => {
    expect(typeof MyComponent.created).toBe('function')
  })

  // Evaluate the results of functions in
  // the raw component options
  it('sets the correct default data', () => {
    expect(typeof MyComponent.data).toBe('function')
    const defaultData = MyComponent.data()
    expect(defaultData.message).toBe('hello!')
  })

  // Inspect the component instance on mount
  it('correctly sets the message when created', () => {
    expect(wrapper.vm.$data.message).toBe('bye!')
  })

  // Mount an instance and inspect the render output
  it('renders the correct message', () => {
    expect(wrapper.text()).toBe('bye!')
  })
})
```
