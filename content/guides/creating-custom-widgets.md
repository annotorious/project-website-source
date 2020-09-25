---
title: "Creating Custom Widgets"
date: 2020-06-20T20:56:23+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "__Work in progress.__ A guide that explains the basics of developing your own customized annotation editor."
weight: 5
meta_title: "Creating custom editor widgets"
meta_description: "This guide explains how you can create custom widgets for the Annotorious/RecogitoJS editor popup" 
meta_link: "https://recogito.github.io/guides/creating-custom-widgets"
---

# Creating Custom Editor Widgets

> Please note that this guide is still under construction.

The __editor popup__ is shared between Annotorious and RecogitoJS. It is possible to extend it with your own widgets.
To write your own editor extensions, you should familiarize yourself with some basics first:

- The [W3C Web Annotation](https://www.w3.org/TR/annotation-model/) concept of __annotation bodies__
- Editor __widgets__ and how they relate to annotation bodies

![Editor popup](/images/guides/editor-popup.png)

## Annotation Bodies

In the terminology of the  [W3C Web Annotation](https://www.w3.org/TR/annotation-model/) spec, an 
annotation consists of multiple __bodies__ - data structures that each represent one "piece" of the
annotation. For example, if an annotation consists of one comment and multiple tags, then each is 
encoded as one body in the data structure.

```json
{ 
  "@context": "http://www.w3.org/ns/anno.jsonld",
  "id": "#ce0ed291-766b-4763-8e91-90ce1d04e706",
  "type": "Annotation",
  "body": [{
    "type": "TextualBody",
    "value": "This is a comment",
    "purpose": "commenting"
  }, {
    "type": "TextualBody",
    "value": "A Tag",
    "purpose": "tagging"
  }, {
    "type": "TextualBody",
    "value": "Another Tag",
    "purpose": "tagging"
  }],

  ...

}
```

To specify the body type, the W3C model has a `type` field, and an extra (optional) 
`purpose`. For example, tags and comments are both of type `TextualBody`, but with different 
purposes - `commenting` and `tagging`.

## Editor Widgets

The [Editor class](https://github.com/recogito/recogito-client-core/blob/master/src/editor/Editor.jsx)
handles some basic general concerns, like positioning of the popup window and managing the editing state. 
Otherwise though, it is just a shell for __widgets__. __Widgets__ implement user interface representations for
particular types of __bodies__. The built-in
[CommentWidget](https://github.com/recogito/recogito-client-core/blob/master/src/editor/widgets/comment/CommentWidget.jsx)
handles `TextualBody` nodes with purpose `commenting` or `replying`. The built-in
[TagWidget](https://github.com/recogito/recogito-client-core/blob/master/src/editor/widgets/tag/TagWidget.jsx)
takes care of `TextualBody` nodes with purpose `tagging`. Want to support a new type of body? Make a new widget for it!

## Anatomy of a Widget

[... TODO ...]