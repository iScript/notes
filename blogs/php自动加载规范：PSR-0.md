title: php自动加载规范：PSR-0       
date: 2014-02-15 08:06:17  
tags:   
    - PHP  
    - psr    
    - 翻译       
 
---

[翻译自官网[http://www.php-fig.org/psr/psr-0/](http://www.php-fig.org/psr/psr-0/)]  

  
### 自动加载规范
下面描述了自动加载操作时必须遵守的强制要求。  

### 要求：
* 一个完全合格的命名空间和类名必须有以下的结构 \<Vendor Name>\(<Namespace>\)*<Class Name>
* 每个命名空间必须有提供者名称("Vendor Name")作为顶级的命名空间
* 每个命名空间可以有多个子命名空间
* 每个命名空间在被从文件系统加载时须将分隔符转换为"系统分隔符"(DIRECTORY_SEPARATOR)
* 每个"_"字符在"类名"中应被转换为系统分隔符。"_"符号在命名空间中没有特殊含义
* 加载文件时完全合格的命名空间和类名必须以".php"结尾
* 提供者名称，命名空间，类名可以由任意大小写字母组成   


### 举个栗子

```php  
\Doctrine\Common\IsolatedClassLoader => /path/to/project/lib/vendor/Doctrine/Common/IsolatedClassLoader.php
\Symfony\Core\Request => /path/to/project/lib/vendor/Symfony/Core/Request.php
\Zend\Acl => /path/to/project/lib/vendor/Zend/Acl.php
\Zend\Mail\Message => /path/to/project/lib/vendor/Zend/Mail/Message.php   
  
```  

### 下划线在命名空间及类中的使用  

```php  
  
\namespace\package\Class_Name => /path/to/project/lib/vendor/namespace/package/Class/Name.php
\namespace\package_name\Class_Name => /path/to/project/lib/vendor/namespace/package_name/Class/Name.php  
  
```  

### 示例实现 
下面是一个简单的示例函数演示如何使用上述标准来自动加载。  

```php 
    
<?php
function autoload($className)
{
    $className = ltrim($className, '\\');
    $fileName  = '';
    $namespace = '';
    if ($lastNsPos = strrpos($className, '\\')) {
        $namespace = substr($className, 0, $lastNsPos);
        $className = substr($className, $lastNsPos + 1);
        $fileName  = str_replace('\\', DIRECTORY_SEPARATOR, $namespace) . DIRECTORY_SEPARATOR;
    }
    $fileName .= str_replace('_', DIRECTORY_SEPARATOR, $className) . '.php';
    require $fileName;
}   
   
```  


### SplClassloader的实现  
接下来这个gist是一个SplClassLoader实现例子，通过它你可以加载按照上面标准来实现的通用类库，这是目前推荐的方式加载遵循这些标准的php类。

[http://gist.github.com/221634](http://gist.github.com/221634)
