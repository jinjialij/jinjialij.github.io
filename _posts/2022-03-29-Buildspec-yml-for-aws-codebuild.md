---
layout: post
title: 'Build a CI/CD Pipeline on AWS CodePipeline'
subtitle: 'For a React App'
date: 2022-03-28
author: 'Jiali'
header-img: 'img/post-bg-react.png'
tags:
  - React
  - AWS
  - Devops
---

# Build a CI/CD Pipeline for a React App | AWS CodePipeline

Add in AWS Codepipeline

Create a `buildspec.yml` file. For yml information, you can view doc [here](https://yaml.org/)

```yml
# a typical format for React
version: 0.2
phases:
  install:
    commands:
      # install node xx version
      # install yarn(optional)
  pre_build:
    commands:
    # install dependencies
  build:
    commands:
      # tests
      - npm test
      # build
      - npm build
artifacts:
  # include all files required to run application
  # notably excluded is node_modules, as this will cause overwrite error on deploy
  files:
    - public/**/*
    - src/**/*
    - package.json
    - appspec.yml
    - scripts/**/*
    discard-path: no
    # base directory of the final build
    base-directory: dist
```

To find exact install node version, go to [Node.js](https://nodejs.org/en/), go to the `Other downloads`, find [Installing Node.js via package manager](https://nodejs.org/en/download/package-manager/), find appropriate one according to your system. For this article, we use `Debian and Ubuntu based Linux distributions` and it leads to [Nodesource](https://github.com/nodesource/distributions/blob/master/README.md). For your nodejs version, copy related commands. For this article, we use Node.js v16:

```cmd
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs
```

```yml
# My yml file
version: 0.2
phases:
  install:
    commands:
      # install node 16
      - echo Installing Node 16
      - curl -fsSL https://deb.nodesource.com/setup_16.x | bash -
      - apt-get install -y nodejs
  pre_build:
    commands:
      # install dependencies
      - echo Installing dependencies
      - npm install
  build:
    commands:
      # tests
      - echo Testing
      - npm test
      # build
      - echo building
      - npm build
artifacts:
  # include all files required to run application
  # notably excluded is node_modules, as this will cause overwrite error on deploy
  files:
    - '**/*'
    discard-path: no
    # base directory of the final build
    base-directory: dist
```
