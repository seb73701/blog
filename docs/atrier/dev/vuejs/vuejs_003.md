---
title: tempo_003
description: 
published: true
date: 2023-04-21T11:13:27.727Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:13:22.783Z
---

---
title: Building an interactive web portfolio with Vue + Three.js — Part Two: Vue Basics
description: 
published: true
date: 2023-02-03T10:45:52.929Z
tags: 
editor: markdown
dateCreated: 2023-01-08T16:27:50.908Z
---

---
title: Building an interactive web portfolio with Vue + Three.js — Part Two: Vue Basics
description: 
published: true
date: 2023-01-27T18:10:37.896Z
tags: 
editor: markdown
dateCreated: 2023-01-08T16:27:50.908Z
---

---
title: Building an interactive web portfolio with Vue + Three.js — Part Two: Vue Basics
description: 
published: true
date: 2023-01-08T16:27:50.908Z
tags: 
editor: markdown
dateCreated: 2023-01-08T16:27:50.908Z
---

# Building an interactive web portfolio with Vue + Three.js — Part Two: Vue Basics

<img src="https://miro.medium.com/max/1400/1*gdd_stw1U8PiSlzk-UspBQ.webp">

In the previous section, we initialized our Vue Project. If you haven’t got the chance to read it, go check it out, it has the initial setup required. 👇

Now It’s time to use Vue and learn how we can structure our webpage with reusable components and keep our code tidy and clean.

## Why Vue instead of React or Angular?
Cutting things short: because it’s simpler.

Vue is a fantastic JavasScript framework that works great for small and big projects. Whereas frameworks like React or Angular are more suitable for bigger enterprise applications.

Remember: we’re just making a personal portfolio, no need for overkill setups! If you have used React or Angular before, you’ll feel right at home using Vue. 😉

## What is a component?
A component is a file that contains code that can be reused with ease. One of the most significant advantages of using web frameworks is that they let us encapsulate our code in smaller, easier-to-read, easier-to-debug components with no hassle.

A Vue component consists of **three** sections:

**Template**: Here is where you write your HTML. In Vue, you can also embed code into the HTML by wrapping it in double curly braces:

```
<template>
  <div>
    {{1 + myVar}}
  </div>
</template>
```

**Script**: This is where you write your component logic. Most of the time, you’ll see a function, known as “data” that returns an object. This is where we can declare the initial reactive state for the component instance. You can send properties (called “props”) to this component. This data is sent as an HTML attribute when calling the component. There are also a whole lot of other things to do in this section but don’t worry; we’ll check them later.

```js
<script>
export default {
 data(){
   return {
       myVar: 2
   }
 }
}
</script>
```

## Structure of a Vue Project
In the Source folder of our project, you’ll find something similar to this file tree:

<img src="https://miro.medium.com/max/334/0*gwKXowzZlQvWEVKB">

**App.vue**: This is the entry point of the project. Here, we will add the elements we want to be always rendered (like the navigation bar or the footer) and the routing.

**Views folder**: Here, we will store the “view” files. You can think of them as Vue components that symbolize different pages on our site, which we will call other components.

**Router folder**: This is the folder where the routing configs are stored. If you don’t know what routing is, it’s pretty simple: with the Vue CLI, we have already installed Vue Router, an extension that lets us add navigation to our site without needing the browser to reload when switching the URL.

**Components folder**: This is where all our reusable components will be stored! Things like buttons, containers, you name it. The components you intend to use on different views should be stored in this folder.

**Assets folder**: As we saw in the last chapter, here is where all our SCSS and static files are stored.

## Routing
With Vue’s routing plugin, we’ll be able to add navigation to our website and access different routes without reloading our browser. We’ll see how we can configure it to make a new page where we can show off our personal projects!

### Making the Vue File
First, we have to make our view Vue file, where we’ll render our page-specific information.

This is pretty straightforward! Make a file called “Projects.vue” inside the views folder, and use the default Vue boilerplate. (Remember to add some text to see if it renders something and specify the style’s language and scope!)

It should look like this:

```
//Projects.vue
<template>
   <div class="projects-section">
       This is the Projects view
   </div>
</template>
 
<script>
export default {};
</script>
 
<style lang="scss" scoped>
.projects-section {
}
</style>
```

Pro-tip: It’s recommended to install the Vetur plugin to add proper code coloring and code snippets for Vue in VS code and the Vue Devtools browser extension.

### Configuring the Router
After creating the file, you need to specify the URL path in index.js inside the router folder.

You’ll need to import the Projects file and add this object right after the “Home” route object:

```js
//router/index.js 
import Projects from '../views/Projects.vue'
...
{
   path: '/projects',
   name: 'Projects',
   component: Projects
 },
```

This is how we declare the path to the new route (in this case, being “yourweburl.com/projects”) and the component to be rendered there (the “Projects” component).

### Adding the Router Link
Now it’s time to access this new route! Vue Router comes with built-in navigation components called router-links: these are native components that accept a “to” prop. It is used to specify where that element should send to us when we click it. We will add it in our prebuilt “Nav” inside our App.vue file. This way, it will always render, and therefore we’ll always be able to switch pages!

```
App.vue
<div id="nav">
   <router-link to="/">Home</router-link> |
   <router-link to="/projects">Projects</router-link>
 </div>
```
You can also customize how the active link should look with the “router-link-exact-active” class.

If everything is working correctly, you should be able to go to the new “projects” route when clicking on the top link!

<img src="https://miro.medium.com/max/1400/0*809R4_vt-nbqWrUv">

## Our first Component
We’ll make a straightforward text container to understand some of the capabilities of a component. As always, we need to start by creating a TextContainer.vue file inside our components folder, in which we’ll add the typical Vue boilerplate code.

Inside the template section, we’ll use the following code: a div with a “text-container” class, and inside it, we’ll also add a “slot” component. This will allow us to accept template content from component calls! We’ll see more about that in a minute.

```
//TextContainer.vue
<template>
 <div class="text-container">
   <slot/>
 </div>
</template>
```

Now, we need to call our component on our Home view. To do this, we need to import it, and thanks to the slot component, we can write in it as if it were a simple HTML element.

```
//Home.vue
<template>
 <div class="home">
   <TextContainer> Lorem Ipsum </TextContainer>
 </div>
</template>
 
<script>
import TextContainer from '@/components/TextContainer.vue';
 
export default {
 name: "Home",
 components: {
   TextContainer,
 },
};
</script>
```

Pro-tip: @ is an alias to /src . You can always use it inside Vue files.

And the best thing: as I said earlier, components are reusable. You can call this text container as many times as you want!

So with a little HTML and CSS magic, we can get something like this with less than half as much code:

<img src="https://miro.medium.com/max/1400/0*c9ajFTPp4L3Q3nCT">

```
<template>
 <div class="home">
   <TextContainer>
     <h2>About me</h2>
     <p>
       Lorem ipsum dolor sit, amet consectetur adipisicing elit. Autem minus
       at, nobis ut beatae incidunt quos a rerum et odit esse ullam sit aliquam
       vel! Doloribus provident magnam accusamus architecto velit a dolores
       explicabo rerum aperiam unde quas sunt magni corrupti, quis vitae vero.
       Explicabo adipisci perferendis pariatur dolores illum!
     </p>
     <p>
       Lorem ipsum dolor sit, amet consectetur adipisicing elit. Autem minus
       at, nobis ut beatae incidunt quos a rerum et odit esse ullam sit aliquam
       vel! Doloribus provident magnam accusamus architecto velit a dolores
       explicabo rerum aperiam unde quas sunt magni corrupti, quis vitae vero.
       Explicabo adipisci perferendis pariatur dolores illum!
     </p>
   </TextContainer>
   <TextContainer>
     <h2>My Skills</h2>
     <ul>
       <li>Lorem ipsum dolor sit amet.</li>
       <li>Lorem ipsum dolor sit amet.</li>
       <li>Lorem ipsum dolor sit amet.</li>
       <li>Lorem ipsum dolor sit amet.</li>
     </ul>
   </TextContainer>
   <TextContainer>
     <h2>Contact me!</h2>
     <p>johndoe@gmail.com</p>
   </TextContainer>
 </div>
</template>
 
<script>
// @ is an alias to /src
import TextContainer from "@/components/TextContainer.vue";
 
export default {
 name: "Home",
 components: {
   TextContainer,
 },
};
</script>
 
<style lang="scss" scoped>
.home {
 display: flex;
 gap: 20px;
 flex-flow: row wrap;
   .text-container {
     flex: 1 1 0px;
     min-width: 45%;
   }
 }
</style>
```

## Getting a Little Bit Deeper: a Projects Showcase Component
Alright, now that we know the basic functionalities of a component, it’s time to learn some of the core concepts. And what better way to do it than by implementing a project showcase carousel?

First and foremost, we have to create the Showcase.vue component and render it on the Projects view. I won’t be going into details. I’m sure you are pretty comfortable with doing that now. I’ll go straight to the point and show you the script section:

```
Showcase.vue
<script>
export default {
 data() {
   return {
     projects: [
       {
         title: "My first Project",
         desc: `Lorem ipsum  ...`,
         techs: ["PostgreSQL", "React", "Redux", "Node"],
         urlText: "Github",
         url: "https://github.com/maxfer03/solana-digital-nomads",
         img: "https://efectocolibri.com/wp-content/uploads/2021/01/placeholder.png",
       },
       ...
     ],
     index: 0,
   };
 },
 methods: {
   async next() {
     if (this.projects.length - 1 === this.index) {
       this.index = 0;
     } else {
       this.index++;
     }
   },
   async prev() {
     if (this.index === 0) {
       this.index = this.projects.length - 1;
     } else {
       this.index--;
     }
   },
 },
};
</script>
```

## Data
As I mentioned, the data function returns all the reactive variables we will use in the component. If you come from a React background, you can think of it as a React state. In this case, we have two variables inside our data: a “projects” array, where we will store all our projects as objects, and the “index,” which is a number. This will be useful to know the current rendered project. We will manipulate this variable to see the next or the previous projects.

## Methods
The methods object is where you store all your functions. These methods can (and probably will) interact with the data variables to make our component truly reactive and updatable. As you can see, we have two ways: the prev and following functions. They update the index element depending on its value, so we can continuously cycle through our projects without getting out of boundaries.

Note: We must call our variables using the “this” keyword to access them. It won’t work otherwise.

This is just the tip of the iceberg regarding what tools we have inside our script section, but it will do just fine for our small portfolio. If you want to dig deeper, you can check the Vue lifecycle, computed properties, and the list goes on!

## The HTML

```
<template>
 <div class="proj-showcase">
   <div class="carousell">
     <button class="carousell-btns btn-back" @click="prev">{{ "<" }}</button>
     <div class="carousell-content">
       <img
         v-if="projects[index].img"
         :alt="projects[index].title + ' image'"
         class="carousell-img"
         :src="projects[index].img"
       />
 
       <div class="carousell-content-text">
         <h3>{{ projects[index].title }}</h3>
         <h4>
           <span v-for="(tech, i) in projects[index].techs" :key="i">
             {{ tech }}{{ i === projects[index].techs.length - 1 ? "" : ", " }}
           </span>
         </h4>
         <p>
           {{ projects[index].desc }}
         </p>
         <a v-if="projects[index].url" :href="projects[index].url">{{
           projects[index].urlText
         }}</a>
         <span v-else>{{ projects[index].urlText }}</span>
       </div>
     </div>
     <button class="carousell-btns btn-next" @click="next">{{ ">" }}</button>
   </div>
 </div>
</template>
```

There are a few new things to note on the HTML here:

**Double braces**: We can embed logic into our template HTML by wrapping the code inside double braces. This way, we can render our variables with ease.

Note: unlike the Script section, here, we can call our variables without the “this” keyword.

**@click** : This is an event. Just as you can have the onClick event in React or Javascript, we apply the @ syntax when using events on Vue: @click, @mouseenter, @mouseleave, etc. As you can see, it’s also possible to call functions when the circumstances are fired.

**:src, :href, :alt** : Using a colon before any given prop is Vue’s strategy to embed Javascript into HTML props and attributes.

**v-if, v-else** :This is a beneficial Vue attribute that allows us to render HTML conditionally! If the v-if condition returns false, then the v-else code will render.

**v-for**: This is probably my favorite Vue feature. It’s a simple for loop that allows us to cycle through some Javascript code or our reactive variables. No hassle, no array mapping, just a simple for loop with a pythonic syntax.

With this knowledge and some CSS, we can get a scalable project and a nice-looking carousel in no time!

<img src="https://miro.medium.com/max/1400/0*8CM_R9HRGY6XiVon">

## Conclusion and next Steps
You can now create a reactive, modular, and clean Vue-powered website with what you’ve learned in this post. Now it’s time to be a little more creative! Aggregate a footer component, or edit the navbar styles, whatever you want! Remember to harness the power of SCSS variables and global classes or even try to introduce some mixins! The sky’s the limit.

Also, take your time to write some nice things about yourself. A portfolio’s priority is to help you sell yourself, so show off! Reveal your most significant strengths, and let your personality shine through your words and stylings. Don’t forget to check out Github for some inspiration! Here and here are some places to start.

You can visit [the repo](https://github.com/maxfer03/vue-three-ns) if you want to use this same code as a base. Beware that if you visit it after the series finishes, you’ll be spoiling it for yourself by anticipating the Three.js implementation!

In the next and last section, we’ll finally implement Three.js into our project and develop a 3D interactive background. Stay tuned!

source : https://medium.com/nicasource/building-an-interactive-web-portfolio-with-vue-three-js-part-two-vue-basics-12fcfc85e033