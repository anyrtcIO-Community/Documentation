## 错误码对照表

以下为介绍RTMeetEngine SDK 的错误码。

名称 | 值            | 备注
---|------------------------------|----
ARMeet_OK | 0 | 正常
ARMeet_UNKNOW | 1 | 未知错误
ARMeet_EXCEPTION | 2 | SDK调用异常
ARMeet_EXP_UNINIT | 3 | SDK未初始化
ARMeet_EXP_PARAMS_INVALIDE | 4 | 参数非法
ARMeet_EXP_NO_NETWORK | 5 | 没有网络链接
ARMeet_EXP_NOT_FOUND_CAMERA | 6 | 没有找到摄像头设备
ARMeet_EXP_NO_CAMERA_PERMISSION | 7 | 没有打开摄像头权限
ARMeet_EXP_NO_AUDIO_PERMISSION | 8 | 没有音频录音权限
ARMeet_EXP_NOT_SUPPOAR_WEBARC | 9 | 浏览器不支持原生的webrtc
ARMeet_NET_ERR | 100 | 网络错误 
ARMeet_NET_DISSCONNECT | 101 | 网络断开
ARMeet_LIVE_ERR | 102 | 直播出错
ARMeet_EXP_ERR | 103 | 异常错误
ARMeet_EXP_UNAUTHORIZED | 104 | 服务未授权(仅可能出现在私有云项目)
ARMeet_BAD_REQ | 201 | 服务不支持的错误请求
ARMeet_AUTH_FAIL | 202  | 认证失败
ARMeet_NO_USER | 203 | 此开发者信息不存在
ARMeet_SVR_ERR | 204 | 服务器内部错误
ARMeet_SQL_ERR | 205 | 服务器内部数据库错误
ARMeet_ARREARS | 206 | 账号欠费
ARMeet_LOCKED | 207 | 账号被锁定
ARMeet_SERVER_NOT_OPEN | 208 | 服务未开通
ARMeet_ALLOC_NO_RES | 209 | 没有服务器资源
ARMeet_SERVER_NO_SURPPOAR | 210 | 不支持的服务
ARMeet_FORCE_EXIT | 211 | 验证UserToken失败
ARMeet_AUTH_TIMEOUT | 212 | 验证超时
ARMeet_NEED_VERTIFY_TOKEN | 213 | 需要验证userToken
ARMeet_WEB_DOMIAN_ERROR | 214 | Web应用的域名验证失败
ARMeet_IOS_BUNDLE_ID_ERROR | 215 | iOS应用的BundleId验证失败
ARMeet_ANDROID_PKG_NAME_ERROR | 216 | Android应用的包名验证失败
ARMeet_NOT_STAAR | 700 | 房间未开始
ARMeet_IS_FULL | 701 | 房间人员已满
ARMeet_NOT_COMPARE | 702 | 房间类型不匹配
