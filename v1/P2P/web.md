## 一、快速开始

### 集成指南

#### 适用范围

本集成文档适用于Web 呼叫 ARCall SDK 3.0.0版本。

#### 兼容情况

- Chrome、Firefox、safari 11(以上)等，具体使用[webRTC检测工具](https://docs.anyrtc.io/v1/tools/%E6%B5%8F%E8%A7%88%E5%99%A8WebRTC%E6%A3%80%E6%B5%8B.html)
- H5支持chrome内核。

#### 导入SDK

##### npm 市场

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

- 前往[SDK 下载页面](https://docs.anyrtc.io/download/js/ArCallKit.3.0.17.js)，`ctrl+s`或`command+s`保存到本地
- 引用

```
<script src="yourAssetsPath/ArCallKit.版本.js"></script>
```
### 开发指南
#### 1. 初始化SDK
集成SDK后，还需对SDK在页面进行初始化操作。
##### 1.1 导入头文件
```
import ArCall from 'ar-call';
```
##### 1.2 实例化对象
options为实例配置，包括自定义数据、码率自适应是否开启、日志级别等。
```
let call = new ArCall(options);
```
##### 1.3 监听回调
```
//上线成功回调
call.on("online-success", () => {
    
});
//上线失败回调
//根据code码查询错误原因
call.on("online-failed", (errCode) => {
    
});
//收到通话请求
call.on("make-call", (roomId, peerUserId, callMode, peerUserData, callExtend) => {
    
});
//其他人视频显示窗口
//对方（或己方）同意视频通话请求后，会收到此回调，收到后需要将`mediaRender`添加展示到页面上。
call.on("stream-subscribed", (peerUserId, pubId, rtcUserData, mediaRender) => {
    
});
//其他人离开移除视频显示窗口
call.on("stream-unsubscribed", (peerUserId, pubId, userId, rtcUserData) => {

});
//收到挂断或者下线
call.on("end-call", (peerUserId, errCode) => {

});
```
##### 1.4 配置开发者信息
配置开发者信息，开发者信息可在anyRTC管理后台中获得，详见[创建anyRTC账号](https://docs.anyrtc.io)。
```
//配置开发者信息
call.initAppInfo(APP_ID, APP_TOKEN);

//配置私有云(默认无需配置)
//call.configServer(SERVE_URL);
```
#### 2. 加入房间
##### 2.1 用户上线
方法用于上线，第一参数是用户ID，第二个参数token为令牌，可为空，具体用法可参考[安全指南](https://docs.anyrtc.io/v1/security/服务级安全设置指南.html)。
```
call.turnOn(userId, token);
```
#### 3. 呼叫
##### 3.1 设置本地显示窗口
设置本地显示窗口，参数constraints为音视频配置项，包含视频帧率、码率、相机类型等。
```
call.setLocalVideoCapturer(constraints);
```
##### 3.2 呼叫用户
调用makeCall方法用于呼叫用户<br/>
peerUserId自定义用户ID、SIP外呼请ID前面添加`+86`例如 `+86185****8888`<br/>
callMode为呼叫模式<br/>
callExtend为自定义拓展数据（选填）<br/>
呼叫模式：
`0`视频呼叫、`2`音频呼叫、`20`音频呼叫客服、`21`视频呼叫客服
 ```
call.makeCall(peerUserId, callMode, callExtend);
```
##### 3.3 通话操作
peerUserId为自定义用户ID。
```
//同意呼叫
call.acceptCall(peerUserId);
//拒绝接听
call.rejectCall(peerUserId);
//结束通话
call.endCall(peerUserId);
```
##### 3.4用户下线
退出。
```
call.turnOff();
```
## 二、API接口文档
### ArCall 接口
#### 初始化实例

##### 示例

```
let call = new ArCall(Options: Object);
```

##### 参数

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

##### 示例

```
call.initAppInfo(appId: string, apptoken: string);
```

##### 参数

| 参数名   |  类型  | 描述      |
| -------- | :----: | --------- |
| appId    | string | 应用ID    |
| apptoken | string | 应用token |

##### 说明

方法配置开发者信息，开发者信息可在anyRTC管理后台中获得，详见[创建anyRTC账号](https://docs.anyrtc.io)。

##### 配置服务地址

##### 示例

```
call.configServer(url);
```

##### 参数

| 参数名 |  类型  | 描述                            |
| ------ | :----: | ------------------------------- |
| url    | string | 服务地址，例如: `www.baidu.com` |

##### 说明

配置服务器地址，私有云需要配置，否则无需配置（如果网站是`HTPPS` 环境不支持IP）。

#### 获取SDK版本号

##### 示例

```
call.getSDKVersion();
```

##### 返回值

获取ARCall SDK版本号

#### 获取媒体设备

##### 示例

```
call.getDevices(deviceType);
```

##### 参数

| 参数名      |  类型  | 描述                             |
| ----------- | :----: | -------------------------------- |
| deviceType  | string | 媒体设备类型`videoinput`、`audioinput`、`audiooutput`，分别是摄像头、麦克风、扬声器 |

##### 返回值

`Promise`对象

`Promise.then` 返回的参数data对象，包含所查询的设备列表，如果deviceType为空则返回videoinput、videooutput、audioinput三个类型的设备列表

##### 说明

获取设备列表，根据设备列表的id可以切换指定摄像头以及指定扬声器作为音频输出设备。

#### 采集本地视频窗口

##### 示例

```
call.setLocalVideoCapturer(constraints);
```

##### 参数

| 参数名      |  类型  | 描述                   |
| ----------- | :----: | ---------------------- |
| constraints | object | 采集媒体配置（非必填） |

**constraints**

| 参数名 |  类型  | 描述                                                         |
| ------ | :----: | ------------------------------------------------------------ |
| video  | object | `enable`是否打开摄像头<br />`deviceId`指定设备，`deviceId` 通过`getDevices` 接口获取 |
| audio  | object | `enable`是否打开摄像头<br />`deviceId`指定设备，`deviceId` 通过`getDevices` 接口获取 |

##### 说明

返回`Promise`对象。

#### 切换媒体输入设备

##### 示例

```
call.switchMediaInputDevice(constraints);
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

##### 返回值

`Promise`对象

`Promise.then` 返回的参数data

| data        |      描述      |
| ----------- | :------------: |
| mediaStream |  MediaStream   |
| mediaRender | HTMLDivElement |

##### 说明

切换设备输入设备（麦克风或摄像头），预览获取新的媒体流之后移除旧的预览窗口。

#### 设置坐席身份

##### 示例

```
call.setAsClerk(strArea, strCallId, strBusiness, nLevel);
```

##### 参数

| 参数名      |  类型  | 描述               |
| ----------- | :----: | ------------------ |
| strArea     | string | 自定义区域         |
| strCallId   | string | 自定义坐席频道ID   |
| strBusiness | string | 自定义业务类型     |
| nLevel      | number | 自定义坐席服务级别 |

##### 说明

设置坐席身份，需要在`turnOn`调用之前设置。

#### 用户上线（登录）

##### 示例

```
call.turnOn(userId， userToken);
```

##### 参数

| 参数名    |  类型  | 描述                                   |
| --------- | :----: | -------------------------------------- |
| userId    | string | 自定义用户ID                           |
| userToken | string | 第三方认证所需要携带的userToken（非必填） |

##### 说明

用户上线，如果后台开启了[服务级安全设置]([https://docs.anyrtc.io/v1/security/%E6%9C%8D%E5%8A%A1%E7%BA%A7%E5%AE%89%E5%85%A8%E8%AE%BE%E7%BD%AE%E6%8C%87%E5%8D%97.html](https://docs.anyrtc.io/v1/security/服务级安全设置指南.html))，将会对userToken进行认证。

#### 用户下线（退出）

##### 示例

```
call.turnOff();
```

#### 发起呼叫请求

##### 示例

```
call.makeCall(peerUserId, callMode, userData, callExtend);
```

##### 参数

| 参数名     |  类型  | 描述                                                         |
| ---------- | :----: | ------------------------------------------------------------ |
| peerUserId | string | 自定义用户ID、SIP外呼请ID前面添加`+86`例如 `+86185****8888`     |
| callMode   | number | 呼叫模式: `0`  视频呼叫`、2`音频呼叫、 `20`音频呼叫客服 、 `21`视频呼叫客服 |
| ~~userData~~   | ~~string~~ | ~~自定义用户数据。~~ 3.0.16版本已移除 |
| callExtend | object | 自定义拓展数据（选填）                                       |

##### 说明

发起呼叫，被叫端会收到`make-call`回调。需要注意以下几点：
1. 3.0.16版本移除了`userData`参数，因为与初始化实例时重复定义（参考new ArCall）
2. 在`online-success`成功之后才可呼叫，否则可能会抛异常（未连接服务就发送消息）
3. SIP外呼仅支持音频呼叫，也就是callMode为`2`时才可呼叫；

#### 同意呼叫请求

##### 示例

```
call.acceptCall(peerUserId);
```

##### 参数

| 参数名     |  类型  | 描述         |
| ---------- | :----: | ------------ |
| peerUserId | string | 自定义用户ID |

##### 说明

同意呼叫请求，呼叫通道建立；

#### 拒绝呼叫请求

##### 示例

```
call.rejectCall(peerUserId);
```

##### 参数

| 参数名     |  类型  | 描述         |
| ---------- | :----: | ------------ |
| peerUserId | string | 自定义用户ID |

##### 说明

拒绝呼叫请求；

#### 结束/取消呼叫请求

##### 示例

```
call.endCall(peerUserId);
```

##### 参数

| 参数名     |  类型  | 描述         |
| ---------- | :----: | ------------ |
| peerUserId | string | 自定义用户ID |

##### 说明

结束或取消呼叫请求；

#### 设置本地音频是否传输

##### 示例

```
call.setLocalAudioEnable(enable);
```

##### 参数

| 参数名 |  类型   | 描述                                        |
| ------ | :-----: | ------------------------------------------- |
| enable | boolean | `true`传输音频，`false`不传输音频，默认传输 |

#### 设置本地视频是否传输

##### 示例

```
call.setLocalVideoEnable(enable);
```

##### 参数

| 参数名 | 类型 | 描述                                        |
| ------ | :--: | ------------------------------------------- |
| enable | boolean | `true`传输视频，`false`不传输视频，默认传输 |

#### 获取本地音频传输是否打开

##### 示例

```
call.getLocalAudioEnable();
```

##### 返回

音频是否传输。 

#### 获取本地视频传输是否打开

##### 示例

```
call.getLocalVideoEnable();
```

##### 返回

视频是否传输。 

#### 发送实时消息

##### 示例

```
call.sendUserMessage(peerUserId, msgContent);
```

##### 参数

| 参数名 | 类型 | 描述                                        |
| ------ | :--: | ------------------------------------------- |
| peerUserId | string | 自定义用户ID |
| msgContent | string | 自定义消息内容，json字符串 |

##### 说明

发送实时消息。 


### ARCall 回调接口

#### 用户上线成功

##### 示例

```
call.on("online-success", () => {

});
```

##### 说明

用户上线成功

#### 用户上线失败

##### 示例

```
call.on("online-failed", (errCode) => {

});
```

##### 参数

| 参数名  |  类型  | 描述           |
| ------- | :----: | -------------- |
| errCode | number | 上线失败错误码 |


#### 进入房间成功

##### 示例

```
call.on("join-room-success", (roomId) => {

});
```

##### 参数

| 参数名  |  类型  | 描述           |
| ------- | :----: | -------------- |
| roomId | string | 房间ID |

##### 说明
该回调仅在开发者后台开启录像功能之后才回调，进入房间成功之后才会开始录像（手动录像、服务端自动录像）。

#### 被呼叫

##### 示例

```
call.on("make-call", (roomId, peerUserId, callMode, peerUserData，callExtend) => {

});
```

##### 参数

| 参数名       |  类型  | 描述                                 |
| ------------ | :----: | ------------------------------------ |
| roomId       | string | 房间ID                               |
| peerUserId   | string | 呼叫者的userId                       |
| callMode     | number | 呼叫模式：`0` 视频呼叫、`2` 音频呼叫 |
| peerUserData | string | 呼叫者的userData                     |
| callExtend   | string | `makeCall`携带的自定义拓展数据       |

##### 说明

当收到远端呼叫时，将会收到此回调，如果同意通话采集本地视频窗口(`setLocalVideoCapturer`)之后再调用`acceptCall` ，否则`rejectCall` 。

#### 被叫同意通话请求

##### 示例

```
call.on("accept-call", (peerUserId) => {

});
```

##### 参数

| 参数名     |  类型  | 描述                 |
| ---------- | :----: | -------------------- |
| peerUserId | string | 呼叫者的自定义userId |

##### 说明

发起呼叫请求`makeCall` ，对方收到`make-call` 回调，并同意`acceptCall`，会收到此回调。

#### 被叫拒绝通话请求

##### 示例

```
call.on("reject-call", (peerUserId, errCode) => {

});
```

##### 参数

| 参数名     |  类型  | 描述                 |
| ---------- | :----: | -------------------- |
| peerUserId | string | 呼叫者的自定义userId |
| errCode | number | 拒绝呼叫的错误码 |

##### 说明

发起呼叫请求`makeCall` ，对方收到`make-call` 回调，并拒绝`rejectCall`，会收到此回调。

#### 通话结束

##### 示例

```
call.on("end-call", (peerUserId, errCode) => {

});
```

##### 参数

| 参数名     |  类型  | 描述                          |
| ---------- | :----: | ----------------------------- |
| peerUserId | string | 对方的自定义userId            |
| errCode    | string | 挂断的错误码。`0`为正常挂断。 |

##### 说明

对方挂断通话`endCall` 或者对方下线`turnOff`。

#### 坐席上线之后实时状态

##### 示例

```
call.on("clerk-cti-status", (queueNum, AllClerk, WorkingClerk) => {

});
```

##### 参数

| 参数名     |  类型  | 描述                          |
| ---------- | :----: | ----------------------------- |
| queueNum   | number | 当前排队有多少人            |
| AllClerk   | number | 所有坐席人数 |
| WorkingClerk    | number | 正在工作的坐席人数 |

##### 说明

坐席上线之后，坐席实时状态回调，仅坐席收到该回调。

#### 用户上线之后实时状态

##### 示例

```
call.on("user-cti-status", (queueNum) => {

});
```

##### 参数

| 参数名     |  类型  | 描述                          |
| ---------- | :----: | ----------------------------- |
| queueNum   | number | 前面还有多少人在排队            |

##### 说明

用户发起`makeCall`之后，排队实时状态回调，直到坐席接收或拒绝通话。

#### 打开远程视频窗口

##### 示例

```
call.on("stream-subscribed", (peerUserId, renderId, peerUserData, render) => {

});
```

##### 参数

| 参数名       |      类型      | 描述                 |
| ------------ | :------------: | -------------------- |
| peerUserId   |     string     | 对方的自定义userId   |
| renderId     |     string     | 视频窗口标识ID       |
| peerUserData |     string     | 对方的自定义userData |
| render       | HTMLDIVElement | 媒体窗口             |

##### 说明

对方（或己方）同意视频通话请求后，会收到此回调，收到回调需要将render添加展示到页面上。

#### 移除远程视频窗口

##### 示例

```
call.on("stream-unsubscribed", (peerUserId, renderId, peerUserData) => {

});
```

##### 参数

| 参数名       |  类型  | 描述                 |
| ------------ | :----: | -------------------- |
| peerUserId   | string | 对方的自定义userId   |
| renderId     | string | 视频窗口标识ID       |
| peerUserData | string | 对方的自定义userData |

##### 说明

对方（或己方）同意视频通话请求后挂断通话，会收到此回调，收到回调需要将render从页面上移除。

#### 收到实时消息

##### 示例

```
call.on("user-message", (peerUserId, msgContent) => {

});
```

##### 参数

| 参数名 | 类型 | 描述                                        |
| ------ | :--: | ------------------------------------------- |
| peerUserId | string | 自定义用户ID |
| msgContent | string | 自定义消息内容，json字符串 |

##### 说明

接收到用户发送的实时消息。 

## 三、更新日志

**Version 3.0.17 （2019-08-08）**

- 修复排队优先级功能无效
- 添加`join-room-success`回调

**Version 3.0.16 （2019-07-16）**

- `makeCall`去掉第三个参数`userData`，与初始化中的userData冲突
- 消息通道优化
- 用户下线优化

**Version 3.0.15 （2019-06-12）**

- 优化兼容性问题，解决Android浏览器不能现实图像的问题

**Version 3.0.12 （2019-06-12）**

- 解决移动网络不互通的BUG
- 优化

**Version 3.0.9 （2019-06-06）**

- 修复客户挂断通话之后坐席窗口黑屏的bug
- 修复P2P VIP通道web被叫通话接不通的bug

**Version 3.0.7 （2019-06-05）**

- 优化排队机制

**Version 3.0.4 （2019-05-28）**

- SDK将方法`switchDevice`更名`switchMediaInputDevice`，同时同步更新文档

**Version 3.0.2 （2019-05-27）**

- 更新文档，文档添加`getDevices`和`switchDevice`的介绍

- 修复远程图像显示不全的问题

**Version 3.0.0 （2019-01-18）**

- SDK版本升级3.0，API接口变更，更加简洁规范
