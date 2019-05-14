# Android

## 一、概述

#### 简介

#### Demo 体验

请根据需求选择渠道安装，安装完对讲调度Demo后，可体验多人实时对讲，智能调度等功能。

- [iOS Demo下载](https://www.pgyer.com/SYA3)

- [Android Demo下载](https://www.pgyer.com/LfFY)

- [Web Demo 体验](https://www.anyrtc.io/demo/dispatch)

#### 源码 GitHub

源码仅供开发者参考，适用于SDK调试，便于快速集成。

- [iOS Demo 源码下载](https://github.com/anyRTC/AR-Talk-iOS)

- [Android Demo 源码下载](https://github.com/anyRTC/AR-Talk-Android)


## 二、集成指南

#### 适用范围

本集成文档适用于 Android ARRtmax SDK 3.0.0 版本。

#### 准备环境

- Android Studio 2.1 或以上版本
- Android 版本不低于 4.0.3 且支持音视频的 Android 设备（不支持模拟器）
- Android 设备已经连接到有效网络

#### 导入SDK

**Gradle方式导入（推荐）**

```
implementation 'org.anyrtc:rtmax_kit:3.0.0'

```

**手动导入**

* 前往GitHub [下载 Demo](https://github.com/anyRTC/AR-Talk-Android)，找到**rtmax_kit-release.aar**文件；

* 将rtmax_kit-release.aar文件放入你项目的libs目录下，并在build文件中声明，如下图

![1.png](http://anyrtcboard.oss-cn-beijing.aliyuncs.com/document/20190128150850.png)


#### 权限说明

使用ARRtmax SDK需以下权限

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

### ARMaxEngine 类

#### 1. 初始化并配置开发者信息

**定义**

```
void initEngine(Context context, boolean useJaveRecord，String appId, String token)
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

### ARMaxOption 配置类

#### 1. 获取配置类

**定义**

```
ARMaxOption option = ARMaxEngine.Inst().getArMaxOption()
```

#### 2. 设置可配置参数

**定义**
```
void setOptionParams(boolean isDefaultFrontCamera, ARVideoCommon.ARVideoOrientation mScreenOriention, ARVideoCommon.ARVideoProfile videoProfile) 
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
isDefaultFrontCamera | boolean | 是否默认前置摄像头 true 前置 false 后置  默认true
videoOrientation | ARVideoOrientation |视频方向 默认竖直
videoProfile | ARVideoProfile | 视频分辨率  默认360x640

**说明**  

可通过上面方法配置，也可单独设置

---

### ARMaxKit 类

#### 1.实例化ARMaxKit对象

**定义** 

```
ARMaxKit arMaxKit = new ARMaxKit(ARMaxEvent maxEvent)

```

#### 2. 加入对讲组

**定义**

```
int joinTalkGroup(String groupId,  String userId,  String userData)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
groupId | String | 对讲组id（同一个anyrtc平台的appid内保持唯一性）
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的相关信息（昵称，头像等），可选


**返回值**

0：成功


#### 3. 切换对讲组

**定义**

```
int switchTalkGroup(String groupId,  String userData)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
groupId | String | 对讲组id（同一个anyrtc平台的appid内保持唯一性）
userData | String | 开发者自己平台的相关信息（昵称，头像等），可选


**返回值**

0：成功


#### 4. 申请对讲

**定义**

```
int applyTalk( int priority) 
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
nPriority | int | 优先级

**说明**  

优先级：申请抢麦用户的级别（0权限最大（数值越大，权限越小）；除0外，可以后台设置0-10之间的抢麦权限大小）


**返回值**

0: 调用OK  -1:未登录  -2:正在对讲中  -3: 资源还在释放中 -4: 操作太过频繁


#### 5. 取消对讲

**定义**

```
void cancelTalk()
```
**说明**   

申请对讲成功（onRTCApplyTalkOk）之后方可结束对讲 


#### 6. 发起呼叫

**定义**

```
int makeCall( String userId,  int type,  String userData)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 被叫用户userid
type | int | 0:视频 1:音频
userData | String |自定义数据

**返回值**

0:调用OK;-1:未登录;-2:没有通话-3:视频资源占用中;-5:本操作不支持自己对自己;-6:会话未创建（没有被呼叫用户）


#### 7. 呼叫邀请

**定义**

```
int inviteCall( String userId,  String userData)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 被邀请用户userid
userData | String |自定义数据

**返回值**

0:调用OK;-1:未登录;-2:没有通话-3:视频资源占用中;-5:本操作不支持自己对自己;-6:会话未创建（没有被呼叫用户）


#### 8. 主叫端结束某一路正在进行的通话

**定义**

```
void endCall( String userId)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 指定用户userid

#### 9. 接受呼叫或通话邀请

**定义**

```
int acceptCall( String callId)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
callId | String | 呼叫请求时收到的callId


#### 10.拒绝呼叫

**定义**

```
void rejectCall()
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
callId | String | 呼叫请求时收到的callId

#### 11.退出当前通话

**定义**

```
void leaveCall()
```


#### 12.发起视频监看（或者收到视频上报请求时查看视频）

**定义**

```
int monitorVideo( String userId,  String userData) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 被监看用户userId
userData | String |自定义数据

**返回值**

0: 调用OK  -1:未登录	-5:本操作不支持自己对自己


#### 13.同意视频监看

**定义**

```
int acceptVideoMonitor( String hostId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
hostId | String | 发起人的userid

**返回值**

0: 调用OK  -1:未登录 -3:视频资源占用中 -5:本操作不支持自己对自己

#### 14.拒绝视频监看

**定义**

```
 int rejectVideoMonitor( String hostId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
hostId | String | 发起人的userid

**返回值**

0: 调用OK  -1:未登录 -3:视频资源占用中 -5:本操作不支持自己对自己

#### 15.监看发起者关闭视频监看

**定义**

```
void closeVideoMonitor( String userId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 被监看用户的userid

#### 16.视频上报

**定义**

```
int reportVideo( String userId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 要向谁上报的用户userid 

**说明**  

接收端收到上报请求时，调用monitorVideo进行视频查看


#### 17.上报者关闭视频上报

**定义**

```
void closeReportVideo()
```


#### 18. 发送消息

**定义**

```
int sendMessage( String userId,  String head,  String strContent)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userName | String | 用户昵称(最大256字节)，不能为空，否则发送失败；
head | String | 用户头像(最大512字节)，可选；
content | String | 消息内容(最大1024字节)不能为空，否则发送失败；


**返回值**

0 成功


#### 19. 设置视频竖屏

**定义**

```
void setScreenToPortrait()
```

#### 20. 设置视频横屏

**定义**

```
void setScreenToLandscape()
```

#### 21. 设置本地音频是否传输

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

#### 22. 设置本地视频是否传输

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

#### 23. 设置本地视频采集窗口

**定义**

```
int setLocalVideoCapturer(long render)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
render | long | 底层视频渲染对象


**返回值**

0/1/2：没有相机权限/打开相机成功/打开相机失败


#### 24. 停止本地视频采集

**定义**

```
 void closeLocalVideoCapture()
```

#### 25.设置其他人视频窗口

**定义**

```
void setRTCRemoteVideoRender(String publishId,long render)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
publishId | String | RTC服务生成的视频标识Id 
 render|long|SDK底层视频显示对象 

**说明**  

该方法用于视频呼叫接通后，回调（OnRTCOpenRemoteVideoRender）使用

#### 26. 切换前后摄像头

**定义**

```
void switchCamera()
```



#### 27. 关闭P2P通话

**定义**

```
void closeP2PTalk()
```
**说明**   

在控制台强插对讲后，关闭和控制台之间的P2P通话


#### 28. 设置录音文件的路径

**定义**

```
int setRecordPath( String callPath,  String talkPath,  String talkP2PPath)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
callPath | String | 呼叫文件的保存路径（文件夹路径）
talkPath | String | 对讲文件保存路径（文件夹路径）
talkP2PPath | String | 强插P2P文件保存路径（文件夹路径）

**返回值**

0/1:设置成功/文件夹不存在


#### 29. 设置本地前置摄像头镜像是否打开

**定义**

```
void setFrontCameraMirrorEnable(boolean bEnable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean | true为打开，alse为关闭 



#### 30. 设置视频网络状态是否打开

**定义**

```
void setNetworkStatus(boolean enable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean | true打开，false关闭

**说明**

默认视频网络状态关闭

#### 31. 获取当前视频网络状态是否打开

**定义**

```
boolean networkStatusEnabled() 
```

**返回值**

视频网络状态检测打开与否

#### 32.是否打开回音消除

**定义**

```
void setForceAecEnable( boolean enable)
```

**返回值**

需在加入对讲组之前设置

#### 32. 设置token验证

**定义**

```
boolean setUserToken(String userToken) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userToken | String | token字符串:客户端向自己服务器申请

**说明**

设置token验证必须放在joinGroup之前


#### 33. 不接收某路视频

**定义**

```
 void muteRemoteVideoStream(String publishId,boolean mute)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
mute | boolean | true禁止，false接收
publishId | String | RTC服务生成的通道Id 


#### 34. 不接收某路音频

**定义**

```
void muteRemoteAudioStream(String publishId,  boolean mute)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
mute | boolean | true禁止，false接收
publishId | String | 流ID

#### 35. 设置远程（其他人）音视频状态

**定义**

```
void setRemoteAVEnable( String peerId,  boolean audioEnable,  boolean videoEnable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | 用户的ID
audioEnable | boolean | true 打开 false 关闭
videoEnable | boolean | true 打开 false 关闭

#### 36. 打开关闭摄像头闪光灯

**定义**

```
void openCameraTorchMode( boolean open)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
open | boolean | 是否开启闪光灯

#### 37. 是否打开音频检测

**定义**

```
void setAudioActiveCheck( boolean audioOnly, final boolean audioDetect)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
audioOnly | boolean |  true:仅音频模式; false: 音视频模式
audioDetect | boolean |  true:打开; false: 关闭

#### 38. 退出对讲组

**定义**

```
void leaveTalkGroup()
```

#### 39. 销毁初始化对象

**定义**

```
void clear()
```
---

### ARMaxEvent 回调类

#### 1. 加入对讲组成功

**定义**

```
void onRTCJoinTalkGroupOK(String groupId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
groupId | String | 群组Id  

#### 2. 加入对讲组失败

**定义**

```
void onRTCJoinTalkGroupFailed(String groupId, int code, String reason)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
groupId | String | 群组Id  
code | int | 错误码
reason | String | 错误原因

#### 3. 离开对讲组

**定义**

```
void onRTCLeaveTalkGroup(int code)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int | 错误码

**说明**  

0：正常退出；100：网络错误，与服务器断开连接；207：强制退出

#### 4. 申请对讲成功

**定义**

```
void onRTCApplyTalkOk()
```

#### 5. 语音通道建立成功

**定义**

```
void onRTCTalkCouldSpeak()
```
**说明**  

申请对讲成功后，语音通道建立成功回调，可以开始讲话

#### 6. 其他人正在对讲组中讲话

**定义**

```
void onRTCTalkOn(String userId, String userData)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid
userData | String | 用户的自定义数据


#### 7. 当用户处于对讲状态时，控制台强制发起P2P通话

**定义**

```
void onRTCTalkP2POn(String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid
userData | String | 用户的自定义数据

#### 8. 与控制台的P2P讲话结束

**定义**

```
void onRTCTalkP2POff(String userData)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userData | String | 用户的自定义数据

#### 9.结束对讲回调

**定义**

```
void onRTCTalkClosed(int code, String userId, String userData)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int | 错误码
userId | String | 用户的userid
userData | String | 用户的自定义数据

#### 10.视频监看请求

**定义**

```
 void onRTCVideoMonitorRequest(String userId, String userData)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid
userData | String | 用户的自定义数据

**说明**  

有用户向本机用户发起视频监看请求

#### 11.视频监看关闭

**定义**

```
 void onRTCVideoMonitorClose(String userId, String userData)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid
userData | String | 用户的自定义数据

#### 12.视频监看请求结果

**定义**

```
void onRTCVideoMonitorResult(String userId, int code, String userData)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int | 错误码
userId | String | 用户的userid
userData | String | 用户的自定义数据

#### 13.收到视频上报请求

**定义**

```
 void onRTCVideoReportRequest(String userId, String userData)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid
userData | String | 用户的自定义数据

#### 14.视频上报关闭

**定义**

```
 void onRTCVideoReportClose(String userId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid

#### 15.主叫方发起通话成功

**定义**

```
void onRTCMakeCallOK(String callId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
callId | String | 呼叫成功时服务器生成的CallId

#### 16.主叫方收到被叫方同意通话

**定义**

```
void onRTCAcceptCall(String userId, String userData)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid
userData | String | 用户的自定义数据

#### 17.主叫方收到被叫方拒绝通话

**定义**

```
void onRTCRejectCall(String userId, int code, String userData)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid
userData | String | 用户的自定义数据


#### 18.被叫方挂断或者被邀请方离开通话

**定义**

```
void onRTCLeaveCall(String userId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid

#### 19.通话释放

**定义**

```
void onRTCLeaveCall(String userId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid

**说明**  

主叫方收到通话结束的回调（被叫方和被邀请方已全部退出或者主叫方挂断所有参与者）


#### 20.被叫方收到通话请求

**定义**

```
void onRTCMakeCall(String callId, int nCallType, String userId, String userData)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
callId | String | 呼叫成功时服务器生成的CallId   
nCallType | int | 呼叫类型（0：视频；1：音频）
userId | String | 用户的userid
userData | String | 用户的自定义数据


#### 21.被叫方收到主叫方挂断通话

**定义**

```
void onRTCEndCall(String callId, String userId, int code)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
callId | String | 呼叫成功时服务器生成的CallId   
userId | String | 用户的userid
code | int | 状态码


#### 22.视频呼叫接通视频窗口打开

**定义**

```
void onRTCOpenRemoteVideoRender(String peerId, String publishId, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的相关信息（昵称，头像等）

**说明**  

其他通话参与者进入视频通话的回调，开发者需调用设置其他与会者视频窗口（setRemoteVideoRender）方法。

#### 23. 视频窗口关闭

**定义**

```
void onRTCCloseRemoteVideoRender(String peerId, String publishId, String userId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id 
publishId | String | RTC服务生成的视频通道Id
userId | String | 开发者自己平台的用户Id

**说明**  

其他对讲者离开的回调，开发者需移除该抢麦者视频窗口，仅视频呼叫时有用


#### 24. 音频模式下呼叫打开音频通道回调

**定义**

```
void onRtcOpenRemoteAudioTrack(String peerId, String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识用户，每次加入对讲组随机生成)
userId | String | 开发者自己平台的用户Id
userData | String | 开发者自己平台的相关信息（昵称，头像等）

**说明**  

**音频模式**下接通 呼叫后回调

#### 25. 音频模式下呼叫关闭音频通道回调

**定义**

```
void onRtcCloseRemoteAudioTrack(String peerId, String userId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String |RTC服务生成的标识Id (用于标识用户，每次加入对讲组随机生成)
userId | String | 开发者自己平台的用户Id

**说明**  

**音频模式**关闭呼叫回调


#### 26. 其他人对音视频的操作通知

**定义**

```
void onRTCRemoteAVStatus(String peerId, boolean audio, boolean video);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识用户，每次加入对讲组随机生成)
audio | boolean | 音频状态 true 打开 false 关闭
video | boolean | 视频状态 true 打开 false 关闭

**说明**  

与会者关闭/开启了音视频

#### 27. 其他人对自己音视频的操作通知

**定义**

```
void onRTCLocalAVStatus(boolean audio, boolean video);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
audio | boolean | 音频状态 true 打开 false 关闭
video | boolean | 视频状态 true 打开 false 关闭

**说明**  

别人对自己音视频的操作

#### 28. 音频监测（其他人声音大小回调）

**定义**

```
void onRTCRemoteAudioActive(String peerId, String userId, int level, int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的标识Id (用于标识用户，每次加入对讲组随机生成)
userId | String | 开发者自己平台的用户Id
level | int | 音量大小
time | int | time时间内不会再回调该方法(毫秒)

#### 29. 音频监测（本地声音大小回调）

**定义**

```
void onRTLocalAudioActive(int level, int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
level | int | 音量大小
time | int | time时间内不会再回调该方法(毫秒)

#### 30. 本地网络状态

**定义**

```
onRTCLocalNetworkStatus(int netSpeed, int packetLost, ARNetQuality netQuality);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
nNetSpeed | int |网络上行
nPacketLost | int | 丢包率(1~100)
netQuality | ARNetQuality | 网络质量

#### 31. 远程（其他人）网络状态

**定义**

```
onRTCRemoteNetworkStatus(String rtcpId, int netSpeed, int packetLost, ARNetQuality netQuality);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId |String | 通道Id
nNetSpeed | int |网络上行
nPacketLost | int | 丢包率(1~100)
netQuality | ARNetQuality | 网络质量

#### 32. 当前对讲组在线人数

**定义**

```
void onRTCMemberNum(int num)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
num |int | 人数


#### 33. 录像地址回调信息

**定义**

```
 void onRTCGotRecordFile(int nRecType, String userData, String filePath)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
nRecType |int | 录音的类型（0/1/2/3：对讲本地录音/对讲远端录音/强插P2P录音/语音通话呼叫录音）
userData |String | 对讲录音所有者的自定义数据 
filePath |String | 录音文件保存路径  

**说明**  

设置录音路径后录音保存路径的回调接口

## 四、更新日志


## 五、错误码对照表