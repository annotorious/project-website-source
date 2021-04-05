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

Want to use Annotorious only for the drawing tools? But don't need 
the editor? That's what __headless mode__ is for! 

With headless mode, you keep all the standard functionality: drawing 
and editing shapes, JavaScript API, lifecycle events. But you are free to
build your own user interface, without being forced to the standard
editor popup.  

## The Headless API

To use Annotorious without the editor popup, the following API options are 
important:

- `disableEditor`, a config option and instance field to switch headless mode
  on and off
- the `createSelection` event, which fires when the user has created a new selection
- the `selectAnnotation` event, which fires when the user selects an existing annotation
- `updateSelected`, an API method for modifying annotation bodies programmatically
- `saveSelected`, an API method that provides a programmatic way to save an (updated)
  annotation

### disableEditor

The `disableEditor` option defines whether Annotorious will show the editor (normal mode)
or not (headless mode). You can disable the editor when you initialize Annotorious:

```js
var anno = Annotorious.init({
  image: 'hallstatt',
  disableEditor: true
});
```

Or you can change the value of the `disableEditor` instance field at any time.

```js
anno.disableEditor = true; // Annotorious is now in headless mode

anno.disableEditor = false; // back to normal mode
```

### createSelection and selectAnnotation

In normal mode, the editor opens when the user creates a new selection, or selects
an existing annotation. In headless mode, you want your code to handle these events.

You can do this by attaching handler functions for the `createSelection` and `selectAnnotation` 
events.

```js
anno.on('createSelection', async function(selection) {
  // The user has created a new shape...
});

anno.on('selectAnnotation', async function(annotation) {
  // The users has selected an existing annotation
});
```

### updateSelected and saveSelected

In your handler functions, you will typically perform modifications on the
selected annotation. The `updateSelected` method allows you to replace the 
current selection with a modified one.

After you have called `updateSelected`, the annotation is still in draft state.
To make the change persistent, you need to save it by calling `saveSelected`.
This will persist the change (and Annotorious will fire `createAnnotation` or 
`updateAnnotation` events accordingly).

> __Important:__ `updateSelected` is an asynchronous operation. It takes a while
> to complete and returns a `Promise`. Make sure you wait until `updateSelected`
> completes before calling `saveSelected`!

```js
// This does not work
anno.updateSelected(modified);
anno.saveSelected();

// This works
await anno.updateSelected(selection);
anno.saveSelected();

// And this works, too
anno.updateSelected(selection).then(function() {
  anno.saveSelected();
});
```

Because some applications will often apply only a single change, and then save immediately,
`updateSelected` also includes a convenience "update and save" option.

```js
// This updates and saves immediately, no
// more need to call .saveSelected
anno.updateSelected(modified, true);
```

## Code Example

The code below shows a full working example: when the user 
creates a new shape, the application inserts a tag automatically, and 
saves the annotation.  (The tag is hard-wired in the example. But you could 
easily make it a variable that's changed via a text input or dropdown in 
your page.) 

1. activates headless mode on init
2. attaches a `createSelection` listener, which inserts a (hard-coded) tag into new selections
3. saves the annotation

```js
var anno = Annotorious.init({
  image: 'hallstatt',
  locale: 'auto',

  // Step 1: activate headless mode by 
  // disabling the editor
  disableEditor: true
});

// Step 2: add a listener that modifies selections
// created by the user
anno.on('createSelection', async function(selection) {

  // Tag to insert
  selection.body = [{
    type: 'TextualBody',
    purpose: 'tagging',
    value: 'MyTag'
  }];

  // Step 3: update the selection and save it
  // Remember that .updateSelected is an async function!
  // You need to wait until it completes before saving
  await anno.updateSelected(selection);
  anno.saveSelected();

  // Alternative:
  // anno.updateSelected(selection, true);
});
```