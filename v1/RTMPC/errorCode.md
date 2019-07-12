## 错误码对照表

以下为介绍RTMPCHybirdEngine SDK 的错误码。

名称 | 值            | 备注
---|------------------------------|----
ARRtmp_OK | 0 | 正常
ARRtmp_UNKNOW | 1 | 未知错误
ARRtmp_EXCEPTION | 2 | SDK调用异常
ARRtmp_EXP_UNINIT | 3 | SDK未初始化
ARRtmp_EXP_PARAMS_INVALIDE | 4 | 参数非法
ARRtmp_EXP_NO_NETWORK | 5 | 没有网络链接
ARRtmp_EXP_NOT_FOUND_CAMERA | 6 | 没有找到摄像头设备
ARRtmp_EXP_NO_CAMERA_PERMISSION | 7 | 没有打开摄像头权限
ARRtmp_EXP_NO_AUDIO_PERMISSION | 8 | 没有音频录音权限
ARRtmp_EXP_NOT_SUPPORT_WEBRTC | 9 | 浏览器不支持原生的webrtc
ARRtmp_NET_ERR | 100 | 网络错误 
ARRtmp_NET_DISSCONNECT | 101 | 网络断开
ARRtmp_LIVE_ERR | 102 | 直播出错
ARRtmp_EXP_ERR | 103 | 异常错误
ARRtmp_EXP_Unauthorized | 104 | 服务未授权(仅可能出现在私有云项目)
ARRtmp_BAD_REQ | 201 | 服务不支持的错误请求
ARRtmp_AUTH_FAIL | 202  | 认证失败
ARRtmp_NO_USER | 203 | 此开发者信息不存在
ARRtmp_SVR_ERR | 204 | 服务器内部错误
ARRtmp_SQL_ERR | 205 | 服务器内部数据库错误
ARRtmp_ARREARS | 206 | 账号欠费
ARRtmp_LOCKED | 207 | 账号被锁定
ARRtmp_SERVER_NOT_OPEN | 208 | 服务未开通
ARRtmp_ALLOC_NO_RES | 209 | 没有服务器资源
ARRtmp_SERVER_NOT_SURPPORT | 210 | 不支持的服务
ARRtmp_FORCE_EXIT | 211 | 强制离开
ARRtmp_NOT_START | 600 | 直播未开始
ARRtmp_HOSTER_REJECT | 601 | 主播拒绝连麦
ARRtmp_LINE_FULL | 602 | 连麦已满
ARRtmp_CLOSE_ERR | 603 | 游客关闭错误，onRtmpPlayerClosed
ARRtmp_HAS_OPENED | 604 | 直播已经开始，不能重复开启
ARRtmp_IS_STOP | 605 | 直播已结束