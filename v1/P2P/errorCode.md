## 错误码对照表

以下为介绍RTCallEngine SDK 的错误码。

名称 | 值            | 备注
---|------------------------------|----
ARCall_OK | 0 | 正常
ARCall_UNKNOW | 1 | 未知错误
ARCall_EXCEPTION | 2 | SDK调用异常
ARCall_EXP_UNINIT | 3 | SDK未初始化
ARCall_EXP_PARAMS_INVALIDE | 4 | 参数非法
ARCall_EXP_NO_NETWORK | 5 | 没有网络链接
ARCall_EXP_NOT_FOUND_CAMERA | 6 | 没有找到摄像头设备
ARCall_EXP_NO_CAMERA_PERMISSION | 7 | 没有打开摄像头权限
ARCall_EXP_NO_AUDIO_PERMISSION | 8 | 没有音频录音权限
ARCall_EXP_NOT_SUPPORT_WEBRTC | 9 | 浏览器不支持原生的webrtc
ARCall_NET_ERR | 100 | 网络错误
ARCall_NET_DISSCONNECT | 101 | 网络断开
ARCall_LIVE_ERR | 102 | 直播出错
ARCall_EXP_ERR | 103 | 异常错误
ARCall_EXP_Unauthorized | 104 | 服务未授权(仅可能出现在私有云项目)
ARCall_BAD_REQ | 201 | 服务不支持的错误请求
ARCall_AUTH_FAIL | 202 | 认证失败
ARCall_NO_USER | 203 | 此开发者信息不存在
ARCall_SVR_ERR | 204 | 服务器内部错误
ARCall_SQL_ERR | 205 | 服务器内部数据库错误
ARCall_ARREARS | 206 | 账号欠费
ARCall_LOCKED | 207 | 账号被锁定
ARCall_SERVER_NOT_OPEN | 208 | 服务未开通
ARCall_ALLOC_NO_RES | 209 | 没有服务器资源
ARCall_SERVER_NOT_SURPPORT | 210 | 不支持的服务
ARCall_FORCE_EXIT | 211 | 强制离开
ARCall_AUTH_TIMEOUT | 212 | 验证超时
ARCall_NEED_VERTIFY_TOKEN | 213 | 需要验证userToken
ARCall_WEB_DOMIAN_ERROR | 214 | Web应用的域名验证失败
ARCall_IOS_BUNDLE_ID_ERROR | 215 | iOS应用的BundleId验证失败
ARCall_ANDROID_PKG_NAME_ERROR | 216 | Android应用的包名验证失败
ARCall_PEER_BUSY | 800 | 对方正忙
ARCall_OFFLINE | 801 | 对方不在线
ARCall_NOT_SELF | 802 | 不能呼叫自己
ARCall_EXP_OFFLINE | 803 | 通话中对方意外掉线
ARCall_EXP_EXIT | 804 | 对方异常导致(如：重复登录帐号将此前的帐号踢出)
ARCall_TIMEOUT | 805 | 呼叫超时(45秒)
ARCall_NOT_SURPPORT | 806 | 不支持