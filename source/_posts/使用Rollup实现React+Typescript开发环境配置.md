---
title: 使用Rollup实现React+Typescript开发环境配置
tags:
  - JavaScript
categories: Code
date: 2017-12-21
---

## 背景

rollup是一个新兴的JavaScript模块打包器，支持最新的ES6，并且可以通过插件实现更多功能
我目前想写一个react插件，对打包工具的要求只需要简单的转换tsx即可，再加上对demo项目搭建一个本地测试服务，所以并没有选择笨重的webpack

<!--more-->

## 先上代码

```javascript
// rollup.config.js
import typescript from 'typescript'
import rollupTypescript from 'rollup-plugin-typescript'
import nodeResolve from 'rollup-plugin-node-resolve'
import commonjs from 'rollup-plugin-commonjs'
import replace from 'rollup-plugin-replace'
import serve from 'rollup-plugin-serve'
import livereload from 'rollup-plugin-livereload'
import uglify from 'rollup-plugin-uglify'

const dev = 'development'
const prod = 'production'

function parseNodeEnv(nodeEnv) {
  if (nodeEnv === prod || nodeEnv === dev) {
    return nodeEnv
  }
  return dev
}

const nodeEnv = parseNodeEnv(process.env.NODE_ENV)
const exampleBasicPath = './example/'
const exampleBundle = exampleBasicPath + '/bundle.js'
const port = 3005

const plugins = [
  replace({
    // The react sources include a reference to process.env.NODE_ENV so we need to replace it here with the actual value
    'process.env.NODE_ENV': JSON.stringify(nodeEnv)
  }),
  rollupTypescript({ typescript, importHelpers: true })
]

const pluginsExample = plugins.concat([
  // nodeResolve makes rollup look for dependencies in the node_modules directory
  nodeResolve(),
  commonjs({
    // All of our own sources will be ES6 modules, so only node_modules need to be resolved with cjs
    include: 'node_modules/**',
    namedExports: {
      // The commonjs plugin can't figure out the exports of some modules, so if rollup gives warnings like:
      // ⚠️   'render' is not exported by 'node_modules/react-dom/index.js'
      // Just add the mentioned file / export here
      'node_modules/react-dom/index.js': ['render'],
      'node_modules/react/react.js': ['Component', 'PropTypes', 'createElement']
    }
  })
])

if (nodeEnv === dev) {
  // For playing around with just frontend code the serve plugin is pretty nice.
  // We removed it when we started doing actual backend work.
  pluginsExample.push(
    serve({
      contentBase: './example/',
      port,
      historyApiFallback: true,
      open: true
    })
  )
  pluginsExample.push(livereload({
    watch: exampleBundle
  }))
}

if (nodeEnv === prod) {
  plugins.push(uglify())
}

export default [
  {
    input: 'src/index.tsx',
    output: {
      file: 'dist/bundle.js',
      format: 'umd',
      name: 'TextScroll'
    },
    watch: {
      include: './src/**'
    },
    plugins
  },
  {
    input: exampleBasicPath + 'index.tsx',
    output: {
      file: exampleBundle,
      format: 'iife',
      name: 'TextScroll'
    },
    watch: {
      include: ['./src/**', exampleBasicPath + '**']
    },
    plugins: pluginsExample
  }
]

```

## 分析

先介绍基本参数
+ input 入口文件
+ output
	- file 打包后文件
	- format 打包类型(amd, cjs, es, iife, umd)
	- name UMD类型需要
+ watch 检测的路径
+ plugins 插件

可以看到代码最后export了一个数组，里面有两个对象，分别负责不同的作用
1. 打包index.tsx文件，只使用`typescript`插件进行转换
2. 打包example里的index.tsx文件，除了`typescript`插件，还使用`nodeResolve`和commanjs把`react`和`reactDOM`库进行打包。以及使用`serve`和`liveload`插件实现本地服务器和代码检测浏览器自动刷新

## 对比Webpack

不再像webpack动辄十几个插件，配置也还算简单，但并没有预期的那么好用，期待[parceljs](https://parceljs.org/)


