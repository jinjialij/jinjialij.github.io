---
layout: post
title: 'Sass quick note'
subtitle: 'Sass cheatsheet'
date: 2022-03-23
author: 'Jiali'
header-img: 'img/post-bg-sass.png'
tags:
  - Sass
  - Frontend
---

# Sass quick note

Sass is a powerful preprocessor, which is a CSS extension language. It has features like nesting, mixins, inheritance, etc. It makes maintain style easier.

## Variable

Use `$variable : value` to declare variables. For example, you use `rgb(55,42,42)` in multiple places. It's very annoying to change all of them one by one. Using variable, it can be fairly easy.

Variables declared at the top level of a stylesheet are global. They can be accessed anywhere in their module after they’ve been declared. Variables declared in blocks are usually local. They can only be accessed within the block.

```css
.class1 {
  color: rgb(55, 42, 42);
}

.class2 {
  color: rgb(55, 42, 42);
}
```

```css
$primary-color: rgb(55,42,42);

.class1 {
  color:$primary-color;
}

.class2 {
  color:$primary-color;
```

## Nesting

In CSS, we usually encounter with a sitution where we want to specify style for elements under a class or element. In Sass, we can nesting these elements under the class just like what we do in html

```css
.class1 ul {
  list-style: none;
}

.class1 li {
  display: inline-block;
}

.class1 a {
  text-decoration: none;
}
```

```css
.class1 {
  ul {
    list-style: none;
  }

  li {
    display: inline-block;
  }

  a {
    text-decoration: none;
  }
}
```

## Parent selector &

It is a selector used in nested selectors to refer to the outer selector.

```css
$primary-color: rgb(55, 42, 42);
$secondary-color: #9c1458;

a {
  &:hover {
    background-color: #9c1458;
  }
  &:active {
    background-color: #9c1458;
  }
}
```

## At-Rules

`@use` loads mixins, functions, and variables from other Sass stylesheets, and combines CSS from multiple stylesheets together.

`@import` extends the CSS at-rule to load styles, mixins, functions, and variables from other stylesheets.

`@mixin` and `@include` makes it easy to re-use chunks of styles.

> **Note**: The Sass team discourages the continued use of the `@import` rule. Sass will gradually phase it out over the next few years, and eventually remove it from the language entirely. Prefer the @use rule instead. (Note that only Dart Sass currently supports `@use.` Users of other implementations must use the `@import` rule instead.)

### @use

You can store variables or styles in files and use them in another file.But you need to name the file starting with underscore, i.e `_variables.scss`, then in another file, add `@use "./variables";` at the top of the file.

> Note: Don't use `@import` as I metioned above

### `@mixin`

Mixins allow you to define styles that can be re-used throughout your stylesheet. The syntax is `@mixin name(<arguments...>) { ... }`

```css
@mixin flexcenter {
  display: flex;
  justify-contet: center;
  align-items: center;
}

.class2 {
  @include flexcenter();
}
```

Mixins can also take arguments

```css
@mixin flexcenter($direction) {
  display: flex;
  justify-contet: center;
  align-items: center;
  flex-direction: $direction;
}

.class2 {
  @include flexcenter(column);
}
```

### @extend

@extend allows one class have all the styles of another class, as well as its own specific styles.
Let's say we have class3 and we would like to have class4 that have the same style, but have a different background. You can use `@extend .class3` to have what `.class3` have and set background to pink, which will override the background defined in `.class3`

```css
.class3 {
  height: 100px;
  color: white;
  background: black;
  button {
    color: white;
    background: blue;
  }
}
```

```css
.class4 {
  @extend .class3 background:pink;
}
```

### Reference

[Sass Documentation](https://sass-lang.com/documentation)
