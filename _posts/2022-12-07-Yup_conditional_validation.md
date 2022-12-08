---
layout: post
title: 'Encounter cyclic dependency error in Yup'
subtitle: 'Conditional validation'
date: 2022-12-01
author: 'Jiali'
header-img: 'img/post-bg-idea.jpg'
tags:
  - Yup
  - Frontend
---

When using Yup to construct a schema to do a conditonal validation( either all fields are empty or all of them have values), it throws error: cyclic dependency within a schema. [Check Explaination here](https://github.com/jquense/yup/issues/176#issuecomment-367352042) 

To get rid of the error, need to add dependecy so that yup won't complain

```
export const getBillSchema = (a, b, c, d) => {
  return Yup.object().shape(
    {
      [a]: Yup.number().when([b, c, d], {
        is: (b, c, d) => b || c || d,
        then: Yup.number().nullable().required('Data not entered'),
        otherwise: Yup.number().nullable().notRequired()
      }),
      [b]: Yup.number().when([a, c, d], {
        is: (a, c, d) => a || c || d,
        then: Yup.number().nullable().required('Data not entered'),
        otherwise: Yup.number().nullable().notRequired()
      }),
      [c]: Yup.number().when([a, b, d], {
        is: (a, b, d) => a || b || d,
        then: Yup.number().nullable().required('Data not entered'),
        otherwise: Yup.number().nullable().notRequired()
      }),
      [d]: Yup.number().when([a, b, c], {
        is: (a, b, c) => a || c || b,
        then: Yup.number().nullable().required('Data not entered'),
        otherwise: Yup.number().nullable().notRequired()
      })
    },
    [
      [a, b],
      [a, c],
      [a, d],
      [b, a],
      [b, c],
      [b, d],

      [c, a],
      [c, b],
      [c, d],

      [d, a],
      [d, b],
      [d, c]
    ]
  );
};
```

# Reference
- [Yup](https://github.com/jquense/yup)
- [Conditionally validate](https://github.com/jquense/yup/issues/176)