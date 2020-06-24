# Single page cmponent

In many Vue projects, global components will be defined using Vue.component, followed by new Vue({ el: '#container' }) to target a container element in the body of every page.

This can work very well for small to medium-sized projects, where JavaScript is only used to enhance certain views. In more complex projects however, or when your frontend is entirely driven by JavaScript, these disadvantages become apparent:

*    **Global definitions** force unique names for every component
*    **String templates** lack syntax highlighting and require ugly slashes for multiline HTML
*    **No CSS support** means that while HTML and JavaScript are modularized into components, CSS is conspicuously left out
*    **No build step** restricts us to HTML and ES5 JavaScript, rather than preprocessors like Pug (formerly Jade) and Babel


>All of these are solved by single-file components with a `.vue` extension, made possible with build tools such as `Webpack` or `Browserify`.

Here’s an example of a file we’ll call `Hello.vue`:

```
    <template>
    <p>{{ greeting }} World!</p>
    </template>

    <script>
    module.exports = {
    data: function() {
        return {
        greeting: "Hello"
        };
    }
    };
    </script>

    <style scoped>
    p {
    font-size: 2em;
    text-align: center;
    }
    </style>


```


## What About Separation of Concerns?
One important thing to note is that **separation of concerns is not equal to separation of file types**. In modern UI development, we have found that instead of dividing the codebase into three huge layers that interweave with one another, it makes much more sense to divide them into loosely-coupled components and compose them. Inside a component, its template, logic and styles are inherently coupled, and collocating them actually makes the component more cohesive and maintainable.

Even if you don’t like the idea of Single-File Components, you can still leverage its hot-reloading and pre-compilation features by separating your JavaScript and CSS into separate files:

```
<!-- my-component.vue -->
<template>
  <div>This will be pre-compiled</div>
</template>
<script src="./my-component.js"></script>
<style src="./my-component.css"></style>

```
