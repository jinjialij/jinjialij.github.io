---
layout: post
title: 'Block user issue in React APP'
subtitle: 'Issue encountered when try to block user if they leave/refresh/close a page'
date: 2023-03-08
author: 'Jiali'
header-img: 'img/post-bg-react.png'
tags:
  - React
  - Frontend
  - React router dom
---

During my work, I was asked to add a feature that pops up a dialogue and asks user if they would like to save their unsaved form as a draft when they are are leaving/refresh/close the page. As I started the task, I find it's bit tricky. I did a lot a research on it and I think it's worth to have a note here. The app uses React 18 along with react-router-dom v6.6

## Task description

When a user input at least one field in the form, a dialogue should popup up on in the following scenario:

1. The user is trying to refresh the page

2. The user is trying to close the tab

3. The user is trying to navigate to any other page

For the first and second cases, I decided to handled in the `beforeunload` event. For the third case, I decide to handle it using `useBlock`, a hook provided by `react-router-dom`. But I encountered issues.

## Beforeunload event

This event is fired when the window, the document and its resources are about to be unloaded. The document is still visible and the event is still cancelable at this point.[Window: beforeunload event MDN](https://developer.mozilla.org/en-US/docs/Web/API/Window/beforeunload_event). If you have the following codes, it will pop up a confirm window to ask if you want to leave even if the page may have unsaved data. (See attached screenshots)

![Leaving page](/img/post-leave_site.png)
![Reload page](/img/post-reload_site.png)

```javascript
const beforeUnloadListener = (event) => {
  event.preventDefault();
  return event.returnValue = '';
};

const nameInput = document.querySelector("#name");

nameInput.addEventListener("input", (event) => {
  if (event.target.value !== "") {
    addEventListener("beforeunload", beforeUnloadListener, {capture: true});
  } else {
    removeEventListener("beforeunload", beforeUnloadListener, {capture: true});
  }
});
```

However, the beforeunload event behaviour differs from different browsers. Some browsers, like Chrome will ignore `window.confirm`. It make it impossible to open a dialog to ask users. If I set a setTimeout function, due to the setTimeout() is an asynchronous function, the result of the opened confirm dialogue is inaccessible. 

## Block navigation
Block navigation is easy if you are using `react-router-dom v5`. You just need to use the hooks it provides: `usePrompt` and `useBlock`. It's tricky when you are using v6 because they remove them during v6-6.6. To solve this, you have four options:
1. Manually adding useBlock
2. Upgrade to v6.8 (They add new unstable useBlock)
3. Downgrade to v5.
4. Figure out a way without using react-router-dom


### Manually adding useBlock
To manually add the hook back, you have to replace `BrowserRouter` with `unstable_HistoryRouter`, install `history` package, and inject it into `unstable_HistoryRouter`. Otherwise, it will throw an error: `navigator.block` is not a function (Because `react-router-dom` v6.6 streamlined and inlined the `history` library and removed the block method).  

This method works but it ships with the risk of using `unstable_HistoryRouter`  

Here are some useful links gives more details:  
[use-blocker in react-router doc](https://github.com/remix-run/react-router/blob/main/decisions/0001-use-blocker.md)  
[issue: Getting usePrompt and useBlocker back in the router #8139](https://github.com/remix-run/react-router/issues/8139)  
[Prompt is not supported in v6](https://reactrouter.com/en/main/upgrading/v5#prompt-is-not-currently-supported)

### Upgrade to v6.8
To use the unstable useBlock, you have to upgrade v6.8. However, this useBlock is depending on data router. So you have to replace `BrowserRouter` with `createBrowserRouter`.   
Since our app has a complex router, it will largely impact the codebase.

### Downgrade to v5
In this solution, you can use `usePrompt` directly but since the codebase using new changes in v6, it also requires a lot refactoring.  

### Other ways but don't work
1. Try to use `beforeunload` event  
This actually doesn't work. When changing navigation with react-router-dom because we use `<Link/>` or `<NavLink/>` so that the URL of the current page is updated using the HTML5 `history.pushState()` or `history.replaceState()` methods. These methods update the browser's history stack without actually navigating to a new page, so the `beforeunload` event is not triggered. So this doesn't work.

2. Try `popstate` event  
Therotically, the [popstate event](https://developer.mozilla.org/en-US/docs/Web/API/Window/popstate_event) only fires when the user clicks the back or forward buttons. There is no event for when the programmer called `window.history.pushState` or `window.history.replaceState` according to [react-router doc](https://github.com/remix-run/react-router/blob/main/docs/start/concepts.md) React-router-dom handles it through the history package. And the v6.6 streamlined and inlined the history library and removed the block method.   
To make block possible, I'll have to create my own history instance, which leads to the the Manually adding useBlock approach.

```javascript
window.addEventListener('popstate', (event) => {
  console.log(`${JSON.stringify(event.state)}`);
});
```


## Useful reference
[How to block user from leaving a page on a single page app](https://heyjiawei.com/block-user-from-leaving-page-on-single-page-app#so-we-can-block-the-user-from-leaving-the-page-on-a-single-page-app)
[Manually add useBlock and usePrompt](https://gist.github.com/rmorse/426ffcc579922a82749934826fa9f743)
[history package](https://github.com/remix-run/history/tree/main)
[react-router-dom](https://github.com/remix-run/react-router)