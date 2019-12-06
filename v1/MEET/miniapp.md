# 实时视频会议小程序SDK

集成实时视频会议小程序SDK为小程序应用提供多人音视频通话能力，可实现一对一单聊和多人群聊，可运用于 社交、会议、在线教育、培训等场景。基于webRTC技术的超低延时音视频通讯解决方案 。几行代码就可赋予小程序实时音视频的能力。

### DEMO源码

[获取源码]( https://github.com/anyRTC/anyRTC-Miniprogram )

### 准备工作

在开始集成SDK之前，我们需要对小程序以及项目进行一些配置，大致分为：微信公众平台设置，小程序项目配置。

#### 微信公众平台设置

- [设置小程序服务类别](#miniAppType)
- [开启实时音视频权限](#miniAppAVSetting)
- [配置服务域名](#miniAppSafeDomain)

#### 小程序项目配置

- [安装SDK](#installSDK)
- [构建npm](#buildSDK)
- [选择依赖库](#selectLibrary)

#### 说明

- [注意事项](#tips)



<h4 href="#miniAppType">设置小程序服务类别</h4>
登录[微信公众号平台](https://mp.weixin.qq.com)，打开设置 -> 基本设置 -> 服务类目 -> 详情 -> 添加服务类目，查找小程序实时音视频支持的[服务类目](https://developers.weixin.qq.com/miniprogram/dev/component/live-pusher.html)，并按要求设置小程序服务类目。

![设置小程序服务类别](https://docs.anyrtc.io/assets/images/web/config_service.png)

<h4 href="#miniAppAVSetting">开启实时音视频权限</h4>
登录[微信公众号平台](https://mp.weixin.qq.com)，打开开发 -> 接口设置，开启实时播放音视频流并开启实时录制音视频流。

![设置小程序服务类别](https://docs.anyrtc.io/assets/images/web/config_api_promise.png)

<h4 href="#miniAppSafeDomain">配置服务域名</h4>
登录[微信公众号平台](https://mp.weixin.qq.com)，打开开发 -> 开发设置 -> 服务器域名 -> 修改，配置服务域名白名单。

![设置小程序服务类别](https://docs.anyrtc.io/assets/images/web/config_domain.png)

<h4 href="#installSDK">安装SDK</h4>
安装 `miniprogram-ar-meet` SDK，在根目录下面执行`npm install miniprogram-ar-meet` 

<h4 href="#buildSDK">构建npm</h4>
打开微信开发者工具，打开设置 -> 项目设置 -> 勾选'使用npm模块'，npm安装`moniprogram-ar-meet`之后，点击工具 -> 构建npm，具体详情可查阅[官方 npm 文档](https://developers.weixin.qq.com/miniprogram/dev/devtools/npm.html)。

<h4 href="#selectLibrary">选择依赖库</h4>
打开微信开发者工具，打开设置 -> 项目设置 -> 勾选'使用npm模块'，SDK需要依赖小程序基础库 2.2.1 及以上版本。

![image-20191121143725728](https://docs.anyrtc.io/assets/images/web/config_IDE.png)



<h4 ref="#tips">注意事项</h4>
- **兼容情况**

  微信小程序基础库需要大于`1.7.0`，低版本需做兼容处理。

- **真机调试**

  开发者工具模拟器除特殊版本之外，不支持实时音视频功能，请使用真机调试。

- **live-player 和 live-pusher 组件**

  分别把 `live-player` 组件和 `live-pusher` 组件的 `mode` 属性设置为 `RTC`。如果需要开关摄像头或禁用麦克风，请参考 [推流API]( https://developers.weixin.qq.com/miniprogram/dev/api/media/live/wx.createLivePusherContext.html ) 或 [拉流API]( https://developers.weixin.qq.com/miniprogram/dev/api/media/live/wx.createLivePlayerContext.html ) 。



# 快速集成

1. 安装 SDK  

```
npm install --save miniprogram-ar-meet
```

2. 导入SDK

```
//导入SDK

import wxRTMeet from "miniprogram-ar-meet";
```

3. 会议初始化

```
//导入SDK
import wxRTMeet from "miniprogram-ar-meet";

//创建实例
let wxmeet = new wxRTMeet();
that.setData({
  wxmeet: wxmeet
});

//设置用户验证token(用于第三方认证，没有可以跳过)
wxmeet.setUserToken("");
//配置开发者应用信息
wxmeet.initAppInfo(config.APP_ID, config.APP_TOKEN);

```

4. 加入房间

```
let roomId = "123";
let userId = "6666";
let userName = "SuperMan";
let userData = JSON.stringify({ userId, userName });

//加入房间
wxmeet.joinRoom(roomId, userId, userName, userData, that.data.enableVideo, that.data.enableAudio);

//加入房间成功回调 - 监听一次即可
wxmeet.on("onJoinRoomOK", () => {
  wx.showToast({
    title: '加入房间成功',
  });
});

//加入房间失败回调
wxmeet.on("onJoinRoomFaild", (code, info) => {
  console.log("加入房间失败：", code, info ? info : "");
});
```

5. 收到推流地址

```
//收到推流URL回调，将URL绑定到live-pusher组件中，其他人将收到自己的视频
wxmeet.on("onGetPushUrl", (code, data) => {
  console.log('onGetPushUrl', code, data);
  if (code === 0) {//成功
    that.setData({
      pushURL: data.pushURL
    });
  } else {
    wx.showToast({
      icon: 'none',
      title: '获取房间签名失败',
    });
  }
});
```

6. 退出房间

```
wxmeet.leaveRoom();
```

# API说明

### 1. 设置第三方userToken验证

**示例**

```
wxmeet.setUserToken();
```

**参数**

| 参数名    | 类型   | 描述                                                         |
| --------- | ------ | ------------------------------------------------------------ |
| userToken | String | 如果配置了第三方授权认证，SDK登录服务器的时候会将userToken发送到授权服务器，如果授权成功才能登录成功。 |



### 2.初始化开发者信息

**示例**

```
wxmeet.initAppInfo(config.APP_ID, config.APP_TOKEN);
```

| 参数名   | 类型   | 描述      |
| -------- | ------ | --------- |
| appId    | String | 应用ID    |
| apptoken | String | 应用token |



### 3.加入房间

**示例**

```
wxmeet.joinRoom(roomId, userId, userName, userData, enableVideo, enableAudio);
```

| 参数名      | 类型    | 描述           |
| ----------- | ------- | -------------- |
| roomId      | String  | 房间ID         |
| userId      | String  | 用户ID         |
| userName    | String  | 用户昵称       |
| userData    | String  | 用户自定义信息 |
| enableVideo | Boolean | 是否开启摄像头 |
| enableAudio | Boolean | 是否开启麦克风 |

### 4.加入房间成功回调

**示例**

```
wxmeet.on("onJoinRoomOK", () => {
	console.log("加入房间成功");
});
```

### 5.加入房间失败回调

**示例**

```
wxmeet.on("onJoinRoomFaild", (code, info) => {
	console.log("onJoinRoomFaild", code, info);
});
```

**参数**

| 参数名 | 类型           | 描述     |
| ------ | -------------- | -------- |
| code   | Number         | 错误码   |
| info   | String\|Object | 错误信息 |



### 6.远程人员加入房间

**示例**

```
wxmeet.on("onMemberJoin", (rtmpUrl, pubId, userId, rtcUserData) => {
	console.log("onMemberJoin", rtmpUrl, pubId, userId, rtcUserData);
});
```

**参数**

| 参数名      | 类型   | 描述                                              |
| ----------- | ------ | ------------------------------------------------- |
| rtmpUrl     | String | 远程人员的视频流，使用 `live-player` 组件进行播放 |
| pubId       | String | 媒体流标识ID                                      |
| userId      | String | 远程人员用户ID                                    |
| rtcUserData | String | 远程人员自定义用户数据                            |

### 7.远程人员离开房间

**示例**

```
wxmeet.on("onMemberLeave", (pubId) => {
	console.log("onMemberLeave", pubId);
});
```

### 8.获取推流地址成功

**示例**

```
wxmeet.on("onGetPushUrl", (code, data) => {
	console.log("onGetPushUrl", code, data);
});
```

**参数**

| 参数名 | 类型   | 描述                                                         |
| ------ | ------ | ------------------------------------------------------------ |
| code   | Number | 获取推流地址错误码                                           |
| data   | Object | 获取推流地址信息，解析之后将获取到的 `pushURL` 放到推流组件 `live-pusher` 中进行推流 |

### 9.被踢出会议回调

**示例**

```
wxmeet.on("onLeaveMeet", (code, info) => {
	console.log("onLeaveMeet", code, info);
});
```

**参数**

| 参数名 | 类型   | 描述                 |
| ------ | ------ | -------------------- |
| code   | Number | 被踢出会议的错误码   |
| info   | Object | 被踢出会议的原因信息 |