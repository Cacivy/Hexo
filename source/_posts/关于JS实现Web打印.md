---
title: 关于JS实现Web打印
tags:
  - JavaScript
  - Chrome
categories: Code
date: 2016-01-01
---


首先Js本身就有调用浏览器打印的方法
```JavaScript
window.print();
```
以上代码即可实现当前页打印，但是如果我们要打印页面局部内容呢？

<!-- more -->

### html插入标签+JS原生实现
```JavaScript
<!DOCTYPE html> 
<html> 
<head> 
<title>打印预览简单实现</title> 
</head> 
<body> 
<div>
这是body 里的内容不需要打印，具体的页面设计根据自己的要求自行设计。如果需要一个页面多个tag,可以动态生成tag  
</div> 
<!--startprint-->
<div> 
这是我需要打印的内容 
</div> 
<!--endprint-->
<script type="text/javascript"> 
function preview() 
{ 
var bdhtml=window.document.body.innerHTML;//获取当前页的html代码 
var startStr="<!--startprint-->";//设置打印开始区域 
var endStr="<!--endprint-->";//设置打印结束区域 
var printHtml=bdhtml.substring(bdhtml.indexOf(startStr)+startStr.length,bdhtml.indexOf(endStr));//从标记里获取需要打印的页面 
 
window.document.body.innerHTML=printHtml;//需要打印的页面 
window.print(); 
window.document.body.innerHTML=bdhtml;//还原界面 
} 
preview(); 
</script> 
</body> 
</html>
```
上面的代码很巧妙的实现了局部打印，还可以使用iframe等方法，但是相对比较繁琐。
这里jQuery也提供了插件可以实现同样的效果
### jQuery插件实现
1、下载jquery.PrintArea.js。
2、调用jquery.min.js和jquery.PrintArea.js
3、调用printArea()方法。
也是比较简便的方法。

好，局部打印已经实现了，如果想要更灵活的展示
```CSS
@media print {
  #nav-area {display: none;}
  }
```
可以使用以上代码隐藏响应元素。

ps:Chrome浏览器存在局部打印预览失败的问题，可以尝试加上以下样式
```CSS
position: fixed;
top: 0;
left: 0;
margin: 0;
```

