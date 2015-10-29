title: linux下的定时任务crontab       
date: 2014-08-27 08:06:17  
tags:   
    - linux  
    - PHP   
    - 定时       
 
---

[环境centos 6.5]
  
### 前言  
crontab命令常见于Unix和类Unix的操作系统之中，用于设置周期性被执行的指令。该命令从标准输入设备读取指令，并将其存放于“crontab”文件中(/var/spool/cron/以用户命名的文件)，以供之后读取和执行。该词来源于希腊语 chronos(χρνο)，原意是时间。  

通常，crontab储存的指令被守护进程 - crond激活在后台运行，每一分钟检查是否有预定的作业需要执行。这类作业一般称为cron jobs。  

### 启动crond进程

```bash
$ service crond start  
```

若没安装请先安装：  

```bash  
$ yum install vixie-cron
$ yum install crontabs  
```  

### crontab常见命令：  

crontab -e 编辑crontab文件，编辑后crond进程自动读取
crontab -l 列出用户crontab文件的详细内容
crontab -r 删除crontab文件  

### crontab文件格式 

crontab文件由6部分组成  

1. minute 一小时中的哪一分钟[0-59]
2. hour 一天中的哪一小时[0-23]
3. day-of-month 一月中的哪一天[1-31]
4. month-of-year 一年中的哪一月[1-12]
5. day-of-week 一周中的哪一天[0-6]
6. commands 执行的命令
 
![ykq](http://7xnv0h.com1.z0.glb.clouddn.com/6608553066097013002.jpg)

这些选项都不能为空，如果用户不需要制定其中的几项，可以使用*表示任何时间。

每个时间字段都可以指定多个值，可以用逗号隔开, 5-8 */5   

```bash  
15 3 * * 1-5 echo 111 > aa.txt    
```  

如上面标示每周一到周5的3点15分执行该计划任务


### 哪些用户可以使用crontab命令  

* /etc/cron.allow 如果这个文件存在，那么只有在此文件中的用户可以使用crontab命令，如果文件不存在则查找/etc/cron.deny
* /etc/cron.deny 如果这个文件存在，则在此文件中的用户都不能使用crontab命令
* 如果2个文件都不存在，则只有root能使用crontab命令
* 如果2个文件都存在，且均为空，则所有用户都能使用crontab命令  

### 定时任务结合PHP  

定时执行php很简单，只需要把命令换成php脚本就行了，如下表示每隔1分钟执行index.php  

```bash  
*/1 * * * * php /usr/www/test/index.php  
```  


