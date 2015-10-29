title: 编写自己的NPM包       
date: 2014-04-21 08:06:17  
tags:   
    - Node.js  
    - npm   
    - javascript       
 
---

### 1.编写模块  
一个最简单的NPM包由主模块index.js和包描述文件package.json组成。  
让我们编写一个生成指定长度随机字符串的NPM模块：  

```javascript   
 
//index.js
module.exports = function(len){
    var rdmString = "";
    for (; rdmString.length < len; rdmString += Math.random().toString(36).substr(2));
    return rdmString.substr(0, len);
}      

``` 

```javascript   
//package.json
{
"name": "random-str",
"version": "0.0.3",
"auth": "ykq",
"scripts": {
    "start": "node app"
 },
"dependencies": {}
}  
 
```  

### 2.注册包仓库账号 npm adduser  

为了维护包，npm必须要使用仓库账号才允许将包发布到仓库中。

注册账号的命令是npm adduser，按照提示填写相关信息即可:
![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/1823676374208641401.png)  

### 3.上传包 npm publish  
在目录下执行npm publish，开始上传包，在这个过程中，npm会将目录打包成一个存档文件，然后上传到官方仓库中。  

![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/4844184349290799100.png)  

### 4.查看包  

上传完成后我们就可以在官方仓库中查看我们刚刚写的包.

[https://www.npmjs.org/](https://www.npmjs.org/)  

![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/6599311670866054238.png)   

### 5.安装及测试包

下载安装很简单，直接npm install random-str

接着测试下随机生成100个字符串,代码如下：  

```javascript  

//app.js
var r = require("random-str")(100);
console.log(r);

```  

效果如下，说明我们编写的包能正常安装和使用了： 
![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/3760505688954558267.png)  




