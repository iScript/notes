title: 细说iOS消息推送   
date: 2015-11-26 17:00:39  
tags:  
	- swift  
	- iOS  
	- 推送  
	 

---

>[转载自[leancloud](https://blog.leancloud.cn/1163/)]

###  APNs 的推送机制 
与 Android 上我们自己实现的推送服务不一样，Apple 对设备的控制非常严格，消息推送的流程必须要经过 APNs：  

![swift](https://dn-yuankeqiang.qbox.me/remote_notif_simple_2x.png)  

这里 Provider 是指某个应用的 Developer，当然如果开发者使用 LeanCloud 的服务，把发送消息的请求委托给我们，那么这里的 Provider 就是 LeanCloud 的推送服务程序了。上图可以分为三步：  

1. LeanCloud 推送服务程序把要发送的消息、目的设备的唯一标识打包，发给 APNs。  
2. APNs 在自身的已注册 Push 服务的应用列表中，查找有相应标识的设备，并把消息发送到设备。  
3. iOS 系统把发来的消息传递给相应的应用程序，并且按照设定弹出 Push 通知。  

为了实现消息推送，有两点非常重要：  

1、App 的推送证书  
要能够完整实现一条消息推送，需要我们在 App ID 中打开 Push Notifications，需要我们准备好 Provisioning Profile 和 SSL 证书，并且一定要注意 Development 和 Distribution 环境是需要分开的。最后，把 SSL 证书导入到 LeanCloud 平台，就可以尝试远程消息推送了。具体的操作流程可以参考我们的使用指南：iOS 推送证书设置指南 。  

2、设备标识 DeviceToken  
知道了谁要推送，或者说要推送给哪个应用之后，APNs 还需要知道推到哪台设备上，这就是设备标识的作用。获取设备标识的流程如下：  
	1. 应用打开推送开关，用户要确认 TA 希望获得该应用的推送消息；  
	2. 应用获得一个 DeviceToken；  
	3. 应用将 DeviceToken 保存起来，这里就是通过 [AVInstallation saveInBackground] 将 DeviceToken 保存到 LeanCloud；  
	4. 当某些特定事件发生，开发者委托 LeanCloud 来发送推送消息，这时候 LeanCloud 的推送服务器就会给 APNs 发送一则推送消息，APNs 最后消息送到用户设备。  

[leancloud](https://dn-yuankeqiang.qbox.me/registration_sequence_2x.png)  

### 推送相关的几个概念  

#### 消息类型  
一条消息推送过来，可以有如下几种表现形式：  
* 显示一个 alert 或者 banner，展现具体内容。  
* 在应用 icon 上提示一个新到消息数。  
* 播放一段声音。  

开发者可以在每次推送的时候设置，在推送达到用户设备时开发者也可以选择不同的提示方式。  

#### 本地消息通知  
iOS 上有两种消息通知，一种是本地消息（Local Notification），一种是远程消息 (Push Notification，也叫 Remote Notification)，设计这两种通知的目的都是为了提醒用户，现在有些什么新鲜的事情发生了，吸引用户重新打开应用。  

本地消息什么时候有用呢？譬如你正在做一个 To-do 的工具类应用，对于用户加入的每一个事项，都会有一个完成的时间点，用户可以要求这个 To-do 应用在事项过期之前的某一个时间点提醒一下 TA。为了达到这一目的，应用就可以调度一个本地通知，在时间点到了之后发出一个 Alert 消息或者其他提示。  

我们在处理推送消息的时候，也可以综合运用这两种方式。  

#### 代码里面如何实现推送 
首先，我们要获取 DeviceToken。  

应用需要每次启动的时候都去注册远程通知——通过调用 UIApplication 的 registerForRemoteNotificationTypes: 方法，传递给它你希望支持的消息类型参数即可，例如：  

```objective-c  
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    // do some initiale working
    ...

    [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeSound];
    return YES;
}    
```

如果注册成功，APNs 会返回给你设备的 token，iOS 系统会把它传递给 app delegate 代理 application:didRegisterForRemoteNotificationsWithDeviceToken: 方法，你应该在这个方法里面把 token 保存到 LeanCloud 后台，例如：  

```objective-c    
- (void)application:(UIApplication *)app didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    NSLog(@"Receive DeviceToken: %@", deviceToken);
    AVInstallation *currentInstallation = [AVInstallation currentInstallation];
    [currentInstallation setDeviceTokenFromData:deviceToken];
    [currentInstallation saveInBackground];
}  
```
  
如果注册失败，application:didFailToRegisterForRemoteNotificationsWithError: 方法会被调用，通过 NSError 参数你可以看到具体的出错信息，例如：  
  
```objective-c  
  
- (void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error {
    NSLog(@"注册失败，无法获取设备 ID, 具体错误: %@", error);
}  
```
    

请注意，注册流程需要在应用每次启动时调用，这并不不会带来额外的负担，因为 iOS 操作系统在第一次获得了有效的 device token 之后，会本地缓存起来，以后应用再调用 registerForRemoteNotificationTypes: 的时候会立刻返回，并不会再进行网络请求。另外，应用层面不应该对 device token 进行缓存，因为 device token 也有可能变化——如果用户重装了操作系统，那么 APNs 再次给出的 device token 就会和之前的不一样，又或者是，用户 恢复了原来的备份到新的设备上，那么原来的 device token 也会失效。  

其次，我们要处理收到消息之后的回调。  

我们可以设想一下消息通知的几种使用场景：  

1/ 在应用没有被启动的时候，接收到了消息通知。这时候操作系统会按照默认的方式来展现一个 alert 消息，在应用的 icon 上标记一个数字，甚至播放一段声音。  
2/ 用户看到消息之后，点击了一下 action 按钮或者点击了应用图标。如果 action 按钮被点击了，系统会通过调用 application:didFinishLaunchingWithOptions: 这个代理方法来启动应用，并且会把 notification 的 payload 数据传递进去。如果应用图标被点击了，系统也一样会调用 application:didFinishLaunchingWithOptions: 这个代理方法来启动应用，唯一不同的是这时候启动参数里面不会有任何 notification 的信息。示例代码如下：

```objective-c  
  
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    // do initializing works
    ...

    if (launchOptions) {
        // do something else
        ...

        [AVAnalytics trackAppOpenedWithLaunchOptions:launchOptions];
    }

    [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeSound];

    return YES;
}   

```   
 
3/ 如果远程消息发送过来的时候，应用正在运行，这时候会发生什么呢？应用代理的 application:didReceiveRemoteNotification: 方法会被调用，同时远程消息中的 payload 数据会作为参数传递进去。示例代码如下：  
 

``` objective-c  
 
 
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
    if (application.applicationState == UIApplicationStateActive) {
        // 转换成一个本地通知，显示到通知栏，你也可以直接显示出一个 alertView，只是那样稍显 aggressive：）
        UILocalNotification *localNotification = [[UILocalNotification alloc] init];
        localNotification.userInfo = userInfo;
        localNotification.soundName = UILocalNotificationDefaultSoundName;
        localNotification.alertBody = [[userInfo objectForKey:@"aps"] objectForKey:@"alert"];
        localNotification.fireDate = [NSDate date];
        [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];
    } else {
        [AVAnalytics trackAppOpenedWithRemoteNotificationPayload:userInfo];
    }
}   
 
```   




   
  
