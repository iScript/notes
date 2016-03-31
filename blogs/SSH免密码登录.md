title : SSH免密码登录
date : 2016-03-31 16:25:00 
tags : 
		- Linux
		- ssh  


---

### SSH
SSH（Secure Shell ）是一种网络协议，用于计算机之间的加密登录。

### 公钥登录
公钥（Public Key）与私钥（Private Key）是通过一种算法得到的一个密钥对（即一个公钥和一个私钥），公钥是密钥对中公开的部分，私钥则是非公开的部分。  
所谓"公钥登录"，原理很简单，就是用户将自己的公钥储存在远程主机上。登录的时候，远程主机会向用户发送一段随机字符串，用户用自己的私钥加密后，再发回来。远程主机用事先储存的公钥进行解密，如果成功，就证明用户是可信的，直接允许登录shell，不再要求密码。

### 具体步骤

##### 1. 通过ssh-keygen生成秘钥   
   
```    
$ ssh-keygen -t rsa    
```  
 
-t指定加密类型为rsa，运行该命令以后，会在$HOME/.ssh/目录下，会新生成两个文件：id_rsa和id_rsa.pub。前者是你的私钥，后者是你的公钥。
![袁克强的博客](http://dn-yuankeqiang.qbox.me/0F6F97CD-7321-47A2-872A-5D881797572D.png?imageView/2/w/500)

###### 2. 复制公钥到远程服务器   
  
```  
$ scp id_rsa.pub root@myhost.com:~/.ssh/id_rsa.pub  
```  

###### 3. 将公钥保存到authorized_keys文件
登录远程服务器后，将公钥保存到authorized_keys文件。  

```    
$ cd ~/.ssh 
$ cat id_rsa.pub >> authorized_keys   
 
```  

---
至此，即可免密码登录远程服务器。



