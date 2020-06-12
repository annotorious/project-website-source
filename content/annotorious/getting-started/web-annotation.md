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

Annotorious supports the [W3C Web Annotation model](https://www.w3.org/TR/annotation-model/), an open standard for
interoperable annotations on the web. However, only a part of the specification is supported at the moment. 
Current limitations are:

- Only annotations with a __single rectangle__ shape are supported
- The [W3C Media Fragments](https://www.w3.org/TR/media-frags/) `FragmentSelector` is the only supported selector type
- Annotation `TextualBody` types with a `purpose` of `commenting`, `replying` or no `purpose` are displayed as comments
- Anntation bodies with a `purpose` of `tagging` are displayed as tags
- Bodies of any other type are ignored (unless you implement your own editor extension)

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

## Annotation IDs

When adding an annotation via the API, the annotation should have an `id`. In principle, 
Annotorious accepts any alphanumeric string. Annotations created by users will automatically 
get a globally unique ID in the form `#{uuid}`. 