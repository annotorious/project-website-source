---
title: "Web Annotation"
date: 2020-05-17T14:33:09+02:00
draft: false
subsection: "getting-started"
---

Annotorious uses the [W3C Web Annotation model](https://www.w3.org/TR/annotation-model/). For the 
time being, the only supported `selector` type is a single __FragmentSelector__ with a rectangle
selection, according to the [W3C Media Fragments URI](https://www.w3.org/TR/media-frags/) format,
using the `xywh` scheme and pixel coordinates.

```json
{ 
  "@context": "http://www.w3.org/ns/anno.jsonld",
  "id": "#a88b22d0-6106-4872-9435-c78b5e89fede",
  "type": "Annotation",
  "body": [{
    "type": "TextualBody",
    "value": "It's Hallstatt in Upper Austria"
  }],
  "target": {
    "selector": {
      "type": "FragmentSelector",
      "conformsTo": "http://www.w3.org/TR/media-frags/",
      "value": "xywh=pixel:270,120,90,170"
    }
  }
}
```