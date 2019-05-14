## 一、概述

### 简介

anyRTC RTC 检测工具提供以下检测功能：

- 浏览器webRTC检测
- 获取浏览器信息、版本、以及操作系统
- 分辨率检测
- 音频大小检测
- 获取设备信息
- 媒体视频窗口截图等等

### Dome体验

检测工具为您的产品带来更好的用户体验。

- [Web Demo 体验](https://demos.anyrtc.io/ar-detect/)	

#### 源码GitHub		

源码仅供开发者参考，适用于SDK调试，便于快速集成。		

- [Web Demo 源码下载](https://github.com/anyRTC/anyRTC-Detect-Web)		

## 二、集成指南		

#### 适用范围		

本集成文档适用于Web ArDetect SDK 3.0.0及以上版本。

#### 准备环境

支持 Chrome、IE 10 +、Firefox、Safari 等主流浏览器环境

#### 导入SDK	

##### npm 市场

- 通过 [npm市场](https://www.npmjs.com/package/ar-detect) 下载：

```
npm install ar-detect --save-dev

import ArDetect from 'ar-detect';
```

- 安装(更新)最新的版本：

```
npm install ar-detect@latest --save-dev

import ArDetect from 'ar-detect';
```

##### js 引用

- 点击[下载 SDK](https://github.com/anyRTC/anyRTC-Detect-web)
- 引用

```
<script src="yourAssetsPath/ArDetect.js"></script>
```

## 三、API接口文档

#### ArDetect对象简介

##### 示例

```
ArDetect = {
	browserInfo,//浏览器信息
	OSName,//操作系统信息
	checkRTCSupport(),//检测浏览器是否支持webRTC
	...
};
```

#### 检测浏览器是否支持webRTC

##### 示例

```
ArDetect.checkRTCSupport();
```

**返回值**

`true` or `false`

#### 获取设备列表

```
ArDetect.getDevices(deviceType).then(data => {
	console.log(data);
}).catch(err => {
	console.log('getDevice error', err);
})
```

##### 参数

| 参数名     |  类型  | 描述                                                         |
| ---------- | :----: | ------------------------------------------------------------ |
| deviceType | string | 设备类型（非必选）：'videoinput' 摄像头、'audioinput'麦克风、'audiooutput'扬声器 |

#### 检测分辨率是否支持

##### 示例

```
ArDetect.checkResolution(resolutionOptions); 
```

##### 参数

| 参数名            | 类型   | 描述                                  |
| ----------------- | ------ | ------------------------------------- |
| resolutionOptions | object | `width`视频宽度<br />`height`视频高度 |

##### 说明

检测本地摄像头是否支持采集设定的分辨率。

##### 返回值

`Promise`对象。

#### 创建本地预览流（打开摄像头）

##### 示例

```
ArDetect.createStream(constraints)
	.then(e => {
		//do something
	})
	.catch(err => {

	});
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

#### 释放采集设备

##### 示例

```
ArDetect.releaseStream();
```

#### 检测媒体流音频

##### 示例

```
let detectContext = ArDetect.detectMediaStreamVolum(stream, callback);
```

##### 参数

| 参数名   |    类型     | 描述                                                |
| -------- | :---------: | --------------------------------------------------- |
| stream   | mediaStream | 媒体流对象，例如：`createStream`返回的媒体流        |
| callback |  function   | 获取媒体流音频大小成功的回调。参数为音频大小`0~100` |

##### 说明

获取指定媒体流的音频大小。

##### 返回值

检测音频大小的上下文对象`detectContext`，结束音频检测时传入。

#### 取消媒体流音频检测

##### 示例

```
ArDetect.endDetectMediaStreamVolume(detectContext);
```

##### 参数

| 参数名       | 类型    | 描述                                       |
| ------------ | ------- | ------------------------------------------ |
| audioContext | object | `detectMediaStreamVolum`时返回的上下文对象 |

##### 说明

该方法需要在`detectMediaStreamVolum`调用之后才能调用。

#### 选择音频输出设备

##### 示例

```
ArDetect.attachSinkId(element, sinkId);
```

##### 参数

| 参数名       | 类型    | 描述                                       |
| ------------ | ------- | ------------------------------------------ |
| element | element | `audio`DOM对象 |
| sinkId | string | 扬声器的设备id(`audiooutput`)， 通过getDeivces('audiooutput')获得 |

##### 说明

获得设备列表，选择指定设备播放音频。

##### 返回值

`Promise`

#### 捕捉视频窗口截图

##### 示例

```
ArDetect.takeMediaStreamSnapshot(videoElement, fileName);
```

##### 参数

| 参数名   | 类型    | 描述                         |
| -------- | ------- | ---------------------------- |
| enable   | element | `video`DOM对象               |
| fileName | string  | 文件名，下载到本地的文件名称 |

## 四、更新日志

**Version 3.0.0 （2019-01-24）**

- SDK版本升级3.0，API接口变更，更加简洁规范

**Version 2.0.0 （2017-09-30）**

- DK版本升级2.0，梳理、完善SDK

## 