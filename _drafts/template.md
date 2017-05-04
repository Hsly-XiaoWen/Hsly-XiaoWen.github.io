---
layout: post
title: 真正认识JS
categories: JS
description: 闭包的一些注意事项
keywords: 闭包，执行上下文
---

## Tips

1、查找所有变量声明，并初始化为undefined。

2、在进入执行上下文时，首先会处理函数变量声明。如果变量名称跟已经声明的形式参数或函数相同，则变量声明不干扰已存在的这类属性。

     换句话说：函数声明会覆盖变量变量声明，但不会覆盖变量赋值
     
     函数声明的优先级高于变量声明的优先级，但函数变量被赋值了就不一样了。

3、变量声明、函数声明会被前置，但是函数表达示并不会，准确说类似变量声明前置。

4、具名的函数表达示的名字只能在该函数内部取到

	`var foo = function bar () {}
	
	console.log('foo', foo); 
	// foo function bar(){}
	
	console.log('bar', bar);
	// Uncaught ReferenceError: bar is not defined
	

5、关于`a.call(null)`

如果第一个参数传入的对象调用者是`null`或者`undefined`的话，`call`方法将把全局对象（浏览器上是`window`对象）作为this的值。所以，不管你什么时候传入`null`或者` undefined`，其`this`都是全局对象`window`。所以，在浏览器上答案是输出`window`对象。

但是但是但是，我们依旧不能忘记一个特殊情况–`严格模式`，在严格模式中，`null `就是`null` ，`undefined `就是 `undefined `
