---
title:  前端项目中的RESTful API管理
date: 2017-08-07
---

## RESTful API

```http
/get /users
/getByID /users/:id
/post /users [body]
/put /users/:id [body]
/patch /users/:id [body]
/delete /users/:id
```

以上是一些基本规范，但REST不是银弹，比如`批量处理`、`getuser`、`login`

<!--more-->

## Mock Server

前端独立于后端，除了约定API，还需要`mock server`
一般使用[json-server](https://github.com/typicode/json-server)配合[faker.js](https://github.com/Marak/faker.js)
接口测试则是通过`postman`，并且可以生成文档、分享等

```js
// index.js
const jsonServer = require('json-server')
const path = require('path')
const server = jsonServer.create()
const data = require('./data')
//path.join(__dirname, 'db.json')
const router = jsonServer.router(data())
const middlewares = jsonServer.defaults()

function resData(data={}, code=200, msg='') {
  return {
    code,
    data,
    msg
  }
}

// Custom output
router.render = (req, res) => {
  res.jsonp(resData(res.locals.data, res.statusCode))
}

// Set default middlewares (logger, static, cors and no-cache)
server.use(middlewares)

// To handle POST, PUT and PATCH you need to use a body-parser
// You can use the one used by JSON Server
server.use(jsonServer.bodyParser)
server.use((req, res, next) => {
  // if (req.method === 'POST') {
  //   req.body.createdAt = Date.now()
  // }
  next()
})

let session

server.get('/api/user', (req, res) => {
  if (session) {
    res.jsonp(resData(session, 200))
  } else {
    res.jsonp(resData(undefined, 403, '未登录'))
  }
})

server.post('/api/login', (req, res) => {
  let body = req.body, code = 302, data = {}, msg = '密码错误'
  if (body.username === 'admin' && body.password === 'admin') {
    msg = ''
    code = 200
    data = req.body
    data.id = 1
    delete data.password
    session = data
  }
  res.jsonp(resData(data, code, msg))
})

server.get('/api/logout', (req, res) => {
  session = null
  res.jsonp(resData({}, 200, '注销成功'))
})

// Use default router
server.use('/api', router)
const port = 9999
server.listen(port, () => {
  console.log('JSON Server is running, port: '+ port)
})
```

```js
// data.js
const faker = require('faker')
faker.locale = 'zh_CN'

module.exports = () => {
  const data = {
    users: [],
    roles: [],
    departments: []
  }
  // Create 100users
  for (let i = 0; i < 100; i++) {
    data.users.push({
      id: i,
      username: faker.name.findName(),
      nikename: faker.name.findName(),
      freezeState: faker.random.boolean(),
      freezeInfo: faker.lorem.sentence()
    })
  }
  for (let i = 0; i < 28; i++) {
    data.roles.push({
      id: i,
      no: faker.name.jobType(),
      name: faker.name.jobType(),
      type: faker.name.jobType()
    })
    data.departments.push({
      id: i,
      name: faker.commerce.department(),
      description: faker.commerce.productAdjective()
    })
  }
  return data
}
```

## API Manage

这里使用axios作为http请求库

```js
// index.js
import instance from './instance'

const apiConfig = {
  login: ['get', 'post'],
  logout: ['get'],
  user: ['get'],
  users: [],
  roles: [],
  departments: []
}

var API = {}

const defaultMethods = ['get', 'post', 'put', 'patch', 'delete']

const getMethod = (key, method) => {
  switch (method) {
  case 'get':
  {
    return (params) => instance.get(key, {
      params
    })
  }
  case 'getById':
  {
    return (id, params) => instance.get(`${key}/${id}`, {params})
  }
  case 'post':
  {
    return (body) => instance.post(key, body)
  }
  case 'put':
  {
    return (id, body) => instance.put(`${key}/${id}`, body)
  }
  case 'patch':
  {
    return (id, body) => instance.patch(`${key}/${id}`, body)
  }
  case 'delete':
  {
    return (id) => instance.delete(`${key}/${id}`)
  }
  default:
    break
  }
}

for (let key of Object.keys(apiConfig)) {
  let list = apiConfig[key].length ? apiConfig[key] : defaultMethods
  list.forEach(method => {
    API[key] = API[key] || {}
    API[key][method] = getMethod(key, method)
  })
}

export default API
```



```js
// instance.js
import { httpConfig as host } from 'config'
import axios from 'axios'
import { message } from 'antd'

const instance = axios.create({
  baseURL: host,
  timeout: 10000,
  withCredentials: true,
  headers: {},
  validateStatus (status) {
    return status >= 200 && status <= 500 // default
  }
})

// 全局request钩子
axios.interceptors.request.use(function (config) {
  return config
}, function (error) {
  return Promise.reject(error)
})

// 全局response钩子
instance.interceptors.response.use(function (response) {
  // 请求失败
  if (response.status.toString().substr(0, 1) !== '2') {
    return Promise.reject(response.data)
  }
  // code 302 登录失败
  if (response.data.code === 302) {
    return Promise.reject(response.data)
  }
  // code 403 无权限
  if (response.data.code === 403) {
    return Promise.reject(response.data)
  }
  // 其它错误
  if (response.data.code.toString().substr(0, 1) !== '2') {
    return Promise.reject(response.data)
  }
  let count = response.headers['x-total-count']
  if (count !== undefined) {
    response.data.count = count
  }
  return response.data
}, (error) => {
  if (error.message.indexOf('timeout') > -1) {
    message.error('请求超时！')
  }
  return Promise.reject(error)
})

export default instance
```

```js
// use

try {

  let res = await API.login.post()

  console.log(res)

} catch (error) {

  console.log(error)

}

// or

API.login.post().then(res => {

  console.log(res)

})
```

以上就实现了api统一管理，并通过API对象下不同接口子对象进行调用