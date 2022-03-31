---
layout: post
title: 'Axios Mocking with React'
subtitle: 'Using axios-mock-adapter'
date: 2022-03-24
author: 'Jiali'
header-img: 'img/post-bg-react.png'
tags:
  - React
  - Axios
  - Frontend
---

# Axios Mocking with React

During development, there are cases that we don't want to call the real API, but also to be able to easily test special cases. Therefore, mock axios call is an option. I read an [article](https://blog.bitsrc.io/axios-mocking-with-reactjs-85d83d51704f) about how to use axios to mock in React. This blog is a summary of what I did when I follow the article and try to build the mock. Related code can be accessed through [CodeSandBox link](https://codesandbox.io/s/axios-mocking-with-react-ruubn5?file=/src/index.js) or [Git repo](). This mocking uses a npm package named as `axios-mock-adapter`. It allows to easily mock requests.

## Install axios-mock-adapter

In the terminal, run `npm install --save-dev axios-mock-adapter`

## Create an Axios instance and pass it to the Axio adapter

The reason that you need to create an Axios instance is because when you don't, you will interfere other axios calls.

```javascript
import axios from 'axios';
import MockAdapter from 'axios-mock-adapter';

const mock = new MockAdapter(axios);
const mockData = {
  users: [{ id: 1, name: 'Jane Smith' }],
};
mock.onGet('/users').reply(200, mockData);

const { data } = await axios.get('/users');
```

The above code doesn't create an instance. Therefore, when you do `const res = await axios(url);`, and you use axios in another files, you will get errors. Because they share the same axios you use to do the mocking. That's why you need a seprate instance for mock.

In `axios-mock-instance.js` file:

```javascript
import axios from 'axios';
import AxiosMockAdapter from 'axios-mock-adapter';
const axiosMockInstance = axios.create();
export const axiosMockAdapterInstance = new AxiosMockAdapter(
  axiosMockInstance,
  { delayResponse: 0 }
);
export default axiosMockInstance;
```

You can use the `mockAdapterInstance` to implement mock get and post methods

```javascript
import { axiosMockAdapterInstance } from './axios-mock-instance';

export const MockGetApi = (baseurl, mockdata, params) => {
  return axiosMockAdapterInstance
    .onGet(baseurl, { params: params })
    .reply(200, mockdata);
};

export const MockPostApi = (baseurl, mockdata, params) => {
  return axiosMockAdapterInstance
    .onPost(baseurl, { params: params })
    .reply(200, mockdata);
};
```

In tests, you can find that `axiosMockInstance` call can get the returned mock data of the `axiosMockAdapterInstance`, by simplely call methods, using the same url and params.

```javascript
const data1 = await axiosMockInstance
  .get(baseurl, { params: searchParams1 })
  .then(function (response) {
    return response.data;
  });
```

> Note: When using params, you must match all key/value pairs passed to that option. Requests that do not map to a mock handler are rejected with a HTTP `404` response. Chaining is also supported. And the order of mock handlers is significant if you chain it.

## Reference

[Axios](https://axios-http.com/docs/intro)  
[Github: axios-mock-adapter](https://github.com/ctimmerm/axios-mock-adapter) / [nicedoc.io: axios-mock-adapter](https://nicedoc.io/ctimmerm/axios-mock-adapter)  
[Axios Mocking with React](https://blog.bitsrc.io/axios-mocking-with-reactjs-85d83d51704f)
