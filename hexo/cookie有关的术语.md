title: cookie有关的术语        
date: 2014-09-14 08:06:17  
tags:   
    - cookie  
    - 安全   
    - session       
 
---

### session cookie 
当cookie没有设置超时时间，那么cookie会在浏览器退出时销毁，这种cookie是session cookie。  

### persistent cookie/tracking cookie 
设置了超时时间的cookie，会在指定时间销毁，cookie的维持时间可以持续到浏览器退出之后，这种cookie被持久化在浏览器中。  
  
很多站点用cookie跟踪用户的历史记录，例如广告类站点会使用cookie记录浏览过哪些内容，搜索引擎会使用cookie记录历史搜索记录，这时也可以称作tracking cookie，因为它被用于追踪用户行为。    

### secure cookie
服务器端设置cookie的时候，可以指定secure属性，这时cookie只有通过https协议传输的时候才会带到网络请求中，不加密的http请求不会带有secure cookie。

设置secure cookie的方式举例：

Set-Cookie: foo=bar; Path=/; Secure  

### HttpOnly cookie 

服务器端设置cookie的时候，也可以指定一个HttpOnly属性。

Set-Cookie: foo=bar; Path=/; HttpOnly

设置了这个属性的cookie在javascript中无法获取到，只会在网络传输过程中带到服务器。  

### third-party cookie  

第三方cookie的使用场景通常是iframe，例如www.a.com潜入了一个www.ad.com的广告iframe，那么www.ad.com设置的cookie不属于www.a.com，被称作第三方cookie。  

### supercookie  

cookie会从属于一个域名，例如www.a.com，或者属于一个子域，例如b.a.com。但是如果cookie被声明为属于.com会发生什么？这个cookie会在任何.com域名生效。这有很大的安全性问题。这种cookie被称作supercookie。

浏览器做出了限制，不允许设置顶级域名cookie(例如.com，.net)和pubic suffix cookie(例如.co.uk，.com.cn)。

现代主流浏览器都很好的处理了supercookie问题，但是如果有些第三方浏览器使用的顶级域名和public suffix列表有问题，那么就可以针对supercookie进行攻击啦。  

### zombie cookie/evercookie 

僵尸cookie是指当用户通过浏览器的设置清除cookie后可以自动重新创建的cookie。原理是通过使用多重技术记录同样的内容(例如flash，silverlight)，当cookie被删除时，从其他存储中恢复。

evercookie是实现僵尸cookie的主要技术手段。  
  
  
 
![cookie](http://7xnv0h.com1.z0.glb.clouddn.com/6608239705284263194.jpg)


