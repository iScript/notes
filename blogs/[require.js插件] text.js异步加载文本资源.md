title: text.js异步加载文本资源[require.js插件]  
date: 2013-12-15 08:06:17
tags:   
    - javascript  
    - require.js  
    - text.js 
 
---

text.js是require.js的一个插件，用于异步加载文本资源，如txt、css、html、xml、svg等。

### 安装text.js
```bash  
$ bower install requirejs-text  
``` 

### text.js使用  
在require.js主模块main.js配置text.js路径  

```javascript  
require.config({
    paths: {
        text : "../bower_components/requirejs-text/text"
    }
});  
```  

使用text.js加载资源  

```javascript  
  
require(['text!../text/a.txt','text!../css/a.css','text!../templates/a.html'], function (aTxt,aCss,aHtml){
	console.log(aTxt);
	console.log(aCss);
	console.log(aHtml);
	document.getElementsByTagName("style")[0].innerHTML = aCss;
	document.getElementsByTagName("body")[0].innerHTML = aHtml;
});   
 
```  

text.js 使用text!+资源路径 来加载资源  
上例使用text.js异步加载了a.txt a.css a.html，使用的是相对路径加载，当然还可以使用绝对路径加载及在配置中的path定义一个目录。  
加载完成后分别在控制台输出及插入到网页里。  

![javascript](http://7xnv0h.com1.z0.glb.clouddn.com/2588725360825939709.png)  

网页里显示  
![javascript](http://7xnv0h.com1.z0.glb.clouddn.com/787004034983012132.png)


