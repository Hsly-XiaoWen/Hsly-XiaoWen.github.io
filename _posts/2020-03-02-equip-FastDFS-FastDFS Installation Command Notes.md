---
layout: post
title: FastDFS安装笔记
categories: FastDFS
description: FastDFS安装笔记
keywords: FastDFS
---
FastDFS：是一个开源的轻量级分布式文件系统，由跟踪服务器（tracker server）、存储服务器（storage server）和客户端（client）三个部分组成，
主要解决了海量数据存储问题

Tracker主要做调度工作，相当于mvc中的controller的角色，在访问上起负载均衡的作用。
跟踪器和存储节点都可以由一台或多台服务器构成，跟踪器和存储节点中的服务器均可以随时增加或下线而
不会影响线上服务，其中跟踪器中的所有服务器都是对等的，可以根据服务器的压力情况随时增加或减少。

Tracker负责管理所有的Storage和group，每个storage在启动后会连接Tracker，告知自己所属的group等信息，并保持周期性的心跳，tracker根据storage的心跳信息，建立group==>[storage server list]的映射表，Tracker需要管理的元信息很少，会全部存储在内存中；另外tracker上的元信息都是由storage汇报的信息生成的，本身不需要持久化任何数据，这样使得tracker非常容易扩展，直接增加tracker机器即可扩展为tracker cluster来服务，cluster里每个tracker之间是完全对等的，所有的tracker都接受stroage的心跳信息，生成元数据信息来提供读写服务。

Storage采用了分卷[Volume]（或分组[group]）的组织方式，存储系统由一个或多个组组成，
组与组之间的文件是相互独立的，所有组的文件容量累加就是整个存储系统中的文件容量。
一个卷[Volume]（组[group]）可以由一台或多台存储服务器组成，
一个组中的存储服务器中的文件都是相同的，组中的多台存储服务器起到了冗余备份和负载均衡的作用，
数据互为备份，存储空间以group内容量最小的storage为准，所以建议group内的多个storage尽量配置相同，
以免造成存储空间的浪费。

## 安装步骤
资源下载：https://github.com/happyfish100/fastdfs/releases

需要文件：libfastcommon-master.zip,fastdfs-5.*.tar.gz

### 安装libfastcommon

1 解压libfastcommon-master.zip文件
  
     unzip libfastcommon-master.zip

  如果报没有unzip command，使用yum install -y unzip下载unzip命令库
  
2 进入解压后的文件：cd libfastcommon-master 

  执行ll命令会发现可执行文件make.sh
  
3 执行make.sh编译程序：./make.sh。如果编译失败，可能缺少gcc编译器：
  
    yum install -y gcc-c++
    if make common not find ：yum install -y make
  
3 安装libfastcommon：./make.sh install

  libfastcommon.so 默认安装到了/usr/lib64/libfastcommon.so。
  
  因为fastdfs的主程序设置安装到/usr/lib/下：cp /usr/lib64/libfastcommon.so /usr/bin/ (如果已有可以不要复制)
  
到此libfastcommon安装完毕


### 安装fastdfs
1 解压文件：tar -zxvf fastdfs-5.*.tar.gz

2 进入解压文件：cd fastdfs-5.*
  执行ll 会看到configure可执行文件
  
3 编译文件：./configure

4 安装：./configure install

  安装会创建三个文件：/usr/local/fastdfs-5.05/(解压文件夹)，/etc/fdfs/(fastdfs配置文件)，
                      /usr/lib/（fastdfs主程序命令所在地）
                      
5 把解压文件下conf文件夹下的文件拷贝到/etc/fdfs/下（图片可以不用）
   
    cp /usr/local/fastdfs-5.05/conf/*.conf /etc/fdfs/
    cp /usr/local/fastdfs-5.05/conf/mime.types /etc/fdfs/
  
安装fastDFS安装完成

## fastdfs tracker，storage，client配置
Tracker配置:修改/etc/fdfs/tracker.conf文件

1 创建用于存储Tracker的数据文件和日志文件的文件夹

    mkdir -p /usr/local/fdfs/tracker (-p递归创建文件夹)
  
2 vim /etc/fdfs/tracker.conf

    需要做的修改：port=22122 #设置tracker的端口号，通常采用22122这个默认端口
                base_path=/usr/local/fdfs/tracker #设置tracker的数据文件和日志目录
		http.server_port=6666 #设置http端口号，默认为8080
		
  修改后保存退出
  
3 启动tracker 

    /usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf
    
  重启tracker /usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf restart
  
  停止tracker /usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf stop
  
  确认启动 如果有fdfs_trackerd说明启动成功
    
    ps -aux |grep fdfs

### storage配置：配置/etc/fdfs/storage.conf
1 创建storage需要的文件夹

    mkdir /usr/local/fdfs/storage       用于storage日志文件存储地址
    mkdir /usr/local/fdfs/storage_data  真正的文件存储路径
    
2 vim /etc/fdfs/storage.conf

    修改文件：disabled=false #启用配置文件（默认启用）
    group_name=group1 #组名，根据实际情况修改
    port=23000 #设置storage的端口号，默认是23000，同一个组的storage端口号必须一致
    base_path=/usr/local/fdfs/storage #设置storage数据文件和日志目录
    store_path_count=1 #存储路径个数，需要和store_path个数匹配
    store_path0=/usr/local/fdfs/storage_data #实际文件存储路径
    tracker_server=192.168.111.11(公网ip地址):22122 # tracker服务器的 IP地址和端口号，如果是单机搭建，IP不要写127.0.0.1，否则启动不成功（此处的ip是我的CentOS虚拟机ip）
    http.server_port=8888 #设置 http 端口号
    
3 启动tracker /usr/bin/fdfs_storaged /etc/fdfs/storage.conf 必须先启动tracker,storage才能启动成功

  重启tracker /usr/bin/fdfs_storaged /etc/fdfs/storage.conf restart
  
  停止tracker /usr/bin/fdfs_storaged /etc/fdfs/storage.conf stop
  
  ps -aux |grep fdfs 确认启动 如果有fdfs_storaged说明启动成功

## client配置:/etc/fdfs/client.conf

1 创建client所需的日志文件保存路径：

    mkdir /usr/local/fdfs/client/
    
2 vim /etc/fdfs/client.conf

  base_path=/usr/local/fdfs/client/ 
  
  tracker_server=192.168.111.11（公网地址）:22122 #tracker服务器IP地址和端口号
  
3 测试文件上传 /usr/bin/fdfs_upload_file  /etc/fdfs/client.conf  /opt/BLIZZARD.jpg
  返回一个上传图片的url地址

fastDFS 不支持http服务，nginx上使用FastDFS的模块fastdfs-nginx-module提供http服务

下载fastdfs-nginx-module源码包 https://github.com/happyfish100 

1 wget https://github.com/happyfish100/xxxx

2 解压压缩文件

3 进入解压文件，cd fastdfs-nginx-module

4 把fastdfs-nginx-module/src/mod_fastdfs.conf 拷贝到/etc/fdfs/下

    cp /mod_fastdfs.conf/src/mod_fastdfs.conf /etc/fdfs/
    
5 把mod_fastdfs.conf安装到nginx。

  如果已经安装好了nginx：进入nginx解压文件下，执行./configure --add-module=/src(src路径)
  
                         make 编译 make install安装（相当重新安装一遍nginx）
                         
  如果没有安装过nginx ：见其他文档，解压好文件后。
  
  进入nginx解压文件下，执行./configure --add-module=/src(src路径)
                        
6 修改/etc/mod_fastdfs.conf 

    base_path=/opt/fastdfs_storage #保存日志目录
    tracker_server=192.168.111.11:22122 #tracker服务器的IP地址以及端口号
    storage_server_port=23000 #storage服务器的端口号
    url_have_group_name = true #文件 url 中是否有 group 名
    store_path0=/opt/fastdfs_storage_data # 存储路径
    group_count = 3 #设置组的个数，事实上这次只使用了group1


#配置完了还不能正常访问文件服务系统的话，多半是端口没有开放

    firewall-cmd --zone=public --add-port=22122/tcp --permanent
    
    firewall-cmd --zone=public --add-port=23000/tcp --permanent

    firewall-cmd --reload 重启防火墙

简化tracker，storage的启动。编写脚本文件，把要执行的命令放到
一个单独的文件中，并设置该文件为可执行

### 报错处理：

fdfs_monitor /etc/fdfs/client.conf 命令查看storage的状态是offline正常为active
                                   
fdfs_monitor /etc/fdfs/client.conf delete group1 storage的IP地址。 #先在tracker上删除storage的信息
