## 一、概述

### 简介

anyRTC提供对会议场景的支持，RTCMeeting SDK，高清流畅的音视频、高安全性、全平台运行、丰富的会议管理功能，支持视频、语音多人会议，适用于会议、培训、互动等多人移动会议。

### Demo体验

请根据需求选择渠道安装，安装完RTMeeting Demo后，可体验在线会议功能，以下Demo与小程序互通。

- [iOS Demo下载](https://www.pgyer.com/xoTQ)

- [Android Demo下载](http://www.pgyer.com/eU0U)

- [Web Demo 体验](https://demos.anyrtc.io/ar-meet/)

### 源码GitHub

源码仅供开发者参考，适用于SDK调试，便于快速集成。

- [iOS Demo 源码下载](https://github.com/anyRTC/anyRTC-Meeting-iOS)

- [Android Demo 源码下载](https://github.com/anyRTC/anyRTC-Meeting-Android)

- [Web Demo 源码下载](https://github.com/anyRTC/anyRTC-Meeting-Web)

- [小程序 Demo 源码下载](https://github.com/anyRTC/anyRTC-Miniprogram)

## 一、集成指南

### 公众平台小程序配置

#### 设置小程序服务类别
登录[公众号平台](https://mp.weixin.qq.com)，打开设置-基本设置-服务类目-详情-添加服务类目，目前小程序实时音视频支持这些[服务类目](https://developers.weixin.qq.com/miniprogram/dev/component/live-pusher.html)，请选择其中一种。
![设置小程序服务类别](/assets/images/web/config_service.png)

#### 开启实时音视频权限
登录[公众号平台](https://mp.weixin.qq.com)，打开开发-接口设置，开启实时播放音视频流并开启实时录制音视频流。
![设置小程序服务类别](/assets/images/web/config_api_promise.png)

#### 配置服务域名
登录[公众号平台](https://mp.weixin.qq.com)，打开开发-开发设置-服务器域名-修改，配置服务域名白名单。
![设置小程序服务类别](/assets/images/web/config_domain.png)

#### npm构建
打开开发者工具，打开设置-项目设置-勾选'使用npm模块'，npm安装`moniprogram-ar-meet`、`moniprogram-ar-push`、`moniprogram-ar-play`之后，点击工具-构建npm。

#### 兼容情况
微信小程序基础库需要大于`1.7.0`，低版本需做兼容处理。

#### 真机调试
开发者工具模拟器除特殊版本之外，不支持实时音视频功能，请使用真机调试。

### 导入SDK

##### npm 市场

- 通过 [npm市场](https://www.npmjs.com/package/miniprogram-ar-meet) 下载：

```
npm install miniprogram-ar-meet@latest --save-dev

import wxRTMeet from 'miniprogram-ar-meet';
```

## 三、API接口文档

请点击[查看](https://www.npmjs.com/package/miniprogram-ar-meet)

## 四、更新日志

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
