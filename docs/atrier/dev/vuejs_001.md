---
title: tempo_001
description: 
published: true
date: 2023-04-21T11:13:13.568Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:13:08.553Z
---

---
title: Building an interactive web portfolio with Vue + Three.js — Part Three: Implementing Three.js
description: 
published: true
date: 2023-02-03T10:45:43.232Z
tags: 
editor: markdown
dateCreated: 2023-01-08T16:07:56.821Z
---

---
title: Building an interactive web portfolio with Vue + Three.js — Part Three: Implementing Three.js
description: 
published: true
date: 2023-01-27T18:10:29.335Z
tags: 
editor: markdown
dateCreated: 2023-01-08T16:07:56.821Z
---

---
title: Building an interactive web portfolio with Vue + Three.js — Part Three: Implementing Three.js
description: 
published: true
date: 2023-01-08T16:19:19.803Z
tags: 
editor: markdown
dateCreated: 2023-01-08T16:07:56.821Z
---

# Building an interactive web portfolio with Vue + Three.js — Part Three: Implementing Three.js

<img src="https://miro.medium.com/max/1400/1*SjBASeN6kVDl2ICGIyFafA.webp">

This is the home stretch! We learned how to set up a Vue + SCSS project in the last two episodes.

, and create a basic portfolio template. Now, it’s time to integrate Three.js into our project!

## But… What is Three.js?
Three.js is a graphics API created with Web GL. It has countless features, but the most popular are the ones regarding 3D rendering. As I said earlier, Three.js is a High-level API for the WebGL engine, so luckily, you don’t need to be a math wizard to render a 10x10 cube.

In this article, we will create an interactive background for our site. But remember: this is just an example. Feel free to do your own thing while building it. Don’t exclusively stick to what I’m showcasing! It is your personal portfolio, after all.

## Installing Three.js
The first thing to do is to download the required dependencies into our project.

```
npm install three
```

## Temporarily disabling Vue
It’s best to start by disabling our Vue container, just for now. This way, we will be sure it won’t affect our Three.js code. Just go to your index.html file and comment on your #app div.

```html
index.html
<!DOCTYPE html>
<html lang="">
 <head>
   <meta charset="utf-8" />
   <meta http-equiv="X-UA-Compatible" content="IE=edge" />
   <meta name="viewport" content="width=device-width,initial-scale=1.0" />
   <link rel="icon" href="<%= BASE_URL %>favicon.ico" />
   <link rel="stylesheet" href="./styles.css">
   <title><%= htmlWebpackPlugin.options.title %></title>
 </head>
 <body>
   <!-- <div id="app"></div> -->
 </body>
</html>
```

## Structuring our project
This guide is based on [this](https://discoverthreejs.com/) fantastic manual. “Discover Three.js” is a perfect resource for anyone who wants to get started with this library. But it’s best suited for vanilla JS projects, so I’ll modify it here and there to explain how to set it up with Vue.

Inside our source folder, we will create a “World” folder. This is where all our Three.js-related code will be placed. Inside it, we will create two folders and one file:

**Components**: Like with Vue, we will store all reusable elements here. Objects, cameras, scenes… We’ll go through them one by one later on.

**Systems**: Here, we save the different systems in our 3D render. In our case, those things are the animation loop, the resizer, the renderer, and the controls.

**World.js**: Here’s where the magic happens. The World.js file is where all the different systems and components are connected and introduced into the HTML.

In the end, you should have a folder structure similar to this:

<img src="https://miro.medium.com/max/640/0*3yODXBFkWL_-uCah">

Now it’s time to get down to business! Sadly, making a 3D scene requires a lot of boilerplate. So we’ll have to write several lines of code before seeing anything. But don’t worry! Remember: patience is a virtue! 😎

## Making our Systems

### Creating the Renderer

Not much to say here. The renderer is, as you may have guessed, what renders everything. We’ll be using WebGL.

```js
renderer.js
import { WebGLRenderer } from "three";
 
function createRenderer() {
 const renderer = new WebGLRenderer({ antialias: true });
 
 // Turn on the physically correct lighting model.
 renderer.physicallyCorrectLights = true;
 
 return renderer;
}
 
export { createRenderer };
```

### Creating the Resizer
The resizer is a crucial element in our project. It handles the resizing window events, which allows us to make our scenes responsive!

```js
Resizer.js
const setSize = (container, camera, renderer) => {
   camera.aspect = window.innerWidth / window.innerHeight;
   camera.updateProjectionMatrix();
    renderer.setSize(window.innerWidth, window.innerHeight);
   renderer.setPixelRatio(window.devicePixelRatio);
 };
  class Resizer {
   constructor(container, camera, renderer) {
     // Set initial size on load.
     setSize(container, camera, renderer);
      window.addEventListener('resize', () => {
       // Set the size again if a resize occurs.
       setSize(container, camera, renderer);
       // Perform any custom actions.
       this.onResize();
     });
   }
    onResize() {}
 }
  export { Resizer };
```

### Creating the Animation Loop
We’ll be making our loop as a class. The animation loop is what gives “life” to our scene. It allows us to animate different elements at once. It has four properties: a camera, a scene, a renderer, and an updatable array where we must push the elements we want to animate. We also have three methods: the start and stop, which, surprisingly, start and stop our loop, and the tick methods. And this one is pretty important: the tick method will run once for every frame of our animation, which updates the positioning of our objects, making it possible to animate!

All this is possible thanks to the Clock object from Three, which allows us to get the current delta: the time difference between the current frame and the last one.

```js
Loop.js
import { Clock } from 'three';
 
const clock = new Clock();
 
 
class Loop {
 constructor(camera, scene, renderer) {
   this.camera = camera;
   this.scene = scene;
   this.renderer = renderer;
   this.updatables = [];
 }
 
 start() {
   this.renderer.setAnimationLoop(() => {
       this.tick();
       // render a frame
       this.renderer.render(this.scene, this.camera);
   });
 }
 
 stop() {
   this.renderer.setAnimationLoop(null);
 }
 
 tick() {
   const delta = clock.getDelta();
   for (const object of this.updatables) {
       object.tick(delta);
   }
 }
}
 
export { Loop }
```

## Making our scene
We’ll create our scene step by step, file by file, to make our project as modular as possible. Working with Three.js can get messy fast if you are not careful. So it’s best to keep our code clean and organized. Each of these modules will be able to export a function that generates a new instance of something. This will allow us to give them arguments and have several cases, such as lights or objects.

### Creating the Camera
With the camera, we can see and manipulate the point of view of our scene that we want our users to have. We’ll use a Perspective camera, the closest thing to a real-life camera Three.js has, with no distortions.

```js
camera.js
import { PerspectiveCamera } from 'three';
 
function createCamera() {
 const camera = new PerspectiveCamera(
   35, // FOV = Field Of View
   1, // Aspect ratio (dummy value)
   0.1, // Near clipping plane
   100, // Far clipping plane
 );
 
 // Move the camera back so we can view the scene
 //      x y  z
 camera.position.set(0, 0, 10);
 camera.tick = (delta) => {
  
 };
 
 return camera;
}
 
export { createCamera };
```

We can configure the field of the camera’s view, the initial position, and several different parameters with simple numerical variables. Easy, right?

As you will see, all of these components have a tick function, which allows them to communicate with the animation loop if necessary!

### Creating the Lights
Controlling the light is not that different, but here we have more options to choose: from the directional, hemisphere, and point light. Let’s keep it simple for now and import a directional light. You can think of this light as the sun’s light: it shines everything from a unique point in space.

```js
lights.js
import {
   DirectionalLight,
   DirectionalLightHelper,
 } from "three";
  function createLights(color) {
   const light = new DirectionalLight(color, 4);
   const lightHelper = new DirectionalLightHelper(light, 0);
   light.position.set(0, 0, 5);
    light.tick = (delta) => {
   
   };
    return { light, lightHelper };
 }
  export { createLights };
```

Just like with the camera, we can set a position for the light, but this time we can also get a light color as an argument!

In this case, we return two objects: the proper light and an optional light helper that allows us to see where the light is and where it is pointing.

### Creating the Scene
A scene is a place where everything will be rendered. Here is where you can see everything and what the camera will show the user.

```js
scene.js
import { Color, Scene } from 'three';
 
function createScene(color) {
 const scene = new Scene();
 
 scene.background = new Color(color);
 
 return scene;
}
 
export { createScene };
```

Like the lights, we have a color argument, but this time it refers to the scene’s background color.

## Making sure everything’s working correctly
Okay, we have written a whole lot of code. I think we should check if everything is working correctly by actually rendering something.

### Updating World.js
Now it’s time to import everything we created and add it to our World.js file!

```js
World.js
import { createCamera } from "./components/camera.js";
import { createLights } from "./components/lights.js";
import { createScene } from "./components/scene.js";
import { createRenderer } from "./systems/renderer.js";
import { Loop } from "./systems/Loop.js";
import { Resizer } from "./systems/Resizer.js";
 
// These variables are module-scoped: we cannot access them
// from outside the module.
let camera;
let renderer;
let scene;
let loop;
 
class World {
   constructor(container) {
     // Instances of camera, scene, and renderer
     camera = createCamera();
     scene = createScene("blue");
     renderer = createRenderer();
      // Initialize Loop
     loop = new Loop(camera, scene, renderer);
      container.append(renderer.domElement);
      // Light Instance, with optional light helper
     const { light, lightHelper } = createLights("white");
      loop.updatables.push(light);
      scene.add(light);
 
     const resizer = new Resizer(container, camera, renderer);
      resizer.onResize = () => {
      this.render();
     };
 
    }
    render() {
     // Draw a single frame
     renderer.render(scene, camera);
   }
    // Animation handlers
   start() {
     loop.start();
   }
    stop() {
     loop.stop();
   }
 }
  export { World };
```

Our World-class accepts a “container” parameter. This is the HTML element where our Three.js will be rendered.

### Updating our HTML and main.js
Now, in our index.html, we have to add our container where the canvas will be inserted.

```html
index.html
<div id="scene-container">
     <!-- Three.js canvas will be inserted here -->
</div>
```

And lastly, we need to call our World-class in our main.js file while also initializing our animation loop with the ‘start’ method.

```js
main.js
import { createApp } from "vue";
import App from "./App.vue";
import router from "./router";
import { World } from "./World/World.js";
 
function main() {
 // Get a reference to the container element
 const container = document.querySelector("#scene-container");
 
 // Create an instance of the World app
 const world = new World(container);
 
 // Start the loop (produce a stream of frames)
 world.start();
}
 
main();
 
createApp(App).use(router).mount("#app");
```

And if everything goes well, you should see something like this:

<img src="https://miro.medium.com/max/1400/0*eNNW9UfiyKGjvhu7">

Okay, I’m not going to lie: that does look pretty anticlimactic. But it works! As you may have noticed, I declared the scene with a blue background in our World.js. And this big blue blob is the actual 3d scene being rendered, with the animation loop running frame by frame! That’s quite the feat.

What I’d recommend doing now is to update the styles of the container to be fullscreen and to avoid overflow. Note that you’ll have to do a dedicated CSS file in the public folder for this. Something like this should do the trick:

```css
#scene-container {
   position: fixed;
   width: 100%;
   height: 100%;
   left: 0;
   top: 0;
}
```

## Our first Render!
Alright! Now that we have all that boilerplate code working, it’s time to render an object. We will make a moving terrain-like plane where the user can rotate the camera.

### Rendering the plane
You might remember I created an empty “objects “folder inside our components folder. Here’s where all your 3D figures will be stored. In this post, I’ll show you how to make a plane, but if you ever want to dive deeper and render different figures, you should make them here.

```js
terrain.js
import {
 PlaneBufferGeometry,
 MeshStandardMaterial,
 Mesh,
 TextureLoader,
} from "three";
 
export default function createTerrain(props) {
 const loader = new TextureLoader();
 const height = loader.load("textures/height.png");
 //                                        w    h 
 const geometry = new PlaneBufferGeometry(150, 150, 64, 64);
 
 const material = new MeshStandardMaterial({
   color: props.color,
 });
 
 
 
 const plane = new Mesh(geometry, material);
 plane.position.set(0, 0, 0);
 plane.rotation.x -= Math.PI * 0.35;
 
 let frame = 0;
 plane.tick = (delta) => {
 
 };
 
 return plane;
}
```

To render a 3D object, we need quite a lot of things. But these are the three most important:

**Geometry**: These are all the vertices that make our object. It will define its shape. Three.js has different types of geometries for cubes, spheres, and more. We’ll be using the “plane” geometry for our project. We can change the size of our geometry by changing the numbers passed to the element.

**Material**: This is what gives our object a surface. Again, we have several different material styles, but let’s keep it simple and use the ‘Standard Material.’ This is a pretty good option because it can react to light. It accepts an object as a parameter, with several customization options. We will pass a color variable here, just like for the scene’s background.

**Mesh**: is the most used way of visualizing three-dimensional objects on a computer. A mesh is composed of geometry and material, and this is our actual 3D object.

We’ll rotate our mesh; this way, it will be easier to notice it in the scene.

We have to add the terrain to the World.js file, just like we did with all the other elements. Remember to add it to the loop updateable and the scene!

```js
World.js
   ...
   let terrain = createTerrain({
     color: color,
   });
 
   loop.updatables.push(light);
   loop.updatables.push(terrain);
 
   scene.add(light, terrain);
   ...
```

You can also create some global color variables to make the palette easier to switch. I chose some green shades to keep that Vue style.

When updated, you should see your plane being rendered! It might not look three-dimensional at first glance, but try changing the rotation of the plane, and you’ll see that it is a 3D object.

<img src="https://miro.medium.com/max/1400/0*kesExho9a7swXDHx">

## Spicing up our plane geometry
So, you just rendered your first 3D object. Well done! But it’s a bit bland. You can’t even notice the 3D awesomeness. We’ll give it a displacement map and animation to make it look like a voxel alien world.

### Updating World.js
We’ll create an array of random values that will dictate how each vertex in our plane will move.

```js
World.js
   // Random values for terrain vertices
   const randomVals = [];
 
   for (let i = 0; i < 12675; i++) {
     randomVals.push(Math.random() - 0.5);
   }
 
   // Terrain Instance
   let terrain = createTerrain({
     color: color,
     randVertexArr: randomVals,
   });
```

As you saw on the declaration of our plane, our function accepts a ‘props’ argument, which is an object. That means we can pass as many opinions as we want! We’ll pass the values array, so we can use it to manipulate our geometry.

### Adding texture to our plane
Now let’s make our plane, well, less plane. First, we have to update our “Mesh Material.” We’ll add a displacement scale and a map. The displacement map is a grayscale texture that Three.js uses as a guide and elevates each vertex depending on how bright that pixel is on the image. The scale is simply a multiplier; the bigger it is, the more pronounced the displacement will be. Optionally, we can also enable flat shading, giving our terrain a retro lighting style.

<img src="https://miro.medium.com/max/640/0*SVpDQKNUN-QnS5bF">

*Example of a displacement map.*

```js
terrain.js 
 const loader = new TextureLoader();
 const height = loader.load("textures/height.png");
 ...
 const material = new MeshStandardMaterial({
   color: props.color,
   flatShading: true,
   displacementMap: height,
   displacementScale: 5,
 });
```

Pro-tip: Save your texture images inside your public folder! Three.js won’t recognize them in the source folder.

You should already be able to see your terrain!

<img src="https://miro.medium.com/max/1400/0*8mXgpl0KIilF_OYo">

### Animating the terrain
Okay, this part is a little tricky. We will use random numbers and initial vertex position arrays to move all our vertices on the y axis. Then we will animate it using cosine waves to make a controlled motion. I’ll explain the code with some comments:

```js
World.js
   // Random values for terrain vertices
   // We could do this on the terrain.js file,
   // but if we want to have a single random
   // number array for more than one terrain
   // instance, then we would be in trouble.
   const randomVals = [];
   for (let i = 0; i < 12675; i++) {
     randomVals.push(Math.random() - 0.5);
   }
 
   // Terrain Instance
   let terrain = createTerrain({
     color: color,
     randVertexArr: randomVals,
   });
terrain.js
const plane = new Mesh(geometry, material);
 plane.position.set(0, 0, 0);
 plane.rotation.x -= Math.PI * 0.35;
  // Storing our original vertices position on a new attribute
 plane.geometry.attributes.position.originalPosition =
   plane.geometry.attributes.position.array;
 // Utilizing our random number array
 const { array } = plane.geometry.attributes.position;
 for (let i = 0; i < array.length; i++) {
   props.randVertexArr.push(Math.random());
 }
 
 plane.geometry.attributes.position.randomValues = props.randVertexArr;
 
 let frame = 0;
 plane.tick = (delta) => {
   frame += 0.01;
   // Destructuring of the random values, the original position and the current vertex position
   const { array, originalPosition, randomValues } =  plane.geometry.attributes.position;
 
   // Animation for loop
   // In our vertex array, we have 3 coordinates: x, y and z. We are
   // going to animate on ONE coordinate only: the z coordinate.
   // This means we have to omit the x and y coords, hence the i+=3 in our loop.
   // Also, to access that y coordinate on each vertex, we have to add 2 to our
   // current i each time.
   for (let i = 0; i < array.length; i += 3) {
     // Accessing the z coord
     array[i + 2] =
       // Try switching these numbers up, or using sine instead of cosine, see how the animation changes.
       originalPosition[i + 2] + Math.cos(frame + randomValues[i + 2]) * 0.002;
   }
   plane.geometry.attributes.position.needsUpdate = true;
 };
```

And voila! Your terrain is moving!

<img src="https://miro.medium.com/max/640/0*Rkj-nwR99aNLi4E1">

As you can see, it’s a pretty subtle effect. It’s more noticeable in bigger sizes. In any case, you can change the terrain animation loop variables to have a more substantial impact.

## Optional features
Okay! We did a lot in this post. But we can still touch it up a little bit and make our scene even better! I have two more features for you to try:

### Background fog
Trying to hide the end of our plane is not a bad idea. We could stretch it and make it bigger, but that’s not the best solution. First, because it will also stretch our height map, which will make the displacement effect softer and harder to notice (maybe you like that look, though!), and secondly, it will be harder to render on lower-end machines.

Luckily, Three.js is back to the rescue. We can add fog to the scene with just some new lines of code. You can also select the fog’s color and intensity.

```js
scene.js
import { Color, Scene, Fog } from "three";
 
function createScene(color) {
 const scene = new Scene();
 
 scene.background = new Color(color);
 
   scene.fog = new Fog(color, 50, 90);
 
 return scene;
}
 
export { createScene };
```

### Rotation and orbit controls
Another great pre-built tool is the “Orbit Controls”! This lets us rotate and pan our camera with ease. It also comes with an auto-rotate feature, which we will be implementing. We’ll first make a new controls.js file in our systems folder.

```js
controls.js
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls.js";
import { MathUtils } from "three";
 
function createControls(camera, canvas) {
 const controls = new OrbitControls(camera, canvas);
 
 // Enable controls?
 controls.enabled = true;
 controls.autoRotate = true;
 controls.autoRotateSpeed = 0.2;
 
 // Control limits
 // It's recommended to set some control boundaries
 // to prevent the user from clipping with the objects.
 
 // Y axis
 controls.minPolarAngle = MathUtils.degToRad(40); // default
 controls.maxPolarAngle = MathUtils.degToRad(75);
 
 // X axis
 //   controls.minAzimuthAngle = ...
 //   controls.maxAzimuthAngle = ...
 
 // Smooth camera:
 // Remember to add to loop updatables to work.
 controls.enableDamping = true;
 controls.enableZoom = false;
 controls.enablePan = false;
 
 controls.tick = () => controls.update();
 
 return controls;
}
 
export { createControls };
```

Then, it’s time to add it to our World.js, just like any other element in our project. Remember to add it to the loop updatables if you want to enable damping.

<img src="https://miro.medium.com/max/640/0*nGufgC9dy_b89SDS">

You’ll also want to tweak your camera and light position and your initial terrain rotation if you’re going to have a better view angle.

I switched mine to these values for this screenshot:

```
Lights: light.position.set(0, 30, 30);
Camera: camera.position.set(0, 10, 30);
Terrain: plane.rotation.x -= Math.PI * 0.5;
```

## Enabling Vue and keeping the mouse rotation
The last part of the project! Now we have to re-enable Vue. Just uncomment the #app div on your HTML, and that’s it… Right? Well, yes and no. Because even though our 3D background still works perfectly, the Vue elements on top of our background will “steal” all the mouse click events, and therefore the orbit controls won’t be allowed to work.

The solution I’ve come up with is to disable the click events where needed. And that sounds like a chore, but it’s not! With a simple SCSS mixin, we’ll be good to go:

```scss
mixins.scss
@mixin child-events-on($debug: false) {
   & > * {
       @if $debug {
           background: red;
       }
       pointer-events: all;
   }
 
   @content;
}
```

This mixin will enable all pointer-events only on the container’s children. You can enable the debugging mode. It will give a red background to all the elements with pointer-events enabled.

Now, we globally disable pointer-events on the #app, and we can enable them on the containers we want by including the mixin: for example, our nav. In the end, your App.vue should look like this:

```
App.vue
<style lang="scss">
#app {
 position: relative;
 padding: 0px auto;
 padding: 0px 2vw;
 z-index: 2;
 pointer-events: none;
 @include desktop {
   padding: 0px 10vw;
 }
}
 
#nav {
 padding: 30px;
 text-align: center;
 @include child-events-on();
 a {
   font-weight: bold;
   color: $text-color;
 
   &.router-link-exact-active {
     color: $active-link;
   }
 }
}
</style>
```

Pro-tip: Remember to update the #app z-index and the position to be on top of the background.

And, at last, our 3D Portfolio is finally ready!

<img src="https://miro.medium.com/max/640/0*mjSIT96tAF9zip-P">

## Conclusion and next steps
What a ride! We’ve created a web portfolio and made it unique by having a randomly-generated animation for our 3D background. We’ve also improved our developer experience by implementing Vue, which helps us make our code modular and clean, and SCSS, which will save us tons of time while writing our CSS.

Now it’s time to add some personality! I made this example quite bland on purpose. If you want your Portfolio to stand out, you have to think of something more unique. Maybe change the font and the color palette. Or you could change the heightmap texture and the animation strength to convey a different effect. Or just scratch the whole terrain altogether and try something completely new. You can check [my own Portfolio](https://max-fer.vercel.app/) to see a very different approach to practically the same idea. In any case, the best place to start is the [Three.js’ docs](https://threejs.org/docs/), and the [homepage](https://threejs.org/) for some inspiration.

If you want to tinker with this example project, here’s the repo. Or check out my [Vue + Three.js starter](https://github.com/maxfer03/vue-three-starter) to make something different.

Don’t forget to deploy your Portfolio, too! So others can see your amazing 3D scenes 😎

Resources used in this series and recommended reads
- [Vue](https://vuejs.org/)
- [SCSS](https://sass-lang.com/)
- [Three.js](https://threejs.org/)
- [Discover Three.js](https://discoverthreejs.com/)
- [Three.js Crash Course](https://www.youtube.com/watch?v=YK1Sw_hnm58)
- [Three.js Displacement map example](https://www.youtube.com/watch?v=2AQLMZwQpDo)
- [Vue + Three.js starter](https://github.com/maxfer03/vue-three-starter)

source : https://medium.com/nicasource/building-an-interactive-web-portfolio-with-vue-three-js-part-three-implementing-three-js-452cb375ef80