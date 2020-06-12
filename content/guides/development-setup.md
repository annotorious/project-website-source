---
title: "Hacker's Guide"
date: 2020-06-04T11:46:22+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "We welcome contributions! Both to our code as well as our documentation. This (work-in-progress) guide aims to explain how to contribute, where to find what in the codebase, and how to set up a development environment for hacking on Annotorious or RecogitoJS."
weight: 9
meta_title: "Development Setup"
meta_description: "This guide explains how to contribute to Annotorious & RecogitoJS, where to find what in the codebase, and how to set up a development environment"
meta_link: "https://recogito.github.io/guides/development-setup"
---

# Hacker's Guide to Annotorious and RecogitoJS

We welcome pull requests to Annotorious and RecogitoJS - both to the code, as well as to our documentation! To contribute,
simply fork the relevant repository and hack away.

## Repositories 

...

## Developing

- Clone the repository
- Run `npm install` to download project dependencies
- Run `npm start` to launch the project in dev mode, with hot-reloading enabled
- Go to <http://localhost:3000/>

## Building a distribution bundle

- Run `npm run build` to build a distribution bundle
- The distribution files will be in the `dist` folder

Want to find your way around in the source code? Read our [Guided Tour of the Codebase](https://github.com/recogito/annotorious/wiki/Guided-Tour).  


To contribute to Annotorious, simply fork [our repository](https://github.com/recogito/annotorious) and hack away. We welcome pull requests - both for our code, as well as for our documentation!

## Developing

- Clone the repository
- Run `npm install` to download project dependencies
- Run `npm start` to launch the project in dev mode, with hot-reloading enabled
- Go to <http://localhost:3000/>

## Building a distribution bundle

- Run `npm run build` to build a distribution bundle
- The distribution files will be in the `dist` folder

Want to find your way around in the source code? Read our [Guided Tour of the Codebase](https://github.com/recogito/annotorious/wiki/Guided-Tour).  

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

