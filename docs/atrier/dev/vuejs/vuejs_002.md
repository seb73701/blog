---
title: tempo_002
description: 
published: true
date: 2023-04-21T11:13:20.690Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:13:16.136Z
---

---
title: Building an interactive web portfolio with Vue + Three.js — Part One: Introduction and Setup
description: 
published: true
date: 2023-02-03T10:45:47.947Z
tags: 
editor: markdown
dateCreated: 2023-01-08T16:17:05.329Z
---

---
title: Building an interactive web portfolio with Vue + Three.js — Part One: Introduction and Setup
description: 
published: true
date: 2023-01-27T18:10:33.587Z
tags: 
editor: markdown
dateCreated: 2023-01-08T16:17:05.329Z
---

---
title: Building an interactive web portfolio with Vue + Three.js — Part One: Introduction and Setup
description: 
published: true
date: 2023-01-08T16:17:05.329Z
tags: 
editor: markdown
dateCreated: 2023-01-08T16:17:05.329Z
---

# Building an interactive web portfolio with Vue + Three.js — Part One: Introduction and Setup

<img src="https://miro.medium.com/max/1400/1*S6vyb2U9Jn5aZujZ2CxQuQ.webp">

It is safe to say that a web portfolio is one of the best ways to show off your projects and share your brand. But this is even more important in the case of web developers, to whom a personal website is the best way to show what one can do.

## Designing as a non-designer
Here’s the thing: it’s pretty hard to make a nice-looking portfolio when you are just starting. And it’s even more complicated when you are not a designer — which is my case.

A good portfolio must communicate your brand and stand out at first sight, but how can we get there when our strengths lie in programming, not design?

I have an idea for you: implementing **3D geometry**! Computer graphics never get out of fashion, and we can create amazing interactive elements while showing off our core strength: programming 😎!

## But graphics programming is HARD, isn’t it?
Of course, it is!

But don’t worry; we will not render the next Minecraft from scratch. As always, the web development scene is packed with incredible tools and libraries, and we’ll be using:

- **[Three.js](https://threejs.org/)**, a JavaScript library that simplifies the rendering process for us. You can think of it as an abstraction of WebGL, the open-source web graphics engine.
- **[Vue.js](https://vuejs.org/)**, a popular JavaScript framework that handles routing and encapsulates our logic into reusable components.
- **[SASS](https://sass-lang.com/)** for a better development experience while making the styles.

At the end of this short series, you’ll be able to build something like this:

<img src="https://miro.medium.com/max/1400/0*LsK8YrESulMAHF3I">

*The project may look a little bit different when we finish the series.*

## Requirements
The requirements for this project are pretty basic! If you know:

✅ Javascript
✅ HTML
✅ CSS

Then you are good to go 👍

## Let’s get started!
With all that said, let’s kick off our project by setting up our Vue app!

### Initializing our Vue project
The easiest way to initialize a Vue app is by using the [Vue CLI](https://cli.vuejs.org/). It’s really straightforward and saves you a lot of time. It will feel similar if you have ever worked with Create React App. If you haven’t, don’t worry, we’ll go step by step.

1. Installing the Vue CLI

You’ll need to download the package from NPM. This command will install the CLI globally.

```
npm install -g @vue/cli
```

2. Initializing the project

Here is where the CLI really shines. You can configure your project by selecting between multiple presets and options. You can do this with the "vue" command that you have installed earlier with npm:

```
vue create your-project-name
```

For our project, we’ll be using the following configuration:

- Preset: Manually select features
- Features ( You can still install the rest, but these are a must):
  - Choose Vue version
  - Babel
  - Router
  - CSS Pre-processors
- Vue version: 3.x
- CSS preprocessor: Sass/SCSS (with dart-sass)
- Placing config: whatever fancies you
- Now we wait…

And done! If everything went well, you should be able to open the folder, run "npm run serve", and see this on your local 8080 port:

<img src="https://miro.medium.com/max/1400/0*XCFsp8mK9vVEeP4I">

*How about that? 👀*

## Configuring Global SCSS stylesheets

Now that our project is initialized, we need to make some changes so we can use global .scss files where we can store our variables, mixins, whatever! We’ll get a little deeper into SCSS in the next chapter.

1. Creating our files
You’ll see a folder called “assets” inside the source folder. Here, we’ll have different folders for static files, like images, svgs, and in our case: the stylesheets.
Inside our assets folder, we create the “scss” folder. In there, we’ll create our variables file, called “variables.scss” (Pretty original, right?)
In the end, the folder should look like this:

<img src="https://miro.medium.com/max/632/1*UyMqiLnX20QqAuB0L3yZNg.webp">

2. In this file, we'll create a test color variable: "$test-variable: #A0D995".
As you may have guessed by now, SCSS allows us to store colors, sizes, and more as variables we can call whenever we want. But beware because it would fail if we tried to use that variable in a Vue component. We’ll fix that with the next and last step.

3. Configuring Vue
You’ll find a file called ‘vue.config.js’ at the root of our project. If you don’t, create it.
Inside it, write:

```js
module.exports = {
  css: {
    loaderOptions: {
      scss: {
//if not working, try with "additionalData" instead of "prependData"
        prependData: `
            @import "@/assets/scss/variables.scss";
          `,
      },
    },
  },
};
```

In the future, if you want to add more .scss files, simply add them here, one import after the other.

4. Calling the variable
Try calling the variable inside a Vue component’s `<style>` tag. For example, as a background color for the App.vue file.

Now restart the project, and… Voila! Your project should look something like this:

<img src="https://miro.medium.com/max/1400/0*EuIWYuPzKKJ1-PNO">

*Do you see how the background changed? This means the variable was successfully called, and we are ready to go!*

## Next Steps

All right! We finished our initial configuration. We initialized our Vue project and configured it to accept global stylesheets. In the next chapter, we’ll dive deeper into Vue and SCSS, and build some components for our portfolio!

For now, if you want to start learning on your own, check out [Vue’s official documentation](https://vuejs.org/guide/introduction.html). Or if you already know Vue and want to start playing with Three.js, check out my [vue-three starter on GitHub](https://github.com/maxfer03/vue-three-starter)!

source : https://medium.com/nicasource/building-an-interactive-web-portfolio-with-vue-three-js-part-one-introduction-and-setup-405426cec84