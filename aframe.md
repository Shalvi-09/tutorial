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
