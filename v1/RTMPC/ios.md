## 一、快速开始

### 集成指南

#### 适用范围

本集成文档适用于iOS RTMPCHybirdEngine SDK 2.0.0 ~ 3.0.1版本

#### 准备环境

- Xcode 9.0+
- iOS 8.0+ 真机（iPhone 或 iPad）
- 请确保你的项目已设置有效的开发者签名

#### 导入SDK

**CocoaPods导入**

* 通过 Cocoapods 下载地址：

```
pod 'RTMPCHybirdEngine'
```
* 如果需要安装指定版本则使用以下方式（以 3.0.1 版本为例）：

```
pod 'RTMPCHybirdEngine', '3.0.1'
```

**手动导入**

* 前往GitHub[下载Demo](https://github.com/anyRTC/anyRTC-RTMPC-iOS)，找到RTMPCHybirdEngine.framework；

* 在Xcode中选择“Add files to 'Your project name'...”，将RTMPCHybirdEngine.framework添加到你的工程目录中
![ios_rtmpc_01](/assets/images/ios/ios_rtmpc_01.png)

* 打开General->Embedded Binaries中添加RTMPCHybirdEngine.framework

![ios_rtmpc_02](/assets/images/ios/ios_rtmpc_02.png)

#### 权限说明

使用RTMPCHybirdEngine SDK 前，需要对设备进行授权。打开 info.plist ，点击 + 图标开始添加：

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

![ios_rtmpc_03](/assets/images/ios/ios_rtmpc_03.png)

### 开发指南

#### 1. 初始化SDK

集成SDK后，还需对SDK进行初始化操作，建议在AppDelegate中完成。

##### 1.1 导入头文件

```
#import <RTMPCHybirdEngine/ARRtmpSDK.h>
```

##### 1.2 配置开发者信息

调用initEngine:token:方法配置开发者信息，开发者信息可在anyRTC管理后台中获得，详见[创建anyRTC账号](https://docs.anyrtc.io)


**示例代码：**

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
// Override point for customization after application launch.
//配置开发者信息
[ARRtmpEngine initEngine:appID token:token];
return YES;
}
```

#### 2. 主播端

##### 2.1 实例化主播对象

调用initWithDelegate:option:方法用于实例化主播对象，，需实现ARHosterRtmpDelegate中的回调方法。第二个参数option为配置信息，包括直播模式、相机类型、帧率、码率等。同时需要实现ARHosterRtcDelegate的回调。

直播模式：

* 音视频直播（ARLivingMediaModeVideo）

* 音频直播（ARLivingMediaModeAudio）

**示例代码：**

```
//配置信息
ARHosterOption *option = [ARHosterOption defaultOption];
//直播模式
option.livingMediaMode = ARLivingMediaModeVideo;
//相机类型
option.cameraType = ARRtmpCameraTypeBeauty;
//实例化主播对象
self.hosterKit = [[ARRtmpHosterKit alloc] initWithDelegate:self option:option];
//rtc回调
self.hosterKit.rtc_delegate = self;
```

##### 2.2 设置本地视频采集窗口

调用setLocalVideoCapturer:方法设置本地视频采集窗口。

**示例代码：**

```
[self.hosterKit setLocalVideoCapturer:self.view];

```

##### 2.3 开始推流

调用startPushRtmpStream:方法开始推流。

**示例代码：**

```
[self.hosterKit startPushRtmpStream:self.liveInfo.push_url];
```

##### 2.4 创建RTC直播间

调用createRTCLineByToken:liveId:userId:userData:liveInfo:方法创建RTC直播间。第一个参数token为令牌，可为空，具体用法可参考[安全指南](https://docs.anyrtc.io/v1/security/服务级安全设置指南.html)。


**示例代码：**

```

//用户信息
ArUserInfo *userInfo = ArUserManager.getUserInfo;
NSDictionary *userData = [NSDictionary dictionaryWithObjectsAndKeys:[NSNumber numberWithInt:1],@"isHost",userInfo.userid,@"userId",userInfo.nickname,@"nickName",userInfo.avatar,@"headUrl", nil];
//直播信息
NSDictionary *liveData = [NSDictionary dictionaryWithObjectsAndKeys:self.liveInfo.pull_url,@"rtmpUrl",self.liveInfo.hls_url,@"hlsUrl",self.liveInfo.anyrtcId,@"anyrtcId",self.liveInfo.anyrtcId,@"liveTopic",[NSNumber numberWithInt:0],@"isLiveLandscape",[NSNumber numberWithInt:0],@"isAudioLive",userInfo.nickname,@"hosterName",nil];

[self.hosterKit createRTCLineByToken:nil liveId:self.liveInfo.anyrtcId userId:userInfo.userid userData:[ArCommon fromDicToJSONStr:userData] liveInfo:[ArCommon fromDicToJSONStr:liveData]]

```

##### 2.5 连麦请求

主播收到游客连麦请求回调（onRTCApplyToLine:），主播可以选择同意（acceptRTCLine）或拒绝（rejectRTCLine）。

**示例代码：**

```
//主播收到游客连麦请求
- (void)onRTCApplyToLine:(NSString *)peerId userId:(NSString *)userId userData:(NSString *)userData {
//同意连麦
[self.hosterKit acceptRTCLine:peerId];
//拒绝连麦
//[self.hosterKit rejectRTCLine:peerId];
}

```

##### 2.6 设置连麦者视频窗口

主播收到游客视频连麦接通回调时（onRTCOpenRemoteVideoRender:），可调用setRemoteVideoRender:pubId:方法用于设置连麦者视频窗口。

**示例代码：**

```
//游客视频连麦接通
- (void)onRTCOpenRemoteVideoRender:(NSString *)peerId pubId:(NSString *)pubId userId:(NSString *)userId userData:(NSString *)userData {
UIView *videoView = [UIView alloc]initWithFrame:CGRectMake(0, 0, 90, 160);
[self.view addSubview:videoView];
[self.hosterKit setRemoteVideoRender:videoView pubId:pubId];
}
```

##### 2.7 销毁主播对象

当主播离开房间时，可调用clear方法销毁主播对象。

**示例代码：**

```
[self.hosterKit clear];
```

#### 3. 游客端

##### 3.1 实例化游客对象

调用initWithDelegate:option:方法实例化游客对象，需实现ARGuestRtmpDelegate中的回调方法。第二个参数option为游客配置信息，包括连麦模式、播放器显示模式、视频方向等等。同时需要实现ARGuestRtcDelegate的回调。

**示例代码：**

```
//配置信息
ARGuestOption *option = [ARGuestOption defaultOption];
//音视频连麦（默认）
option.linkMediaModel = ARLinkMediaModeVideo;
//实例化游客对象
self.guestKit = [[ARRtmpGuestKit alloc] initWithDelegate:self option:option];
//rtc回调
self.guestKit.rtc_delegate = self;

```

##### 3.2 开始RTMP播放

调用startRtmpPlay:render:方法用于拉流。

**示例代码：**

```
[self.guestKit startRtmpPlay:self.mainModel.rtmpUrl render:self.localView];
```

##### 3.3 加入RTC直播间

调用joinRTCLineByToken:liveId:userID:userData:方法加入RTC直播，第一个参数token为令牌，可为空，具体用法可参考[安全指南](https://docs.anyrtc.io/v1/security/服务级安全设置指南.html)。

**示例代码：**

```
//加入RTC
ArUserInfo *userInfo = ArUserManager.getUserInfo;
NSDictionary *userData = [NSDictionary dictionaryWithObjectsAndKeys:userInfo.nickname,@"nickName",@"",@"headUrl" ,nil];
[self.guestKit joinRTCLineByToken:nil liveId:self.mainModel.anyrtcId userID:userInfo.userid userData:[ArCommon fromDicToJSONStr:userData]];
```

##### 3.4 申请连麦

调用applyRTCLine方法申请连麦。

**示例代码：**

```
[self.guestKit applyRTCLine];
```

##### 3.5 设置本地视频采集

当游客申请连麦结果回调（onRTCApplyLineResult）为ARRtmp_OK时，调用setLocalVideoCapturer:方法用于设置本地视频采集。

**示例代码：**

```
//游客申请连麦结果回调
- (void)onRTCApplyLineResult:(ARRtmpCode)code {
if (code == ARRtmp_OK) {
UIView *videoView = [UIView alloc]initWithFrame:CGRectMake(0, 0, 90, 160);
[self.view addSubview:videoView];
[self.guestKit setLocalVideoCapturer:videoView];
}
}
```

##### 3.6 设置其他连麦者视频窗口

游客请求连麦成功后，如果当前有其他人正在连麦，会收到其他游客视频连麦接通的回调（onRTCOpenRemoteVideoRender:），可调用setRemoteVideoRender:pubId:方法用于设置连麦者视频窗口。

**示例代码：**

```
//其他游客视频连麦接通
- (void)onRTCOpenRemoteVideoRender:(NSString *)peerId pubId:(NSString *)pubId userId:(NSString *)userId userData:(NSString *)userData {
UIView *videoView = [UIView alloc]initWithFrame:CGRectMake(0, 0, 90, 160);
[self.view addSubview:videoView];
[self.guestKit setRemoteVideoRender:videoView pubId:pubId];
}

```

##### 3.7 销毁游客对象

当游客离开房间时，可调用clear方法销毁游客对象。

**示例代码：**

```
[self.guestKit clear];
```

## 二、API接口文档

### ARRtmpEngine 接口类

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

RTMPC SDK 版本号

#### 4. 设置打印日志级别

**定义**

```
+ (void)setLogLevel:(ARLogModel)levelModel;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
levelModel | ARLogModel | 日志级别

### ARRtmpHosterKit 接口类

#### 1. 实例化主播对象

**定义**

```
- (instancetype)initWithDelegate:(id<ARHosterRtmpDelegate>)delegate option:(ARHosterOption *)option;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
delegate | id<ARHosterRtmpDelegate> | RTMP推流相关回调代理
option | ARHosterOption | 配置项

**说明**

主播对象实例。

#### 2. 销毁主播对象

**定义**

```
- (void)clear;
```

**说明**

相当于析构函数。

#### 3. 设置本地视频采集窗口

**定义**

```
- (void)setLocalVideoCapturer:(UIView *)render;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView | 视频显示对象

#### 4. 开始推流

**定义**

```
- (BOOL)startPushRtmpStream:(NSString *)pushUrl;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
pushUrl | NSString | RTMP推流地址

**返回**

YES为推流成功，NO为推流失败。

#### 5. 停止推流

**定义**

```
- (void)stopRtmpStream;
```

**说明**

调用此方法停止推流并且关闭RTC服务，直播关闭。

#### 6. 设置本地音频是否传输

**定义**

```
- (void)setLocalAudioEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES为传输音频，NO为不传输音频，默认传输

#### 7. 设置本地视频是否传输

**定义**

```
- (void)setLocalVideoEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES为传输视频，NO为不传输视频，默认视频传输

#### 8. 切换前后摄像头

**定义**

```
- (void)switchCamera;
```

#### 9. 设置滤镜

**定义**

```
- (void)setCameraFilter:(ARCameraFilterMode)filter;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
filter | ARCameraFilterMode | 滤镜模式

**说明**

使用美颜相机模式，默认开启美颜。

#### 10. 打开手机闪光灯

**定义**

```
- (BOOL)openCameraTorchMode:(BOOL)on;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
on | BOOL | YES为打开，NO为关闭

**返回**

操作是否成功

#### 11. 打开对焦功能(iOS特有)

**定义**

```
- (void)setCameraFocusImage:(UIImage *)image;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
image | UIImage | 点击自动对焦时的图片

**说明**

默认关闭自动对焦。

#### 12. 设置相机焦距

**定义**

```
- (void)setCameraZoom:(CGFloat)distance;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
distance | CGFloat | 焦距调整(1.0~3.0)，默认为1.0

#### 13. 获取相机当前焦距

**定义**

```
- (CGFloat)getCameraZoom
```

**返回**

相机当前焦距(1.0~3.0)。

#### 14. 设置本地前置摄像头镜像是否打开

**定义**

```
- (BOOL)setFontCameraMirrorEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES为打开，NO为关闭，默认打开

**返回**

操作是否成功。

#### 15. 设置水印

**定义**

```
- (void)setLogoView:(UIView *)logoView;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
logoView | UIView | 水印视图

**说明**

只有美颜相机才有用。

#### 16. 设置token验证

**定义**

```
- (BOOL)setUserToken:(NSString *)userToken;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userToken | NSString | token字符串，客户端向自己服务器申请

**说明**

设置token验证必须放在createRTCLine之前。

#### 17. 创建RTC链接

**定义**

```
- (BOOL)createRTCLine:(NSString *)anyRTCId userId:(NSString *)userId userData:(NSString *)userData liveInfo:(NSString *)liveInfo
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
anyRTCId | NSString | 开发者业务系统中保持唯一的id（必填）
userId | NSString | 主播在开发者自己平台的id，可选
userData | NSString | 主播在开发者自己平台的相关信息（昵称，头像等），可选。(限制512字节)
liveInfo | NSString | 直播间相关信息（推流地址，直播间名称等），可选。（限制1024字节）

**返回**

打开RTC成功与否。

**说明**

userId若不设置，发消息接口不能使用。userData，将会出现在游客连麦回调中，若不设置，人员上下线接口将无用。liveInfo该信息在ARRtmpHttpKit类中getLivingList方法中获取。若不设置，获取直播列表信息为空。该方法须在开始推流（startPushRtmpStream）方法后调用。

#### 18. 同意游客连麦请求

**定义**

```
- (BOOL)acceptRTCLine:(NSString *)peerId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的连麦者标识Id 。(用于标识连麦用户，每次连麦随机生成)

**返回**

YES为同意连麦，NO为同意连麦失败（可能超过连麦数量）

**说明**

调用此方法即可同意游客的连麦请求。同意后视频直播模式下将会回调游客视频连麦接通方法，具体操作可查看ARRtmpHosterKitDelegate回调。音频直播模式下将会回调游客音频连麦接通方法，具体操作可查看 游客音频连麦接通回调。

#### 19. 拒绝游客连麦请求

**定义**

```
- (void)rejectRTCLine:(NSString *)peerId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | peerId RTC服务生成的连麦者标识Id 。(用于标识连麦用户，每次连麦随机生成)

**说明**

当有游客请求连麦时，可调用此方法拒绝。

#### 20. 设置连麦者视频窗口

**定义**

```
- (void)setRemoteVideoRender:(UIView *)render pubId:(NSString *)pubId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView | 对方视频的窗口，本地设置
pubId | NSString | 连麦者视频流id(用于标识连麦者发布的流)

**说明**

该方法用于游客申请连麦接通后，游客视频连麦接通回调中(onRTCOpenRemoteVideoRender)使用。

#### 21. 挂断游客连麦

**定义**

```
- (void)hangupRTCLine:(NSString *)peerId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的连麦者标识Id 。(用于标识连麦用户，每次连麦随机生成)

**说明**

与游客连麦过程中，可调用此方法挂断连麦。

#### 22. 发送消息

**定义**

```
- (BOOL)sendUserMessage:(ARMessageType)type userName:(NSString *)userName userHeader:(NSString *)headerUrl content:(NSString *)content;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
type | ARMessageType | 消息类型，ARNomalMessageType普通消息，ARBarrageMessageType弹幕消息
userName | NSString | 用户昵称，不能为空，否则发送失败(最大256字节)
headerUrl | NSString | 用户头像，可选(最大512字节)
content | NSString | 消息内容不能为空，否则发送失败(最大1024字节)

**返回值**

YES发送成功，NO发送失败。

**说明**

默认普通消息，以上参数均会出现在游客/主播消息回调方法中，如果创建RTC连接（createRTCLine）没有设置userId，发送失败。

#### 23. 关闭RTC链接

**定义**

```
- (void)closeRTCLine;
```

**说明**

主播端如果调用此方法，将会关闭RTC服务，若开启了直播在线功能（可在www.anyrtc.io 应用管理中心开通），游客端会收到主播已离开(onRTCLineLeave)的回调。

#### 24. 设置副视频、连麦用户关闭视频后显示填充图

**定义**

```
- (void)setVideoSubBackground:(NSString *)filePath;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
filePath | NSString | 图片的地址（jpg,jpeg格式的图片）

**说明**

vip专用接口，预想使用，请联系商务。

#### 25. 设置合成视频显示模板

**定义**

```
- (void)setMixVideoModel:(ARMixVideoLayoutModel)mixModel;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
mixModel | ARMixVideoLayoutModel | 模板类型

**说明**

该方法配合setVideoTemplate方法使用。

#### 26. 设置合成流显示位置

**定义**

```
- (void)setVideoTemplate:(ARMixVideoHorModel)hor ver:(ARMixVideoVerModel)ver dir:(ARMixVideoDirModel)dir horPad:(int)horPad verPad:(int)verPad borderWidth:(int)borderWidth;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
hor | ARMixVideoHorModel | 水平排布 ARMixVideoHorLeftModel/ARMixVideoHorCenterModel/ARMixVideoHorRightModel:水平左边/水平中间/水平右边
ver | ARMixVideoVerModel | 竖直排布 ARMixVideoVerTopModel/ARMixVideoVerCenterModel/ARMixVideoVerBottomModel:垂直顶部/垂直居中/垂直底部
dir | ARMixVideoDirModel | 排布方向 ARMixVideoDirHorModel/ARMixVideoDirVerModel:水平排布/垂直排布
horPad | int | 水平的间距（左右间距：最左边或者最后边的视频离边框的距离）
verPad | int | 垂直的间距（上下间距：最上面或者最下面离边框的距离）
borderWidth | int | 小窗口的边框的宽度（边框为白色）

### ARHosterRtmpDelegate 接口类

#### 1. RTMP 服务连接成功

**定义**

```
- (void)onRtmpStreamOk;
```

**说明**

RTMP连接成功，视频正在缓存。

#### 2. RTMP 服务重连

**定义**

```
- (void)onRtmpStreamReconnecting:(int)times;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
times | int | 重连次数

#### 3. RTMP 推流状态

**定义**

```
- (void)onRtmpStreamStatus:(int)delayTime netBand:(int)netBand;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
delayTime | int | 推流的延迟时间（单位：ms）
netBand | int | 当前的上行带宽（单位：byte）

#### 4. RTMP 服务连接失败

**定义**

```
- (void)onRtmpStreamFailed:(ARRtmpCode)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | ARRtmpCode | 状态码

#### 5. RTMP 服务关闭

**定义**

```
- (void)onRtmpStreamClosed;
```

**说明**

停止推流时回调该方法。



### ARHosterRtcDelegate 接口类

#### 1. 创建RTC服务连接结果

**定义**

```
- (void)onRTCCreateLineResult:(ARRtmpCode)code reason:(NSString *)reason;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
appId | ARRtmpCode | 状态码
reason | NSString | 错误原因，RTC错误或者token错误（错误值自己平台定义）

#### 2. 主播收到游客连麦请求

**定义**

```
- (void)onRTCApplyToLine:(NSString *)peerId userId:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | 连麦者标识Id（用于标识连麦用户，每次连麦随机生成)
userId | NSString | 游客在自己业务平台的Id
userData | NSString | 自定义参数（可查看游客端加入RTC连接方法)

**说明**

当有游客申请连麦时将会走此回调方法，可以在回调中调用拒绝(rejectRTCLine)或同意连麦(acceptRTCLine)。

#### 3. 游客取消连麦申请

**定义**

```
- (void)onRTCCancelLine:(ARRtmpCode)code peerId:(NSString *)peerId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | ARRtmpCode | 状态码
peerId | NSString | 连麦者标识Id（用于标识连麦用户，每次连麦随机生成）

**说明**

游客取消申请连麦后将会回调此方法或者同意别人的连麦，连麦数已满。

#### 4. RTC 服务关闭

**定义**

```
- (void)onRTCLineClosed:(ARRtmpCode)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | ARRtmpCode | 状态码

#### 5. 游客视频连麦接通

**定义**

```
- (void)onRTCOpenRemoteVideoRender:(NSString *)peerId pubId:(NSString *)pubId userId:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | 连麦者标识Id（用于标识连麦用户，每次连麦随机生成）
pubId | NSString | 连麦者视频流Id(用于标识连麦者发布的流)
userId | NSString | 游客在开发者平台的用户Id
userData | NSString | 游客加入RTC连接的自定义参数体（可查看游客端加入RTC连接方法)

**说明**

主播与游客的连麦接通后视频将要显示的回调，开发者需调用设置连麦者视频窗口（setRemoteVideoRender）方法。

#### 6. 游客视频连麦挂断

**定义**

```
- (void)onRTCCloseRemoteVideoRender:(NSString *)peerId pubId:(NSString *)pubId userId:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | 连麦者标识Id（用于标识连麦用户，每次连麦随机生成)
pubId | NSString | 连麦者视频流Id(用于标识连麦者发布的流)
userId | NSString | 游客在开发者平台的用户Id

**说明**

主播与游客的连麦挂断后将会回调此方法，挂断不分游客主动挂断还是主播挂断游客，均会回调此方法，需本地移除连麦者视图。

#### 7. 游客音频连麦接通

**定义**

```
- (void)onRTCOpenRemoteAudioLine:(NSString *)peerId userId:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | 连麦者标识Id（用于标识连麦用户，每次连麦随机生成）
userId | NSString | 游客在开发者平台的用户Id
userData | NSString | 游客加入RTC连接的自定义参数体（可查看游客端加入RTC连接方法）

**说明**

该回调仅在音频直播时有效。

#### 8. 游客音频连麦挂断

**定义**

```
- (void)onRTCCloseRemoteAudioLine:(NSString *)peerId userId:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | 连麦者标识Id（用于标识连麦用户，每次连麦随机生成）
userId | NSString | 游客在开发者平台的用户Id

**说明**

该回调仅在音频直播时有效。

#### 9. 其他人对音视频的操作

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

**说明**

例如对方关闭了音频，对方关闭了视频。

#### 10. 本地视频第一帧

**定义**

```
- (void)onRTCFirstLocalVideoFrame:(CGSize)size;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 11. 远程视频第一帧

**定义**

```
- (void)onRTCFirstRemoteVideoFrame:(CGSize)size pubId:(NSString *)pubId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)

#### 12. 本地窗口大小的回调

**定义**

```
- (void)onRTCLocalVideoViewChanged:(CGSize)size;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 13. 远程窗口大小的回调

**定义**

```
- (void)onRTCRemoteVideoViewChanged:(CGSize)size pubId:(NSString *)pubId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)

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

本地关闭音频后（setLocalAudioEnable为NO）,该回调将不再回调。

#### 15. 其他与会者音频检测回调

**定义**

```
- (void)onRTCRemoteAudioActive:(NSString *)peerId userId:(NSString *)userId audioLevel:(int)level showTime:(int)time;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的连麦者标识Id（用于标识连麦用户，每次连麦随机生成）
userId | NSString | 连麦者在自己平台的用户Id
level | NSString | 音频大小（0~100）
time | int | 音频检测在nTime毫秒内不会再回调该方法（单位：毫秒）

**说明**

与会者关闭音频后（setLocalAudioEnable为NO）,该回调将不再回调。

#### 16. 收到消息回调

**定义**

```
- (void)onRTCUserMessage:(ARMessageType)type userId:(NSString *)userId userName:(NSString *)userName userHeader:(NSString *)headerUrl content:(NSString *)content;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
type | ARMessageType | 消息类型，ARNomalMessageType普通消息，ARBarrageMessageType弹幕消息
userId | NSString | 发送消息者在自己平台下的Id
userName | NSString | 发送消息者的昵称
headerUrl | NSString | 发送者的头像
content | NSString | 消息内容

#### 17. 直播间实时在线人数变化通知

**定义**

```
- (void)onRTCMemberListNotify:(NSString *)serverId roomId:(NSString *)roomId allMember:(int)totalMember;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
serverId | NSString | 服务器Id
roomId | NSString | 房间Id
totalMember | int | 当前在线人数

**说明**

serverId和roomId参数用于请求人员列表。

#### 18. 获取视频的原始采集数据

**定义**

```
- (CVPixelBufferRef)onRTCCaptureVideoPixelBuffer:(CMSampleBufferRef)sampleBuffer;
```

**参数**

| 参数名       |       类型        | 描述     |
| ------------ | :---------------: | -------- |
| sampleBuffer | CMSampleBufferRef | 视频数据 |

**返回值**

视频对象（处理过或者没做处理）。

**说明**

获取视频原始数据，先设置摄像机类型为ARRtmpCameraTypeThreeFilter；

### ARRtmpGuestKit 接口类

#### 1. 实例化游客对象

**定义**

```
- (instancetype)initWithDelegate:(id<ARGuestRtmpDelegate>)delegate option:(ARGuestOption *)option
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
delegate | id<ARGuestRtmpDelegate> | RTMP相关回调代理
option | ARGuestOption | 配置项

**返回**

游客对象。

#### 2. 销毁游客端对象

**定义**

```
- (void)clear;
```

**说明**

销毁游客端资源，可在直播结束，页面关闭时调用。

#### 3. 设置音频是否传输

**定义**

```
- (void)setLocalAudioEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES传输音频，NO不传输音频，默认音频传输

#### 4. 设置视频是否传输

**定义**

```
- (void)setLocalVideoEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES传输视频，NO不传输视频，默认视频传输

#### 5. 设置本地视频采集

**定义**

```
- (void)setLocalVideoCapturer:(UIView *)render;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView | 视频显示对象

**说明**

用于连麦接通后(onRTCApplyLineResult)，设置本地连麦图像采集。

#### 6. 设置其他连麦者视频窗口

**定义**

```
- (void)setRemoteVideoRender:(UIView *)render pubId:(NSString *)pubId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
render | UIView | 显示对方视频
pubId | NSString | 连麦者视频流id(用于标识连麦者发布的流)

**说明**

该方法用于其他游客视频连麦接通回调方法（onRTCOpenRemoteVideoRender）中。

#### 7. 切换前后摄像头

**定义**

```
- (void)switchCamera;
```

#### 8. 打开手机闪光灯

**定义**

```
-(BOOL)openCameraTorchMode:(BOOL)on;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
on | BOOL | YES打开，NO关闭

**说明**

操作是否成功。

#### 9. 打开对焦功能(iOS特有)

**定义**

```
- (void)setCameraFocusImage:(UIImage *)image;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
image | UIImage | 点击自动对焦时的图片

**说明**

默认关闭自动对焦。

#### 10. 设置相机焦距

**定义**

```
- (void)setCameraZoom:(CGFloat)distance;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
distance | CGFloat | 焦距调整(1.0~3.0)，默认为1.0

#### 11. 获取相机当前焦距

**定义**

```
- (CGFloat)getCameraZoom;
```

**返回**

相机当前焦距(1.0~3.0)。

#### 12. 设置本地前置摄像头镜像是否打开

**定义**

```
- (BOOL)setFontCameraMirrorEnable:(BOOL)enable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | BOOL | YES为打开，NO为关闭，默认打开

#### 13. 开始RTMP播放

**定义**

```
- (BOOL)startRtmpPlay:(NSString *)rtmpUrl render:(UIView *)render;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
rtmpUrl | NSString | RTMP拉流地址
render | NSString | 视频显示对象

**返回值**

YES成功，NO失败。

#### 14. 设置播放器显示模式

**定义**

```
- (void)updatePlayerRenderModel:(ARVideoRenderMode)videoRenderMode;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
videoRenderMode | ARVideoRenderMode | 显示模式，默认ARVideoRenderScaleAspectFill，等比例填充视图模式

#### 15. 停止RTMP播放

**定义**

```
- (BOOL)stopRtmpPlay;
```
**返回**

操作是否成功。

**说明**

调用此方法相当于停止拉流并且关闭RTC服务。

#### 16. 设置token验证

**定义**

```
- (BOOL)setUserToken:(NSString *)userToken;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userToken | NSString | token字符串，客户端向自己服务器申请

**返回**

设置token验证是否成功

**说明**

设置token验证必须放在joinRTCLine之前。

#### 17. 加入RTC连接

**定义**

```
- (BOOL)joinRTCLine:(NSString *)anyRTCId userID:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
anyRTCId | NSString | 对应主播端的anyRTCId
userId | NSString | 游客业务平台的业务Id，可选，如果不设置userId，发消息接口不能使用
userData | NSString | 游客业务平台的相关信息（昵称，头像等），可选(限制512字节)

**返回**

打开RTC成功与否，成功后可以进行连麦等操作

**说明**

userId若不设置，发消息接口不能使用。userData将会出现在游客连麦回调中，若不设置，人员上下线接口将无用，建议设置，此方法需在startRtmpPlay之后调用。

#### 18. 申请连麦

**定义**

```
- (BOOL)applyRTCLine;
```
**返回**

YES申请成功，NO申请失败

**说明**

申请失败的时候，注意输出日志，权限问题。

#### 19. 挂断连麦

**定义**

```
- (void)hangupRTCLine;
```

**说明**

游客取消连麦、挂断连麦均调此方法。

#### 20. 发送消息

**定义**

```
- (BOOL)sendUserMessage:(ARMessageType)type userName:(NSString *)userName userHeader:(NSString *)headerUrl content:(NSString *)content
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
type | ARMessageType | 消息类型，ARNomalMessageType普通消息，ARBarrageMessageType弹幕消息
userName | NSString | 用户昵称，不能为空，否则发送失败(最大256字节)
headerUrl | NSString | 用户头像，可选(最大512字节)
content | NSString | 消息内容，不能为空，否则发送失败(最大1024字节)

**返回**

YES发送成功，NO发送失败。

**说明**

默认普通消息，以上参数均会出现在游客/主播消息回调方法中, 如果加入RTC连麦（joinRTCLine）没有设置userId，发送失败。

#### 21. 关闭RTC连接

**定义**

```
- (void)leaveRTCLine;
```

**说明**

用于关闭RTC服务，将无法进行聊天互动，人员上下线等。

### ARGuestRtmpDelegate接口类

#### 1. RTMP 连接成功

**定义**

```
- (void)onRtmpPlayerOk;
```

**说明**

RTMP服务器连接成功，视频正在缓存。

#### 2. RTMP 开始播放

**定义**

```
- (void)onRtmpPlayerStart;
```

**说明**

第一帧视频图像。

#### 3. RTMP 当前播放状态

**定义**

```
- (void)onRtmpPlayerStatus:(int)cacheTime bitrate:(int)bitrate;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
cacheTime | int | 缓存时间 (单位：ms)
bitrate | int | 当前码率大小（单位：byte）

**说明**

当主播处于直播状态会一直调用此方法。

#### 4. RTMP播放缓冲进度

**定义**

```
- (void)onRtmpPlayerLoading:(int)percent;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
percent | int | 缓冲百分比(0-100)

**说明**

回调该方法时，percent为0时，页面可以进行缓冲提示，当为100时，缓冲提示去掉。

#### 5. RTMP播放器关闭

**定义**

```
- (void)onRtmpPlayerClosed:(ARRtmpCode)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | ARRtmpCode | 状态码

**说明**

主播停止推流会回调此方法。

### ARGuestRtcDelegate 接口类

#### 1. RTC服务连接结果

**定义**

```
- (void)onRTCJoinLineResult:(ARRtmpCode)code reason:(NSString *)reason;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | ARRtmpCode | 状态码
reason | NSString | 错误原因，RTC错误或者token错误（错误值自己平台定义）

**说明**

code为0时joinRTC服务成功，若code为其它值，可查看状态码。

#### 2. 游客申请连麦结果回调

**定义**

```
- (void)onRTCApplyLineResult:(ARRtmpCode)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | ARRtmpCode | 状态码

**说明**

此时应调用设置本地视频采集（setLocalVideoCapturer），code为0时意味着连麦成功，code为其它值，具体原因可查看状态码。

#### 3. 主播挂断游客连麦

**定义**

```
- (void)onRTCHangupLine;
```

**说明**

此时应移除本地连麦窗口，主播挂断别人的不会走该回调。

#### 4. 断开RTC服务连接

**定义**

```
- (void)onRTCLineLeave:(ARRtmpCode)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | ARRtmpCode | 状态码

**说明**

主播端关闭直播断开RTC服务后会回调此方法。自己加入RTC后，断网后在链接网络，会回调此方法。

#### 5. 其他游客视频连麦接通

**定义**

```
- (void)onRTCOpenRemoteVideoRender:(NSString *)peerId pubId:(NSString *)pubId userId:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | 连麦者标识Id（用于标识连麦用户，每次连麦随机生成）
pubId | NSString | 连麦者视频流Id(用于标识连麦者发布的流)
userId | NSString | 游客业务平台的用户Id
userData | NSString | 游客业务平台的相关信息（昵称，头像等)

**说明**

游客同样也在连麦中才会调用，此时应调用设置其它连麦者视频窗口(setRemoteVideoRender)方法用于显示连麦者图像。

#### 6. 其他游客视频连麦挂断

**定义**

```
- (void)onRTCCloseRemoteVideoRender:(NSString *)peerId pubId:(NSString *)pubId userId:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | 连麦者标识Id（用于标识连麦用户，每次连麦随机生成)
pubId | NSString | 连麦者视频流Id(用于标识连麦者发布的流)
userId | NSString | 游客业务平台的用户Id

**说明**

游客同样也在连麦中才会走该回调。不论是其他游客主动挂断连麦还是主播挂断游客连麦均会走该回调。只有在连麦中才会调用，此时应移除本地连麦者图像。

#### 7. 其他游客音频连麦接通

**定义**

```
- (void)onRTCOpenRemoteAudioLine:(NSString *)peerId userId:(NSString *)userId userData:(NSString *)userData;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | 连麦者标识Id（用于标识连麦用户，每次连麦随机生成）
userId | NSString | 游客业务平台的用户Id
userData | NSString | 游客业务平台的相关信息（昵称，头像等）

**说明**

仅在音频直播时有效。

#### 8. 其他游客连麦音频挂断

**定义**

```
- (void)onRTCCloseRemoteAudioLine:(NSString *)peerId userId:(NSString *)userId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | 连麦者标识Id（用于标识连麦用户，每次连麦随机生成）
userId | NSString | 连麦用户的第三方Id，请保持平台唯一

**说明**

仅在音频直播时有效。

#### 9. 其他人对音视频的操作

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

#### 10. 本地Player播放第一帧

**定义**

```
- (void)onRTCFirstPlayerVideoFrame:(CGSize)size;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 11. 本地视频第一帧

**定义**

```
- (void)onRTCFirstLocalVideoFrame:(CGSize)size;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 12. 远程视频第一帧

**定义**

```
- (void)onRTCFirstRemoteVideoFrame:(CGSize)size pubId:(NSString *)pubId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)

#### 13. 本地Player窗口大小的回调

**定义**

```
- (void)onRTCPlayerViewChanged:(CGSize)size;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 14. 本地窗口大小的回调

**定义**

```
- (void)onRTCLocalVideoViewChanged:(CGSize)size;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小

#### 15. 远程窗口大小的回调

**定义**

```
- (void)onRTCRemoteVideoViewChanged:(CGSize)size pubId:(NSString *)pubId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
size | CGSize | 视频窗口大小
pubId | NSString | RTC服务生成流的Id (用于标识与会者发布的流)

#### 16. 本地音频检测回调

**定义**

```
- (void)onRTCLocalAudioActive:(int)level showTime:(int)time;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
level | int | 音频大小（0~100）
time | int | 音频检测在time毫秒内不会再回调该方法（单位：毫秒）

**说明**

本地关闭音频后（setLocalAudioEnable为NO）,该回调将不再回调。

#### 17. 主持人音频检测

**定义**

```
- (void)onRTCHosterAudioActive:(NSString *)userId audioLevel:(int)level showTime:(int)time;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | NSString | 主持人的用户Id
level | int | 音频大小（0~100）
time | int | 音频检测在time毫秒内不会再回调该方法（单位：毫秒）

#### 18. 其他与会者音频检测回调

**定义**

```
- (void)onRTCRemoteAudioActive:(NSString *)peerId userId:(NSString *)userId audioLevel:(int)level showTime:(int)time;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | NSString | RTC服务生成的与会者标识Id（用于标识与会者用户，每次随机生成）
userId | NSString | 用户Id
level | int | 音频大小（0~100）
time | int | 音频检测在time毫秒内不会再回调该方法（单位：毫秒）

#### 19. 收到消息回调

**定义**

```
- (void)onRTCUserMessage:(ARMessageType)type userId:(NSString *)userId userName:(NSString *)userName userHeader:(NSString *)headerUrl content:(NSString *)content;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
type | ARMessageType | 消息类型，ARNomalMessageType普通消息，ARBarrageMessageType弹幕消息
userId | NSString | 发送消息者在自己平台下的Id
userName | NSString | 发送消息者的昵称
headerUrl | NSString | 发送者的头像
content | NSString | 消息内容

#### 20. 直播间实时在线人数变化通知

**定义**

```
- (void)onRTCMemberListNotify:(NSString *)serverId roomId:(NSString *)roomId allMember:(int)totalMember;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
serverId | NSString | 服务器Id
roomId | NSString | 房间Id
totalMember | int | 当前在线人数

**说明**

serverId和roomId参数用于请求人员列表。

#### 21. 直播开始

**定义**

```
- (void)onRTCLiveStart;
```

#### 22. 直播结束

**定义**

```
- (void)onRTCLiveStop;
```

#### 23. 获取视频的原始采集数据

**定义**

```
- (CVPixelBufferRef)onRTCCaptureVideoPixelBuffer:(CMSampleBufferRef)sampleBuffer;
```

**参数**

| 参数名       |       类型        | 描述     |
| ------------ | :---------------: | -------- |
| sampleBuffer | CMSampleBufferRef | 视频数据 |

**返回值**

视频对象（处理过或者没做处理）。

**说明**

获取视频原始数据，先设置摄像机类型为ARRtmpCameraTypeThreeFilter

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

**Version 3.0.1 （2019-05-23）**

* 游客端添加"获取视频的原始采集数据"的回调</br>

```
//获取视频的原始采集数据
- (CVPixelBufferRef)onRTCCaptureVideoPixelBuffer:(CMSampleBufferRef)sampleBuffer;
```
* 修复音频模式下不操作会锁屏的问题。

**Version 3.0.0 （2019-05-15）**

* SDK版本升级3.0，API接口变更

**Version 2.0.0 （2017-09-30）**

* SDK版本升级2.0，梳理、完善SDK
