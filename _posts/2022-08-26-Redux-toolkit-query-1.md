---
layout: post
title: 'Redux toolkit query (1)'
subtitle: 'A study note: Using RTK query in React (1)'
date: 2022-08-01
author: 'Jiali'
header-img: 'img/post-bg-react.png'
tags:
  - Frontend
  - RTKQuery
  - ReduxToolkit
  - Redux
---

This is a study note for an [online course for RTK query, lesson 6](https://www.youtube.com/watch?v=HyZzCHgG3AY).
RTK Query is a powerful data fetching and caching tool. Other alternatives:  Apollo Client, React Query, Urql, and SWR
This article will go over basic content and an example for a GET api endpoint query.

# RTK Query

## Basic Apis

### createApi()

> The core of RTK Query's functionality. It allows you to define a set of endpoints describe how to retrieve data from a series of endpoints, including configuration of how to fetch and transform that data. In most cases, you should use this once per app, with "one API slice per base URL" as a rule of thumb.

### fetchBaseQuery() 

> This is a very small wrapper around [`fetch`](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) that aims to simplify requests. It is not a full-blown replacement for `axios`, `superagent`, or any other more heavy-weight library, but it will cover the large majority of your needs.

> It takes all standard options from fetch's [`RequestInit`](https://developer.mozilla.org/en-US/docs/Web/API/WindowOrWorkerGlobalScope/fetch) interface, as well as `baseUrl`, a `prepareHeaders` function, an optional `fetch` function, and a `paramsSerializer` function.

### setupListeners()

> A utility used to enable refetchOnMount and refetchOnReconnect behaviors.

### transformResponse

> allows manipulation of the data returned by a query or mutation before it hits the cache.
> transformResponse is called with the data that a successful baseQuery returns for the corresponding endpoint, and the return value of transformResponse is used as the cached data associated with that endpoint call.

### tagType

> When defining a tag type, you will be able to provide them with providesTags and invalidate them with invalidatesTags when configuring endpoints.

```javascript
// apiSlice.js
import { createApi, fetchBaseQuery } from '@reduxjs/toolkit/query/react'

export const apiSlice = createApi({
    reducerPath: 'api', // optional
    baseQuery: fetchBaseQuery({ baseUrl: 'http://localhost:3500' }),
    tagTypes: ['Todos'], // optional, used for invalidation 
    endpoints: (builder) => ({
        getTodos: builder.query({// Query endpoints
            query: () => '/todos',
            transformResponse: res => res.sort((a, b) => b.id - a.id),// manipulate data
            providesTags: ['Todos']// Used by query endpoints. Determines which 'tag' is attached to the cached data returned by the query.
        })
    })
})

// hooks that are auto-generated based on the defined endpoint
export const {
    useGetTodosQuery
} = apiSlice

```

## Configure the Store

The pokemonApi slice contains an auto-generated Redux slice reducer and a custom middleware that manages subscription lifetimes. Both of those need to be added to the Redux store:

```javascript
import { configureStore } from '@reduxjs/toolkit'
import { apiSlice } from "./features/api/apiSlice";

export const store = configureStore({
  reducer: {
    // Add the generated reducer as a specific top-level slice
    [apiSlice.reducerPath]: apiSlice.reducer,
  },
  // Adding the api middleware enables caching, invalidation, polling,
  // and other useful features of `rtk-query`.
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(apiSlice.middleware),
});
```

### Use Hooks in Components

#### Some Query Hook Return Values

- data
  -  The latest returned result regardless of hook arg, if present.
- currentData 
  - The latest returned result for the current hook arg, if present.
- error 
  - The error result if present.
- isLoading 
  - When true, indicates that the query is currently loading for the first time, and has no data yet. This will be true for the first request fired off, but not for subsequent requests.
- isSuccess 
  - When true, indicates that the query has data from a successful request.
- isError 
  - When true, indicates that the query is in an error state.
- refetch 
  - A function to force refetch the query

[Frequently Used Query Hook Return Values] (https://redux-toolkit.js.org/rtk-query/usage/queries#frequently-used-query-hook-return-values)

```javascript
const TodoList = () => {
    const [newTodo, setNewTodo] = useState('')

    // Using a query hook automatically fetches data and returns query values
    // Query hooks automatically begin fetching data as soon as the component is mounted.
    const {
        data: todos,
        isLoading,
        isSuccess,
        isError,
        error
    } = useGetTodosQuery();
    // Individual hooks are also accessible under the generated endpoints:
    // const { data, error, isLoading } = apiSlice.endpoints.getTodos.useQuery()
  

    let content;
    if (isLoading) {
        content = <p>Loading...</p>
    } else if (isSuccess) {
        content = JSON.stringify(todos)
    } else if (isError) {
        content = <p>{error}</p>
    }

    return (
        <main>
            <h1>Todo List</h1>
            {content}
        </main>
    )
}

```

[GitHub Complete Code for TodoList component](https://github.com/jinjialij/react_redux_toolkit_example/blob/main/06_lesson/src/features/todos/TodoList.js)

# Reference
- [RTK Query overview](https://redux-toolkit.js.org/rtk-query/overview)

