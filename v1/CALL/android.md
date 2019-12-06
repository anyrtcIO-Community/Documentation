## 一、概述

### 简介

### Demo体验

请根据需求选择渠道安装，安装完ARCall呼叫 Demo后，可体验点对点音视频呼叫，监看等功能。

- [iOS Demo下载](https://itunes.apple.com/cn/app/anyrtc点对点/id1316858730?mt=8)

- [Android Demo下载](https://www.pgyer.com/3blO)

### 源码GitHub

源码仅供开发者参考，适用于SDK调试，便于快速集成。

- [iOS Demo 源码下载](https://github.com/anyRTC/AR-Call-iOS)

- [Android Demo 源码下载](https://github.com/anyRTC/AR-Call-Android)


## 二、集成指南

### 适用范围

本集成文档适用于Android ARCall SDK 3.0.0+版本。

### 准备环境

- Android Studio 2.1或以上版本
- Android 版本不低于 4.0.3 且支持音视频的 Android 设备（不支持模拟器）
- Android 设备已经连接到有效网络

### 导入SDK

**Gradle方式导入**[ ![Download](https://api.bintray.com/packages/dyncanyrtc/ar_dev/call/images/download.svg) ](https://bintray.com/dyncanyrtc/ar_dev/call/_latestVersion)

添加Jcenter仓库 Gradle依赖：

```
dependencies {
   compile 'org.ar:arcall_kit:3.0.8'(最新版见上面图标版本号)
}
```

或者 Maven
```
<dependency>
  <groupId>org.ar</groupId>
  <artifactId>arcall_kit</artifactId>
  <version>3.0.8</version>
  <type>pom</type>
</dependency>
```


---

## 三、开发指南

集成SDK后，还需对SDK进行初始化操作，建议在Application中完成。

#### 1.1 初始化SDK并配置开发者信息

调用 initEngine() 方法配置开发者信息，开发者信息可在anyRTC管理后台中获得，详见[创建anyRTC账号](https://docs.anyrtc.io)


**示例代码：**

```
public class ARApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        ARCallEngine.Inst().initEngine(getApplicationContext(),  "AppId",  "AppToken");
    }
}

```

> 自定义的Application需在AndroidManifest.xml注册 

#### 1.2 获取会议配置类并设置相关配置

**示例代码：**

``` 
//获取ARCall配置类
ARCallOption option=ARCallEngine.Inst().getARCallOption();
option.setVideoProfile(ARVideoCommon.ARVideoProfile.ARVideoProfile480x640);
option.setVideoFps(ARVideoCommon.ARVideoFrameRate.ARVideoFrameRateFps20);

```
#### 1.3 实例化Call对象并设置回调

**示例代码：**

``` 
ARCallKit arCallKit = new ARCallKit();
arCallKit.setArCallEvent(ARCallEvent arCallEvent);
```

> 建议写成单例模式，具体参照demo

#### 1.4 上线

**示例代码：**

```
 //上线
if (arCallKit.isTurnOff()) {
    arCallKit.turnOn("userId","userData");
}

```
> 上线成功会回调onConnected()，发布失败会回调onDisconnect()，**程序启动只需调用一次即可**

#### 1.5 实例化视频显示View

**示例代码：**

``` 
ARVideoView videoView = new ARVideoView(rl_video,  ARCallEngine.Inst().Egl(),this,false);

videoView.setVideoViewLayout(true,Gravity.CENTER, LinearLayout.VERTICAL);

```
> ARVideoView 对象是显示视频，调整视频窗口摆放位置的类，可由开发者自定义，具体可参照Demo


#### 1.6 打开本地摄像头采集

**示例代码：**

```
mMeetKit.setLocalVideoCapturer(videoView.openLocalVideoRender().GetRenderPointer());

```
> 注意安卓动态权限处理，这里需要录音和摄像头权限



#### 1.7 呼叫其他人

**示例代码：**

```
//主动呼叫
arCallKit.makeCallUser(String callId,ARUserOption option);

```
> 返回值为1时呼叫成功，被呼叫人会收到onRTCMakeCall()回调


#### 1.8 取消呼叫他人/挂断

**示例代码：**

```
arCallKit.endCall(callId);
//如果打开了本地摄像头，还应调用
arCallKit.stopCapturer();

```
> 对方将收到onRTCEndCall()回调

#### 1.9 拒绝他人呼叫

**示例代码：**

```
arCallKit.rejectCall(callId);

```
> 对方将收到onRTCRejectCall()回调


#### 2.0 接受他人呼叫

**示例代码：**

```
arCallKit.accpetCall(callId);

```
> 接受呼叫后，双方呼叫通道开启，将回调onRTCOpenRemoteVideoRender()方法，在该回调中设置显示对方视频，参考 2.1

#### 2.1 呼叫接通后对方视频即将显示

**示例代码：**

```
//显示对方视频
final VideoRenderer render = mVideoView.openRemoteVideoRender(strVidRenderId);
if (null != render) {
    arCallKit.setRemoteVideoRender(strVidRenderId, render.GetRenderPointer());
}

```
> 对方同意通话后会回调onRTCOpenRemoteVideoRender()方法，在该回调中应显示对方视频，参照上述代码，具体可查看demo

#### 2.2  呼叫挂断后对方视频即将关闭

**示例代码：**

```
//移除对方视频
arVideoView.removeRemoteRender(strVidRenderId);
arCallKit.setRTCRemoteVideoRender(strVidRenderId,0);

```
> 对方挂断通话后会回调onRTCCloseRemoteVideoRender()方法，在该回调中应移除对方视频，参照上述代码，具体可查看demo

#### 2.3  下线

**示例代码：**

```
//下线
arCallKit.turnOff();

```
> 下线后将收不到任何呼叫

#### 2.4 释放ARCall对象

**示例代码：**

```
 arCallKit.clear();

```
> 如果不希望程序在退出后，还受到呼叫请求，则彻底释放对象并下线


#### 2.5 权限说明

使用ARCall SDK需以下权限

```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
####  2.6 混淆配置
在Proguard混淆文件中增加以下配置：

```
-dontwarn org.ar.**
-keep class org.ar.**{*;}
-dontwarn org.webrtc.**
-keep class org.webrtc.**{*;}
```

---
## 四 、API接口文档

### ARCallEngine 类

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

### ARCallOption 配置类

### 1. 获取配置类

**定义**

```
ARCallOption option = ARCallEngine.Inst().getARCallOption()
```

### 2. 设置可配置参数

**定义**
```
void setOptionParams(boolean isDefaultFrontCamera, ARVideoCommon.ARVideoOrientation mScreenOriention, ARVideoCommon.ARVideoProfile videoProfile, ARVideoCommon.ARVideoFrameRate videoFps) 
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
isDefaultFrontCamera | boolean | 是否默认前置摄像头 true 前置 false 后置  默认true
videoOrientation | ARVideoOrientation |视频方向 默认竖直
videoProfile | ARVideoProfile | 视频分辨率  默认360x640
videoFps | ARVideoFrameRate | 视频帧率  默认15帧

**说明**  

可通过上面方法配置，也可单独设置


---

### ARCallKit 类

### 1. 实例化ARCallKit对象

**定义**

```
ARCallKit arCallKit = new ARCallKit(ARCallEvent arCallEvent)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
arCallEvent | ARCallEvent | 回调实现类

**说明**

也有无参构造方法，在实例化之后必须设置调用
**setArCallEvent(ARCallEvent arCallEvent)**  方法设置回调

### 2. 设置token验证

**定义**

```
boolean setUserToken(String userToken) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userToken | String | token字符串:客户端向自己服务器申请

**说明**

设置token验证必须放上线之前

### 3. 上线

**定义**

```
boolean turnOn(String userId,Stirng userData) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid（必填）
userData|String|用户自定义数据 

**返回值**

ture/false 上线成功/上线失败

### 4. 下线

**定义**

```
void turnOff()
```
### 5. 发起P2P呼叫

**定义**

```
int makeCallUser(String userId, ARUserOption option )
```


**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid（必填）
option | ARUserOption | 呼叫个人配置类

**返回值**

-5/-4/-3/-2/-1/0/1：userId为空字符串/正在通话中，呼叫失败/MemberList为空/不能自己呼叫自己/操作频繁/呼叫失败（没有RECORD_AUDIO权限）/呼叫成功

### 6. 发起群组呼叫

**定义**

```
int makeCallGroup(String groupId, ARGroupOption groupOption)
```


**参数**

参数名 | 类型 | 描述
---|:---:|---
groupId | String | 群组呼叫的会议id（必填）
groupOption | ARGroupOption | 呼叫群组配置类

**返回值**

-5/-4/-3/-2/-1/0/1：userId为空字符串/正在通话中，呼叫失败/MemberList为空/不能自己呼叫自己/操作频繁/呼叫失败（没有RECORD_AUDIO权限）/呼叫成功

### 7. 发起坐席呼叫

**定义**

```
int makeCallQueue(String queueId, ARQueueOption arQueueOption)
```


**参数**

参数名 | 类型 | 描述
---|:---:|---
queueId | String | 呼叫坐席的频道id（必填）
arQueueOption | ARQueueOption | 呼叫坐席配置类

**返回值**

-5/-4/-3/-2/-1/0/1：userId为空字符串/正在通话中，呼叫失败/MemberList为空/不能自己呼叫自己/操作频繁/呼叫失败（没有RECORD_AUDIO权限）/呼叫成功

**说明**

ARQueueOption类参数说明

参数名 | 类型 | 描述
---|:---:|---
callMode | ARCallMode | 呼叫模式（该模式呼叫只能用call_cit_audio , call_cit_video 两种）
level | int | 优先级：0等级最大，值越大，等级越小
area | String | 服务地区
business | String | 服务范围

### 8. 邀请用户参与呼叫

**定义**

```
int inviteCall(String userId)
```


**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid（必填）

**返回值**

0/1：失败（没有RECORD_AUDIO权限）/成功

**说明**

用于群组呼叫中


### 9. 挂断呼叫

**定义**

```
void endCall( String userId)
```


**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid（必填）

### 10. 接受呼叫申请

**定义**

```
int accpetCall(String userId) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid（必填）

**返回值**

0/1:失败（没有RECORD_AUDIO权限）/成功

### 11. 拒绝呼叫申请

**定义**

```
void rejectCall( String userId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid（必填）

### 12. 群组呼叫设置Zoom模式

**定义**

```
void setZoomMode( ARMeetZoomMode mode)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
mode | ARMeetZoomMode | Zoom下的几个模式（必填）

### 13. Zoom模式下设置当前视频页数

**定义**

```
void setZoomPage( int nPages)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
nPages | int | 当前视频分页页数（必填，从0开始）

### 14. Zoom模式下设置当前页数id及显示个数

**定义**

```
void setZoomPageIdx( int nIdx, int showNum)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
nIdx | int | 当前视频分页页数（必填，从0开始）
showNum | int | 当前页显示视频个数（必填）

### 15. 呼叫手机号

**定义**

```
void switchToPstn()
```

### 16. 呼叫分机号

**定义**

```
void switchToExtension()
```


### 17. 设置本地视频采集窗口

**定义**

```
int setLocalVideoCapturer(long render)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
render | long | 底层视频渲染对象

**返回值**

0/1/2/3：没有相机权限/打开成功/打开相机失败/相机已打开， 未释放

### 18. 重启本地视频采集窗口

**定义**

```
int restartLocalVideoCapturer(long render)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
render | long | 底层视频渲染对象

**返回值**

0/1/2/3：没有相机权限/打开成功/打开相机失败/相机已打开， 未释放

### 19. 设置其他人视频窗口

**定义**

```
void setRTCRemoteVideoRender(String peerId,long render)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成的用户标识Id 
 render|long|SDK底层视频显示对象 

**说明**  

该方法用于视频呼叫接通后，回调（OnRTCOpenRemoteVideoRender）使用

### 20. 发送消息

**定义**

```
void sendMessage(String userId,  String content)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户id，不能为空，否则发送失败；
content | String | 消息内容(最大1024字节)不能为空，否则发送失败；

### 21. 设置本地音频是否传输

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

### 22. 设置本地视频是否传输

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


### 23. 切换前后摄像头

**定义**

```
void switchCamera()

```

### 24. 抓拍对方图片

**定义**

```
int snapPicture(String fileName)

```

**参数**

参数名 | 类型 | 描述
---|:---:|---
fileName | String| 文件名

**说明**

抓拍视频一帧作为图片，需注意安卓动态权限处理

### 25. 开始录制对方视频

**定义**

```
int startRecordVideo( String fileName) 

```

**参数**

参数名 | 类型 | 描述
---|:---:|---
fileName | String| 文件名

**说明**  

录取一段视频，需注意安卓动态权限处理

### 25. 停止录制对方视频

**定义**

```
void stopRecordVideo()

```

### 27. 停止本地摄像头采集

**定义**

```
void stopCapturer()

```

### 28. 切换至音频模式

**定义**

```
void swtichToAudioMode()

```

### 29. 是否开启云端录像

**定义**

```
 void setCloudRecord( boolean enable)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean| 是否开启

### 30. 设置客服是否可用

**定义**

```
void setAvalible( boolean enable)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean| 是否可用

### 31. 设置客服频道ID，服务区域

**定义**

```
void setAsClerk( String channelId,  ARClertOption arClertOption)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
channelId | String| 频道ID
arClertOption|ARClertOption|坐席配置类

**说明**

ARClertOption类参数说明

参数名 | 类型 | 描述
---|:---:|---
level | int | 优先级：0等级最大，值越大，等级越小
area | String | 服务地区
business | String | 服务范围

### 32. 销毁ARCallKit对象

**定义**

```
void clear()

```

---

### ARCallEvent 回调类

### 1. 服务连接成功

**定义**

```
void onConnected()

```

### 2. 服务连接断开

**定义**

```
void onDisconnect(int code)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int| 响应码

### 3. 加入房间成功

**定义**

```
void onRTCJoinRoomOk(String meetId)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
meetId | String| 房间号

**说明**  

只有打开VIP或呼叫类型是AR_Call_Meet_Invite的时候才有回调，roomId参数仅供录像时使用。

### 4. 收到呼叫

**定义**

```
void onRTCMakeCall( String userId, ARCallMode callMode, String userData, String extend)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 呼叫的用户ID 
callMode | ARCallMode| 呼叫模式
userData | String| 呼叫用户自定义数据
extend | String| 呼叫自定义数据

### 5. 对方接受呼叫

**定义**

```
void onRTCAcceptCall(String userId)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 被呼叫用户的id 

### 6. 对方拒绝呼叫

**定义**

```
void onRTCRejectCall(String userId, int code)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 被呼叫用户的id 
code | int| 状态码 

### 7. 对方挂断呼叫

**定义**

```
 void onRTCEndCall(String userId, int code)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 被呼叫用户的id 
code | int| 状态码 

### 8. 是否支持sip呼叫

**定义**

```
 void onRTCSipSupport(boolean bPstn, boolean bExtension, boolean bNull)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
bPstn | Boolean| 是否支持手机呼叫 
bExtension | Boolean| 是否支持座机分机呼叫 
bNull | Boolean| 拓展使用，暂时无用 

### 9. 对方切换到音频通话模式

**定义**

```
void onRTCSwithToAudioMode()

```

### 10. 收到消息

**定义**

```
void onRTCUserMessage(String userId, String message)

```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 对方Id 
message | String| 消息内容

### 11. 对方视频接通视频即将显示回调

**定义**

```
void onRTCOpenRemoteVideoRender(String userId, String vidRenderId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | RTC服务生成的用户标识Id
vidRenderId | String | RTC服务生成的视频通道Id
userData | String | 用户的自定义数据


**说明**  

开发者需调用设置其他与会者视频窗口（setRemoteVideoRender）方法。

### 12. 对方视频挂断视频即将关闭回调

**定义**

```
void onRTCCloseRemoteVideoRender(String userId, String vidRenderId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | RTC服务生成的用户标识Id
vidRenderId | String | RTC服务生成的视频通道Id

**说明**  

主叫与被叫的挂断后将会回调此方法。此时应将视频窗口移除

### 13. 对方音频通道打开回调

**定义**

```
void onRTCOpenRemoteAudioTrack(String userId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | RTC服务生成的用户标识Id
userData | String | 用户的自定义数据

**说明**  

对方同意后，音频通道建立成功回调此方法。

### 14. 对方音频通道关闭回调

**定义**

```
void onRTCCloseRemoteAudioTrack(String userId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | RTC服务生成的用户标识Id

**说明**  

主叫与被叫的挂断后音频通道断开，将会回调此方法。

### 15. 远端音量实时监测回调

**定义**

```
void onRTCRemoteAudioActive(String userId, int level, int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | RTC服务生成的用户标识Id
level | int | 声音音量
time | int | 监测时间监测（单位：毫秒）

**说明**  

打开音频实时监测的情况下，回调该方法

### 16. 本地音量实时监测回调

**定义**

```
void onRTCLocalAudioActive(int level, int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
level | int | 声音音量
time | int | 监测时间监测（单位：毫秒）

**说明**  

打开音频实时监测的情况下，回调该方法

### 17. 远端网络监测回调

**定义**

```
void onRTCRemoteNetworkStatus(String userId, int netSpeed, int packetLost, ARNetQuality netQuality);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | RTC服务生成的用户标识Id
netSpeed | int | 当前PeerUserId的网络带宽
packetLost | int | 当前PeerUserId的丢包率
netQuality | ARNetQuality | 当前PeerUserId的丢包率

**说明**  

打开实时网络监测开关后，回调此方法。

### 18. 本地网络监测回调

**定义**

```
void onRTCLocalNetworkStatus(int netSpeed, int packetLost, ARNetQuality netQuality);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
netSpeed | int | 当前PeerUserId的网络带宽
packetLost | int | 当前PeerUserId的丢包率
netQuality | ARNetQuality | 当前PeerUserId的丢包率

**说明**  

打开实时网络监测开关后，回调此方法。

### 19. Zoom模式状态回调

**定义**

```
void onRTCZoomPageInfo(ARMeetZoomMode zoomMode, int allPages, int curPage, int allRender, int scrnBeginIdx, int num);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
zoomMode | ARMeetZoomMode | 当前的Zoom模式
allPages | int | Zoom模式视频的分页总页数
curPage | int | Zoom模式视频的当前页数
allRender | int | Zoom模式视频的当前页的视频总数
scrnBeginIdx | int | Zoom模式视频的当前页的视频个数索引
num | int | Zoom模式视频的总数目

**说明**  

主叫与被叫的挂断后将会回调此方法。此时应将视频窗口移除

### 20. Zoom模式下用户进去群组呼叫回调

**定义**

```
void onRTCUserCome(String userId, String vidRenderId, String userData);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户标识Id
vidRenderId | String | 	RTC服务生成的视频通道Id
userData | String | 用户的自定义数据

**说明**  

群组呼叫模式下， 用户进入群组呼叫回调此方法。

### 21. Zoom模式下用户离开群组呼叫回调

**定义**

```
void onRTCUserOut(String userId, String vidRenderId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户标识Id
vidRenderId | String | 	RTC服务生成的视频通道Id

**说明**  

群组呼叫模式下，用户的离开时回调此方法。

### 22. 当前排队人数

**定义**

```
void onRTCUserCTIStatus(int nQueueNum);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
nQueueNum | int |当前排队人数

**说明**  

坐席呼叫模式下，回调此方法。

### 23. 坐席状态

**定义**

```
void onRTCClerkCTIStatus(int nQueueNum，int nAllClerk, int nWorkingClerk);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
nQueueNum | int | 排队人数
nAllClerk|int|总坐席数
nWorkingClerk|int|正在工作中座席数

**说明**  

坐席呼叫模式下，回调此方法。。

### 24. 音视频状态回调

**定义**

```
void onRTCAVStatus(String peerId, boolean bAudio, boolean bVideo);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
peerId | String | RTC服务生成，用来标识用户的Id
bAudio|boolean|音频打开或关闭
bVideo|boolean|视频打开或关闭



## 四、更新日志

- v 3.1.0
1.   增加onRTCJoinRoomOk回调 
1.   去除onRTCMakeCall回调中第一个MeetId参数 
1.   turnOn方法增加userData参数   
1.   ARUserOption等配置类中去除userData参数

- 全新发布,替代老的P2P SDK，功能升级，全面优化

## 五、错误码对照表

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

