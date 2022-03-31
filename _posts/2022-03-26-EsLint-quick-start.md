---
layout: post
title: 'EsLint in VScode'
subtitle: 'With Prettier'
date: 2022-03-26
author: 'Jiali'
header-img: 'img/post-bg-eslint.webp'
tags:
  - Eslint
  - Frontend
---

# EsLint and Prettier in VScode

## Initialize a project from scrach

Run `npm init -y` to initialize the project  
Run `npm install eslint --save-dev` to install ESLint locally in dev dependencies  
Run `npm init @eslint/config` to config

A `.eslintrc.{js,yml,json}` file is added  
For json file:

```json
{
  "extends": "eslint:recommended",
  "rules": {
    "semi": ["error", "always"],
    "quotes": ["error", "double"]
  }
}
```

For js file in React project

```javascript
module.exports = {
  env: {
    browser: true,
    es2021: true,
  },
  extends: ['plugin:react/recommended'],
  parserOptions: {
    ecmaFeatures: {
      jsx: true,
    },
    ecmaVersion: 12,
    sourceType: 'module',
  },
  plugins: ['react'],
  rules: {
    'react/jsx-indent': 'off',
    'react/jsx-indent-props': 'off',
    'react/react-in-jsx-scope': 'off', // not needed in React17
    'comma-dangle': 'off',
    'linebreak-style': 0, //different os have different linebreak-style
  },
};
```

### Use it

You can use npx to run ESLint on the command line like this:

```cmd
# Run on two files
npx eslint file1.js file2.js

# Run on multiple files
npx eslint lib/**

```

To run it in npm, you need to add lint in package.json file  
You can run `npm run lint` in React package

```json
{
  "scripts": {
    "lint": "eslint src/**/*.js src/**/*.jsx"
  }
}
```

## Add Prettier in VScode

In the VScode `Perefernce`->`Setting`, search `formatting`
Turn on `Format On Save` and select `Prettier` as `Default Formatter`

Or, [Open setting.json file and update it.](https://code.visualstudio.com/docs/getstarted/settings#_settingsjson)

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode"
}
```

For workplace, select Workspace and select `Prettier` as `Default Formatter`. A `.vscode` file and `setting.json` inside it will be created automatically.

For `Prettier`, you can config in `.prettierrc.json` For example, you want to use Single quotes instead of double quotes

```json
{
  "singleQuote": true
}
```

## Reference

[ESlint Doc](https://eslint.org/docs/user-guide/getting-started)

[Prettier Config](https://prettier.io/docs/en/configuration.html)
