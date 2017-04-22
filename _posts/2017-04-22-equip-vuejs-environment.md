---
layout: post
title: 搭建vue项目环境 -实记
categories: JS
description: vue是一个前端框架，特点是数据绑定与组件化。vue虽然是中国人开源的，但是其目前相当火，特来准备学习下。我使用的系统是windows10。
keywords: npm,vue
---

vue是一个前端框架，特点是数据绑定与组件化。vue虽然是中国人开源的，但是其相当火，特来准备学习下，我使用的是windows10。



## 环境搭建


1、安装Node.js

   到官网下载安装包直接安装，安装完成后再win+R-->cmd-->输入 node -v，显示版本则成功。

2、 npm -v同样能看到其版本号，因为nodejs自带npm

3、配置npm全局模块存放路径及cache路径

    npm config set prefix "D:\Program Files\nodejs\node_global"

    npm config set cache "D:\Program Files\nodejs\node_cache"

4、配置环境变量NODE_PATH指向以下路径

    D:\Program Files\nodejs\node_modules

5、安装cnpm

   由于国内使用npm会比较慢，可用淘宝镜像cnpm代替npm使用。
   
   cnpm 安装
    
  ![](/images/posts/JS/cnpm.png)
    
    注意：npm由node自带。window下需要安装nodejs和Git。
    
6、安装vue-cli (vue脚手架)

    输入命令：npm install --g vue-cli (可用cnpm代替npm)
    
### 填坑


   1. cnpm -v
      *报错*：cnpm不是内部或外部命令......
      
      解决：此问题说明cnpm没有安装成功，找到users/Yaoyao/.npmrc文件，删除，再执行以上命令（即重新安装cnpm），再输入cnpm-v就能看到显示版本号了。
  ![](/images/posts/JS/cnpm_v.png)成功！
     
   2. 输入命令：vue -v
    
    error:unkonw option 'v'
     
     解决：大写‘V’
  ![](/images/posts/JS/vue_v.png)
     
## 开始使用vue


1、创建vue项目

  ![](/images/posts/JS/vuejs_1.png)

    回车，（即默认文件名为my-first-...）
    
2、
  ![](/images/posts/JS/vuejs_2.png)
 
   添加描述、作者（我这里有默认的直接enter）等
 
   文件夹位置如下：
   
   ![](/images/posts/JS/vuejs_3.png)
 
3、接下来下载需要使用的所有依赖
 
   ![](/images/posts/JS/vuejs_4.png)
 
     已成功下载到node_modules文件夹里
   
4、跑起来

  ![](/images/posts/JS/vuejs_5.png)
    回车

   ![](/images/posts/JS/vuejs_6.png)

服务器已被启动，它监听的端口是localhost:8080

5、打开浏览器进入http:localhost:8080

  ![](/images/posts/JS/vuejsTemplate.png)

这就是刚搭建的vue模板

6、现在可以在src目录下进行具体开发。

## 配置 sublime Text


    我用sublime text3来编辑Vue的项目，可以安装Vue的插件。
    
1、如果之前ST3未安装sublime Package Control，那么首先要安装它。
    
   ![](/images/posts/JS/ST3_packageControl.png)
   
   `import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by) `
   
2、 安装好后，ctrl+shift+p出现下面界面则成功。并找到Install Package并点击，如图：
   
 ![](/images/posts/JS/ST3_1.png)
 
3、 再在框中输入vue，出现一系列相关
  
 ![](/images/posts/JS/ST3_2.png)
 
 选择第二个：Vue Syntax Highlight.
 
 之后，再重新打开Ｖｕｅ的文件，vue代码高亮，就大功告成了！
   
   

   