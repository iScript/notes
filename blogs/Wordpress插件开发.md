title: Wordpress插件开发     
date: 2014-01-12 08:06:17  
tags:   
    - PHP  
    - wordpress     
    - 插件     
 
---

Wordpress插件允许你通过一种简单的方式来修改、定义和强化博客的功能。  
你可以在不修改WordPress的核心代码的情况下，通过插件来直接向WordPress中增加功能。  
WordPress插件可以是一个程序，也可以是PHP语言编写的一个或一组函数。  
它可以通过插件 API提供的一系列方法和接口，来向WordPress中增加一些特定的功能或服务，并且让它们看上去就像是WordPress原有的功能一样。  

### 钩子机制、动作和过滤器 
在说开发wordpress插件之前先讲下wordpress中的钩子机制。

WordPress 中有一种钩子机制，允许插件把一些功能“挂载”到 WordPress 当中。  
也就是说，在系统运行至某一个环节时，去调用插件内的一些函数。执行挂勾分为两种：动作和过滤器。

动作 (Action)：

动作是WordPress运行到某些环节，或者在某些事件发生时，就执行指定的 PHP 函数。
wordpress中的动作响应函数是add_action(),如下：  
add_action( '动作名', '响应函数名', [优先级], [参数数目] );

wordpress中的动作：[http://codex.wordpress.org/Plugin_API/Action_Reference](http://codex.wordpress.org/Plugin_API/Action_Reference)



过滤器 (Filter)：

过滤器是一类函数，WordPress执行传递和处理数据的过程中，在针对这些数据做出某些动作之前的特定点运行(例如将数据写入数据库或将其传递到浏览器页面)。  
wordpress中注册过滤器的函数是add_filter(),如下：  
add_filter('过滤名', '过滤函数', [优先级], [参数数目] );  

### 开始创建一个插件 
知道了wordpress中的钩子机制直接之后让我们来创建一个百度分享的插件 。

wordpress中的插件目录为：wp_content/plugins

让我们在该目录下创建一个文件夹bdShare，并在bdShare文件夹下创建插件主文件bdShare.php。

标准插件信息：

插件的主文件顶部必须包括一个标准插件信息头。WordPress通过标准信息头识别插件的存在，并把它加入到控制面板的插件管理页面，这样插件才能激活，载入插件，并运行里面的函数。

在bdShare.php中写入标准插件信息：

```php  
<?php

/*

Plugin Name: 百度分享插件
Plugin URI: http://yuankeqiang.lofter.com
Description: 百度你这么吊你妈知道吗
Version: 1.0
Author: 袁克强
Author URI: http://yuankeqiang.lofter.com

*/  
```  

保存文件，登陆wordpress后台并进入插件菜单就可以看到我们的插件信息了  

![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/2098395951478375538.jpg)  

但是目前就算启用了插件也没有任何功能。  

接着在bdShare.php加上百度分享的代码，并在wordpress加载完头部模板(wp_head)后执行这段代码：  

```php  
function bdShare(){echo '<script>window._bd_share_config={"common":{"bdSnsKey":{},"bdText":"","bdMini":"2","bdMiniList":false,"bdPic":"","bdStyle":"0","bdSize":"16"},"slide":{"type":"slide","bdImg":"1","bdPos":"right","bdTop":"100.5"}};with(document)0[(getElementsByTagName("head")[0]||body).appendChild(createElement("script")).src="http://bdimg.share.baidu.com/static/api/js/share.js?v=89343201.js?cdnversion="+~(-new Date()/36e5)];</script>';}

add_action("wp_head","bdShare");  
```  

最后保存代码并在后台启用该插件，刷新wordpress首页即可看到我们的百度分享插件:  
![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/6608259496491981029.jpg)  


### 完毕
wordpress中的插件开发就是这么简单，利用wordpress中的钩子机制及其自带的强大的函数就能创造出各种神奇的插件。。。。。。

