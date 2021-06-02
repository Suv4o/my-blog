<h1 align="center">MY PERSONAL BLOG</h1>
<p align="center">
Website: <a href="https://www.trpkovski.com/">https://www.trpkovski.com/</a>
</p>

<p align="center">
<img src="https://res.cloudinary.com/suv4o/image/upload/c_scale,q_10,w_400/v1621774991/github/my-blog_qgsorv.gif" />
</p>

<p align="center">
My personal blog was build with <a href="https://vuepress.vuejs.org/">VuePress</a>, a vue-powered static site generator. The media content is hosted and managed by <a href="https://cloudinary.com/">Cloudinary</a>. This allows for images to be delivered in the correct format and size on every browser. <a href="https://firebase.google.com/">Firebase</a> was used to send emails via the Get In Touch page and Subscription form . Follow the instructions below on how to connect your Firebase account. Feel free to jump on the link above to engage with the website.
</p>

<h2 align="center">Getting Started is Simple</h2>

#### Install dependencies

```
npm install
```

#### Configure your Firebase

Add your Firebase configuration in `config.js`

```js
    [
      "script",
      {},
      `var config = {
        apiKey: "YOUR FIREBASE API KEY",
        authDomain: "YOUR FIREBASE AUTH DOMAIN",
        databaseURL: "YOUR FIREBASE DATABASE URL",
        projectId: "YOUR FIREBASE PROJECT ID",
        storageBucket: "YOUR FIREBASE STORAGE BUCKET",
        messagingSenderId: "YOUR FIREBASE MESSAGING SENDER ID",
        appId: "YOUR FIREBASE appId",
        measurementId: "YOUR FIREBASE MEASUREMENT ID",
      };
      firebase.initializeApp(config);
      const firestore = firebase.firestore();
      const functions = firebase.functions();`,
    ],
```

#### Serve at localhost:8080

```
npm run dev
```

#### Generate static project

```
npm run build
```
