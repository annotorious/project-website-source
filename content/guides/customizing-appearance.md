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

# Customizing the Visual Appearance

Need a different look and feel for Annotorious to match your application? Customizing
the visual appearance is easy! All elements of the annotation layer and the editor window
can be styled with CSS.

## Customizing the Annotation Layer

Annotations in Annotorious are rendered using [SVG](https://developer.mozilla.org/en-US/docs/Web/SVG).
Annotorious applies various CSS classes to the different types of shapes. By writing your own stylesheet
for these classes, you can completely alter the visual appearance of every graphical element.

To make styling more flexible, Annotorious adds a few extras:

- For every annotation shape, Annotorious renders two SVG shapes, exactly on top of each other. The two shapes
  have different CSS classes - `a9s-inner` and `a9s-outer`. You can use this to create line styles with an outline.
  (__TODO: example code and image__)
- When the user creates a new selection, Annotorious renders a mask element. You can use this to dim the area outside
  of the selection. (__TODO: example code and image__)

## Customizing the Editor Popup

[...TODO...]
