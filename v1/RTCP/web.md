## 一、概述

### 简介

anyRTC提供对实时直播场景的支持，RTCPEngine SDK 能够实现一对一、一对多的纯音频和视频实时直播，相比RTMPC延时更低、极简API接口。适用于在线娃娃机、智能硬件、在线医疗、视频招聘、相亲交友等多种场景。

### Dome体验

请根据需求选择渠道安装，安装完会议Demo后，可体验多人音视频会议功能。

- [iOS Demo下载](https://www.pgyer.com/anyrtc_rtcp_ios)		
- [Android Demo下载](https://www.pgyer.com/anyrtc_rtcp1)		
- [Web Demo 体验](https://demos.anyrtc.io/ar-rtcp/)	

#### 源码GitHub		

源码仅供开发者参考，适用于SDK调试，便于快速集成。		


- [iOS Demo 源码下载](https://github.com/anyRTC/anyRTC-RTCP-iOS)		
- [Android Demo 源码下载](https://github.com/anyRTC/anyRTC-RTCP-Android)		
- [Web Demo 源码下载](https://github.com/anyRTC/anyRTC-RTCP-Web)		

## 二、集成指南		

#### 适用范围		

本集成文档适用于Web ARRtcpKit SDK 3.0.0及以上版本。

#### 准备环境

支持 Chrome、IE 10 +、Firefox、Safari 等主流浏览器环境

#### 导入SDK	

##### npm 市场

- 通过 [npm市场](https://www.npmjs.com/package/ar-rtcp) 下载：

```
npm install ar-rtcp --save-dev

import ArRtcpKit from 'ar-rtcp';
```

- 安装或更新至最新版本：

```
npm install ar-rtcp@latest --save-dev

import ArRtcpKit from 'ar-rtcp';
```

##### js 引用

- 前往[SDK 下载页面](https://docs.anyrtc.io/download/js/ArRtcpKit.3.0.4.js)，`ctrl+s`或`command+s`保存到本地
- 引用

```
<script src="yourAssetsPath/ArRtcpKit.版本号.js"></script>
```

## 三、API接口文档

### ARRtcpKit API介绍

#### 1. 实例化对象

```
var rtcp = new ARRtcpKit(Options);
```

##### 参数

| 参数名  |  类型  | 描述     |
| ------- | :----: | -------- |
| Options | Object | 实例配置 |

`Options`

| 参数名       |  类型   | 描述                                         |
| ------------ | :-----: | -------------------------------------------- |
| videoProfile | String  | 视频分辨率，发布媒体流的类型为音视频时才成效 |
| logLevel     | String  | 打印日志级别： `info`、`warning`、`error`    |
| userId       | String  | 自定义用户ID                                 |
| userData     | String  | 自定义用户数据，推荐JSON字符串               |
| autoBitrate  | boolean | 是否开启码流自适应                           |

`videoProfile`

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

#### 2. 设置UserToken

##### 示例

```
rtcp.setUserToken(userToken);
```

##### 参数

| 参数名    | 类型   | 描述                         |
| --------- | ------ | ---------------------------- |
| userToken | String | anyRTC云平台的应用的appToken |

##### 说明

该方法为配置UserToken必须放在`publish`、`subscribe`之前，适用于[企业级安全认证](https://docs.anyrtc.io/security/%E6%9C%8D%E5%8A%A1%E7%BA%A7%E5%AE%89%E5%85%A8%E8%AE%BE%E7%BD%AE%E6%8C%87%E5%8D%97.html)。

#### 3. 配置开发者信息

##### 示例

```
rtcp.initAppInfo(appid, apptoken); 
```

##### 参数

| 参数名   | 类型   | 描述                      |
| -------- | ------ | ------------------------- |
| appId    | String | anyRTC云平台的应用id      |
| appToken | String | anyRTC云平台的应用的Token |

##### 说明

该方法为配置开发者信息，上述参数均可在 [www.anyrtc.io](https://www.anyrtc.io/) 应用管理中获得。

#### 4. 配置私有云

##### 示例

```
rtcp.configServer(address);
```

##### 参数

| 参数名  |  类型  | 描述           |
| ------- | :----: | -------------- |
| address | String | 私有云服务地址 |

##### 说明

配置私有云信息，当使用私有云时才需要进行配置，默认无需配置。

#### 5. 获取SDK版本号

##### 示例

```
rtcp.getSDKVersion();
```

**返回值**

ARRtcpKit SDK版本号。

##### 说明

获取当前SDK版本。

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
rtcp.setLocalVideoCapturer(constraints);
```

##### 参数

| 参数名      |  类型  | 描述                             |
| ----------- | :----: | -------------------------------- |
| constraints | Object | 音视频配置，可以不填写默认为开启 |

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

#### 4. 设置本地音频是否传输

##### 示例

```
rtcp.setLocalAudioEnable(enable);
```

##### 参数

| 参数名 | 类型    | 描述                              |
| ------ | ------- | --------------------------------- |
| enable | Boolean | true传输，false不传输，默认为true |

##### 说明

该方法需要在setLocalVideoCapturer回调成功之后才能调用。

#### 5. 设置本地视频是否传输

##### 示例

```
rtcp.setLocalVideoEnable(enable);
```

##### 参数

| 参数名 | 类型    | 描述                              |
| ------ | ------- | --------------------------------- |
| enable | Boolean | true传输，false不传输，默认为true |

##### 说明

该方法需要在setLocalVideoCapturer回调成功之后才能调用。

#### 6. 获取本地视频传输是否打开

##### 示例

```
rtcp.getLocalVideoEnable();
```

##### 说明

获取本地视频是否传输。

#### 7. 获取本地音频传输是否打开

##### 示例

```
rtcp.getLocalAudioEnable();
```

##### 说明

获取本地音频是否传输。

#### 8. 发布媒体

##### 示例

```
rtcp.publish(mediaType);
```

##### 参数

| 参数名    | 类型   | 描述                          |
| --------- | ------ | ----------------------------- |
| mediaType | Number | 媒体类型：`0` 视音频，`1`音频 |

##### 说明

如果发布成功，回调中会分配一个频道Id，用户订阅该频道即可观看发布者图像

#### 9. 取消发布媒体

##### 示例

```
rtcp.unPublish();
```

##### 说明

取消发布媒体。

#### 10. 发布辅流

##### 示例

```
rtcp.publishEx(scrnStream);
```

##### 参数

| 参数名     | 类型   | 描述                                   |
| ---------- | ------ | -------------------------------------- |
| scrnStream | String | 需要发布的辅流（如：屏幕共享的视频流） |

##### 说明

发布辅流（屏幕共享流），发布共享流之前，需要获取到屏幕共享流，具体使用请查看[ar-share-screen](<https://www.npmjs.com/package/ar-share-screen>)。

#### 11. 取消发布辅流

##### 示例

```
rtcp.unPublishEx();
```

##### 说明

取消发布辅流。

#### 12. 媒体订阅

##### 示例

```
rtcp.subscribe(rtcpId);
```

##### 参数

| 参数名 | 类型   | 描述             |
| ------ | ------ | ---------------- |
| rtcpId | String | 系统分配的频道ID |

##### 说明

媒体订阅。

#### 13. 取消媒体订阅

##### 示例

```
rtcp.unSubscribe(rtcpId);
```

##### 参数

| 参数名 | 类型   | 描述             |
| ------ | ------ | ---------------- |
| rtcpId | String | 系统分配的频道ID |

##### 说明

取消媒体订阅。

#### 14. 结束直播

##### 示例

```
rtcp.close();
```

##### 说明

结束直播。

### ARRtcpKit 回调

> 监听回调请在初始化之后监听，如果遇到回调多次的情况可能是监听多次回调引发的BUG。

#### 1. 远程屏幕共享媒体流已订阅

##### 示例

```
rtcp.on("exstream-subscribed", function(pubId, mediaRender){});
```

##### 参数

| 参数名 | 类型   | 描述                          |
| ------ | ------ | ----------------------------- |
| pubId  | String | RTC服务生成媒体流的唯一标识ID |

##### 说明

已订阅远程的屏幕共享媒体流，创建该媒体流的视图窗口，等待接收媒体流并展示。

#### 2. 远程屏幕共享媒体流被移除

##### 示例

```
rtcp.on("exstream-unsubscribed", function(pubId){});
```

##### 参数

| 参数名 | 类型   | 描述                          |
| ------ | ------ | ----------------------------- |
| pubId  | String | RTC服务生成媒体流的唯一标识ID |

##### 说明

远程的屏幕共享媒体流被移除，取消订阅并移除该媒体流的视图窗口。

#### 3. 远程媒体流已订阅

##### 示例

```
rtcp.on("stream-subscribed", function(pubId, mediaRender){});
```

##### 参数

| 参数名 | 类型   | 描述                          |
| ------ | ------ | ----------------------------- |
| pubId  | String | RTC服务生成媒体流的唯一标识ID |

##### 说明

已订阅远程媒体流，创建该媒体流的视图窗口，等待接收媒体流并展示。

#### 4. 远程媒体流被移除

##### 示例

```
rtcp.on("stream-unsubscribed", function(pubId){});
```

##### 参数

| 参数名 | 类型   | 描述                          |
| ------ | ------ | ----------------------------- |
| pubId  | String | RTC服务生成媒体流的唯一标识ID |

##### 说明

远程媒体流被移除，取消订阅并移除该媒体流的视图窗口。

#### 5. 订阅媒体流音量大小变化

##### 示例

```
rtcp.on("audio-volume", function(isRemote, pubId, audioLevel){});
```

##### 参数

| 参数名     | 类型    | 描述                                                         |
| ---------- | ------- | ------------------------------------------------------------ |
| isRemote   | Boolean | 是否是远程用户                                               |
| pubId      | String  | RTC服务生成媒体流的唯一标识ID                                |
| audioLevel | Number  | 音量范围为 0 到 100 之间的整数。通常在列表中音量大于 5 的用户为持续说话的人 |

##### 说明

提示房间内谁在说话以及说话者的音量。每30毫秒回调一次。

#### 6. 订阅媒体流网络变化

##### 示例

```
rtcp.on("network-status", function(isRemote, pubId, videoBytes, ARNetQuality){});
```

##### 参数

| 参数名       | 类型    | 描述                                                         |
| ------------ | ------- | ------------------------------------------------------------ |
| isRemote     | Boolean | 是否是远程用户                                               |
| pubId        | String  | RTC服务生成媒体流的唯一标识ID                                |
| videoBytes   | Number  | 视频码率                                                     |
| ARNetQuality | String  | 网络状况，根据丢包率严重分为5个等级，`ARNetQualityExcellent`(优)、`ARNetQualityGood`（良好）、`ARNetQualityAccepted`（一般）、`ARNetQualityBad`（差）、`ARNetQualityVBad`(极差) |

##### 说明

网络状态每一秒回调一次。

#### 7. 加入频道成功

##### 示例

```
rtcp.on("join-success", function(){});
```

#### 8. 加入频道失败

##### 示例

```
rtcp.on("join-failed", function(code){});
```

##### 参数

| 参数名 | 类型   | 描述                   |
| ------ | ------ | ---------------------- |
| code   | Number | 错误码，详情查看错误码 |

#### 11. 发布媒体流成功

##### 示例

```
rtcp.on("stream-published", function(pubId){});
```

##### 参数

| 参数名 | 类型   | 描述                          |
| ------ | ------ | ----------------------------- |
| pubId  | String | RTC服务生成媒体流的唯一标识ID |

##### 说明

发布媒体流成功，得到pubId。

#### 12. 发布媒体流失败

##### 示例

```
rtcp.on("stream-publish-failed", function(){});
```

发布媒体流失败。

#### 13. 辅流发布成功

##### 示例

```
rtcp.on("exstream-published", function(pubId){});
```

##### 参数

| 参数名 | 类型   | 描述                          |
| ------ | ------ | ----------------------------- |
| pubId  | String | RTC服务生成媒体流的唯一标识ID |

##### 说明

辅流发布成功，获得媒体流标识Id。

#### 14. 服务器断开连接

##### 示例

```
rtcp.on("server-disconnect", function(){});
```

##### 说明

服务器断开连接。

## 四、更新日志

**Version 3.0.4 （2019-05-27）**

- 更新文档，文档添加`getDevices`和`switchDevice`的介绍

- 修复远程图像显示不全的问题

**Version 3.0.0 （2019-01-24）**

- SDK版本升级3.0，API接口变更，更加简洁规范

## 五、错误码对照表		

以下为介绍 RTCPEngine SDK 的错误码。

| 名称                            | 值   | 备注                               |
| ------------------------------- | ---- | ---------------------------------- |
| ARRtcp_OK                       | 0    | 正常                               |
| ARRtcp_UNKNOW                   | 1    | 未知错误                           |
| ARRtcp_EXCEPTION                | 2    | SDK调用异常                        |
| ARRtcp_EXP_UNINIT               | 3    | SDK未初始化                        |
| ARRtcp_EXP_PARAMS_INVALIDE      | 4    | 参数非法                           |
| ARRtcp_EXP_NO_NETWORK           | 5    | 没有网络链接                       |
| ARRtcp_EXP_NOT_FOUND_CAMERA     | 6    | 没有找到摄像头设备                 |
| ARRtcp_EXP_NO_CAMERA_PERMISSION | 7    | 没有打开摄像头权限                 |
| ARRtcp_EXP_NO_AUDIO_PERMISSION  | 8    | 没有音频录音权限                   |
| ARRtcp_EXP_NOT_SUPPOAR_WEBARC   | 9    | 浏览器不支持原生的webrtc           |
| ARRtcp_NET_ERR                  | 100  | 网络错误                           |
| ARRtcp_NET_DISSCONNECT          | 101  | 网络断开                           |
| ARRtcp_LIVE_ERR                 | 102  | 直播出错                           |
| ARRtcp_EXP_ERR                  | 103  | 异常错误                           |
| ARRtcp_EXP_UNAUTHORIZED         | 104  | 服务未授权(仅可能出现在私有云项目) |
| ARRtcp_BAD_REQ                  | 201  | 服务不支持的错误请求               |
| ARRtcp_AUTH_FAIL                | 202  | 认证失败                           |
| ARRtcp_NO_USER                  | 203  | 此开发者信息不存在                 |
| ARRtcp_SVR_ERR                  | 204  | 服务器内部错误                     |
| ARRtcp_SQL_ERR                  | 205  | 服务器内部数据库错误               |
| ARRtcp_ARREARS                  | 206  | 账号欠费                           |
| ARRtcp_LOCKED                   | 207  | 账号被锁定                         |
| ARRtcp_SERVER_NOT_OPEN          | 208  | 服务未开通                         |
| ARRtcp_ALLOC_NO_RES             | 209  | 没有服务器资源                     |
| ARRtcp_SERVER_NO_SURPPOAR       | 210  | 不支持的服务                       |
| ARRtcp_FORCE_EXIT               | 211  | 强制离开                           |
| ARRtcp_NOT_START                | 800  | 会议未开始                         |