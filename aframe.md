https://aframe.io/docs/1.0.0/introduction/
# Getting Started

```
<html>
  <head>
    <script src="https://aframe.io/releases/1.0.3/aframe.min.js"></script>
  </head>
  <body>
    <a-scene>
      <a-box position="-1 0.5 -3" rotation="0 45 0" color="#4CC3D9"></a-box>
      <a-sphere position="0 1.25 -5" radius="1.25" color="#EF2D5E"></a-sphere>
      <a-cylinder position="1 0.75 -3" radius="0.5" height="1.5" color="#FFC65D"></a-cylinder>
      <a-plane position="0 0 -4" rotation="-90 0 0" width="4" height="4" color="#7BC8A4"></a-plane>
      <a-sky color="#ECECEC"></a-sky>
    </a-scene>
  </body>
</html>
```

## What is WebVR?
WebVR is a JavaScript API for creating immersive 3D, virtual reality experiences in your browser. Or simply put, allows VR in the browser over the Web.

A-Frame uses the WebVR API to gain access to VR headset sensor data (position, orientation) to transform the camera and to render content directly to VR headsets. Note that WebVR, which provides data, should not be confused nor conflated with WebGL, which provides graphics and rendering.

## Which Platforms Does A-Frame Support?
A-Frame supports almost all platforms through browsers. General platforms that A-Frame supports include:

VR on desktop with a headset
VR on mobile with a headset
VR on standalone headset
Flat on desktop (i.e., mouse and keyboard)
Flat mobile (i.e., magic window)
Some other platforms that have been shown to work with A-Frame include:

Augmented reality (AR) on AR headsets (e.g., Magic Leap, HoloLens)
Augmented reality (AR) on mobile (i.e., magic window, ARKit, ARCore)

## HTML & Primitives
A-Frame is based on top of HTML and the DOM using a polyfill for Custom Elements.While the HTML layer looks basic, HTML and the DOM are only the outermost abstraction layer of A-Frame. Underneath, A-Frame is an `entity-component framework` for three.js that is exposed declaratively.


![Scene with sphere, box, cylinder](https://user-images.githubusercontent.com/674727/52090525-79b04d80-2566-11e9-993f-7a8b19ca25b1.png)

### Primitives

A-Frame provides a handful of elements such as `<a-box>` or `<a-sky>` called __primitives__ that wrap the _entity-component pattern_ to make it appealing for beginners. At the bottom of the documentation navigation sidebar, we can see every primitive that A-Frame provides out of the box. Developers can create their own primitives as well.

Below is the Hello, WebVR example that uses a few basic primitives. A-Frame provides primitives to create meshes, render 360° content, customize the environment, place the camera, etc.

```
<html>
  <head>
    <script src="https://aframe.io/releases/1.0.3/aframe.min.js"></script>
  </head>
  <body>
    <a-scene>
      <a-box position="-1 0.5 -3" rotation="0 45 0" color="#4CC3D9"></a-box>
      <a-sphere position="0 1.25 -5" radius="1.25" color="#EF2D5E"></a-sphere>
      <a-cylinder position="1 0.75 -3" radius="0.5" height="1.5" color="#FFC65D"></a-cylinder>
      <a-plane position="0 0 -4" rotation="-90 0 0" width="4" height="4" color="#7BC8A4"></a-plane>
      <a-sky color="#ECECEC"></a-sky>
    </a-scene>
  </body>
</html>
```
Primitives act as a convenience layer (i.e., syntactic sugar) primarily for newcomers. Keep in mind for now that primitives are <a-entity>s under the hood that:

* Have a semantic name (e.g., <a-box>)
* Have a preset bundle of components with default values
* Map or proxy HTML attributes to component data
  
Under the hood, this <a-box> primitive:
`<a-box color="red" width="3"></a-box>`
  represents this entity-component form:
`<a-entity geometry="primitive: box; width: 3" material="color: red"></a-entity>`


<a-box> defaults the geometry.primitive property to box. And the primitive maps the HTML width attribute to the underlying geometry.width property as well as the HTML color attribute to the underlying material.color property.
  
  
  __NOTE:__ Primitives are just <a-entity>s under the hood. This means primitives have the same API as entities such as positioning, rotating, scaling, and attaching components.
  
  
  ## Registering a Primitive
  We can register our own primitives (i.e., register an element) using `AFRAME.registerPrimitive(name, definition)`. __name is a string and must contain a dash (i.e. 'a-foo')__. `definition` is a JavaScript object defining these properties:
  
  | Property |	Description |	Example |
  | -------- | ------------ | -------- |
  | defaultComponents	 | Object specifying default components of the primitive. The keys are the components’ names and the values are the components’ default data. | {geometry: {primitive: 'box'}} |
| mappings | Object specifying mapping between HTML attribute name and component property names. Whenever the HTML attribute name is updated, the primitive will update the corresponding component property. The component property is defined using a dot syntax ${componentName}.${propertyName}.	| {depth: 'geometry.depth', height: 'geometry.height', width: 'geometry.width'}
 |
  

### Example
For example, below is A-Frame’s registration for the <a-box> primitive:
  
  ```
  var extendDeep = AFRAME.utils.extendDeep;

// The mesh mixin provides common material properties for creating mesh-based primitives.
// This makes the material component a default component and maps all the base material properties.
var meshMixin = AFRAME.primitives.getMeshMixin();

AFRAME.registerPrimitive('a-box', extendDeep({}, meshMixin, {
  // Preset default components. These components and component properties will be attached to the entity out-of-the-box.
  defaultComponents: {
    geometry: {primitive: 'box'}
  },

  // Defined mappings from HTML attributes to component properties (using dots as delimiters).
  // If we set `depth="5"` in HTML, then the primitive will automatically set `geometry="depth: 5"`.
  mappings: {
    depth: 'geometry.depth',
    height: 'geometry.height',
    width: 'geometry.width'
  }
}));

```

### For example, Don McCurdy’s aframe-extras package includes an <a-ocean> primitive that wraps his ocean component. Here is the definition for <a-ocean>.

```
AFRAME.registerPrimitive('a-ocean', {
  // Attaches the `ocean` component by default.
  // Defaults the ocean to be parallel to the ground.
  defaultComponents: {
    ocean: {},
    rotation: {x: -90, y: 0, z: 0}
  },

  // Maps HTML attributes to the `ocean` component's properties.
  mappings: {
    width: 'ocean.width',
    depth: 'ocean.depth',
    density: 'ocean.density',
    color: 'ocean.color',
    opacity: 'ocean.opacity'
  }
});

```

With the <a-ocean> primitive registered, we’d be able to create oceans using a line of traditional HTML:

`<a-ocean color="aqua" depth="100" width="100"></a-ocean>`

  
## Entity-Component-System
A-Frame is a three.js framework with an entity-component-system (ECS) architecture. ECS architecture is a common and desirable pattern in 3D and game development that follows the composition over inheritance and hierarchy principle.

The benefits of ECS include:

* Greater flexibility when defining objects by mixing and matching reusable parts.
* Eliminates the problems of long inheritance chains with complex interwoven functionality.
* Promotes clean design via decoupling, encapsulation, modularization, reusability.
* Most scalable way to build a VR application in terms of complexity.
* Proven architecture for 3D and VR development.
* Allows for extending new features (possibly sharing them as community components).

On the 2D Web, we lay out elements that have fixed behavior in a hierarchy. 3D and VR is different; there are infinite types of possible objects that have unbounded behavior. ECS provides a manageable pattern to construct types of objects.

A basic definition of ECS involves:

* Entities are container objects into which components can be attached. Entities are the base of all objects in the scene. Without components, entities neither do nor render anything, similar to empty <div>s.
* Components are reusable modules or data containers that can be attached to entities to provide appearance, behavior, and/or functionality. Components are like plug-and-play for objects. All logic is implemented through components, and we define different types of objects by mixing, matching, and configuring components. Like alchemy!
* Systems provide global scope, management, and services for classes of components. Systems are often optional, but we can use them to separate logic and data; systems handle the logic, components act as data containers.
  
 ### Examples
Some abstract examples of different types of entities built from composing together different components:

* Box = Position + Geometry + Material
* Light Bulb = Position + Light + Geometry + Material + Shadow
* Sign = Position + Geometry + Material + Text
* VR Controller = Position + Rotation + Input + Model + Grab + Gestures
* Ball = Position + Velocity + Physics + Geometry + Material
* Player = Position + Camera + Input + Avatar + Identity

## ECS in A-Frame
A-Frame has APIs that represents each piece of ECS:

* Entities are represented by the <a-entity> element and prototype.
* Components are represented by HTML attributes on <a-entity>‘s. Underneath, components are objects containing a schema, lifecycle handlers, and methods. Components are registered via the AFRAME.registerComponent (name, definition) API.
* Systems are represented by <a-scene>‘s HTML attributes. System are similar to components in definition. Systems are registered via the AFRAME.registerSystem (name, definition) API.
  
  ### Syntax
  
  We create <a-entity> and attach components as HTML attributes. Most components have multiple properties that are represented by a syntax similar to HTMLElement.style CSS. This syntax takes the form with a colon (:) separating property names from property values, and a semicolon (;) separating different property declarations:

`<a-entity ${componentName}="${propertyName1}: ${propertyValue1}; ${propertyName2}: ${propertyValue2}">`

A-Frame components can do anything. Developers are given permissionless innovation to create components to extend any feature. Components have full access to JavaScript, three.js, and Web APIs (e.g., WebRTC, Speech Recognition).

```

AFRAME.registerComponent('foo', {
  schema: {
    bar: {type: 'number'},
    baz: {type: 'string'}
  },

  init: function () {
    // Do something when component first attached.
  },

  update: function () {
    // Do something when component's data is updated.
  },

  remove: function () {
    // Do something the component or its entity is detached.
  },

  tick: function (time, timeDelta) {
    // Do something on every scene tick or frame.
  }
});

<a-entity foo="bar: 5; baz: bazValue"></a-entity>
```

### Component-Based Development
For building VR applications, we recommend placing all application code within components (and systems). An ideal A-Frame codebase consists purely of modular, encapsulated, and decoupled components. These components can be unit tested in isolation or alongside other components.

A simple ECS codebase might be structured like:

```
index.html
components/
  ball.js
  collidable.js
  grabbable.js
  enemy.js
  scoreboard.js
  throwable.js
  ```
__Components can set other components on the entity, making them a higher-order or higher-level component in abstraction.__

## Community developed Components
Components can be shared into the A-Frame ecosystem for the community to use. The wonderful thing about A-Frame’s ECS is extensibility. An experienced developer can develop a physics system or graphics shader components, and an novice developer can take those components and use them in their scene from HTML just by dropping in a <script> tag. We can use powerful published components without having to touch JavaScript.


### Where to Find Components

#### npm
Most A-Frame components are published on npm as well as GitHub. We can use npm’s [search to search for aframe-components](https://www.npmjs.com/search?q=aframe-component). npm lets us sort by quality, popularity, and maintenance. This is a great place to look for a more complete list of components.

GitHub Projects
Many A-Frame applications are developed purely from components, and many of those A-Frame applications are open source on GitHub. Their codebases will contain components that we can use directly, refer to, or copy from. Projects to look at include:

* [BeatSaver Viewer](https://github.com/supermedium/beatsaver-viewer/)
* [Super Says](https://github.com/supermedium/supersays/)
* [A-Painter](https://github.com/aframevr/a-painter/)
* [A-Blast](https://github.com/aframevr/a-blast/)
* [Weekly on aframe blog](https://aframe.io/blog/)

#### Using a Community Component
Once we find a component that we want to use, we can include the component as a <script> tag and use it from HTML.

For example, let’s use IdeaSpaceVR’s [particle system component:](https://www.npmjs.com/package/aframe-particle-system-component)


First, we have to grab a CDN link to the component JS file. The documentation of the component will usually have a CDN link or usage information. But a way to get the most up-to-date CDN link is to use unpkg.com.

unpkg is a CDN that automatically hosts everything that is published to npm. unpkg can resolve semantic versioning and provide us the version of the component we want. A URL takes the form of:

https://unpkg.com/<npm package name>@<version>/<path to file>
If we want the latest version, we can exclude the version:

https://unpkg.com/<npm package name>/<path to file>
Rather than typing in the path to the built component JS file, we can exclude path to file to be able to browse the directories of the component package. The JS file will usually be in a folder called dist/ or build/ and end with .min.js.

For the particle system component, we head to:

`https://unpkg.com/aframe-particle-system-component/`
Note the ending slash (/). Find the file we need, right click, and hit Copy Link to Address to copy the CDN link into our clipboard.
![](https://cloud.githubusercontent.com/assets/674727/25502028/cbfd7b3a-2b49-11e7-914d-a8280aa47ace.jpg)
Finally include the file in head of your html.

```
<html>
  <head>
    <script src="https://aframe.io/releases/1.0.3/aframe.min.js"></script>
    <script src="https://unpkg.com/aframe-particle-system-component@1.0.9/dist/aframe-particle-system-component.min.js"></script>
  </head>
  <body>
     <a-scene>
      <a-entity particle-system="preset: snow" position="0 0 -10"></a-entity>
    </a-scene>
  </body>
</html>
```


Below is a complete example of using various community components from the Registry and using the unpkg CDN

```
<html>
  <head>
    <script src="https://aframe.io/releases/1.0.3/aframe.min.js"></script>
    <script src="https://unpkg.com/aframe-animation-component@3.2.1/dist/aframe-animation-component.min.js"></script>
    <script src="https://unpkg.com/aframe-particle-system-component@1.0.x/dist/aframe-particle-system-component.min.js"></script>
    <script src="https://unpkg.com/aframe-extras.ocean@%5E3.5.x/dist/aframe-extras.ocean.min.js"></script>
    <script src="https://unpkg.com/aframe-gradient-sky@1.0.4/dist/gradientsky.min.js"></script>
  </head>
  <body>
    <a-scene>
      <a-entity id="rain" particle-system="preset: rain; color: #24CAFF; particleCount: 5000"></a-entity>

      <a-entity id="sphere" geometry="primitive: sphere"
                material="color: #EFEFEF; shader: flat"
                position="0 0.15 -5"
                light="type: point; intensity: 5"
                animation="property: position; easing: easeInOutQuad; dir: alternate; dur: 1000; to: 0 -0.10 -5; loop: true"></a-entity>

      <a-entity id="ocean" ocean="density: 20; width: 50; depth: 50; speed: 4"
                material="color: #9CE3F9; opacity: 0.75; metalness: 0; roughness: 1"
                rotation="-90 0 0"></a-entity>

      <a-entity id="sky" geometry="primitive: sphere; radius: 5000"
                material="shader: gradient; topColor: 235 235 245; bottomColor: 185 185 210"
                scale="-1 1 1"></a-entity>

      <a-entity id="light" light="type: ambient; color: #888"></a-entity>
    </a-scene>
  </body>
</html>
```

![](https://cloud.githubusercontent.com/assets/674727/25502318/0f76ceec-2b4b-11e7-9829-cb3784b20dc1.gif)
