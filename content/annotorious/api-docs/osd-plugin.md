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