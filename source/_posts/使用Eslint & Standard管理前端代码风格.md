---
title: 使用Eslint & Standard管理前端代码风格
tags:
  - JavaScript
  - Eslint
  - Standard
categories: Code
date: 2017-08-09
---

## Eslint

eslint是一个QA工具，用来保证团队代码风格一致性，以及避免低级错误，支持JS/JSX文件

通过.eslintrc.json可以对eslint进行配置，并且可以安装插件进行不同代码风格的自动配置

## [Standard](https://github.com/standard/standard)

standard是一套JavaScript 代码规范，自带 linter & 代码自动修正

<!-- more -->

### 细则

- **使用两个空格** – 进行缩进
- **字符串使用单引号** – 需要转义的地方除外
- **不再有冗余的变量** – 这是导致 *大量* bug 的源头!
- **无分号**
- **行首不要以 `(`, `[`, or `` ` `` 开头**
  - 这是省略分号时**唯一**会造成问题的地方 – *工具里已加了自动检测！*
- **关键字后加空格** `if (condition) { ... }`
- **函数名后加空格** `function name (arg) { ... }`
- 坚持使用全等 `===` 摒弃 `==` 一但在需要检查 `null || undefined` 时可以使用 `obj == null`。
- 一定要处理 Node.js 中错误回调传递进来的 `err` 参数。
- 使用浏览器全局变量时加上 `window` 前缀 – `document` 和 `navigator` 除外
  - 避免无意中使用到了这些命名看上去很普通的全局变量， `open`, `length`,
    `event` 还有 `name`。
- **[查看更多](https://github.com/standard/standard)**

## 配置

standard作为插件的形式配置Eslin，并进行代码风格检查

###  config

```bash
npm install --save-dev eslint-config-standard eslint-config-standard-jsx eslint-plugin-standard eslint-plugin-promise eslint-plugin-import eslint-plugin-node eslint-plugin-react
```

// .eslintrc.json

```json
{
  "env": {
    "browser": true,
    "commonjs": true,
    "es6": true,
    "node": true
  },
  "extends": ["standard", "standard-jsx"],
  "parserOptions": {
    "ecmaFeatures": {
      "experimentalObjectRestSpread": true,
      "jsx": true
    },
    "sourceType": "module"
  },
  "plugins": [
    "react"
  ],
  "parser": "babel-eslint",
  "rules": {
    "strict": 0,
    "no-console": 0,
    "no-unused-vars": 1,
    "jsx-quotes": ["error", "prefer-double"],
    "react/prop-types": 0,
    "react/no-children-prop": 0,
    "react/self-closing-comp": 1,
    "react/jsx-pascal-case": 1,
    "react/jsx-closing-bracket-location": 1,
    "react/sort-comp": 1,
    "react/jsx-uses-vars": 1,
    "react/react-in-jsx-scope": 1,
    "react/display-name": 0,
    "react/jsx-key": 1,
    "react/jsx-tag-spacing": 1
  }
}
```

<div class="tip">

因为standard在jsx中会默认使用单引号的规则，所以需要加上`"jsx-quotes": ["error", "prefer-double"]`保证jsx中属性为双引号

</div>

### fix

`"fix": "./node_modules/.bin/eslint src/**/*.js src/**/*.jsx --fix"`

### pre-commit

```json
npm i pre-commmit --save-dev
```

// eslint.sh
```js
#!/bin/bash

for file in $(git diff --cached --name-only | grep -E '\.(js|jsx)$')
do
  git show ":$file" | node_modules/.bin/eslint --stdin --stdin-filename "$file" # we only want to lint the staged changes, not any un-staged changes
  if [ $? -ne 0 ]; then
    echo "ESLint failed on staged file '$file'. Please check your code and try again. You can run ESLint manually via npm run eslint."
    exit 1 # exit with failure status
  fi
done
```

// package.json
```json
{
  "scripts": {
    "eslint": "sh eslint.sh"
  },
  "pre-commit": [
    "eslint": "sh eslint.sh",
    "eslint"
  ]
}
```

<div class="tip">

这样每次commit都会对暂存区的js/jsx文件进行检测了(检测会花费一些时间，耐心点)，当然也可以直接修改`.git/hooks/pre-commit`，因为git hook无法提交，所以改为外部脚本。

</div>