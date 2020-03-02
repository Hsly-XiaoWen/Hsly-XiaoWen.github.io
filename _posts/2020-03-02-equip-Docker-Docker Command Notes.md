---
layout: post
title: Docker简单入门
categories: Docker
description: Docker简单入门
keywords: Docker
---
Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的镜像中，
然后发布到任何流行的 Linux或Windows 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。

## Docker简单入门
docker简单入门的命令

下载安装docker

    yum install docker
    
开启docker服务

    systemctl start docker
    
关闭docker服务 

    systemctl stop docker
    
重启docker服务

    systemctl restart docker
    
设置docker为开机自启


    systemctl enable docker
    
查看docker的版本信息
     
     docker version

从远程下载镜像文件

    docker pull +镜像地址（从网易蜂巢查找）
    
删除本地车库的镜像
    
    docker rmi -f +镜像ID

查看本机所有的镜像文件：docker images

开启一个镜像容器：

    1 docker run +镜像名称(在前台进行）
    2 docker run -t +镜像ID
	docker run -d -p 端口号：端口号 镜像名称（开启应用用于外界访问镜像）
	
关闭镜像容器
     
     docker stop +镜像ID（可以是容器ID）
     
清理所有已经停止的容器：docker rm $(docker ps -a -q)

进去容器（相当于一台主机）：

     docker exec -it 容器id bash
     docker attach <ContainerID> （启动时后缀加了/bin/bash）
     
查看正在运行的镜像容器

     docker ps
     docker ps -a 查看所有镜像容器包括已经停止
     
重启已经停止的镜像容器

     docker start ContainerID

创建镜像文件Dockerfile格式：from +镜像（表示以该镜像为基础镜像）
                    
MAINTAINER +用户名 +邮箱

COPY 源文件 指定目录（把源文件拷贝到指定目录下）

COPY 源文件 指定目录（可以有多个）

RUN  在创建镜像时候要执行的软件安装命令（比如安装vim,允许多个）

例如：Tomcat镜像为基础镜像，要把web项目拷贝到tomcat的WebApps目录下

docker build .给当前目录的Dockerfile创建一个镜像

docker build -t +name .给当前镜像定义一个名称

docker commit ID new_image_name  保存对容器的修改（例如：修改了tomcat的配置文件会永远保存）

防火墙对docker的影响：docker开启的容器相当于虚拟机，使用网络桥接的方式和主机使用相同的网络
CentOS-7 中介绍了 firewalld，firewall的底层是使用iptables进行数据过滤，建立在iptables之上，这可能会与 Docker 产生冲突。
当 firewalld 启动或者重启的时候，将会从 iptables 中移除 DOCKER 的规则，从而影响了 Docker 的正常工作。
当你使用的是 Systemd 的时候， firewalld 会在 Docker 之前启动，但是如果你在 Docker 启动之后再启动 或者重启 firewalld ，
你就需要重启 Docker 进程了。  


## 安装一个centos 系统
    docker run -d -i -t <imageID> /bin/bash 启动一个一直停留在后台运行的Centos了。
	如果少了/bin/bash的话，Docker会生成一个Container但是马上就停止了，
	不会一致运行即使有了-d参数。
	docker attach <ContainerID> 进入安装的centos系统
如果没有ifconfig

yum search ifconfig  查询所需程序

yum install net-tools.x86_64 安装ifconfig

安装SSL协议

yum install -y openssh-server

/usr/sbin/sshd -D  启动SSL服务

