## 一、快速开始

### 集成指南

#### 适用范围

本集成文档适用于iOS RTMeetEngine SDK 2.0.0 ~ 3.0.1版本。

#### 准备环境

- Xcode 9.0+。
- iOS 8.0+ 真机（iPhone 或 iPad）。
- 请确保你的项目已设置有效的开发者签名。

#### 导入SDK

**CocoaPods导入**

* 通过 Cocoapods 下载地址：

```
pod 'RTMeetEngine'
```
* 如果需要安装指定版本则使用以下方式（以 3.0.1 版本为例）：

```
pod 'RTMeetEngine', '3.0.1'
```

**手动导入**

* 前往GitHub[下载Demo](https://github.com/anyRTC/anyRTC-RTCP-iOS)，找到RTMeetEngine.framework；

* 在Xcode中选择“Add files to 'Your project name'...”，将RTMeetEngine.framework添加到你的工程目录中
![ios_meet_01](/assets/images/ios/ios_meet_01.png)

* 打开General->Embedded Binaries中添加RTMeetEngine.framework

![ios_meet_02](/assets/images/ios/ios_meet_02.png)

#### 权限说明

使用RTMeetEngine SDK 前，需要对设备进行授权。打开 info.plist ，点击 + 图标开始添加：

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

#### 后台模式(Background Modes)

勾选Audio, AirPlay and Picture in Picture

![ios_meet_03](/assets/images/ios/ios_meet_03.png)

### 开发指南

#### 1. 初始化SDK

集成SDK后，还需对SDK进行初始化操作，建议在AppDelegate中完成。

##### 1.1 导入头文件

```
#import <RTMeetEngine/ARMeetSDK.h>
```

##### 1.2 配置开发者信息

调用initEngine:token:方法配置开发者信息，开发者信息可在anyRTC管理后台中获得，详见[创建anyRTC账号](https://docs.anyrtc.io)


**示例代码：**

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
// Override point for customization after application launch.
//配置开发者信息
[ARMeetEngine initEngine:appID token:token];
//配置私有云(默认无需配置)
//[ARMeetEngine configServerForPriCloud:@"XXX" port:XXX];
return YES;
}
```

#### 2. 加入房间

##### 2.1 实例化会议对象

调用initWithDelegate:option:方法实例化会议对象，需实现ARMeetKitDelegate中的回调方法。第二个参数option为会议配置项，包括媒体类型、会议类型、帧率、码率、相机类型等。

会议类型：

* 一般模式（ARMeetTypeNomal）：大家进入会议互相观看

* 主持模式（ARMeetTypeHoster）：主持人进入，可以看到所有人，其他人员只看到主持人

* live模式（ARMeetTypeLive）：大班课模式

* zoom模式（ARMeetTypeZoom）

**示例代码：**

```
- (void)initializeMeet {
//配置信息
ARMeetOption *option = [ARMeetOption defaultOption];
ARVideoConfig *config = [[ARVideoConfig alloc] init];
config.cameraOrientation = ARCameraPortrait;
option.videoConfig = config;
//实例化会议对象
self.meetKit = [[ARMeetKit alloc] initWithDelegate:self option:option];
}
```

##### 2.2 设置本地视频采集窗口

调用setLocalVideoCapturer:方法用于设置本地视频采集窗口。

**示例代码：**

```
[self.meetKit setLocalVideoCapturer:self.view];
```

##### 2.3 加入会议

调用joinRTCByToken:meetId:userId:userData:方法用于加入会议，第一个参数token为令牌，可为空，具体用法可参考[安全指南](https://docs.anyrtc.io/v1/security/服务级安全设置指南.html)。

**示例代码：**

```
[self.meetKit joinRTCByToken:nil meetId:self.meetId userId:[NSString stringWithFormat:@"%d",arc4random() % 100000] userData:@""];
```

##### 2.4 设置其他人视频显示窗口

调用setRemoteVideoRender:pubId:方法设置其他人视频显示窗口，用于收到对方视频即将显示（onRTCOpenRemoteVideoRender）的回调时调用。

**示例代码：**

```
//其他与会者加入(音视频)
- (void)onRTCOpenRemoteVideoRender:(NSString *)peerId pubId:(NSString *)pubId userId:(NSString *)userId userData:(NSString *)userData {
UIView *videoView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 90, 160)];
[self.view addSubview:videoView];
[self.meetKit setRemoteVideoRender:videoView pubId:pubId];
}
```

#### 3. 离开房间

调用leaveRTC方法用于离开房间。

**示例代码：**

```
[self.meetKit leaveRTC];
```

## 二、API接口文档

### ARMeetEngine 接口类

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

RTMeeting SDK版本号

#### 4. 设置打印日志级别

**定义**

```
+ (void)setLogLevel:(ARLogModel)levelModel;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
levelModel | ARLogModel | 日志级别

### ARMeetKit 接口类

#### 1. 实例化会议对象

**定义**

```
- (instancetype)initWithDelegate:(id <ARMeetKitDelegate>)delegate option:(ARMeetOption *)option;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
delegate | id <ARMeetKitDelegate> | RTC相关回调代理
option | ARMeetOption | 配置信息

**返回**

会议对象。 

#### 2. 设置本地音频是否传输

**定义**

```
- (void)setLocalAudioEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES传输音频，NO不传输音频，默认传输

#### 3. 设置本地视频是否传输

**定义**

```
- (void)setLocalVideoEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES为传输视频，NO为不传输视频，默认视频传输

#### 4. 获取本地音频传输是否打开

**定义**

```
- (BOOL)localAudioEnabled;
```

**返回**

音频是否传输。 

#### 5. 获取本地视频传输是否打开

**定义**

```
- (BOOL)localVideoEnabled;
```

**返回**

视频是否传输。 

#### 6. 切换前后摄像头

**定义**

```
- (void)switchCamera;
```

#### 7. 设置扬声器开关

**定义**

```
- (void)setSpeakerOn:(BOOL)on;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
on | BOOL | YES打开扬声器，NO关闭扬声器，默认打开

#### 8. 设置音频检测

**定义**

```
- (BOOL)setAudioActiveCheck:(BOOL)on;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
on | BOOL | 是否开启音频检测，默认打开

**返回**

操作是否成功。 

#### 9. 设置本地视频采集窗口

**定义**

```
- (void)setLocalVideoCapturer:(UIView *)render;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView | 视频显示对象

#### 10. 设置本地显示模式

**定义**

```
- (void)updateLocalVideoRenderModel:(ARVideoRenderMode)videoRenderMode;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
videoRenderMode | ARVideoRenderMode | 显示模式，默认ARVideoRenderScaleAspectFill，等比例填充视图模式

#### 11. 重置音频录音和播放

**定义**

```
- (void)doRestartAudioRecord;
```

**说明**

使用AVplayer播放后调用该方法。 

#### 12. 设置本地前置摄像头镜像是否打开

**定义**

```
- (BOOL)setFontCameraMirrorEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES打开，NO关闭

**返回**

镜像成功与否。 

#### 13. 设置滤镜

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

#### 14. 设置远端音视频是否传输

**定义**

```
- (BOOL)setRemoteAVEnable:(NSString *)peerId audio:(BOOL)audio video:(BOOL)video;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id
audio | BOOL | YES传输音频，NO不传输音频
video | BOOL | YES传输视频，NO不传输视频

**返回**

操作是否成功。 

#### 15. 本地是否接收远程的视频

**定义**

```
- (BOOL)muteRemoteVideoStream:(BOOL)mute pubId:(NSString *)pubId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
mute | BOOL | YES不接收，NO接收
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)

**返回**

操作是否成功。 

#### 16. 本地是否接收远程的音频

**定义**

```
- (BOOL)muteRemoteAudioStream:(BOOL)mute pubId:(NSString *)pubId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
mute | BOOL | YES不接收，NO接收
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)

**返回**

操作是否成功。 

#### 17. 截图功能

**定义**

```
- (void)snapPicture:(NSString*)userId complete:(ScreenshotsBlock)block;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 用户Id
block | ScreenshotsBlock | 数据回调

#### 18. 设置使用外置摄像头

**定义**

```
- (void)setExternalCameraCapturer:(BOOL)enable dataType:(ARCaptureType)type;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | 是否使用外置摄像头
type | ARCaptureType | YUV420PType、RGBType

#### 19. 向内部塞流

**定义**

```
- (BOOL)sendExternalCameraCapturerBuffer:(CVPixelBufferRef)bufferRef rotation:(ARVideoRotation)rotation;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
bufferRef | CVPixelBufferRef | 视频流数据
rotation | ARVideoRotation | 视频方向

#### 20. 向内部塞流

**定义**

```
- (BOOL)sendExternalCameraData:(NSData*)data rotation:(ARVideoRotation)rotation pixelsWidth:(int)width
pixelsHeight:(int)height;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
data | NSData | 视频流数据（yuv420格式的，或者ARGB数据）
rotation | ARVideoRotation | 视频方向
width | int | 视频宽
height | int | 视频高

#### 21. 加入会议

**定义**

```
- (BOOL)joinRTCByToken:(NSString* _Nullable)token
  meetId:(NSString *)meetId
  userId:(NSString *)userId
userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
token | NSString | 客户端向自己服务申请获得，参考企业级安全指南
meetId | NSString | 会议号（可以在anyRTC平台获得，也可以根据自己平台，分配唯一的一个Id号）
userId | NSString | 开发者自己平台的用户id
userData | NSString | 开发者自己平台的相关信息（昵称，头像等），还可以加入字段来限制会议人数：MaxJoiner，可选。(限制512字节)

**返回**

加入会议成功或者失败。 

#### 22. 离开会议室

**定义**

```
- (void)leaveRTC;
```

**说明**

相当于析构函数。 

#### 23. 设置其他人视频显示窗口

**定义**

```
- (void)setRemoteVideoRender:(UIView *)render pubId:(NSString *)pubId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView | 对方视频的视图窗口
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)

**说明**

该方法用于与会者接通后，与会者视频接通回调中(onRTCOpenRemoteVideoRender)使用。 

#### 24. 设置某个人的显示模式

**定义**

```
- (void)updateRTCVideoRenderModel:(ARVideoRenderMode)videoRenderMode pubId:(NSString *)pubId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
videoRenderMode | ARVideoRenderMode | 显示模式，默认ARVideoRenderScaleToFill，等比例填充视图模式
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)

#### 25. 设置驾驶模式

**定义**

```
- (void)setDriveModel:(BOOL)open;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
open | BOOL | 是否打开，默认关闭

#### 26. 设置某路视频广播

**定义**

```
- (void)setRTCBroadCast:(BOOL)enable peerId:(NSString *)peerId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | 广播与取消广播
peerId | NSString | 视频流Id

#### 27. 1v1授课模式

**定义**

```
- (void)setRTCTalkOnly:(BOOL)enable peerId:(NSString *)peerId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES授课，NO取消授课
peerId | NSString | 视频流Id

#### 28. 发送消息

**定义**

```
- (BOOL)sendUserMessage:(NSString *)userName userHeader:(NSString *)headerUrl content:(NSString *)content;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userName | NSString | 用户昵称，不能为空，否则发送失败(最大256字节)
headerUrl | NSString | 用户头像，可选(最大512字节)
content | NSString | 消息内容不能为空，否则发送失败(最大1024字节)

**返回**

YES发送成功，NO发送失败

**说明**

默认普通消息，以上参数均会出现在参会者的消息回调方法中，如果加入RTC（joinRTC）没有设置userid，发送失败。

#### 29. 设置网络质量是否打开

**定义**

```
- (void)setNetworkStatus:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES打开，NO关闭，默认关闭

#### 30. 获取当前网络状态是否打开

**定义**

```
- (BOOL)networkStatusEnabled;
```

**返回**

获取网络状态。 

#### 31. 设置音频数据是否回调

**定义**

```
- (void)setAudioDataCallBack:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES打开回调，NO关闭回调，默认关闭

#### 32. 获取当前音频回调是否打开

**定义**

```
- (BOOL)audioDataCallBackEnabled;
```

#### 33. 重新采样率

**定义**

```
- (BOOL)resamplerLocalAudio:(int)sampleRate channel:(int)channel;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
sampleRate | int |  采样率，仅支持 8000,16000,32000,44100,48000
channel | int | 频道数量

#### 34. 获取人员列表

**定义**

```
- (NSArray<ARUserItem *> *)getUserList;
```

**返回**

人员列表。 

#### 35. 判断是否可以共享

**定义**

```
- (void)canShare:(int)type;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
type | int | 共享类型，类型自己平台设定，例如1为白板，２为文档

#### 36. 打开共享信息

**定义**

```
- (void)openShareInfo:(NSString *)shearInfo;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
shearInfo | NSString | 共享相关信息(限制512字节)

**说明**

打开白板成功与失败，参考onRTCShareEnable回调方法。 

#### 37. 关闭共享

**定义**

```
- (void)closeShare;
```

#### 38. 设置Zoom显示模式

**定义**

```
- (BOOL)setZoomModel:(ARZoomType)type;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
type | ARZoomType | 模式，默认为ARZoomTypeSingle模式

**说明**

必须先设置会议模式为zoom模式才有效。 

#### 39. 设置显示页码

**定义**

```
- (BOOL)setZoomPage:(int)page;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
page | int | 页码（从0开始，每页加上自己的视频流为4路）

**说明**

分屏显示ARZoomTypeNomal。 

#### 40. 设置显示区域从nIndex 到第几个

**定义**

```
- (BOOL)setZoomPageIndex:(int)index showNum:(int)showNum;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
index | int | 开始标记
showNum | int | 默认为4，onRTCZoomPageInfo回调里的showNum

#### 41. 开始录制

**定义**

```
- (int)startRecording:(NSString *)filePath recordVideo:(BOOL)video;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
filePath | NSString | 沙盒路径+文件名称
video | BOOL | YES录制本地视频；NO不录制视频

#### 42. 结束录制

**定义**

```
- (int)stopRecording;
```
**返回**

0成功，-1失败。 

### ARMeetKitDelegate 接口类

#### 1. 加入会议成功

**定义**

```
- (void)onRTCJoinMeetOK:(NSString *)meetId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
meetId | NSString | anyRTCId 会议号(在开发者业务系统中保持唯一的Id)

#### 2. 加入会议失败

**定义**

```
- (void)onRTCJoinMeetFailed:(NSString *)meetId code:(ARMeetCode)code reason:(NSString *)reason;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
meetId | NSString | 会议号(在开发者业务系统中保持唯一的Id)
code | ARMeetCode | 状态码
token | NSString | 错误原因，RTC错误或者token错误(错误值自己平台定义)

#### 3. RTC服务已断开

**定义**

```
- (void)onRTCConnectionLost;
```

**说明**

收到该消息后，在网络恢复后，会有重连，重连成功，会有onRTCJoinMeetOK回调。 

#### 4. 离开会议

**定义**

```
- (void)onRTCLeaveMeet:(ARMeetCode)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | ARMeetCode | 状态码

#### 5. 其他与会者加入(音视频)

**定义**

```
- (void)onRTCOpenRemoteVideoRender:(NSString *)peerId pubId:(NSString *)pubId userId:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)
userId | NSString | 开发者自己平台的用户Id
userData | NSString | 开发者自己平台的相关信息（昵称，头像等)

**说明**

其他与会者进入会议的回调，开发者需调用设置其他与会者视频窗口(setRemoteVideoRender)方法。 

#### 6. 其他与会者离开(音视频)

**定义**

```
- (void)onRTCCloseRemoteVideoRender:(NSString *)peerId pubId:(NSString *)pubId userId:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)
userId | NSString | 开发者自己平台的用户Id

**说明**

其他与会者离开将会回调此方法，需本地移除与会者视频视图。 

#### 7. 其他与会者加入(音频)

**定义**

```
- (void)onRTCOpenRemoteAudioTrack:(NSString *)peerId userId:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
userId | NSString | 开发者自己平台的Id
userData | NSString | 开发者自己平台的相关信息（昵称、头像等)

#### 8. 其他与会者离开(音频)

**定义**

```
- (void)onRTCCloseRemoteAudioTrack:(NSString *)peerId userId:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
userId | NSString | 开发者自己平台的用户Id

#### 9. 用户开启桌面共享

**定义**

```
- (void)onRTCOpenRemoteScreenRender:(NSString *)peerId pubId:(NSString *)pubId userId:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)
userId | NSString | 开发者自己平台的Id
userData | NSString | 开发者自己平台的相关信息（昵称、头像等)

**说明**

开发者需调用设置其他与会者视频窗口(setRTCVideoRender)方法。 

#### 10. 用户退出桌面共享

**定义**

```
- (void)onRTCCloseRemoteScreenRender:(NSString *)peerId pubId:(NSString *)pubId userId:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)
userId | NSString | 开发者自己平台的用户Id

**说明**

其他与会者离开将会回调此方法，需本地移除屏幕共享窗口。

#### 11. 其他与会者对音视频的操作

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

#### 12. 别人对自己音视频的操作

**定义**

```
- (void)onRTCLocalAVStatus:(BOOL)audio video:(BOOL)video;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
audio | BOOL | YES为打开音频，NO为关闭音频
video | BOOL | YES为打开视频，NO为关闭视频

#### 13. 本地视频第一帧

**定义**

```
- (void)onRTCFirstLocalVideoFrame:(CGSize)size;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 14. 远程视频第一帧

**定义**

```
- (void)onRTCFirstRemoteVideoFrame:(CGSize)size pubId:(NSString *)pubId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | NSString | 视频窗口大小
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)

#### 15. 本地窗口大小的回调

**定义**

```
- (void)onRTCLocalVideoViewChanged:(CGSize)size;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 16. 远程窗口大小的回调

**定义**

```
- (void)onRTCRemoteVideoViewChanged:(CGSize)size pubId:(NSString *)pubId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)

#### 17. 其他与会者音频检测回调

**定义**

```
- (void)onRTCRemoteAudioActive:(NSString *)peerId userId:(NSString *)userId audioLevel:(int)level showTime:(int)time;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的与会者标识Id（用于标识与会者用户，每次随机生成）
userId | NSString | 开发者自己平台的用户Id
level | int | 音频大小（0~100）
time | int | 音频检测在nTime毫秒内不会再回调该方法（单位：毫秒）

**说明**

与会者关闭音频后（setLocalAudioEnable为NO）,该回调将不再回调。对方关闭音频检测后（setAudioActiveCheck为NO）,该回调也将不再回调。 

#### 18. 本地音频检测回调

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

本地关闭音频后（setLocalAudioEnable为NO）,该回调将不再回调。对方关闭音频检测后（setAudioActiveCheck为NO）,该回调也将不再回调。 

#### 19. 其他与会者网络质量回调

**定义**

```
- (void)onRTCRemoteNetworkStatus:(NSString *)peerId userId:(NSString *)userId netSpeed:(int)netSpeed packetLost:(int)packetLost netQuality:(ARNetQuality)netQuality;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的与会者标识Id（用于标识与会者用户，每次随机生成）
userId | NSString | 用户平台Id
netSpeed | int | 网络上行
packetLost | int | 丢包率
netQuality | ARNetQuality | 网络质量

#### 20. 本地网络质量回调

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

#### 21. 收到消息回调

**定义**

```
- (void)onRTCUserMessage:(NSString *)userId userName:(NSString *)userName userHeader:(NSString *)headerUrl content:(NSString *)content;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 发送消息者在自己平台下的Id
userName | NSString | 发送消息者的昵称
headerUrl | NSString | 发送者的头像
content | NSString | 消息内容

#### 22. 主持人上线

**定义**

```
- (void)onRTCHosterOnLine:(NSString *)peerId userId:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
userId | NSString | 开发者自己平台的Id
userData | NSString | 开发者自己平台的相关信息（昵称，头像等)

**说明**

只有主持模式下的游客身份登录才有用。 

#### 23. 主持人下线

**定义**

```
- (void)onRTCHosterOffLine:(NSString *)peerId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)

**说明**

只有主持模式下的游客身份登录才有用。 

#### 24. 1v1授课开启

**定义**

```
- (void)onRTCTalkOnlyOn:(NSString *)peerId userId:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)
userId | NSString | 开发者自己平台的Id
userData | NSString | 开发者自己平台的相关信息（昵称，头像等)

#### 25. 1v1授课关闭

**定义**

```
- (void)onRTCTalkOnlyOff:(NSString *)peerId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)

#### 26. zoom模式页码变化

**定义**

```
- (void)onRTCZoomPageInfo:(ARZoomType)zoomType
allPage:(int)allPage
currentPage:(int)currentPage
allRenderNum:(int)allRenderNum
beginIndex:(int)index
showNum:(int)showNum;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
zoomType | ARZoomType | 当前模式
allPage | int | 总页码（一页显示4个）
currentPage | int | 当前页
allRenderNum | int | 当前服务上有多少个渲染，根据此数量来判断页码添加删除
index | int | 开始位置
showNum | int | 显示多少个

#### 27. 获取视频的原始采集数据

**定义**

```
- (CVPixelBufferRef)onRTCCaptureVideoPixelBuffer:(CMSampleBufferRef)sampleBuffer;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
sampleBuffer | CMSampleBufferRef | 视频数据

#### 28. 本地音频数据回调

**定义**

```
- (void)onRTCLocalAudioPcmBuffer:(const char * _Nullable)audioSamples sample:(int)sampleRate channel:(int)channel length:(int)length;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
audioSamples | char | pcm数据
sampleRate | int | 采样率
channel | int | 声道
length | int | 数据长度

#### 29. 远程音频数据回调

**定义**

```
- (void)onRTCRemoteAudioPcmBuffer:(const char * _Nullable)audioSamples sample:(int)sampleRate channel:(int)channel length:(int)length peerId:(NSString *_Nullable)peerId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
audioSamples | char | pcm数据
sampleRate | int | 采样率
channel | int | 声道
length | int | 数据长度
peerId | NSString | RTC服务生成的与会者标识Id（用于标识与会者用户，每次随机生成）

### ARShareDelegate 接口类

#### 1. 判断是否可以开启共享回调

**定义**

```
- (void)onRTCShareEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES可以共享，NO不可以共享

#### 2. 共享开启

**定义**

```
- (void)onRTCShareOpen:(int)type shareInfo:(NSString *)shareInfo userId:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
type | int | 共享类型
shareInfo | NSString | 共享信息
userId | NSString | 开发者自己平台的Id
userData | NSString | 开发者自己平台的相关信息（昵称，头像等)

#### 3. 共享关闭

**定义**

```
- (void)onRTCShareClose;
```

**说明**

打开共享的人关闭了共享。 

## 三、更新日志

**Version 3.0.1 （2019-10-15）**

* 新增塞流接口（setExternalCameraCapturer）；

* 新增流量信息监测以及音频数据信息；

* 新增录制相关方法（startRecording:recordVideo:）。

**Version 3.0.0 （2019-05-15）**

* SDK版本升级3.0，API接口变更

**Version 2.0.0 （2017-09-30）**

* SDK版本升级2.0，梳理、完善SDK
