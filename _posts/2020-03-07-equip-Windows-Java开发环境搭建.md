---
layout: post
title: Windows搭建Java开发环境
categories: Windows
description: Windows搭建Java开发环境
keywords: Windows
---
入职新公司再搭建新的Java开发环境浪费了大量的时间，写篇博文记录资源、破解方案、安装方式等。
主要安装jdk+git+maven+idea+xshell+navicat premium+redis图形化界面。

### 安装JDK
Windows环境jdk1.8资源链接

    https://pan.baidu.com/s/1rknMxHYy7Xf4P252cZSgwA 提取码：obhu

1、选择“计算机>>单击右键>>属性>>高级系统设置>>高级>>环境变量”

2、创建新的系统变量命名为JAVA_HOME，变量值为JAVA安装路径

3、修改系统变量path的值，再最前面添加：%JAVA_HOME%\bin;

### 安装配置Git
Git-2.14.1-64下载链接

    https://pan.baidu.com/s/1cntw1TVjQt_ub239q-kMMA 提取码：674f 

1 创建私服账号，已创建GitLab为例子

2 桌面右键打开Git Bash界面，检查是否存在密钥对：cat ~/.ssh/id_rsa.pub

3 创建新的SSH密钥对，执行以下命令

    ssh-keygen -t rsa -C "example@qq.com" -b 4096
    example@qq.com:表示在Gitlab上注册的账号
    
4 一直回车，入如下图
  ![](/images/posts/windows/git_success.png)

5 登录Gitlab服务器，点击右上角头像>>Settings，添加SSH Keys:

6 进入git bash界面，拉取代码git clone url即可

### Maven环境搭建
maven3.6.1资源链接

    https://pan.baidu.com/s/1XxjmtH97VLdGPfsEwfbFeQ 提取码：fdmx 

1 解压资源文件，进入高级属性配置MAVEN_HOME指向maven解压目录

2 修改path变量，添加%MAVEN_HOME%/bin

3 进入cmd，mvn -version校验是否成功

4 修改用户目录下的.m2目录下的setting.xml文件

### 安装破解Idea
idea2018.3.2链接
    
    https://pan.baidu.com/s/13EObikOzWshjeN8IE6cYmw 提取码：miqa 

idea破解包JetbrainsIdesCrack-4.2-release.jar链接

    https://pan.baidu.com/s/1KrOOnd7Q79V9oXHp_bXQnw 提取码：bjo1 

1 下载安装idea，将JetbrainsIdesCrack-4.2-release.jar放到idea安装命令的bin包里面

2 对bin下的idea.exe.vmoptions和idea64.exe.vmoptions这两个文件进行修改，打开文件在末尾添加如下配置指令：

    -javaagent:D:/idea/bin/JetbrainsIdesCrack-4.2-release.jar（为包路径）
    
3 保存成功后启动idea，输入注册码
   
    ThisCrackLicenseId-{
    "licenseId":"ThisCrackLicenseId",
    "licenseeName":"Rover12421",
    "assigneeName":"",
    "assigneeEmail":"rover12421@163.com",
    "licenseRestriction":"For Rover12421 Crack, Only Test! Please support genuine!!!",
    "checkConcurrentUse":false,
    "products":[
    {"code":"II","paidUpTo":"9998-12-31"},
    {"code":"DM","paidUpTo":"9998-12-31"},
    {"code":"AC","paidUpTo":"9998-12-31"},
    {"code":"RS0","paidUpTo":"9998-12-31"},
    {"code":"WS","paidUpTo":"9998-12-31"},
    {"code":"DPN","paidUpTo":"9998-12-31"},
    {"code":"RC","paidUpTo":"9998-12-31"},
    {"code":"PS","paidUpTo":"9998-12-31"},
    {"code":"DC","paidUpTo":"9998-12-31"},
    {"code":"RM","paidUpTo":"9998-12-31"},
    {"code":"CL","paidUpTo":"9998-12-31"},
    {"code":"PC","paidUpTo":"9998-12-31"}
    ],
    "hash":"2911276/0",
    "gracePeriodDays":7,
    "autoProlongated":false}

更多方式[参考](https://blog.csdn.net/java_zyq/article/details/88532526)

### 安装xShell工具
xShell6链接

    https://pan.baidu.com/s/15hzesXYDuz-qjuFk99ceKA 提取码：uyo9

1 一直下一步到结束为止

2 添加公司服务器地址，配置账号&密码（区分开发、测试、线上环境）

### 安装数据库图形化界面
navicat premium支持各种数据库图形化界面操作

    https://pan.baidu.com/s/13NBCc9Cg2A89IZP-T1yTtQ 提取码：6yk8 

navicat premium破解包链接：

    https://pan.baidu.com/s/1YQoqPvWSwGXw07m3Fg4jxA 提取码：egui 
                  
破解方式:安装navicat后，请根据自己安装Navicat Premium 12的语言（简体中文、繁体中文、英文）和版本位数（32位、64位），
将对应文件夹里的所有文件拷贝至Navicat Premium 12安装位置的根目录（即能看到navicat.exe的那个目录）。

### Jmeter工具以及
压测工具Jmeter以及插件
将JmterPlugins的两个插件放到jmeter解压包lib/ext目录下可以统计tps，响应时间等

    https://pan.baidu.com/s/17QXPJT8KeXOe8fiXbKknOg 提取码：ny3y
    
    Jmeter插件方便查看Tps等指标
    https://pan.baidu.com/s/1JrCqLE4hZ2TWS5a7gwoxFw 提取码：infp
    https://pan.baidu.com/s/1n2D8lh3QCvToZzM-RDgB6w 提取码：dj5f


### 其他辅助工具
redis图形化界面资源链接

    https://pan.baidu.com/s/1pA9G0Wx06IG3PbHmYrxI-A 提取码：kezf
  
postman工具链接

    https://pan.baidu.com/s/1Y0IY33ayefG6Fxye_yQVmg 提取码：zyo0
       
jca461.jar用于分析线程栈dump信息，分析线程dump情况（http://spotify.github.io/threaddump-analyzer 在线分析线程dump情情况
    
    https://pan.baidu.com/s/1JAvPoNfkoalS0Cq5k22QQQ 提取码：hlsh 
                               
ha456.jar用于分析堆dump，分析堆内存使用对象情况

    https://pan.baidu.com/s/1nyZWnjDR_2tyBn_lcy9QtA 提取码：p9ed

jar包反编译工具

    https://pan.baidu.com/s/1J5qzINlcmiajrNsC_T2tTQ 提取码：lz3f
 
谷歌访问外网插件链接
     
     https://pan.baidu.com/s/1NG-krcGjMowWgL7W1hzmIQ 提取码：cqwc 


       


