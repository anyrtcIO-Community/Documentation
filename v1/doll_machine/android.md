## 一、概述

#### 简介

#### Demo体验

请根据需求选择渠道安装，安装完娃娃机 Demo后，可体验在线抓娃娃。

- [iOS Demo下载](https://www.pgyer.com/fYGm)

- [Android Demo下载](https://www.pgyer.com/3blO)

- [Web Demo体验](http://wawaji.anyrtc.cc/)
- 
#### 源码GitHub

源码仅供开发者参考，适用于SDK调试，便于快速集成。

- [iOS Demo 源码下载](https://github.com/anyRTC/anyRTC-P2P-iOS)

- [Android Demo 源码下载](https://github.com/anyRTC/anyRTC-WaWa-Client-Android)

- [Web Demo 源码下载](https://github.com/anyRTC/anyRTC-WaWa-Client-Web)

## 二、集成指南

#### 适用范围

本集成文档适用于Android 娃娃机 SDK 3.0.0版本。

#### 准备环境

- Android Studio 2.1或以上版本
- Android 设备已经连接到有效网络

#### 导入SDK

**Gradle方式导入**[ ![Download](https://api.bintray.com/packages/dyncanyrtc/anyrtc_dev/anyRTCWaWaClient/images/download.svg) ](https://bintray.com/dyncanyrtc/anyrtc_dev/anyRTCWaWaClient/_latestVersion)

添加Jcenter仓库 Gradle依赖：

```
dependencies {
   compile 'org.anyrtc:anyrtcwawaclient:1.0.0'
}
```

或者 Maven
```
<dependency>
  <groupId>org.anyrtc</groupId>
  <artifactId>anyrtcwawaclient</artifactId>
  <version>1.0.0</version>
  <type>pom</type>
</dependency>
```


#### 权限说明

使用ARP2P SDK需以下权限

```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
#### 混淆配置
为了避免混淆SDK，在Proguard混淆文件中增加以下配置：

```
-dontwarn org.anyrtc.**
-keep class org.anyrtc.**{*;}
-dontwarn org.ar.**
-keep class org.ar.**{*;}
-dontwarn org.webrtc.**
-keep class org.webrtc.**{*;}
```
---
## 三、API接口文档

### ARWaWaClient 类

#### 1. 初始化并配置开发者信息

**定义**

```
void initEngineWithARInfo(String developerid, String appid, String appkey, String apptoken)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
developerid|String|developerid
appId | String | appId
appkey|String|appkey
token | String  | token

**说明**

该方法为配置开发者信息，上述参数均可在https://www.anyrtc.io/ 应用管理中获得；建议在Application调用。


#### 2. 设置回调监听

**定义**

```
void setServerListener(WaWaServerListener serverListener)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
serverListener|WaWaServerListener|WaWaServerListener实现类

**说明**

这个必须在所有操作的最前面设置，否在将收不到任何回调信息

#### 3. 打开服务

**定义**

```
void openServer()
```
**说明**

打开服务连接

#### 4. 获取娃娃机列表

**定义**

```
void getRoomList()
```
**说明**

获取娃娃机列表


#### 5. 进入房间

**定义**

```
void joinRoom(String roomID,String userid,String username,String userIcon)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
roomID|String|娃娃机的Id（娃娃机开启时会生成，列表信息中会带有该字段）
strUserId|String|用户Id
strUserName|String|用户昵称
strUserIcon|String|用户头像

**说明**

加入娃娃机房间



#### 6. 离开房间
**定义**
```
void leaveRoom()
```
**说明**  

离开娃娃机房间


#### 7. 用户预约

**定义**
```
void book()
```
**说明**  

进入玩娃娃机队列

#### 8. 用户取消预约

**定义**
```
void unBook()
```
**说明**  

离开玩娃娃机队列

#### 9. 开始游戏

**定义**
```
void play()
```
**说明**  

每局开始时调用，娃娃机将进入可操作状态

#### 10. 取消游戏

**定义**
```
void cancleStart()
```
**说明**  

当收到准备开始游戏时，若不想玩了可调用该方法取消。如果已经调用开局，此方法无效。

#### 11. 发送操作指令

**定义**
```
void  sendControlCmd(int cmd)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
cmd|int|操作指令

**说明**  

 CMD_FORWARD (0) , 向前  
 
CMD_BACKWARD(1),  向后  

CMD_LEFT(2),     向左  
        
 CMD_RIGHT(3),  向右  
 
CMD_GRAB(4), 下抓  

CMD_SWITCH_CAMERA(5) 切换视角

#### 12. 关闭服务

**定义**
```
void closeServer()
```

**说明**  

关闭服务




### WaWaServerListener 回调类

#### 1.连接服务成功

**定义**

```
void onConnectServerSuccess()
```

#### 2.与服务断开连接

**定义**

```
void onDisconnect() 
```

#### 3.重连服务中

**定义**

```
void onReconnect()
```

#### 4.初始化anyRTC成功

**定义**

```
void onConnAnyRTCServerSuccess()
```


#### 5.初始化anyRTC失败

**定义**

```
void onConnAnyRTCFaild() 
```

#### 6.娃娃机列表信息

**定义**

```
void onGetRoomList(String roomList)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
roomList | String |房间列表信息(Json) 


#### 7.加入娃娃机

**定义**

```
void onJoinRoom(int code,String videoInfo)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int |code 0 成功
videoInfo | String |视频流信息

#### 8.离开娃娃机房间

**定义**

```
void onLeaveRoom(int code)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int |code 0 成功

#### 9.指令信息

**定义**

```
void onControlCmd(int code,String cmd)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int |状态码
cmd | String |操作指令

#### 10.视频流地址更新

**定义**

```
void onRoomUrlUpdate(String strVideoInfo)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
strVideoInfo | String |视频流地址

#### 11.预约结果

**定义**

```
void onBookResult(int code，String strbookMember))
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int |状态码 code ==0 成功  204 上局还未结束
strbookMember | String |预约人数

#### 12.取消预约

**定义**

```
void onUnBookResult(int code,String strbookMember)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
code | int |状态码
strbookMember | String |已经预约的人数信息

#### 13.人员数量更新

**定义**

```
void onBookMemberUpdata(String strBookMember)
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
strbookMember | String |人数信息

#### 14.准备开始

**定义**

```
void onReadyStart()
```
**说明**

当用户预约进入排队队列后，轮到该用户时将会回调此方法。此时可开局可取消开局。

#### 15.准备超时

**定义**

```
void onReadyTimeout();
```
**说明**

服务将会从排队队列移除该用户


#### 16.抓娃娃超时

**定义**

```
void onPlayTimeout()
```
**说明**

服务将会下发下抓命令给娃娃机


#### 17.抓娃娃结果

**定义**

```
void onResult(boolean result)
```
**说明**

 true 抓到娃娃  false 没抓到
