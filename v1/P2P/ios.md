# iOS

## 一、概述

### 简介

AnyRTC提供对语音通话场景的支持，RTCallEngine SDK，支持视频、语音、优先视频等多种呼叫模式，适用于网络电话、活动、教育等多种呼叫场景。

### Demo体验

请根据需求选择渠道安装，安装完P2P Demo后，可体验点对点呼叫功能。

- [iOS Demo下载](https://www.pgyer.com/anyrtc_p2p_ios)

- [Android Demo下载](https://www.pgyer.com/3blO)

### 源码GitHub

源码仅供开发者参考，适用于SDK调试，便于快速集成。

- [iOS Demo 源码下载](https://github.com/anyRTC/anyRTC-P2P-iOS)

- [Android Demo 源码下载](https://github.com/anyRTC/anyRTC-P2P-Android)


## 二、集成指南


### 适用范围

本集成文档适用于iOS RTCallEngine SDK 3.0.0版本。

### 准备环境

- Xcode 9.0+。
- iOS 8.0+ 真机（iPhone 或 iPad）。
- 请确保你的项目已设置有效的开发者签名。

### 导入SDK

**CocoaPods导入**

* 通过 Cocoapods 下载地址：

```
pod 'RTCallEngine'
```
* 如果需要安装指定版本则使用以下方式（以 3.0.0 版本为例）：

```
pod 'RTCallEngine', '3.0.0'
```

**手动导入**

* 前往GitHub[下载Demo](https://github.com/anyRTC/anyRTC-RTCP-iOS)，找到RTCallEngine.framework；

* 在Xcode中选择“Add files to 'Your project name'...”，将RTCallEngine.framework添加到你的工程目录中
![ios_p2p_01](/assets/images/ios/ios_p2p_01.png)

* 打开General->Embedded Binaries中添加RTCallEngine.framework

![ios_p2p_02](/assets/images/ios/ios_p2p_02.png)

### 权限说明

使用RTCallEngine SDK 前，需要对设备进行授权。打开 info.plist ，点击 + 图标开始添加：

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

![ios_p2p_03](/assets/images/ios/ios_p2p_03.png)

## 三、API接口文档

### ARCallEngine 接口类

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

RTCallEngine SDK 版本号。

#### 4. 设置打印日志级别

**定义**

```
+ (void)setLogLevel:(ARLogModel)levelModel;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
levelModel | ARLogModel | 日志级别

### ARCallKit 接口类

#### 1. 实例化Call对象

**定义**

```
- (instancetype)initWithDelegate:(id <ARCallKitDelegate>)delegate;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
delegate | id <ARCallKitDelegate> | RTC相关回调代理

#### 2. 设置客服

**定义**

```
- (void)setAsClerk:(NSString*)queueId option:(ARClertOption*)option;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
queueId | NSString | 队列Id
option | ARClertOption | 客服配置

**说明**

需放在上线（turnOn）之前使用。

#### 3. 上线

**定义**

```
- (void)turnOnByToken:(NSString* _Nullable)token userId:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
token | NSString | 令牌:客户端向自己服务申请获得，参考企业级安全指南
userId | NSString | 用户Id，确保平台唯一，不能为空

#### 4. 下线

**定义**

```
- (void)turnOff;
```

#### 5. 挂起

**定义**

```
- (void)setAvalible:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | 挂起与否

**说明**

挂起后将不再接收呼叫。

#### 6. 设置本地显示窗口

**定义**

```
- (void)setLocalVideoCapturer:(UIView *)render option:(ARCallOption *)option;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView | 本地视频窗口
option | ARCallOption | 配置项

**说明**

必须放到 makeCall方法之后调用。

#### 7. 设置滤镜

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

#### 8. 设置其他视频显示窗口

**定义**

```
- (void)setRemoteVideoRender:(UIView *)render renderId:(NSString *)renderId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
render | NSString | 视频显示窗口
renderId | NSString | 渲染Id

#### 9.设置某个人的显示模式

**定义**

```
- (void)updateRTCVideoRenderModel:(ARVideoRenderMode)renderMode renderId:(NSString *)renderId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
renderMode | ARVideoRenderMode | 显示模式，默认ARVideoRenderScaleToFill，等比例填充视图模式
renderId | NSString | 渲染Id

#### 10. 呼叫用户

**定义**

```
- (int)makeCallUser:(NSString *)userId option:(ARUserOption *)option;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫的用户Id
option | ARUserOption | 配置项

**返回值**

操作是否成功。0：成功;-1:操作频繁;-2：不能呼叫自己;-3：呼叫列表不能为空;-4：呼叫session已存在;-5：呼叫ID不能为空:-6：userData:超过1024个字节:-7:   ARCallKit还未初始化。

#### 11. 呼叫群组

**定义**

```
- (int)makeCallGroup:(NSString *)groupId option:(ARGroupOption *)option;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
groupId | NSString | 群组Id
option | ARGroupOption | 配置项

**返回值**

操作是否成功。0：成功;-1:操作频繁;-2：不能呼叫自己;-3：呼叫列表不能为空;-4：呼叫session已存在;-5：呼叫ID不能为空:-6：userData:超过1024个字节:-7:ARCallKit还未初始化

#### 12. 呼叫客服

**定义**

```
- (int)makeCallQueue:(NSString *)queueId option:(ARQueueOption *)option;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
queueId | NSString | 队列
option | NSString | 呼叫配置

**返回值**

操作是否成功。0：成功;-1:操作频繁;-2：不能呼叫自己;-3：呼叫列表不能为空;-4：呼叫session已存在;-5：呼叫ID不能为空:-6：userData:超过1024个字节:-7:ARCallKit还未初始化。

#### 13. 邀请用户

**定义**

```
- (int)inviteCall:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id

**返回值**

操作是否成功 0：成功; -1:userId为空;-2：不是会议类型不能调用;-3：ARCallKit还未初始化。

**说明**

只有建立会议呼叫之后才能调用。

#### 14. 挂断通话

**定义**

```
- (void)endCall:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫的用户Id

#### 15. 同意通话

**定义**

```
- (void)acceptCall:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫的用户Id

#### 16. 拒绝通话

**定义**

```
- (void)rejectCall:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫的用户Id

#### 17. 转呼手机

**定义**

```
- (void)switchToPstn;
```

**说明**

呼叫用户的时候才能用；如果呼叫模式为视频会自动转为音频模式。

#### 18. 转呼座机

**定义**

```
- (void)switchToExtension;
```

**说明**

呼叫用户的时候才能用，如果呼叫模式为视频会自动转为音频模式。

#### 19. 切换音频模式

**定义**

```
- (BOOL)swithToAudioMode;
```

**说明**

操作是否成功。成功之后，把本地视频视图清除掉。

#### 20. 发送消息

**定义**

```
- (void)sendUserMessage:(NSString *)content;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
content | NSString | 发送消息的内容(限制1024字节)

#### 21. 设置本地音频是否传输

**定义**

```
- (void)setLocalAudioEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES为传输音频，NO为不传输音频，默认音频传输

#### 22. 设置本地视频是否传输

**定义**

```
- (void)setLocalVideoEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES为传输视频，NO为不传输视频，默认视频传输

#### 23. 获取本地音频传输是否打开

**定义**

```
- (BOOL)localAudioEnabled;
```

#### 24. 获取本地视频传输是否打开

**定义**

```
- (BOOL)localAudioEnabled;
```

#### 25. 切换前后摄像头

**定义**

```
- (void)switchCamera;
```

#### 26. 设置扬声器开关

**定义**

```
- (void)setSpeakerOn:(BOOL)on;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
on | BOOL | YES打开扬声器，NO关闭扬声器，默认扬声器打开

#### 27. 获取人员列表

**定义**

```
- (NSArray<ARUserItem *> *)getUserList;
```

#### 28. 抓图

**定义**

```
- (UIImage*)snapPicture:(NSString*)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | userId 用户Id

#### 29. 设置会议模式

**定义**

```
- (BOOL)setCallZoomMode:(ARCallZoomType)type;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
type | ARCallZoomType | 模式类型

#### 30. 设置显示页码

**定义**

```
- (BOOL)setCallZoomPage:(int)page;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
page | int | 页码（从0开始，每页加上自己的视频流为4路）

#### 31. 清空会话

**定义**

```
- (void)close;
```
**说明**

当别人挂断或者自己挂断离开的时候，调用该方法用于清空本地视频。

### ARCallKitDelegate 接口类

#### 1. 服务器链接成功

**定义**

```
- (void)onRTCConnected;
```

#### 2. 服务器断开

**定义**

```
- (void)onRTCDisconnect:(ARCallCode)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | ARCallCode | 状态码

#### 3. 收到呼叫的回调

**定义**

```
- (void)onRTCMakeCall:(NSString *)meetId userId:(NSString *)userId userData:(NSString *)userData callModel:(ARCallMode)callMode extend:(NSString*)extend;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
meetId | NSString |  会议Id,当AR_Call_Meet_Invite类型的时候，该参数为空
userId | NSString | 呼叫方的Id
userData | NSString | 呼叫方的自定义信息
callMode | ARCallMode | 呼叫类型
extend | NSString | 扩展信息，用户自定义,呼叫群组的时候带的

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
- (void)onRTCRejectCall:(NSString *)userId code:(ARCallCode)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 被呼叫方的Id
code | ARCallCode | 状态码

#### 6. 收到对方挂断的回调

**定义**

```
- (void)onRTCEndCall:(NSString *)userId code:(ARCallCode)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户id
code | ARCallCode | 状态码

#### 7. 收到对方视频视频即将显示的回调

**定义**

```
- (void)onRTCOpenRemoteVideoRender:(NSString *)userId renderId:(NSString*)renderId userData:(NSString*)userData;

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫的用户Id
renderId | NSString | 渲染Id
userData | NSString | 用户信息

**说明**

收到该回调，调用setRemoteVideoRender显示对方视频窗口。

#### 8. 收到对方视频离开的回调

**定义**

```
- (void)onRTCCloseRemoteVideoRender:(NSString *)userId  renderId:(NSString*)renderId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 呼叫的用户Id
renderId | NSString | 渲染Id

#### 9. 收到对方音频即将播放的回调

**定义**

```
- (void)onRTCOpenRemoteAudioTrack:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id
userData | NSString | 用户信息

#### 10. 收到对方音频离开的回调

**定义**

```
- (void)onRTCCloseRemoteAudioTrack:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id

#### 11. 是否支持SIP呼叫

**定义**

```
- (void)onRTCSipSupport:(BOOL)pstn extension:(BOOL)extension null:(BOOL)null;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
pstn | BOOL | YES:可转接手机，用户调用switchToPstn即可去呼叫手机
extensionextension | BOOL | YES:转接分机，用户调用switchToExtension即可去呼叫分机
null | BOOL | 暂时可不用

#### 12. 切换到音频模式

**定义**

```
- (void)onRTCSwithToAudioMode;
```
**说明**

收到该回调，做视频视图的清理工作。

#### 13. 收到消息回调

**定义**

```
- (void)onRTCUserMessage:(NSString *)userId message:(NSString *)content;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 发送消息的用户Id
content | NSString | 消息内容

#### 14. 其他与会者对音视频的操作

**定义**

```
- (void)onRTCRemoteAVStatus:(NSString *)userId audio:(BOOL)audio video:(BOOL)video;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id
audio | BOOL | YES为打开音频，NO为关闭音频
video | BOOL | YES为打开视频，NO为关闭视频

#### 15. 别人对自己音视频的操作

**定义**

```
- (void)onRTCLocalAVStatus:(BOOL)audio video:(BOOL)video;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
audio | BOOL | YES为打开音频，NO为关闭音频
video | BOOL | YES为打开视频，NO为关闭视频

#### 16. 本地视频第一帧

**定义**

```
- (void)onRTCFirstLocalVideoFrame:(CGSize)size;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 17. 远程视频第一帧

**定义**

```
- (void)onRTCFirstRemoteVideoFrame:(CGSize)size renderId:(NSString *)renderId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小
renderId | NSString | 渲染Id (用于标识与会者发布的流)

#### 18. 本地窗口大小的回调

**定义**

```
- (void)onRTCLocalVideoViewChanged:(CGSize)size;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 19. 远程窗口大小的回调

**定义**

```
- (void)onRTCRemoteVideoViewChanged:(CGSize)size renderId:(NSString *)renderId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小
renderId | NSString | 渲染Id (用于标识与会者发布的流)

#### 20. 会议模式页码变化

**定义**

```
- (void)onRTCMeetPageInfo:(ARCallZoomType)type allPage:(int)allPage currentPage:(int)currentPage allRenderNum:(int)allRenderNum beginIndex:(int)index
showNum:(int)showNum;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
type | ARCallZoomType | 当前模式
allPage | int | 总页码（一页显示4个）
currentPage | int | 当前页
allRenderNum | int | 当前服务上有多少个渲染，根据此数量来判断页码添加删除
index | int | 开始位置
showNum | int | 显示多少个

#### 21. 呼叫队列用户的排队信息

**定义**

```
- (void)onRTCUserCTIStatus:(int)queueNum;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
queueNum | int | 前面排队人员

#### 22. 服务队列的相关队列信息

**定义**

```
- (void) onRTCClerkCTIStatus:(int)queueNum clerkNum:(int)allClerk woringClerkNum:(int)woringNum;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
queueNum | int | 队列数
allClerk | int | 总客服数
woringNum | int | 正在忙的客服

## 四、更新日志

**Version 3.0.0 （2019-05-15）**

* SDK版本升级3.0，API接口变更

**Version 2.0.0 （2017-09-30）**

* SDK版本升级2.0，梳理、完善SDK

## 五、错误码对照表

以下为介绍 iOS RTCallEngine SDK 的错误码。

名称 | 值            | 备注
---|------------------------------|----
ARCall_OK | 0 | 正常
ARCall_UNKNOW | 1 | 未知错误
ARCall_EXCEPTION | 2 | SDK调用异常
ARCall_EXP_UNINIT | 3 | SDK未初始化
ARCall_EXP_PARAMS_INVALIDE | 4 | 参数非法
ARCall_EXP_NO_NETWORK | 5 | 没有网络链接
ARCall_EXP_NOT_FOUND_CAMERA | 6 | 没有找到摄像头设备
ARCall_EXP_NO_CAMERA_PERMISSION | 7 | 没有打开摄像头权限
ARCall_EXP_NO_AUDIO_PERMISSION | 8 | 没有音频录音权限
ARCall_EXP_NOT_SUPPORT_WEBRTC | 9 | 浏览器不支持原生的webrtc
ARCall_NET_ERR | 100 | 网络错误
ARCall_NET_DISSCONNECT | 101 | 网络断开
ARCall_LIVE_ERR | 102 | 直播出错
ARCall_EXP_ERR | 103 | 异常错误
ARCall_EXP_Unauthorized | 104 | 服务未授权(仅可能出现在私有云项目)
ARCall_BAD_REQ | 201 | 服务不支持的错误请求
ARCall_AUTH_FAIL | 202 | 认证失败
ARCall_NO_USER | 203 | 此开发者信息不存在
ARCall_SVR_ERR | 204 | 服务器内部错误
ARCall_SQL_ERR | 205 | 服务器内部数据库错误
ARCall_ARREARS | 206 | 账号欠费
ARCall_LOCKED | 207 | 账号被锁定
ARCall_SERVER_NOT_OPEN | 208 | 服务未开通
ARCall_ALLOC_NO_RES | 209 | 没有服务器资源
ARCall_SERVER_NOT_SURPPORT | 210 | 不支持的服务
ARCall_FORCE_EXIT | 211 | 强制离开
ARCall_AUTH_TIMEOUT | 212 | 验证超时
ARCall_NEED_VERTIFY_TOKEN | 213 | 需要验证userToken
ARCall_WEB_DOMIAN_ERROR | 214 | Web应用的域名验证失败
ARCall_IOS_BUNDLE_ID_ERROR | 215 | iOS应用的BundleId验证失败
ARCall_ANDROID_PKG_NAME_ERROR | 216 | Android应用的包名验证失败
ARCall_PEER_BUSY | 800 | 对方正忙
ARCall_OFFLINE | 801 | 对方不在线
ARCall_NOT_SELF | 802 | 不能呼叫自己
ARCall_EXP_OFFLINE | 803 | 通话中对方意外掉线
ARCall_EXP_EXIT | 804 | 对方异常导致(如：重复登录帐号将此前的帐号踢出)
ARCall_TIMEOUT | 805 | 呼叫超时(45秒)
ARCall_NOT_SURPPORT | 806 | 不支持

  



