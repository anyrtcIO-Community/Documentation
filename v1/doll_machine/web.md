## 一、概述

### 简介
第三代娃娃机在线解决方案，全新娱乐方式，超低延时娱乐。</br>

### Demo体验

请根据需求选择渠道安装，安装完RTWawaji Demo后，可体验在线娃娃机功能。

- [iOS Demo下载](https://www.pgyer.com/anyrtc_wawaji_ios)

- [Android Demo下载](https://www.pgyer.com/anyRTC_Wawaji)

- [Web Demo 体验](http://wawaji.anyrtc.cc/)

### 源码GitHub

- [iOS Demo 源码下载](https://github.com/anyRTC/anyRTC-Meeting-iOS)

- [Android 服务端Demo 源码下载](https://github.com/AnyRTC/anyRTC-WaWa-Server-Android)

- [Android Demo 源码下载](https://github.com/AnyRTC/anyRTC-WaWa-Client-Android)

## 二、集成指南

### 适用范围

本集成文档适用于Web RTWawaji SDK 3.0.0版本。

### 兼容情况

- Chrome、Firefox、safari 11(以上)或其他谷歌内核浏览器
- H5支持chrome内核

## 三、API接口文档

### ARMeet 类方法介绍

#### 1.初始化实例

##### 示例

```
var WaWaClient = AnyRTCWaWaClient || window.AnyRTCWaWaClient;
var anyClient = new WaWaClient();
```
#### 2.获取SDK版本信息

##### 示例

```
anyClient.getSdkVersion();
```

##### 说明
该方法为获取版本信息。

#### 3.配置开发者信息

##### 示例

```
anyClient.initEngineWithAnyRTCInfo(DEV_ID, APP_ID, APP_KEY, APP_TOKEN);
```
##### 参数

参数名 | 类型 | 描述
---|:---:|---
DEV_ID | String | 平台开发者信息
APP_ID | String | 平台应用appid
APP_KEY | String | 平台应用appkey
APP_TOKEN | String | 平台应用appToken

##### 说明

该方法为配置开发者信息，上述参数均可在[https://www.anyrtc.io/ ](https://www.anyrtc.io/)应用管理中获得。


#### 4.开启服务并连接远程娃娃机

##### 示例

```
anyClient.openServer();
```

##### 说明
该方法为开启服务连接远程歪歪机，收到成功回调后可以进行后续操作。

#### 5.获取房间列表

##### 示例

```
anyClient.getRoomList();
```

##### 说明
该方法为获取房间列表。

#### 6.加入房间

##### 示例

```
anyClient.joinRoom(anyrtcid, userid, username, usericon, usertype);
```
##### 参数

参数名 | 类型 | 描述
---|:---:|---
anyrtcid | String | 平台开发者信息
userid | String | 用户ID
username | String | 用户名称
usericon | String | 用户头像
usertype | Number | 用户类型

##### 说明
该方法为加入房间，加入房间成功之后可进行后续操作。

#### 7.离开房间

##### 示例

```
anyClient.leaveRoom();
```

##### 说明
该方法为离开房间。

#### 8.预约

##### 示例

```
anyClient.book();
```

##### 说明
预约

#### 9.取消预约

##### 示例

```
anyClient.unbook();
```

##### 说明
取消预约

#### 10.开始游戏

##### 示例

```
anyClient.play();
```

##### 说明
开始游戏

#### 11.取消游戏

##### 示例

```
anyClient.canclePlay();
```

##### 说明
取消游戏。

#### 12.操作控制

##### 示例

```
anyClient.sendControlCmd(type);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
type | Number | 0向上，1向下，2向左，3向右，4抓取，5旋转摄像头

##### 说明
操作控制。

### anyClient 接口类

#### 1.连接娃娃机成功

##### 示例

```
anyClient.on("onConnectServerSuccess", function () { });
```

##### 说明
连接娃娃机成功

#### 2.初始化anyrtc成功

##### 示例

```
anyClient.on("onInitAnyRTCSuccess", function () { });
```

##### 说明
初始化anyrtc成功

#### 3.初始化anyRtc失败

##### 示例

```
anyClient.on("onInitAnyRTCFaild", function () { });
```

##### 说明
初始化anyRtc失败

#### 4.加入房间

##### 示例

```
anyClient.on("onJoinRoom", function (code, videoInfo, memberNum) { });
```
##### 参数
参数名 | 类型 | 描述
---|:---:|---
code | Number | 状态码
videoInfo | String | 视频流信息
memberNum | Number | 房间内人数

##### 说明
加入房间

#### 5.预约结果

##### 示例

```
anyClient.on("onGetRoomList", function () { });
```

##### 说明
预约结果

#### 6.获取房间列表

##### 示例

```
anyClient.on("onGetRoomList", function (roomList) { });
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
roomList | String | 房间列表

##### 说明
获取房间列表

#### 7.预约结果

##### 示例

```
anyClient.on("onBookResult", function (code, BookNum) { });
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
code | Number | 状态码
BookNum | Number | 预约人数

##### 说明
预约结果

#### 8.取消预约结果

##### 示例

```
anyClient.on("onUnBookResult", function (code, BookNum) { });
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
code | Number | 状态码
BookNum | Number | 预约人数

##### 说明
取消预约结果

#### 9.排队人数更新

##### 示例

```
anyClient.on("onBookMemberUpdate", function (bookMember) { });
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
bookMember | Number | 排队人数

##### 说明
排队人数更新

#### 10.指令回掉

##### 示例

```
anyClient.on("onControlCmd", function (code, cmd) { });
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
code | Number | 状态码
cmd | String | 方向 left、right、down、up 

##### 说明
指令回掉

#### 11.准备开始通知

##### 示例

```
anyClient.on("onReadyStart", function () { });
```

##### 说明
准备开始通知

#### 12.抓娃娃结果

##### 示例

```
anyClient.on("onResult", function (result) { });
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
result | Boolean | true为成功, false为失败

##### 说明
抓娃娃结果

#### 13.房间人数更新

##### 示例

```
anyClient.on("onRoomMemberUpdate", function (roomMember) { });

```
##### 参数
参数名 | 类型 | 描述
---|:---:|---
roomMember | Number | 房间人数

##### 说明
房间人数更新

#### 14.抓娃娃超时

##### 示例

```
anyClient.on("onPlayTimeout", function () { });
```

##### 说明
抓娃娃超时了

#### 15.准备超时通知

##### 示例

```
anyClient.on("onReadyTimeout", function () { });
```

##### 说明
准备超时通知

#### 16.娃娃机视频流变化

##### 示例

```
anyClient.on("onRoomUrlUpdate", function (code, data) { });
```
##### 参数
参数名 | 类型 | 描述
---|:---:|---
code | Number | 返回状态码
data | String | 返回错误

##### 说明
娃娃机视频流变化

#### 17.娃娃机离开房间

##### 示例

```
anyClient.on("onWaWaLeave", function () { });
```

##### 说明
娃娃机离开房间

#### 18.与服务断开连接

##### 示例

```
anyClient.on("onDisconnect", function () { });
```

##### 说明
和服务器断开连接，正在尝试重连

#### 19.重连服务

##### 示例

```
anyClient.on("onReconnect", function () { });
```

##### 说明
正在重连中...