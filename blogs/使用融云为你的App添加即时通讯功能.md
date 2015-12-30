title: 使用融云为你的App添加即时通讯功能  
date: 2015-10-30 10:49:39  
tags:  
	- swift
	- iOS
	- 融云 

---

[使用swift语言，为项目添加一个单对单通讯功能]

### 简介
融云是一款第三方即时通讯SDK，通过融云平台，开发者不必搭建服务端硬件环境，就可以将即时通讯、实时网络能力快速集成至应用中。  
官网：[http://www.rongcloud.cn/](http://www.rongcloud.cn/)  
> 类似的SDK还有leancloud、环信等。  

### 创建应用  
在进行应用开发之前，需要前往[融云开发者平台](https://developer.rongcloud.cn/signin) 创建应用。  
获得App Key和App Secret 。

### 通过CocoaPods安装SDK
在Podfile中加入pod 'RongCloudIMKit'  
执行命令安装  

```bash  
$ pod install  
```  

安装完成后在swift与objective-c的桥接文件中导入SDK  

```swift  
#import <RongIMKit/RongIMKit.h>  
``` 

### 初始化SDK  
初始化SDK，通过token链接服务器，并设置当前的用户。  

```swift  
//初始化SDK
RCIM.sharedRCIM().initWithAppKey("cpj2xarljexxx")  
// 连接服务器
RCIM.sharedRCIM().connectWithToken("/3SuTgMXKxywuZcZSyVeUyKCc+4pFtAAaogrQ+3UGk7BaEgBD5YClHs6HVIYJGTtuuaS2B1/6ACXwLNzSRhJTA==", success: { (str:String!) -> Void in
    let currentUserInfo = RCUserInfo(userId: "001", name: "袁克强", portrait: nil)
    RCIMClient.sharedRCIMClient().currentUserInfo = currentUserInfo
    print("连接成功!")
}, error: {(error)-> Void in
    print(error);
}, tokenIncorrect: {()-> Void in
    print("token错误");
})  
```  


*注：这里使用写死的token与用户信息，实际开发中我们需要登录后保存用户信息，并通过UserId远程获取token 参考[http://www.rongcloud.cn/docs/server.html#_用户服务_原身份认证服务_](http://www.rongcloud.cn/docs/server.html#_用户服务_原身份认证服务_)，融云服务器不保存用户信息。*


### 启动单聊会话界面
融云IMKit的会话界面已经高度集成，你只需要启动会话界面。 
下面的例子是一个Button点击事件触发（聊天对象为自己）。

```swift  
func createConversation(){
    let conVC = RCConversationViewController();
    conVC.targetId = "001"	
    conVC.userName = "袁克强"
    conVC.conversationType = RCConversationType.ConversationType_PRIVATE
    conVC.title = v.userName;
    self.navigationController?.pushViewController(conVC, animated: true);
}  
    
```  

### 效果 
![ios](http://7xnv0h.com1.z0.glb.clouddn.com/0C756E83-9237-4359-8BE4-18B055DDC00B.png)    

至此，一个简单的类似微信的单对单通讯功能已经开发完毕。  



