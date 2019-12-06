

## 一、集成指南


### 准备环境

- Android Studio 2.1或以上版本
- Android 版本不低于 4.0.3 且支持音视频的 Android 设备（不支持模拟器）
- Android 设备已经连接到有效网络

### 导入SDK

**Gradle方式导入**[ ![Download](https://api.bintray.com/packages/dyncanyrtc/ar_dev/rtcp/images/download.svg) ](https://bintray.com/dyncanyrtc/ar_dev/rtcp/_latestVersion)


添加Jcenter仓库 Gradle依赖：

```
dependencies {
  compile 'org.ar:rtcp_kit:3.0.8'(最新版见上面图标版本号)
}
```

或者 Maven
```
<dependency>
  <groupId>org.ar</groupId>
  <artifactId>rtcp_kit</artifactId>
  <version>3.0.8</version>
  <type>pom</type>
</dependency>
```



## 二、开发指南

集成SDK后，还需对SDK进行初始化操作，建议在Application中完成。

#### 1.1 初始化SDK并配置开发者信息

调用 initEngine() 方法配置开发者信息，开发者信息可在anyRTC管理后台中获得，详见[创建anyRTC账号](https://docs.anyrtc.io)


**示例代码：**

```
public class ARApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        ARRtcpEngine.Inst().initEngine(getApplicationContext(),  "AppId",  "AppToken");
    }
}

```

> 自定义的Application需在AndroidManifest.xml注册 

#### 1.2 设置发布视频流配置信息

**示例代码：**

```
//获取配置类
ARRtcpOption anyRTCRTCPOption = ARRtcpEngine.Inst().getARRtcpOption();
//设置前后置摄像头 视频横竖屏 视频分辨率 帧率等
anyRTCRTCPOption.setOptionParams(true, ARVideoCommon.ARVideoOrientation.Portrait,ARVideoCommon.ARVideoProfile.ARVideoProfile480x640, ARVideoCommon.ARVideoFrameRate.ARVideoFrameRateFps15);

```

#### 1.3 实例化实时直播对象并设置回调

**示例代码：**

``` 
ARRtcpKit mRtcpKit = new ARRtcpKit(String userId, String userData);

mRtcpKit.setRtcpEvent(ARRtcpEvent rtcpEvent)

```

#### 1.4 实例化视频显示View

**示例代码：**

``` 
ARVideoView videoView = new ARVideoView(rl_video,  ARRtcpEngine.Inst().Egl(),this,false);

videoView.setVideoViewLayout(true,Gravity.CENTER, LinearLayout.VERTICAL);

```
> ARVideoView 对象是显示视频，调整视频窗口摆放位置的类，可由开发者自定义，具体可参照Demo

#### 1.5 打开本地摄像头采集

**示例代码：**

```
rtcpKit.setLocalVideoCapturer(videoView.openLocalVideoRender().GetRenderPointer());

```
> 注意安卓动态权限处理，这里需要录音和摄像头权限

#### 1.6 发布媒体流

**示例代码：**

```
//发布视频流
rtcpKit.publishByToken("", ARVideoCommon.ARMediaType.Video);

```
> 该方法第二个参数为媒体流类型，这以发布视频流为例，发布视频流需先打开本地摄像头采集。发布成功会回调onPublishOK()，发布失败会回调onPublishFailed() 

#### 1.7 订阅媒体流

**示例代码：**

```
//订阅流
  rtcpKit.subscribe("rtcpId","token");

```
> 订阅成功会回调onSubscribeOK()，订阅失败会回调onSubscribeFailed()，SDK支持订阅多路流 

#### 1.8 显示订阅媒体流

**示例代码：**

```
//显示流
long renderPointer = videoView.subscribeRemoteVideo(rtcpId).GetRenderPointer();
rtcpKit.setRemoteVideoRender(rtcpId, renderPointer);

```
> 如果发布的是视频流，订阅成功会回调onSubscribeOK()， 随即回调onRTCOpenRemoteVideoRender() ,在该回调方法中调用上面示例代码，显示对方发布的流，具体参照demo。

> 如果发布的是音频流，订阅成功会回调onSubscribeOK()，
随即回调onRTCOpenRemoteAudioTrack()

#### 1.9 取消发布流

**示例代码：**
```
//取消发布
rtcpKit.unPublish();
//停止采集
rtcpKit.stopCapture();
//移除本地视频预览图像
videoView.removeLocalVideoRender();
```
> 取消发布媒体流后，订阅方会收到onRTCCloseRemoteVideoRender()回调，在该回调中，应移除对方视频，具体参照demo


#### 2.0 取消订阅

**示例代码：**
```
//取消订阅
rtcpKit.unSubscribe();
```
> 在离开之前，应将订阅的流全部取消订阅


#### 2.1 释放RTCP对象

**示例代码：**
```
//取消订阅
rtcpKit.clean();
```
> 应在直播页面结束时调用，单例写法时，在程序退出时调用即可。


#### 2.2 权限说明

使用RTCP SDK需以下权限

```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
#### 2.3 混淆配置
在Proguard混淆文件中增加以下配置：

```
-dontwarn org.anyrtc.**
-keep class org.anyrtc.**{*;}
-dontwarn org.ar.**
-keep class org.ar.**{*;}
-dontwarn org.webrtc.**
-keep class org.webrtc.**{*;}
```

## 三、API接口文档

### ARRtcpEngine 类

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
**说明**  

关闭硬解码,默认打开。

### 5. 关闭硬编码(安卓特有)

**定义**

```
void disableHWEncode()
```
**说明**  

关闭硬编码,默认关闭。

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

### ARRtcpOption配置类

### 1. 获取配置类

**定义**

```
ARRtcpOption rtcpOption = ARRtcpEngine.Inst().getARRtcpOption();
```
### 2. 设置可配置参数

**定义**
```
void setOptionParams(boolean isDefaultFrontCamera, ARVideoCommon.ARVideoOrientation videoOrientation, ARVideoCommon.ARVideoProfile videoProfile, ARVideoCommon.ARVideoFrameRate videoFps) 

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
isDefaultFrontCamera | boolean | 是否默认前置摄像头 true 前置 false 后置  默认true
videoOrientation | ARVideoOrientation |视频方向 默认竖直
videoProfile | ARVideoProfile | 视频分辨率  默认360x640
videoFps | ARVideoFrameRate |视频帧率  默认 Fps15

### ARRtcpKit 类

### 1. 实例化ARRtcpKit对象

**定义**

```
ARRtcpKit rtcpKit = new ARRtcpKit(String userId,String userData);

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
userId | String | 用户ID，确保自己平台唯一，不能为空
userData | String |用户自定义信息


### 2. 设置ARRtcpKit接口回调

**定义**

```
void setRtcpEvent(ARRtcpEvent rtcpEvent)

```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpEvent | ARRtcpEvent | RTCP相关回调实现类


### 3. 设置本地视频采集窗口

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


### 4. 设置显示其他人的视频窗口

**定义**

```
void setRemoteVideoRender(String rtcpId, long render)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | RTCP服务生成的通道Id (用于标识通道，每次发布随机生成)
render | long |  底层视频渲染对象

**说明**

该方法用于订阅成功通后，视频即将显示的回调中（onRTCOpenVideoRender）使用


### 5. 设置本地音频是否传输

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

### 6. 设置本地视频是否传输

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

### 7. 获取本地音频传输是否打开

**定义**

```
 boolean getLocalAudioEnabled()
```

**返回值**

音频传输与否

### 8. 获取本地视频传输是否打开

**定义**

```
boolean getLocalVideoEnabled()
```

**返回值**

视频传输与否


### 9. 设置视频竖屏

**定义**

```
void setScreenToPortrait()
```

### 10. 设置视频横屏

**定义**

```
void setScreenToLandscape()
```

### 11. 切换前后摄像头

**定义**

```
void switchCamera()
```

### 12. 设置本地前置摄像头镜像是否打开

**定义**

```
void setFrontCameraMirrorEnable(boolean bEnable)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
enable | boolean | true为打开，alse为关闭 

### 13. 获取前置摄像头是否镜像

**定义**

```
 boolean getFrontCameraMirror()
```

**返回值**

是否镜像，默认关闭。

### 14. 不接收某人视频

**定义**

```
 void muteRemoteVideoStream(String rtcpId,boolean mute)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 流ID
mute | boolean | true禁止，false接收

### 15. 不接收某人音频

**定义**

```
void muteRemoteAudioStream(String rtcpId,  boolean mute)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 流ID
mute | boolean | true禁止，false接收


### 16. 发布媒体

**定义**

```
int publishByToken( String token,  ARVideoCommon.ARMediaType mediaType)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
token | String| 令牌:客户端向自己服务申请获得，参考企业级安全指南
mediaType | ARMediaType | 发布媒体类型

**返回值**

发布结果,0/1:发布失败（没有RECORD_AUDIO权限）/发布成功。

### 17. 取消发布媒体

**定义**

```
 void unPublish()
```

### 18. 订阅视频

**定义**

```
int subscribe(String rtcpId,String token)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 订阅视频的频道Id
token | String | 令牌:客户端向自己服务申请获得，参考企业级安全指南

### 19. 取消订阅媒体流

**定义**

```
void unSubscribe(String rtcpId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String| 媒体频道Id



### 20. 停止视频采集

**定义**

```
void stopCapture()
```
**说明**  

停止视频采集


### 21. 设置音频检测

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

### 22. 获取音频检测是否打开

**定义**

```
boolean isOpenAudioCheck()
```
**返回值**

音频检测打开与否


### 23. 打开或关闭网络状态监测

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

### 24. 获取网络监测是否打开

**定义**

```
boolean networkStatusEnabled() 
```

**返回值**

视频网络状态检测打开与否

### 25. 发布辅流

**定义**

```
void publishEx()
```


### 26. 取消发布辅流

**定义**

```
 void unPublishEx()
```

### 27.设置是否采用ARCamera

**定义**

```
void setUsedARCamera( boolean usedARCamera)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
usedARCamera | boolean | true 使用ARCamera，false 不使用ARCamera采集的数据

**说明**

默认使用ARCamera， 如果设置为false，必须调用setByteBufferFrameCaptured才能本地显示

### 28.设置本地显示的视频数据

**定义**

```
void setByteBufferFrameCaptured( byte[] data,  int width,  int height,  int rotation,  long timeStamp)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
data | byte[] | true 使用ARCamera，false 不使用ARCamera采集的数据
width | int | 宽
height | int | 高
rotation | int | 旋转角度
timeStamp | long | 时间戳

**说明**

不使用ARCamera，必须调用setByteBufferFrameCaptured才能本地显示

### 29.设置ARCamera视频数据回调

**定义**

```
void setARCameraCaptureObserver( ARCameraCapturerObserver capturerObserver)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
capturerObserver | ARCameraCapturerObserver | 回调



### 30. 关闭离开

**定义**

```
void clean()
```
**说明**  

停止采集，释放SDK

### ARRtcpEvent 回调接口类

### 1. 发布媒体成功回调

**定义**

```
void onPublishOK(String rtcpId,String liveInfo);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id
liveInfo | String | 直播信息

### 2. 发布媒体失败回调

**定义**

```
void onPublishFailed(int code, String reason);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int | 失败的code值
reason | String | 错误原因

### 3. 发布辅流媒体成功回调

**定义**

```
void onPublishExOK(String rtcpId,String liveInfo);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id
liveInfo | String | 直播信息

### 4. 发布辅流媒体失败回调

**定义**

```
void onPublishExFailed(int code, String reason);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int | 失败的code值
reason | String | 错误原因

### 5. 订阅通道成功的回调

**定义**

```
void onSubscribeOK(String rtcpId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id

### 6. 订阅通道失败的回调

**定义**

```
void onSubscribeFailed(String rtcpId, int code, String reason);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id
code | int | 失败的code值
reason | String | 错误原因

### 7. 订阅后音视频即将显示的回调

**定义**

```
void onRTCOpenRemoteVideoRender(String rtcpId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id

### 8. 订阅的音视频离开的回调

**定义**

```
void onRTCCloseRemoteVideoRender(String rtcpId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id

### 9. 订阅音频后成功的回调

**定义**

```
void onRTCOpenRemoteAudioTrack(String rtcpId);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | String | 通道Id

### 10. 订阅的音频离开的回调

**定义**

```
void onRTCCloseRemoteAudioTrack(String rtcpId)
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId |String | 通道Id

### 11. 订阅的流 音视频状态回调

**定义**

```
void onRTCRemoteAVStatus(String rtcpId, boolean audio, boolean video);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId |String | 通道Id
audio | boolean | true 音频打开 false 音频关闭
video | boolean | true 视频打开 false 视频关闭

### 12. 本地RTC音频检测

**定义**

```
void onRTLocalAudioActive(int level, int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
level | int | 音频检测音量（0~100）
time | int | 音频检测在time毫秒内不会再回调该方法（单位：毫秒）

**说明**

自己本地音频检测

### 13. 远程（其他人）RTC音频检测

**定义**

```
void onRTCRemoteAudioActive(String rtcpId, int level, int time);
```

**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId |String | 通道Id
level | int | 音频检测音量（0~100）
time | int | 音频检测在time毫秒内不会再回调该方法（单位：毫秒）

**说明**

对方关闭音频传输后（setLocalAudioEnable为false）,该回调将不再回调对方音频状态；对方关闭音频检测后（setAudioActiveCheck为false）,该回调也将不再回调对方音频状态。

### 14. 本地网络状态

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

### 15. 远程（其他人）网络状态

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


## 四、更新日志
**Version 3.0.0 （2019-05-15）**

* SDK版本升级3.0，API接口变更

**Version 2.0.0 （2017-09-30）**

* SDK版本升级2.0，梳理、完善SDK


  



