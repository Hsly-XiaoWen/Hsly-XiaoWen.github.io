---
layout: post
title: nodejs模块——fs模块
categories: [nodejs]
description: 记录下nodejs经常用到的一些模块及其具体用法
keywords: nodejs, 模块
---

## fs模块

fs模块用于对系统文件及目录进行读写操作，使用`require(‘fs’)`引入fs模块，模块中所有方法都包含`同步`和`异步`两种形式。
var fs = require('fs'); // 载入fs模块

	`fs.unlink('/tmp/shiyanlou', function(err) {
	    if (err) {
	        throw err;
	    }
	    console.log('成功删除了 /tmp/shiyanlou');
	});`
异步：有一个回调函数

### readFile读取文件

  1、 异步写法： `fs.readFile(filename,[option],callback)`
   
   readFile的回调函数`calback`接受两参数err、data，err是读取文件出错时触发的错误对象，data是从文件读取的数据内容。
   
   2、同步写法：`fs.readFile<font face="red">Sync</font>(filename,[options])`
 
  |option   | option对象，包含encoding，编码格式 | 该项是可选的|
  |:--:|:--|:--|
  
### writeFile写入文件

 `fs.writeFile(filenae,data,[option],callback(err))`
 
### fs.read和fs.write与前两者的区别

  功能类似，都是读写文件，但`fs.read`和`fs.write`提供更底层的操作，实际应用中多用前两者。
  
  使用fs.read、fs.write读写文件需要使用[fs.open和fs.close](http://www.cnblogs.com/starof/p/5038300.html)打开文件\关闭文件

  <font face="red">红色字体</font>

### 目录操作

1、创建目录

语法：`fs.mkdir(path,[mode],callback)`

删除目录：`fs.rmdir(path,callback)`，只能删除空目录

	|path|callback|
	|:--:|:--:|
	|[mode]|目录的权限（默认为0777）|
	|callback | 回调函数|
	
2、读取目录

语法：`fs.readdir(path,callback)`


fs模块详细介绍[看这里](http://www.cnblogs.com/starof/p/5038300.html)
