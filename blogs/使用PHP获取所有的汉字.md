title: 使用PHP获取所有的汉字  
date: 2013-11-19 08:00:00  
tags:   
    - PHP  
    - json  
    
---

```php
<?php
header('Content-Type: text/html;charset=utf8'); 
$start = hexdec('4e00'); $end = hexdec('9fa5');
for($i=$start; $i<$end; $i++) { 
    print_r(json_decode('["\u'.dechex($i).'"]'));
}  
```


4e00为汉字十六进制的下界，9fa5为汉字十六进制的上界，我们遍历每一个汉字的十六进制对应得十进制码，然后再通过json_decode解码得到最终的汉字。


