## 一、概述

### 简介

ARCall SDK支持点对点视频呼叫、音频呼叫。适用于在客服咨询、视频面签、视频开户、视频报警等多种业务场景。

## 二、集成指南

### 适用范围

本集成文档适用于Web 呼叫 ARCall SDK 3.0.0版本。

### 兼容情况

- Chrome、Firefox、safari 11(以上)等，具体使用[webRTC检测工具](https://docs.anyrtc.io/v1/tools/%E6%B5%8F%E8%A7%88%E5%99%A8WebRTC%E6%A3%80%E6%B5%8B.html)
- H5支持chrome内核。

### 导入SDK

**npm 市场**

- 通过 npm市场 下载：

```
npm install ar-call --save-dev

import ArCall from 'ar-call';
```

- 安装或更新至最新版本：

```
npm install ar-call@latest --save-dev

import ArCall from 'ar-call';
```

##### js 引用

- 前往[SDK 下载页面](https://docs.anyrtc.io/download/js/ArCallKit.3.0.4.js)，`ctrl+s`或`command+s`保存到本地
- 引用

```
<script src="yourAssetsPath/ArCallKit.版本.js"></script>
```

## 三、API接口文档

### ARCall API介绍

#### 初始化实例

**示例**

```
let call = new ArCall(Options: Object);
```

**参数**

| 参数名  |  类型  | 描述     |
| ------- | :----: | -------- |
| Options | object | 实例配置 |

**Options**

| 参数名      |  类型   | 描述                                 |
| ----------- | :-----: | ------------------------------------ |
| userData    | string  | 自定义用户数据                       |
| autoBitrate | boolean | 是否开启码率自适用                   |
| logLevel    | string  | 日志级别；`info`、`error`、`warning` |

#### 配置开发者信息

**定义**

```
call.initAppInfo(appId: string, apptoken: string);
```

**参数**

| 参数名   |  类型  | 描述      |
| -------- | :----: | --------- |
| appId    | string | 应用ID    |
| apptoken | string | 应用token |

**说明**

该方法为配置开发者信息。

#### 配置服务地址

**定义**

```
call.configServer(url);
```

**参数**

| 参数名 |  类型  | 描述                            |
| ------ | :----: | ------------------------------- |
| url    | string | 服务地址，例如: `www.baidu.com` |

**说明**

配置服务器地址，私有云需要配置，否则无需配置（如果网站是`HTPPS` 环境不支持IP）。

#### 获取SDK版本号

**定义**

```
call.getSDKVersion();
```

**返回值**

获取ARCall SDK版本号

#### 获取媒体设备

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

#### 采集本地视频窗口

**定义**

```
call.setLocalVideoCapturer(constraints);
```

**参数**

| 参数名      |  类型  | 描述                   |
| ----------- | :----: | ---------------------- |
| constraints | object | 采集媒体配置（非必填） |

**constraints**

| 参数名 |  类型  | 描述                                                         |
| ------ | :----: | ------------------------------------------------------------ |
| video  | object | `enable`是否打开摄像头<br />`deviceId`指定设备，`deviceId` 通过`getDevices` 接口获取 |
| audio  | object | `enable`是否打开摄像头<br />`deviceId`指定设备，`deviceId` 通过`getDevices` 接口获取 |

**说明**

返回`Promise`对象。

#### 切换媒体输入设备

##### 示例

```
meet.switchMediaInputDevice(constraints);
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

切换设备输入设备（麦克风或摄像头），预览获取新的媒体流之后移除旧的预览窗口。

#### 设置坐席身份

**定义**

```
call.setAsClerk(strArea, strCallId, strBusiness, nLevel);
```

**参数**

| 参数名      |  类型  | 描述               |
| ----------- | :----: | ------------------ |
| strArea     | string | 自定义区域         |
| strCallId   | string | 自定义坐席频道ID   |
| strBusiness | string | 自定义业务类型     |
| nLevel      | number | 自定义坐席服务级别 |

**说明**

设置坐席身份，需要在`turnOn`调用之前设置。

#### 用户上线（登录）

**定义**

```
call.turnOn(userId， userToken);
```

**参数**

| 参数名    |  类型  | 描述                                   |
| --------- | :----: | -------------------------------------- |
| userId    | string | 自定义用户ID                           |
| userToken | string | 第三方认证所需要携带的userToken（非必填） |

**说明**

用户上线，如果后台开启了[服务级安全设置]([https://docs.anyrtc.io/v1/security/%E6%9C%8D%E5%8A%A1%E7%BA%A7%E5%AE%89%E5%85%A8%E8%AE%BE%E7%BD%AE%E6%8C%87%E5%8D%97.html](https://docs.anyrtc.io/v1/security/服务级安全设置指南.html))，将会对userToken进行认证。

#### 用户下线（退出）

**定义**

```
call.turnOff();
```

#### 发起呼叫请求

**定义**

```
call.makeCall(peerUserId, callMode, userData, callExtend);
```

**参数**

| 参数名     |  类型  | 描述                                                         |
| ---------- | :----: | ------------------------------------------------------------ |
| peerUserId | string | 自定义用户ID                                                 |
| callMode   | number | 呼叫模式: `0`  视频呼叫`、2`音频呼叫、 `20`音频呼叫客服 、 `21`视频呼叫客服 |
| userData   | string | 自定义用户数据                                               |
| callExtend | object | 自定义拓展数据（选填）                                       |

**说明**

发起呼叫，被叫端会收到`make-call`回调。

#### 同意呼叫请求

**定义**

```
call.acceptCall(peerUserId);
```

**参数**

| 参数名     |  类型  | 描述         |
| ---------- | :----: | ------------ |
| peerUserId | string | 自定义用户ID |

**说明**

同意呼叫请求，呼叫通道建立；

#### 拒绝呼叫请求

**定义**

```
call.rejectCall(peerUserId);
```

**参数**

| 参数名     |  类型  | 描述         |
| ---------- | :----: | ------------ |
| peerUserId | string | 自定义用户ID |

**说明**

拒绝呼叫请求；

#### 结束/取消呼叫请求

**定义**

```
call.endCall(peerUserId);
```

**参数**

| 参数名     |  类型  | 描述         |
| ---------- | :----: | ------------ |
| peerUserId | string | 自定义用户ID |

**说明**

结束或取消呼叫请求；

#### 设置本地音频是否传输

**定义**

```
call.setLocalAudioEnable(enable);
```

**参数**

| 参数名 |  类型   | 描述                                        |
| ------ | :-----: | ------------------------------------------- |
| enable | boolean | `true`传输音频，`false`不传输音频，默认传输 |

#### 设置本地视频是否传输

**定义**

```
call.setLocalVideoEnable(enable);
```

**参数**

| 参数名 | 类型 | 描述                                        |
| ------ | :--: | ------------------------------------------- |
| enable | boolean | `true`传输视频，`false`不传输视频，默认传输 |

#### 获取本地音频传输是否打开

**定义**

```
call.getLocalAudioEnable();
```

**返回**

音频是否传输。 

#### 获取本地视频传输是否打开

**定义**

```
call.getLocalVideoEnable();
```

**返回**

视频是否传输。 

#### 发送实时消息

**定义**

```
call.sendUserMessage(peerUserId, msgContent);
```

**参数**

| 参数名 | 类型 | 描述                                        |
| ------ | :--: | ------------------------------------------- |
| peerUserId | string | 自定义用户ID |
| msgContent | string | 自定义消息内容，json字符串 |

**说明**

发送实时消息。 

### ARCall 回调接口

#### 用户上线成功

**定义**

```
call.on("online-success", () => {

});
```

**说明**

用户上线成功

#### 用户上线失败

**定义**

```
call.on("online-failed", (errCode) => {

});
```

**参数**

| 参数名  |  类型  | 描述           |
| ------- | :----: | -------------- |
| errCode | number | 上线失败错误码 |

#### 被呼叫

**定义**

```
call.on("make-call", (roomId, peerUserId, callMode, peerUserData，callExtend) => {

});
```

**参数**

| 参数名       |  类型  | 描述                                 |
| ------------ | :----: | ------------------------------------ |
| roomId       | string | 房间ID                               |
| peerUserId   | string | 呼叫者的userId                       |
| callMode     | number | 呼叫模式：`0` 视频呼叫、`2` 音频呼叫 |
| peerUserData | string | 呼叫者的userData                     |
| callExtend   | string | `makeCall`携带的自定义拓展数据       |

**说明**

当收到远端呼叫时，将会收到此回调，如果同意通话采集本地视频窗口(`setLocalVideoCapturer`)之后再调用`acceptCall` ，否则`rejectCall` 。

#### 被叫同意通话请求

**定义**

```
call.on("accept-call", (peerUserId) => {

});
```

**参数**

| 参数名     |  类型  | 描述                 |
| ---------- | :----: | -------------------- |
| peerUserId | string | 呼叫者的自定义userId |

**说明**

发起呼叫请求`makeCall` ，对方收到`make-call` 回调，并同意`acceptCall`，会收到此回调。

#### 被叫拒绝通话请求

**定义**

```
call.on("reject-call", (peerUserId) => {

});
```

**参数**

| 参数名     |  类型  | 描述                 |
| ---------- | :----: | -------------------- |
| peerUserId | string | 呼叫者的自定义userId |

**说明**

发起呼叫请求`makeCall` ，对方收到`make-call` 回调，并拒绝`rejectCall`，会收到此回调。

#### 通话结束

**定义**

```
call.on("end-call", (peerUserId, errCode) => {

});
```

**参数**

| 参数名     |  类型  | 描述                          |
| ---------- | :----: | ----------------------------- |
| peerUserId | string | 对方的自定义userId            |
| errCode    | string | 挂断的错误码。`0`为正常挂断。 |

**说明**

对方挂断通话`endCall` 或者对方下线`turnOff`。

#### 打开远程视频窗口

**定义**

```
call.on("stream-subscribed", (peerUserId, renderId, peerUserId, peerUserData, render) => {

});
```

**参数**

| 参数名       |      类型      | 描述                 |
| ------------ | :------------: | -------------------- |
| peerUserId   |     string     | 对方的自定义userId   |
| renderId     |     string     | 视频窗口标识ID       |
| peerUserId   |     string     | 对方的自定义userId   |
| peerUserData |     string     | 对方的自定义userData |
| render       | HTMLDIVElement | 媒体窗口             |

**说明**

对方（或己方）同意视频通话请求后，会收到此回调，收到回调需要将render添加展示到页面上。

#### 移除远程视频窗口

**定义**

```
call.on("stream-unsubscribed", (peerUserId, renderId, peerUserId, peerUserData) => {

});
```

**参数**

| 参数名       |  类型  | 描述                 |
| ------------ | :----: | -------------------- |
| peerUserId   | string | 对方的自定义userId   |
| renderId     | string | 视频窗口标识ID       |
| peerUserId   | string | 对方的自定义userId   |
| peerUserData | string | 对方的自定义userData |

**说明**

对方（或己方）同意视频通话请求后挂断通话，会收到此回调，收到回调需要将render从页面上移除。

#### 收到实时消息

**定义**

```
call.on("user-message", (peerUserId, msgContent) => {

});
```

**参数**

| 参数名 | 类型 | 描述                                        |
| ------ | :--: | ------------------------------------------- |
| peerUserId | string | 自定义用户ID |
| msgContent | string | 自定义消息内容，json字符串 |

**说明**

接收到用户发送的实时消息。 

## 四、更新日志

**Version 3.0.4 （2019-05-28）**

- SDK将方法`switchDevice`更名`switchMediaInputDevice`，同时同步更新文档

**Version 3.0.2 （2019-05-27）**

- 更新文档，文档添加`getDevices`和`switchDevice`的介绍

- 修复远程图像显示不全的问题

**Version 3.0.0 （2019-01-18）**

- SDK版本升级3.0，API接口变更，更加简洁规范

## 五、错误码对照表

以下为介绍 iOS RTMeetEngine SDK 的错误码。

| 名称                 | 值   | 备注                                           |
| -------------------- | ---- | ---------------------------------------------- |
| RTCCall_OK           | 0    | 正常                                           |
| RTCCall_PEER_BUSY    | 800  | 对方正忙                                       |
| RTCCall_OFFLINE      | 801  | 对方不在线                                     |
| RTCCall_NOT_SELF     | 802  | 不能呼叫自己                                   |
| RTCCall_EXP_OFFLINE  | 803  | 通话中对方意外掉线                             |
| RTCCall_EXP_EXIT     | 804  | 对方异常导致(如：重复登录帐号将此前的帐号踢出) |
| RTCCall_TIMEOUT      | 805  | 呼叫超时(45秒)                                 |
| RTCCall_NOT_SURPPORT | 806  | 不支持                                         |
