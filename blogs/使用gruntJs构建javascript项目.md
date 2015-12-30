title: 使用gruntJs构建javascript项目  
date: 2013-10-15 08:00:00    
tags:   
    - javascript  
    - grunt  
    - 自动化 
    
---
gruntJs，一个专为JavaScript提供的自动化项目构建工具。. 如果你需要重复的执行像压缩, 编译, 单元测试, 代码检查以及打包发布的任务. 那么你可以使用Grunt来处理这些任务, 你所需要做的只是配置好Grunt, 这样能很大程度的简化你的工作.

官网 : [http://gruntjs.com/](http://gruntjs.com/)  
github : [https://github.com/gruntjs/grunt](https://github.com/gruntjs/grunt)  

### 安装grunt  
``` bash  
$ npm install -g grunt-cli  
$ npm install -g grunt  
```  
恩，你没看错。通过npm(Node Package Manager)只需要2条命令就完成了grunt的安装。

### 在项目中使用grunt
想要在项目中使用grunt，首先需要往项目里添加两个文件:package.json和Gruntfile.js
package.json:该文件用来为npm存放项目配置的元数据，基于Packages/1.1 规范。  
在这个文件里你可以定义你的应用名称( name )、应用描述( description )、应用依赖模块( dependencies )、开发环境依赖模块( devDependencies )等。  
如：  
```javascript
{
    "name": "hello world",
    "author": "ykq",
    "homepage": "http://iscript.github.io", 
     "dependencies": { 
         "express": "latest" 
     }, 
     "devDependencies": { 
         "grunt": "latest", 
         "grunt-contrib-uglify": "latest", 
     } 
} 
```  

接着在项目目录下执行npm install即可根据package.json安装项目所需的依赖。

Gruntfile.js:这个文件就是grunt的配置了，其中详细定义了每个任务的细节和执行任务的顺序等。  
  
```javascript
module.exports = function(grunt) {
grunt.initConfig({
	pkg: grunt.file.readJSON('package.json'),
	//压缩任务
	uglify : {
		options : {
		banner : '/*! <%= pkg.name %> */\n'
		},
		build : { src : 'app.js',dest : 'app.min.js'} 
	} 
}); 
//加载grunt需要的插件： 
grunt.loadNpmTasks('grunt-contrib-uglify'); 
 
//告诉grunt该怎么执行这些任务： 
grunt.registerTask('default', ['uglify']); 
}; 
 ```  
 
上面这段代码就是通过grunt来执行一个简单的压缩任务。  
在项目所在目录，命令行输入grunt即可将app.js压缩成app.min.js。

### grunt插件
grunt只是一个自动化管理工具，不管是安装或配置都不复杂，复杂度源于成百上千的第三方插件。  
常用插件：  
* grunt-contrib-uglify ：压缩js代码
* grunt-contrib-concat ：合并js文件
* grunt-contrib-qunit ：单元测试
* grunt-contrib-jshint ：js代码检查
* grunt-contrib-watch ：监控文件修改并重新执行注册的任务 

官方插件清单:[http://gruntjs.com/plugins](http://gruntjs.com/plugins)

#### 使用ng-Boilerplate来通过grunt完美构建AngularJs项目
[https://github.com/ngbp/ng-boilerplate](https://github.com/ngbp/ng-boilerplate)











