# iOS

## 一、概述

### 简介

anyRTC提供在线抓娃娃机场景的支持，RTCWaWaJiEngine SDK 提供极简接口，实现多平台快速接入。

### Demo体验

请根据需求选择渠道安装，安装完娃娃机 Demo后，可体验在线实时抓娃娃功能。

- [iOS Demo下载](https://www.pgyer.com/fYGm)

- [Android Demo下载](https://www.pgyer.com/3blO)

- [Web Demo体验](http://wawaji.anyrtc.cc/)

### 源码GitHub

源码仅供开发者参考，适用于SDK调试，便于快速集成。

- [iOS Demo 源码下载](https://github.com/anyRTC/anyRTC-WaWa-Client-iOS)

- [Android Demo 源码下载](https://github.com/anyRTC/anyRTC-WaWa-Client-Android)

- [Web Demo 源码下载](https://github.com/anyRTC/anyRTC-WaWa-Client-Web)


## 二、集成指南

### 适用范围

本集成文档适用于iOS RTCWaWaJiEngine SDK 2.0.0 ~ 3.0.0版本。

### 准备环境

- Xcode 9.0+。
- iOS 8.0+ 真机（iPhone 或 iPad）。
- 请确保你的项目已设置有效的开发者签名。

### 导入SDK

**手动导入**

* 前往GitHub[下载Demo](https://github.com/anyRTC/anyRTC-RTCP-iOS)，找到RTCWaWaJiEngine.framework；

* 在Xcode中选择“Add files to 'Your project name'...”，将RTCWaWaJiEngine.framework添加到你的工程目录中
![1.png](https://upload-images.jianshu.io/upload_images/2478176-9655284250ad1070.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 打开General->Embedded Binaries中添加RTCWaWaJiEngine.framework

![1.png](https://upload-images.jianshu.io/upload_images/2478176-7dbc0f736825ad6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 权限说明

使用RTCWaWaJiEngine SDK 前，需要对设备进行授权。打开 info.plist ，点击 + 图标开始添加：

* 添加设备使用「网络」的权限
```
	<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSAllowsArbitraryLoads</key>
		<true/>
	</dict>
```

* 添加设备使用「相机」的权限
```
	<key>NSCameraUsageDescription</key>
	<string>XXX请求访问相机用于...</string>
```

* 添加设备使用「麦克风」的权限

```
	<key>NSMicrophoneUsageDescription</key>
	<string>XXX请求访问麦克风用于...</string>
```

### 后台模式(Background Modes)

勾选Audio, AirPlay and Picture in Picture

![1.png](https://upload-images.jianshu.io/upload_images/2478176-b402ff28caf96212.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 三、API接口文档

### ARWaWaKit 接口类

#### 1. 配置开发者信息

**定义**

```
- (void)initEngine:(NSString *)developerId appId:(NSString *)appId key:(NSString *)key toke:(NSString *)token;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
developerId | NSString | 开发者Id
appId | NSString | appId
key | NSString | key
token | NSString | token

**说明**

该方法为配置开发者信息，上述参数均可在[https://www.anyrtc.io/ ](https://www.anyrtc.io/)应用管理中获得，建议在AppDelegate.m调用。

#### 2. 获取房间列表

**定义**

```
- (void)getRoomListWithBlock:(void(^)(NSDictionary *listDic))complete;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
complete | void(^)(NSDictionary *listDic) | 房间列表回调

#### 3. 加入房间

**定义**

```
- (void)joinRoom:(NSString *)anyRTCId userId:(NSString *)userId userName:(NSString *)userName userIcon:(NSString *)userIcon;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
anyRTCId | NSString | 房间id
userId | NSString | 用户id
userName | NSString | 房间姓名
userIcon | NSString | 用户头像

#### 4. 预约

**定义**

```
- (void)makeBook;
```

#### 5. 取消预约

**定义**

```
- (void)cancelBook;
```

#### 6. 开始游戏

**定义**

```
- (void)startPlay;
```

#### 7. 取消游戏

**定义**

```
- (void)cancelPlay;
```

#### 8. 控制命令

**定义**

```
- (void)sendControlCmd:(ARWaWaJi_CMD_State)cmdState;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
cmdState | ARWaWaJi_CMD_State | 指令

#### 9. 离开房间

**定义**

```
- (void)leaveRoom;
```

### ARWawaKitDelegate 接口类

#### 1. 连接服务成功

**定义**

```
- (void)onConnectServerSuccess;
```

#### 2. 与服务断开连接

**定义**

```
- (void)onDisconnect;
```

#### 3. 与服务重连中

**定义**

```
- (void)onReconnect;
```

#### 4. 初始化anyRTC成功

**定义**

```
- (void)onInitAnyRTCSuccess;
```

#### 5. 初始化anyRTC失败

**定义**

```
- (void)onInitAnyRTCFailed;
```

#### 6. 进入房间回调

**定义**

```
- (void)onJoinRoom:(NSString *)rtcpId roomMember:(int)roomMember code:(int)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | NSString | 通道Id
roomMember | int | 房间总人数
code | int | 状态码

#### 7. 离开房间回调

**定义**

```
- (void)onLeaveRoom:(int)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int | 状态码

#### 8. 娃娃机连接断开回调

**定义**

```
- (void)onWawajiDisconnect;
```

#### 9. 娃娃机断开重连回调

**定义**

```
- (void)onWawajiReconnect:(NSString *)rtcpId;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
rtcpId | NSString | 通道Id

#### 10. 预约结果回调

**定义**

```
- (void)onBookResult:(int)bookMember code:(int)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
bookMember | int | 预约总人数
code | int | 状态码

#### 11. 控制命令回调

**定义**

```
- (void)onControlCmd:(ARWaWaJi_CMD_State)cmd code:(int)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
cmd | ARWaWaJi_CMD_State | 命令
code | int | 状态码

#### 12. 抓娃娃结果回调

**定义**

```
- (void)onGrabResult:(int)code;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int | 1成功，0失败

#### 13. 房间人数变化回调

**定义**

```
- (void)onRoomMemberUpdate:(int)roomMember;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
roomMember | int | 房间总人数

**说明**

当有人进入或者离开房间会回调该方法。

#### 14. 排队人数变化回调

**定义**

```
- (void)onBookMemberUpdate:(int)bookMember;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
bookMember | int | 预约人数

#### 15. 排队抓娃娃准备回调

**定义**

```
- (void)onReadyPlay;
```

#### 16. 准备超时回调

**定义**

```
- (void)onReadyPlayTimeout;
```
**说明**

抓娃娃急者没有点击开始也没有点击取消，会回调该接口。


#### 17. 抓娃娃超时回调

**定义**

```
- (void)onPlayTimeout;
```

  



