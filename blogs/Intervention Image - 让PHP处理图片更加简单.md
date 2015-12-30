title: Intervention Image - 让PHP处理图片更加简单       
date: 2014-03-29 08:06:17  
tags:   
    - PHP  
    - 图片     
    - GD库       
 
---

### 简介
Intervention Image是一个基于PHP GD库的图片处理库。  
相对于使用PHP原生图片处理函数的复杂和繁琐，该库能让我们处理图片变得更加的简单和富有表现力。  

github : [https://github.com/Intervention/image](https://github.com/Intervention/image)
官方文档 : [http://intervention.olivervogel.net/image/getting_started/introduction](http://intervention.olivervogel.net/image/getting_started/introduction)
  
  
### Intervention Image使用
Intervention Image的安装和使用都十分的简单，安装只需敲一下composer install的命令，使用也很简单，官方文档写的很详细就不多说了，这里本人简单演示下使用Intervention Image库生成验证码和给图片打水印。  

### 绘制验证码 

```php 

require 'vendor/autoload.php';
use Intervention\Image\Image;
$charset = "ABCDEFGHKMNPRSTUVWXYZ23456789"; 
$cWidth = 150;  //画布宽度
$cHeight = 30;  //画布高度
$code = "";
$color =  array('#99c525','#fc9721','#8c659d','#00afd8');
$img = Image::canvas($cWidth, $cHeight, '#ccc');
for ($i=0;$i<4;$i++) {
    //画出干扰线
    $img->line($color[array_rand($color,1)],mt_rand(0,$cWidth),mt_rand(0,$cHeight),mt_rand(0,$cWidth),mt_rand(0,$cHeight));
    //随机取出验证码
    $code .= $charset[mt_rand(0,strlen($charset)-1)];
    //画出验证码
    $img->text($code[$i],(30*$i)+10,25,function($font) use($color){
        $font->file('fonts/Arial.ttf');
        $font->size(24);
        $font->color($color[array_rand($color,1)]);
        $font->angle(mt_rand(-30,30));
    });
}
echo $img->response();  
```  

![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/1597651967909882829.png)  

### 图片水印  

```php  
require 'vendor/autoload.php';
use Intervention\Image\Image;
$img = Image::make('bg.jpg')->resize(400,250)->text('@袁克强',330,240,function($font){
    $font->file('fonts/华文黑体.ttf');
    $font->size(18);
    $font->color("#fff");
    $font->angle(20);
})->save('bg.jpg',75);  
```  

![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/6597579940053741825.png)


