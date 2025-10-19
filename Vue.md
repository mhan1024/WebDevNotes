# ✅ Vue
Website: <a href="https://vuejs.org">https://vuejs.org</a>\
Table of Contents
- [Project Setup (Vite)](#project-setup-vite)
- [Project Setup (Vue CLI)](#project-setup-vue-cli)
- [](##)
- [](##)
- [](##)

<a id="project-setup-vite"></a>
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

<a id="project-setup-vue-cli"></a>
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
<a id=""></a>
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

## 🗺️ Router
<a id=""></a>
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
<a id=""></a>
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
