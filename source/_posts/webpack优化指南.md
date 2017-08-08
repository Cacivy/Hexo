---
title: webpack优化指南
tags:
  - JavaScript
  - Webpack
categories: Code
date: 2017-07-25
---

主要针对以下几个问题进行优化：
1. 打包编译速度慢
2. 打包后文件体积大

使用`webpack-bundle-analyzer`插件可以图形化观看每个module的体积大小

<!-- more -->

- 抽取公共库部分合并从html中引入，然后配置externals，如

```js
 externals: {
    'react': 'React',
    'react-dom': 'ReactDOM',
    'redux': 'Redux',
    'react-redux': 'ReactRedux',
    'axios': 'axios'
  }
```

- ExtractTextPlugin把css文件单独抽离出来

- gzip压缩

```js
new CompressionWebpackPlugin({ //gzip 压缩
  asset: '[path].gz[query]',
  algorithm: 'gzip',
  test: new RegExp(
    '\\.(js|css)$' //压缩 js 与 css
  ),
  threshold: 10240,
  minRatio: 0
}),
```

- 忽略moment中其它语言包

```
new webpack.ContextReplacementPlugin(/moment[\/\\]locale$/, /zh/)
```

- 使用DllPlugin抽离公共库，然后通过script标签引入

```js
// webpack.vendor.config.js
const vendors = ['react', 'react-dom', 'mobx', 'mobx-react', 'axios'];
module.exports = {
  output: {
    path: paths.appPublic,
    filename: '[name].[chunkhash:8].js',
    library: '[name]_[chunkhash:8]',
  },
  entry: {
    'vendor': vendors,
  },
  plugins: [
    new webpack.DefinePlugin(env.stringified),
    new webpack.optimize.UglifyJsPlugin({
      compress: {
        warnings: false,
        comparisons: false,
      },
      output: {
        comments: false,
      },
      sourceMap: true,
    }),
    new webpack.DllPlugin({
      path: path.join(__dirname, '[name]-manifest.json'),
      name: '[name]_[chunkhash:8]',
      context: __dirname,
    })
  ],
};

// webpack.config.prod.js
new webpack.DllReferencePlugin({
  context: __dirname, 
  manifest: require('./vendor-manifest.json')
})
```




