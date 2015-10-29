title: 使用腾讯信鸽推送iOS消息        
date: 2015-04-10 08:06:17  
tags:   
    - swift  
    - iOS  
    - 推送         
 
---

近期App需要添加推送功能，选择了腾讯信鸽作为消息推送平台。  

### 信鸽

[信鸽](http://xg.qq.com/)（XG Push）是一款专业移动App推送平台，支持百亿级的通知/消息推送，秒级触达移动用户，现已全面支持Android和iOS两大主流平台。开发者可以方便地通过嵌入SDK，通过API调用或者Web端可视化操作，实现对特定用户推送，大幅提升用户活跃度，有效唤醒沉睡用户，并实时查看推送效果。  

简单记录开发过程（使用swift语言）：  

### 1. 接入应用  

登录[http://xg.qq.com/](http://xg.qq.com/)  创建应用，获得ACCESS ID及ACCESS KEY。

![xinge](http://7xnv0h.com1.z0.glb.clouddn.com/2437291823355654328.png)  

### 2. 制作证书并上传
官方提供了详细的证书制作指南 [http://developer.xg.qq.com/index.php/IOS_证书设置指南](http://developer.xg.qq.com/index.php/IOS_证书设置指南)  

制作完成后上传即可。  

![xinge](http://7xnv0h.com1.z0.glb.clouddn.com/2448832297400692896.png)  

### 3. 下载SDK及工程配置  

1. 下载信鸽SDK压缩包到本地并解压；

2. 创建或打开Xcode iOS工程；

3. 将XGSetting.h 和 XGPush.h 和 libXG-SDK.a添加到Xcode工程；

4. 添加对以下libraries的引用。包括CFNetwork.framework , SystemConfiguration.framework , CoreTelephony.framework , libz.dylib , libXG-SDK.a，Security.framework
![xinge](http://7xnv0h.com1.z0.glb.clouddn.com/Ios_pic1.png) 
5. 最后由于SDK是object-c版本的，在链接文件中引入头信息以便swift调用。  
\#import "XGPush.h"  
\#import "XGSetting.h"


### 4. 使用swift调用SDK
[参考技术开发文档](http://developer.xg.qq.com/index.php/IOS_SDK)  

主要相关代码(AppDelegate.swift)：  

```swift  

// ........

func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {

 XGPush.startApp(220010xxxx,appKey:"IVPV4T39xxxx");
   XGPush.initForReregister { () -> Void in
        if(!XGPush.isUnRegisterStatus()){
            println("注册通知");
			if(UIApplication.instancesRespondToSelector(Selector( "registerUserNotificationSettings:" ))){
                application.registerUserNotificationSettings ( UIUserNotificationSettings (forTypes:  UIUserNotificationType.Sound |  UIUserNotificationType.Alert |  UIUserNotificationType.Badge, categories:  nil ))
                application.registerForRemoteNotifications();
            } else {
                application.registerForRemoteNotificationTypes(.Alert | .Sound | .Badge);
            }
        }
    }
    //推送反馈(app不在前台运行时，点击推送激活时)
    XGPush.handleLaunching(launchOptions)
}

// 远程推送注册成功，获取deviceToken
func application(application: UIApplication , didRegisterForRemoteNotificationsWithDeviceToken deviceToken: NSData ) {
    var  deviceTokenStr : String = XGPush.registerDevice(deviceToken);
    println(deviceTokenStr)
}


// 远程推送注册失败
func application(application: UIApplication , didFailToRegisterForRemoteNotificationsWithError error: NSError ) {
    if error.code == 3010 {
        println ( "Push notifications are not supported in the iOS Simulator." )
    } else {
        println ( "application:didFailToRegisterForRemoteNotificationsWithError: \(error) " )
    }
}

   
//app在前台运行时收到远程通知
func application(application: UIApplication, didReceiveRemoteNotification userInfo: [NSObject : AnyObject]) {
    println(userInfo);
    XGPush.handleReceiveNotification(userInfo);
}

//收到本地通知
func application(application: UIApplication, didReceiveLocalNotification notification: UILocalNotification) {
    // ...
}  

``` 


### 5. 进行验证测试  

根据之前代码里获得的deviceToken添加一台测试设备。  
![xinge](http://7xnv0h.com1.z0.glb.clouddn.com/979532918971158346.png)  

在腾讯信鸽平台创建一条开发环境的推送，手机就能收到推送的消息了。  

![xinge](http://7xnv0h.com1.z0.glb.clouddn.com/3833689182899793133.png)


  
