---
title: "Extending the Editor"
date: 2020-09-24T14:24:22+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "This guide explains how you can extend the Annotorious/RecogitoJS editor popup with your own UI components."
weight: 2
meta_title: "Customizing the Annotorious/RecogitoJS editor"
meta_description: "This guide explains how you can extend the Annotorious/RecogitoJS editor popup with your own UI components."
meta_link: "https://recogito.github.io/guides/extending-the-editor"
---

# Extending the Editor

Per default, the Annotorious/RecogitoJS editor popup features two widgets:

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

- The name of a built-in widget - currently either `COMMENT` or `TAG`
- A plugin function (see more info on [creating your own widget plugins below]())
- An object with a `widget` property that has the widget name or plugin function as value, and additional
  arguments that will serve as configuration for the widget

## Extending the Editor 

You can extend the editor with your own __widget functions__. A widget function is just an ordinary JavaScript 
function that takes a set of input arguments (details below) and returns a DOM element, which will appear
in the editor.

> Before you start writing your own widgets, you should first familiarize yourself with the
> the [W3C Web Annotation specification](/annotorious/getting-started/web-annotation/), in 
> particular the concepts of __annotation bodies__ and body __purposes__, and 
> [how they relates to editor widgets](/guides/creating-custom-widgets/). 

### Widget Code Example

![Editor popup](/images/guides/colorselector-widget.png)

This minimal widget adds a simple color selector to the editor. Clicking the
selector creates a new annotation body with the purpose `highlighting`.

1. It searches the annotation bodies for a body with `purpose: 'highlighting'`
2. If a body exists, it stores the body value in the `currentColorValue` variable
3. The `addTag` function is called whenever the user makes a selection in the widget. 
   (We'll create the widget later in the code). If the annotation already has a
   `highlighting` body, `addTag` updates this body. If the annotation does not have
   a `highlighting` body yet, `addTag` appends a new one.
4. The remainder of the code renders the user interface elements: 3 identical buttons
   in different colors. Clicking a button triggers `addTag`.
5. Just add a bit of CSS for style

Since the `highlighting` body is now stored in the annotation, we can write a 
[formatter](/annotorious/api-docs/annotorious/#formatters) that renders highlighted 
annotations in different colors. 
 
```js
var ColorSelectorWidget = function(args) {

  // 1. Find a current color setting in the annotation, if any
  var currentColorBody = args.annotation ? 
    args.annotation.bodies.find(function(b) {
      return b.purpose == 'highlighting';
    }) : null;

  // 2. Keep the value in a variable
  var currentColorValue = currentColorBody ? currentColorBody.value : null;

  // 3. Triggers callbacks on user action
  var addTag = function(evt) {
    if (currentColorBody) {
      args.onUpdateBody(currentColorBody, {
        type: 'TextualBody',
        purpose: 'highlighting',
        value: evt.target.dataset.tag;
      });
    } else { 
      args.onAppendBody({
        type: 'TextualBody',
        purpose: 'highlighting',
        value: evt.target.dataset.tag;
      });
    }
  }

  // 4. This part renders the UI elements
  var createButton = function(value) {
    var button = document.createElement('button');

    if (value == currentColorValue)
      button.className = 'selected';

    button.dataset.tag = value;
    button.style.backgroundColor = value;
    button.addEventListener('click', addTag); 
    return button;
  }

  var container = document.createElement('div');
  container.className = 'colorselector-widget';
  
  var button1 = createButton('RED');
  var button2 = createButton('GREEN');
  var button3 = createButton('BLUE');

  container.appendChild(button1);
  container.appendChild(button2);
  container.appendChild(button3);

  return container;
}
```

```css
/* 5. CSS styles for the color selector widget */
.helloworld-plugin {
  padding:5px;
  border-bottom:1px solid #e5e5e5;
}

.helloworld-plugin button {
  outline:none;
  border:none;
  display:inline-block;
  width:20px;
  height:20px;
  border-radius:50%;
  cursor:pointer;
  opacity:0.5;
  margin:4px;
}

.helloworld-plugin button.selected,
.helloworld-plugin button:hover {
  opacity:1;
}
```