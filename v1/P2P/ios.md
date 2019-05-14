# iOS

## 一、概述

### 简介

P2P音视频可以实现一对一单聊，内置推送服务，保证必达；支持视频、语音、优先视频等多种呼叫模式，适用于网络电话、社交、企业通信等场景。

## 二、集成指南


### 适用范围

本集成文档适用于iOS RTCP2PEngine SDK 2.0.0 ~ 3.0.0版本。

### 准备环境

- Xcode 9.0+。
- iOS 8.0+ 真机（iPhone 或 iPad）。
- 请确保你的项目已设置有效的开发者签名。

### 导入SDK

**CocoaPods导入**

* 通过 Cocoapods 下载地址：

```
pod 'RTCP2PEngine'
```
* 如果需要安装指定版本则使用以下方式（以 3.0.0 版本为例）：

```
pod 'RTCP2PEngine', '3.0.0'
```

**手动导入**

* 前往GitHub[下载Demo](https://github.com/anyRTC/anyRTC-RTCP-iOS)，找到RTCP2PEngine.framework；

* 在Xcode中选择“Add files to 'Your project name'...”，将RTCP2PEngine.framework添加到你的工程目录中
![1.png](https://upload-images.jianshu.io/upload_images/2478176-bab8dde2048c627e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 打开General->Embedded Binaries中添加RTCP2PEngine.framework

![2.png](https://upload-images.jianshu.io/upload_images/2478176-5415c862146eec63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 权限说明

使用RTCP2PEngine SDK 前，需要对设备进行授权。打开 info.plist ，点击 + 图标开始添加：

* 添加设备使用「网络」的权限
```
	<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSAllowsArbitraryLoads</key>
		<true/>
	</dict>
```

* 添加设备使用「相机」的权限
```
	<key>NSCameraUsageDescription</key>
	<string>XXX请求访问相机用于...</string>
```

* 添加设备使用「麦克风」的权限

```
	<key>NSMicrophoneUsageDescription</key>
	<string>XXX请求访问麦克风用于...</string>
```

### 后台模式(Background Modes)

勾选Audio, AirPlay and Picture in Picture

![3.png](https://upload-images.jianshu.io/upload_images/2478176-944fda94bbdb6fda.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 三、API接口文档

### ARRtcpEngine 接口类

#### 1. 配置开发者信息

**定义**

```
+ (void)initEngine:(NSString *)appId token:(NSString *)token;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
appId | NSString | appId
token | NSString | token

**说明**

该方法为配置开发者信息，上述参数均可在[https://www.anyrtc.io/ ](https://www.anyrtc.io/)应用管理中获得，建议在AppDelegate.m调用。

#### 2. 配置私有云

**定义**

```
+ (void)configServerForPriCloud:(NSString *)address port:(int)port;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
address | NSString | 私有云服务地址
port | int | 私有云服务端口

**说明**

配置私有云信息，当使用私有云时才需要进行配置，默认无需配置。

#### 3. 获取SDK版本号

**定义**

```
+ (NSString *)getSDKVersion;
```
**返回值**

P2P SDK版本号

#### 4. 设置打印日志级别

**定义**

```
+ (void)setLogLevel:(ARLogModel)levelModel;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
levelModel | ARLogModel | 日志级别

### ARP2PKit 接口类

#### 1. 实例化P2P对象

**定义**

```
- (instancetype)initWithDelegate:(id <ARP2PKitDelegate>)delegate option:(ARP2POption *)option;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
delegate | id <ARP2PKitDelegate> | RTC相关回调代理
option | ARP2POption | 配置信息

**返回**

P2P实例对象。

#### 2. 设置token验证

**定义**

```
- (BOOL)setUserToken:(NSString *)userToken;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userToken | NSString | token字符串，客户端向自己服务器申请

**说明**

设置token验证必须放在turnOn之前。 

#### 3. 上线

**定义**

```
- (void)turnOn:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户自己平台的Id，确保唯一，不能为空

**说明**

如果更换用户，需先下线(turnOff)。

#### 4. 下线

**定义**

```
- (void)turnOff;
```

#### 5. 设置本地显示窗口

**定义**

```
- (void)setLocalVideoCapturer:(UIView *)render;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView | 本地视频窗口

#### 6. 设置滤镜

**定义**

```
- (void)setCameraFilter:(ARCameraFilterMode)filter;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
filter | ARCameraFilterMode | 滤镜模式

**说明**

只有使用美颜相机模式才有用。

#### 7. 呼叫

**定义**

```
- (void)makeCall:(NSString *)userId callModel:(ARP2PCallModel)callMode userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫的用户Id
callMode | ARP2PCallModel | 呼叫模式
userData | NSString | 用户自己的自定义信息

#### 8. 挂断通话

**定义**

```
- (void)endCall:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫的用户Id

#### 9. 同意通话

**定义**

```
- (void)acceptCall:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫的用户Id

#### 10. 拒绝通话

**定义**

```
- (void)rejectCall:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫的用户Id

#### 11. 设置其他视频显示窗口

**定义**

```
- (void)setRemoteVideoRender:(UIView *)render userId:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView | 视频显示窗口
userId | NSString | 呼叫的用户Id

#### 12. 切换音频模式

**定义**

```
- (BOOL)onRTCSwithToAudioMode;
```

**说明**

操作是否成功。 

#### 13. 发送消息

**定义**

```
- (void)sendUserMessage:(NSString *)userId message:(NSString *)content;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 发送消息者的用户Id
content | NSString | 发送消息的内容(内容限制在512个字节内)

#### 14. 抓取对方的图片

**定义**

```
- (BOOL)snapPicture:(NSString *)filePath;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
filePath | NSString | 本地存储的地址＋图片名称

#### 15. 开始录制对方的视频

**定义**

```
- (BOOL)startRecordVideo:(NSString *)filePath;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
filePath | NSString | 本地存储的地址＋视频名称

#### 16. 停止录制对方的视频

**定义**

```
- (void)stopRecordVideo;
```

#### 17. 设置本地音频是否传输

**定义**

```
- (void)setLocalAudioEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES为传输音频，NO为不传输音频，默认音频传输

#### 18. 设置本地视频是否传输

**定义**

```
- (void)setLocalVideoEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES为传输视频，NO为不传输视频，默认视频传输

#### 19. 切换前后摄像头

**定义**

```
- (void)switchCamera;
```

#### 20. 设置扬声器开关

**定义**

```
- (void)setSpeakerOn:(BOOL)on;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
on | BOOL | YES打开扬声器，NO关闭扬声器，默认扬声器打开

#### 21. 清空会话

**定义**

```
- (void)close;
```

**说明**

当别人挂断或者自己挂断离开的时候，调用该方法用于清空本地视频。 

### ARP2PKitDelegate 接口类

#### 1. 服务器链接成功

**定义**

```
- (void)onRTCConnected;
```

#### 2. 服务器断开

**定义**

```
- (void)onRTCDisconnect:(ARP2PCode)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | ARP2PCode | 状态码

#### 3. 收到呼叫的回调

**定义**

```
- (void)onRTCMakeCall:(NSString *)userId callModel:(ARP2PCallModel)callMode userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫方的Id
callMode | ARP2PCallModel | 呼叫类型
userData | NSString | 呼叫方的自定义信息

#### 4. 收到对方同意的回调

**定义**

```
- (void)onRTCAcceptCall:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫方的Id

#### 5. 收到对方拒绝的回调

**定义**

```
- (void)onRTCRejectCall:(NSString *)userId code:(ARP2PCode)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 被呼叫方的Id
code | ARP2PCode | 状态码

#### 6. 收到对方挂断的回调

**定义**

```
- (void)onRTCEndCall:(NSString *)userId code:(ARP2PCode)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户id
code | ARP2PCode | 状态码

#### 7. 收到对方视频视频即将显示的回调

**定义**

```
- (void)onRTCOpenRemoteVideoRender:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫的用户Id

**说明**

可以调用setRemoteVideoRender显示对方视频窗口。

#### 8. 收到对方视频离开的回调

**定义**

```
- (void)onRTCCloseRemoteVideoRender:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫的用户Id

#### 9. 切换到音频模式

**定义**

```
- (void)onRTCSwithToAudioMode;
```

#### 10. 收到消息回调

**定义**

```
- (void)onRTCUserMessage:(NSString *)userId message:(NSString *)content;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户id
content | NSString | 消息内容

## 四、更新日志

**Version 3.0.0 （2019-01-18）**

* SDK版本升级3.0，API接口变更，更加简洁规范

**Version 2.0.0 （2017-09-30）**

* SDK版本升级2.0，梳理、完善SDK

## 五、错误码对照表

以下为介绍 iOS RTCP2PEngine SDK 的错误码。

名称 | 值            | 备注
---|------------------------------|----
ARP2P_OK | 0 | 正常
ARP2P_UNKNOW | 1 | 未知错误
ARP2P_EXCEPTION | 2 | SDK调用异常
ARP2P_EXP_UNINIT | 3 | SDK未初始化
ARP2P_EXP_PARAMS_INVALIDE | 4 | 参数非法
ARP2P_EXP_NO_NETWORK | 5 | 没有网络链接
ARP2P_EXP_NOT_FOUND_CAMERA | 6 | 没有找到摄像头设备
ARP2P_EXP_NO_CAMERA_PERMISSION | 7 | 没有打开摄像头权限
ARP2P_EXP_NO_AUDIO_PERMISSION | 8 | 没有音频录音权限
ARP2P_EXP_NOT_SUPPORT_WEBRTC | 9 | 浏览器不支持原生的webrtc
ARP2P_NET_ERR | 100 | 网络错误 
ARP2P_NET_DISSCONNECT | 101 | 网络断开
ARP2P_LIVE_ERR | 102 | 直播出错
ARP2P_EXP_ERR | 103 | 异常错误
ARP2P_EXP_Unauthorized | 104 | 服务未授权(仅可能出现在私有云项目)
ARP2P_BAD_REQ | 201 | 服务不支持的错误请求
ARP2P_AUTH_FAIL | 202  | 认证失败
ARP2P_NO_USER | 203 | 此开发者信息不存在
ARP2P_SVR_ERR | 204 | 服务器内部错误
ARP2P_SQL_ERR | 205 | 服务器内部数据库错误
ARP2P_ARREARS | 206 | 账号欠费
ARP2P_LOCKED | 207 | 账号被锁定
ARP2P_SERVER_NOT_OPEN | 208 | 服务未开通
ARP2P_ALLOC_NO_RES | 209 | 没有服务器资源
ARP2P_SERVER_NOT_SURPPORT | 210 | 不支持的服务
ARP2P_FORCE_EXIT | 211 | 强制离开
ARP2P_PEER_BUSY | 800 | 对方正忙
ARP2P_OFFLINE | 801 | 对方不在线
ARP2P_NOT_SELF | 802 | 不能呼叫自己
ARP2P_EXP_OFFLINE | 803 | 通话中对方意外掉线
ARP2P_EXP_EXIT | 804 | 对方异常导致(如：重复登录帐号将此前的帐号踢出)
ARP2P_TIMEOUT | 805 | 呼叫超时(45秒)
ARP2P_NOT_SURPPORT | 806 | 不支持

  



