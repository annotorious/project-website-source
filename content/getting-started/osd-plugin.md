---
title: "OpenSeadragon Plugin"
date: 2020-06-01T10:20:00+02:00
draft: false
layout: "section-page"
subsection: "getting-started"
blurb: "The __OpenSeadragon plugin__ is an extension to the [OpenSeadragon](http://openseadragon.github.io/) viewer for zoomable high-resolution images."
weight: 2
meta_title: "Getting Started With Annotorious OpenSeadragon"
meta_description: "Examples and instructions for getting started with the Annotorious OpenSeadragon plugin for image annotation"
meta_link: "https://recogito.github.io/annotorious/getting-started/osd-plugin"
---

# Getting Started with the OpenSeadragon Plugin

The __Annotorious OpenSeadragon plugin__ is an extension to the [OpenSeadragon](http://openseadragon.github.io/)
viewer for zoomable high-resolution images. __Click or tap__ the annotation to edit. Hold the `SHIFT` key while 
clicking and dragging the mouse to create a new annotation.

{{< inline-osd-demo >}}

Download the [latest release](https://github.com/recogito/annotorious-openseadragon/releases/latest)
and include script and stylesheet file in your web page. Make sure to include the script __after__ the
OpenSeadragon script.

> Important! Annotorious version 2.7.x strictly requires OpenSeadragon 3.0 or higher.
> For compatibility with OpenSeadragon v2.4 and below, please use Annotorious 2.6.x. 

```html
<!-- CSS stylesheet -->
<link rel="stylesheet" href="annotorious.min.css">

<!-- JS -->
<script src="openseadragon/openseadragon.3.0.0.min.js"></script>
<script src="openseadragon-annotorious.min.js"></script>
```

Alternatively, grab the files from CDN.

```html
<!-- CSS stylesheet -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@recogito/annotorious-openseadragon@{{< version-osd >}}/dist/annotorious.min.css">

<!-- JS -->
<script src="https://cdn.jsdelivr.net/npm/@recogito/annotorious-openseadragon@{{< version-osd >}}/dist/openseadragon-annotorious.min.js"></script>
```

> Important! You __must__ include the stylesheet in the \<head\> of your page. If you include it
> anywhere in your \<body\>, Annotorious will not work when switching OpenSeadragon to fullscreen mode. 

Create the viewer and initialize the Annotorious plugin.

```html
<script>
  window.onload = function() {
    var viewer = OpenSeadragon({
      id: "openseadragon1",
      tileSources: {
        type: "image",
        url: "1280px-Hallstatt.jpg"
      }
    });

    // Initialize the Annotorious plugin
    var anno = OpenSeadragon.Annotorious(viewer);

    // Load annotations in W3C WebAnnotation format
    anno.loadAnnotations('annotations.w3c.json');
  }
</script>
```

The plugin takes a config object as optional second argument. See the [API Reference](/annotorious/api-docs/osd-plugin/) for details.

```javascript
var anno = OpenSeadragon.Annotorious(viewer, config);
```

## Using NPM

If you use npm, `npm install @recogito/annotorious-openseadragon` and then

```javascript
import OpenSeadragon from 'openseadragon';
import * as Annotorious from '@recogito/annotorious-openseadragon';

import '@recogito/annotorious-openseadragon/dist/annotorious.min.css';

const viewer = OpenSeadragon({
  id: "openseadragon",
  tileSources: {
    type: "image",
    url: "1280px-Hallstatt.jpg"
  }
 });

const config = {}; // Optional plugin config options

Annotorious(viewer, config);
```

# A Note on Fullscreen Mode

If you experience the problem that __annotations don't appear in OpenSeadragon
fullscreen mode__: please make sure that you include the Annotorious 
stylesheet file (`annotorious.min.css`) __in the &lt;head&gt; of your document!__

Reason: when switching to fullscreen mode, OpenSeadragon temporarily removes 
all content from the page body, and replaces it with the viewer element. If you
include the Annotorious stylesheet somewhere inside the body, it will get removed 
from the page in fullscreen mode, causing annotations to disappear. __ALWAYS add 
the Annotorious stylesheet in the document &lt;head&gt;!__

