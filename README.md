# Vue.js
Personalized Notes for Vue.js

## Extensions
```
Vetur | For Vue specific syntax highlighting (VsCode)
Vue.js devtools | To Help Debug (Browser Extension)
```

## Resources
* [Vue Router](https://router.vuejs.org/guide/)
## Set Up
* Like Angular, we have a VUE cli to support us
```
npm i @vue/cli
```
* `vue ui` opens an UI in `locahost:8080` to set up the project

### Firebase Setup
```
npm install vuefire firebase
```
```js
// main.js
import { createApp } from 'vue'
import App from './App.vue'

import { VueFire, VueFireFirestoreOptionsAPI } from 'vuefire'

const app = createApp(App)

app.use(VueFire, {
    modules: [VueFireFirestoreOptionsAPI()]
})

app.mount('#app')

```
```js
// firebase.js
import { initializeApp } from 'firebase/app';
import { getFirestore } from 'firebase/firestore';
import { getAuth } from 'firebase/auth';
import { getStorage } from 'firebase/storage';

const firebaseConfig = {
    apiKey: "AIzaSyDgKbsC-8fjY-WzlmPrL-C0wNWvNXuJu5w",
    authDomain: "walkie-talkie-dcfee.firebaseapp.com",
    projectId: "walkie-talkie-dcfee",
    storageBucket: "walkie-talkie-dcfee.appspot.com",
    messagingSenderId: "829120246881",
    appId: "1:829120246881:web:6c9bf19d2b0e63dd326620"
  };

export const app = initializeApp(firebaseConfig);

export const auth = getAuth(app);
export const db = getFirestore(app)
export const storage = getStorage(app);
```
### Routing Setup
```
npm install vue-router  
```
```js
// main.js
import { createRouter, createWebHashHistory } from 'vue-router'
import HomeComponent from './components/HomeComponent'

const routes = [
    { path: '/', component: HomeComponent}
]

// Router Set Up
const router = createRouter({
    history: createWebHashHistory(),
    routes
})

const app = createApp(App)
app.use(router)
app.mount('#app')
```
```html
<!-- App.vue -->
<template>
  <router-view></router-view>
</template>
```
* Adding `router-view` in `App.vue` allows us to use all the routes

## Passing Data to Template
* To pass data to the template of a component you need to use the `data()` function
```html
<template>
  <button @click="signInAnonymously(auth)">Login</button>
</template>

<script>
  import { auth } from '../firebase'
  import { signInAnonymously } from '@firebase/auth';

  export default {
      data() {
          return { auth, signInAnonymously }
      }
  }
</script>
```

## Register a Component 
* After you create a component, you need to register it in the component array before using it
```html
<template>
    <h1>Home Page</h1>
    <LoginComponent />
</template>

<script>
  import LoginComponent from './LoginComponent.vue';

  export default {
      components: {
          LoginComponent
      }
  }

</script>
```

## Slot
* **Slot** can be used to pass data from parent component to chiild component
```html
<!-- HomeComponent.vue -->

<UserComponent>
    <!-- this template fragment is passed to the child - UserComponent -->
    <template #userrr="{ user }">
        
        {{ user.email }}

    </template>

</UserComponent> 
```
```html
<!-- UserComponent.vue -->
<div >
    <slot name="userrr" :user="firebaseUser"></slot>
</div>
```

* If there are multiple _slots_ in UserComponent, we target the slot with the name from the parent component, `#userrr` in this case. If it is not a named slot, then it would be the _default_ slot (`#default`)

### Slot Props
* You can have _slot_ props, which can be accessed by the parent component. In the above example, `user` is a slot 

## Conditional Rendering
```html
<UserProfile  v-if="loggedIn" />
<LoginComponent v-else />
```
* Here, UserProfile is displayed if the _loggedIn_ value evaluates to be true

## Passing props to children 
```html
<UserProfile :user="firebaseUser" />
```
* Here, we pass the prop _user_ to the childComponent- UserProfile
```js
export default {
    data () {
       ...
    },
    props: ['user']
}
```
* Register the prop _user_ and you can start using it directly in the template