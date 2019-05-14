# iOS

## 一、概述

### 简介

实时直播SDK能够实现一对一、一对多的纯音频和视频实时直播，相比RTMPC延时更低、极简API接口。适用于在线娃娃机、智能硬件、在线医疗、视频招聘、相亲交友等多种场景。

## 二、集成指南

### 适用范围

本集成文档适用于iOS RTCPEngine SDK 2.0.0 ~ 3.0.0版本。

### 准备环境

- Xcode 9.0+。
- iOS 8.0+ 真机（iPhone 或 iPad）。
- 请确保你的项目已设置有效的开发者签名。

### 导入SDK

**CocoaPods导入**

* 通过 Cocoapods 下载地址：

```
pod 'RTCPEngine'
```
* 如果需要安装指定版本则使用以下方式（以 3.0.0 版本为例）：

```
pod 'RTCPEngine', '3.0.0'
```

**手动导入**

* 前往GitHub[下载Demo](https://github.com/anyRTC/anyRTC-RTCP-iOS)，找到RTCPEngine.framework；

* 在Xcode中选择“Add files to 'Your project name'...”，将RTCPEngine.framework添加到你的工程目录中
![1.png](https://upload-images.jianshu.io/upload_images/2478176-9655284250ad1070.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 打开General->Embedded Binaries中添加RTCPEngine.framework

![1.png](https://upload-images.jianshu.io/upload_images/2478176-7dbc0f736825ad6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 权限说明

使用RTCPEngine SDK 前，需要对设备进行授权。打开 info.plist ，点击 + 图标开始添加：

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

![1.png](https://upload-images.jianshu.io/upload_images/2478176-b402ff28caf96212.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

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

RTCP SDK版本号

#### 4. 设置打印日志级别

**定义**

```
+ (void)setLogLevel:(ARLogModel)levelModel;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
levelModel | ARLogModel | 日志级别

### ARRtcpKit 接口类

#### 1. 实例化RTCPKit对象

**定义**

```
- (instancetype)initWithDelegate:(id<ARRtcpKitDelegate>)delegate userId:(NSString *)userId userData:(NSString *)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
delegate | id<ARRtcpKitDelegate> | RTCP相关回调代理
userId | NSString | 用户自己平台的Id，确保唯一，不能为空
userData | NSString | 用户自定义信息

**返回**

实时直播对象。

#### 2. 设置本地视频采集窗口

**定义**

```
- (void)setLocalVideoCapturer:(UIView *)render option:(ARRtcpOption *)option;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView | 视频显示对象
option | ARRtcpOption | 配置信息

#### 3. 设置本地显示模式

**定义**

```
- (void)updateLocalVideoRenderModel:(ARVideoRenderMode)videoRenderMode;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
videoRenderMode | ARVideoRenderMode| 显示模式，默认ARVideoRenderScaleAspectFill，等比例填充视图模式

#### 4. 设置本地音频是否传输

**定义**

```
- (void)setLocalAudioEnable:(BOOL)enable;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL| YES传输音频，NO不传输音频，默认传输

#### 5. 设置本地视频是否传输

**定义**

```
- (void)setLocalVideoEnable:(BOOL)enable;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL| YES传输视频，NO为不传输视频，默认视频传输

#### 6. 获取本地音频传输是否打开

**定义**

```
- (BOOL)localAudioEnabled;
```

**返回值**

音频传输与否

#### 7. 获取本地视频传输是否打开

**定义**

```
- (BOOL)localVideoEnabled;
```

**返回值**

视频传输与否

#### 8. 设置扬声器开关

**定义**

```
- (void)setSpeakerOn:(BOOL)on;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
on | BOOL | YES打开扬声器，NO关闭扬声器，默认打开扬声器

#### 9. 切换前后摄像头

**定义**

```
- (void)switchCamera;
```

#### 10. 设置本地前置摄像头镜像是否打开

**定义**

```
- (BOOL)setFontCameraMirrorEnable:(BOOL)enable;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES打开，NO关闭

**返回**

本地前置摄像头镜像是否成功打开

#### 11. 前置摄像头是否镜像

**定义**

```
- (BOOL)fontCameraMirror;
```

**返回值**

是否镜像，默认打开。

#### 12. 禁止接收某人视频

**定义**

```
- (int)muteRemoteVideoStream:(BOOL)mute pubId:(NSString *)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
mute | BOOL | YES禁止，NO接收
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

**返回值**

操作是否成功

#### 13. 禁止接收某人音频

**定义**

```
- (int)muteRemoteAudioStream:(BOOL)mute pubId:(NSString *)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
mute | BOOL | YES禁止，NO接收
pubId | BOOL | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

**返回值**

操作是否成功

#### 14. 设置token验证

**定义**

```
- (BOOL)setUserToken:(NSString *)userToken;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userToken | NSString | token字符串，客户端向自己服务器申请

**返回**

设置token验证是否成功。

**说明**

设置token验证必须放在publish、subscribe之前。

#### 15. 发布媒体

**定义**

```
- (void)publish:(int)mediaType anyRTCId:(NSString *)anyRTCId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
nMediaType | int | 0为发布音视频，1为只发布音频
anyRTCId | NSString | 该参数可以随意填写，但是不能为空，如果发布成功，SDK会给你分配一个频道Id

#### 16. 取消发布媒体

**定义**

```
- (void)unPublish;
```
**说明**

取消发布媒体之后，下次再发布的时候，还需要在调用setLocalVideoCapturer

#### 17. 订阅视频

**定义**

```
- (void)subscribe:(NSString *)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

#### 18. 取消订阅媒体流

**定义**

```
- (void)unSubscribe:(NSString *)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

#### 19. 设置显示其他人的视频窗口

**定义**

```
- (void)setRemoteVideoRender:(UIView *)render pubId:(NSString *)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView | 对方视频的窗口，本地设置
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

**说明**

该方法用于订阅成功通后，视频即将显示的回调中(onRTCRemoteOpenVideoRender)使用。

#### 20. 设置其他人显示模式

**定义**

```
- (void)updateRTCVideoRenderModel:(ARVideoRenderMode)videoRenderMode;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
videoRenderMode | ARVideoRenderMode | 显示模式，默认ARVideoRenderScaleAspectFill，等比例填充视图模式

#### 20. 关闭离开

**定义**

```
- (void)close;
```

#### 21. 设置音频检测

**定义**

```
- (void)setAudioActiveCheck:(BOOL)on;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
on | BOOL | 是否开启音频检测，默认打开

#### 22. 获取音频检测是否打开

**定义**

```
- (BOOL)audioActiveCheck;
```

#### 23. 设置视频网络状态是否打开

**定义**

```
- (void)setNetworkStatus:(BOOL)enable;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES打开，NO关闭，默认关闭

#### 24. 获取当前视频网络状态是否打开

**定义**

```
- (BOOL)networkStatusEnabled;
```

### ARRtcpKitDelegate 接口类

#### 1. 发布媒体成功回调

**定义**

```
- (void)onRTCPublishOK:(NSString *)pubId liveInfo:(NSString *)liveInfo;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)
liveInfo | NSString | 直播信息

#### 2. 发布媒体失败回调

**定义**

```
- (void)onRTCPublishFailed:(ARRtcpCode)code reason:(NSString *)reason;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int | 状态码
reason | NSString | 错误原因，RTC错误或者token错误（错误值自己平台定义）

#### 3. 订阅通道成功的回调

**定义**

```
- (void)onRTCSubscribeOK:(NSString *)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

#### 4. 订阅通道失败的回调

**定义**

```
- (void)onRTCSubscribeFailed:(NSString *)pubId code:(ARRtcpCode)code reason:(NSString *)reason;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)
code | int | 状态码
reason | NSString | 错误原因，RTC错误或者token错误（错误值自己平台定义）

#### 5. 订阅后音视频即将显示的回调

**定义**

```
- (void)onRTCOpenRemoteVideoRender:(NSString *)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

#### 6. 订阅的音视频离开的回调

**定义**

```
- (void)onRTCCloseRemoteVideoRender:(NSString *)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

#### 7. 订阅音频后成功的回调

**定义**

```
- (void)onRTCOpenRemoteAudioTrack:(NSString *)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

#### 8. 订阅的音频离开的回调

**定义**

```
- (void)onRTCCloseRemoteAudioTrack:(NSString *)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

#### 9. 本地视频第一帧

**定义**

```
- (void)onRTCFirstLocalVideoFrame:(CGSize)size;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 10. 远程视频第一帧

**定义**

```
- (void)onRTCFirstRemoteVideoFrame:(CGSize)size pubId:(NSString *)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

#### 11. 本地窗口大小的回调

**定义**

```
- (void)onRTCLocalVideoViewChanged:(CGSize)size;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 12. 远程窗口大小的回调

**定义**

```
- (void)onRTCRemoteVideoViewChanged:(CGSize)size pubId:(NSString *)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

#### 13. 其他发布者的音频检测回调

**定义**

```
- (void)onRTCRemoteAudioActive:(NSString *)pubId audioLevel:(int)level showTime:(int)time;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)
level | int | 音频大小（0~100）
time | int | 音频检测在nTime毫秒内不会再回调该方法（单位：毫秒）

**说明**

与会者关闭音频后（setLocalAudioEnable为NO）,该回调将不再回调。对方关闭音频检测后（setAudioActiveCheck为NO）,该回调也将不再回调。

#### 14. 本地音频检测回调

**定义**

```
- (void)onRTCLocalAudioActive:(int)level showTime:(int)time;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
level | int | 音频大小（0~100）
time | int | 音频检测在nTime毫秒内不会再回调该方法（单位：毫秒）

**说明**

本地关闭音频后（setLocalAudioEnable为NO）,该回调将不再回调。本地关闭音频检测后（setAudioActiveCheck为NO）,该回调也将不再回调。

#### 15. 其他发布者的网络质量回调

**定义**

```
- (void)onRTCRemoteNetworkStatus:(NSString *)pubId netSpeed:(int)netSpeed packetLost:(int)packetLost netQuality:(ARNetQuality)netQuality;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
pubId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)
netSpeed | int | 网络上行
packetLost | int | 丢包率
netQuality | ARNetQuality | 网络质量

#### 16. 本地网络质量回调

**定义**

```
- (void)onRTCLocalNetworkStatus:(int)netSpeed packetLost:(int)packetLost netQuality:(ARNetQuality)netQuality;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
netSpeed | int | 网络上行
packetLost | int | 丢包率
netQuality | ARNetQuality | 网络质量

## 四、更新日志

**Version 3.0.0 （2019-01-18）**

* SDK版本升级3.0，API接口变更，更加简洁规范

**Version 2.0.0 （2017-09-30）**

* SDK版本升级2.0，梳理、完善SDK

## 五、错误码对照表

以下为介绍 iOS RTCPEngine SDK 的错误码。

名称 | 值            | 备注
---|------------------------------|----
ARRtcp_OK | 0 | 正常
ARRtcp_UNKNOW | 1 | 未知错误
ARRtcp_EXCEPTION | 2 | SDK调用异常
ARRtcp_EXP_UNINIT | 3 | SDK未初始化
ARRtcp_EXP_PARAMS_INVALIDE | 4 | 参数非法
ARRtcp_EXP_NO_NETWORK | 5 | 没有网络链接
ARRtcp_EXP_NOT_FOUND_CAMERA | 6 | 没有找到摄像头设备
ARRtcp_EXP_NO_CAMERA_PERMISSION | 7 | 没有打开摄像头权限
ARRtcp_EXP_NO_AUDIO_PERMISSION | 8 | 没有音频录音权限
ARRtcp_EXP_NOT_SUPPOAR_WEBARC | 9 | 浏览器不支持原生的webrtc
ARRtcp_NET_ERR | 100 | 网络错误 
ARRtcp_NET_DISSCONNECT | 101 | 网络断开
ARRtcp_LIVE_ERR | 102 | 直播出错
ARRtcp_EXP_ERR | 103 | 异常错误
ARRtcp_EXP_UNAUTHORIZED | 104 | 服务未授权(仅可能出现在私有云项目)
ARRtcp_BAD_REQ | 201 | 服务不支持的错误请求
ARRtcp_AUTH_FAIL | 202  | 认证失败
ARRtcp_NO_USER | 203 | 此开发者信息不存在
ARRtcp_SVR_ERR | 204 | 服务器内部错误
ARRtcp_SQL_ERR | 205 | 服务器内部数据库错误
ARRtcp_ARREARS | 206 | 账号欠费
ARRtcp_LOCKED | 207 | 账号被锁定
ARRtcp_SERVER_NOT_OPEN | 208 | 服务未开通
ARRtcp_ALLOC_NO_RES | 209 | 没有服务器资源
ARRtcp_SERVER_NO_SURPPOAR | 210 | 不支持的服务
ARRtcp_FORCE_EXIT | 211 | 强制离开
ARRtcp_NOT_START | 800 | 会议未开始

  



