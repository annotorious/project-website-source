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

The __editor popup__ is shared between Annotorious and RecogitoJS. It is possible to extend it with your own widgets. However,
this currently requires you to create your own project fork. (This is going to change - bear with us.) 
To write your own 
editor extensions, you should familiarize yourself with some basics first:

- The [W3C Web Annotation](https://www.w3.org/TR/annotation-model/) concept of __annotation bodies__
- Editor __widgets__ and how they relate to annotation bodies
- Also, check the [Hacker's Guide](/guides/development-setup)

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
particular types of __bodies__. For example, the 
[CommentWidget](https://github.com/recogito/recogito-client-core/blob/master/src/editor/widgets/comment/CommentWidget.jsx)
handles `TextualBody` nodes with purpose `commenting` or `replying`. The 
[TagWidget](https://github.com/recogito/recogito-client-core/blob/master/src/editor/widgets/tag/TagWidget.jsx)
takes care of `TextualBody` nodes with purpose `tagging`. Want to support a new type of body? Make a new widget for it and append it to the Editor!

## The Editor React Component

The Editor is a [React](https://reactjs.org/) component that is instantiated in the following way. In
RecogitoJS, instantiation happens in the [TextAnnotator component](https://github.com/recogito/recogito-client-core/blob/master/src/TextAnnotator.jsx).

```jsx
<Editor
  wrapperEl={this.props.wrapperEl}
  annotation={this.state.selectedAnnotation}
  bounds={this.state.selectionBounds}
  onAnnotationCreated={this.onCreateOrUpdateAnnotation('onAnnotationCreated')}
  onAnnotationUpdated={this.onCreateOrUpdateAnnotation('onAnnotationUpdated')}
  onAnnotationDeleted={this.onDeleteAnnotation}
  onCancel={this.onCancelAnnotation}>
  <CommentWidget />
  <TagWidget />
</Editor>
```

## Anatomy of a Widget

When the Editor opens an annotation, it passes it to the widgets through `props.annotation`. Furthermore,
it attaches a number of event callbacks. These allow the widgets to trigger changes to the annotation bodies:

- `props.onAppendBody` for appending a new body to the annotation
- `props.onUpdateBody` to change an existing body
- `props.onRemoveBody` to delete an existing body from the annotation