# git-lint-work-flow

## Introduction

使用`git`的`pre-commit`钩子来对工作区的文件进行格式化以及按规则lint文件。

## Install

### eslint and some plugins

```bash
yarn add eslint eslint-plugin-vue@next eslint-plugin-import@latest eslint-plugin-node@latest eslint-plugin-promise@latest eslint-plugin-standard eslint-config-standard babel-eslint -D
```
然后添加`.eslintrc.js`以及`.eslintignore`文件

### stylelint and some plugins

```bash
yarn add stylelint stylelint-order stylelint-scss stylelint-config-standard stylelint-config-idiomatic-order -D
```
然后添加`.eslintrc.js`以及`.eslintignore`文件

### prettier and some plugins
```bash
yarn add prettier eslint-config-prettier eslint-plugin-prettier stylelint-config-prettier -D
```
然后添加`.prettierrc`以及`.prettierignore`文件

### git hook operate
```bash
yarn add husky lint-staged -D
```
然后修改`package.json`，添加git hook相关操作，添加如下内容，用于commit时格式化文件以及根据lint规则检测文件
```json
{
  "scripts": {
    "lint": "npm run lint:js && npm run lint:style",
    "lint:js": "eslint ./src/**/*.{js,vue} --fix",
    "lint:style": "stylelint './src/**/*.{vue,scss}' --fix",
    "lint-staged": "lint-staged"
  },
  "husky": {
    "hooks": {
      "pre-commit": "npm run lint-staged"
    }
  },
  "lint-staged": {
    "**/*.{js,vue,scss,css}": [
      "npm run prettier",
      "git add"
    ],
    "**/*.{js,vue}": [
      "npm run lint:js",
      "git add"
    ],
    "**/*.{vue,scss,css}": [
      "npm run lint:style",
      "git add"
    ]
  }
}
```

## Tips

关于为什么使用`eslint-plugin-vue@next`这个插件，而不是使用4.x版本，参照[这篇文章](https://www.zhaofinger.com/detail/82)。