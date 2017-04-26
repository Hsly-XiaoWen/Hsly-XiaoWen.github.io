---
layout: post
title: TCP协议及状态码笔记
categories: 计算机网络
description: TCP协议，HTTP。
keywords: TCP，HTTP
---

研一的时候看了几本关于协议的书，感觉看得还蛮深的，后面做大数据方面的，也没用上多少，没想到现在已经忘得差不多了，没办法最近又重新翻出来，温故知新啊。

## 协议

jawil的[通俗大白话讲解TCP协议的三次握手和四次分手](https://github.com/jawil/blog/issues/14)，写得比较详细，可以看下。

## HTTP状态码

### HTTP状态码分类

HTTP状态码被分为五大类，目前我们使用的HTP协议版本是1.1。

|                |     已定义范围      |        分类      |  
| :----------: | :------------------: |--------------: |
| 1XX         | 100-101              |信息提示       |
|2XX|200-206|成功|
|3XX|300-305|重定向|
|4XX|400-415|客户端错误|
|5XX|500-505|服务器错误|

### 常见状态码

|200 OK 服务器成功处理了请求 |
|:---|
|301/302 Moved Permanently（重定向） 请求的URL已移走Response中应该包含一个Location URL，说明资源现在所处位置|
|304 Not Modified（未修改） 客户的缓存资源是最新的，要客户端使用缓存|
|404 Not Found 未找到资源|
|501 Internal Server Error 服务器遇到一个错误，使其无法对请求提供服务|

[HTTP状态码作用](http://caibaojian.com/http-status-code.html)有介绍详细每个状态码的具体含义。