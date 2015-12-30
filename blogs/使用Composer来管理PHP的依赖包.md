title: 使用Composer来管理PHP的依赖包  
date: 2013-10-22 08:00:00  
tags:   
    - 框架  
    - PHP  
    - Laravel  
    
---
如今有大量的PHP函数库、框架和组件可供选择，一个项目中可能会使用其中的若干——这就是项目的依赖。到目前为止，PHP还没有有效的 项目依赖管理方案。即使你手工的管理它们，你还不得不处理它们的自动加载问题。

Composer 是PHP(5.3.2+)中用来管理依赖(dependency)关系的工具。你可以在自己的项目中声明所依赖的外部工具库(libraries)，Composer会帮你安装这些依赖的库文件，并配置好自动加载路径。


### 全局安装Composer (环境为mac OSX 10.8.4) 
``` bash
$ curl -sS https://getcomposer.org/installer | /usr/app/php5.3/bin/php  
$ mv composer.phar /usr/local/bin/composer  
$ composer   
```

### 如何定义和安装依赖 
首先，在项目所在目录创建文件composer.json，下面是一个依赖Twig例子：    

``` javascript
{    
    "require":{       
         "twig/twig": "1.8.*"   
     }
}
```
第二步：在项目根目录运行：  
``` bash
$ composer install 
```  
这会在vendors/下载和安装项目依赖。最后在应用的PHP入口文件添加下面代码，告诉PHP使用Composer自动加载器加载项目的依赖库：  
``` php 
<?php require 'vendor/autoload.php';
```  
现在你就可以使用项目依赖的库了，它们会在需要的时候自动加载。  

### 最后，使用composer安装我最喜欢的laravel框架
1.从github下载laravel框架  
2.解压laravel到网站根目录并执行composer install来安装。  
``` bash
$ cd /usr/app/laravel && composer install  
```  
3. 在浏览器输入http://127.0.0.1 检测安装是否成功  

![Atl laravel](http://7xnv0h.com1.z0.glb.clouddn.com/2853030364039488816.png)



