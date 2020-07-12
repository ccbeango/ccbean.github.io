---
title: BOM对象总结
date: 2020-05-15 09:32:10
categories: 
  - JavaScript
  - BOM
tag: 
  - JavaScript
  - BOM

---

# BOM对象总结（未完）

>  读《JavaScript高级程序设计》

ECMAScript是JavaScript的核心，但如果要在Web中使用JavaScript，那么BOM（浏览器对象模型）则无疑才是真正的核心。

BOM对象提供了很多对象，如window、location、navigator，用于访问浏览器的功能，这些功能与任何网页内容无关。location、navigator实际上都是window对象的属性。

W3C为了把浏览器中JavaScript最基本的部分标准化，已经将BOM的主要方面纳入了HTML5的规范中。

<!-- more -->

## window对象

BOM的核心对象是window，它表示浏览器的一个实例。在浏览器中，window对象有双重角色，它既是通过JavaScript访问浏览器窗口的一个接口，又是ECMAScript规定的Global对象。

### 全局作用域

由于window对象同时扮演着ECMAScript中Global对象的角色，因此所有在全局作用域中声明的变量、函数都会变成window对象的属性和方法。

```javascript
var age = 29;
function sayAge() {
  alert(this.age);
}

alert(window.age); // 29
sayAge(); // 29
window.sayAge(); // 29
```

在全局作用域中定义了一个变量`age`和一个函数`sayAge()`，它们被自动归在了window对象名下。于是，可以通过`window.age`访问变量`age`，可以通过`window.sayAge()`访问函数`sayAge()`。由于`sayAge()`在全局作用域中，因此`this.age`被映射到`window.age`，最终结果是一致的。

抛开全局变量会成为window对象的属性不谈，定义全局变量与在`window`对象上直接定义属性有一些差别：全局变量不能通过`delete`操作符删除，而直接在`window`对象上定义的属性可以。

```javascript
var age = 29; 
window.color = "red"; 

delete window.age; // return false
delete window.color; //returns true 

alert(window.age); //29 
alert(window.color); //undefined
```

因为通过`var`语句添加的window属性的数据属性`[[Configurable]]`值是`false`，因此不可删除。

要记住的一点是，尝试访问未声明的变量会抛出错误，但是通过查询window对象，可以知道某个可能未声明的变量是否存在。

```javascript
// 报错，因为oldValue未定义
var newValue = oldValue;

// 不报错，因为这是一次属性查询，newValue的值是undefined
var newValue = window.oldValue;
```

### 窗口关系及框架

如果页面中包含框架`frame`，则每个框架都拥有自己的window对象，并且保存再frames集合中，可以通过索引或框架名称来访问相应的window对象。每个window对象都有一个`name`属性，其中包含框架的名称。

现在基本已经不使用frame了。

### 窗口位置

用来确定和修改window对象位置的属性和方法有很多。IE、Safari、Opera、Chrome都提供了`screenLeft`和`screenRight`属性，分别用于表示窗口相对于屏幕左边和上边的位置。FireFox在`screenX`和`screenY`中提供，Safari和Chrome也同时支持这两个属性。

下面代码可实现跨浏览器获取窗口左边和上边的位置

```javascript
var leftPos = (typeof window.screenLeft == "number") ?  window.screenLeft : window.screenX; 
var topPos = (typeof window.screenTop == "number") ? 
 window.screenTop : window.screenY;
```

不同的浏览器对于上述属性的定义不同，跨浏览器无法实现获取准确的坐标值。

有两个函数也可以设置窗口位置，`moveTo()`和`moveBy()`，这两个方法都接受两个参数，其中`moveTo()`接收的是新坐标的x和y坐标值，而`moveBy()`接收的是在水平和垂直方向上移动的像素数。

```javascript
// 将窗口移动到屏幕左上角
window.moveTo(0,0); 
// 将窗口向下移动100像素
window.moveBy(0,100); 
// 将窗口移动到200,300
window.moveTo(200,300); 
// 将窗口向左移动50像素
window.moveBy(-50,0);
```

这两个方法可能被浏览器禁用，且不适用于框架，只能对最外层的window对象使用。

### 窗口大小

IE9+、FireFox、Safari、Opera和Chrome提供了四个属性，`innerWidth`、`innerHeigh`、`outerWidth`和`outerHeight`。不同浏览器中的定义也不同，IE9+、Safari和FireFox中，`outerWidth`和`outerHeight`返回浏览器窗口本身尺寸，`innerWidth`和`innerHeight`表示该容器中页面视图区的大小（减去边框的宽度）。在Chrome中，`innerWidth`、`innerHeigh`与`outerWidth`、`outerHeight`返回相同的值，即视口（viewport）大小而非浏览器窗口大小。

使用`resizeTo()`和`resizeBy()`也可以调整浏览器窗口大小。两个方法都接收两个参数，`resizeTo()`接收浏览器窗口的新宽度和新高度，`resizeBy()`接收新窗口与原窗口的宽度和高度之差。

```javascript
// 调整到100 100
window.resizeTo(100, 100);
// 调整到200 150
window.resizeBy(100, 50);
// 调整到300 300
window.resizeTo(300, 300);
```

这两个方法也可能被浏览器禁用，且不适用于框架，只能对最外层的window对象使用。

### 导航和打开窗口

现在函数查看变了，[在此](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/open)可查看。

`window.open()`方法可以导航到一个特定的URL，也可以打开一个新的浏览器窗口。

`window.open()`接收四个参数：

* 要加载的URL
* 窗口目标。窗口自定义名称或特殊的窗口名称`_self`、`_parent`、`_top`或`_blank`
* 特性字符串 一个逗号分隔的设置字符串，表示新窗口中都显示那些特性。
* 表示新页面是否取代浏览器历史记录中当前加载页面的布尔值

通常只需传递第一个参数，最后一个参数只在不打开新窗口的情况下使用。

如果`window.open()`传递第二个参数，而且该参数是已有窗口或框架名称，就会在具有该名称的窗口或框架中加载第一个参数指定的URL。

```javascript
// 等同于 < a href="http://www.wrox.com" target="topFrame"></a>
window.open("http://www.wrox.com/", "topFrame");
```

第三个参数在第二个参数并不是一个已经存在的窗口或框架时，会根据第三个参数位置上传入的字符串创建一个新窗口或标签页。如果没有传第三个参数，会打开一个全部默认参数的新浏览器窗口。

![BOM对象总结01](https://raw.githubusercontent.com/ccbeango/blogImages/master/HTML+CSS/BOM%E5%AF%B9%E8%B1%A1%E6%80%BB%E7%BB%9301.png)

```javascript
window.open("http://www.ccbeango.com/","wroxWindow", 
 "height=400,width=400,top=10,left=10,resizable=yes")
```

### 超时调用和间歇调用

JavaScript运行通过设置超时时间值和间歇时间值来调度代码在特定的时刻执行。

**超时调用**

超时调用需要使用window对象的`setTimeout()`方法，它接收两个参数，要执行的代码和以毫秒表示的时间。

```javascript
// 不推荐
setTimeout("alert('Hello world!') ", 1000);

// 推荐
setTimeout(function() { 
 alert("Hello world!"); 
}, 1000);
```

第二个参数是一个表示等待多长时间的毫秒数，但经过时间后指定的代码不一定会执行。JavaScript十一个单线程序的解释器，因此一定时间内只能执行一段代码。为了控制执行的代码，就有一个JavaScript任务队列。

这些任务会按照他们添加到队列的顺序执行。`setTimeout()`的第二个参数告诉JavaScript再过多长时间会把当前的任务添加到队列中。

如果队列是空的，那么会立即执行；如果不是，那么它要等待前面的代码执行完了以后再执行。