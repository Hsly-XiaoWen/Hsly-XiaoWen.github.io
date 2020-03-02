---
layout: post
title: Nginx安装笔记
categories: Nginx
description: Nginx安装笔记，Keepalived+Nginx 配置高可用+负载均衡
keywords: Nginx
---

使用nginx实现负载均衡，使用Keepalived搭建高可用nginx集群


## Nginx安装步骤
1 前置环境安装
安装gcc yum install -y gcc-c++
安装pcre 是用正则表达式库，http需要使用pcre解析正则表达式
yum install -y pcre prce-devel
安装zlib zlib提供多种压缩方式，nginx使用的zlib对http内容进行压缩
yum install -y zlib zlib-devel
安装openSSL 安全套接字层密码库
yum install -y openssl opensll-devel

2 安装Nginx
下载nginx安装包
wget http://nginx.org/download/nginx-1.9.14.tar.gz
解压nginx安装包
tar -zxvf nginx-1.9.14.tar.gz
cd nginx-1.9.14 进入nginx解压文件
./configure --prefix=/usr/local/nginx  运行解压文件下的configure文件安装
                                       --prefix安装文件前缀，文件安装目录下
make       编译 （make的过程是把各种语言写的源码文件，变成可执行文件和各种库文件）
make install 安装 （make install是把这些编译出来的可执行文件和库文件复制到合适的地方）

启动nginx
安装目录下/sbin/nginx 启动nginx
安装目录下/sbin/nginx -s reload 重启nginx
安装目录下/sbin/nginx -s stop   关闭nginx


配置nginx
安装目录下/conf/nginx/conf
http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
    //配置负载均衡点，指定的ip集合负载均衡提高服务
    upstream tomcats{
	  server 118.89.29.12:88 weight=2;
	  server 118.89.29.12:90;
	}

    server {
        //监听端口
        listen       80;
	//服务器名称
        server_name  localhost;
       
        location / {
            proxy_pass  http://tomcats;//反向代理，由该指定的ip提高服务
	    index  index.html index.htm;//初始化界面
        }
}

keepalived:1 定时检查nginx是否在使用
           2 虚拟出一个ip地址共多台nginx服务器使用，达到高可用作用
	     一台为master，一台为backup 一台宕机另一台顶上

基于centos7.3安装keepalived
keepalived源码文件下载：http://www.keepalived.org/software/keepalived-1.2.20.tar.gz
wget http://www.keepalived.org/software/keepalived-1.2.20.tar.gz

1 解压文件：tar -zxvf keepalived-1.2.20.tar.gz
2 进入解压文件:cd keepalived-1.2.20
3 执行文件：./configure --prefix=/usr/local/keepalived/ 指定安装程序位置
4 编译与安装：make && make install
安装完成，进行相关配置
创建文件：mkdir /etc/keepalived/
拷贝文件：cp /usr/local/keepalived/etc/keepalived/keepalived.conf /etc/keepalived/keepalived.conf
          cp /usr/local/keepalived/etc/rc.d/init.d/keepalived /etc/rc.d/init.d/keepalived
	  cp /usr/local/keepalived/etc/sysconfig/keepalived /etc/sysconfig/keepalived

###编写 Nginx 状态检测脚本 
      /etc/keepalived/nginx_check.sh
      !/bin/bash
      A=`ps -C nginx ὀ~Sno-header |wc -l`
      if [ $A -eq 0 ];then
          /usr/local/nginx/sbin/nginx #nginx启动文件的位置
          sleep 2
      if [ `ps -C nginx --no-header |wc -l` -eq 0 ];then
        killall keepalived
         fi
      fi

###修改/etc/keepalived/keepalived.conf
global_defs {
  router_id 118.89.29.12
}
vrrp_script chk_nginx {
   script "/etc/keepalived/nginx_check.sh"
   interval 2
   weight -20
}
vrrp_instance VI_1 {
   state MASTER #主nginx配置master ，从nginx服务器配置为backup
   interface eth0#网卡位置，eth0/eth1/eth2
   virtual_router_id 11 #任意给，主从nginx服务器该值要保持一致
   mcast_src_ip 118.89.29.12 #本机ip地址，公网地址
   priority 100 #优先级
   advert_int 1
   authentication {
      auth_type PASS
      auth_pass 1111
   }
   track_script {
      chk_nginx #nginx检测脚本信息
   }
    virtual_ipaddress {
      118.89.29.12 #keepalived虚拟出来的ip地址，必须同一网段下有效
   }
}

启动keepalived：systemctl start keepalived
关闭keepalived：systemctl stop keepalived
重启keepalived：systemctl restart keepalived
设置为开机自启：systemctl enable keepalived
注意：启动keepalived时候会检测nginx是否启动，如果没有启动就自动启动nginx