# Web屏幕共享

## 一、概述

### 简介

anyRTC为web端提供的捕获屏幕共享SDK。

### 兼容性说明

- 火狐浏览器或谷歌72版本以上的浏览器**无需**安装插件，其他浏览器需要插件支持。
- QQ浏览器、360浏览器、谷歌浏览器低于72版本的需要前往相对应的拓展程序市场下载。[谷歌市场](https://chrome.google.com/webstore/detail/anyrtc-screenshare/daiabbkkhgegdmhfpocaakcgbajnkgbp?hl=zh-CN)、[QQ浏览器市场](https://appcenter.browser.qq.com/search/detail?key=anyrtc&id=daiabbkkhgegdmhfpocaakcgbajnkgbp &title=anyRTC-ScreenShare)、[360 安全浏览器](https://ext.se.360.cn/webstore/detail/ncimcjgppbokkenggioiebjhicflnfmp)

## 二、集成指南

### 导入SDK

**npm 市场**

* 通过 npm市场 下载：

```
npm install ar-share-screen --save-dev

import getScreenStream from "ar-share-screen";
```

* 安装最新版本：

```
npm install ar-share-screen@latest --save-dev

import getScreenStream from 'ar-share-screen';
```

**js 引用**

* 点击[下载 SDK](https://github.com/anyRTC/anyRTC-Meet-web)

* 引用

```
<!-- 先再头部引入样式 -->
<link rel="stylesheet" href="路径/index.css">

<script src="路径/ArShareScreen.js"></script>
```

## 三、API接口文档

### 获取屏幕共享流

**示例**

```
getScreenStream.then(e => {
    if (e === 'no-ready') {
        alert(
        '1. 请检查是否安装"anyRTC-ScreenShare"屏幕共享插件,如果没有请点击https://chrome.google.com/webstore/detail/anyrtc-screenshare/daiabbkkhgegdmhfpocaakcgbajnkgbp?hl=zh-CN下载\n' +
        '2. 安装了屏幕共享插件，但是没有启用该插件。\n' + 
        '说明：\n火狐浏览器或谷歌版本72以上无需安装插件。\n' +
            '360、QQ平台也有对应的插件下次。');
            //结合SDK closeShare
      }
      else if (e === 'no-support') {
            alert(`该浏览器不支持，请选择谷歌、火狐、QQ、360浏览器`);
            //结合SDK closeShare
      }
      else if (e === "user-cancel") {
            alert(`共享被取消`);
            //结合SDK closeShare
      }
        else if (e === "user-Denied") {
            alert(`用户未授权`);
            //结合SDK closeShare
        }
      else {
        //预览屏幕共享流 e.video
            let screenSteam = e.stream;
            //预览
            let screenView = document.createElement('div');
            screenView.id = "myScreen";
            screenView.appendChild(e.video);
            document.body.appendChild(screenView);
            //结合SDK 发布媒体流
      }
    }).catch(err => {
        console.log('error', `获取屏幕共享流失败`);
        //结合SDK closeShare
    throw new Error(err);
});
```

**说明**
获取屏幕共享流。

## 四、更新日志

**Version 3.0.0 （2019-01-18）**

* SDK版本升级3.0，API接口变更，更加简洁规范

**Version 2.0.0 （2017-09-30）**

* SDK版本升级2.0，梳理、完善SDK