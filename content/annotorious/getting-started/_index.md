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

Annotorious lets your users draw rectangle selections on images, and add comments
and labels. Try it out on the image below: __click the annotation__ to edit, __click and drag__ 
with the mouse to create a new annotation

{{< inline-demo >}}

## Include via Script Tag

To include Annotorious on your page, download the [latest release](https://github.com/recogito/annotorious/releases/latest)
and add the script at the end of the `<body>` section.


```html
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

Alternatively, you can grab the script from the [jsDelivr CDN](https://www.jsdelivr.com/package/npm/@recogito/annotorious). 

```html
<script src="https://cdn.jsdelivr.net/npm/@recogito/annotorious@2.0.4/dist/annotorious.min.js"></script>
```

## Using NPM

If you use npm, `npm install @recogito/annotorious` and then

```javascript
import { Annotorious } from '@recogito/annotorious';

const anno = new Annotorious({
  image: document.getElementById('my-image')
});
```

# OpenSeadragon Plugin

There is a separate version of Annotorious which plugs into the [OpenSeadragon viewer](http://openseadragon.github.io/)
for high-resolution images. Setup is just as easy as for the standard version. [Read more](/annotorious/getting-started/osd-plugin)

# Next Steps

Once you have the basics up and running, you will probably want to know more about the data format
of the annotations, and how you can store them permanently. Read more about the 
[W3C Web Annotation Model](/annotorious/getting-started/web-annotation) and [how to integrate Annotorious 
with a backend](/annotorious/getting-started/storing-annotations).