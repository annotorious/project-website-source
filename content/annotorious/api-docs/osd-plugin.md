---
title: "API Docs | Annotorious OSD Plugin"
date: 2020-05-17T14:15:19+02:00
draft: false
layout: "api-doc"
meta_title: "Annotorious OpenSeadragon API"
meta_description: "API Documentation for the Annotorious OpenSeadragon plugin"
meta_link: "https://recogito.github.io/annotorious/api-docs/osd-plugin"
---

# API Reference: OpenSeadragon Plugin

> The __OpenSeadragon Plugin__ is an extension to the [OpenSeadragon](http://openseadragon.github.io/),
> zoomable image viewer. The API reference for the Annotorious standard version is available 
> [here](/annotorious/api-docs/annotorious).

## Initializing the Plugin

Initialize Annotorious on an OpenSeadragon viewer instance with 

```javascript
var anno = OpenSeadragon.Annotorious(viewer, config);
```

The `config` is optional, and must be an object with the following properties:

| Property    | Type | Value                                                                       | Default |
|-------------|------|-------------------------------------------------------------------------------|---------|
| `readOnly`  | Boolean | Set to `true` to display annotations read-only            | `false`    |
| `tagVocabulary` | `Array` | A list of strings to use as a pre-defined tag vocabulary in the tagging widget | - |

## Instance Methods

### .addAnnotation

```js
anno.addAnnotation(annotation);
```

Adds an annotation programmatically. The format is the 
[W3C WebAnnotation model](https://github.com/recogito/annotorious-openseadragon/wiki/Web-Annotation-Model). 
At the moment, only a single `FragmentSelector` with an `xywh=pixel` fragment is supported.

| Argument     | Type | Value                                         |
|--------------|------|-----------------------------------------|
| `annotation` | Object | the annotation in W3C WebAnnotation format    |

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

### .fitBounds

```js
anno.fitBounds(arg [, immediately]);
```

Makes the OpenSeadragon viewport pan and zoom to the bounds of the specified annotation.

| Argument  | Type | Value                                    |
|-----------|------|------------------------------------|
| `arg` | String, Object | the annotation or the annotation ID |
| `immediately` | Boolean | if `true` pans and zooms without animation |

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
| `callback` | Function | the function used when binding to the event |

### .on

```js
anno.on(event, callback);
```

Subscribe to an event. (See [Events](#events) for the list.)

| Argument   | Type | Value                                          |
|------------|------|------------------------------------------|
| `event`    | String | the name of the event                          |
| `callback` | Function | the function to call when the event is emitted |

### .panTo

```js
anno.panTo(arg [, immediately]);
```

Pans the OpenSeadragon viewport to the specified annotation.

| Argument  | Type | Value                                    |
|-----------|------|------------------------------------|
| `arg` | String, Object | the annotation or the annotation ID |
| `immediately` | Boolean | if `true` pans without animation |

### .removeAnnotation

```js
anno.removeAnnotation(arg);
```

Removes an annotation programmatically. 

| Argument     | Type | Value                                         |
|--------------|------|-----------------------------------------|
| `arg` | String, Object | the annotation in W3C WebAnnotation format or the annotation ID |

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
| `arg` | String, Object | the annotation or the annotation ID |

### .setAnnotations

```js
anno.setAnnotations(annotations);
```

Renders the list of annotations to the image, removing any previously
existing annotations.

| Argument      | Type | Value                                         |
|---------------|------|-----------------------------------------|
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

### .setDrawingEnabled

```js
anno.setDrawingEnabled(enabled);
```

Switches the OpenSeadragon viewer from its normal zoom & pan behavior to 
"drawing mode". That means the user no longer needs to hold __SHIFT__ in 
order to start drawing. Instead, drawing starts immediately when clicking and
holding the mouse button.

After the annotation is created or canceled, OpenSeadragon automatically 
switches back to normal zoom & pan behavior. 

| Argument      | Type | Value                                         |
|---------------|------|-----------------------------------------|
| `enabled` | Boolean | if `true` OpenSeadragon switches from navigation to drawing mode |

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

### cancelSelection

```js
anno.on('cancelSelection', function(selection) {
  // 
});
```

Fired when the creates a selection and then aborts by clicking cancel.

| Argument     | Type | Value                                      |
|--------------|------|--------------------------------------|
| `selection` | Object | the canceled selection in W3C WebAnnotation format |

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
|--------------|------|----------------------------------|
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

Fired when the user selects an annotation. Note that this event will __not__ fire when 
the selection is made programmatically through the `selectAnnotation(arg)` API method.

```js
anno.on('selectAnnotation', function(annotation) {
  // 
});
```

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