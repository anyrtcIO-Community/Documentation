# Documentation
包含平台所有文档

## 一、概述

### 简介
anyRTC提供对实时直播场景的支持，RTCPEngine SDK 能够实现一对一、一对多的纯音频和视频实时直播，相比[互动连麦(RTMPC)]()延时更低、极简API接口。适用于在线娃娃机、智能硬件、在线医疗、视频招聘、相亲交友等多种场景。由于RTCP只提供基础的音视频发布和订阅功能，因此可以自由组合成各种复杂的应用场景，由自己的业务系统控制。

### Dome体验

请根据需求选择渠道安装，安装完会议Demo后，可体验多人音视频会议功能。

- [iOS Demo下载](https://www.pgyer.com/anyrtc_rtcp_ios)		
		
- [Android Demo下载](https://www.pgyer.com/anyrtc_rtcp1)		
		
- [Web Demo 体验](https://www.anyrtc.io/demo/rtcp)	


#### 源码GitHub		
		
源码仅供开发者参考，适用于SDK调试，便于快速集成。		
		
- [iOS Demo 源码下载](https://github.com/anyRTC/anyRTC-RTCP-iOS)		
		
- [Android Demo 源码下载](https://github.com/anyRTC/anyRTC-RTCP-Android)		

- [Web Demo 源码下载](https://github.com/anyRTC/anyRTC-RTCP-Android)

## 二、集成指南		
		
#### 适用范围		
		
本集成文档适用于Web ArRTCP SDK 2.0.0 ~ 3.0.0版本。

#### 准备环境
- 支持 Chrome、IE 10 +、Firefox、Safari 等主流浏览器环境
- 域名需要配置SSL证书（https）

#### 下载SDK	

下载 Js SDK  

点击下载[ArRTCP SDK]()  

或npm安装   
```
npm i ar-rtcp
```
#### 导入SDK

可使用script直接引入
```
 <script src="jZego-rtc-1.0.1.js"></script>
```
或者
```
import {ArRTCPKit} from 'ar-rtcp'或
var ArRTCPKit = require('ar-rtcp');
```

#### 权限说明

#### 混淆配置

## 三、API接口文档

### ArRTCPKit API接口

#### 1. 实例化对象
```
var ArRTCP = new (ArRTCPKit || window.ArRTCPKit)();
```

#### 2. 配置开发者信息
##### 示例

```
ArRTCP.initEngine(appId, token);
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
appId | String | anyRTC云平台应用id
token | String | anyRTC云平台应用的appToken

##### 说明

该方法为配置开发者信息，上述参数均可在 [www.anyrtc.io](https://www.anyrtc.io/) 应用管理中获得。如果设置了[应用级安全设置]()或者[企业级安全设置]()anyRTC服务将会对服务请求进行认证，防止应用信息被盗影响业务系统正常运作。		

#### 3. 配置私有云

##### 示例
```
ArRTCP.configServerForPriCloud(address, port);
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
address | String | 私有云服务地址
port | Number | 私有云服务端口

##### 说明
配置私有云信息，如果部署了私有云的用户需要配置私有云服务器地址，否则连接公有云与私有云业务不互通。可选

#### 4. 设置视频分辨率

##### 示例
```
ArRTCP.setVideoProfile(ARVideoProfile);
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
ARVideoProfile | String | 视频分辨率 默认`ARVideoProfile640x360`

视频分辨率  
ARVideoProfile | 类型 | 描述 
---|---|---
ARVideoProfile160x120 | String | 160x120(120P)
ARVideoProfile120x120 | String | 120x120
ARVideoProfile320x180 | String | 320x180(180P)
ARVideoProfile180x180 | String | 180x180
ARVideoProfile240x180 | String | 240x180
ARVideoProfile320x240 | String | 320x240(240P)
ARVideoProfile240x240 | String | 240x240
ARVideoProfile424x240 | String | 424x240
ARVideoProfile640x360 | String | 640x360(360P)
ARVideoProfile360x360 | String | 360x360
ARVideoProfile480x360 | String | 480x360
ARVideoProfile640x480 | String | 640x480(480P)
ARVideoProfile480x480 | String | 480x480
ARVideoProfile848x480 | String | 848x480
<!-- ARVideoProfile960x720 | String | 960x720 -->
ARVideoProfile1280x720 | String | 1280x720(720P)
ARVideoProfile1920x1080 | String | 1920x1080(1080P)
ARVideoProfile2560x1440 | String | 2560x1440(1440P)
ARVideoProfile3840x2160 | String | 3840x2160(4K)

##### 说明
设置视频分辨率，默认`ARVideoProfile640x360`，请在`setLocalVideoCapturer`之前调用。

#### 5. 获取当前视频分辨率
##### 示例
```
ArRTCP.getVideoProfile();
```
##### 说明
获取当前设置的视频分辨率。

#### 6. 获取SDK版本号
##### 示例
```
ArRTCP.getSDKVersion();
```
#### 7. 打印日志级别

##### 示例
```
ArRTCP.setLogLevel(ArLogLevelModel);
```

##### 说明
获取当前SDK的版本信息。

##### 参数

参数名 | 类型 | 描述 
---|---|---
ArLogLevelModel | String | 日志级别

日志级别  

ArLogLevelModel | 类型 | 描述 
---|---|---
none | String | 不打印日志
info | String | 打印信息日志
warning | String | 打印警告日志
error | String | 打印错误日志
all | String | 打印所有日志

##### 说明
设置日志级别。可选

### ARRtcpKit 接口类

#### 1. 获取设备列表
##### 示例

```
ArRTCP.getDevices(deviceType).then(devices => {
  //获取列表成功
}).catch(err => {
  //获取失败
});
```
配合`async/await`使用
```
async function getDevice() {
  let devices = await ArRTCP.getDevices('videoinput');

}
```

##### 参数
参数名 | 类型 | 描述 
---|---|---
deviceType | String | 设备类型（非必选填）

设备类型  
deviceType | 类型 | 描述 
---|---|---
audioinput | String | 音频输入设备
audiooutput | String | 音频输出设备
videoinput | String | 视频输入设备
all | String | 查询所有设备类型(默认)

##### 说明
返回`Promise`对象，该方法用于获取指定设备类型的设备列表，不支持热拔插，可用定时器来监听设备热拔插。

#### 2. 设置本地视频采集窗口
##### 示例

```
ArRTCP.setLocalVideoCapturer(mediaOptions).then(stream => {
  //获取本地媒体流成功，预览本地媒体流，将媒体流绑定到video标签上
}).catch(err => {
  //获取失败
});;
```
配合`async/await`使用
```
async function doPreview() {
  let localMediaStream = await ArRTCP.setLocalVideoCapturer();
  //获取本地媒体流成功，预览本地媒体流，将媒体流绑定到video标签上
}
```

##### 参数
设备配置（非必选）
mediaOptions | 类型 | 描述 
---|---|---
video | Object | `video`对象，配置摄像头是否打开、打开指定的摄像头设备
audio | Object | `audio`对象，配置麦克风是否打开、打开指定的麦克风设备

`video`对象 
video | 类型 | 描述 
---|---|---
enabled | Boolean | 是否打开摄像头，`true`打开，`false`关闭。默认打开
deviceId | String | 设备ID，通过`getDevices`获取。

`audio`对象 
audio | 类型 | 描述 
---|---|---
enabled | Boolean | 是否打开麦克风，`true`打开，`false`关闭。默认打开
deviceId | String | 设备ID，通过`getDevices`获取。

##### 说明
返回`Promise`对象，采集本地音视频媒体流，将媒体流绑定到video标签上可以预览本地图像，退出房间时或挂断通话时需要移除本地图像（video标签）。

#### 3. 设置本地音频是否传输
##### 示例
```
ArRTCP.setLocalAudioEnable(enable);
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
enable | Boolean | true传输，false不传输，默认为true

##### 说明
该方法需要在setLocalVideoCapturer回调成功之后才能调用。

#### 4. 设置本地视频是否传输
##### 示例
```
ArRTCP.setLocalVideoEnable(enable);
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
enable | Boolean | true传输，false不传输，默认为true

##### 说明
该方法需要在setLocalVideoCapturer回调成功之后才能调用。

#### 5. 获取本地视频传输是否打开
##### 示例
```
ArRTCP.getLocalVideoEnable();
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
enable | Boolean | true传输，false不传输，默认为true

##### 说明
该方法需要在setLocalVideoCapturer回调成功之后才能调用。

#### 6. 获取本地音频传输是否打开
##### 示例
```
ArRTCP.getLocalAudioEnable();
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
enable | Boolean | true传输，false不传输，默认为true

##### 说明
该方法需要在setLocalVideoCapturer回调成功之后才能调用。

#### 7. 切换摄像头
##### 示例
```
ArRTCP.switchDevice(mediaOptions);
```
##### 参数
设备配置（非必选）
mediaOptions | 类型 | 描述 
---|---|---
video | Object | `video`对象，配置摄像头是否打开、打开指定的摄像头设备
audio | Object | `audio`对象，配置麦克风是否打开、打开指定的麦克风设备

`video`对象 
video | 类型 | 描述 
---|---|---
enabled | Boolean | 是否打开摄像头，`true`打开，`false`关闭。默认打开
deviceId | String | 设备ID，通过`getDevices`获取。

`audio`对象 
audio | 类型 | 描述 
---|---|---
enabled | Boolean | 是否打开麦克风，`true`打开，`false`关闭。默认打开
deviceId | String | 设备ID，通过`getDevices`获取。

##### 说明
切换媒体设备并获取媒体流，并将更改后的媒体流通知给房间内（已订阅自己媒体流）的其他人员。在收到本地媒体流更新回调时候，需要将本地媒体流重新绑定到video标签上。该方法需要在setLocalVideoCapturer回调成功之后才能调用。

#### 8. 发布媒体流
##### 示例
```
ArRTCP.publish(nMediaType, anyRTCId);
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
nMediaType | Number | 0为发布音视频，1为只发布音频
anyRTCId | String | 自定义标识ID(房间ID)

##### 说明
发布媒体流成功(onRTCPublishOK)之后，将会收到媒体流标识ID（pubId），通过订阅该媒体流标识ID订阅该发布者的媒体流。

#### 9. 取消发布媒体流
##### 示例
```
ArRTCP.unPublish();
```

##### 说明
停止发布已发布的媒体流。

#### 10. 发布媒体辅流
##### 示例
```
ArRTCP.publishEx(mediaStream);
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
mediaStream | MediaStream | 需要发布的媒体流（屏幕共享媒体流）

##### 说明
只有在发布媒体（主）流成功之后，才能发布辅流。[屏幕共享插件]()

#### 11. 取消发布媒体辅流
##### 示例
```
ArRTCP.unPublishEx();
```

##### 说明
取消发布媒体辅流，最多只能发布一路主流和一路辅流。

#### 12. 订阅媒体辅流
##### 示例
```
ArRTCP.subscribe(pubId);
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
pubId | String | 媒体流标识ID，媒体流发布成功之后获得

##### 说明
订阅已经发布成功的媒体流

#### 13. 取消订阅媒体辅流
##### 示例
```
ArRTCP.unSubscribe(pubId);
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
pubId | String | 媒体流标识ID，媒体流发布成功之后获得

##### 说明
取消订阅某路的媒体流

#### 13. 禁止接收某路视频流
##### 示例
```
ArRTCP.muteRemoteVideoStream(enable, pubId);
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
enable | Boolean | `true`禁止，`false`接收
pubId | String | 媒体流标识ID，媒体流发布成功之后获得

##### 说明
禁止接收某路订阅的视频流

#### 14. 禁止接收某路音频流
##### 示例
```
ArRTCP.muteRemoteAudioStream(enable, pubId);
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
enable | Boolean | `true`禁止，`false`接收
pubId | String | 媒体流标识ID，媒体流发布成功之后获得

##### 说明
禁止接收某路订阅的视频流

#### 15. 设置token验证
##### 示例
```
ArRTCP.setUserToken(userToken);
```
##### 参数

参数名 | 类型 | 描述 
---|---|---
userToken | String | token字符串，客户端向自己服务器申请

##### 说明
如果设置了[企业级安全设置]()，只有通过企业认证之后才可使用anyRTC服务。设置token验证必须放在publish、subscribe之前。

#### 16. 设置远端视频窗口
##### 示例
```
ArRTCP.setRemoteVideoRender(stream, render);
```

##### 参数
参数名 | 类型 | 描述 
---|---|---
stream | String | RTC实时视频流
render | String | 音视频容器DOM对象（video或audio)

##### 说明
将RTC实时媒体流绑定到远端人员标识的video或audio标签中。

#### 17. 离开anyRTC频道
##### 示例
```
ArRTCP.close();
```

##### 说明
断开服务链接，释放实例。

#### 18. 设置音频检测
##### 示例
```
ArRTCP.setAudioActiveCheck(on);
```

##### 参数
参数名 | 类型 | 描述 
---|---|---
on | Boolean | 是否开启音频检测，默认打开(true)

##### 说明
打开音频检测会收到已订阅的媒体的音频大小回调。

#### 19. 获取音频检测是否打开
##### 示例
```
ArRTCP.getAudioActiveCheck();
```

##### 说明
返回`true`或`false`。true为打开 false为关闭

#### 20. 设置视频网络状态检测
##### 示例
```
ArRTCP.setNetworkStatus(on);
```

##### 参数
参数名 | 类型 | 描述 
---|---|---
on | Boolean | 是否开启视频网络状态检测，默认打开

##### 说明
打开视频网络状态检测会收到已订阅的媒体的视频码率和丢包率回调。

#### 21. 获取视频网络状态是否打开
##### 示例
```
ArRTCP.getNetworkStatus();
```

##### 说明
返回`true`或`false`。true为打开 false为关闭

### ArRTCP 回调

#### 1. 发布媒体成功回调
##### 示例
```
ArRTCP.onRTCPublishOK = (pubId) => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
pubId | String | 发布成功的媒体流标识ID
<!-- liveInfo | String | 安卓特有（开通H5直播功能之后，安卓终端会打开） -->

##### 说明
发布媒体成功之后会收到该回调，获得频道ID，用户订阅该频道即可观看发布者图像。

#### 2. 发布媒体失败回调
##### 示例
```
ArRTCP.onRTCPublishFailed = (code, reason) => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
code | Int | 状态码，错误原因可查看nCode对应原因
reason | String | 错误原因，RTC错误或者token错误（错误值自己平台定义）

##### 说明
发布媒体失败回调。

#### 3. 发布媒体辅流成功回调
##### 示例
```
ArRTCP.onRTCPublishExOK = (pubId) => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
pubId | String | 发布成功的媒体流标识ID
<!-- liveInfo | String | 安卓特有（开通H5直播功能之后，安卓终端会打开） -->

##### 说明
发布媒体辅流成功之后会收到该回调，获得频道ID，用户订阅该频道即可观看发布者图像。

#### 4. 发布媒体辅流失败回调
##### 示例
```
ArRTCP.onRTCPublishExFailed = (code, reason) => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
code | Int | 状态码，错误原因可查看nCode对应原因
reason | String | 错误原因，RTC错误或者token错误（错误值自己平台定义）

##### 说明
发布媒体辅流失败回调。

#### 5. 订阅频道成功的回调
##### 示例
```
ArRTCP.onRTCSubscribeOK = pubId => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
pubId | String | 发布成功的媒体流标识ID

##### 说明
订阅频道成功的回调。

#### 6. 订阅频道失败的回调 
##### 示例
```
ArRTCP.onRTCSubscribeFailed = (pubId, code, reason) => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
pubId | String | 发布成功的媒体流标识ID
code | Number | 状态码 错误原因可查看nCode对应原因
reason | String | 错误原因，RTC错误或者token错误（错误值自己平台定义）

##### 说明
订阅媒体失败回调。

#### 7. 订阅成功后创建媒体流窗口(视频即将显示)
##### 示例
```
ArRTCP.onRTCRemoteOpenVideoRender = pubId => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
pubId | String | 发布成功的媒体流标识ID

##### 说明
订阅成功后，创建显示窗口，等收到媒体流回调之后再将媒体流绑定到窗口。

#### 8. 取消订阅后移除媒体流窗口(移除视频)
##### 示例
```
ArRTCP.onRTCRemoteCloseVideoRender = pubId => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
pubId | String | 发布成功的媒体流标识ID

##### 说明
比如对方关闭了音频，对方关闭了视频均会走该回调。

#### 9. 订阅之后收到媒体流 
##### 示例
```
ArRTCP.onRTCRemoteMediaStream = (stream, pubId) => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
stream | MediaStream |  音视频流
pubId | String | 发布成功的媒体流标识ID

##### 说明
收到该回调应该调用RTCPKit方法'setRemoteVideoRender'。

#### 10. 监听订阅媒体流的音频大小
##### 示例
```
ArRTCP.onRTCRemoteAudioActive = (pubId, audioLevel) => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
pubId | String | 发布成功的媒体流标识ID
audioLevel | Number | 音频音量(0~100)

##### 说明
监听订阅媒体流的音频音量大小变化。

#### 11. 监听本地媒体流的音频大小
##### 示例
```
ArRTCP.onRTCLocalAudioActive = audioLevel => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
audioLevel | Number | 音频音量(0~100)

##### 说明
监听本地媒体流的音频音量大小变化。

#### 12. 监听订阅媒体流的网络质量
##### 示例
```
ArRTCP.onRTCRemoteNetworkStatus = (pubId, netSpeed, packetLost, netQuality) => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
pubId | String | 发布成功的媒体流标识ID
netSpeed | Number | 网络上行
packetLost | Number | 丢包率
netQuality | Number | 网络质量

##### 说明
监听订阅媒体流的音频音量大小变化。

#### 13. 监听本地媒体流的网络质量
##### 示例
```
ArRTCP.onRTCLocalNetworkStatus = (netSpeed, packetLost, netQuality) => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
netSpeed | Number | 网络上行
packetLost | Number | 丢包率
netQuality | Number | 网络质量

##### 说明
监听本地媒体流的音频音量大小变化。

#### 14. 监听SDK错误
##### 示例
```
ArRTCP.onSDKError = (methodName, errorInfo) => {

};
```
##### 参数
参数名 | 类型 | 描述 
---|---|---
methodName | String | 出现异常的接口名字
errorInfo | String | 错误信息

##### 说明
监听SDK错误。

## 四、更新日志
**Version 3.0.0 （2019-01-24）**

- SDK版本升级3.0，API接口变更，更加简洁规范

**Version 2.0.0 （2017-09-30）**

- DK版本升级2.0，梳理、完善SDK

## 五、错误码对照表		

以下为介绍 iOS RTCPEngine SDK 的错误码。		

名称 | 值 | 备注
---|---|---
AnyRTC_OK | 0 | 正常		
AnyRTC_UNKNOW | 1 | 未知错误		
AnyRTC_EXCEPTION | 2 | SDK调用异常		
AnyRTC_EXP_UNINIT | 3 | SDK未初始化		
AnyRTC_EXP_PARAMS_INVALIDE | 4 | 参数非法		
AnyRTC_EXP_NO_NETWORK | 5 | 没有网络链接		
AnyRTC_EXP_NOT_FOUND_CAMERA | 6 | 没有找到摄像头设备		
AnyRTC_EXP_NO_CAMERA_PERMISSION | 7 | 没有打开摄像头权限		
AnyRTC_EXP_NO_AUDIO_PERMISSION | 8 | 没有音频录音权限		
AnyRTC_EXP_NOT_SUPPORT_WEBRTC | 9 | 浏览器不支持原生的webrtc		
AnyRTC_NET_ERR | 100 | 网络错误 		
AnyRTC_NET_DISSCONNECT | 101 | 网络断开		
AnyRTC_LIVE_ERR | 102 | 直播出错		
AnyRTC_EXP_ERR | 103 | 异常错误		
AnyRTC_EXP_Unauthorized | 104 | 服务未授权(仅可能出现在私有云项目)		
AnyRTC_BAD_REQ | 201 | 服务不支持的错误请求		
AnyRTC_AUTH_FAIL | 202  | 认证失败		
AnyRTC_NO_USER | 203 | 此开发者信息不存在		
AnyRTC_SVR_ERR | 204 | 服务器内部错误		
AnyRTC_SQL_ERR | 205 | 服务器内部数据库错误		
AnyRTC_ARREARS | 206 | 账号欠费		
AnyRTC_LOCKED | 207 | 账号被锁定		
AnyRTC_SERVER_NOT_OPEN | 208 | 服务未开通		
AnyRTC_ALLOC_NO_RES | 209 | 没有服务器资源		
AnyRTC_SERVER_NOT_SURPPORT | 210 | 不支持的服务		
AnyRTC_FORCE_EXIT | 211 | 强制离开		

