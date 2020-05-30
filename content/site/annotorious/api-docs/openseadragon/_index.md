---
title: "Openseadragon"
date: 2020-05-17T14:15:19+02:00
draft: false
---

## Initializing OpenSeadragon Annotorious

Initialize Annotorious on an OpenSeadragon viewer instance with 

```javascript
var anno = OpenSeadragon.Annotorious(viewer, config);
```

The `config` is optional, and must be an object with the following properties:

| Property    | Value                                                                               | Default |
|-------------|-------------------------------------------------------------------------------------|---------|
| `readOnly`  | Set to `true` to display annotations read-only            | `false`    |

<br/><br/>

## Instance Methods

### `addAnnotation(annotation)`

Adds an annotation programmatically. The format is the 
[W3C WebAnnotation model](https://github.com/recogito/annotorious-openseadragon/wiki/Web-Annotation-Model). 
At the moment, only a single `FragmentSelector` with an `xywh=pixel` fragment is supported.

| Argument     | Value                                         |
|--------------|-----------------------------------------------|
| `annotation` | the annotation in W3C WebAnnotation format    |

<br/>

### `removeAnnotation(arg)`

Removes an annotation programmatically. 

| Argument     | Value                                         |
|--------------|-----------------------------------------------|
| `arg` | the annotation in W3C WebAnnotation format or the annotation ID |

<br/>

### `setAnnotations(annotations)`

Renders the list of annotations to the image, removing any previously
existing annotations.

| Argument      | Value                                         |
|---------------|-----------------------------------------------|
| `annotations` | array of annotations in W3C WebAnnotation format |


<br/>

### `loadAnnotations(url)`

Loads annotations from a JSON file. The method returns a promise, in 
case you want to perform an action after the annotations have loaded.

```javascript
anno.loadAnnotations(url).then(function(annotations) {
  // Do something
});
```

| Argument  | Value                                    |
|-----------|------------------------------------------|
| `url`     | the URL to HTTP GET the annotations from |

<br/>

### `getAnnotations()`

Returns all annotations according to the current rendered state, in W3C Web Annotation format. 

<br/>

### `selectAnnotation(arg)`

Selects an annotation programmatically, highlighting its shape, and opening the editor popup. 

- If no argument is provided (or the annotation or ID is unknown), this method will deselect 
  the current selection, if any
- The method will return the selected annotation as a result
- Note that the the `selectAnnotation` event will __not__ fire when using this method

| Argument  | Value                                    |
|-----------|------------------------------------------|
| `arg` | the annotation or the annotation ID |

<br/>

### `panTo(arg, immediately)`

Pans the OpenSeadragon viewport to the specified annotation.

| Argument  | Value                                    |
|-----------|------------------------------------------|
| `arg` | the annotation or the annotation ID |
| `immediately` | if `true` pans without animation |

<br/>

### `fitBounds(arg, immediately)`

Makes the OpenSeadragon viewport pan and zoom to the bounds of the specified annotation.

| Argument  | Value                                    |
|-----------|------------------------------------------|
| `arg` | the annotation or the annotation ID |
| `immediately` | if `true` pans and zooms without animation |

<br/>

### `destroy()`

Destroys this instance of Annotorious, removing the annotation layer on the image.

<br/>

### `on(event, callback)` 

Subscribe to an event. (See [Events](#events) for the list.)

| Argument   | Value                                          |
|------------|------------------------------------------------|
| `event`    | the name of the event                          |
| `callback` | the function to call when the event is emitted |

<br/>

### `off(event[, callback])`

Unsubscribe from an event. If no callback is provided,
all event handlers for this event will be unsubscribed.

| Argument   | Value                                       |
|------------|---------------------------------------------|
| `event`    | the name of the event                       |
| `callback` | the function used when binding to the event |

<br/><br/>

## Events

### `selectAnnotation(annotation)` 

Fired when the user selects an annotation. Note that this event will __not__ fire when 
the selection is made programmatically through the `selectAnnotation(arg)` API method.


| Argument     | Value                                      |
|--------------|--------------------------------------------|
| `annotation` | the annotation in W3C WebAnnotation format |

<br/>

### `createAnnotation(annotation)` 

Fired when a new annotation is created from a user selection.

| Argument     | Value                                      |
|--------------|--------------------------------------------|
| `annotation` | the annotation in W3C WebAnnotation format |

<br/>

### `updateAnnotation(annotation, previous)`

Fired when an existing annotation was updated.

| Argument     | Value                                  |
|--------------|----------------------------------------|
| `annotation` | the updated annotation                 |
| `previous`   | the annotation state before the update |

<br/>

### `deleteAnnotation(annotation)`

Fired when an existing annotation was deleted.

| Argument     | Value                                  |
|--------------|----------------------------------------|
| `annotation` | the deleted annotation                 |

<br/>

### `mouseEnterAnnotation(annotation, event)`

Fired when the mouse moves into an existing annotation.

| Argument     | Value                                  |
|--------------|----------------------------------------|
| `annotation` | the annotation                         |
| `event`      | the original mouse event               |

<br/>

### `mouseLeaveAnnotation(annotation, event)`

Fired when the mouse moves out of an existing annotation.

| Argument     | Value                                  |
|--------------|----------------------------------------|
| `annotation` | the annotation                         |
| `event`      | the original mouse event               |

<br/>
