## Web常见问题

### 1. 看不到本地预览图像？

调用`setLocalVideoCapturer`并将`mediaRender`添加到页面中，却看不到本地图像，请检测以下因素：

- 检查是否有摄像头（不含虚拟摄像头）并安装驱动
- 检查是否禁用摄像头
- 当浏览器调起`是否运行打开摄像头`授权申请时，是否同意授权
- 浏览器控制台是否报错

> 打开摄像头错误大致分为以下几种：

![打开摄像头失败的错误信息](/assets/images/web/FAQ/getUserMediaError.png)

### 2. 收到远程视频窗口回调却看不到视频？

当收到`stream-subscribed`或者`exstream-subscribed`回调，却看不到视频图像，检查是否设置`mediaRender`窗口大小，此处以会议为例：

```
meet.on("stream-subscribed", (peerId, pubId, userId, userData, mediaRender) => {
  let videoBox = document.createElement('div');
  //设置容器的大小
  videoBox.style.width = "320px";
  videoBox.style.height = "240px";
  mediaRender.id = pubId;
  //mediaRender的大小为videoBox父容器的大小
  videoBox.appendChild(mediaRender);
  document.body.appendChild(videoBox);
});
```

排除以上原因，则为远程参会端打开摄像头失败或未推流失败。

### 3. 如何用`iframe`嵌套实时通信页面

使用`iframe`嵌套一个实时通讯的页面，需要在`iframe`标签上添加一些权限说明，例如：

```
<iframe
  src="需要嵌套的链接地址"
  width="1080"
  height="720"
  allow="camera; microphone;"
  frameborder="0">
</iframe>
```

###其他问题
请前往官方唯一[技术论坛](https://bbs.anyrtc.io/)进行提问
    

