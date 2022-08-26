---
layout: post
title: 'Redux toolkit query (2)'
subtitle: 'A study note: Using RTK query in React (2)'
date: 2022-08-30
author: 'Jiali'
header-img: 'img/post-bg-redux.png'
tags:
  - Frontend
  - RTKQuery
  - ReduxToolkit
  - Redux
---

Continue Redux toolkit query (1), adding more to the apiSlice to support POST, DELETE. It also includes build.mutation

# RTK query (2)

## Mutation

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
        }),
        addTodo: builder.mutation({// mutation endpointes : https://redux-toolkit.js.org/rtk-query/usage/mutations#defining-mutation-endpoints
            query: (todo) => ({
                url: '/todos',
                method: 'POST',
                body: todo
            }),
            invalidatesTags: ['Todos']// Determines which cached data should be either re-fetched or removed from the cache
        }),
        updateTodo: builder.mutation({
            query: (todo) => ({
                url: `/todos/${todo.id}`,
                method: 'PATCH',
                body: todo
            }),
            invalidatesTags: ['Todos']
        }),
        deleteTodo: builder.mutation({
            query: ({ id }) => ({
                url: `/todos/${id}`,
                method: 'DELETE',
                body: id
            }),
            invalidatesTags: ['Todos']
        }),
    })
})

// hooks that are auto-generated based on the defined endpoint
export const {
    useGetTodosQuery,
    useAddTodoMutation,
    useUpdateTodoMutation,
    useDeleteTodoMutation
} = apiSlice

```
