---
title: 如何优雅的实现queryString转换
tags:
  - JavaScript
categories: Code
date: 2017-08-23
---
[**URL**](https://developer.mozilla.org/en-US/docs/Web/API/URL)和[**URLSearchParams**](https://developer.mozilla.org/en-US/docs/Web/API/URLSearchParams)都是window的内置对象，借助于它们可以实现url querystring转换

### Constructor

```js
_const url_ = new URL(_urlString_, [_baseURLstring_])

_const url_ = new URL(_urlString_, _baseURLobject_)
```
_urlString_

一个绝对或相对URL

_baseURLstring _可选

`_urlString_`为相对URL时使用，默认为“about：blank”，如果是无效的绝对URL则会引发异常

_baseURLobject_

URL对象，作用同`_baseURLstring _`

<!-- more -->

### toJSON

通过URL对象可以很方便的把search字符串转换为json

```js
/**
* get url query object
* @param  {string}  str e: '?a=1&b=2&c=aa'
* @return  {object} o: {a: "1", b: "2", c: "aa"}
*/

// before
export const toJSON = (str) => {
  let obj = {}
  str.substr(1).split('&').forEach(item => {
    let kv = item.split('=')
    if (kv && kv.length) {
      obj[kv[0]] = kv[1]
    }
  })
  return obj
}

// after
export const toJSON = (str) => {
  let obj = {}
  let url = new URL(str)
  for (let [key, value] of url.searchParams.entries()) {
    obj[key] = value
  }
  return  obj
}
```

### toQueryString

这里需要用到URLSearchParams对象

```js
/**
* convert object to querystring
* @param  {object}  obj e: {a: 1, b: 2, c: 'aa'}
* @return  {string} o: {a: 1, b: 2, c: 'aa'}
*/

// before
export const toQuery = (obj) => {
  let str = '?'
  Object.keys(obj).forEach((key, i) => {
    str += `${i ? '&' : ''}${key}=${obj[key]}`
  })
  return str
}


// after
export const toQuery = (obj) => {
  let params = new  URLSearchParams()
  Object.keys(obj).forEach(key => {
    params.append(key, obj.key)
  })
  return params.toString()
}
```

通过以上代码对比，虽然代码量相差不多，带上明显后者实现更加优雅，而且不易出错，避免了操作字符串的各种问题