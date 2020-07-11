---
title: "API Docs | Annotorious"
date: 2020-05-17T14:14:36+02:00
draft: false
layout: "api-doc"
meta_title: "Annotorious API Docs"
meta_description: "API Documentation for the Annotorious image annotation library"
meta_link: "https://recogito.github.io/annotorious/api-docs/annotorious"
---

# API Reference: Annotorious

> The __standard version__ of Annotorious works on normal images embedded in websites
> or web applications. The API reference for the Annotorious OpenSeadragon plugin is 
> available [here](/annotorious/api-docs/osd-plugin).

## Initializing Annotorious

Initialize an Annotorious instance on an image with 

```javascript
var config = {
  image: document.getElementById('my-image'),
  readOnly: true
};

var anno = Annotorious.init(config);
```

The config object can have the following properties:

| Property    | Type | Value                                                                               | Default |
|-------------|------|-------------------------------------------------------------------------------------|---------|
| `image`     | `Element`, `String` | Image element or, alternatively, element ID                          | -       |
| `readOnly`  | `Boolean` | Set to `true` to display all annotations read-only                             | `false` |
| `headless`  | `Boolean` | Completely disables the editor popup. Drawing is still possible, and lifecycle events still fire. Can be used in conjunction with [applyTemplate](#applytemplate). Note that headless mode currently supports only shape creation, not editing. | `false`    |
| `tagVocabulary` | `Array` | A list of strings to use as a pre-defined tag vocabulary in the tagging widget | - |

## Instance Methods

### .addAnnotation

```js
anno.addAnnotation(annotation [, readOnly]);
```

Adds an annotation programmatically. The format is the 
[W3C WebAnnotation model](/annotorious/getting-started/web-annotation). At the moment, 
only a single `FragmentSelector` with an `xywh=pixel` fragment is supported.

| Argument     | Type    | Value                                                       |
|--------------|---------|-------------------------------------------------------------|
| `annotation` | Object  | the annotation according to the W3C WebAnnotation format    |
| `readOnly`   | Boolean | set the second arg to `true` to display the annotation in read-only mode |

### .applyTemplate

```js
anno.applyTemplate(template);
```

When a new annotation is created, Annotorious will automatically apply the given 
template. The template can be a single annotation body, or an array of bodies.
If `openEditor` is set to true, the editor popup will open, allowing the user to 
edit the annotation normally. Otherwise, Annotorious will __instantly__ create an 
annotation from the template bodies __only__. The editor will not open. 

Use this method if you want to implement batch- or fast annotation modes, where
the same annotation should be applied repeatedly, and the user should only take
care of drawing selections.

| Argument   | Type | Value                                    |
|------------|------|------------------------------------|
| `template` | Object | the template body or list of bodies to apply automatically |
| `openEditor` | Boolean | if `true`, the editor will open after the template is applied |

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

### .getAnnotations

```js
anno.getAnnotations();
```

Returns all annotations according to the current rendered state, in W3C Web Annotation format. 

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

| Argument  | Type | Value                                    |
|-----------|------|------------------------------------|
| `url`     | String | the URL to HTTP GET the annotations from |

### .off

```js
anno.off(event [, callback]);
```

Unsubscribe from an event. If no callback is provided,
all event handlers for this event will be unsubscribed.

| Argument   | Type | Value                                       |
|------------|------|---------------------------------------|
| `event`    | String | the name of the event                       |
| `callback` | Function |  the function used when binding to the event |

### .on

```js
anno.on(event, callback);
```

Subscribe to an event. (See [Events](#events) for the list.)

| Argument   | Type | Value                                          |
|------------|------|------------------------------------------|
| `event`    | String | the name of the event                          |
| `callback` | Function |the function to call when the event is emitted |

### .removeAnnotation

```js
anno.removeAnnotation(arg);
```

Removes an annotation programmatically. 

| Argument     | Type           | Value                                                           |
|--------------|----------------|-----------------------------------------------------------------|
| `arg`        | Object, String | the annotation in W3C WebAnnotation format or the annotation ID |

### .selectAnnotation

```js
anno.selectAnnotation(arg);
```

Selects an annotation programmatically, highlighting its shape, and opening the editor popup. 

- If no argument is provided (or the annotation or ID is unknown), this method will deselect 
  the current selection, if any
- The method will return the selected annotation as a result
- Note that the the `selectAnnotation` event will __not__ fire when using this method

| Argument  | Type | Value                                    |
|-----------|------|------------------------------------|
| `arg` | Object, String | the annotation or the annotation ID |

### .setAnnotations

```js
anno.setAnnotations(annotations);
```

Renders the list of annotations to the image, removing any previously
existing annotations.

| Argument      | Type | Value                                         |
|---------------|------|-----------------------------------------|
| `annotations` | Array | array of annotations in W3C WebAnnotation format |

### .setAnnotationsVisible

```js
anno.setAnnotationsVisible(visible);
```

Shows or hides the annotation layer.

| Argument  | Type | Value                                    |
|-----------|------|------------------------------------|
| `visible` | Boolean | if `true` show the annotation layer, otherwise hide it |

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

| Property      | Type | Value                                             |
|---------------|------|---------------------------------------------|
| `id`          | String | __REQUIRED__ the user ID, which should be a URI  |
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

> This feature is experimental. The API will likely change in the future.

Switches between the different available drawing tools. At the moment, only
the built-in rubberband rectangle and polygon tools are available. 

| Argument   | Type | Value                                         |
|------------|------|-----------------------------------------|
| `toolName` | String | Either `rect` or `polygon` |

### .setServerTime 

Set a "server time" timestamp. When using [authInfo](#setauthinfo), this method helps to synchronize the
`created` timestamp that Annotorious inserts into the annotation with your server environment, avoiding 
problems when the clock isn't properly set in the user's browser.

After setting server time, the Annotorious will adjust the `created` timestamps by the difference between
server time the user's local clock.

## Events

### createAnnotation

```js
anno.on('createAnnotation', function(annotation) {
  // 
});
```

Fired when a new annotation is created from a user selection.

| Argument     | Type | Value                                      |
|--------------|------|--------------------------------------|
| `annotation` | Object | the annotation in W3C WebAnnotation format |

### deleteAnnotation

```js
anno.on('deleteAnnotation', function(annotation) {
  // 
});
```

Fired when an existing annotation was deleted.

| Argument     | Type | Value                                  |
|--------------|-------|---------------------------------|
| `annotation` | Object | the deleted annotation                 |

### mouseEnterAnnotation

```js
anno.on('mouseEnterAnnotation', function(annotation, event) {
  // 
});
```

Fired when the mouse moves into an existing annotation.

| Argument     | Type | Value                                  |
|--------------|------|----------------------------------|
| `annotation` | Object | the annotation                         |
| `event`      | Object | the original mouse event               |

### mouseLeaveAnnotation

```js
anno.on('mouseLeaveAnnotation', function(annotation, event) {
  // 
});
```

Fired when the mouse moves out of an existing annotation.

| Argument     | Type | Value                                  |
|--------------|------|----------------------------------|
| `annotation` | Object | the annotation                         |
| `event`      | Object | the original mouse event               |

### selectAnnotation

```js
anno.on('selectAnnotation', function(annotation) {
  // 
});
```

Fired when the user selects an annotation. Note that this event will __not__ fire when 
the selection is made programmatically through the `selectAnnotation(arg)` API method.

| Argument     | Type | Value                                      |
|--------------|------|--------------------------------------|
| `annotation` | Object | the annotation in W3C WebAnnotation format |

### updateAnnotation

```js
anno.on('updateAnnotation', function(annotation, previous) {
  // 
});
```

Fired when an existing annotation was updated.

| Argument     | Type | Value                                  |
|--------------|------|----------------------------------|
| `annotation` | Object | the updated annotation                 |
| `previous`   | Object | the annotation state before the update |



