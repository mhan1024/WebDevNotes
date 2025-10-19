# ✅ Vue
Website: <a href="https://vuejs.org">https://vuejs.org</a>

Table of Contents:
- [Project Setup (Vite)](#-project-setup-vite)
- [Project Setup (Vue CLI)](#-project-setup-vue-cli)
- [Project Structure](#%EF%B8%8F-project-structure)
- [Basics](#-basics)
- [Methods](#%EF%B8%8F-methods)
- [Computed Properties](#-computed-properties)
- [Watchers](#-watchers)
- [Directives](#-directives)
- [Router](#%EF%B8%8F-router)
- [Forms](#-forms)

## 🧩 Project Setup (Vite)
- Create a new project\
 `npm create vite@latest <project-name>`
- Select Vue when prompted to choose a framework.
- Navigate into the project\
  `cd <project-name>`
- Install dependencies\
   `npm install`
- Run the development server (port 5173)\
   `npm run dev`
- Go to <a href="http://localhost:5173">http://localhost:5173</a>

## 🧩 Project Setup (Vue CLI)
- Install Vue CLI (if needed)\
  `npm install -g @vue/cli`
- Create a new project\
  `vue create <project-name>`
- Navigate into the project\
  `cd <project-name>`
- Run the development server (port 8080)\
   `npm run serve`
- Go to <a href="http://localhost:8080">http://localhost:8080</a>

## 🏗️ Project Structure
  ```
  <project-name>/ 
  ├─ public/           # Static assets (index.html) 
  ├─ src/
  │  ├─ assets/        # Images, styles, fonts
  │  ├─ components/    # Reusable Vue components
  │  ├─ pages/         # Views / Pages
  │  ├─ App.vue        # Root component
  │  └─ main.js        # Entry point, router & app initialization
  ├─ package.json
  └─ README.md
  ```

## 📘 Basics
- Text interpolation: `{{ Vue instance }}`
- Vue instance: connects data, HTML, and logic
  ```
  const app = Vue.createApp({
    data() {
      return {
        message: 'Hello Vue!'
      }
    }
  })
  
  app.mount('#app')
  ```
  
## ⚙️ Methods
Used to store functions that belong to the Vue instance
- Syntax:
  ```
   methods: {
     function() {
       // do something
     }
   }
  ```

## 🧠 Computed Properties
A variable that automatically calculate a value based on other data and update whenever the data changes
- Syntax:
  ```
   computed: {
     function() {
       // do something
     }
   }
  ```
  
## 👀 Watchers
A method that "watches" a data property and reacts when it changes by running some code automatically
- Syntax:
  ```
   watch: {
     function() {
       // do something
     }
   }
  ```

## 🎯 Directives
These are special HTML attributes that give the HTML tag extra functionality.
- Denoted with the prefix `v-`
- Creates responsive pages with less code \
  
v-bind: lets us bind an HTML attribute to data and allows changing the attribute value dynamically
- Syntax:
  `<div v-bind:[attribute]="[Vue data]"></div>`
- Can be used to change the class attribute
  ```
   <div v-bind:class="className">
     The class is set with Vue
   </div>
  ```
- Shorthand: `<div :[attribute]="[Vue data]"></div>"`

v-if: lets us create an HTML element only if a condition is true (conditional rendering)
- Includes: `v-if, v-else-if, v-else`
- Syntax: `<div v-if="condition"></div>`

v-show: lets us hide an element when the condition is false
- Syntax: `<div v-show="condition">This div tag can be hidden when condition == false</div>`

v-for: lets us create HTML elements based on an array
- Elements created with this directive will automatically update when the array changes
- Syntax:
  ```
   <ol>
     <li v-for="x in arr">{{ x }}</li>
   </ol>
  ```
- Get index:
  ```
   <p v-for="(x, index) in arr">
     {{ index }}: "{{ x }}"
   </p>
  ```
  
v-on: lets us perform actions based on specified events
- onclick Event: `<tag v-on:click="action"></tag>`
- oninput Event: `<tag v-on:input="action"></tag>`
- mousemove Event: `<tag v-on:mousemove="action"></tag>`
- Shorthand: `<tag @:[attribute]="action"></tag>"`

v-model: lets us create a link between the input element's value attribute and a data value in the Vue instance
- Syntax: `<input type="type" v-model="Vue instance">`
  
## 🗺️ Router
- Install Vue Router package\
  `npm install vue-router`\
- In `main.js`:
  ```
  import { createRouter, createWebHistory } from 'vue-router'
  import <name>Page from './pages/<name>Page.vue'
  ...

  const router = createRouter({
    history: createWebHistory(),
    routes: [
      { path: '/<path>', component: <name>Page },
      ...
    ]
  });

  const app = createApp(App)
  app.use(router);
  app.mount('#app');
 
  ```
- In `App.vue`:
  ```
  <script setup>
    import <name>Page from './pages/<name>Page.vue'
    ...
  </script>

  <template>
    <div>
      <router-link to="/<path>">PAGE</router-link>
      ...
      <router-view></router-view>
    </div>
  </template>
  ```

## 📝 Forms
- v-model: a special attribute that creates 2-way binding between a form input and a data property
- 2-way binding: when the user types something in the input, the data will update automatically, and when the data changes, the input will update automatically
```
<template>
  <form @submit.prevent="submitForm">
    <input v-model="form.name" placeholder="Name" />
    <input v-model="form.email" placeholder="Email" />
    <button type="submit">Submit</button>
  </form>
</template>

<script>
import { reactive } from 'vue';

export default {
  setup() {
    const form = reactive({
      name: '',
      email: ''
    });

    const submitForm = () => {
      console.log(form.name, form.email);
    };

    return { form, submitForm };
  }
};
</script>
```


