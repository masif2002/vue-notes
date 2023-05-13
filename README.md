# Vue.js
Personalized Notes for Vue.js

## Extensions
```
Vetur | For Vue specific syntax highlighting (VsCode)
Vue.js devtools | To Help Debug (Browser Extension)
```

## Resources
* [Vue Router](https://router.vuejs.org/guide/)
* [Slots in Vue](https://vuejs.org/guide/components/slots.html)
* [Realtime Data fetching with Vuefire](https://vuefire.vuejs.org/guide/options-api-realtime-data.html)
* [Navigation Guards](https://router.vuejs.org/guide/advanced/navigation-guards.html#global-before-guards)
* [router.push() | Programmatic Navigation](https://router.vuejs.org/guide/essentials/navigation.html)
* [Ref in Vue](https://vuejs.org/guide/essentials/template-refs.html)
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
import { firebaseApp } from './firebase'


const app = createApp(App)

// Setup for only firestore with VueFire 
app.use(VueFire, {
    firebaseApp, 
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

export const firebaseApp = initializeApp(firebaseConfig);

export const auth = getAuth(firebaseApp);
export const db = getFirestore(firebaseApp)
export const storage = getStorage(firebaseApp);
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
## Named Routes
```js
const routes = [
    { path: '/', component: HomeComponent},
    { path: '/chat/:id', component: ChatRoom,  name: 'chat'},
]
```
```html
<router-link :to="{name: 'chat', params: {id: chat.id}}">{{ chat.id }}</router-link>
```
* `router-link` is just like an `<a>` tag in html. Since, we named the routes, we can reference it with the name in the template
* Side note: We can get the query params like this `this.$route.params.<param_name_defined_in_router>` 
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
      data () {
          return { auth, signInAnonymously }
      }
  }
</script>
```
* Similarly, you can also pass data using `computed` property. But the main difference is that data that has some custom logic that needs to be performed
```html
<template>
    <p>Welcome to chat room <strong>{{ chatId }}</strong></p>
</template>

<script>
export default {
    computed: {
        chatId() {
            return this.$route.params.id
        }
    }
}
</script>
```  
* Both the properties are reactive. Meaning, the component is re rendered when the values change
## Scoped Styles
```js
<style scoped>
    button {
        color: #0000
    } 
</style>
```
* Using the **scoped** keyword keeps the style scoped to the particular component 

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
* You can have _slot_ props, which can be accessed by the parent component. In the above example, `user` is a slot prop

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

## Two Way Binding
* You can bind variables that are subjected to change whether in the template or in the script. Regardless of where the value is mutated, the value will be reflected everywhere it is used. We can achieve this using `v-model` in Vue
```html
<input type="email" v-model="email">
<input type="password" v-model="password">
``` 
```js
export default {
    data() {
        return { 
            email:'',
            password:'',
        }
    }
}
```
## Directives
### If Else
```html
<UserProfile v-if="user" />
<LoginComponent v-else />
```
* If the value on the right returns to be true, the component is rendered, or else the component with `v-else` directive is rendered if provided
### List Rendering
```html
<li v-for="chat of chats" :key="chat.id">
    {{ chat.id }}
</li>
```
* `v-for` directive is used to loop over a list. The `:key` must be provided to uniquely identify the segment

## Real time data fetching with VueFire
```js
export default {
    props: ['userId'],
    data () {return {
        chats: []
    }},
    // Realtime Data fetching using VueFire
    watch: {
        // Waits until UserId is received from the parent component         
        userId: {
            immediate: true,
            handler(userId) {
                this.$firestoreBind('chats', query(collection(db, 'chats'), where('owner', '==', userId)))
            }
        }
    },
}
```
* Here, we watch for the _UserId_ prop. After it has been received from the parent component, we use it to fetch data from firestore in realtime. Any data that is updated and added in the firestore database is reflected in the app realtime 
> Ref: https://vuefire.vuejs.org/guide/options-api-realtime-data.html