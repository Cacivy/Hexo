---
title: My Resume
date: 2016-06-13
---

### 个人信息

```js
姓名: 詹韬
手机: 13161472239
年龄: 21
邮箱: 609448234@qq.com
Github: http://github.com/cacivy
Blog: http://blog.cacivy.com/
```

<!-- more -->

### 技术栈

+ 前端

    熟练 JQuery AvalonJS VueJS WebPack ES6 BootStrap CSS3

    了解 MVVM(Vue | Avalon |Augular) Components(React | Vue | Vuex) Gulp Yeoman Fetch Promise

+ 后端

    熟练 WebForm Asp.Net MVC 多线程 Cors跨域

    了解 反射 Delegate NodeJS Express Solr

+ ​数据库

    熟练 MySQL

    了解 Redis MongoDB


### 项目经验

#### 某汽车电商前台

  负责前端优化，页面需求实现，以及后期维护

  使用到的技术 jQuery.lazy AvalonJS 函数节流 transition unslider BootStrap

> 1.首屏生成纯静态页面，优化页面速度  
> 2.保养页、配件选择页面使用AvalonJS，实现类购物车功能、并使store.js缓存数据
> 3.购物车页面实时保存MongoDB，使用函数节流，添加点击态，带来更好的用户体验
> 4.安装店页面使用百度地图API，并使用Geolocation实现地理定位，并计算最近的安装店以及其它距离信息。因为安装店太多使用jQuery.lazy加载安装店图片，节省资源
> 5.使用gulp合并压缩每个页面的js，节省http请求

#### POP商家平台

  负责前后台的架构及开发，一期独立完成

> 1.最初因为兼容问题选择了AvalonJS，一个很实用的mvvm框架，而且可以兼容到IE6，性能也不错。用来开发了商品管理模块，具体有多下拉框联查、通过table添加商品、弹出框手风琴控件展示数据等，上线半年稳定。
> ​2.第二期使用了AngularJS，对应MVC框架的Controller，主要是CURD页面，通过扩展可以满足很多需求，如w5cvalidator、angular-router，但是感觉不如Avalon简单清晰

#### Saas云平台

  负责供应商端项目开发以及整体项目的搭建及开发，并帮其它项目成员编写可复用的Components

  使用Asp.Net MVC + VueJS进行前后端开发，后端主要提供数据接口。

> 1.使用Vue-cli 以及 Vue-Strap、vue-resource进行前端的搭建，并根据业务需求实现其它组件，高定制化，可打包给其它项目使用
> 2.因为并不是纯前端开发，所以并没有使用Vue-router和Vuex，而且使用webpack单页面打包
> 3.项目开发难点主要是一些组件的开发，实现了列入上传、分页、列表等通用控件

#### 知乎日报

​  根据知乎日报的api实现列表展示以及文章查看

​  通过vue-cli搭建的一个spa项目，并使用vue-route + vuex

​  项目地址：https://github.com/Cacivy/zhihudaily

> 1.使用node.js代理调用知乎api解决跨域问题，并用vue-resource进行请求
> 2.列表页轮播图使用swiper封装的vue组件，展示时间使用momentjs库，整体组件切换添加transition动画
> 3.使用sass预处理器，并使用normalize.css实现css reset

### 工作经验

北京北迈科技股份有限公司 2014/09/20-至今

### 工作期望

  希望能接触到更多的前端技术，和一群热爱技术的人共事，并且有良好的工作氛围。
