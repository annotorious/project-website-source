---
title: "Annotorious in Vue"
date: 2022-09-18T08:54:00+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "A code example that shows how to use Annotorious in a Vue application."
weight: 4
meta_title: "Annotorious in Vue"
meta_description: "A code example that shows how to use Annotorious in Vue.js"
meta_link: "https://recogito.github.io/guides/annotorious-in-vue"
---

# Using Annotorious in Vue.js

Below is a minimal example for using Annotorious in Vue.js. Thanks to 
[Grant Brits](https://github.com/gbrits) for this contribution!

```html
<template>
  <div>
    <img id="plan" src="img.png" style="width: 100%; max-width: 1024px;" />
  </div>
</template>

<script>
  import { Annotorious} from '@recogito/annotorious';
  import '@recogito/annotorious/dist/annotorious.min.css';

  export default {
    
    data() {
      return {
        anno: null
      }
    },

    methods: {
      initAnno() {
        this.anno = new Annotorious({
          image: document.getElementById("plan")
        });

        this.anno.on('createAnnotation', function (annotation) {
          console.log('Created annotation', annotation);
        });

        this.anno.on('createSelection', function (selection) {
          console.log('Created selection', selection);
        });

        this.anno.on('deleteAnnotation', function (annotation) {
          console.log('Delete annotation', selection);
        });

        this.anno.on('mouseEnterAnnotation', function (annotation, element) {
          console.log('Mouse ENTERED annotation', annotation);
        });

        this.anno.on('selectAnnotation', function (annotation, element) {
          console.log('Select annotation', annotation);
        });

        this.anno.on('cancelSelected', function (selection) {
          console.log('UNSELECTED');
        });

        this.anno.on('clickAnnotation', function (annotation, element) {
          console.log('Clicked annotation', annotation);
        });
      }
    },

    mounted() {
      this.initAnno();
    }
  }
</script>
```