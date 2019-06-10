## 一、概述

### 简介

anyRTC提供对会议场景的支持，RTCMeeting SDK，高清流畅的音视频、高安全性、全平台运行、丰富的会议管理功能，支持视频、语音多人会议，适用于会议、培训、互动等多人移动会议。

### Demo体验

请根据需求选择渠道安装，安装完RTMeeting Demo后，可体验在线会议功能。

- [iOS Demo下载](https://www.pgyer.com/xoTQ)
- [Android Demo下载](http://www.pgyer.com/eU0U)
- [Web Demo 体验](https://demos.anyrtc.io/ar-meet/)

### 源码GitHub

源码仅供开发者参考，适用于SDK调试，便于快速集成。

- [iOS Demo 源码下载](https://github.com/anyRTC/anyRTC-Meeting-iOS)
- [Android Demo 源码下载](https://github.com/anyRTC/anyRTC-Meeting-Android)
- [Web Demo 源码下载](https://github.com/anyRTC/anyRTC-Meeting-Web)

## 二、集成指南

### 适用范围

本集成文档适用于Web RTMeetEngine SDK 3.0.0及以上版本。

### 兼容情况

- Chrome、Firefox、safari 11(以上)或其他谷歌内核浏览器
- H5支持chrome内核
- 配合[anyRTC webRTC检测工具](https://www.npmjs.com/package/ar-detect)、[anyRTC 屏幕共享SDK](<https://www.npmjs.com/package/ar-share-screen>)使用

### 导入SDK

##### npm 市场

- 通过 [npm市场](https://www.npmjs.com/package/ar-meet) 下载：

```
npm install ar-meet --save-dev

import ArMeetKit from 'ar-meet';
```

- 安装或更新至最新版本：

```
npm install ar-meet@latest --save-dev

import ArMeetKit from 'ar-meet';
```

##### js 引用

- 前往[SDK 下载页面](https://docs.anyrtc.io/download/js/ArMeetKit.3.0.7.js)，`ctrl+s`或`command+s`保存到本地
- 引用

```
<script src="yourAssetsPath/ArMeetKit.版本.js"></script>
```

## 三、API接口文档

### ARMeet 类方法介绍

#### 1.初始化实例

##### 示例

```
let meet = new ArMeetKit(Options);
```

##### 参数

| 参数名  |  类型  | 描述     |
| ------- | :----: | -------- |
| Options | Object | 实例配置 |

**Options**

| 参数名       |  类型   | 描述                                                         |
| ------------ | :-----: | ------------------------------------------------------------ |
| meetMode     | Number  | 会议模式： 0 视频会议 1 主持人模式；2 ZOOM模式; 默认`0`（视频会议） |
| audioMeet    | Boolean | 音频会议： false 视频会议 true 音频会议；默认`false`（音视频会议） |
| videoProfile | String  | 视频分辨率                                                   |
| userRole     | Number  | 用户身份：0 与会者 1 主持人 2 监看者；默认`0`;（注：会议模式为`0`时用户身份仅有与会者和监看者身份生效，当会议模式为`1`时仅与会者和主持人身份生效） |
| logLevel     | String  | 打印日志级别： `info`、`warning`、`error`                    |
| userId       | String  | 自定义用户ID                                                 |
| userData     | String  | 自定义用户数据，推荐JSON字符串                               |
| autoBitrate  | boolean | 是否开启码流自适应                                           |

**videoProfile**

| 参数名               |  类型  | 描述           |
| -------------------- | :----: | -------------- |
| ARVideoProfile120P   | String | 160*120 65     |
| ARVideoProfile180P   | String | 320*180 140    |
| ARVideoProfile240P   | String | 320*240 200    |
| ARVideoProfile360P   | String | 640*360 400    |
| ARVideoProfile360P_1 | String | 640*360 512    |
| ARVideoProfile480P   | String | 640*480 512    |
| ARVideoProfile480P_1 | String | 640*480 768    |
| ARVideoProfile720P   | String | 1280*720 1280  |
| ARVideoProfile1080P  | String | 1920*1080 2048 |

#### 2. 配置开发者信息

##### 示例

```
meet.initAppInfo(appId, appToken);
```

##### 参数

| 参数名   |  类型  | 描述                    |
| -------- | :----: | ----------------------- |
| appId    | String | anyRTC云平台的应用ID    |
| appToken | String | anyRTC云平台的应用Token |

##### 说明

该方法为配置开发者信息，上述参数均可在[https://www.anyrtc.io/ ](https://www.anyrtc.io/)应用管理中获得。

#### 3. 配置私有云

##### 示例

```
meet.configServer(address);
```

##### 参数

| 参数名  |  类型  | 描述                                |
| ------- | :----: | ----------------------------------- |
| address | String | 私有云服务地址（HTTPS环境不支持IP） |

##### 说明

配置私有云信息，当使用私有云时才需要进行配置，默认无需配置。

#### 4. 获取SDK版本号

##### 示例

```
meet.getSDKVersion();
```

##### 说明

返回RTMeeting SDK版本号。

### ARMeetKit 接口类

#### 1. 获取媒体设备

##### 示例

```
meet.getDevices(deviceType);
```

##### 参数

| 参数名      |  类型  | 描述                             |
| ----------- | :----: | -------------------------------- |
| deviceType  | string | 媒体设备类型`videoinput`、`audioinput`、`audiooutput`，分别是摄像头、麦克风、扬声器 |

**Return**

`Promise`对象

`Promise.then` 返回的参数data对象，包含所查询的设备列表，如果deviceType为空则返回videoinput、videooutput、audioinput三个类型的设备列表

##### 说明

获取设备列表，根据设备列表的id可以切换指定摄像头以及指定扬声器作为音频输出设备。

#### 2. 预览本地音视频

##### 示例

```
meet.setLocalVideoCapturer(constraints);
```

##### 参数

| 参数名      |  类型  | 描述                             |
| ----------- | :----: | -------------------------------- |
| constraints | Object | 音视频配置（非必填项） |

**constraints**

| 参数名 |  类型  | 描述       |
| ------ | :----: | ---------- |
| video  | Object | 视频配置； |
| audio  | Object | 音频配置； |

| video    |  类型   | 描述                                         |
| -------- | :-----: | -------------------------------------------- |
| enable   | Boolean | `true`为采集摄像头， `false`;                |
| devideId | String  | devideId: 设备ID，可通过`getDevices`方法获取 |

| audio    |  类型   | 描述                                         |
| -------- | :-----: | -------------------------------------------- |
| enable   | Boolean | `true`为采集麦克风， `false`;                |
| devideId | String  | devideId: 设备ID，可通过`getDevices`方法获取 |

**Return**

`Promise`对象

`Promise.then` 返回的参数data

| data        |      描述      |
| ----------- | :------------: |
| mediaStream |  MediaStream   |
| mediaRender | HTMLDivElement |

##### 说明

预览本地音视频，如果`video.enable`为`true`时，表示允许采集摄像头；`video.deviceId`表示采集指定摄像头的媒体流。

#### 3. 切换设备

##### 示例

```
meet.switchDevice(constraints);
```
##### 参数

| 参数名      |  类型  | 描述                             |
| ----------- | :----: | -------------------------------- |
| constraints | Object | 音视频配置（非必填项） |

**constraints**

| 参数名 |  类型  | 描述       |
| ------ | :----: | ---------- |
| video  | Object | 视频配置； |
| audio  | Object | 音频配置； |

| video    |  类型   | 描述                                         |
| -------- | :-----: | -------------------------------------------- |
| enable   | Boolean | `true`为采集摄像头， `false`;                |
| devideId | String  | devideId: 设备ID，可通过`getDevices`方法获取 |

| audio    |  类型   | 描述                                         |
| -------- | :-----: | -------------------------------------------- |
| enable   | Boolean | `true`为采集麦克风， `false`;                |
| devideId | String  | devideId: 设备ID，可通过`getDevices`方法获取 |

**Return**

`Promise`对象

`Promise.then` 返回的参数data

| data        |      描述      |
| ----------- | :------------: |
| mediaStream |  MediaStream   |
| mediaRender | HTMLDivElement |

##### 说明

切换设备获取新的媒体流，将新的mediaRender展示到页面。

#### 4. 设置本地视频是否传输

##### 示例

```
meet.setLocalVideoEnable(enable);
```

##### 参数

| 参数名 |  类型   | 描述                                                |
| ------ | :-----: | --------------------------------------------------- |
| enable | Boolean | `true`为传输视频，`false`为不传输视频，默认视频传输 |

#### 5. 设置本地音频是否传输

##### 示例

```
meet.setLocalAudioEnable(enable);
```

##### 参数

| 参数名 |  类型   | 描述                                                |
| ------ | :-----: | --------------------------------------------------- |
| enable | Boolean | `true`为传输视频，`false`为不传输视频，默认视频传输 |

#### 6. 获取本地视频传输是否打开

##### 示例

```
meet.getLocalVideoEnable();
```

##### 说明

视频是否传输。 

#### 7. 获取本地音频传输是否打开

##### 示例

```
meet.getLocalAudioEnable();
```

##### 说明

音频是否传输。 

#### 8. 设置远端音视频是否传输

##### 示例

```
meet.setRemoteAVEnable(peerId, audioEnable, videoEnable);
```

##### 参数

| 参数名      |  类型   | 描述                                                       |
| ----------- | :-----: | ---------------------------------------------------------- |
| peerId      | String  | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成) |
| audioEnable | Boolean | 音频是否传输                                               |
| videoEnable | Boolean | 视频是否传输                                               |

#### 9. 打开共享通道

##### 示例

```
meet.openShare(type);
```

##### 参数

| 参数名 |  类型  | 描述                 |
| ------ | :----: | -------------------- |
| type   | Number | 自定义共享通道标识id |

##### 说明

打开共享通道之后，将会收到`share-result`回调。

#### 10. 设置共享信息

##### 示例

```
meet.setShareInfo(shareInfo);
```

##### 参数

| 参数名    |  类型  | 描述                           |
| --------- | :----: | ------------------------------ |
| shareInfo | String | 自定义共享信息，推荐JSON字符串 |

##### 说明

打开共享通道成功(`share-result` )之后调用，其他端在收到`share-opened`回调时将收到该参数。

#### 11. 关闭共享通道

##### 示例

```
meet.closeShare(type);
```

##### 参数

| 参数名 |  类型  | 描述                 |
| ------ | :----: | -------------------- |
| type   | Number | 自定义共享通道标识id |

##### 说明

结束共享，其他参会者将会收到`share-closed`

#### 12. 开始屏幕共享

##### 示例

```
meet.startScreenCap(scrnStream);
```

##### 参数

| 参数名     |    类型     | 描述           |
| ---------- | :---------: | -------------- |
| scrnStream | MediaStream | 屏幕共享媒体流 |

##### 说明

该接口表示发布该屏幕共享流，发布共享流之前，需要获取到屏幕共享流，具体使用请前往npm 市场查看[ar-share-screen](<https://www.npmjs.com/package/ar-share-screen>)。

#### 13. 结束屏幕共享

##### 示例

```
meet.stopScreenCap();
```

##### 说明

停止该屏幕共享流。

#### 14. 加入房间

##### 示例

```
meet.joinRTC(roomId, userToken);
```

##### 参数

| 参数名    |  类型  | 描述                                                         |
| --------- | :----: | ------------------------------------------------------------ |
| roomId    | String | 房间唯一标识ID                                               |
| userToken | String | 用于企业认证，如果开通了企业认证的客户，需要使用该方法进行初始化设置 |

#### 15. 发送用户实时消息

##### 示例

```
meet.sendUserMessage(userName, userHeaderUrl, content);
```

##### 参数

| 参数名        |  类型  | 描述                                           |
| ------------- | :----: | ---------------------------------------------- |
| userName      | String | 自定义用户昵称（最大256字节）                  |
| userHeaderUrl | String | 自定义头像URL（最大512字节）                   |
| content       | String | 自定义消息内容，推荐JSON字符串（最大1024字节） |

#### 16. 设置某路视频广播 -- (仅主持人模式有效)

##### 示例

```
meet.setBroadCast(enable, peerId);
```

##### 参数

| 参数名 |  类型   | 描述                                                       |
| ------ | :-----: | ---------------------------------------------------------- |
| enable | Boolean | 是否广播用户图像: `true`为广播，`false`为不广播            |
| peerId | String  | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成) |

##### 说明

主持人模式，主持人可以看到所有用户的图像，与会者只能看到主持人模式，如果主持人广播某人的视频，其他都可以看到被广播者的图像。取消广播，将会移除被广播者的图像。

#### 17. 设置是否单独聊天 -- (仅主持人模式有效)

##### 示例

```
meet.setTalkOnly(enable, peerId);
```

##### 参数

| 参数名 |  类型   | 描述                                                       |
| ------ | :-----: | ---------------------------------------------------------- |
| enable | Boolean | 是否设置单聊: `true`为广播，`false`为不广播                |
| peerId | String  | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成) |

##### 说明

主持人设置单聊对象，房间内的人将不能听到单聊双方的语音。

#### 18. 离开房间

##### 示例

```
meet.leaveRTC();
```

##### 说明

释放实例。离开页面或退出房间时需要调用此方法，其他端才会收到用户离开的回调，否则会有视图残留在页面。

#### 19. 设置音频检测是否打开

##### 示例

```
meet.setAudioActiveCheck(enable);
```

##### 参数

| 参数名 |  类型   | 描述                              |
| ------ | :-----: | --------------------------------- |
| enable | Boolean | `true`打开，`false`关闭，默认关闭 |

#### 20. 获取音频检测是否打开

##### 示例

```
meet.getAudioActiveCheck();
```

##### 说明

获取网络状态。 

#### 21. 设置网络质量是否打开

##### 示例

```
meet.setNetworkStatus(enable);
```

##### 参数

| 参数名 |  类型   | 描述                              |
| ------ | :-----: | --------------------------------- |
| enable | Boolean | `true`打开，`false`关闭，默认关闭 |

#### 22. 获取当前网络状态是否打开

##### 示例

```
meet.getNetworkStatus();
```

##### 说明

获取网络状态。 

#### 23. 设置zoom模式

##### 示例

```
meet.setZoomMode(zoomMode);
```

##### 参数

| 参数名   |  类型  | 描述                                             |
| -------- | :----: | ------------------------------------------------ |
| zoomMode | Number | `0`为演讲者模式；`1`为画廊模式；默认演讲者模式。 |

#### 24. 切换zoom模式

##### 示例

```
meet.switchZoomMode(zoomMode);
```

##### 参数

| 参数名   |  类型  | 描述                                             |
| -------- | :----: | ------------------------------------------------ |
| zoomMode | Number | `0`为演讲者模式；`1`为画廊模式；默认演讲者模式。 |

#### 25. 获取人员列表

##### 示例

```
meet.getUserList();
```

##### 说明

人员列表。 

### ARMeetKitDelegate 接口类

#### 1. 加入会议成功

##### 示例

```
meet.on('join-success', () => {});
```

#### 2. 加入会议失败

##### 示例

```
meet.on('join-failed', (code) => {});
```

##### 参数

| 参数名 |  类型  | 描述                   |
| ------ | :----: | ---------------------- |
| code   | Number | 错误码，详情查看错误码 |

#### 3. 远程媒体流已订阅

##### 示例

```
meet.on('stream-subscribed', (peerId, pubId, userId, userData, mediaRender) => {});
```

##### 参数

| 参数名      |  类型  | 描述                                                         |
| ----------- | :----: | ------------------------------------------------------------ |
| peerId      | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)   |
| pubId       | String | RTC服务生成媒体流的唯一标识ID                                |
| userId      | String | 自定义用户ID(该加入人员的用户信息，初始化实例时携带的`userId`) |
| userData    | String | 自定义用户数据(该加入人员的用户信息，初始化实例时携带的`userData`) |
| mediaRender | String | 远程视频                                                     |

##### 说明

已订阅远程媒体流，创建该媒体流的视图窗口，等待接收媒体流并展示。

#### 4. 远程媒体流被移除

##### 示例

```
meet.on('stream-unsubscribed', (peerId, pubId, userId, userData) => {});
```

##### 参数

| 参数名   |  类型  | 描述                                                         |
| -------- | :----: | ------------------------------------------------------------ |
| peerId   | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)   |
| pubId    | String | RTC服务生成媒体流的唯一标识ID                                |
| userId   | String | 自定义用户ID(该加入人员的用户信息，初始化实例时携带的`userId`) |
| userData | String | 自定义用户数据(该加入人员的用户信息，初始化实例时携带的`userData`) |

##### 说明

远程媒体流被移除，取消订阅并移除该媒体流的视图窗口。

#### 5. 远程屏幕共享媒体流已订阅

##### 示例

```
meet.on('exstream-subscribed', (peerId, pubId, userId, userData, mediaRender) => {});
```

##### 参数

| 参数名      |  类型  | 描述                                                         |
| ----------- | :----: | ------------------------------------------------------------ |
| peerId      | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)   |
| pubId       | String | RTC服务生成媒体流的唯一标识ID                                |
| userId      | String | 自定义用户ID(该加入人员的用户信息，初始化实例时携带的`userId`) |
| userData    | String | 自定义用户数据(该加入人员的用户信息，初始化实例时携带的`userData`) |
| mediaRender | String | 远程视频                                                     |

##### 说明

已订阅远程的屏幕共享媒体流，创建该媒体流的视图窗口，等待接收媒体流并展示。

#### 6. 远程屏幕共享媒体流被移除

##### 示例

```
meet.on('exstream-unsubscribed', (peerId, pubId, userId, userData) => {});
```

##### 参数

| 参数名   |  类型  | 描述                                                         |
| -------- | :----: | ------------------------------------------------------------ |
| peerId   | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成)   |
| pubId    | String | RTC服务生成媒体流的唯一标识ID                                |
| userId   | String | 自定义用户ID(该加入人员的用户信息，初始化实例时携带的`userId`) |
| userData | String | 自定义用户数据(该加入人员的用户信息，初始化实例时携带的`userData`) |

##### 说明

远程的屏幕共享媒体流被移除，取消订阅并移除该媒体流的视图窗口。

> 注意：一个用户（peerId）最多可以发布两路媒体流（pubId），当人员离开时，该人员发布的两路媒体流将会被移除；只有发布了主流，才能发布辅流(屏幕共享)。

#### 7. 远程人员媒体流的音视频状态

##### 示例

```
meet.on('av-status', (isRemote, pubId, audioEnable, videoEnable) => {});
```

##### 参数

| 参数名      |  类型   | 描述                                    |
| ----------- | :-----: | --------------------------------------- |
| isRemote    | Boolean | 是否是远程用户                          |
| pubId       | String  | RTC服务生成媒体流的唯一标识ID           |
| audioEnable | Boolean | 音频状态；`true`为打开；`false`为打开。 |
| videoEnable | Boolean | 视频状态；`true`为打开；`false`为打开。 |

##### 说明

远程人员媒体流的音视频状态回调，当远程人员`setLocalVideoEnable`和`setLocalAudioEnable`时，房间其他人员将会收到此回调。

#### 8. 音频音量大小变化

##### 示例

```
meet.on('audio-volume', (isRemote, pubId, audioVolume) => {});
```

##### 参数

| 参数名      |  类型   | 描述                                                         |
| ----------- | :-----: | ------------------------------------------------------------ |
| isRemote    | Boolean | 是否是远程用户                                               |
| pubId       | String  | RTC服务生成媒体流的唯一标识ID                                |
| audioVolume | Number  | 音量范围为 0 到 100 之间的整数。通常在列表中音量大于 5 的用户为持续说话的人 |

##### 说明

提示房间内谁在说话以及说话者的音量。每30毫秒回调一次。

#### 9. 网络状态变化

##### 示例

```
meet.on('network-status', (isRemote, pubId, videoBytes, ARNetQulity) => {});
```

##### 参数

| 参数名      |  类型   | 描述                                                         |
| ----------- | :-----: | ------------------------------------------------------------ |
| isRemote    | Boolean | 是否是远程用户                                               |
| pubId       | String  | RTC服务生成媒体流的唯一标识ID                                |
| videoBytes  | Number  | 视频码率                                                     |
| ARNetQulity | String  | 网络状况，根据丢包率严重分为5个等级，`ARNetQualityExcellent`(优)、`ARNetQualityGood`（良好）、`ARNetQualityAccepted`（一般）、`ARNetQualityBad`（差）、`ARNetQualityVBad`(极差) |

##### 说明

网络状态每一秒回调一次。

#### 10. 共享通道被开启

##### 示例

```
meet.on('share-opened', (shareType, shareInfo, userId, userData) => {});
```

##### 参数

| 参数名    |  类型  | 描述                         |
| --------- | :----: | ---------------------------- |
| shareType | Number | 共享者设置的自定义共享通道ID |
| shareInfo | String | 共享者设置的自定义共享信息   |
| userId    | String | 自定义用户ID                 |
| userData  | String | 自定义用户数据               |

##### 说明

远程人员打开共享通道`openShare`。

#### 11. 共享通道被关闭

##### 示例

```
meet.on('share-closed', () => {});
```

##### 说明

远程人员关闭共享通道`close-share`。

#### 12. 打开共享通道结果

##### 示例

```
meet.on('share-result', (isOpen) => {});
```

##### 参数

| 参数名 |  类型   | 描述         |
| ------ | :-----: | ------------ |
| isOpen | Boolean | 是否打开成功 |

##### 说明

当调用`openShare`时，该回调将会返回打开共享通道是否成功。

#### 13. 收到用户消息

##### 示例

```
meet.on('user-message', (userId, userName, userAvatar, msgContent) => {});
```

##### 参数

| 参数名     |  类型  | 描述                                              |
| ---------- | :----: | ------------------------------------------------- |
| userId     | String | 自定义用户ID(发送者userId)                        |
| userName   | String | 自定义用户昵称（`sendUserMessage`携带的用户昵称） |
| userAvatar | String | 自定义用户头像(`sendUserMessage`携带的用户头像)   |
| msgContent | String | 自定义消息内容(`sendUserMessage`携带的消息内容)   |

##### 说明

收到房间内用户发送的实时消息，根据实时消息可以进行一些业务拓展（消息主体可以以JSON字符串的形式传出，收到消息之后进行解析），比如: 踢人等。

#### 14. 本地图像第一帧

##### 示例

```
meet.on('local-video-size', (videoWidth, videoHeight) => {});
```

##### 参数

| 参数名      |  类型  | 描述     |
| ----------- | :----: | -------- |
| videoWidth  | Number | 视频宽度 |
| videoHeight | Number | 视频高度 |

##### 说明

本地图像返回的第一帧，目前仅返回视频图像的大小，如果与设置的分辨率不匹配说明摄像头不支持不支持预设分辨，已切换为最佳分辨率。

#### 15. 主持人上线 -- (仅主持人模式可用)

##### 示例

```
meet.on('hoster-online', (peerId, userId, userData) => {});
```

##### 参数

| 参数名   |  类型  | 描述                                                       |
| -------- | :----: | ---------------------------------------------------------- |
| peerId   | String | RTC服务生成的标识Id (用于标识与会者，每次加入会议随机生成) |
| userId   | String | 自定义用户ID(主持人的用户ID)                               |
| userData | String | 自定义用户数据(主持人的用户数据)                           |

##### 说明

说明主持人在线。该回调只有在会议模式为主持人模式时才能生效；

#### 16. 主持人下线 -- (仅主持人模式可用)

##### 示例

```
meet.on('hoster-offline', (peerId) => {});
```

##### 参数

| 参数名 |  类型  | 描述                                              |
| ------ | :----: | ------------------------------------------------- |
| peerId | String | 主持人离开 (用于标识与会者，每次加入会议随机生成) |

##### 说明

说明主持人下线。该回调只有在会议模式为主持人模式时才能生效；

#### 17. 主持人开启1V1单聊 -- (仅主持人模式可用)

##### 示例

```
meet.on('talk-only-on', (peerId, userId, userData) => {});
```

##### 参数

| 参数名   |  类型  | 描述                                              |
| -------- | :----: | ------------------------------------------------- |
| peerId   | String | 主持人离开 (用于标识与会者，每次加入会议随机生成) |
| userId   | String | 自定义用户ID(主持人的用户ID)                      |
| userData | String | 自定义用户数据(主持人的用户数据)                  |

##### 说明

主持人设置了`1V1`单聊，仅单聊双方双方的语音互通，其他禁音但是可以接收视频图像，音频会议除外。

#### 19. 主持人结束1V1单聊 -- (仅主持人模式可用)

##### 示例

```
meet.on('talk-only-off', (peerId) => {});
```

##### 参数

| 参数名 |  类型  | 描述                                              |
| ------ | :----: | ------------------------------------------------- |
| peerId | String | 主持人离开 (用于标识与会者，每次加入会议随机生成) |

##### 说明

主持人关闭了`1V1`单聊。

#### 20. 获取zoom演讲者模式下的信息

##### 示例

```
meet.on('zoom-info', (zoomMode, pubRender) => {});
```

##### 参数

| 参数名    |  类型  | 描述                         |
| --------- | :----: | ---------------------------- |
| zoomMode  | Number | zoom模式(`0`演讲者，`1`画廊) |
| pubRender | Number | 人数（不包括自己）           |

##### 说明

获取演讲者模式下的人数信息。

#### 21. 当前说话的演讲者

##### 示例

```
meet.on('zoom-speaker', (zoomMode, pubId, zoomUserMember) => {});
```

##### 参数

| 参数名   |  类型  | 描述                          |
| -------- | :----: | ----------------------------- |
| zoomMode | Number | zoom模式(`0`演讲者，`1`画廊)  |
| pubId    | String | RTC服务生成媒体流的唯一标识ID |
| zoomMode | Number | zoom模式房间人数              |

##### 说明

主持人关闭了`1V1`单聊。

## 四、更新日志

**Version 3.0.7 （2019-06-10）**

- 修复已知BUG

**Version 3.0.6 （2019-05-31）**

- 添加`takeVideoRenderSnapshot`、`downloadVideoRenderSnapshot`和`attachSinkId`方法

- 修复主持人模式异常

**Version 3.0.5 （2019-05-27）**

- 更新文档，文档添加`getDevices`和`switchDevice`的介绍

- 修复远程图像显示不全的问题

**Version 3.0.4 （2019-05-24）**

- `stream-subscribed`、`exstream-subscribed`、`stream-unsubscribed`、`exstream-unsubscribed`回调添加userId回调参数，作为第三个回调参数返回

- 修复切换摄像头的内部错误

**Version 3.0.2 （2019-05-21）**

- 修复会议窗口显像过慢的问题

**Version 3.0.1 （2019-05-21）**

- 修复开启屏幕共享的用户离会之后屏幕共享提示框残留的问题

**Version 3.0.0 （2019-05-18）**

- SDK版本升级3.0，API接口变更，更加简洁规范

## 五、错误码对照表

以下为介绍 Web RTMeetEngine SDK 的错误码。

| 名称                            | 值   | 备注                               |
| ------------------------------- | ---- | ---------------------------------- |
| ARMeet_OK                       | 0    | 正常                               |
| ARMeet_UNKNOW                   | 1    | 未知错误                           |
| ARMeet_EXCEPTION                | 2    | SDK调用异常                        |
| ARMeet_EXP_UNINIT               | 3    | SDK未初始化                        |
| ARMeet_EXP_PARAMS_INVALIDE      | 4    | 参数非法                           |
| ARMeet_EXP_NO_NETWORK           | 5    | 没有网络链接                       |
| ARMeet_EXP_NOT_FOUND_CAMERA     | 6    | 没有找到摄像头设备                 |
| ARMeet_EXP_NO_CAMERA_PERMISSION | 7    | 没有打开摄像头权限                 |
| ARMeet_EXP_NO_AUDIO_PERMISSION  | 8    | 没有音频录音权限                   |
| ARMeet_EXP_NOT_SUPPOAR_WEBARC   | 9    | 浏览器不支持原生的webrtc           |
| ARMeet_NET_ERR                  | 100  | 网络错误                           |
| ARMeet_NET_DISSCONNECT          | 101  | 网络断开                           |
| ARMeet_LIVE_ERR                 | 102  | 直播出错                           |
| ARMeet_EXP_ERR                  | 103  | 异常错误                           |
| ARMeet_EXP_UNAUTHORIZED         | 104  | 服务未授权(仅可能出现在私有云项目) |
| ARMeet_BAD_REQ                  | 201  | 服务不支持的错误请求               |
| ARMeet_AUTH_FAIL                | 202  | 认证失败                           |
| ARMeet_NO_USER                  | 203  | 此开发者信息不存在                 |
| ARMeet_SVR_ERR                  | 204  | 服务器内部错误                     |
| ARMeet_SQL_ERR                  | 205  | 服务器内部数据库错误               |
| ARMeet_ARREARS                  | 206  | 账号欠费                           |
| ARMeet_LOCKED                   | 207  | 账号被锁定                         |
| ARMeet_SERVER_NOT_OPEN          | 208  | 服务未开通                         |
| ARMeet_ALLOC_NO_RES             | 209  | 没有服务器资源                     |
| ARMeet_SERVER_NO_SURPPOAR       | 210  | 不支持的服务                       |
| ARMeet_FORCE_EXIT               | 211  | 强制离开                           |
| ARMeet_NOT_STAAR                | 700  | 房间未开始                         |
| ARMeet_IS_FULL                  | 701  | 房间人员已满                       |
| ARMeet_NOT_COMPARE              | 702  | 房间类型不匹配                     |