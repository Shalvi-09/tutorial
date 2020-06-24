# View Js Baic

- [View Js Baic](#view-js-baic)
  - [VueJs](#vuejs)
    - [React and VueJs](#react-and-vuejs)
  - [Installation](#installation)


## VueJs
Vue (pronounced /vjuː/, like view) is a progressive framework for building user interfaces.

The core library is **focused on the view layer only**, and is easy to pick up and integrate with other libraries or existing projects. 

### React and VueJs

React and Vue share many similarities. They both:

 * utilize a virtual DOM
 * provide reactive and composable view components
 * maintain focus in the core library, with concerns such as routing and global state management handled by companion libraries
 

 ## Installation
 The easiest way to try out Vue.js is using the Hello World example. Feel free to open it in another tab and follow along as we go through some basic examples. Or, you can create an `index.html` file and include Vue with:

 ```
 <!-- development version, includes helpful console warnings -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```
  *The Installation page provides more options of installing Vue. Note: We do not recommend that beginners start with vue-cli, especially if you are not yet familiar with Node.js-based build tools.*


  ```
  <!-- index.html -->
<!DOCTYPE html>
<html>
<!-- .......... -->
<body>
<!-- Other html stuff -->
    <div id="app">
    {{ message }}
    </div>



<script>
    var app = new Vue({
    el: '#app',
    data: {
        message: 'Hello Vue!'
    }
    });

</script>
</body>
</html>

  ```


Thats the basic `vue` application. simply create one htmlfile and include the cdn link of view js and you are done.

