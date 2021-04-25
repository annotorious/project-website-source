---
title: "Annotorious in React"
date: 2021-04-25T16:45:12+02:00
draft: false
layout: "section-page"
subsection: "guides"
blurb: "A how-to that explains how you can use Annotorious in a React application."
weight: 4
meta_title: "Annotorious in React"
meta_description: "A how-to that explains how you can use Annotorious in a React application."
meta_link: "https://recogito.github.io/guides/annotorious-in-react"
---

# Using Annotorious in React

At the moment, Annotorious doesn't have native React integration (yet). If you want to use
Annotorious in your own React project, there are a few things to consider:

- __Annotorious modifies the DOM directly:__ you need to use [refs](https://reactjs.org/docs/refs-and-the-dom.html)
  to access image DOM elements.
- __Annotorious has an imperative API:__ wrapping it for React requires a few workarounds, including initialization
  in a `useEffect` hook, and storing the `anno` object in the component state.

Below is a minimal React component to get you started:

```jsx
import { useEffect, useRef, useState } from 'react';
import { Annotorious } from '@recogito/annotorious';

import '@recogito/annotorious/dist/annotorious.min.css';

function App() {

  // Ref to the image DOM element
  const imgEl = useRef();

  // The current Annotorious instance
  const [ anno, setAnno ] = useState();

  // Current drawing tool name
  const [ tool, setTool ] = useState('rect');

  // Init Annotorious when the component
  // mounts, and keep the current 'anno'
  // instance in the application state
  useEffect(() => {
    let annotorious = null;

    if (imgEl.current) {
      // Init
      annotorious = new Annotorious({
        image: imgEl.current
      });

      // Attach event handlers here
      annotorious.on('createAnnotation', annotation => {
        console.log('created', annotation);
      });

      annotorious.on('updateAnnotation', (annotation, previous) => {
        console.log('updated', annotation, previous);
      });

      annotorious.on('deleteAnnotation', annotation => {
        console.log('deleted', annotation);
      });
    }

    // Keep current Annotorious instance in state
    setAnno(annotorious);

    // Cleanup: destroy current instance
    return () => annotorious.destroy();
  }, []);

  // Toggles current tool + button label
  const toggleTool = () => {
    if (tool === 'rect') {
      setTool('polygon');
      anno.setDrawingTool('polygon');
    } else {
      setTool('rect');
      anno.setDrawingTool('rect');
    }
  }

  return (
    <div>
      <div>
        <button
          onClick={toggleTool}>
            { tool === 'rect' ? 'RECTANGLE' : 'POLYGON' }
        </button>
      </div>

      <img 
        ref={imgEl} 
        src="640px-Hallstatt.jpg" 
        alt="Hallstatt Town Square" />
    </div>
  );
}

export default App;
```