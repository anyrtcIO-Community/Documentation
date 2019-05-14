# Android

## 一、概述

#### 简介

P2P音视频可以实现一对一单聊，内置推送服务，保证必达；支持视频、语音、优先视频等多种呼叫模式，适用于网络电话、社交、企业通信等场景。

## 二、集成指南

#### 适用范围

本集成文档适用于Android ARP2P SDK 3.0.0版本。

#### 准备环境

- Android Studio 2.1或以上版本
- Android 版本不低于 4.0.3 且支持音视频的 Android 设备（不支持模拟器）
- Android 设备已经连接到有效网络

#### 导入SDK

**Gradle方式导入（推荐）**

```
implementation 'org.anyrtc:rtp2pcall_kit:3.0.0'

```

**手动导入**

* 前往GitHub[下载Demo](https://github.com/anyRTC/anyRTC-P2P-Android)，找到**rtp2pcall_kit-release.aar**文件；

* 将rtp2pcall_kit-release.aar文件放入你项目的libs目录下，并在build文件中声明，如下图

![1.png](http://anyrtcboard.oss-cn-beijing.aliyuncs.com/document/20190128150907.png)


#### 权限说明

使用ARP2P SDK需以下权限

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
---
## 三、API接口文档

### ARP2PEngine 类

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

### ARP2POption 配置类

#### 1. 获取配置类

**定义**

```
ARP2POption option = ARP2PEngine.Inst().getP2POption()
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

### ARP2PKit 类

#### 1. 实例化ARP2PKit对象

**定义**

```
ARP2PKit p2pKit = new ARP2PKit(ARP2PEvent p2pEvent)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
p2pEvent | ARP2PEvent | 回调实现类

**说明**

也有无参构造方法，在实例化之后必须设置调用
**setP2PEvent(ARP2PEvent p2pEvent)**  方法设置回调

#### 2. 设置token验证

**定义**

```
boolean setUserToken(String userToken) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userToken | String | token字符串:客户端向自己服务器申请

**说明**

设置token验证必须放P2P上线之前

#### 3. P2P上线

**定义**

```
boolean turnOn(String userId) 
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid（必填）

**返回值**

ture/false 上线成功/上线失败

#### 4. P2P下线

**定义**

```
void turnOff()
```
#### 5. 发起P2P呼叫

**定义**

```
int makeCall(String userId, ARP2PCallMode callMode, String userData)
```


**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid（必填）
callMode | ARP2PCallMode | 呼叫模式 
userData | String | 用户自定义数据

**返回值**

0/1:呼叫失败（没有RECORD_AUDIO权限）/呼叫成功

#### 6. 挂断P2P呼叫

**定义**

```
void endCall( String userId)
```


**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid（必填）

#### 7. 接受P2P呼叫申请

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

#### 8. 拒绝P2P呼叫申请

**定义**

```
void rejectCall( String userId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid（必填）

#### 9. 设置本地视频采集窗口

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

#### 10.设置其他人视频窗口

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

#### 11. 发送消息

**定义**

```
void sendMessage(String userId,  String content)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户id，不能为空，否则发送失败；
content | String | 消息内容(最大1024字节)不能为空，否则发送失败；

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

#### 15. 抓拍对方图片

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

#### 16. 开始录像

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

#### 17. 停止录像

**定义**

```
void stopRecordVideo()

```

#### 18. 停止本地摄像头采集

**定义**

```
void stopCapturer()

```

#### 19. 切换至音频模式

**定义**

```
void swtichToAudioMode()

```

#### 20. 销毁P2PKit对象

**定义**

```
void clear()

```

---

### ARP2PEvent 回调类

#### 1. P2P服务连接成功

**定义**

```
void onConnected()

```

#### 2. P2P服务连接成功

**定义**

```
void onDisconnect(int code)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int| 响应码


#### 3. 收到P2P呼叫

**定义**

```
void onRTCMakeCall(String userId, ARP2PCallMode callMode, String userData)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 呼叫的用户ID 
callMode | ARP2PCallMode| 呼叫模式
userData | String| 呼叫用户自定义数据

#### 4. 对方接受P2P呼叫

**定义**

```
void onRTCAcceptCall(String userId)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 被呼叫用户的id 

#### 4. 对方拒绝P2P呼叫

**定义**

```
void onRTCRejectCall(String userId, int code)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 被呼叫用户的id 
code | int| 状态码 

#### 5. 对方挂断P2P呼叫

**定义**

```
 void onRTCEndCall(String userId, int code)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 被呼叫用户的id 
code | int| 状态码 

#### 6. 对方切换到音频通话模式

**定义**

```
void onRTCSwithToAudioMode()

```

#### 7. 收到消息

**定义**

```
void onRTCUserMessage(String userId, String message)

```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 对方Id 
message | String| 消息内容

#### 7. 收到消息

**定义**

```
void onRTCUserMessage(String userId, String message)

```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 对方Id 
message | String| 消息内容

#### 8. 对方视频接通视频即将显示回调

**定义**

```
void onRTCOpenRemoteVideoRender(String publishId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---

publishId | String | RTC服务生成的视频通道Id


**说明**  

开发者需调用设置其他与会者视频窗口（setRemoteVideoRender）方法。

#### 9. 对方视频挂断视频即将关闭回调

**定义**

```
void onRTCCloseRemoteVideoRender(String publishId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
publishId | String | RTC服务生成的视频通道Id

**说明**  

主叫与被叫的P2P挂断后将会回调此方法。此时应将视频窗口移除

## 四、更新日志



## 五、错误码对照表
