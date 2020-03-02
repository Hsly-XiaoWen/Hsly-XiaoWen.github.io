---
layout: post
title: Redis安装笔记
categories: Redis
description: Redis安装笔记
keywords: Redis
---

## Redis安装使用
Redis安装使用

1 下载安装包（出现wget命令不存在时:安装wget，yum install wget）
   
    wget http://download.redis.io/releases/redis-4.0.6.tar.gz
    
2 解压安装包 
    
    tar -zxvf redis-4.0.6.tar.gz
     
3 进入解压文件

    cd redis-4.0.6
4 编译安装文件：

    make
    
    make指令执行失败的时，安装配置环境：yum -y install gcc gcc-c++ libstdc++-devel

redis配置成服务启动：
    
     vim /usr/lib/systemd/system/redis.service

添加一下内容

    [Unit]
    Description=Redis
    After=syslog.target network.target remote-fs.target nss-lookup.target

    [Service]
    Type=forking
    ExecStart=/usr/local/redis/redis-4.0.6/src/redis-server /usr/local/redis/redis-4.0.6/redis.conf
    ExecReload=/bin/kill -s HUP $MAINPID 
    ExecStop=/bin/kill -s QUIT $MAINPID 
    PrivateTmp=true
 
     [Install]
     WantedBy=multi-user.target


###启动redis：

    /redis-4.0.6/src/redis-server 以守护线程的方式启动（保护当前进程，不能进行其他操作）
    /redis-4.0.6/src/reids-cli    打开redis客户端



### 远程访问linux需要修改redis的配置文件

1 打开redis端口，端口在redis.conf文件查看

2 注释掉redis.conf里面的bind 127.0.0.1

3 必须设置密码 打redis.conf 把requiredpass 注释去掉并后面加上密码

4 启动redis时后面跟上redis.conf配置文件

redis操作指令

    http://www.redis.net.cn/order
    
flushall 清空redis内的所有键值对

redis性能测试:redis自带了性能测试

    在redis../src/执行./redis-benchmark -n 1000 1000表示要操作的数据量