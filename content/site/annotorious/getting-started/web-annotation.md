---
title: "Web Annotation Model"
date: 2020-05-17T14:33:09+02:00
draft: false
layout: "section-page"
subsection: "getting-started"
weight: 3
---

# The W3C Web Annotation Model 

Annotorious uses the [W3C Web Annotation model](https://www.w3.org/TR/annotation-model/). Only a limited range 
of the specification is supported at the moment.

- Only annotations with a single rectangle selection
- `selector` must be a __FragmentSelector__, according to the 
  [W3C Media Fragments URI](https://www.w3.org/TR/media-frags/) format
- Only `xywh` scheme with pixel coordinates
- `id` __should__ be provided, but can be any alphanumeric string
- When creating new annotations, Annotorious will automatically generate globally unique IDs in the
  form `#{uuid}`

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