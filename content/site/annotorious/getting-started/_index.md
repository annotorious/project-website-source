---
title: "Getting Started"
date: 2020-05-17T14:10:57+02:00
draft: false
---

If you use npm, `npm install @recogito/annotorious` and then

```javascript
import { Annotorious } from '@recogito/annotorious';

const anno = new Annotorious({
  image: document.getElementById('my-image')
});
```

Otherwise download the [latest release](https://github.com/recogito/annotorious/releases/latest)
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



