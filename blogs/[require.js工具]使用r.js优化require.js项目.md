title: 使用r.js优化require.js项目[require.js工具] 
date: 2013-12-17 08:06:17
tags:   
    - javascript  
    - require.js  
    - grunt 
 
---

RequireJS提供一个基于node.js的命令行工具r.js用来压缩多个js文件。  
它的主要作用是将多个模块文件压缩合并成一个脚本文件，以减少网页的HTTP请求数。  
![require.js](http://7xnv0h.com1.z0.glb.clouddn.com/6597565646402518692.png)

### 如何使用？ 
官方文档：[http://www.requirejs.org/docs/optimization.html](http://www.requirejs.org/docs/optimization.html) 
 
这里具体如何使用就不多说了，无非就是用npm下载后几个命令而已。
由于目前端项目大多数都是使用grunt构建的，且r.js也支持grunt。
这里就讲下如何使用grunt配合r.js实现自动化优化require.js项目。

### grunt-contrib-requirejs  
grunt-contrib-requirejs是grunt官方出的一款用于优化require.js项目的grunt插件，基于r.js。  

### 安装grunt-contrib-requirejs  
```bash  
npm install grunt-contrib-requirejs --save-dev  
```  
使用npm install即可完成该插件的安装。

(另外关于如何安装grunt及大概的使用方法可参考本人之前的博客。)


### 配置grunt-contrib-requirejs 

```javascript  
module.exports = function(grunt){
    grunt.initConfig({
        pkg : grunt.file.readJSON('package.json'),
        requirejs : {
            compile: {
                options: {
                    name : "main",
                    optimize: "uglify",
                    mainConfigFile: "./js/main.js",
                    out: "./compile/all.js"
                }
            }
        }
    });
    grunt.loadNpmTasks('grunt-contrib-requirejs');
    grunt.registerTask('default',['requirejs']);
}  
```  

配置说明：主要配置项在option属性中  
optimize代表使用uglify进行压缩。
mainConfigFile代表require.js主模块位置，通过读取主模块里的require.config({})配置来获取各个js的路径。  
out代表压缩后的输出位置。  

当然不止仅仅这几个配置，详细请看： [https://github.com/jrburke/r.js/blob/master/build/example.build.js](https://github.com/jrburke/r.js/blob/master/build/example.build.js)


### 运行

配置好后命令行下输入grunt执行任务。

执行完毕后可以看到compile文件夹下多个个all.js，打开可以看到所有通过require.js加载的js文件被压缩成了一行。  

![require.js](http://7xnv0h.com1.z0.glb.clouddn.com/792633534517225844.png)  

额。。。require.js部分暂时告一段落。。
