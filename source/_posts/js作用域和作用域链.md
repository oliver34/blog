---
title: js作用域和作用域链
date: 2016-09-08 16:29:48
tags:
---

对于js的 **全局作用域** 和 **作用域链** 的相关概念一直很模糊，最近认真学习了一番。

对于大多数语言来说，作用域都分为**全局作用域** 和 **局部作用域** ，js亦是如此。

1. 全局作用域
在代码中处处都能够访问到的对象即拥有全局作用域，主要有以下几种情形拥有全局作用域：

  - 最外层函数和申明在函数外部的变量拥有全局作用域

```javascript 
var globalScope = "全局变量";

function outerFun(){
  var localScope = "局部变量";
  function innerFun(){
    console.log(localScope);
  }
  innerFun();
}
console.log(globalScope);//"全局变量"
console.log(localScope);//Uncaught ReferenceError: localScope is not defined
outerFun();//"局部变量"
innerFun();//Uncaught ReferenceError: innerFun is not defined
```

`globalScope` 和 `outerFun`都是定义在最外层的，所以拥有全局作用域。而 `localScope` 和 `innerFun`都是定义在函数内部的，所以访问会出错。

- 非严格模式下所有未定义直接赋值的变量自动变为全局变量，拥有全局作用域
``` javascript
function outerFun(){
  const localScope = "局部变量";
  globalScope = "全局变量";
  console.log(localScope);
}
outerFun();//"局部变量"
console.log(globalScope);//"全局变量"
console.log(localScope);//Uncaught ReferenceError: localScope is not defined
```

  当然前提这不是在 严格模式(use strict) 下,如果在严格模式下，未声明变量就赋值是会报错的。

- 所有window对象的属性拥有全局作用域
通常情况下window对象的内置属性都拥有全局作用域，下面是一下常用的window的属性

属性|描述
--|--
closed|返回窗口是否已被关闭。 
history|对 History 对象的只读引用。请参数 History 对象。
location|用于窗口或框架的 Location 对象。
name|设置或返回窗口的名称。
screenLeft screenTop screenX screenY|只读整数。声明了窗口的左上角在屏幕上的的 x 坐标和 y 坐标。IE、Safari 和 Opera 支持 screenLeft 和 screenTop，而 Firefox 和 Safari 支持 screenX 和 screenY。

2. 局部作用域
局部作用域又可以分为 函数作用域 和 块级作用域

函数作用域：变量在定义的函数内及嵌套的子函数内处处可见；
块级函数域：变量在离开定义的块级代码后马上被回收。
- 函数作用域
先看一段代码：
```js
var scope = 'global';
function getScope(){
	console.log(scope);//undefined
	var scope="local" ;
	console.log(scope);//local
};
getScope();
```

上面那段代码输出的结果是 `undefined` 和 `local`，第二个结果没有疑问，但是为什么第一个不是输出 `global`?不是定义了全局变量 `var scope = 'global'`为什么还会 undefined 呢？
这就是函数作用域 变量在定义的函数内及嵌套的子函数内处处可见 ，上面的代码在函数 getScope内部定义scope的时候，实际上进行了 变量提升(hoisting) ,所以上面的代码是与下面的代码等价的

``` js
var scope = 'global';
function getScope(){
	var scope;
	console.log(scope);//undefined
	scope="local" ;
	console.log(scope);//local
};
getScope();
```
- 块级作用域
js中依然存在块级作用域，比如 let 定义的变量遵从块级作用域，不会提升，不会在整个函数域内起作用。

2.作用域链
> 参考资料

[Js作用域与作用域链详解](http://blog.csdn.net/yueguanghaidao/article/details/9568071) 

[JavaScript 开发进阶：理解 JavaScript 作用域和作用域链](http://www.cnblogs.com/lhb25/archive/2011/09/06/javascript-scope-chain.html) 

[js的函数作用域跟块级作用域](http://blog.csdn.net/huangjq36sysu/article/details/51085674)