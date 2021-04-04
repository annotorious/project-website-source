---
title: "Using Firebase for Storage"
date: 2020-05-01T13:28:54+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "In order to store annotations, you need a server backend that is able to handle W3C Web Annotations. This guide provides a simple recipe for using Firebase, an online cloud storage service by Google, as your personal annotation server."
weight: 4
meta_title: "Using Firebase for Storage"
meta_description: "This guide provides a simple recipe for using Firebase, an online cloud storage service by Google, as your personal annotation server"
meta_link: "https://recogito.github.io/guides/using-firebase-for-storage"
---

# Using Firebase for Storage

A quick and easy way to set up your own annotation store without managing your own server is 
through [Firebase](https://firebase.google.com/), a web application development platform by Google.
Firebase includes a cloud-based document database with a JavaScript SDK for storing, updating and 
deleting JSON records. All you need to do is wire up the Firebase storage SDK operations with the 
corresponding events from Annotorious.

![Firebase Screenshot](/images/guides/firebase.png)
 
## Firebase Storage Explained: Lifecycle Events & CRUD Operations

The basic idea behind integrating Firebase is:

- Listen to Annotorious lifecycle events
- Call corresponding CRUD (Create, Read, Update, Delete) operations through the 
  Firebase SDK.

Example: when Annotorious fires the `createAnnotation` event, store the annotation by 
calling the `.add()` method on your Firebase connection. Firebase operations return 
a `Promise`, so you can execute code after completion, and handle connection errors.

```javascript
anno.on('createAnnotation', function(annotation) {
  db.collection('annotations').add(annotation).then(function() {
    console.log('Stored annotation');
  });
});
```
You can wire up the rest of the events - `update`, `delete` - in the same way.

## Annotorious Firebase Plugin

The [Firebase Storage Plugin for Annotorious and RecogitoJS](https://github.com/recogito/recogito-client-plugins/tree/main/packages/storage-firebase) provides an example implementation.

Include the pluign via the `<script>` tag.

```js
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@recogito/firebase-storage@latest/dist/recogito-firebase-storage.min.js"></script>
```

Or use npm.

```sh
$ npm install @recogito/firebase-storage
```

Initialize the plugin after Annotorious. It will register the lifecycle event listeners
and connect them to the SDK CRUD operations.

```js
var image = document.getElementById('my-image');

// Init Annotorious
var anno = Annotorious.init({ image });

// Firebase will auto-generate this config for you when you 
// register your account. Just paste your own settings in here.
var firebaseConfig = {
  apiKey:        "-- your firebase api key here --",
  authDomain:    "-- your authdomain here --",
  databaseURL:   "-- your database url --",
  projectId:     "-- your project id --",
  storageBucket: "-- your storage bucket --",
  messagingSenderId: "...",
  appId: "..."
};

var settings = {
  annotationTarget: image.src,  // mandatory
  collectionName: 'annotations' // optional (default = 'annotations')
}

// Init the storage plugin
recogito.FirebaseStorage(anno, firebaseConfig, settings);
```