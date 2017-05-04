---
layout: post
title: CSS常见布局解决方案
categories: CSS
description: 各种布局的解决方案
keywords: CSS，布局
---

 >布局方式：标准文档流，浮动布局，定位布局，[flex](https://juejin.im/post/58e3a5a0a0bb9f0069fc16bb)布局
## 水平居中布局

1、margin + 定宽

	`<div class="parent">
	  <div class="child">Demo</div>
	</div>
	
	<style>
	  .child {
	    width: 100px;
	    margin: 0 auto;
	  }
	</style>`

2、table + margin

	<div class="parent">
	  <div class="child">Demo</div>
	</div>
	
	<style>
	  .child {
	    display: table;
	    margin: 0 auto;
	  }
	</style>

3、inline-block + text-align

	<div class="parent">
	  <div class="child">Demo</div>
	</div>
	
	<style>
	  .child {
	    display: inline-block;
	  }
	  .parent {
	    text-align: center;
	  }
	</style>

兼容性佳（兼容IE6、7）

4、absolute + margin-left

	<div class="parent">
	  <div class="child">Demo</div>
	</div>
	
	<style>
	.parent {
	    position: relative;
	  }
	  .child {
	    position: absolute;
	    left: 50%;
	    width: 100px;
	    margin-left: -50px;  /* width/2 */
	  }
	  </style>

宽度固定

相比于用transform，兼容性更好

5、absolute + transform

	<div class="parent">
	  <div class="child">Demo</div>
	</div>
	
	<style>
	  .parent {
	    position: relative;
	  }
	  .child {
	    position: absolute;
	    left: 50%;
	    transform: translateX(-50%);
	  }
	</style>
6、flex + justify-content

	<div class="parent">
	  <div class="child">Demo</div>
	</div>
	
	<style>
	  .parent {
	    display: flex;
	    justify-content: center;
	  }
	</style>

`flex`有兼容性问题

## 垂直居中

1、table-cell +vertical-align

	<style>
	  .parent {
	    display: table-cell;
	    vertical-align: middle;
	  }
	</style>

2、absolute + transform

	<style>
	  .parent {
	    position: relative;
	  }
	  .child {
	    position: absolute;
	    top: 50%;
	    transform: translateY(-50%);
	  }
	</style>

3、flex + align-items

	<style>
	  .parent {
	    display: flex;
	    align-items: center;
	  }
	</style>

## 水平垂直居中

1、absolute + transform

	<style>
	  .parent {
	    position: relative;
	  }
	  .child {
	    position: absolute;
	    left: 50%;
	    top: 50%;
	    transform: translate(-50%, -50%);
	  }
	</style>

2、inline-block + text-align +table-cell + vertical-align

	<style>
	  .parent {
	    text-align: center;
	    display: table-cell;
	    vertical-align: middle;
	  }
	  .child {
	    display: inline-block;
	  }
	</style>

3、[flex](https://juejin.im/post/58e3a5a0a0bb9f0069fc16bb) + justify-content +align-items

	<style>
	  .parent {
	    display: flex;
	    justify-content: center; /* 水平居中 */
	    align-items: center; /*垂直居中*/
	  }
	</style>

有兼容性问题

## 一列定宽，一列自适应

1、float + margin

	<div class="parent">
	  <div class="left">
	    <p>left</p>
	  </div>
	  <div class="right">
	    <p>right</p>
	    <p>right</p>
	  </div>
	</div>
	
	<style>
	  .left {
	    float: left;
	    width: 100px;
	  }
	  .right {
	    margin-left: 100px
	    /*间距可再加入 margin-left */
	  }
	</style>

2、float + overflow

	<style>
	  .left {
	    float: left;
	    width: 100px;
	  }
	  .right {
	    overflow: hidden;
	  }
	</style>

3、table

	<style>
	  .parent {
	    display: table;
	    width: 100%;
	    table-layout: fixed;
	  }
	  .left {
	    display: table-cell;
	    width: 100px;
	  }
	  .right {
	    display: table-cell;
	    /*宽度为剩余宽度*/
	  }
	</style>

4、flex

	<style>
	  .parent {
	    display: flex;
	  }
	  .left {
	    width: 100px;
	    margin-left: 20px;
	  }
	  .right {
	    flex: 1;
	  }
	</style>

性能问题，只适合小范围布局

低版本浏览器兼容问题

## 等分布局

1、float

	<div class="parent">
	  <div class="column">
	    <p>1</p>
	  </div>
	  <div class="column">
	    <p>2</p>
	  </div>
	  <div class="column">
	    <p>3</p>
	  </div>
	  <div class="column">
	    <p>4</p>
	  </div>
	</div>
	<style>
	  .parent {
	    margin-left: -20px;
	  }
	  .column {
	    float: left;
	    width: 25%;
	    padding-left: 20px;
	    box-sizing: border-box;
	  }
	</style>

2、flex

	<style>
	  .parent {
	    display: flex;
	  }
	  .column {
	    flex: 1;
	  }
	  .column+.column { /* 相邻兄弟选择器 */
	    margin-left: 20px;
	  }
	</style>

3、table

	<style>
	  .parent-fix {
	    margin-left: -20px;
	  }
	  .parent {
	    display: table;
	    width: 100%;
	    /*可以布局优先，也可以单元格宽度平分在没有设置的情况下*/
	    table-layout: fixed;
	  }
	  .column {
	    display: table-cell;
	    padding-left: 20px;
	  }
	</style>

## 等高布局

1、table

	<div class="parent">
	  <div class="left">
	    <p>left</p>
	  </div>
	  <div class="right">
	    <p>right</p>
	    <p>right</p>
	  </div>
	</div>
	
	<style>
	  .parent {
	    display: table;
	    width: 100%;
	    table-layout: fixed;
	  }
	  .left {
	    display: table-cell;
	    width: 100px;
	  }
	  .right {
	    display: table-cell
	    /*宽度为剩余宽度*/
	  }
	</style>

2、flex

	<style>
	  .parent {
	    display: flex;
	  }
	  .left {
	    width: 100px;
	    margin-left: 20px;
	  }
	  .right {
	    flex: 1;
	  }
	</style>

注意：实际使用了`align-tiems:stretch`，flex默认的`align-items`的值为`stretch`

3、float

	<style>
	  .parent {
	    overflow: hidden;
	  }
	  .left,
	  .right {
	    padding-bottom: 9999px;
	    margin-bottom: -9999px;
	  }
	  .left {
	    float: left;
	    width: 100px;
	    margin-right: 20px;
	  }
	  .right {
	    overflow: hidden;
	  }
	</style>
此方法为伪等高

原文出处为[XiNG](http://www.xingxin.me/posts/590058affd9e613545f2d1f3)






