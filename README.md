# Vue.js
Personalized Notes for Vue.js

## Extensions
```
Vetur | For Vue specific syntax highlighting (VsCode)
Vue.js devtools | To Help Debug (Browser Extension)
```
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