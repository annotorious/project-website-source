---
title: "Storing Annotations"
date: 2020-06-01T10:20:00+02:00
draft: false
layout: "section-page"
subsection: "getting-started"
weight: 4
---

# Storing Annotations

To store annotations, you need an annotation server or database backend.
Annotorious - as well as its sister project [RecogitoJS](https://github.com/recogito/recogito) - __do not 
include their own storage solution__. They provide __the user interface functionality__ for annotation 
__only__. To store annotations, you need to connect them to your own backend, using the [API](https://github.com/recogito/annotorious/wiki/API-Reference).

When an annotation is created, updated or deleted by the user, Annotorious emits the following events 
through the API:

- __createAnnotation__
- __updateAnnotation__
- __deleteAnnotation__

You need to subscribe to these events, and execute the corresponding write operations to your backend. 

```javascript
anno.on('createAnnotation', function(annotation) {
  // This part depends entirely on how your backend works
  axios.post('http://www.example.com/annotations/).then((response) => {
    console.log('Stored.');
  });
});
```

## Using Cloud Storage Services

If you don't want to run your own application backend, cloud storage services offer a good alternative.
See [this example for using Google Firebase](https://github.com/recogito/annotorious/wiki/Using-Firebase-for-Storage) to create your own free & personal annotation store with minimum effort.
