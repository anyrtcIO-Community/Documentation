# 快速开始

## 集成指南

### 适用范围

本集成文档适用于Web RTMaxEngine SDK 3.0.0及以上版本。

### 兼容情况

* Chrome、Firefox、safari 11(以上)或其他谷歌内核浏览器。
* H5支持chrome内核。
* 配合[anyRTC webRTC检测工具](https://docs.anyrtc.io/v1/tools/%E6%B5%8F%E8%A7%88%E5%99%A8WebRTC%E6%A3%80%E6%B5%8B.html)使用。

### 导入SDK

**js 引用**
* 前往[SDK 下载页面](https://docs.anyrtc.io/download/js/RTMaxKit.js.zip)。
* 此SDK现仅支持直接引用，后续支持npm安装。
* 直接引用相关的.js文件（注意引用顺序）


```
<script src="https://www.anyrtc.io/websdk/RTMax/版本/adapter.js"></script>
<script src="https://www.anyrtc.io/websdk/RTMax/版本/xmlhttp.js"></script>
<script src="https://www.anyrtc.io/websdk/RTMax/版本/anyrtc.js"></script>
<script src="https://www.anyrtc.io/websdk/RTMax/版本/RTMaxKit.js"></script>
```

### 开发指南

#### 1. 初始化SDK

引入SDK后，还需对SDK在页面进行初始化操作。

##### 1.1 实例化对象

```
let rtcMax =  new RTMaxKit();
```

##### 1.2 监听回调

```
//加入对讲组成功
rtcMax.on('onRTCJoinTalkGroupOK', function (myId) {

});

//加入失败
//根据code码查询错误原因
rtcMax.on('onRTCJoinTalkGroupFailed', function (error) {

});

//设置其他人视频显示窗口
//对方（或己方）同意视频通话请求后，会收到此回调
rtcMax.on('onRTCOpenVideoRender', function (pubId, videoRender, UserData, type) {

});

//其他人离开移除视频显示窗口
rtcMax.on('onRTCCloseVideoRender', function (pubId, type) {
 
});
```

##### 1.3 配置开发者信息

配置开发者信息，开发者信息可在anyRTC管理后台中获得，详见创建anyRTC账号。

```
//配置开发者信息
rtcMax.initEngineWithAnyRTCInfo(developerId, appId, appKey, appToken, domain);

//配置私有云
rtcMax.configServerForPriCloud(address, port);
```

#### 2. 加入房间

##### 2.1 设置本地视频采集窗口

设置本地显示窗口，参数constraints为音视频配置项，包含视频帧率、码率、相机类型等。

```
//打开摄像头
rtcMax.setLocalVideoCapturer(constraints);
```

##### 2.2 其他常用方法

```
//申请对讲
rtcMax.applyTalk(0);

//结束对讲
rtcMax.cancelTalk();

//发起监看
rtcMax.monitorVideo(strUserId, strUserData)

//发起通话
rtcMax.makeCall(strUserId, nType, strUserData);

//打断通话&打断强插对讲（不包含对讲）
rtcMax.breakTalk(strGroupId);
```

##### 2.3 退出对讲组

```
//退出对讲组
rtcMax.leaveTalkGroup();
```

## 二、API 接口文档

### RTMaxKit 实例化

#### 1. 初始化实例

##### 示例

```
let rtcMax = new RTMaxKit();
```

#### 2. 配置开发者信息

##### 示例

```
rtcMax.initEngineWithAnyRTCInfo(developerId, appId, appKey, appToken, domain);
```

**参数**

参数名 | 类型 | 描述
---|---|---
developerId	 | String | anyRTC云平台的开发者id
appId | String | anyRTC云平台的应用id
appKey | String | anyRTC云平台的应用的appKey
appToken | String | anyRTC云平台的应用的appToken
domain | String | anyRTC云平台的应用的业务域名

**说明**

方法配置开发者信息，开发者信息可在anyRTC管理后台中获得，详见[创建anyRTC账号](https://docs.anyrtc.io/)。

#### 3. 配置私有云

**示例**

```
rtcMax.configServerForPriCloud(address, port);
```

**参数**

参数名 | 类型 | 描述
---|---|---
address	 | String | 私有云服务地址
port | Number | 私有云服务端口

**说明**

配置服务器地址，私有云需要配置，否则无需配置（如果网站是HTPPS 服务器地址不支持IP）

#### 4. 获取SDK版本号

**示例**

```
rtcMax.getSdkVersion();
```

**说明**

获取当前SDK版本

### RTMaxKit 接口类

#### 1. 预览本地音视频

**示例**

```
rtcMax.setLocalVideoCapturer(constraints);
```

**参数**

参数名 | 类型 | 描述
---|---|---
constraints	 | Object | 音视频配置，可以不填写默认为开启

**constraints**

参数名 | 类型 | 描述
---|---|---
video | Object | 视频配置
audio | Object | 音频配置

video | 类型 | 描述
---|---|---
enable | Boolean | `true`为采集摄像头， `false`则为不采集
devideId | String | devideId: 设备ID，可通过`getDevices`方法获取

audio | 类型 | 描述
---|---|---
enable | Boolean | `true`为采集麦克风， `false`则为不采集
devideId | String | devideId: 设备ID，可通过`getDevices`方法获取

**Return**

`Promise` 对象

`Promise.then `返回的参数data
audio | 描述 
---|---
mediaStream | MediaStream 
mediaRender | HTMLDivElement

**说明**

预览本地音视频，如果`video.enable`为`true`时，表示允许采集摄像头；`video.deviceId`表示采集指定摄像头的媒体流。

#### 2. 设置本地视频是否传输

**示例**

```
rtcMax.setLocalVideoEnable(enable);
```

**参数**

参数名 | 类型 | 描述
---|---|---
enable | Boolean | `true`为传输视频，`false`为不传输视频，默认视频传输
		
#### 3. 设置本地音频是否传输

**示例**

```
rtcMax.setLocalAudioEnable(enable);
```

**参数**

参数名 | 类型 | 描述
---|---|---
enable | Boolean | `true`为传输音频，`false`为不传输音频，默认视频传输

#### 4. 设置其他与会者视频窗口

**示例**

```
rtcMax.setRTCVideoRender(stream, dRender);
```

**参数**

参数名 | 类型 | 描述
---|---|---
stream | String | RTC视频流
dRender | String | 对方视频的窗口，本地设置

**说明**

该方法用于与会者接通后，与会者视频接通回调中`OnRTCOpenVideoRender`使用。

#### 5. 加入房间

**示例**

```
rtcMax.joinRTC(anyRTCId, userId, userData);
```

**参数**

参数名 | 类型 | 描述
---|---|---
anyRTCId | String | 对讲组号
userId | String | 用户的第三方平台的用户id
userData | String | 开发者自己平台的相关信息（昵称，头像等），可选。(限制512字节)

#### 6. 加入对讲组

**示例**

```
rtcMax.joinTalkGroup(groupId, userId, userData);
```

**参数**

参数名 | 类型 | 描述
---|---|---
groupId | String | groupId 对讲组id（同一个anyrtc平台的appid内保持唯一性）
userId | String | 用户的第三方平台的用户id
userData | String | 加入对讲组的用户自定义数据, 可以为JSON字符串，小于512字节

**说明**

加入对讲组成功会收到onRTCJoinTalkGroupOK回调，失败则收到onRTCJoinTalkGroupFailed。

#### 7. 切换对讲组

**示例**
```
rtcMax.switchTalkGroup(groupId, userData);
```

**参数**

参数名 | 类型 | 描述
---|---|---
groupId | String | 对讲组id（同一个anyrtc平台的appid内保持唯一性）
userData | String | 加入对讲组的用户自定义数据, 可以为JSON字符串，小于512字节

**说明**

切换对讲组成功会收到onRTCJoinTalkGroupOK回调，失败则收到onRTCJoinTalkGroupFailed。

#### 8. 退出对讲组

**示例**
```
rtcMax.leaveTalkGroup();
```

**说明**

退出当前对讲组。

#### 9. 退出房间;

**示例**
```
rtcMax.leaveRTC();
```

**说明**

释放实例。离开页面或退出房间时需要调用此方法，其他端才会收到用户离开的回调，否则会有视图残留在页面。

#### 10. 申请对讲

**示例**
```
rtcMax.applyTalk(priority);
```

**参数**

参数名 | 类型 | 描述
---|---|---
priority | Number | 申请抢麦用户的级别（0权限最大（数值越大，权限越小）；除0以外，可以后台设置0-10之间的抢麦权限大小））

**说明**

在对讲组中发言者需申请对讲，申请对讲成功将会收到`onRTCApplyTalkOk`回调，反之收到`onRTCTalkClosed`结束对讲回调。

#### 11. 取消对讲

**示例**

```
rtcMax.cancelTalk(priority);
```

**说明**

申请对讲成功（onRTCApplyTalkOk）之后，主动结束（广播)对讲。

#### 12. 强插对讲

**示例**

```
rtcMax.talkP2P(peerUserId, userData);
```

**参数**

参数名 | 类型 | 描述
---|---|---
peerUserId | String |正在对讲的用户id (需要p2ptalk的用户id)
userData | String | 发起人的用户自定义数据, 可以为`JSON`字符串，小于512字节

**说明**

强制与当前正在对讲的用户发起一对一的对讲。不影响对讲组的其他成员对讲。一对一对讲不受对讲组中的其他成员影响，一对一对讲内容也不会影响对讲组中的成员。
当用户正在对讲时才可强制发起P2P通话,该用户会收到`onRTCTalkP2POn`。

#### 13. 取消强插对讲

**示例**

```
rtcMax.talkP2PClose(peerUserId);
```

**参数**

参数名 | 类型 | 描述
---|---|---
peerUserId | String |正在对讲的用户id (需要p2ptalk的用户id) 

**说明**

控制台关闭强插对讲，该用户会收到`onRTCTalkP2POff`。

#### 14. 结束强插对讲

**示例**

```
rtcMax.closeP2PTalk();
```

**说明**

控制台关闭强插对讲，该用户会收到`onRTCTalkP2POff`。

#### 15. 打断对讲组的所有对讲

**示例**

```
rtcMax.breakTalk(groupId);
```

**参数**

参数名 | 类型 | 描述
---|---|---
groupId | String | 对讲组的id，将打断该对讲组内的所有对讲

**说明**

打断指定对讲组中的对讲，正在发言的人讲被强制结束对讲，不能用于打断强插对讲（因为对讲与强插对讲互不影响）。

#### 16. 打断某个用户的通话与强插对讲
**示例**

```
rtcMax.breakCall(groupId);
```

**参数**

参数名 | 类型 | 描述
---|---|---
groupId | String | 打断该用户的通话、强插对讲（不可打断对讲）

**说明**

打断指定用户的呼叫与强插对讲(P2P)，不能打断对讲(广播)。
注意：
* 通话强拆，比如是A是发起者，B和C是被叫。
* 如果我强拆A，则这个会话就结束
* 如果我强插B或C，这个会话还会继续，只是B或C退出了而已

#### 17. 发起通话请求
**示例**

```
rtcMax.makeCall(callUserId, type, userData);
```

**参数**

参数名 | 类型 | 描述
---|---|---
callUserId | String | 向该用户发起通话
type | Number | 0：发起视频通话 1：发起音频通话
userData | String | 发起人的用户自定义数据, 可以为JSON字符串，小于512字节

**说明**

主动向用户发起音频呼叫或视频会叫，对方会收到`onRTCMakeCall`回调，当通话被释放(譬如，发起方被`breakCall`，或是当前呼叫方或邀请方均已退出通话)，发起方会收到`onRTCReleaseCall`回调。

#### 18. 发起通话邀请 

**示例**

```
rtcMax.inviteCall(callUserId, userData);
```

**参数**

参数名 | 类型 | 描述
---|---|---
callUserId | String | 向该用户发起通话邀请
userData | String |发起人的用户自定义数据, 可以为JSON字符串，小于512字节

**说明**

发起通话邀请必须建立在发起通话请求成功（`makeCall`）的基础上。

#### 19. 接受通话请求
**示例**

```
rtcMax.acceptCall(callUserId);
```

**参数**

参数名 | 类型 | 描述
---|---|---
callUserId | String | 打断该用户的通话、强插对讲（不可打断对讲）

**说明**

当收到通话请求（通话请求或者通话邀）时，同意通话请求，建立通话连接。

#### 20. 拒绝通话请求
**示例**

```
rtcMax.rejectCall(callUserId);
```

**参数**

参数名 | 类型 | 描述
---|---|---
callUserId | String | 打断该用户的通话、强插对讲（不可打断对讲）

**说明**

当收到通话请求（通话请求或者通话邀）时，拒绝通话请求，建立通话连接。

#### 21. 主叫方结束通话
**示例**

```
rtcMax.endCall(callUserId);
```

**参数**

参数名 | 类型 | 描述
---|---|---
callUserId | String | 打断该用户的通话、强插对讲（不可打断对讲）

**说明**

主叫方结束与某人通话，无论对方是呼叫方还是邀请方，而非`leaveCall`。
当通话内呼叫方与邀请方均已退出通话（会被`breakCall`强拆），主叫方将会收到`onRTCReleaseCall`回调。

#### 22. 发起视频监看
**示例**

```
rtcMax.monitorVideo(userId);
```

**参数**

参数名 | 类型 | 描述
---|---|---
userId | String | 被监看用户userId
userData | String | 发起人的用户自定义数据, 可以为JSON字符串，小于512字节

**说明**

主动监看某个用户的视频；若收到用户“视频上报”时，同时调用该接口实现用户上报流程。

#### 23. 结束视频监看
**示例**

```
rtcMax.closeVideoMonitor(userId);
```

**参数**

参数名 | 类型 | 描述
---|---|---
userId | String | 被监看用户userId

**说明**

取消对指定用户的视频监看。

#### 24. 设置远端用户是否可以接受自己的音视频
**示例**

```
rtcMax.setRemoteCtrlAVStatus(userId, audioEnabled, videoEnabled);
```

**参数**

参数名 | 类型 | 描述
---|---|---
userId | String | 被监看用户userId
audioEnabled | Boolean | 该用户是否可以听到自己的音频，true：可听到；false不可听到
videoEnabled | Boolean | 被该用户是否可以看到自己的视频，true：可看到；false不可看到

**说明**

设置指定用户是否看到自己的图像或听到自己的声音；建立通话后可以调用当前方法，让通话中的其他成员是否可接收自己的音视频。

#### 25. 发送对讲组消息
**示例**

```
rtcMax.sendUserMessage(userName, userHeaderUrl, content);
```

**参数**

参数名 | 类型 | 描述
---|---|---
userName | String | 自己的昵称
userHeaderUrl | String | 自己的头像
content | String | 自定义消息内容

**说明**

群组广播消息，组内人员将会收到`onRTCUserMessage`回调。

### RTMaxKit 回调接口

#### 1. 设置本地视频采集窗口结果

**示例**

```
rtcMax.on('onSetLocalVideoCapturerResult', function(code, render, stream){
    
}); 
```

**参数**

参数名 | 类型 | 描述
---|---|---
code | Number | 错误码：0为成功，其他请参考错误码
render | String | Video或audio element对象
stream | String | RTC视频流

**说明**

设置本地视频采集窗口结果。

#### 2. 收到对讲组人员的音视频流

**示例**

```
rtcMax.on('onRemoteVideoStream', function(stream, RTCPubId, type){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
stream | String | RTC视频流
RTCPubId | String | 对讲组人员的ID
type | Number | 对讲type: 0 对讲；1 强插对讲；2 视频监控；3 音频呼叫；4 视频呼叫

**说明**

收到对讲组人员的音视频流时，需要给`strRTCPubId`对应的视频窗口添加音视频流(调用`setRTCVideoRender`)。

#### 3. 设置对讲组人员的视频窗口

**示例**

```
rtcMax.on('onRTCOpenVideoRender', function(RTCPubId, render, userData, type){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
RTCPubId | String | 用户ID
render | String | Video或audio element对象
userData | String | 加入人员的用户数据
type | Number | 对讲type: 0 对讲；1 强插对讲；2 视频监控；3 音频呼叫；4 视频呼叫

**说明**

远端人员发起对讲，或者呼叫建立时需要设置远端人员的音视频窗口，等待其音视频流。此时应该添加'strRTCPubId'对应的视图窗口（video或audio DOM元素）。

#### 4. 设置对讲组人员的视频窗口

**示例**

```
rtcMax.on('onRTCCloseVideoRender', function(RTCPubId, type){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
RTCPubId | String | 用户ID
type | Number | 对讲type: 0 对讲；1 强插对讲；2 视频监控；3 音频呼叫；4 视频呼叫

**说明**

通话结束或视频监控结束，发言移除`strRTCPubId`对应的视频窗口即可。

#### 5. 加入对讲组成功

**示例**

```
rtcMax.on('onRTCJoinTalkGroupOK', function(groupId){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
groupId | String | 对讲组ID

**说明**

加入对讲组成功回调。

#### 6. 加入对讲组失败

**示例**

```
rtcMax.on('onRTCJoinTalkGroupFailed', function(code){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
code | Number | 错误码

**说明**

加入对讲组失败回调。

#### 7. 收到离开对讲组回调

**示例**

```
rtcMax.on('onRTCLeaveTalkGroup', function(){
    
});
```

**说明**

收到此回调，退出当前对讲组。


#### 8. 申请对讲成功

**示例**

```
rtcMax.on('onRTCApplyTalkOk', function(){
    
});
```

**说明**

申请对讲成功回调，反之收到`nRTCTalkClosed回调`。

#### 9. 申请对讲失败

**示例**

```
rtcMax.on('onRTCTalkClosed', function(code){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
code | Number | 错误码

**说明**

申请对讲失败回调。

#### 9. 其他人正在对讲

**示例**

```
rtcMax.on('onRTCTalkOn', function(userId, userData){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
userId | String | 对讲用户的id
userData | String | 对讲用户的自定义用户数据


**说明**

收到其他人正在对讲回调。

#### 10. 结束对讲回调

**示例**

```
rtcMax.on('onRTCTalkClosed', function(code, userId, userData){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
code | Number | 错误码，0 为正常退出对讲
userId | String | 对讲用户的id
userData | String | 对讲用户的自定义用户数据


**说明**

其他人结束对讲或发起对讲失败的回调。

#### 11. 强插对讲成功回调

**示例**

```
rtcMax.on('onRTCTalkP2POk', function(userData){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
userData | String | 对讲用户的自定义用户数据


**说明**

强制发起一对一对讲成功。

#### 12. 结束强插对讲回调

**示例**

```
rtcMax.on('onRTCTalkP2PClosed', function(code, userData){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
code | Number | 错误码
userData | String | 对讲用户的自定义用户数据


**说明**

收到结束一对一对讲回调。

#### 13. 监看用户视频结果回调

**示例**

```
rtcMax.on('onRTCVideoMonitorResult', function(code, userId, userData){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
code | Number | 错误码
userId | String | 被监看用户的id
userData | String | 	被监看用户的自定义用户数据


**说明**

监看用户视频成功回调。

#### 14. 监看用户视频结果回调

**示例**

```
rtcMax.on('onRTCVideoMonitorClose', function(userId, userData){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
userId | String | 被监看用户的id
userData | String | 被监看用户的自定义用户数据

**说明**

被监看端挂断或主动关闭监看。

#### 15. 收到用户上报视频请求

**示例**

```
rtcMax.on('onRTCVideoReport', function(userId, userData){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
userId | String | 上报端用户的id
userData | String | 上报端用户的自定义用户数据

**说明**

收到用户上报视频请求时，如果同意则主动向该用户发起监看视频请求，反之忽略。

#### 16. 用户结束上报视频回调

**示例**

```
rtcMax.on('onRTCVideoReportClose', function(userId, userData){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
userId | String | 上报端用户的id
userData | String | 上报端用户的自定义用户数据

**说明**

用户上报之后并且主动监看用户之后，必须由上报端取消上报，接受上报端无法结束上报操作。

#### 17. 发起通话请求成功

**示例**

```
rtcMax.on('onRTCMakeCallOK', function(callId){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
callId | String | 呼叫用户的id

**说明**

发起通话请求成功，等待被呼叫方同意或拒绝。

#### 18. 用户同意通话请求回调

**示例**

```
rtcMax.on('onRTCAcceptCall', function(userId, userData){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
userId | String | 该用户的id
userData | String | 该用户的自定义用户数据

**说明**

用户同意通话请求回调。

#### 19. 用户同意通话请求回调

**示例**

```
rtcMax.on('onRTCRejectCall', function(userId, code, userData){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
userId | String | 该用户的id
code | Number | 错误码
userData | String | 该用户的自定义用户数据

**说明**

用户拒绝通话请求回调。

#### 20. 用户离开当前通话

**示例**

```
rtcMax.on('onRTCLeaveCall', function(userId){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
userId | String | 该用户的id

**说明**

户同意通话请求后主动退出当前通话（被叫方调用LeaveCall）。

#### 21. 用户离开当前通话

**示例**

```
rtcMax.on('onRTCMakeCall', function(){
    
});
```

**说明**

当前通话所有的呼叫方与邀请方均已退出当前通话时，会释放当前通话。

#### 22. 收到通话请求

**示例**

```
rtcMax.on('onRTCMakeCall', function(callId, type, userId, userData){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
callId | String | 通话ID
type | Number | 	通话类型 : 0 音视频；1 音频；
userId | String | 发起方用户的id
userData | String | 发起方用户的自定义用户数据

**说明**

收到通话呼叫或通话请求会收到该请求，如果同意通话请求调用`acceptCall`，否则调用`rejectCall`。

#### 23. 收到主叫方挂断通话

**示例**

```
rtcMax.on('onRTCEndCall', function(callId, userId, userData){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
callId | String | 通话ID
userId | String | 发起方用户的id
userData | String | 发起方用户的自定义用户数据

**说明**

当同意主叫方发起的通话呼叫或通话请求时，主叫方主动挂断与自己的通话，将会收到该回调。

#### 24. 收到对讲组广播消息

**示例**

```
rtcMax.on('onRTCUserMessage', function(userName, headerUrl, content){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
userName | String | 发送者用户昵称
headerUrl | String | 发送者用户头像
content | String | 发送消息内容

**说明**

收到当前对讲组的广播消息。

#### 25. 用户的音视频状态变化

**示例**

```
rtcMax.on('onRTCAVStatus', function(userId, bAudio, bVideo){
    
});
```

**参数**

参数名 | 类型 | 描述
---|---|---
userId | String | 发送者用户id
bAudio | Boolean | true为打开音频，false为关闭音频
bVideo | Boolean | true为打开视频，false为关闭视频

**说明**

收到当前对讲组内用户的音视频状态变化。