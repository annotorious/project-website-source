---
title: "Customizing Appearance"
date: 2020-09-24T10:47:22+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "This guide explains how you can customize the visual appearance of annotations, and apply your own style rules by implementing a Formatter function."
weight: 2
meta_title: "Customizing the visual appearance of Annotorious"
meta_description: "This guide explains how you can customize the visual appearance of annotations, and apply your own style rules by implementing a Formatter function."
meta_link: "https://recogito.github.io/guides/customizing-style"
---

# Customizing Visual Appearance

Need a different look and feel? Customizing the visual appearance of Annotorious is easy! 
All elements of the annotation layer and the editor window can be styled with CSS.

## Customizing the Annotation Layer

Annotorious renders annotations using [SVG](https://developer.mozilla.org/en-US/docs/Web/SVG), which
means you can completely alter the visual appearance of every graphical element via CSS.

To make styling even more flexible, Annotorious adds a few extras:

- For every annotation shape, Annotorious renders two SVG shapes, exactly on top of each other. The two shapes
  have different CSS classes - `a9s-inner` and `a9s-outer`. You can use this to create line styles with an outline.
  (The default style renders an outer black line and a slightly thinner white inner line.)
- When the user creates a new selection, Annotorious renders a mask element. In the default theme, the mask is 
  hidden. But you can enable it via CSS to add a dimming effect to the area outside the selection.

### Custom Style Example

![A custom annotation layer style](/images/guides/custom-annotationlayer-style.jpg)

This example applies custom CSS rules to enable the dim mask and give different appearance to the 
annotation outlines.

```css
svg.a9s-annotationlayer .a9s-selection .a9s-outer, 
svg.a9s-annotationlayer .a9s-annotation .a9s-outer {
  display:none;
}

svg.a9s-annotationlayer .a9s-selection .a9s-inner,
svg.a9s-annotationlayer .a9s-annotation .a9s-inner  {
  stroke-width:4;
  stroke:white;
  stroke-dasharray:5;
}

svg.a9s-annotationlayer .a9s-annotation.editable:hover .a9s-inner {
  fill:transparent;
}

svg.a9s-annotationlayer .a9s-handle .a9s-handle-outer {
  display:none;
}

svg.a9s-annotationlayer .a9s-handle .a9s-handle-inner {
  fill:white;
  stroke:white;
  transform:scale(1, 1);
}

svg.a9s-annotationlayer .a9s-selection-mask {
  fill:rgba(0, 0, 0, 0.6);
}
```

## Customizing the Editor Popup Style

[...TODO...]
