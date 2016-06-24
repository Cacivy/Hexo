---
title: unslider轮播插件
tags:
  - JavaScript
  - Chrome
categories: Code
date: 2016-01-14
---

# [unslider官网](http://unslider.com/)

这是一个(图片)轮播插件，小而轻。

顶部是该插件的官网链接，BootStrap中文网也做了一个中文站，但是并没有同步更新，下载跳转链接也是404，api是旧的，无法使用。

言归正传，这个js插件可以很好的实现轮播功能，而且可以灵活定制。

<!-- more -->

### 下载

1. [通过github](https://github.com/idiot/unslider)
点击Download ZIP
2. git命令(需要安装git bash)
```bash
git clone git@github.com:idiot/unslider.git
```
3. 使用[bower](http://bower.io/)(需要安装node.js),
这是Twitter推出的一个Web开发的包管理器，它的强大就不在这细说了。
首先安装bower，然后通过bower安装unslider，unslider就会出现在当前目录下
```bash
npm install -g bower
bower install unslider
```

### 使用

#### HTML

```HTML
<div class="my-slider">
	<ul>
		<li>My slide</li>
		<li>Another slide</li>
		<li>My last slide</li>
	</ul>
</div>
```
#### 引入需要的js类库和unslider插件

```javascript
<script src="//code.jquery.com/jquery-2.1.4.min.js"></script>
<script src="/path/to/unslider.js"></script>
```

#### 引入外部样式表(css)

```HTML
<link rel="stylesheet" href="/path/to/unslider/dist/css/unslider.css">
```

#### js代码

```javascript
<script>
  jQuery(document).ready(function($) {
    $('.my-slider').unslider();
  });
</script>
```

以上是官方给出的例子，很方便，同时可根据需要进行一些修改。

### 定制

主要介绍一些可定制的Options

```javascript
//以下可以根据自己需要进行修改
$('.my-demo-slider').unslider({
  autoplay: true,             //是否自动切换
   animation: 'fade',          // 动画效果 1.horizontal 2.vertical 3.fade
   speed: 300,               // 动画速度
   delay: 3000,              //  多少ms切换一次
   keys: false,               //  键盘控制
   nav: true,               //  是否显示导航
   arrows: false,            //   左右切换按钮是否显示
   fluid: false              //  是否自适应
});
```
