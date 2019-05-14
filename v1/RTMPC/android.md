# Android

## 一、概述

#### 简介

互动连麦基于RTMP 基础上添加RTC实时互动的功能；推流断线重连，拉流秒开，最大支持4人同时互动连麦，观众人数不限，适用于游戏直播、美女秀场等场景

## 二、集成指南

#### 适用范围

本集成文档适用于Android ARRtmpc SDK 3.0.0版本

#### 准备环境

- Android Studio 2.1或以上版本
- Android 版本不低于 4.0.3 且支持音视频的 Android 设备（不支持模拟器）
- Android 设备已经连接到有效网络

#### 导入SDK

**Gradle方式导入（推荐）**

```
implementation 'org.anyrtc:rtmpc_hybrid:3.0.0'

```

**手动导入**

* 前往GitHub[下载Demo](https://github.com/anyRTC/anyRTC-RTMPC-Android)，找到**rtmpc_hybrid-release.aar**文件；

* 将rtmpc_hybrid-release.aar文件放入你项目的libs目录下，并在build文件中声明，如下图

![1.png](http://anyrtcboard.oss-cn-beijing.aliyuncs.com/document/20190128150859.png)


#### 权限说明

使用ARRtmpc SDK需以下权限

```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
#### 混淆配置
为了避免混淆SDK，在Proguard混淆文件中增加以下配置：

```
-dontwarn org.anyrtc.**
-keep class org.anyrtc.**{*;}
-dontwarn org.webrtc.**
-keep class org.webrtc.**{*;}
```

## 三、API接口文档

### ARRtmpcEngine 类

#### 1. 初始化并配置开发者信息

**定义**

```
void initEngine(Context context, String appId, String token)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
context | Context | 上下文对象
appId | String | appId
token | String  | token

**说明**

该方法为配置开发者信息，上述参数均可在https://www.anyrtc.io/ 应用管理中获得；建议在Application调用。

#### 2. 配置私有云

**定义**

```
void configServerForPriCloud(String address,int port)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
address | String | 私有云服务地址
port | int | 私有云服务端口

**说明**

配置私有云信息，当使用私有云时才需要进行配置，默认无需配置。

#### 3. 获取SDK版本号

**定义**

```
String getSdkVersion()
```
**返回值**

SDK版本号

#### 4. 关闭硬解码(安卓特有)

**定义**

```
void disableHWDecode()
```

#### 5. 关闭硬编码(安卓特有)

**定义**

```
void disableHWEncode()
```
#### 6. 设置日志显示级别

**定义**

```
void setLogLevel(ARLogLevel logLevel) 
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
logLevel | ARLogLevel | 日志显示级别
---
### ARRtmpcHosterOption主播端配置类

#### 1. 获取配置类

**定义**

```
ARRtmpcHosterOption arHostOption = ARRtmpcEngine.Inst().getHosterOption();
```
#### 2. 设置可配置参数

**定义**
```
void setOptionParams(boolean isDefaultFrontCamera, ARVideoCommon.ARVideoOrientation videoOrientation, ARVideoCommon.ARVideoProfile videoProfile, ARVideoCommon.ARMediaType mediaType, ARRtmpcLineLayoutTemplate lineLayoutTemplate)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
isDefaultFrontCamera | boolean | 是否默认前置摄像头 true 前置 false 后置  默认true
videoOrientation | ARVideoOrientation |视频方向 默认竖直
videoProfile | ARVideoProfile | 视频分辨率  默认360x640
mediaType | ARMediaType |发布媒体类型 Video音视频 Audio 音频 默认音视频
lineLayoutTemplate|ARRtmpcLineLayoutTemplate|连麦合成画面布局样式

**说明**  

可通过上面方法配置，也可单独设置

---

### ARRtmpcHosterKit主播类

#### 1. 实例化ARRtmpcHosterKit对象

**定义**

```
ARRtmpcHosterKit hostKit = new ARRtmpcHosterKit(ARRtmpcHosterEvent hosterEvent);

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
hosterEvent | ARRtmpcHosterEvent | 回调实现类

#### 2. 设置本地视频采集窗口

**定义**

```
int setLocalVideoCapturer(long renderPointer)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
render | long | 底层视频渲染对象


**返回值**

0/1/2：没有相机权限/打开相机成功/打开相机失败

#### 3. 开始推流

**定义**

```
void startPushRtmpStream(String pushUrl)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
pushUrl | String | 推流地址


**说明**  

传入推流地址开始推流

#### 4. 设置token验证

**定义**

```
boolean setUserToken(String userToken) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userToken | String | token字符串:客户端向自己服务器申请

**说明**

设置token验证必须放在joinRTCLine之前

#### 4. 加入RTC连接

**定义**

```
int joinRTCLine(String anyrtcId,  String userId, String userData)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
anyrtcId | String | 在开发者业务系统中保持唯一的Id（必填） 
userId | String | 主播在开发者自己平台的Id
userData | String | 播在开发者自己平台的相关信息（昵称，头像等）

**返回值**  

0：调用成功；4：参数非法

**说明**  

该方法须在开始推流（startRtmpPlay）方法后调用

#### 5. 同意游客连麦请求

**定义**

```
void acceptRTCLine(String peerId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的连麦者标识Id 。(用于标识连麦用户，每次连麦随机生成)） 


**说明**  

调用此方法即可同意游客的连麦请求。

#### 6.拒绝游客连麦请求

**定义**

```
void rejectRTCLine(String peerId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的连麦者标识Id 。(用于标识连麦用户，每次连麦随机生成)） 


**说明**  

调用此方法即可拒绝游客的连麦请求

#### 7.挂断游客连麦

**定义**

```
void hangupRTCLine(String peerId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的连麦者标识Id 。(用于标识连麦用户，每次连麦随机生成)） 


**说明**  

调用此方法即可挂断与游客的连麦

#### 8.停止推流

**定义**

```
void stopRtmpStream()
```
**说明**  

停止推流

#### 9.设置其他人视频窗口

**定义**

```
void setRTCRemoteVideoRender(String publishId,long render)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
publishId | String | RTC服务生成是视频通道Id 
 render|long|SDK底层视频显示对象 

**说明**  

该方法用于游客申请连麦接通后，游客视频连麦接通回调中（OnRTCOpenRemoteVideoRender）使用

#### 10. 发送消息

**定义**

```
boolean sendMessage(int type,String userName, String headUrl, String content)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
type|int|消息类型 0 普通消息 1 弹幕消息
userName | String | 用户昵称(最大256字节)，不能为空，否则发送失败；
headUrl | String | 用户头像(最大512字节)，可选；
content | String | 消息内容(最大1024字节)不能为空，否则发送失败；


**返回值**

true 发送成功 false 发送失败

#### 11. 关闭RTC连接

**定义**

```
void closeRTCLine()
```

**说明**

一般不调用。主播端如果调用此方法，将会关闭RTC服务，游客端将会收主播已离开onRTCLineLeave回调。

#### 12. 设置合成流连麦视频窗口位置

**定义**

```
void setVideoTemplate( ARRtmpcVideoHorizontal eHor, ARRtmpcVideoVertical eVer,ARRtmpcVideoDirection eDir, int ePadhor, int ePadver, int nWLineWidth)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
eHor|ARRtmpcVideoHorizontal|横向位置
eVer | ARRtmpcVideoVertical |竖向位置
eDir | ARRtmpcVideoDirection |排布方向
ePadhor | int |  横向的间距（左右间距：最左边或者最后边的视频离边框的距离）
ePadver|int|竖向的间距（上下间距：最上面或者最下面离边框的距离）
nWLineWidth|int|合成小视频白边宽度（上下间距：最上面或者最下面离边框的距离）

#### 13. 设置合成视频显示模板

**定义**

```
void setMixVideoModel(ARRtmpcLineLayoutTemplate layoutTemplate)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
layoutTemplate|ARRtmpcLineLayoutTemplate|布局样式

#### 14. 设置视频的默认背景图片

**定义**

```
int setVideoSubBackground(String filePath)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
filePath|String|图片的路径

**返回值**  

0/1/2:没有读取文件权限/打开设置成功/文件不存在

**说明** 

一定要打开读取权限，仅支持jpg和png的图片格式（仅支持640*640分辨率以内）

#### 15. 设置本地音频是否传输

**定义**

```
void setLocalAudioEnable(boolean enabled)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean| 打开或关闭本地音频传输

**说明**

true为传输音频，false为不传输音频，默认传输

#### 16. 设置本地视频是否传输

**定义**

```
void setLocalVideoEnable(boolean enabled)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean| 打开或关闭本地视频传输

**说明**

true为传输视频，false为不传输视频，默认视频传输


#### 17. 切换前后摄像头

**定义**

```
void switchCamera()

```

#### 18. 设置录像地址（地址为拉流地址）

**定义**

```
void setRtmpRecordUrl( String url)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
url | String| 设置Rtmp录制地址，需放在开始推流方法前.并且**必须在平台上开启录像服务**

**说明**

设置录像地址（地址为拉流地址）

#### 19. 设置前置摄像头镜像是否打开

**定义**

```
void setFrontCameraMirrorEnable(boolean enable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean| true 打开 false 关闭

**说明**

是否打开镜像模式，默认关闭

#### 20. 设置相机支持范围内的焦距

**定义**

```
void setCameraZoom( int distance)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
distance | int| 焦距

**说明**

设置相机支持范围内的焦距

#### 21. 获取相机最大焦距

**定义**

```
int getCameraMaxZoom()
```

**返回值**

最大焦距

#### 22. 获取相机当前焦距

**定义**

```
int getCameraZoom()
```

**返回值**

当前焦距

#### 22. 判断是否可变焦

**定义**

```
boolean isZoomSupported()
boolean isSmoothZoomSupported()
```

**返回值**

是否支持变焦/平滑变焦

**说明**  
在设置变焦前先用该方法判断是否支持变焦

#### 23. 打开关闭摄像头闪光灯

**定义**

```
void openCameraTorchMode(final boolean open)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
open | boolean | 是否开启闪光灯

#### 24. 设置音频检测

**定义**

```
void setAudioActiveCheck(boolean open)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
open | boolean | 是否开启音频检测

**说明**

默认音频检测打开


#### 25.设置视频横屏模式

**定义**

```
 void setScreenToLandscape()
```

#### 26.设置视频竖屏模式

**定义**

```
void setScreenToPortrait()
```

#### 27. 设置左侧logo水印

**定义**

```
int setVideoLogo( String path,  int x,  int y)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
path | String | 水印图片文件路径
x | int | 距左上角X轴距离
y | int | 距左上角Y轴距离

**说明**

仅支持jpg图片，注意安卓动态权限处理


#### 28. 设置右侧logo水印

**定义**

```
int setVideoTopRightLogo( String path,  int x,  int y)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
path | String | 水印图片文件路径
x | int | 距右上角X轴距离
y | int | 距右上角Y轴距离

**说明**

仅支持jpg图片，注意安卓动态权限处理

#### 29.销毁主播端

**定义**

```
void clear() 
```
### ARRtmpcHosterEvent主播回调类

#### 1. RTMP服务连接成功

**定义**

```
void onRtmpStreamOk()
```

#### 2. RTMP 服务重连

**定义**

```
void onRtmpStreamReconnecting(int times)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
times | int| 重连次数

#### 3. RTMP推流状态

**定义**

```
void onRtmpStreamStatus(int delayTime, int netBand)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
times | int| 推流的延迟时间(单位：ms)
netBand | int| 当前的上行的带宽（单位：byte）

#### 4. RTMP服务连接失败

**定义**

```
void onRtmpStreamFailed(int code)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int| 状态码

#### 4. RTMP服务关闭

**定义**

```
 void onRtmpStreamClosed()
```
#### 5. 创建RTC服务结果

**定义**

```
void onRTCCreateLineResult(int code, String reason);
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int| 状态码
reason | String| 原因

**说明**  

code==0 时，连接服务成功
code为其他值时均为失败，具体可查看code对应说明

#### 6. 主播收到游客连麦请求

**定义**

```
void onRTCApplyToLine(String peerId, String userId, String userData)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String| 连麦者标识id（用于标识连麦用户，每次连麦随机生成）
userId | String| 游客在自己业务平台的UserId
userData | String| 游客加入RTC连接的自定义参数体（可查看游客端加入RTC连接方法） 


#### 7. 游客取消连麦申请

**定义**

```
void onRTCCancelLine(int code, String peerId)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int| 状态码
peerId | String| 连麦者标识id（用于标识连麦用户，每次连麦随机生成）


#### 8. RTC服务关闭

**定义**

```
void onRTCLineClosed(int code,String reason)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int| 状态码
reason | String| 说明

#### 9. 连麦者视频即将显示回调

**定义**

```
void onRTCOpenRemoteVideoRender(String peerId, String publishId, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | 连麦者标识id（用于标识连麦用户，每次连麦随机生成)  
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的相关信息（昵称，头像等）

**说明**  

主播与游客的连麦接通后视频将要显示时回调此方法

#### 10. 连麦者视频关闭

**定义**

```
void onRTCCloseRemoteVideoRender(String peerId, String publishId, String userId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | 连麦者标识id（用于标识连麦用户，每次连麦随机生成)  
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id

**说明**  

当与连麦的人断开连麦时会回调此方法

#### 11. （语音连麦）连麦接通后

**定义**

```
void onRTCOpenRemoteAudioLine(String peerId, String publishId, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | 连麦者标识id（用于标识连麦用户，每次连麦随机生成)  
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的相关信息（昵称，头像等）

**说明**  

**语音模式下**，主播与游客的连麦接通后回调此方法

#### 12. （语音连麦）连麦断开后

**定义**

```
void onRTCCloseRemoteAudioLine(String peerId, String publishId, String userId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | 连麦者标识id（用于标识连麦用户，每次连麦随机生成)  
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id

**说明**  

**语音连麦模式下**，当与连麦的人断开连麦时会回调此方法

#### 13. 连麦的人音视频状态回调

**定义**

```
void onRTCRemoteAVStatus(String peerId, boolean audio, boolean video);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId |String | 连麦者标识id（用于标识连麦用户，每次连麦随机生成)  
audio | boolean | true 音频打开 false 音频关闭
video | boolean | true 视频打开 false 视频关闭

#### 14. 本地RTC音频检测

**定义**

```
void onRTLocalAudioActive(int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
time | int | 音频检测在time毫秒内不会再回调该方法（单位：毫秒）


#### 15. 远程（连麦的人）RTC音频检测

**定义**

```
void onRTCRemoteAudioActive(String peerId, int level, int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId |String | 连麦者标识id（用于标识连麦用户，每次连麦随机生成)  
time | int | 音频检测在time毫秒内不会再回调该方法（单位：毫秒）

#### 15. 收到消息

**定义**

```
void onRTCUserMessage(int type,String userId, String userName, String headUrl, String message)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
type|int|消息类型 0 普通消息 1 弹幕消息
userId |String | 发送消息者在自己平台下的Id
userName | String |发送消息者的昵称
headUrl | String | 发送者的头像
message | String | 消息内容

**说明**

收到其他人发送的消息，（该参数来源均为发送消息时所带参数）

#### 16. 实时在线人数变化通知

**定义**

```
void onRTCMemberNotify(String serverId, String roomId, int totalMember)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
serverId |String | 服务器地址，用于请求人员列表的参数
roomId | String |房间Id,用于请求人员列表的参数
totalMember | int | 当前在线人数 

**说明**  

serverAddress和roomId参数用于请求人员列表

---

### ARRtmpcGuestOption游客端配置类

#### 1. 获取配置类

**定义**

```
ARRtmpcGuestOption arGuestOption = ARRtmpcEngine.Inst().getGuestOption();
```
#### 2. 设置可配置参数

**定义**
```
void setOptionParams(boolean isDefaultFrontCamera, ARVideoCommon.ARVideoOrientation videoOrientation, ARVideoCommon.ARMediaType mediaType)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
isDefaultFrontCamera | boolean | 是否默认前置摄像头 true 前置 false 后置  默认true
videoOrientation | ARVideoOrientation |视频方向 默认竖直
mediaType | ARMediaType |发布媒体类型 Video音视频 Audio 音频 默认音视频

**说明**  

可通过上面方法配置，也可单独设置

---

### ARRtmpcGuestKit游客类

#### 1. 实例化ARRtmpcGuestKit对象

**定义**

```
ARRtmpcGuestKit guestKit = new ARRtmpcGuestKit(ARRtmpcGuestEvent guestEvent);

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
guestEvent | ARRtmpcGuestEvent | 回调实现类

#### 2. 开始播放RTMP流

**定义**
```
 void startRtmpPlay( String pullUrl,  long render)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
pullUrl | String | 拉流地址
render | long |SDK底层视频显示对象

#### 3. 加入RTC连接

**定义**
```
int joinRTCLine(String anyRTCId,String userId, String userData)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
anyRTCId | String | 主播对应的anyRTCId 
userId | String |游客业务平台的用户id
userData | String |游客业务平台自定义数据

**返回值**  

0：调用成功；4：参数非法；

**说明**  

此方法需在startRtmpPlay()之后调用

#### 4. 设置本地视频采集窗口

**定义**

```
int setLocalVideoCapturer(long renderPointer)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
render | long | 底层视频渲染对象


**返回值**

0/1/2：没有相机权限/打开相机成功/打开相机失败

#### 5.设置其他人视频窗口

**定义**

```
void setRTCRemoteVideoRender(String publishId,long render)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
publishId | String | RTC服务生成视频通道ID 
 render|long|SDK底层视频显示对象 

**说明**  

该方法用于（OnRTCOpenRemoteVideoRender）回调中使用

#### 6.申请连麦

**定义**

```
int applyRTCLine()
```
**返回值**  

0/1：失败（没有录音权限）/成功


#### 7.挂断连麦


**定义**

```
void hangupRTCLine()
```

#### 8.关闭RTC连接

**定义**

```
void leaveRTCLine()
```

**说明**  

用于关闭RTC服务，将无法进行聊天互动，人员上下线等

#### 9.设置视频横屏模式

**定义**

```
 void setScreenToLandscape()
```

#### 10.设置视频竖屏模式

**定义**

```
void setScreenToPortrait()
```

#### 11. 发送消息

**定义**

```
boolean sendMessage(int type,String userName, String headUrl, String content)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
type|int|消息类型 0 普通消息 1 弹幕消息
userName | String | 用户昵称(最大256字节)，不能为空，否则发送失败；
headUrl | String | 用户头像(最大512字节)，可选；
content | String | 消息内容(最大1024字节)不能为空，否则发送失败；


**返回值**

true 发送成功 false 发送失败


#### 12. 设置本地音频是否传输

**定义**

```
void setLocalAudioEnable(boolean enabled)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean| 打开或关闭本地音频传输

**说明**

true为传输音频，false为不传输音频，默认传输

#### 13. 设置本地视频是否传输

**定义**

```
void setLocalVideoEnable(boolean enabled)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean| 打开或关闭本地视频传输

**说明**

true为传输视频，false为不传输视频，默认视频传输


#### 14. 切换前后摄像头

**定义**

```
void switchCamera()

```
#### 15. 设置音频检测

**定义**

```
void setAudioActiveCheck(boolean open)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
open | boolean | 是否开启音频检测

**说明**

默认音频检测打开

#### 16. 设置前置摄像头镜像是否打开

**定义**

```
void setFrontCameraMirrorEnable(boolean enable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean| true 打开 false 关闭

**说明**

是否打开镜像模式，默认关闭

#### 17. 销毁游客端

**定义**

```
void clear()
```
---

### ARRtmpcGuestEvent主播回调类

#### 1. RTMP连接成功

**定义**

```
void onRtmpPlayerOk()
```

#### 2. RTMP开始播放

**定义**

```
void onRtmpPlayerStart()
```

#### 3. RTMP当前播放状态

**定义**

```
void onRtmpPlayerStatus(int cacheTime, int bitrate)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
cacheTime | int|缓存时间(单位：ms)
bitrate | int|当前码率大小(单位：byte)

**说明**

在主播处于直播状态时，将会一直回调此方法

#### 4. RTMP播放缓冲进度

**定义**

```
void onRtmpPlayerLoading(int percent)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
percent| int|缓存百分比，0-100

**说明**

弱网下rtmp播放出现卡顿时，当前缓冲进度。nPercent为0时，页面可以进行缓冲提示。当为100时，缓冲提示去掉

#### 5.RTMP播放器关闭

**定义**

```
void onRtmpPlayerClosed(int code)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
code| int|状态码

**说明**

主播停止推流将会回调此方法

#### 6.RTC服务连接结果

**定义**

```
void onRTCJoinLineResult(int code, String reason)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code| int|状态码
reason| String|说明

**说明**

code==0时，连接服务成功
code为其他值时均为失败，具体可查看code对应说明

#### 7.申请连麦结果

**定义**

```
void onRTCApplyLineResult(int code)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
code| int|状态码

**说明**

code==0时连麦成功
code为其他值时均为失败，具体可查看code对应说明

#### 8.主播挂断游客连麦

**定义**

```
void onRTCHangupLine()
```

**说明**  

主播挂断游客的连麦。
视频直播中此时应移除本地连麦小窗口图像


#### 9.断开RTC服务连接

**定义**

```
void onRTCLineLeave(int code,String reason)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
code| int|状态码


#### 10. 其他人视频即将显示回调

**定义**

```
void onRTCOpenRemoteVideoRender(String peerId, String publishId, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | 连麦者标识id（用于标识连麦用户，每次连麦随机生成)  
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的相关信息（昵称，头像等）

**说明**  

主播与游客的连麦接通后视频将要显示时回调此方法

#### 10. 其他连麦者视频关闭

**定义**

```
void onRTCCloseRemoteVideoRender(String peerId, String publishId, String userId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | 连麦者标识id（用于标识连麦用户，每次连麦随机生成)  
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id

**说明**  

当与连麦的人断开连麦时会回调此方法

#### 11. 其他连麦者（语音连麦）连麦接通后

**定义**

```
void onRTCOpenRemoteAudioLine(String peerId, String publishId, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | 连麦者标识id（用于标识连麦用户，每次连麦随机生成)  
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的相关信息（昵称，头像等）

**说明**  

**语音模式下**，主播与游客的连麦接通后回调此方法

#### 12. 其他连麦者（语音连麦）连麦断开后

**定义**

```
void onRTCCloseRemoteAudioLine(String peerId, String publishId, String userId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | 连麦者标识id（用于标识连麦用户，每次连麦随机生成)  
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id

**说明**  

**语音连麦模式下**，当与连麦的人断开连麦时会回调此方法

#### 13. 连麦的人音视频状态回调

**定义**

```
void onRTCRemoteAVStatus(String peerId, boolean audio, boolean video);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId |String | 连麦者标识id（用于标识连麦用户，每次连麦随机生成)  
audio | boolean | true 音频打开 false 音频关闭
video | boolean | true 视频打开 false 视频关闭


#### 14. 本地RTC音频检测

**定义**

```
void onRTLocalAudioActive(int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
time | int | 音频检测在time毫秒内不会再回调该方法（单位：毫秒）


#### 15. 远程（连麦的人）RTC音频检测

**定义**

```
void onRTCRemoteAudioActive(String peerId, int level, int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId |String | 连麦者标识id（用于标识连麦用户，每次连麦随机生成)  
time | int | 音频检测在time毫秒内不会再回调该方法（单位：毫秒）

#### 15. 收到消息

**定义**

```
void onRTCUserMessage(int type,String userId, String userName, String headUrl, String message)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
type|int|消息类型 0 普通消息 1 弹幕消息
userId |String | 发送消息者在自己平台下的Id
userName | String |发送消息者的昵称
headUrl | String | 发送者的头像
message | String | 消息内容

**说明**

收到其他人发送的消息，（该参数来源均为发送消息时所带参数）

#### 16. 实时在线人数变化通知

**定义**

```
void onRTCMemberNotify(String serverId, String roomId, int totalMember)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
serverId |String | 服务器地址，用于请求人员列表的参数
roomId | String |房间Id,用于请求人员列表的参数
totalMember | int | 当前在线人数 

**说明**  

serverAddress和roomId参数用于请求人员列表

---

## 四、更新日志


## 五、错误码对照表
