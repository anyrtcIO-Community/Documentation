## 一、快速开始

### 集成指南

#### 适用范围		

本集成文档适用于Web ARRtcpKit SDK 3.0.0及以上版本。

#### 兼容情况

- Chrome、Firefox、safari 11(以上)等，具体使用[webRTC检测工具](https://docs.anyrtc.io/v1/tools/%E6%B5%8F%E8%A7%88%E5%99%A8WebRTC%E6%A3%80%E6%B5%8B.html)
- H5支持chrome内核。

#### 准备环境

支持 Chrome、IE 10 +、Firefox、Safari 等主流浏览器环境

#### 导入SDK	

##### npm 市场

- 通过 [npm市场](https://www.npmjs.com/package/ar-rtcp) 下载：

```
npm install ar-rtcp --save-dev

import ArRTCPKit from 'ar-rtcp';
```

- 安装或更新至最新版本：

```
npm install ar-rtcp@latest --save-dev

import ArRTCPKit from 'ar-rtcp';
```

##### js 引用

- 前往[SDK 下载页面](https://docs.anyrtc.io/download/js/ArRtcpKit.3.0.10.js)，`ctrl+s`或`command+s`保存到本地
- 引用

```
<script src="yourAssetsPath/ArRtcpKit.版本号.js"></script>
```

### 开发指南
#### 1. 初始化SDK
集成SDK后，还需对SDK在页面进行初始化操作。
##### 1.1 导入头文件
```
import ArRTCPKit from 'ar-rtcp';
```
##### 1.2 实例化对象
```
let options = {};
let rtcp = new ArRTCPKit (options);
```

##### 1.3 监听回调

```
//加入频道成功
rtcp.on("join-success", () =>{

);
//加入频道失败
//根据code码查询错误原因
rtcp.on("join-failed", (code) => {
  
});

//收到远程视频
rtcp.on("stream-subscribed", (rtcpID, mediaRender) =>{

});
//收到辅流视频
rtcp.on("exstream-subscribed", (rtcpID, mediaRender) =>{

});
```

##### 1.4 配置开发者信息
配置开发者信息，开发者信息可在anyRTC管理后台中获得，详见[创建anyRTC账号](https://docs.anyrtc.io)。
```
//配置开发者信息
rtcp.initAppInfo(APP_ID, APP_TOKEN);

//配置私有云(默认无需配置)
//rtcp.configServer(SERVE_URL);
```
##### 2.设置本地显示窗口
设置本地显示窗口，参数constraints为音视频配置项，包含视频帧率、码率、相机类型等。
```
rtcp.setLocalVideoCapturer({
  video: {
    enabled: true,
    deviceId: ""
  },
  audio: {
    enabled: true,
    deviceId: ""
  }
}).then(e => {
  //将视频e.mediaRender绑定到页面
  //可以在这里发布视频
}).catch(err => {
   console.log(err);
});

```
##### 3.视频流操作
`mediaType`媒体类型：0 视音频，1音频<br />
`stream`辅流视频<br />
`rtcpID`系统分配的频道ID<br />
`mediaRender`视频对象<br />
```
//发布视频流
rtcp.publish(mediaType);
//取消发布
rtcp.unPublishEx();
//发布屏幕辅流(屏幕共享)
rtcp.publishEx(stream)
//取消发布屏幕辅流(屏幕共享)
rtcp.unPublishEx();
//订阅
rtcp.unSubscribe(rtcpID)
//取消订阅
rtcp.unSubscribe();
```
##### 4.退出
离开房间。
```
rtcp.close();
```
## 二、API接口文档

### ArRtcpKit 接口

#### 1. 实例化对象

```
var rtcp = new ArRTCPKit(Options);
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
### ArRtcpKit 接口类

#### 1. 获取媒体设备

##### 示例

```
rtcp.getDevices(deviceType);
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
rtcp.switchDevice(constraints);
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
rtcp.publish(mediaType, roomId);
```

##### 参数

| 参数名    | 类型   | 描述                          |
| --------- | ------ | ----------------------------- |
| mediaType | Number | 媒体类型：`0` 视音频，`1`音频 |
| roomId | string | 房间ID，用于标识 |

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

#### 14. 监听某个房间的媒体流变化 NEW

##### 示例

```
rtcp.listen(roomId);
```

##### 参数

| 参数名 | 类型   | 描述             |
| ------ | ------ | ---------------- |
| roomId | String | 媒体流发布书，指定的roomId |

##### 说明

监听某个房间的媒体流的发布与取消发布。

#### 15. 取消监听某个房间的媒体流变化 NEW

##### 示例

```
rtcp.unListen(roomId);
```

##### 参数

| 参数名 | 类型   | 描述             |
| ------ | ------ | ---------------- |
| roomId | String | 媒体流发布书，指定的roomId |

##### 说明

取消监听某个房间的媒体流的发布与取消发布。

#### 16. 结束直播

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
rtcp.on("exstream-subscribed", function(pubId, mediaRender){

});
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
rtcp.on("exstream-unsubscribed", function(pubId){

});
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
rtcp.on("stream-subscribed", function(pubId, mediaRender){

});
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
rtcp.on("stream-unsubscribed", function(pubId){

});
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
rtcp.on("audio-volume", function(isRemote, pubId, audioLevel){

});
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
rtcp.on("network-status", function(isRemote, pubId, videoBytes, ARNetQuality){

});
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
rtcp.on("join-success", function(){

});
```

#### 8. 加入频道失败

##### 示例

```
rtcp.on("join-failed", function(code){

});
```

##### 参数

| 参数名 | 类型   | 描述                   |
| ------ | ------ | ---------------------- |
| code   | Number | 错误码，详情查看错误码 |

#### 11. 发布媒体流成功

##### 示例

```
rtcp.on("stream-published", function(pubId, roomId){

});
```

##### 参数

| 参数名 | 类型   | 描述                          |
| ------ | ------ | ----------------------------- |
| pubId  | String | RTC服务生成媒体流的唯一标识ID |
| roomId  | String | 自定义频道ID,可通过listen方法，监听房间内所有的（实时）视频流变化。 |

##### 说明

发布媒体流成功，返回roomId和pubId。

#### 12. 发布媒体流失败

##### 示例

```
rtcp.on("stream-publish-failed", function(){

});
```

发布媒体流失败。

#### 13. 辅流发布成功

##### 示例

```
rtcp.on("exstream-published", function(pubId){

});
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
rtcp.on("server-disconnect", function(){

});
```

##### 说明

服务器断开连接。

## 三、更新日志

**Version 3.0.9 （2019-09-11）**

- `stream-published`添加roomId回调参数
- 更新文档

**Version 3.0.7 （2019-08-13）**

- `publish`添加参数`roomId`
- 添加方法`listen`和`unListen`

**Version 3.0.5 （2019-06-21）**

- 优化

**Version 3.0.4 （2019-05-27）**

- 更新文档，文档添加`getDevices`和`switchDevice`的介绍

- 修复远程图像显示不全的问题

**Version 3.0.0 （2019-01-24）**

- SDK版本升级3.0，API接口变更，更加简洁规范