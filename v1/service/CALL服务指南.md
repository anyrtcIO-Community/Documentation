# CALL服务指南

## 简介
CALL即点对点通话，只在用户两端建立通信，没有中间服务维护用户状态。anyRTC针对点对点功能推出消息推送和VIP通道功能。

### 消息推送：
当用户离线时，则CALL呼叫不能呼通，这种情况下，anyRTC使用第三方消息推送的方式通知用户有点对点呼叫。

#### 说明
暂时支持的第三方推送为：极光(#https://docs.jiguang.cn/)，信鸽(#https://xg.qq.com/docs/)和个推(#http://docs.getui.com/)。anyRTC不收取推送相关的费用。

#### 设置
当您需要推送服务时，需要先在推送平台上创建并和获取到对应的信息。不同的推送平台所需要的推送信息不同。

##### 极光：
AppKey, Master Secret
##### 信鸽：
AppKey, Master Secret, accessId
##### 个推：
AppKey, Master Secret

推送铃声如果不设置将使用默认铃声，如果设置请参考各个推送平台的文档。
极光(#https://docs.jiguang.cn/)，信鸽(#https://xg.qq.com/docs/)和个推(#http://docs.getui.com/)


### VIP通道：
当有国外用户使用CALL业务时，P2P打洞可能会出现失败，这种情况下，anyRTC利用服务器进行中转建立CALL呼叫。

#### 说明
VIP通道费用请联系客服。
客服QQ号：47894118

