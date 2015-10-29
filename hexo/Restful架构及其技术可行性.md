title: Restful架构及其技术可行性       
date: 2014-04-07 08:06:17  
tags:   
    - RESTful  
    - PHP   
    - Laravel       
 
---

### RESTful简介  
RESTful (Representation State Transfer) 描述了一个架构样式的网络系统，比如 web 应用程序。它首次出现在 2000 年 Roy Fielding 的博士论文中，他是 HTTP 规范的主要编写者之一。RESTful 指的是一组架构约束条件和原则，满足这些约束条件和原则的应用程序或设计就是 RESTful。  

![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/4945796815883174034.jpg)  

这里对什么是RESTful不作详细的论述，具体大家可以参考阮一峰的博客[http://www.ruanyifeng.com/blog/2011/09/restful.html](http://www.ruanyifeng.com/blog/2011/09/restful.html)  

  
简单来说，RESTful架构就是一组约束条件和规则：    
1. 每一个URI代表一种资源。  
2. 客户端和服务器之间，传递这种资源的某种表现层。  
3. 客户端通过四个HTTP动词（GET、POST、PUT、DELETE），对服务器端资源进行操作，实现"表现层状态转化"。  

比如一个电影的网站，REST 的 URI 可设计为：  

GET     http://api.douban.com/movie          //电影列表  
GET     http://api.douban.com/movie/123      //查看某部电影  
POST    http://api.douban.com/movie          //添加电影  
PUT     http://api.douban.com/movie/123      //修改某部电影  
DELETE  http://api.douban.com/movie/123      //删除某部电影 


### RESTful架构的技术可行性 

基于RESTful架构的网站好处有很多，但我们也要测试在项目用应用RESTful是否有兼容性问题，即HTTP的4个动作(GET/POST/PUT/DELETE)在服务端和浏览器端是否都能正常发送和接收。由于GET和POST这2个method已经快被我们用烂了，我们只要测试下PUT和DELETE这2个method。  

### 客户端浏览器方面： 

由于历史原因，目前浏览器的\<form\>标签只支持GET和POST，但是通过ajax技术，几乎所有的主流浏览器都支持GET/POST/PUT/DELETE这4个方法，测试代码如下：  

```javascript  
$.ajax({url:'/rest',type:'put',success:function(data){
	alert("浏览器支持PUT:"+data);
}});   
```  


```javascript  
$.ajax({url:'/rest',type:'delete',success:function(data){
    alert("浏览器支持DELETE:"+data);
}});  
```  

本人相继测试了IE6、IE8、chrome、firefox、safari、UC、安卓原生浏览器、iphone4 safari等浏览器，都能正常运行。    
![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/4910893918770980570.jpg)  

![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/6608189127748129598.jpg)  

如果你使用了前端MVC框架如Angular.js、backbone等，那就更棒了！它们提供的RESTful模块能让我们的操作变得更加简单和富有表现力。  

### 服务端方面(以PHP为例)： 

我们知道PHP分别提供了$_GET和$_POST这2个超全局数组分别用于处理get和post请求，但是却没有$_PUT和$_DELETE来处理put和delete。怎么办？没关系，自己封装一个(怎么封装百度一下)。  

  
另外目前几乎所有主流的PHP框架都对RESTful提供了很好的支持，如Laravel、zend、symfony等。  

Laravel的路由：  

```php  
Route::put('/rest',function(){ echo 'true';exit;  });
Route::delete('/rest',function(){ echo 'true';exit;  });  
```  

Laravel的资源控制器让围绕资源构建RESTful模式变得更加简单：  

```php  
Route::resource('photo','PhotoController');  
``` 

### 总结： 
经过测试，在项目中使用RESTful架构是完全可行的。

RESTful虽好，可不要贪杯哦。具体情况还要灵活使用，不要被它捆住。


 