## 一、概述

### 简介

### Demo体验

请根据需求选择渠道安装，安装完P2P Demo后，可体验点对点音视频呼叫，监看等功能。

- [iOS Demo下载](https://itunes.apple.com/cn/app/anyrtc点对点/id1316858730?mt=8)

- [Android Demo下载](https://www.pgyer.com/3blO)

### 源码GitHub

源码仅供开发者参考，适用于SDK调试，便于快速集成。

- [iOS Demo 源码下载](https://github.com/anyRTC/anyRTC-P2P-iOS)

- [Android Demo 源码下载](https://github.com/anyRTC/anyRTC-P2P-Android)


## 二、集成指南

### 适用范围

本集成文档适用于Android ARP2P SDK 3.0.0版本。

### 准备环境

- Android Studio 2.1或以上版本
- Android 版本不低于 4.0.3 且支持音视频的 Android 设备（不支持模拟器）
- Android 设备已经连接到有效网络

### 导入SDK

**Gradle方式导入**[ ![Download](https://api.bintray.com/packages/dyncanyrtc/ar_dev/p2p/images/download.svg) ](https://bintray.com/dyncanyrtc/ar_dev/p2p/_latestVersion)

添加Jcenter仓库 Gradle依赖：

```
dependencies {
   compile 'org.ar:rtp2pcall_kit:3.0.2'
}
```

或者 Maven
```
<dependency>
  <groupId>org.ar</groupId>
  <artifactId>rtp2pcall_kit</artifactId>
  <version>3.0.1</version>
  <type>pom</type>
</dependency>
```


### 权限说明

使用ARP2P SDK需以下权限

```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
### 混淆配置
为了避免混淆SDK，在Proguard混淆文件中增加以下配置：

```
-dontwarn org.anyrtc.**
-keep class org.anyrtc.**{*;}
-dontwarn org.ar.**
-keep class org.ar.**{*;}
-dontwarn org.webrtc.**
-keep class org.webrtc.**{*;}
```
---
## 三、API接口文档

### ARP2PEngine 类

### 1. 初始化并配置开发者信息

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

### 2. 配置私有云

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

### 3. 获取SDK版本号

**定义**

```
String getSdkVersion()
```
**返回值**

SDK版本号

### 4. 关闭硬解码(安卓特有)

**定义**

```
void disableHWDecode()
```

### 5. 关闭硬编码(安卓特有)

**定义**

```
void disableHWEncode()
```


### 6. 设置日志显示级别

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

### 1. 获取配置类

**定义**

```
ARP2POption option = ARP2PEngine.Inst().getP2POption()
```

### 2. 设置可配置参数

**定义**
```
void setOptionParams(boolean isDefaultFrontCamera, ARVideoCommon.ARVideoOrientation mScreenOriention, ARVideoCommon.ARVideoProfile videoProfile,ARVideoCommon.ARVideoFrameRate videoFps) 
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
isDefaultFrontCamera | boolean | 是否默认前置摄像头 true 前置 false 后置  默认true
videoOrientation | ARVideoOrientation |视频方向 默认竖直
videoProfile | ARVideoProfile | 视频分辨率  默认360x640
videoFps | ARVideoFrameRate |视频帧率  默认 Fps15

**说明**  

可通过上面方法配置，也可单独设置


---

### ARP2PKit 类

### 1. 实例化ARP2PKit对象

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

设置token验证必须放P2P上线之前

### 3. P2P上线

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

### 4. P2P下线

**定义**

```
void turnOff()
```
### 5. 发起P2P呼叫

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

### 6. 挂断P2P呼叫

**定义**

```
void endCall( String userId)
```


**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid（必填）

### 7. 接受P2P呼叫申请

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

### 8. 拒绝P2P呼叫申请

**定义**

```
void rejectCall( String userId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户的userid（必填）

### 9. 设置本地视频采集窗口

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

### 10.设置其他人视频窗口

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

### 11. 发送消息

**定义**

```
void sendMessage(String userId,  String content)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户id，不能为空，否则发送失败；
content | String | 消息内容(最大1024字节)不能为空，否则发送失败；

### 12. 设置本地音频是否传输

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

### 13. 设置本地视频是否传输

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


### 14. 切换前后摄像头

**定义**

```
void switchCamera()

```

### 15. 抓拍对方图片

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

### 16. 开始录像

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

### 17. 停止录像

**定义**

```
void stopRecordVideo()

```

### 18. 停止本地摄像头采集

**定义**

```
void stopCapturer()

```

### 19. 切换至音频模式

**定义**

```
void swtichToAudioMode()

```

### 20. 销毁P2PKit对象

**定义**

```
void clear()

```

---

### ARP2PEvent 回调类

### 1. P2P服务连接成功

**定义**

```
void onConnected()

```

### 2. P2P服务连接成功

**定义**

```
void onDisconnect(int code)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int| 响应码


### 3. 收到P2P呼叫

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

### 4. 对方接受P2P呼叫

**定义**

```
void onRTCAcceptCall(String userId)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 被呼叫用户的id 

### 4. 对方拒绝P2P呼叫

**定义**

```
void onRTCRejectCall(String userId, int code)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 被呼叫用户的id 
code | int| 状态码 

### 5. 对方挂断P2P呼叫

**定义**

```
 void onRTCEndCall(String userId, int code)

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 被呼叫用户的id 
code | int| 状态码 

### 6. 对方切换到音频通话模式

**定义**

```
void onRTCSwithToAudioMode()

```

### 7. 收到消息

**定义**

```
void onRTCUserMessage(String userId, String message)

```

**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String| 对方Id 
message | String| 消息内容


### 8. 对方视频接通视频即将显示回调

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

### 9. 对方视频挂断视频即将关闭回调

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

**Version 3.0.0 （2019-05-15）**

* SDK版本升级3.0，API接口变更

**Version 2.0.0 （2017-09-30）**

* SDK版本升级2.0，梳理、完善SDK

## 五、错误码

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