---
title: "Hacker's Guide"
date: 2020-06-04T11:46:22+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "We welcome contributions to Annotorious and RecogitoJS. This work-in-progress guide aims to explain where to find what in the codebase, and how you can set up your development environment."
weight: 9
---

When working on __RecogitoJS__ or __Annotorious__, you may sometimes need to modify code in 
__recogito-client-core__. To make this work, you have to `npm link` your local projects.

- Clone the [RecogitoJS](https://github.com/recogito/recogito-js) and/or 
  [Annotorious](https://github.com/recogito/annotorious) repositories
- Clone the [recogito-client-core repository](https://github.com/recogito/recogito-client-core)
- Run `npm install` in all repositories to install dependencies
- Link your local clone of __recogito-client-core__, so that changes you make to
  recogito-client-core are reflected in RecogitoJS/Annotorious

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

