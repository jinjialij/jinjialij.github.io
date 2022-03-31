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

## Initialize a project

`npm init -y`  
`npm install eslint --save-dev`  
`npm init @eslint/config`

Add `.eslintrc` file

```json
{
  "extends": "eslint:recommended",
  "rules": {
    "semi": ["error", "always"],
    "quotes": ["error", "double"]
  }
}
```

In the `Perefernce`->`Setting`, search `formatting`
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
