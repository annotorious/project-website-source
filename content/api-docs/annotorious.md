---
title: "API Docs | Annotorious"
date: 2020-05-17T14:14:36+02:00
draft: false
layout: "api-doc"
meta_title: "Annotorious API Docs"
meta_description: "API Documentation for the Annotorious image annotation library"
meta_link: "https://annotorious.github.io/api-docs/annotorious"
---

# API Reference: Annotorious

> The __standard version__ of Annotorious works with normal images embedded in websites
> or web applications. If you are looking for the Annotorious OpenSeadragon plugin,
> [see here instead](/annotorious/api-docs/osd-plugin).

## Initialization

When included via `<script>` tag: 

```javascript
var config = {
  image: document.getElementById('my-image'),
  readOnly: true
};

var anno = Annotorious.init(config);
```

With npm:

```javascript
import { Annotorious } from '@recogito/annotorious';
import '@recogito/annotorious/dist/annotorious.min.css';

const config = {
  image: document.getElementById('my-image')
}

const anno = new Annotorious(config);
```

The config object supports the following properties:

| Property            | Type           | Default | Description                                                                                                                |
| ------------------- | -------------- | ------- | -------------------------------------------------------------------------------------------------------------------------- |
| `allowEmpty`        | Boolean        | false   | Annotations created without bodies are normally discarded. Set to `true` to allow empty annotations.                       |
| `crosshair`         | Boolean        | false   | When the mouse is over the image, show a crosshair instead of the mouse cursor                                             |
| `disableEditor`     | Boolean        | false   | Disable the editor if you only need drawing functionality, but not the popup.                                              |
| `disableSelect`     | Boolean        | false   | Disables selection functionality. Clicking will no longer open the editor. (The `clickAnnotation` event still fires.)      |
| `drawOnSingleClick` | Boolean        | false   | If `true` users can start drawing with a single click also, not just with click-and-drag                                             |
| `formatters`        | Array \| Function | -       | A [formatter function](#formatters) or list of functions providing custom style rules.                                                          |
| `fragmentUnit`      | String         | 'pixel' | Store rectangle coordinates in `pixel` units (default) or `percent` units.                                                 | 
| `handleRadius`      | Number         | 6       | Radius of the shape resize handles.                                                                                        |
| `image`             | Elem \| String | -       | __Required.__ Image DOM element or element ID.                                                                             |
| `locale`            | String         | -       | Two-character ISO language code or `auto` to use the browser setting.                                                      |
| `messages`          | Object         | -       | Custom UI labels. Requires a [message dictionary](https://annotorious.github.io/guides/contributing-ui-translations/) object. |
| `readOnly`          | Boolean        | false   | Display annotations in read-only mode.                                                                                     |
| `widgets`           | Array          | -       | A list of editor widget definitions (see [this guide](/guides/configuring-the-editor/) for details).                       |

## Instance Fields

### disableEditor

```js
console.log(anno.disableEditor); // true or fals
anno.disableEditor = !anno.disableEditor; // toggles state
```

Change the operation mode between __normal__ (drawing tools & editor popup) and __headless__. In 
headless mode, only drawing tools and lifecycle events are active. The editor will not open.

### disableSelect

```js
console.log(anno.disableSelect); // true or false
anno.disableSelect = !anno.disableSelect;
```

Disables selection functionality. Clicking an annotation will no longer open the editor, or fire
the `selectAnnotation` event. The `clickAnnotation` event will still fire!

Setting `disableSelect` to `true` will __not__ clear the current selection, if any.

### formatters

```js
anno.formatters = [ ...anno.formatters, MyFormatter ];
```

The [formatter](#formatters) functions on this Annotorious instance.

### readOnly

```js
console.log(anno.readOnly); // true or fals
anno.readOnly = !anno.readOnly; // toggles state
```

Change display mode between __normal__ (annotations are editable) and __read-only__. 

### widgets

```js
anno.widgets = [...anno.widgets, MyCustomWidget ];
```

Dynamically changes the current set of editor widgets.

## Instance Methods

### .addAnnotation

```js
anno.addAnnotation(annotation [, readOnly]);
```

Adds an annotation programmatically. The format is the 
[W3C WebAnnotation model](/annotorious/getting-started/web-annotation).

| Argument     | Type    | Value                                                                    |
| ------------ | ------- | ------------------------------------------------------------------------ |
| `annotation` | Object  | the annotation according to the W3C WebAnnotation format                 |
| `readOnly`   | Boolean | set the second arg to `true` to display the annotation in read-only mode |

### .addDrawingTool

```js
anno.addDrawingTool(plugin);
```

Register a drawing tool plugin.

### .cancelSelected

```js
anno.cancelSelected();
```

Programmatically cancel the current selection, if any.

__Note:__ programmatic cancel __will not__ trigger the [cancelSelected event](#cancelselected-1)!

### .clearAnnotations

```js
anno.clearAnnotations();
```

Delete all annotations from the image.


### .clearAuthInfo

```js
anno.clearAuthInfo();
```

Clears the user auth information. Annotorious will no longer insert creator data
when a new annotation is created or updated. (See [setAuthInfo](#setauthinfo) for 
more information about creator data.)

### .destroy

```js
anno.destroy();
```

Destroys this instance of Annotorious, removing the annotation layer on the image.

### .getAnnotationById

```js
const annotation = getAnnotationById(id);
```

Returns the annotation with the specified ID.

| Argument      | Type   | Value             |
| ------------- | ------ | ----------------- |
| `id`          | String | the annotation ID |

### .getAnnotations

```js
const annotations = anno.getAnnotations();
```

Returns all annotations according to the current rendered state, in W3C Web Annotation format. 

### .getImageSnippetById

```js
const { snippet, transform } = anno.getImageSnippetById(annotationId);
```

Returns an object containing:

- A DOM CANVAS element with the given annotation's image snippet
- A coordinate transform function that translates X/Y coordinates in the 
  snippet coordinate space back to the coordinate space of the full image

| Field       | Type     | Value                                                             |
| ----------- | -------- | ----------------------------------------------------------------- |
| `snippet`   | Canvas   | the image under the given annotations' bounds as a CANVAS element |
| `transform` | Function | coordinate conversion function                                    |

### .getSelected

```js
const selected = anno.getSelected();
```

Returns the currently selected annotation.

### .getSelectedImageSnippet

```js
const { snippet, transform } = anno.getSelectedImageSnippet();
```

Returns an object containing:

- A DOM CANVAS element, in the size of the current selection's bounding box, with 
  the selected image snippet 
- A coordinate transform function that translates X/Y coordinates in the snippet coordinate
  space back to the coordinate space of the full image

| Field       | Type     | Value                                                            |
| ----------- | -------- | ---------------------------------------------------------------- |
| `snippet`   | Canvas   | the image under the current selection bounds as a CANVAS element |
| `transform` | Function | coordinate conversion function                                   |

### .listDrawingTools

```js
const toolNames = anno.listDrawingTools();
```

Returns a list of the available drawing tools, including those from registered drawing
tool plugins.

### .loadAnnotations

```js
anno.loadAnnotations(url);
```

Loads annotations from a JSON URL. The method returns a promise, in 
case you want to perform an action after the annotations have loaded.

```javascript
anno.loadAnnotations(url).then(function(annotations) {
  // Do something
});
```

| Argument | Type   | Value                                    |
| -------- | ------ | ---------------------------------------- |
| `url`    | String | the URL to HTTP GET the annotations from |

### .off

```js
anno.off(event [, callback]);
```

Unsubscribe from an event. If no callback is provided,
all event handlers for this event will be unsubscribed.

| Argument   | Type     | Value                                       |
| ---------- | -------- | ------------------------------------------- |
| `event`    | String   | the name of the event                       |
| `callback` | Function | the function used when binding to the event |

### .on

```js
anno.on(event, callback);
```

Subscribe to an event. (See [Events](#events) for the list.)

| Argument   | Type     | Value                                          |
| ---------- | -------- | ---------------------------------------------- |
| `event`    | String   | the name of the event                          |
| `callback` | Function | the function to call when the event is emitted |

### .once

```js
anno.once(event, callback);
```

Subscribe to an event only __once__. (See [Events](#events) for the list.)

| Argument   | Type     | Value                                          |
| ---------- | -------- | ---------------------------------------------- |
| `event`    | String   | the name of the event                          |
| `callback` | Function | the function to call when the event is emitted |

### .removeAnnotation

```js
anno.removeAnnotation(arg);
```

Removes an annotation programmatically. __Note:__ programmatic remove __will not__ trigger the [deleteAnnotation event](#deleteannotation)!

| Argument | Type           | Value                                                           |
| -------- | -------------- | --------------------------------------------------------------- |
| `arg`    | Object, String | the annotation in W3C WebAnnotation format or the annotation ID |

### .removeDrawingTool

```js
anno.removeDrawingTool(id);
```

Removes the drawing tool with the specified tool ID.

| Argument | Type   | Value               |
| -------- | ------ | ------------------- |
| `id`     | String | the drawing tool ID |

### .saveSelected

```js
anno.saveSelected();
```

Saves the current selection. This is essentially a programmatic way to hit the __Ok__ button on 
the editor.


### .selectAnnotation

```js
anno.selectAnnotation(arg);
```

Selects an annotation programmatically, highlighting its shape, and opening the editor popup. 
__Note:__ programmatic select __will not__ trigger the [selectAnnotation event](#selectannotation-1)!

- If no argument is provided (or the annotation or ID is unknown), this method will deselect 
  the current selection, if any
- The method will return the selected annotation as a result
- Note that the the `selectAnnotation` event will __not__ fire when using this method

| Argument | Type           | Value                               |
| -------- | -------------- | ----------------------------------- |
| `arg`    | Object, String | the annotation or the annotation ID |

### .setAnnotations

```js
anno.setAnnotations(annotations);
```

Renders the list of annotations to the image, removing any previously
existing annotations.

| Argument      | Type  | Value                                            |
| ------------- | ----- | ------------------------------------------------ |
| `annotations` | Array | array of annotations in W3C WebAnnotation format |

### .setAuthInfo 

```js
var anno = Annotorious.init({ image: 'my-image' });

anno.setAuthInfo({
  id: 'http://recogito.example.com/rainer',
  displayName: 'rainer'
});
```

Specifies user authentication information. Annotorious will use this information when annotations
are created or updated, and display it in the editor popup. 

![Editor popup example](/images/setAuthInfo.png)

Set this data right after initializing Annotorious, and in case the user login status in your host
application changes. The argument to `.setAuthInfo` is an object with the following properties:

| Property      | Type   | Value                                             |
| ------------- | ------ | ------------------------------------------------- |
| `id`          | String | __REQUIRED__ the user ID, which should be a URI   |
| `displayName` | String | __REQUIRED__ the user name, for display in the UI |

Annotorious will insert this data into every new annotation body that gets created:

```javascript
{ 
  type: "Annotation",
  
  // ...  

  body:[{
    type: "TextualBody",
    value:"My comment",
    purpose: "commenting",
    created: "2020-05-18T09:39:47.582Z",
    creator: {
      id: "http://recogito.example.com/rainer",
      name: "rainer"
    }
  }]
}
```

### .setDrawingTool

```js
anno.setDrawingTool(toolName);
```

Switches between the different available drawing tools. Per default, Annotorious
provides a standard rectangle tool (`rect`) and a standard polygon drawing tool
(`polygon`).

More tools may be available through plugins. Use [.listDrawingTool](#listdrawingtools)
to get the list of registered tools.

| Argument   | Type   | Value                    |
| ---------- | ------ | ------------------------ |
| `toolName` | String | E.g. `rect` or `polygon` |

### .setServerTime 

Set a "server time" timestamp. When using [authInfo](#setauthinfo), this method helps to synchronize the
`created` timestamp that Annotorious inserts into the annotation with your server environment, avoiding 
problems when the clock isn't properly set in the user's browser.

After setting server time, the Annotorious will adjust the `created` timestamps by the difference between
server time the user's local clock.

### .setVisible

```js
anno.setVisible(visible);
```

Shows or hides the annotation layer.

| Argument  | Type    | Value                                                  |
| --------- | ------- | ------------------------------------------------------ |
| `visible` | Boolean | if `true` show the annotation layer, otherwise hide it |

### .updateSelected

```js
anno.updateSelected(annotation[, saveImmediately]);
```

Replace the currently selected annotation with the one given as argument to .updateSelected.
Optionally: save the annotation immediately, triggering a `createAnnotation` or 
`updateAnnotation` event.

> __Important:__ this method returns a Promise. The update will __not__ be effective
> immediately. Make sure you wait until the Promise completes before saving the annotation.

Use this method for applications that  (with `disableEditor` set 
to `true`).

```js
var anno = Annotorious.init({
  image: 'my-image'
  disableEditor: true
});

anno.on('createSelection', async function(selection) {
  selection.body = [{
    type: 'TextualBody',
    purpose: 'tagging',
    value: 'MyTag'
  }];

  // Make sure to wait before saving!
  await anno.updateSelected(selection);
  anno.saveSelected();

  // Or: anno.updateSelected(selection, true);
});
```

## Events

### cancelSelected

```js
anno.on('cancelSelected', function(selection) {
  // 
});
```

Fired when the user has canceled a selection, by hitting __Cancel__ in the editor, or by
clicking or tapping outside the selected annotation shape.

| Argument    | Type   | Value                                              |
| ----------- | ------ | -------------------------------------------------- |
| `selection` | Object | the canceled selection in W3C WebAnnotation format |

### changeSelected

```js
anno.on('changeSelected', function(selected, previous) {
  // 
});
```

Fired when the user changed the selection from one annotation to another. Essentially a shorthand.
`cancelSelected` and `selectAnnotation` will fire in addition.

| Argument   | Type   | Value                        |
| ---------- | ------ | ---------------------------- |
| `selected` | Object | the selected annotation      |
| `previous` | Object | the annotation that was selected before |

### changeSelectionTarget

```js
anno.on('changeSelectionTarget', function(target) {
  // 
});
```

Fired when the shape of a newly created selection, or of a selected annotation 
is moved or resized.

| Argument | Type   | Value                        |
| -------- | ------ | ---------------------------- |
| `target` | Object | the W3C WebAnnotation target |

### clickAnnotation

```js
anno.on('clickAnnotation', function(annotation, element) {
  // 
});
```

Fired every time the user clicks an annotation (regardless of whether it is already
selected or not).

| Argument     | Type    | Value                                      |
| ------------ | ------- | ------------------------------------------ |
| `annotation` | Object  | the clicked annotation                     |
| `element`    | Element | the clicked annotation SVG shape element   |

### createAnnotation

```js
anno.on('createAnnotation', function(annotation, overrideId) {
  // 
});
```

Fired when a new annotation is created from a user selection.

| Argument     | Type     | Value                                      |
| ------------ | -------- | ------------------------------------------ |
| `annotation` | Object   | the annotation in W3C WebAnnotation format |
| `overrideId` | Function | a callback function for assigning a custom annotation ID |

When an annotation is created, Annotorious automatically assigns it a globally unique 
ID. It is possible to override this ID with your own (e.g. server-generated) ID via 
the overrideId callback function.

```js
r.on('createAnnotation', async (annotation, overrideId) => {
  // POST to the server and receive a new ID
  const newId = await server.createAnnotation(annotation);

  // Inject that ID into RecogitoJS
  overrideId(newId);
});
```

### createSelection

```js
anno.on('createSelection', function(selection) {
  // 
});
```

Fired when a new selection shape is drawn on the image.

| Argument    | Type   | Value                                            |
| ----------- | ------ | ------------------------------------------------ |
| `selection` | Object | the selection object in W3C WebAnnotation format |

### deleteAnnotation

```js
anno.on('deleteAnnotation', function(annotation) {
  // 
});
```

Fired when an existing annotation was deleted.

| Argument     | Type   | Value                  |
| ------------ | ------ | ---------------------- |
| `annotation` | Object | the deleted annotation |

### mouseEnterAnnotation

```js
anno.on('mouseEnterAnnotation', function(annotation, element) {
  // 
});
```

Fired when the mouse moves into an existing annotation.

| Argument     | Type   | Value                    |
| ------------ | ------ | ------------------------ |
| `annotation` | Object | the annotation           |
| `element`    | Object | the annotation shape SVG element |

### mouseLeaveAnnotation

```js
anno.on('mouseLeaveAnnotation', function(annotation, element) {
  // 
});
```

Fired when the mouse moves out of an existing annotation.

| Argument     | Type   | Value                    |
| ------------ | ------ | ------------------------ |
| `annotation` | Object | the annotation           |
| `element`    | Object | the annotation shape SVG element |

### selectAnnotation

```js
anno.on('selectAnnotation', function(annotation, element) {
  // 
});
```

Fired when the user selects an annotation. Note that this event will __not__ fire when 
the selection is made programmatically through the `selectAnnotation(arg)` API method.

| Argument     | Type    | Value                                      |
| ------------ | ------- | ------------------------------------------ |
| `annotation` | Object  | the annotation in W3C WebAnnotation format |
| `element`    | Element | the annotation SVG shape element           |

### startSelection

```js
anno.on('startSelection', function(point) {
  console.log(point.x, point.y)
});
```

Fired when the user starts drawing a new shape.

| Argument | Type   | Value                                                            |
| -------- | ------ | ---------------------------------------------------------------- |
| `point`  | Object | the x/y coordinates of the start point (image pixel coordinates) |


### updateAnnotation

```js
anno.on('updateAnnotation', function(annotation, previous) {
  // 
});
```

Fired when an existing annotation was updated.

| Argument     | Type   | Value                                  |
| ------------ | ------ | -------------------------------------- |
| `annotation` | Object | the updated annotation                 |
| `previous`   | Object | the annotation state before the update |

## Formatters

Per default, Annotorious renders annotations as SVG [group](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/g)
elements with an `a9s-annotation` CSS class. A formatter function allows you to dynamically add additional 
attributes and elements to the SVG annotation shape.

A formatter is a JavaScript function takes a single argument - the annotation - and must return a __string__, a
__DOM element__ or an __object__. 

- If a string is returned, it will be appended to the annotation element CSS class list. 
- If an object is returned, it should have one or more of the following properties:
  - `className` a string to be added to the CSS class list
  - `element` a DOM element to be appended to the SVG group
  - `data-*` a data attribute to add to the annotation SVG element
  - `style` a list of CSS styles (in the form of a string) 

#### Element Caveats

When using DOM elements, keep in mind that you should specify at least the x,y positions and the width and height attributes.  
The x and y positions are calculated from the upper left when using the box tool. If using SVG elements then make sure to use 
the appropriate fill/stroke attributes.  

### String Example

```js
/** 
 * This simple formatter will add an extra 'long' CSS class to 
 * long comments
 */
var formatter = function(annotation) {

  var longComments = annotation.bodies.filter(function(body) {
    var isComment = body.type === 'TextualBody' && 
      (body.purpose === 'commenting' || body.purpose === 'replying');

    var isLong = body.value.length > 100;

    return isComment && isLong;
  });

  if (longComments.length > 0) {
    // This annotation contains long comments - add CSS class
    return 'long';
  }
}

var anno = Annotorious.init({
  image: document.getElementyById('my-image'),
  locale: 'auto',
  formatter: formatter
});
```

You can then apply a visual style to long comments in your own CSS stylesheet.

```css
.a9s-annotation.long {
  stroke-width:4;
  stroke:red;
}
```

### Object Example

```js
/**
 * This formatter will add usernames from the annotation as a data
 * attribute and apply an inline style (red outline) where multiple 
 * users have annotated
 */
var formatter = function(annotation) {

  var contributors = [];

  annotation.bodies.forEach(function(body) {
    if (body.creator)
      contributors.push(body.creator.id);
  });

  if (contributors.length > 1) {
    return {
      'data-users': contributors.join(', '),
      'style': 'stroke-width:2; stroke: red'
    }
  }
}

var anno = Annotorious.init({
  image: document.getElementyById('my-image'),
  locale: 'auto',
  formatter: formatter
});
```

