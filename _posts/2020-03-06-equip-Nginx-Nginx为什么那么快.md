---
layout: post
title: Nginx
categories: nginx
description: nginx为什么
keywords: Nginx
---

Nginx 是一款轻量级的Web服务器、反向代理服务器，由于它的内存占用少，启动极快，高并发能力强，在互联网项目中广泛应用。
采用的是多进程（单线程） & 多路IO复用模型，使用的epoll模型的并发时间驱动型服务器。

### Nginx的工作模式
   Nginx是多进程模式，master-worker模式。一个master多个worker。
   
   ![](/images/posts/nginx/master-worker.png)
   
   master进程
   
    读取并验证配置文件nginx.conf
    监控 worker 进程的运行状态，当 worker 进程退出后(异常情况下)，会自动启动新的 worker 进程。
   
   worker进程
   
     每一个Worker进程都维护一个线程（避免线程切换），处理连接和请求；
     注意Worker进程的个数由配置文件决定，一般和CPU个数相关（避免进程切换），配置几个就有几个Worker进程。
   
   当一个 worker 进程在 accept() 这个连接之后，就开始读取请求，解析请求，处理请求，产生数据后，再返回给客户端，最后才断开连接，一个完整的请求。一个请求，
   完全由 worker 进程来处理，而且只能在一个 worker 进程中处理。
   
   采用io多路复用epoll模型。epoll模型基于事件驱动机制，它可以监控多个事件是否准备完毕，
   如果OK，那么放入epoll队列中，这个过程是异步的。worker只需要从epoll队列循环处理即可。
   
   >epoll模型 与 select模型 相比最大的优点是不会随着 sockfd 数目增长而降低效率，使用 select() 时，内核采用轮训的方法来查看是否有fd 准备好，
   其中的保存 sockfd 的是类似数组的数据结构 fd_set，key 为 fd，value 为 0 或者 1。
   
   基于该工作模式，有以下的优点：
   
   
    nginx的多进程以及io多路复用epoll模型，保证了nginx可以处理10w+(理论上取决于内存而不是linux文件句柄)的用户请求。
   
    master-worker模式保证了nginx的高可靠性。当其中一个worker退出时候，master马上会重新启动新的worker进程。多个worker相不影响。
   
    nginx 处理请求是异步非阻塞的，一个请求全程由一个进程一个线程完成，多路复用
    
    高度模块化的设计，编写模块相对简单；
   
### Nginx的热部署
   修改配置文件nginx.conf后，重新生成新的worker进程，当然会以新的配置进行处理请求，而且新的请求必须都交给新的worker进程，
   至于老的worker进程，等把那些以前的请求处理完毕后，kill掉即可。
   
### Nginx的高可用
   Keepalived+Nginx实现高可用。使用keepalived主要是用来防止服务器单点发生故障，可以通过和Nginx配合来实现Web服务的高可用
   
### Apache 和 Nginx对比
   
   1 nginx和Apache的核心区别：Apache是同步多进程模式，一个连接一个进程，采用select模型，受文件句柄限制。
   nginx是异步的，多个连接（万级别）可以对应一个进程
   
   2 nginx的配置比apache简单
   
   3 相比 Apache，Nginx 使用更少的资源，支持更多的并发连接，体现更高的效率。
   
   4 Apache在动态请求方面比nginx好，nginx在静态请求方方面比apach好
   
   5 [更多参考](https://www.cnblogs.com/cunkouzh/p/5410154.html)
   

   
   
   
   



