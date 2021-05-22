---
title: "Getting Started"
date: 2020-05-17T14:10:57+02:00
draft: false
layout: "section-index"
include_summaries: false
meta_title: "Getting Started With Annotorious"
meta_description: "Examples and instructions for getting started with the Annotorious image annotation library"
meta_link: "https://recogito.github.io/annotorious/getting-started"
---

# Getting Started with Annotorious

Annotorious lets your users select __rectangle__ and __polygon shapes__ on images, and add comments
and tags. Try it on the image below: __click__ or __tap__ the annotation to edit, __click or tap anywhere 
and drag__ to create a new annotation. 

Use the icons to switch between rectangle and polygon drawing mode. When drawing a polygon, double click 
closes the shape. When using touch, keep the finger on the screen and hold for a while to close the polygon. 

{{< inline-demo >}}

## Include via Script Tag

To include Annotorious on your page, download the [latest release](https://github.com/recogito/annotorious/releases/latest)
and add the script and stylesheet files to your page source code.


```html
<head>
  <link rel="stylesheet" href="annotorious.min.css">
</head>

<body>
  <div id="content">
    <img id="hallstatt" src="640px-Hallstatt.jpg">
  </div>
  <script>
    (function() {
      var anno = Annotorious.init({
        image: 'hallstatt' // image element or ID
      });

      anno.loadAnnotations('annotations.w3c.json');

      // Add event handlers using .on  
      r.on('createAnnotation', function(annotation) {
        // Do something
      });
    })()
  </script>

  <script type="text/javascript" src="annotorious.min.js"></script>
</body>
```

## From CDN

Alternatively, you can grab script and stylesheet directly from the [jsDelivr CDN](https://www.jsdelivr.com/package/npm/@recogito/annotorious). 

```html
<!-- CSS stylesheet -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@recogito/annotorious@{{< version-annotorious >}}/dist/annotorious.min.css">

<!-- JS -->
<script src="https://cdn.jsdelivr.net/npm/@recogito/annotorious@{{< version-annotorious >}}/dist/annotorious.min.js"></script>
```

## Using NPM

If you use npm, `npm install @recogito/annotorious` and then

```javascript
import { Annotorious } from '@recogito/annotorious';

import '@recogito/annotorious/dist/annotorious.min.css';

const anno = new Annotorious({
  image: document.getElementById('my-image')
});
```

# A Note on Styled Images

Because of the way Annotorious works, some CSS style rules applied directly to the \<img\> 
element can cause compatibility issues. This is especially the case for `position`, `margin` 
and `padding` rules.

If you need to apply these CSS styles, please do not apply them to the `<img>` directly.
Instead, __wrap the image in a \<div\> and apply your styles to the wrapper \<div\>__. 

```html
<!-- This does not work -->
<img src="my-image-1.jpg" style="position:absolute; top:10px; left:10px;" >
<img src="my-image-2.jpg" style="padding:20px" >

<!-- This works -->
<div style="position:absolute; top:10px; left:10px;">
  <img src="my-image-1.jpg">
</div>

<div style="padding:20px;">
  <img src="my-image-2.jpg">
</div>
```


# OpenSeadragon Plugin

There is a separate version of Annotorious which plugs into the [OpenSeadragon viewer](http://openseadragon.github.io/)
for high-resolution images. Setup is just as easy as for the standard version. [Read more](/annotorious/getting-started/osd-plugin)

# Next Steps

Once you have the basics up and running, you will probably want to know more about 
Annotorious' [JavaScript API](/annotorious/api-docs/annotorious/), or check our
[guides section](/guides) for more help and tutorials. 

# Read More

- [OpenSeadragon Plugin](/annotorious/getting-started/osd-plugin/)
- [Web Annotation Model](/annotorious/getting-started/web-annotation/)
- [Storing Annotations](/annotorious/getting-started/storing-annotations/)
