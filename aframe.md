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

  
