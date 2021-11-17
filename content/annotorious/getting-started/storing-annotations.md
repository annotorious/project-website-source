---
title: "Storing Annotations"
date: 2020-06-01T10:20:00+02:00
draft: false
layout: "section-page"
subsection: "getting-started"
blurb: "To store annotations, you need an annotation server. __Annotorious does not come with built-in storage__. Learn how you can use the JavaScript API to set up your own solution that fits your needs." 
weight: 4
meta_title: "Getting Started: Storing Annotations"
meta_description: "Learn how you can use the Annotorious JavaScript API to integrate a storage backend"
meta_link: "https://recogito.github.io/annotorious/getting-started/storing-annotations"
---

# Storing Annotations

__Important:__ Annotorious does not provide a backend for storing annotations online. It provides 
__user interface functionality only__. To store annotations, you need to connect them to your own 
backend, using the [JavaScript API](/annotorious/api-docs/).

When users create, update or delete annotations, Annotorious emits the following events:

- __createAnnotation__
- __updateAnnotation__
- __deleteAnnotation__

Subscribe to these events, and execute the corresponding write operations to your backend. 

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
See [our Google Firebase storage plugin](https://recogito.github.io/guides/using-firebase-for-storage/) for a free & personal annotation store solution.
