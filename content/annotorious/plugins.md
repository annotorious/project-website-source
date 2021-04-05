---
title: "Plugins"
date: 2021-04-05T10:09:57+02:00
draft: false
layout: "single"
include_summaries: false
meta_title: "Annotorious Plugins and Extensions"
meta_description: "Plugins and Extensions for the Annotorious image annotation library"
meta_link: "https://recogito.github.io/annotorious/plugins"
---

# Plugins

Plugins extend the functionality of Annotorious. The following plugins are currently
available:

## [Firebase Storage](https://github.com/recogito/recogito-client-plugins/tree/main/packages/storage-firebase)

A storage plugin that uses Google Firebase as a cloud annotation store.

## [Tensorflow Tag Suggestions](https://github.com/recogito/recogito-client-plugins/tree/main/packages/annotorious-tensorflow-tag-suggestions)

A plugin that uses [TensorflowJS](https://www.tensorflow.org/js) to provide AI-powered 
automatic tag suggestions. Tag image regions manually first. After learning from at least two 
examples, the plugin provides tag suggestions automatically.

## [Tilted Box Drawing Tool](https://github.com/recogito/recogito-client-plugins/tree/main/packages/annotorious-tilted-box)

A plugin that adds a new drawing tool to Annotorious and Annotorious OpenSeadragon:
the __Tilted Box__. Draw a rectangle with arbitrary rotation with only two clicks.

