---
title: "Web Annotation Model"
date: 2020-05-17T14:33:09+02:00
draft: false
layout: "section-page"
subsection: "getting-started"
blurb: "Annotorious uses the [W3C Web Annotation model](https://www.w3.org/TR/annotation-model/). Learn the basics of how annotations are encoded as JSON, and what parts of the standard Annotorious currently supports."
weight: 3
meta_title: "Annotorious & Web Annotation"
meta_description: "Learn the basics of how Annotorious uses the W3C Web Annotation standard"
meta_link: "https://recogito.github.io/annotorious/getting-started/web-annotation"
---

# The W3C Web Annotation Model 

Annotorious supports a subset of the [W3C Web Annotation model](https://www.w3.org/TR/annotation-model/), 
an open standard for interoperable annotations. 

- Only a __single target__ shape per annotation is supported.
- __[FragmentSelectors](https://www.w3.org/TR/annotation-model/#fragment-selector)__ of type [Media Fragment](https://www.w3.org/TR/media-frags/) 
  are supported for rectangle shapes (see example below).
- __[SVG Selectors](https://www.w3.org/TR/annotation-model/#svg-selector)__ supported for polygon shapes.
- Annotation `TextualBody` types with a `purpose` of `commenting`, `replying` or no `purpose` are displayed as __comments__.
- Annotation bodies with a `purpose` of `tagging` are displayed as __tags__.
- Bodies of any other type are __ignored__, unless you implement your own editor extension.


## Annotation IDs

__Important:__ Annotorious requires an `id` on every annotation. The ID can be any alphanumeric string. 
Annotations created by users will automatically get a globally unique ID in the form `#{uuid}`. 

## Example

```json
[{ 
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
}, { 
  "@context": "http://www.w3.org/ns/anno.jsonld",
  "id": "#218d01ff-f077-4cc3-992d-1c81c426e51b",
  "type": "Annotation",
  "body": [{
    "type": "TextualBody",
    "purpose": "tagging",
    "value": "Church"
  }],
  "target": {
    "selector": [{
      "type": "SvgSelector",
      "value": "<svg><polygon points='172,160 199,15 223,6 239,132 244,173 285,179 313,208 313,251 218,306 170,290 172,160'></polygon></svg>"
    }]
  }
}]
```