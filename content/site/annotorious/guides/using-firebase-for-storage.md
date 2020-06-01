---
title: "Using Firebase for Storage"
date: 2020-05-01T13:28:54+02:00
draft: false
subsection: "guides"
---

# Using Firebase for Storage

A quick and easy way to set up your own annotation store without managing your own server is 
through [Firebase](https://firebase.google.com/), a web application development platform by Google.
 
Firebase includes a cloud-based document database with a JavaScript SDK for storing, updating and 
deleting JSON records. All you need to do is wire up the Firebase storage SDK operations with the corresponding events from Annotorious.

E.g. when Annotorious fires the `createAnnotation` event, store the annotation
via the `.add()` method on the cloud storage SDK. Storage operations return a `Promise`, so you can execute
code after completion, and handle connection errors.

```javascript
anno.on('createAnnotation', function(annotation) {
  db.collection('annotations').add(annotation).then(function() {
    console.log('Stored annotation');
  });
});
```

The HTML below provides a minimal, but fully working storage example.

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Annotorious | Firebase Demo</title>
    <link href="https://fonts.googleapis.com/css?family=Lato&display=swap" rel="stylesheet">
    <script src="https://recogito.github.io/annotorious/annotorious.min.js"></script>
    <script src="https://www.gstatic.com/firebasejs/7.14.3/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/7.14.3/firebase-firestore.js"></script>
    <script>
      // Firebase will auto-generate this config for you when you create your app.
      // Just paste your own settings in here.
      var firebaseConfig = {
        apiKey: "-- your firebase api key here --",
        authDomain: "-- your authdomain here --",
        databaseURL: "-- your database url --",
        projectId: "-- your project id --",
        storageBucket: "-- your storage bucket --",
        messagingSenderId: "...",
        appId: "..."
      };

      firebase.initializeApp(firebaseConfig);

      var db = firebase.firestore();

      // Helper to find a Firebase doc by annotation ID
      var findById = function(id) {
        var query = db.collection('annotations').where('id', '==', id);
        return query.get().then(function(querySnapshot) {
          return query.docs[0];
        });
      }

      window.onload = function() {
        var image = document.getElementById('hallstatt');
        var anno = Annotorious.init({ image });

        // Load annotations for this image
        db.collection('annotations').where('target.source', '==', image.src)
          .get().then(function(querySnapshot) {
            var annotations = querySnapshot.docs.map(function(doc) { 
              return doc.data(); 
            });

            anno.setAnnotations(annotations);
          });

        anno.on('createAnnotation', function(a) {
          db.collection('annotations').add(a).then(function() {
            console.log('Stored annotation');
          });
        });

        anno.on('updateAnnotation', function(annotation, previous) {
          findById(previous.id).then(function(doc) {
            doc.ref.update(annotation);
          });
        });

        anno.on('deleteAnnotation', function(annotation) {
          findById(annotation.id).then(function(doc) {
            doc.ref.delete();
          });
        });
      }
    </script>
  </head>
  <body>
    <div id="content">      
      <img id="hallstatt" src="https://recogito.github.io/annotorious/640px-Hallstatt.jpg">
      <p class="caption">
        Sample image source: <a href="http://commons.wikimedia.org/wiki/File:Hallstatt_300.jpg">Hallstatt,
        Austria</a>, by Nick Csakany/Wikimedia Commons. Public Domain.
      </p>
    </div>
  </body>
</html>
```