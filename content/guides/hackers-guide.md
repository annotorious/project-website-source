---
title: "Hacker's Guide"
date: 2020-06-04T11:46:22+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "We welcome contributions! Both to our code as well as our documentation. This guide explains how to set up your development environment for hacking Annotorious or RecogitoJS."
weight: 99
meta_title: "Annotorious/RecogitoJS Development Setup"
meta_description: "This guide explains how to set up your development environment for hacking Annotorious or RecogitoJS."
meta_link: "https://recogito.github.io/guides/hackers-guide"
---

# Hacker's Guide to Annotorious and RecogitoJS

We welcome pull requests to Annotorious and RecogitoJS - both to the code, as well as to our documentation! To contribute,
simply fork the relevant repository and hack away. Our code is located in the following repositories

- __[recogito/annotorious](https://github.com/recogito/annotorious)__. Annotorious application entry point, SVG rendering
  and drawing tool base classes.
- __[recogito/annotorious-openseadragon](https://github.com/recogito/annotorious-openseadragon)__. The OpenSeadragon plugin.
  Imports most functionality from annotorious and recogito-client-core.
- __[recogito/recogito-client-core](https://github.com/recogito/recogito-client-core)__. A base module that contains shared
  code for Annotorious and RecogitoJS, most importantly the code for the editor popup. 
- __[recogito/recogito-js](https://github.com/recogito/recogito-js)__. RecogitoJS application entry point, text annotation
  functionality.

## Running in Development Mode

To hack on __Annotorious__, the __OpenSeadragon plugin__ or __RecogitoJS__, you need to run them in development mode.

- Clone the repository
- Run `npm install` to download project dependencies
- Run `npm start` to launch the project in dev mode, with hot-reloading enabled
- If your browser doesn't open automatically, go to <http://localhost:3000/>

## Building a distribution bundle

- Run `npm run build` to build a distribution bundle
- The distribution files will be in the `dist` folder

## Hacking recogito-client-core

> When working on __Annotorious__, the __OpenSeadragon plugin__ or __RecogitoJS__, you may need to modify code in 
> __recogito-client-core__, too. To do this, you have to set up your environment so that it points to your local
> copy of __recogito-client-core__, rather than the official package published on NPM. To make this work, you 
> have to `npm link` your local projects.

- Clone the [recogito-client-core repository](https://github.com/recogito/recogito-client-core)
- Run `npm install` to install dependencies
- Link your local clone of __recogito-client-core__, so that changes you make to
  recogito-client-core are reflected in your RecogitoJS/Annotorious project

```shell
$ cd recogito-client-core
$ npm install 
$ npm link # Creates a global symlink
$ cd ..

$ cd annotorious
$ npm install
$ npm link @recogito/recogito-client-core # Links to the global symlink

# Annotorious now uses your local clone of recogito-client-core
# instead of the latest published NPM package
$ npm start

# Same procedure for RecogitoJS if needed
```

