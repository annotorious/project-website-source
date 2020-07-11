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

The __OpenSeadragon plugin__ is an extension to the [OpenSeadragon](http://openseadragon.github.io/)
viewer for zoomable high-resolution images. Click the annotation to edit. Hold `SHIFT` while clicking 
and dragging the mouse to create a new annotation.

{{< inline-osd-demo >}}

Download the [latest release](https://github.com/recogito/annotorious-openseadragon/releases/latest)
and include it in your web page. Make sure to include the script __after__ the
OpenSeadragon script.

```html
<script src="openseadragon/openseadragon.2.4.2.min.js"></script>
<script src="openseadragon-annotorious.min.js"></script>
```

Alternatively, grab the script from CDN.

```html
<script src="https://cdn.jsdelivr.net/npm/@recogito/annotorious-openseadragon@2.0.6/dist/openseadragon-annotorious.min.js"></script>
```

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