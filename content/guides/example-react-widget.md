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

> __WORK IN PROGRESS!__ The code in this tutorial will __not yet work__ in the current release 
> versions, but will be supported with the next upcoming Annotorious release.

In this guide, we re-recreate the example [color selector widget from the previous tutorial](/guides/editor-widgets/) 
as a React component. The widget adds a section to the editor with three buttons: red, green and blue. 
Clicking one of the buttons will set an annotation body with the color value, and body purpose of `highlighting`. 


![Editor popup](/images/guides/colorselector-widget.png)

JSX code for the plugin is below. A full project (including a webpack build configuration) 
is [available on GitHub](https://github.com/recogito/recogito-client-plugins/tree/main/packages/widget-react-helloworld).
Here's what the code does, step by step:

1. To get the annotation's current color value, we grab the first body with `purpose: 'highlighting'`.
2. The `setHighlightBody` function is called when the user selects a color. It uses `props.onUpsertBody`: 
   this callback either replaces the the annotation's current `highlighting` body (=update), or adds a
   new `highlighting` body, in case the annotation doesn't have one already (=insert).
3. The remainder of the code renders the user interface elements: 3 identical buttons
   in different colors. Clicking a button triggers `setHighlightBody`.

```jsx
import React from 'preact/compat';

const HelloWorldWidget = props => {

  const { annotation } = props;

  // We'll be using 'highlighting' as body purpose for 
  // this type of widget
  const currentHighlight = annotation ? 
    annotation.bodies.find(b => b.purpose === 'highlighting') : null;

  // This function handles body updates as the user presses buttons
  const setHighlightBody = value => () => {
    props.onUpsertBody(currentHighlight, { value, purpose: 'highlighting' });
  }

  return (
    <div className="helloworld-widget">
      { [ 'red', 'green', 'blue' ].map(color => 

        <button 
          key={color}
    
          // Set class to 'selected' if this button value equals current highlight
          className={currentHighlight?.value === color ? 'selected' : null}

          // Set color value as CSS background
          style={{ backgroundColor: color }}

          // On click add (or replace) the current highlight body
          onClick={setHighlightBody(color)} />

    )}
    </div>
  )

}

export default HelloWorldWidget;
```

## Using the Plugin

When you include the plugin with a `<script>` tag, it will make itself available
through a global `recogito.HelloWorldWidget` variable. Add it to the `widgets` list 
in the config object when you initialize Annotorious.

```html
<!DOCTYPE html>
<html>
  <head>
    <!-- Load Annotorious first -->
    <link href="/annotorious.min.css" rel="stylesheet">
    <script src="/annotorious.min.js"></script>

    <!-- Load plugin script next -->
    <script src="recogito-helloworld-widget.js"></script>
  </head>
  <body>
    <img id="hallstatt" src="640px-Hallstatt.jpg" />

    <script type="text/javascript">
      (function() {

        // Add plugin to the widgets on init
        var anno = Annotorious.init({
          image: 'hallstatt',
          widgets: [ 
            recogito.HelloWorldWidget,
            'COMMENT',
            'TAG'
          ]
        });
        
      })();
    </script>
  </body>
</html>
```

Full project [available on GitHub](https://github.com/recogito/recogito-client-plugins/tree/main/packages/widget-react-helloworld).