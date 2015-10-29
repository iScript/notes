title: Angular.js整合socket.io   
date: 2014-01-04 08:06:17  
tags:   
    - javascript  
    - angular.js   
    - socket     
 
---

### 前言 
作为下一代的 Web 标准，HTML5 拥有许多引人注目的新特性，如 Canvas、本地存储、多媒体编程接口、WebSocket 等等。  
这其中WebSocket使得浏览器对 Socket 的支持成为可能，从而在浏览器和服务器之间提供了一个基于 TCP 连接的双向通道。Web 开发人员可以非常方便地使用 WebSocket构建实时 web 应用，开发人员的手中从此又多了一柄神兵利器。  

### socket.IO  
Socket.IO 是一个功能非常强大的框架，能够帮助你构建基于 WebSocket的跨浏览器的实时应用。支持主流浏览器，多种平台，多种传输模式，还可以集合 Exppress 框架构建各种功能复杂的实时应用。  
官网:[http:www.socket.io](http:www.socket.io)  

### AngularJs  
AngularJs是一个Google出的现代化MVVM前端框架，具有数据绑定、依赖注入、模板等功能。  
通过AngularJs的数据双向绑定，利用socket.io从服务端推送一个消息到客户端，客户端内容自动发生改变，这是多么令人兴奋得一件事。

### 把Socket.IO封装到一个AngularJS服务里  
代码如下:  
![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/779685685588560439.png)  

此处只封装了两个函数,分别是Socket.IO API中的on事件和emit事件方法。  
API中还有很多事件方法,我们可以以类似的方式封装他们。  


封装完成后我们就可以在angularJs的控制器或其他地方注入该服务，
让我们用该服务向node.js服务端发送一个socket消息：  
![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/150589112640474987.png)  

执行代码后打开node.js控制台，可以看到服务端正确的接收到了该消息  
![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/2673449328897841104.png)  

