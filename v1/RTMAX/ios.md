# iOS

## 一、概述

### 简介

智能调度能够实现公网对讲、视频上报、音视频呼叫，满足公安、消防、政府机构、机场、铁路、港口、物流运输等场景

### Demo 体验

请根据需求选择渠道安装，安装完对讲调度Demo后，可体验多人实时对讲，智能调度等功能。

- [iOS Demo下载](https://www.pgyer.com/SYA3)

- [Android Demo下载](https://www.pgyer.com/GVI3)

- [Web Demo 体验](https://demos.anyrtc.io/ar-talk/)

### 源码 GitHub

源码仅供开发者参考，适用于SDK调试，便于快速集成。

- [iOS Demo 源码下载](https://github.com/anyRTC/AR-Talk-iOS)

- [Android Demo 源码下载](https://github.com/anyRTC/AR-Talk-Android)

- [Web Demo 源码下载](https://github.com/anyRTC/AR-Talk-Web)

## 二、集成指南

### 适用范围

本集成文档适用于iOS RTMaxEngine SDK 3.0.0 以上版本。

### 准备环境

- Xcode 9.0+。
- iOS 8.0+ 真机（iPhone 或 iPad）。
- 请确保你的项目已设置有效的开发者签名。

### 导入SDK

**CocoaPods导入**

* 通过 Cocoapods 下载地址：

```
pod 'RTMaxEngine'
```
* 如果需要安装指定版本则使用以下方式（以 3.0.0 版本为例）：

```
pod 'RTMaxEngine', '3.0.0'
```

**手动导入**

* 前往GitHub[下载Demo](https://github.com/anyRTC/AR-Talk-iOS)，找到RTMaxEngine.framework；

* 在Xcode中选择“Add files to 'Your project name'...”，将RTMaxEngine.framework添加到你的工程目录中

![ios_max_01](/assets/images/ios_max_01.png)
* 打开General->Embedded Binaries中添加RTMaxEngine.framework

![ios_max_02](/assets/images/ios_max_02.png)

### 权限说明

使用RTMaxEngine SDK 前，需要对设备进行授权。打开 info.plist ，点击 + 图标开始添加：

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

![ios_max_03](/assets/images/ios_max_03.png)


## 三、API接口文档

### ARMaxEngine 接口类

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

### ARMaxKit 接口类

#### 1. 实例化ARMaxKit对象

**定义**

```
- (instancetype)initWithDelegate:(id<ARMaxKitDelegate>)delegate;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
delegate | id<ARMaxKitDelegate> | 对讲调度相关回调代理

**返回**

对讲调度对象。

#### 2. 设置token验证

**定义**

```
- (BOOL)setUserToken:(NSString*)userToken;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userToken | NSString | token字符串:客户端向自己服务器申请

**返回值**

设置成功与否。

#### 3. 加入对讲组

**定义**

```
- (int)joinTalkGroup:(NSString*)groupId userId:(NSString*)userId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
groupId | NSString | 对讲组id（同一个anyrtc平台的appid内保持唯一性）
userId | NSString | 用户的第三方平台的用户Id
userData | NSString | 用户的自定义数据

**返回值**

-1：groupId为空；0：成功;2：userData 大于512


#### 4. 切换对讲组

**定义**

```
- (int)switchTalkGroup:(NSString*)groupId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
groupId | NSString | 对讲组id（同一个anyrtc平台的appid内保持唯一性）
userId | NSString | 用户的第三方平台的用户Id
userData | NSString | 用户的自定义数据

**返回值**

-2：切换对讲组失败，-1：groupId为空，0：成功；2：userData 大于512字节


#### 5. 离开对讲组

**定义**

```
- (void)leaveTalkGroup;
```

**说明**

该方法为释放对讲调度资源

#### 6. 切换对讲组

**定义**

```
- (int)applyTalk:(int)priority;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
priority | int | 申请抢麦用户的级别（0权限最大（数值越大，权限越小）；除0意外，可以后台设置0-10之间的抢麦权限大小）

**返回值**

0: 调用OK  -1:未登录  -2:正在对讲中  -3: 资源还在释放中 -4: 操作太过频繁

#### 7. 取消抢麦/下麦

**定义**

```
- (void)cancleTalk;
```

#### 8. 设置本地视频采集窗口

**定义**

```
- (void)setLocalVideoCapturer:(UIView*)render option:(ARMaxOption*)option;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView | 视频显示对象
option | ARMaxOption | 视频配置

**说明**

该方法用于本地视频采集

#### 9. 切换前后摄像头

**定义**

```
- (void)switchCamera;
```

**说明**

切换本地前后摄像头

#### 10. 设置音量检测

**定义**

```
- (void)setAudioActiveCheck:(BOOL)on;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
on | BOOL | YES为打开音量检测；NO为关闭音量检测

**说明**

默认打开

#### 11. 获取音频检测是否打开

**定义**

```
- (BOOL)audioActiveCheck;
```

#### 12. 设置本地音频是否传输

**定义**

```
- (void)setLocalAudioEnable:(BOOL)enable;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES为传输音频,NO为不传输音频

**说明**

默认传输

#### 13. 设置本地视频是否传输

**定义**

```
- (void)setLocalVideoEnable:(BOOL)enable;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES为传输视频，NO为不传输视频

**说明**

默认传输


#### 14. 设置录像

**定义**

```
- (BOOL)setRecordPath:(NSString*)filePath talkPath:(NSString*)talkPath talkP2PPath:(NSString*)talkP2PPath;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
filePath | NSString | 呼叫通话录音的保存路径（文件夹路径）
talkPath | NSString | 对讲录音的保存路径（文件夹路径）
talkP2PPath | NSString | 强插P2P录像的保存路径（文件夹路径）

**返回值**

0:设置失败；1：设置成功


#### 15. 设置远端用户是否可以接收自己的音视频

**定义**

```
- (void)setRemoteAVEnable:(NSString*)userId audio:(BOOL)enable video:(BOOL)enable;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 远端用户userId
enable | BOOL | YES：接收音频，NO：不接收音频
enable | BOOL | YES：接收视频，NO：不接收视频

#### 16. 设置不接收指定通道的音视频

**定义**

```
- (void)setRemotePeerAVEnable:(NSString*)peerId audio:(BOOL)enable video:(BOOL)enable;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | 视频通道的peerId
enable | BOOL | YES：接收音频，NO：不接收音频
enable | BOOL | YES：接收视频，NO：不接收视频

#### 17. 打开或者关闭网络状态回调

**定义**

```
- (void)setNetworkStatus:(BOOL)enable;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES:打开网络状态回调;NO:关闭网络状态回调

**说明**

默认关闭


#### 18. 获取网络状态是否打开

**定义**

```
- (BOOL)networkStatusEnabled;
```

**返回值**

YES:网络状态打开;NO:网络状态关闭

#### 19. 关闭P2P通话

**定义**

```
- (void)closeP2PTalk;
```

**说明**

在控制台强插对讲后，关闭和控制台之间的P2P通话

#### 20. 发起呼叫

**定义**

```
- (int)makeCall:(NSString*)userId type:(int)type userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 被呼叫用户Id
type | int | 呼叫类型：0:视频;1:音频
userData | NSString | 用户的自定义数据

**返回值**

0:调用OK;-1:未登录;-2:没有通话-3:视频资源占用中;-5:本操作不支持自己对自己;-6:会话未创建（没有被呼叫用户）


#### 21. 邀请用户

**定义**

```
- (int)inviteCall:(NSString*)userId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 被邀请用户的Id
userData | NSString | 用户的自定义数据

**返回值**

0:调用OK;-1:未登录;-2:没有通话-3:视频资源占用中;-5:本操作不支持自己对自己;-6:会话未创建（没有被呼叫用户）

#### 22. 主叫端结束某一路正在进行的通话

**定义**

```
- (void)endCall:(NSString*)userId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 结束的这路视频的用户ID

#### 23. 同意呼叫

**定义**

```
- (int)acceptCall:(NSString*)callId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
callId | NSString | 呼叫请求时收到的Id

**说明**

0:调用OK;-1:未登录;-2:会话不存在-3:视频资源占用中;


#### 24. 拒绝通话

**定义**

```
- (void)rejectCall:(NSString*)callId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
callId | NSString | 呼叫的Id

#### 25. 被叫端离开通话

**定义**

```
- (void)leaveCall;
```

**说明**

调用该方法后，把本地视频窗口从父视图上删除


#### 26. 设置显示其他端视频窗口

**定义**

```
- (void)setRemoteVideoRender:(UIView*)render pubId:(NSString*)pubId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView |  显示对方视频的窗口
pubId | NSString |  RTC服务生成流的ID (用于标识与会者发布的流)

**说明**

该方法用于与会者接通后，与会者视频接通回调中（onRTCRemoteOpenVideoRender）使用。


#### 27. 发起视频监看（或者收到视频上报请求时查看视频）

**定义**

```
- (int)monitorVideo:(NSString*)userId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString |  被监看用户Id
userData | NSString | 用户的自定义数据

**返回值**

0:调用OK;-1:未登录;-5:本操作不支持自己对自己

#### 28. 同意别人视频监看

**定义**

```
- (int)acceptVideoMonitor:(NSString*)userId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString |  被监看用户Id

**说明**

该方法为被监看方调用，在接收到回掉（onRTCVideoMonitorRequest）后调用

**返回值**

0:调用OK;-1:未登录;-3:视频资源占用中;-5:本操作不支持自己对自己


#### 28. 拒绝监看

**定义**

```
-(void)rejectVideoMonitor:(NSString*)userId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString |  发起人的用户Id

#### 29. 监看发起者关闭视频监看（谁监看谁关闭）

**定义**

```
- (void)closeVideoMonitor:(NSString*)userId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString |  被监看的用户Id

**说明**

关闭成功会有回掉（onRTCRemoteCloseVideoRender），把显示的视图从父视图上删除掉


#### 30. 视频上报（接收端收到上报请求时，调用monitorVideo进行视频查看）

**定义**

```
- (int)reportVideo:(NSString*)userId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString |  上报对象的用户的Id

**返回值**

0:调用OK;-1:未登录;-3:视频资源占用中;-5:本操作不支持自己对自己


#### 31. 上报者关闭上报

**定义**

```
- (void)closeReportVideo;
```

**说明**

上报功能只能自己关闭


#### 32. 发送消息

**定义**

```
- (BOOL)sendUserMessage:(NSString*)userName userHeader:(NSString*)userHeaderUrl content:(NSString*)content;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userName | NSString | 用户昵称(最大256字节)，不能为空，否则发送失败
userHeaderUrl | NSString | 用户头像(最大512字节)，可选
content | NSString | 消息内容(最大1024字节)不能为空，否则发送失败

**返回值**

YES/NO 发送成功/发送失败

**说明**

如果加入RTC（joinRTC）没有设置userid，发送失败


### ARMaxKitDelegate 回调类

#### 1. 加入对讲组成功/切换对讲组成功回调

**定义**

```
- (void)onRTCJoinTalkGroupOK:(NSString *)groupId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
groupId | NSString | 组ID

#### 2. 加入对讲组失败/切换对讲组失败回调

**定义**

```
- (void)onRTCJoinTalkGroupFailed:(NSString*)groupId code:(ARMaxCode)code reason:(NSString*)reason;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
groupId | NSString | 组ID
code | ARMaxCode | 错误码
reason | NSString | 错误原因,rtc错误，或者token错误（错误值自己平台定义）

#### 3. 离开对讲组回调

**定义**

```
- (void)onRTCLeaveTalkGroup:(ARMaxCode)code;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
code | ARMaxCode | 错误码；0是正常退出；100：网络错误；207：强制退出。其他值参考错误码

#### 4. 申请对讲成功回调

**定义**

```
- (void)onRTCApplyTalkOk;
```

#### 5. 申请对讲成功之后，语音通道建立，可以开始讲话回调

**定义**

```
- (void)onRTCTalkCouldSpeak;
```

#### 6. 其他人正在对讲组中讲话的回调

**定义**

```
- (void)onRTCTalkOn:(NSString*)userId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id
userData | NSString | 用户自定义数据

#### 7. 对讲结束回调

**定义**

```
- (void)onRTCTalkClosed:(ARMaxCode)code userId:(NSString*)userId userData:(NSString*)userData;
```

**参数**
参数名 | 类型 | 描述
---|:---:|---
code | ARMaxCode | 错误码 0：正常结束对讲；其他参考错误码
userId | NSString | 用户Id
userData | NSString | 用户自定义数据

#### 8. 当前对讲组在线人数回调

**定义**

```
- (void)onRTCMemberNum:(int)num;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
num | int | 当前在线人员数量

#### 9. 当前对讲组在线人数回调

**定义**

```
- (void)onRTCTalkP2POn:(NSString*)userId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id
userData | NSString | 用户自定义数据

#### 10. 与控制台的P2P讲话结束回调

**定义**

```
- (void)onRTCTalkP2POff:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userData | NSString | 用户自定义数据

#### 11. 收到监看请求回调

**定义**

```
- (void)onRTCVideoMonitorRequest:(NSString*)userId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id
userData | NSString | 用户自定义数据

#### 12. 视频监看结束回调

**定义**

```
- (void)onRTCVideoMonitorClose:(NSString*)userId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id
userData | NSString | 用户自定义数据


#### 13. 视频监看请求结果回调

**定义**

```
- (void)onRTCVideoMonitorResult:(NSString*)userId code:(ARMaxCode)code userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id
code | ARMaxCode | 错误码
userData | NSString | 用户自定义数据


#### 14. 收到视频上报请求回调

**定义**

```
- (void)onRTCVideoReportRequest:(NSString*)userId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id
userData | NSString | 用户自定义数据

#### 15. 收到视频上报结束回调

**定义**

```
- (void)onRTCVideoReportClose:(NSString*)userId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id

#### 16. 主叫方发起通话成功回调

**定义**

```
- (void)onRTCMakeCallOK:(NSString*)callId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
callId | NSString | 呼叫Id

#### 17. 主叫方收到被叫方同意通话回调

**定义**

```
- (void)onRTCAcceptCall:(NSString*)userId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id
userData | NSString | 用户自定义数据

#### 18. 主叫方收到被叫方通话拒绝的回调

**定义**

```
- (void)onRTCRejectCall:(NSString*)userId code:(ARMaxCode)code userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id
code | ARMaxCode | 错误码
userData | NSString | 用户自定义数据


#### 19. 被叫方调用leaveCall方法时，主叫方收到的回调

**定义**

```
- (void)onRTCLeaveCall:(NSString*)userId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id

#### 20. 主叫方收到通话结束（被叫方和邀请发已全部退出或者主叫方挂断所有参与者）回调

**定义**

```
- (void)onRTCReleaseCall:(NSString*)callId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
callId | NSString | 呼叫Id

#### 21. 被叫方收到通话请求回调

**定义**

```
- (void)onRTCMakeCall:(NSString*)callId callType:(int)callType userId:(NSString*)userId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
callId | NSString | 呼叫Id
callType | int | 呼叫类型:0:视频;1:音频
userId | NSString | 用户Id
userData | NSString | 用户自定义数据

#### 22. 被叫方收到主叫方挂断通话(收到该回掉，把本地视图从父视图删除)回调

**定义**

```
- (void)onRTCEndCall:(NSString*)callId userId:(NSString*)userId code:(ARMaxCode)code;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
callId | NSString | 呼叫Id
userId | NSString | 用户Id
code | ARMaxCode | 错误码

#### 23. 其他与会者加入（音视频）回调

**定义**

```
-(void)onRTCRemoteOpenVideoRender:(NSString*)peerId pubId:(NSString *)pubId userId:(NSString*)userId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)；
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)；
userId | NSString | 用户Id
userData | NSString | 用户自定义数据

#### 24. 其他与会者离开（音视频）回调

**定义**

```
-(void)onRTCRemoteCloseVideoRender:(NSString*)peerId pubId:(NSString *)pubId userId:(NSString*)userId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)；
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)；
userId | NSString | 用户Id


#### 25. 其他与会者离开（音视频）回调

**定义**

```
- (void)onRTCRemoteOpenAudioTrack:(NSString*)peerId userId:(NSString *)userId userData:(NSString*)userData;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)；
userId | NSString | 用户Id
userData | NSString | 用户自定义数据

#### 26. 其他与会者离开（音频）回调

**定义**

```
- (void)onRTCRemoteCloseAudioTrack:(NSString*)peerId userId:(NSString *)userId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)；
userId | NSString | 用户Id

#### 27. 本地视频第一帧

**定义**

```
-(void)onRTCFirstLocalVideoFrame:(CGSize)size;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 28. 远程视频第一帧

**定义**

```
- (void)onRTCFirstRemoteVideoFrame:(CGSize)size rtcpId:(NSString *)rtcpId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小
rtcpId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)

#### 29. 本地窗口大小的回调

**定义**

```
- (void)onRTCLocalVideoViewChanged:(CGSize)size;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 30. 远程窗口大小的回调

**定义**

```
- (void)onRTCRemoteVideoViewChanged:(CGSize)size rtcpId:(NSString *)rtcpId;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小
rtcpId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)


#### 31. 其他与会者对音视频的操作回调

**定义**

```
- (void)onRTCRemoteAVStatus:(NSString *)peerId audio:(BOOL)audio video:(BOOL)video;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
audio | BOOL | YES为打开音频，NO为关闭音频
video | BOOL | YES为打开视频，NO为关闭视频

#### 32. 别人对自己音视频的操作回调

**定义**

```
- (void)onRTCLocalAVStatus:(BOOL)audio video:(BOOL)video;
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
audio | BOOL | YES为打开音频，NO为关闭音频
video | BOOL | YES为打开视频，NO为关闭视频


#### 33. 其他与会者的声音大小回调

**定义**

```
- (void)onRTCRemoteAudioActive:(NSString *)rtcpId audioLevel:(int)level showTime:(int)time;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)
level | int | 音频大小（0~100）
time | int | 音频检测在nTime毫秒内不会再回调该方法（单位：毫秒）

**说明**

与会者关闭音频后（setLocalAudioEnable为NO）,该回调将不再回调。对方关闭音频检测后（setAudioActiveCheck为NO）,该回调也将不再回调。

#### 34. 本地声音大小回调

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

#### 35. 其他与会者网络质量回调

**定义**

```
- (void)onRTCRemoteNetworkStatus:(NSString *)peerId netSpeed:(int)netSpeed packetLost:(int)packetLost netQuality:(ARNetQuality)netQuality;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)
netSpeed | int | 网络上行
packetLost | int | 丢包率
netQuality | ARNetQuality | 网络质量

#### 36. 本地网络质量回调

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

#### 37. 录像地址回调信息回调

**定义**

```
- (void)onRTCGotRecordFile:(int)recordType userData:(NSString*)userData filePath:(NSString*)filePath;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
recordType | int | 录音的类型（0/1/2/3：对讲本地录音/对讲远端录音/强插P2P录音/语音通话呼叫录音）
userData | NSString | 用户自定义数据
filePath | NSString | 录音文件的路径

#### 38. 收到消息回调

**定义**

```
- (void)onRTCUserMessage:(NSString*)userId userName:(NSString*)userName userHeader:(NSString*)userHeaderUrl content:(NSString*)content;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 发送消息者在自己平台下的Id；
userName | NSString | 发送消息者的昵称
userHeaderUrl | NSString | 发送者的头像
content | NSString | 消息内容


## 四、更新日志

**Version 3.0.0 （2019-05-15）**

* SDK版本升级3.0，API接口变更，更加简洁规范
* 更改SDK配置项，配置更简单
* 断网重连优化
* 分辨率选项增加，帧率可配置
* 添加用户服务级token安全认证，服务更安全

**Version 2.0.0 （2017-09-30）**

* SDK版本升级2.0，梳理、完善SDK

## 五、错误码对照表

以下为介绍 iOS ARMaxEngine SDK 的错误码。

名称 | 值            | 备注
---|------------------------------|----
ARMax_OK | 0 | 正常
ARMax_UNKNOW | 1 | 未知错误
ARMax_EXCEPTION | 2 | SDK调用异常
ARMax_EXP_UNINIT | 3 | SDK未初始化
ARMax_EXP_PARAMS_INVALIDE | 4 | 参数非法
ARMax_EXP_NO_NETWORK | 5 | 没有网络链接
ARMax_EXP_NOT_FOUND_CAMERA | 6 | 没有找到摄像头设备
ARMax_EXP_NO_CAMERA_PERMISSION | 7 | 没有打开摄像头权限
ARMax_EXP_NO_AUDIO_PERMISSION | 8 | 没有音频录音权限
ARMax_EXP_NOT_SUPPORT_WEBRTC | 9 | 浏览器不支持原生的webrtc
ARMax_NET_ERR | 100 | 网络错误 
ARMax_NET_DISSCONNECT | 101 | 网络断开
ARMax_LIVE_ERR | 102 | 直播出错
ARMax_EXP_ERR | 103 | 异常错误
ARMax_EXP_Unauthorized | 104 | 服务未授权(仅可能出现在私有云项目)
ARMax_BAD_REQ | 201 | 服务不支持的错误请求
ARMax_AUTH_FAIL | 202  | 认证失败
ARMax_NO_USER | 203 | 此开发者信息不存在
ARMax_SVR_ERR | 204 | 服务器内部错误
ARMax_SQL_ERR | 205 | 服务器内部数据库错误
ARMax_ARREARS | 206 | 账号欠费
ARMax_LOCKED | 207 | 账号被锁定
ARMax_SERVER_NOT_OPEN | 208 | 服务未开通
ARMax_ALLOC_NO_RES | 209 | 没有服务器资源
ARMax_SERVER_NOT_SURPPORT | 210 | 不支持的服务
ARMax_FORCE_EXIT | 211 | 强制离开
ARMax_APPLY_SVR_ERR | 800 | 申请麦但是服务器异常 (没有MCU服务器,暂停申请)
ARMax_APPLY_BUSY | 801 | 当前你正在忙
ARMax_APPLY_NO_PRIO | 802 | 当前麦被占用 (有人正在说话切你的权限不够)
ARMax_APPLY_INITING | 803 | 正在初始化中 (自身的通道没有发布成功,不能申请)
ARMax_APPLY_ING | 804 | 等待上麦
ARMax_ROBBED | 810 | 麦被抢掉了
ARMax_BREAKED | 811 | 麦被释放了
ARMax_RELEASED_BY_P2P | 812 | 麦被释放了，因为要对讲
ARMax_P2P_OFFLINE | 820 | 强插时，对方可能不在线了或异常离线
ARMax_P2P_BUSY | 821 | 强插时，对方正忙
ARMax_P2P_NOT_TALK | 822 |  强插时，对方不在麦上
ARMax_V_MON_OFFLINE | 830 | 视频监看时，对方不在线，或下线了
ARMax_V_MON_GRABED | 831 | 视频监看被抢占了
ARMax_CALL_OFFLINE | 840 | 对方不在线或掉线了
ARMax_CALL_NO_PRIO | 841 | 发起呼叫时自己有其他业务再进行(资源被占用)
ARMax_CALL_NOT_FOUND | 842 | 会话不存在




