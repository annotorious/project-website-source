---
title: "Annotorious"
date: 2020-05-17T14:00:29+02:00
draft: false
type: "annotorious"
layout: "splash"
meta_title: "Annotorious"
meta_description: "A JavaScript library for image annotation. Add drawing, commenting and labeling functionality to images on your website with a few lines of JavaScript."
meta_link: "https://recogito.github.io/site/annotorious"
---

```js
var anno = Annotorious.init({
  image: document.getElementById('image-to-annotate')
});

// Load annotations in W3C Web Annotation format
anno.loadAnnotations('annotations.json');

// Attach listeners to handle annotation events
anno.on('createAnnotation', function(annotation)) {
  console.log('Created!');
};
```


