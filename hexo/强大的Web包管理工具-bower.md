title: 强大的Web包管理工具-bower  
date: 2013-11-08 08:00:00  
tags:   
    - bower
    - node.js
    - javascript

    
---
强大的Web包管理工具-bower

## 前言 
每当我们开始一个新的web前端项目，我们做个第一件事都是去下载相应的js类库文件。  
如我们项目要用到Jquery，我们就要去Jquery的官网下载Jquery库。  
接着若我们的前端UI要用到Bootstrap，我们还要去Bootstrap的官网下载Bootstrap,而最新的Bootstrap3需要Jquery1.9以上的支持，我们不得不又要检查Jquery的版本是否合适，不合适的话要重新下载。  
甚至复杂一点的angular.js的项目我们可能要依赖angular的一些模块：angular-route、angular-animate等。  
像angular-route.js这个路由模块在angular1.2之前是内置于angular的，而angular1.2之后是从angular分离出来的(上次升级angular被坑了2小时。。)。  
我去，这些问题想想都头疼。这时候我们需要一个工具来管理这些库及其依赖。  

### bower介绍 
bower是twitter推出的一款web软件包管理工具，使用nodejs开发，开源。  
提供添加新web包，更新web包，删除web包，发布web包，管理包依赖等功能。  

### bower安装dd
bower的安装是通过node.js的npm管理工具来安装的 ：     
 
``` bash 
$ npm install -g bower   
```  
  
    
### bower命令
安装完成后，我们可以通过bower --help来查看bower有什么命令  
![Alt ddd](http://7xnv0h.com1.z0.glb.clouddn.com/4938196991511821694.jpg)   
各命令说明：    
* cache : bower缓存管理
* help : 显示Bower命令的帮助信息
* home : 通过浏览器打开一个包的github发布页
* info : 查看包的信息
* init : 创建bower.json文件
* install : 安装包到项目
* link : 在本地bower库建立一个项目链接
* list : 列出项目已安装的包
* lookup : 根据包名查询包的URL
* prune : 删除项目无关的包
* register : 注册一个包
* search : 搜索包
* update : 更新项目的包
* uninstall : 删除项目的包  


### bower使用 
使用bower来安装包及依赖：  

bower install    -- 根据bower.json配置文件来安装web包。  
bower install <package>   -- 根据包名称来安装web包，默认安装最新版。  
bower install <package>#<version>    -- 安装该版本的web包，#号好后跟版本号。  
bower install <name>=<package>#<version>    -- 使用别名安装该版本的web包。  

使用bower来安装bootstrap ：  
 
```bash  
$ cd /www/test    
$ bower install bootstrap  
```  

执行以上命令自动安装bootstrap框架及它所依赖的最新版的Jquery。

通过bower list --path 你可以得到每个类库的路径名称映射。  
![Alt bower](http://7xnv0h.com1.z0.glb.clouddn.com/3147453189678882323.jpg)  
这样就能再项目中使用对应的类库了。  


```html   
<script src="bower_components/jquery/jquery.js"></script>

```  

 

其他命令的使用：  
还有其他命令用法也基本大同小异：  
如bower update jquery 更新jquery包。 bower uninstall jquery 删除jquery包。  
这里就不多说了大家动手敲一遍就会了。  






