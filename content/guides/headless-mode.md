---
title: "Headless Mode"
date: 2021-04-03T10:28:42+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "A Guide to Headless Mode, which allows you to use Annotorious without the editor popup."
weight: 3
meta_title: "Using Headless Mode"
meta_description: "A Guide to Headless Mode, which allows you to use Annotorious without the editor popup."
meta_link: "https://recogito.github.io/guides/using-headless-mode"
---

# Headless Mode: Using Annotorious without the Editor Popup

Need only the drawing tools? But not need the editor? That's what 
__headless mode__ is there for! 

With headless mode, you keep all the standard functionality: drawing 
and editing shapes, JavaScript API, lifecycle events. But you can build
your own user interface, without being forced to the standard
Annotorious editor popup.  

Demo coming soon...

## The Headless API

To use Annotorious without the editor popup, the following API options are 
most important:

- `disableEditor`, a config option and instance field to switch headless mode
  on and off
- the `createSelection` event, which fires when the user has created a new selection
- `updateSelected`, an API method for modifying annotation bodies programmatically
- `saveSelected`, an API method that provides a programmatic way to save an (updated)
  annotation

### disableEditor

Coming soon...

### createSelection

Coming soon...

### updateSelected and saveSelected

updateSelected and saveSelected

## Code Example

This minimal example:

- activates headles mode on init
- attaches a `createSelection` listener which inserts a (hard-coded) tag into new selections
- saves the annotation

```js
var anno = Annotorious.init({
  image: 'hallstatt',
  locale: 'auto',

  // Important: this switches Annotorious 
  // to headless mode!
  disableEditor: true
});

anno.on('createSelection', async function(selection) {
  // When the user creates a selection, insert this tag
  selection.body = [{
    type: 'TextualBody',
    purpose: 'tagging',
    value: 'MyTag'
  }];

  // Remember that .updateSelected is an async function!
  await anno.updateSelected(selection);
  anno.saveSelected();

  // Alternative:
  // anno.updateSelected(selection, true); // Saves immediately
});
```