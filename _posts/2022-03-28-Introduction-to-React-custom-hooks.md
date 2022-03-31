---
layout: post
title: 'Introduction to React custom hooks'
subtitle: 'A brief guide'
date: 2022-03-28
author: 'Jiali'
header-img: 'img/post-bg-react.png'
tags:
  - React
  - Hooks
  - Frontend
---

# Introduction to React custom hooks

In this article, we will introduce what are React custom hooks, how to build and use custom hooks, and when to use them.

## What is a custom hook?

Since React 16.8, Hooks are introduced. Custom hooks, just as React built-in hooks, are JavaScript functions. Custom hooks are built by ourselves, with our defined logic and functionalities. Inside custom hooks, we can use other React hooks. In this way, we can extract component logic into reusable functions.

## How to build a custom hook

As React defined, A custom Hook is a JavaScript function whose name starts with `use` and it calls other Hooks.

> **Note:** It’s very important to follow the naming convention. It tells React that the rules of Hooks apply to it. Otherwise, it is only a normal function. React can’t tell if the custom hook contains calls to Hooks inside of it and may cause errors.

Consider if we have a form component that allows users to input first and last names, we will use multiple states to handle the change.

```javascript
function NameForm() {
  const [enteredFirstName, setEnteredFirstName] = useState('');
  const [enteredLastName, setEnteredLastName] = useState('');

  const firstNameChangeHanlder = (event) => {
    setEnteredFirstName(event.target.value);
  };
  const lastNameChangeHanlder = (event) => {
    setEnteredLastName(event.target.value);
  };

  return (
    <form>
      <label htmlFor="name">First Name</label>
      <input
        type="text"
        value={enteredFirstName}
        onChange={firstNameChangeHanlder}
      />

      <label htmlFor="name">Last Name</label>
      <input
        type="text"
        value={enteredLastName}
        onChange={lastNameChangeHanlder}
      />
    </form>
  );
}
```

As you can see, it uses two useState hooks to update input changes for each input. Also, those codes repeat the same logic. So, we would like to extract it into a custom hook. Here’s how we transform it into a custom hook.

```javascript
import React, { useState } from 'react';

function useTextInput() {
  const [enteredValue, setEnteredValue] = useState('');

  const valueChangeHandler = (event) => {
    setEnteredValue(event.target.value);
  };

  return {
    value: enteredValue,
    valueChangeHandler,
  };
}

export default useTextInput;
```

## Use the custom hook

In the `NameForm` component, we can now use our custom hook. We import `useTextInput` and deconstruct return values of `useTextInput`. Then, in the JSX codes, we use these variables.

```javascript
import useTextInput from '../hooks/use-input';

function NameForm() {
  const { value: firstNameValue, valueChangeHandler: firstNameChangeHanlder } =
    useTextInput();

  const { value: lastNameValue, valueChangeHandler: lastNameChangeHanlder } =
    useTextInput();

  return (
    <form>
      <label htmlFor="name">First Name</label>
      <input
        type="text"
        value={firstNameValue}
        onChange={firstNameChangeHanlder}
      />

      <label htmlFor="name">Last Name</label>
      <input
        type="text"
        value={lastNameValue}
        onChange={lastNameChangeHanlder}
      />
    </form>
  );
}

export default NameForm;
```

Moreover, you can also pass parameters and functions in custom hooks. Here we allow the `useTextInput` to accept a value variable and a validate function.

```javascript
function useTextInput(value, validate) {
  const [enteredValue, setEnteredValue] = useState(value);

  const isValid = validate(enteredValue);

  const valueChangeHandler = (event) => {
    setEnteredValue(event.target.value);
  };

  return {
    value: enteredValue,
    isValid: isValid,
    valueChangeHandler,
  };
}
```

The passing value will be the initial value of the enteredValue. It uses the validate function to determine if the value is empty.

```javascript
const {
  value: firstNameValue,
  valueChangeHandler: firstNameChangeHanlder,
  isValid: firstNameValid,
} = useTextInput('Jane', (value) => value.trim() !== '');
```

To use the custom hook in the `NameForm` component, you only need to pass ‘Jane’ and a function to `useTextInput`. That’s how custom hooks accept parameters.

## Why and when using custom hooks

The main reason to write a custom hook is for code reusability. From the above example, we can see when components share the same logic, and they all use React hooks inside themselves, custom hooks are a handy way to reduce duplicate codes. We take out the shared codes and refactor them by creating a separate function to hold them. As hooks have two rules:

- Only Call Hooks at the Top Level
- Only Call Hooks from React Functions

If we want to extract a component that uses hooks, we will use custom hooks. It keeps components simple and enables the shared logic to be used among components. As a result, it improves code reusability. Besides, it also isolates the testable logic and ensures testing.

For offical reference:
[React documentation](https://reactjs.org/docs/hooks-custom.html)
