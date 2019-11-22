## 错误码对照表

以下为介绍RTCPEngine SDK 的错误码。

名称 | 值            | 备注
---|------------------------------|----
ARRtcp_OK | 0 | 正常
ARRtcp_UNKNOW | 1 | 未知错误
ARRtcp_EXCEPTION | 2 | SDK调用异常
ARRtcp_EXP_UNINIT | 3 | SDK未初始化
ARRtcp_EXP_PARAMS_INVALIDE | 4 | 参数非法
ARRtcp_EXP_NO_NETWORK | 5 | 没有网络链接
ARRtcp_EXP_NOT_FOUND_CAMERA | 6 | 没有找到摄像头设备
ARRtcp_EXP_NO_CAMERA_PERMISSION | 7 | 没有打开摄像头权限
ARRtcp_EXP_NO_AUDIO_PERMISSION | 8 | 没有音频录音权限
ARRtcp_EXP_NOT_SUPPOAR_WEBARC | 9 | 浏览器不支持原生的webrtc
ARRtcp_NET_ERR | 100 | 网络错误 
ARRtcp_NET_DISSCONNECT | 101 | 网络断开
ARRtcp_LIVE_ERR | 102 | 直播出错
ARRtcp_EXP_ERR | 103 | 异常错误
ARRtcp_EXP_UNAUTHORIZED | 104 | 服务未授权(仅可能出现在私有云项目)
ARRtcp_BAD_REQ | 201 | 服务不支持的错误请求
ARRtcp_AUTH_FAIL | 202  | 认证失败
ARRtcp_NO_USER | 203 | 此开发者信息不存在
ARRtcp_SVR_ERR | 204 | 服务器内部错误
ARRtcp_SQL_ERR | 205 | 服务器内部数据库错误
ARRtcp_ARREARS | 206 | 账号欠费
ARRtcp_LOCKED | 207 | 账号被锁定
ARRtcp_SERVER_NOT_OPEN | 208 | 服务未开通
ARRtcp_ALLOC_NO_RES | 209 | 没有服务器资源
ARRtcp_SERVER_NO_SURPPOAR | 210 | 不支持的服务
ARRtcp_FORCE_EXIT | 211 | 验证UserToken失败
ARRtcp_AUTH_TIMEOUT | 212 | 验证超时
ARRtcp_NEED_VERTIFY_TOKEN | 213 | 需要验证userToken
ARRtcp_NOT_START | 800 | 会议未开始
