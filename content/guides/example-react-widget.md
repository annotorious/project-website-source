---
title: "React Editor Widget"
date: 2021-02-20T10:28:42+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "This guide re-implements the example widget from the previous guide in React."
weight: 6
meta_title: "Example React Widget"
meta_description: "An example Annotorious/RecogitoJS editor widget built with React."
meta_link: "https://recogito.github.io/guides/example-react-widget"
---

# An Example React Editor Widget

In this guide, we re-recreate the [example color selector widget](/guides/editor-widgets/) as a React component.

> __WORK IN PROGRESS__ This tutorial does __not yet work__ in the current release version,
> but will be supported with the next Annotorious release.

![Editor popup](/images/guides/colorselector-widget.png)

The widget adds a section to the editor with three buttons (red, green, blue). Clicking a button sets an annotation
body with purpose `highlighting`. JSX code for the plugin is below. A full project (including a webpack build 
configuration) is [available on GitHub](https://github.com/recogito/recogito-client-plugins/tree/main/packages/widget-react-helloworld).

```jsx
import React from 'preact/compat';

const HelloWorldWidget = props => {

  const { annotation } = props;

  // We'll be using 'highlighting' as body purpose for 
  // this type of widget
  const currentHighlight = annotation ? 
    annotation.bodies.find(b => b.purpose === 'highlighting') : null;

  // This function handles body updates as the user presses buttons
  const setHighlightBody = value => evt => {
    props.onUpsertBody(currentHighlight, { value, purpose: 'highlighting' });
  }

  // Just a helper to create the color buttons
  const createButton = value =>
    <button 
      // Set class to 'selected' if this button value equals current highlight
      className={currentHighlight?.value === value ? 'selected' : null}

      // Set color value as CSS background
      style={{ backgroundColor: value }}

      // On click add (or replace) the current highlight body
      onClick={setHighlightBody(value)}></button>

  return (
    <div className="helloworld-widget">
      { createButton('red') }
      { createButton('green') }
      { createButton('blue') }
    </div>
  )

}

export default HelloWorldWidget;
```

Here's what the code does, explained step by step:

1. We grab the first body with `purpose: 'highlighting'` from the annotation.
2. The `setHighlightBody` function is called whenever the user makes a selection in the widget. 
   It uses the `props.onUpsertBody` function to add or replace the current `highlighting`
   body with the currently selected value.
3. The remainder of the code renders the user interface elements: 3 identical buttons
   in different colors. Clicking a button triggers `setHighlightBody`.


## Using the Plugin

Include the plugin in your page, and add it to the `widgets` list in the initialization 
config object. 

```html
<!DOCTYPE html>
<html>
  <head>
    <link href="/annotorious.min.css" rel="stylesheet">
    <script src="/annotorious.min.js"></script>

    <!-- Loading the script creates a global 'recogito.HelloWorldWidget' var -->
    <script src="recogito-helloworld-widget.js"></script>
    <style>
      .helloworld-widget {
        padding:5px;
        border-bottom:1px solid #e5e5e5;
      }

      .helloworld-widget button {
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

      .helloworld-widget button.selected,
      .helloworld-widget button:hover {
        opacity:1;
      }
    </style>
  </head>
  <body>
    <img id="hallstatt" src="640px-Hallstatt.jpg" />

    <script type="text/javascript">
      (function() {
        var anno = Annotorious.init({
          image: 'hallstatt',
          widgets: [ 
            recogito.HelloWorldWidget, // Add the plugin here
            'COMMENT',
            'TAG'
          ]
        });
      })();
    </script>
  </body>
</html>
```
