---
title: "Example React Plugin"
date: 2021-02-20T10:28:42+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "An example Annotorious/RecogitoJS editor plugin built with React."
weight: 3
meta_title: "Example React Plugin"
meta_description: "An example Annotorious/RecogitoJS editor plugin built with React."
meta_link: "https://recogito.github.io/guides/example-react-plugin"
---

# Example React Plugin

> This guide was created by [Alaksandar Jesus Gene](https://github.com/alaksandarjesus) and
> is re-published here (with minor modifications) with his kind permission. 
> (See [original source](https://github.com/recogito/recogito-client-core/issues/51).)

## A Category & Subcategory Dropdown Plugin

This example plugin implements a category dropdown. Dropdown options can optionally depend
on a parent dropdown, to implement hierarchical tagging. E.g. selecting `Animal` from the first
dropdown will set the options `['Lion', 'Tiger']` on the second dropdown, whereas selecting
`Building` will set the options `['Villa', 'Apartment']`.

![image](https://user-images.githubusercontent.com/4712552/108470941-b74cb400-72b0-11eb-85af-e637b8960213.png)

Selections will be stored in the annotation under different `purpose` values (`category` and 
`subcategory`).

## Project Setup

[..TODO..]

## SelectWidget.jsx

```js
import React from "preact/compat";
import { useState } from "preact/hooks";

const SelectWidget = props => { 
  // All bodies stored under this widget's purpose 
  const all = props.annotation ?  
    props.annotation.bodies.filter(body => 
      (body.purpose === props.purpose)) : [];
      
  const lastBody = 
    all && all.length ? all[all.length - 1] : null;

  // If this widget is configured with a parent widget,
  // get the bodies stored under the parent purpose
  const parentAll =
    props.annotation && props.parent ? 
      props.annotation.bodies.filter(body => 
        body.purpose === props.parent) : [];

  const lastParentBody =
    parentAll && parentAll.length ? parentAll[parentAll.length - 1] : null;

  const onChange = (event) => {
    props.onAppendBody({ value:event.target.value, purpose:props.purpose });
  }

  const isSelected = (option) => {
    if (lastAnnotation) {
      return lastAnnotation.value === option?'selected':'';
    }

    if (props.defaultValue) {
      return props.defaultValue === option?'selected':'';;
    }

    return '';
  }

  const getOptionTags = options =>{
    if (!props.parent) {
      return options.map( option => {
        return <option value={option} selected={isSelected(option)}>{option}</option>
      })
    } else if (lastParentAnnotation) {
      const subcategoryOptions = options[lastParentAnnotation.value];
      return subcategoryOptions.map(option => {
          return <option value={option} selected={isSelected(option)}>{option}</option>
      })
    }
    
    return null;
  }
    

  return(
    <div className="r6o-widget r6o-select">
      <select onChange={onChange}>
        <option value="">{props.label}</option>
        {getOptionTags(props.options)}
      </select>
    </div>
  )

}

export default SelectWidget;
```

And to init use:

```js
var anno = Annotorious.init({
    image: "hallstatt",
    widgets: [{ 
      widget: "SELECT", 
      options: [ "Animal", "Building", "Waterbody"],
      purpose: "category",
      label: "Independent, but has child dropdown"
    }, {
      widget: "SELECT", 
      options: { "Animal": ["Lion","Tiger"], "Building": ["Villa","Apartment"], "Waterbody": ["Lake", "River"] }, 
      purpose:"subcategory",
      label:"Options are dependent to parent dropdown", 
      parent:"category"
    }, {
      widget: "SELECT", 
      options: [ "High", "Low", "Severe"],
      purpose:"severity", 
      label:"Independent Dropdown" 
    }]
});
```