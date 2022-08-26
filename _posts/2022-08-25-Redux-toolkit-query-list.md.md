---
layout: post
title: 'Redux toolkit query learning memo'
subtitle: 'A study note memo: Using RTK query in React'
date: 2022-08-01
author: 'Jiali'
header-img: 'img/post-bg-redux-toolkit.png'
tags:
  - Frontend
  - RTKQuery
  - ReduxToolkit
  - Redux
---

This is a study note memo for learning RTK query along with an [online course for RTK query, lesson 6](https://www.youtube.com/watch?v=HyZzCHgG3AY).
RTK Query is a powerful data fetching and caching tool. Other alternatives:  Apollo Client, React Query, Urql, and SWR
This article will go over basic content and an example for a GET api endpoint query.

# RTK Query conetnt
- Basic Apis and set up
- Mutation
- Cache Behavior
- Automated Re-fetching
- Manual Cache Updates
  - Optimistic Updates
- Conditional Fetching (https://redux-toolkit.js.org/rtk-query/usage/conditional-fetching#overview)
> RTK Query supports conditional fetching to enable that behavior.
> If you want to prevent a query from automatically running, you can use the skip parameter in a hook.
> When skip is true (or skipToken is passed in as arg):
- Pagination
- Prefetching


# Reference
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)

