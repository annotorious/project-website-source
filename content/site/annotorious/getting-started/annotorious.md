---
title: "Annotorious"
date: 2020-06-01T10:20:00+02:00
draft: false
layout: "section-page"
subsection: "getting-started"
blurb: "The Annotorious __standard version__ works on normal images embedded in websites or web applications."
weight: 1
meta_title: "Getting Started With Annotorious"
meta_description: "Examples and instructions for getting started with the Annotorious image annotation library"
meta_link: "https://recogito.github.io/site/annotorious/getting-started/annotorious"
---

# Getting Started with Annotorious

The Annotorious standard version works on normal images embedded in websites or web applications.

## Using NPM

If you use npm, `npm install @recogito/annotorious` and then

```javascript
import { Annotorious } from '@recogito/annotorious';

const anno = new Annotorious({
  image: document.getElementById('my-image')
});
```

## Using the Script Tag

Download the [latest release](https://github.com/recogito/annotorious/releases/latest)
and add the script to your HTML page at the end of the `<body>` section.

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

  <script type="text/javascript" src="annotorious-2.0.1-alpha.min.js"></script>
</body>
```

For more information on using Annotorious, see the full [API Reference](https://github.com/recogito/annotorious/wiki/API-Reference).



