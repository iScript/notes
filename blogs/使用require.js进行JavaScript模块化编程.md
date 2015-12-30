title: 使用require.js进行JavaScript模块化编程  
date: 2013-12-10 08:00:00  
tags:   
    - javascript    
    - require.js  
    - AMD
    
---

随着互联网的飞速发展，Javascript开发越来越复杂。Javascript的模块化开发显的越来越重要。  
目前，主要的Javascript模块规范共有两种：CommonJS和AMD。

### CommonJS  
CommonJS主要指服务器端的javascript模块的规范。它的终极目标是提供一个类似Python，Ruby和Java标准库。这样的话，开发者可以使用CommonJS API编写应用程序，然后这些应用可以运行在不同的JavaScript解释器和不同的主机环境中。CommonJS的实现有Node.js、CouchDB等。  

### AMD规范  
AMD是"Asynchronous Module Definition"的缩写，意思就是"异步模块定义"。它采用异步方式加载模块，模块的加载不影响它后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成之后，这个回调函数才会运行。require.js是AMD规范最著名的实现。  

### 选择哪种？
CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是异步加载模块，允许指定回调函数。由于Node.js主要用于服务器编程，模块文件一般都已经存在于本地硬盘，所以加载起来比较快，不用考虑非同步加载的方式，所以CommonJS规范比较适用。但是，如果是浏览器环境，要从服务器端加载模块js，加载的过程中，浏览器会停止网页渲。这时就建议采用异步加载方式，很好的避免模块加载的过程中网页失去响应。  

### require.js介绍
Require.JS是一个基于AMD规范的javascript模块加载框架。实现js文件的异步加载，管理模块之间的依赖性，提升网页的加载速度。  
官网：[www.requirejs.org/](www.requirejs.org/)  

![alt require.js](http://7xnv0h.com1.z0.glb.clouddn.com/1756966804728113694.png)  

### 在网页中使用require.js  
```html  
<script src="js/require.js" data-main="js/main"></script>  
```  

下载后在网页中引入require.js并指定data-main属性。该属性的作用是指定网页程序的主模块。在上例中，就是js目录下面的main.js，这个文件会第一个被require.js加载。 

### 定义模块 
根据AMD规范，定义一个模块采用define函数  
```javascript  
define(id, dependencies, factory);  
```  
参数：  
  id : 模块标示符[可省略]  
  dependencies : 所依赖的模块[可省略]  
  factory : 模块的实现，或者一个JavaScript对象。 
  
### require.js定义模块的几种方式 
1.定义一个最简单的模块：  

```javascript    
define({
	color: "black",
	size: "unisize"
}); 

```  


2.传入匿名函数定义一个模块：  

```javascript
define(function () {
	return {
	    color: "black",
	    size: "unisize"
	}
});
```  

3.定义一个模块并依赖于cart.js  

```javascript
define(["./cart"], function(cart) {
    return {
        color: "blue",
        size: "large",
        addToCart: function() {
            cart.add(this);
        }
    }
});
```  

4. 定义一个名称为hello且依赖于cart.js的模块  

```javascript  
define("hello",["./cart"],function(cart) {
    //do something
});  
```  

5.兼容commonJS模块的写法(使用require获取依赖模块，使用exports导出API)  

```javascript  
define(function(require, exports, module) {
    var base = require('base');
    exports.show = function() {
        // todo with module base
    } 
});  
``` 

### require.js加载模块 
require.js通过require()函数来异步加载模块，其接受两个参数。  
第一个参数是一个数组，表示所依赖的模块。第二个参数是一个回调函数，当前面指定的模块都加载成功后，它将被调用，且加载的过程中浏览器不会失去响应。  

```javascript  
 //main.js
require(['cart'], function (cart){
     //假设我们使用前面第2种方法定义一个模块cart.js,然后这里加载cart.js,下面输出"black"
    alert(cart.color);
});  
```  

### 模块加载的配置

使用require.config()方法，我们可以对模块的加载行为进行自定义配置。  
require.config()方法一般写在主模块(main.js)最开始。参数为一个对象，通过对象的键值对加载进行配置:  

```javascript  
require.config({
    paths: {
        "angular": "https://ajax.googleapis.com/ajax/libs/angularjs/1.2.4/angular.min",
        "angularRoute": "../bower_components/angular-route/angular-route"
    },
    shim: {
        'angular' : {'exports' : 'angular'},
        'angularRoute': {deps:['angular']}
    }
});  
``` 

上面代码意思是配置angular.js及angular-route.js的加载路径，并配置angular的外部输出名、angularRoute依赖于angular(加载顺序)。  

其他一些配置：  
* baseUrl ： 指定模块根路径
* paths ： 指定模块的加载路径
* shim ：声明依赖项、外部输出等
* map : 对于给定的相同的模块名，加载不同的模块，而不是加载相同的模块
* config : 传递一个配置信息到模块
* packages : 配置从CommonJS 包来加载模块
* deps : 全局配置模块的依赖项的数组
* callback ： 所有依赖项加载后执行的回调函数
* ...


### require.js一些插件 
domready插件，可以让回调函数在页面DOM结构加载完成后再运行。  
text插件，让require.js除了能加载js文件还能加载txt文件、css文件等。  
插件清单：[https://github.com/jrburke/requirejs/wiki/Plugins](https://github.com/jrburke/requirejs/wiki/Plugins)


---
呼，看了半天官方文档终于把require.js基本用法写完了。

