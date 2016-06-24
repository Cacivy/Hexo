---
title: JavaScript高级程序设计
tags:
  - JavaScript
categories: Code
date: 2016-01-03
---
## 基本概念

### 3.6.6　label语句
使用label语句可以在代码中添加标签，以便将来使用。以下是label语句的语法：

``` js
label: statement
```

下面是一个示例：

``` js
start: for (var i=0; i < count; i++) {
    alert(i);
}
```

这个例子中定义的start标签可以在将来由break或continue语句引用。加标签的语句一般都要与for语句等循环语句配合使用。

<!-- more -->

### 3.6.8　with语句
with语句的作用是将代码的作用域设置到一个特定的对象中。with语句的语法如下：
with (expression) statement;
定义with语句的目的主要是为了简化多次编写同一个对象的工作，如下面的例子所示：

``` js
var qs = location.search.substring(1);

var hostName = location.hostname;
var url = location.href;
```

上面几行代码都包含location对象。如果使用with语句，可以把上面的代码改写成如下所示：

``` js
with(location){
    var qs = search.substring(1);
    var hostName = hostname;
    var url = href;
}
```
WithStatementExample01.htm
在这个重写后的例子中，使用with语句关联了location对象。这意味着在with语句的代码块内部，每个变量首先被认为是一个局部变量，而如果在局部环境中找不到该变量的定义，就会查询location对象中是否有同名的属性。如果发现了同名属性，则以location对象属性的值作为变量的值。

<div class="tip">
 严格模式下不允许使用with语句，否则将视为语法错误。
 由于大量使用with语句会导致性能下降，同时也会给调试代码造成困难，因此在开发大型应用程序时，不建议使用with语句。
</div>

### 5.2.8　迭代方法
ECMAScript 5为数组定义了5个迭代方法。每个方法都接收两个参数：要在每一项上运行的函数和（可选的）运行该函数的作用域对象——影响this的值。传入这些方法中的函数会接收三个参数：数组项的值、该项在数组中的位置和数组对象本身。根据使用的方法不同，这个函数执行后的返回值可能会也可能不会影响访问的返回值。以下是这5个迭代方法的作用。

+ every()：对数组中的每一项运行给定函数，如果该函数对每一项都返回true，则返回true。
+ filter()：对数组中的每一项运行给定函数，返回该函数会返回true的项组成的数组。
+ forEach()：对数组中的每一项运行给定函数。这个方法没有返回值。
+ map()：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
+ some()：对数组中的每一项运行给定函数，如果该函数对任一项返回true，则返回true。

<div class='tip'>
以上方法都不会修改数组中的包含的值。
</div>

在这些方法中，最相似的是every()和some()，它们都用于查询数组中的项是否满足某个条件。对every()来说，传入的函数必须对每一项都返回true，这个方法才返回true；否则，它就返回false。而some()方法则是只要传入的函数对数组中的某一项返回true，就会返回true。请看以下例子。

``` js
var numbers = [1,2,3,4,5,4,3,2,1];

var everyResult = numbers.every(function(item, index, array){
    return (item > 2);
});

alert(everyResult);     //false

var someResult = numbers.some(function(item, index, array){
    return (item > 2);
});

alert(someResult);      //true
```

以上代码调用了every()和some()，传入的函数只要给定项大于2就会返回true。对于every()，它返回的是false，因为只有部分数组项符合条件。对于some()，结果就是true，因为至少有一项是大于2的。
下面再看一看filter()函数，它利用指定的函数确定是否在返回的数组中包含的某一项。例如，要返回一个所有数值都大于2的数组，可以使用以下代码。

```js
var numbers = [1,2,3,4,5,4,3,2,1];

var filterResult = numbers.filter(function(item, index, array){
    return (item > 2);
});

alert(filterResult);       //[3,4,5,4,3]
```

这里，通过调用filter()方法创建并返回了包含3、4、5、4、3的数组，因为传入的函数对它们每一项都返回true。这个方法对查询符合某些条件的所有数组项非常有用。
map()也返回一个数组，而这个数组的每一项都是在原始数组中的对应项上运行传入函数的结果。例如，可以给数组中的每一项乘以2，然后返回这些乘积组成的数组，如下所示。

```js
var numbers = [1,2,3,4,5,4,3,2,1];

var mapResult = numbers.map(function(item, index, array){
    return item * 2;
});

alert(mapResult);   //[2,4,6,8,10,8,6,4,2]
```

以上代码返回的数组中包含给每个数乘以2之后的结果。这个方法适合创建包含的项与另一个数组一一对应的数组。
最后一个方法是forEach()，它只是对数组中的每一项运行传入的函数。这个方法没有返回值，本质上与使用for循环迭代数组一样。来看一个例子。

```js
var numbers = [1,2,3,4,5,4,3,2,1];

numbers.forEach(function(item, index, array){
    //执行某些操作
});
```

这些数组方法通过执行不同的操作，可以大大方便处理数组的任务。支持这些迭代方法的浏览器有IE9+、Firefox 2+、Safari 3+、Opera 9.5+和Chrome。
