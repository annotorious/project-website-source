---
title: "Configuring the Editor"
date: 2020-09-24T14:24:22+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "This guide explains how you can configure the widgets that appear in the editor, and how you can pass additional configuration options to specific widgets."
weight: 1
meta_title: "Configuring the Annotorious/RecogitoJS editor"
meta_description: "This guide explains how you can configure the widgets that appear in the editor, and how you can pass additional configuration options to specific widgets."
meta_link: "https://annotorious.github.io/guides/configuring-the-editor"
---

# Customizing the Editor

Per default, the Annotorious editor popup features two widgets:

- A __comment widget__ for writing comments and replies
- A __tag widget__ for adding tags, either freetext or supported by a pre-configured vocabulary and an autosuggest dropdown

![Editor popup](/images/guides/editor-popup-with-vocab.png)

You can can customize this setup when you initialize Annotorious/RecogitoJS using the 
`widgets` config option. The configuration below will give you the editor shown in the 
image above.

```js
var anno = Annotorious.init({
  image: 'hallstatt',
  widgets: [
    'COMMENT',
    { widget: 'TAG', vocabulary: [ 'Animal', 'Building', 'Waterbody'] }
  ]
});
```

Alternatively, the configuration below will give you an editor with just a comment widget, but no tag widget.

```js
var anno = Annotorious.init({
  image: 'hallstatt',
  widgets: [
    'COMMENT'
  ]
});
``` 

Each item in the `widgets` array can be one of the following:

- The name of a __built-in widget__ - currently either `COMMENT` or `TAG`
- An editor __plugin__ 
- An object with a `widget` property that has the widget name or plugin as value, and 
  additional configuration arguments for the widget

If you want to learn how to write your own extensions for the editor, 
[start with this guide](/guides/editor-widgets/).

## Tagging Vocabularies

__Note:__ the tag widget also supports semantic vocabularies, where each term has a label and a URI.

```js
var anno = Annotorious.init({
  image: 'hallstatt',
  widgets: [{ 
    widget: 'TAG',
    vocabulary: [ 
      { label: 'Place', uri: 'http://www.example.com/ontology/place' },
      { label: 'Person', uri: 'http://www.example.com/ontology/person' }, 
      { label: 'Event', uri: 'http://www.example.com/ontology/event' }
    ] 
  }]
});
```
