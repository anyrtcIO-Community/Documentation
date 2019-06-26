## 一、概述

### 简介
ARBoardSDK 即白板SDK,提供了包括画笔、背景设置、标准图像、框选等基本功能，同时还支持文档展示和多段互动。
### Demo 体验

请根据需求选择渠道安装，安装完画板Demo后，可体验多人在线画板功能。

- [iOS Demo下载](https://www.pgyer.com/t1Dys)

- [Android Demo下载](https://www.pgyer.com/yhUN)

- [Web Demo 体验](https://demos.anyrtc.io/ar-whiteboard)

### 源码 GitHub

源码仅供开发者参考，适用于SDK调试，便于快速集成。

- [iOS Demo 源码下载](https://github.com/anyRTC/anyRTC-Whiteboard-iOS)

- [Android Demo 源码下载](https://github.com/anyRTC/anyRTC-Whiteboard-Android)

- [Web Demo 源码下载](https://github.com/anyRTC/anyRTC-Whiteboard-Web)
- 
## 二、集成指南

### 适用范围

本集成文档适用于 Android ARBoard SDK 3.0.0+版本。


### 导入SDK

**Gradle方式导入**[ ![Download](https://api.bintray.com/packages/dyncanyrtc/ar_dev/board/images/download.svg) ](https://bintray.com/dyncanyrtc/ar_dev/board/_latestVersion)

```
dependencies {
    compile 'org.ar:board:3.0.2'
}

```
或者 Maven
```
<dependency>
  <groupId>org.ar</groupId>
  <artifactId>board</artifactId>
  <version>3.0.2</version>
  <type>pom</type>
</dependency>

```

**主要类文件概览** 

类名                | 主要功能
---                 | ---
ARBoardEngine | 开发者配置信息、获取版本号、是否打印日志
ARBoardTextureView  | 白板视图类，白板操作类，包含了所有白板相关的操作接口
ARBoardConfig   | 白板配置类，包含设置画笔，颜色，粗细等相关操作
### 2.使用方法
**2.1集成**

添加Jcenter仓库 Gradle依赖：


**2.2配置开发者** 

方法 | 说明
---|---
initEngine() | 配置开发者信息

**参数**

参数名 | 类型 | 描述
---|:---:|---
appId | String | 应用ID
token | String | 应用Token

> 注:使用ARBoardSDK必须先配置开发者信息，可从www.anyrtc.cc管理中心获取。

**2.2.1是否打印日志** 

```
ARBoardEngine.Inst().setDebugLog(false);//true

```

**2.2.2获取SDK版本号** 

```
 ARBoardEngine.Inst().getSdkVersion();

```


**2.3添加白板** 
```
 <org.ar.arboard.weight.ARBoardView
        android:id="@+id/wb_board"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
</org.ar.arboard.weight.ARBoardView>
```
> 说明：直接在布局文件中添加即可，**为了保证数据的准确展示，各端白板视图的长宽比需保持一致。**

**2.4注册回调监听** 

```
class YourActivity extends AppCompatActivity implements ARBoardListener{

    ARBoardView arBoardView
    
    arBoardView.setWhiteBoardListener(this);
}
```
**2.5设置图片加载器（安卓特有）** 

```
public class ImageLoader extends org.ar.arboard.imageloader.ImageLoader {

    @Override
    public void displayImage(Context context, Object path, PinchImageView imageView) {
        RequestManager requestManager= Glide.with(context);
        requestManager.load(path).
                into(imageView);
    }
}

arBoardView.setImageLoader(new ImageLoader())
```
> 说明：新建一个类，继承ImageLoader，在重载方法displayImage()里用您项目中加载网络图片的方式加载即可。

**2.6初始化画板** 
接口 | 说明
---|---
initWithRoomId() | ARBoardView


ARBoardView 的指定初始化方法，需传入一个房间 ID,文件ID,用户ID,图片地址集合, 参数说明如下：

参数名 | 说明
---|---
String roomId |房间ID **不能为空**
String fileID | 文件ID:每个画板保持唯一,自己业务维持；**不能为空**
String userID | 用户ID:用户平台用户ID;为空:则不能操作画板；**不能为空**
List<String> imageList | 背景数组:画板根据数组个数来创建多少个面板；**不能为空**

>注:用户如果第一次进入则创建，第二次进入则拉取白板数据；用户可以根据自己的实际情况来使用

**2.7 白板配置类操作**


方法 | 说明
---|---
arBoardView.getARBoardConfig().set/getBrushColor(): | 设置/获取画笔颜色
arBoardView.getARBoardConfig().set/getBoardWidth(): | 设置/获取画笔粗细
arBoardView.getARBoardConfig().set/getBrushModel(): | 设置/获取画笔类型

 
画笔类型包含以下5种

类型 | 说明
---|---
None |无效果
Graffiti| 涂鸦
Arrow | 箭头
Line | 直线
Rect | 矩形
Transform | 缩放、移动
TransformSync | 缩放、移动 (同步)

```
示例：设置画笔类型为箭头
arBoardView.getARBoardConfig().setBrushModel(ARBoardConfig.BrushModel.Arrow);
```

**2.8 白板基本操作**

方法 | 说明
---|---
undo() | 撤销上一步操作
prePage(boolean isSync) | 上一页，是否同步
nextPage(boolean isSync) | 下一页，是否同步
switchPage(int pageNum,boolean isSync) | 跳到某一页，其他人是否同步翻页
addBoard(boolean isBefore,String imageUrl) | 添加一页，向前或向后，图片地址
deleteCurrentBoard() | 删除当前画板，包括画笔和背景图片
updateCurrentBgImage(String imageUrl) | 更新当前页背景，图片地址
getCurrentSnapShotImage(ARScreenShotResult result) | 截取当前画板，结果监听
destoryBoard() | 清空白板所有数据，包含背景图片
clearAllDraws() | 清空当前白板涂鸦数据，不包含背景图片
clearCurrentDraw() | 清空当前涂鸦内容，不包括背景图片
sendMessage(String message)|发送自定义消息
leave() | 离开白板，断开连接



### 3. 回调信息

接口 | 说明
---|---
initBoardScuess | 白板初始化成功
onBoardError()| 白板发生错误，错误码
onBoardServerDisconnect | 白板断开连接
onBoardPageChange()|当翻页以或者添加一页，删除一页时会给用户回调页码信息以及当前画板的图片URL
onBoardDrawsChangeTimestamp(long timestamp)|其他人对于画板操作的时间戳
onBoardMessage|收到消息
onBoardDestroy|画板销毁：调了destoryBoard()方法

**错误码**

错误码 | 说明
---|---
3000 | 参数为空或者参数错误
30001| 当前无网络
40000| 图片加载器为空
201| Session过期
202| 开发者信息错误
203| 账号欠费
206| 该功能未开通
301| 数据库异常
