---
layout: post
title: 'Improve Formik performance with MUI'
subtitle: ''
date: 2023-01-27
author: 'Jiali'
header-img: 'img/post-bg-react.png'
tags:
  - React
  - Frontend
  - MUI
  - Formik
---

My project needs to render dynamic forms, which could be really big (according to the data in the database). I build them using Formik because it needs complex validation, data mapping, state control. When it's a big form, the form will be very sluggish when user inputting data in the form. Thus I have to find a way to improve its performance to gurantee that users can interact with the form smoothly.  

## Reason

The sluggish is due to re-render issue in Formik. Whenever you `setFieldValue` or `setValues`, Formik object will re-render. And it's per keystroke. It means if you have hundreds of input field, every keystroke in any field will rerender the formik object. Since my form is dynamically updated according to any change user made in the form, and the page will re-calculate, re-validate immediately, the sluggish is appearent and affect the user experience.  

The improvement I made follows what [Improving Formik Performance when it's Slow (Material UI)](https://hackernoon.com/improving-formik-performance-when-its-slow-material-ui) suggests. The principle is to re-render formik based on onBlur event instead of per keystroke. The source code is [here](https://github.com/superjose/increase-formik-performance-react/blob/main/src/components/Fields/Form/PerformantTextField/index.tsx)

The changes I made mainly in `onBlur`. The code is [here](https://gist.github.com/jinjialij/8be890d2ee992987164eb2cdc3f3f2fd)

```javascript
const onBlur = (evt: React.FocusEvent<HTMLInputElement>) => {
      const val = evt.target.value || "";
      window.setTimeout(() => {
        // Add your own logic
        field.onChange({
          target: {
            name: props.name,
            value: props.type === "number" ? parseInt(val, 10) : val,
          },
        });
      }, 0);
    };
```

After add the improved textField and usePropagateRef hook, the sluggish issue is gone.   

## Reference
[Improving Formik Performance when it's Slow (Material UI)](https://hackernoon.com/improving-formik-performance-when-its-slow-material-ui)
[Code source](https://github.com/superjose/increase-formik-performance-react/blob/main/src/components/Fields/Form/PerformantTextField/index.tsx)