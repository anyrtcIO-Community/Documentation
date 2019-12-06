
## 一、集成指南

### 适用范围

本集成文档适用于Android ARBoard SDK 3.0.0+版本。

### 准备环境

- Android Studio 2.1或以上版本
- Android 版本不低于 4.0.3 且支持音视频的 Android 设备（不支持模拟器）
- Android 设备已经连接到有效网络

### 导入SDK

**Gradle方式导入**[ ![Download](https://api.bintray.com/packages/dyncanyrtc/ar_dev/board/images/download.svg) ](https://bintray.com/dyncanyrtc/ar_dev/board/_latestVersion)

添加Jcenter仓库 Gradle依赖：

```
dependencies {
    compile 'org.ar:board:3.0.3''
}

```
---
## 二、开发指南

集成SDK后，还需对SDK进行初始化操作，建议在Application中完成。

#### 1.1 初始化SDK并配置开发者信息

调用 initEngine() 方法配置开发者信息，开发者信息可在anyRTC管理后台中获得，详见[创建anyRTC账号](https://docs.anyrtc.io)


**示例代码：**

```
public class ARApplication extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        ARBoardEngine.Inst().initEngine("AppId",  "AppToken");
    }
}

```

> 自定义的Application需在AndroidManifest.xml注册 


#### 1.2 添加白板View到布局当中

**示例代码**

```
<RelativeLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <org.ar.arboard.weight.ARBoardView
        android:id="@+id/ar_board"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true">
    </org.ar.arboard.weight.ARBoardView>
    
</RelativeLayout>
```
> 直接在布局文件中添加即可，**为了保证数据的准确展示，各端白板视图的长宽比需保持一致。**

#### 1.3 设置画板监听回调

**示例代码**

```
arBoardView.setWhiteBoardListener(ARBoardListener arBoardListener);
```

#### 1.4 设置图片加载器

**示例代码**

```
public class BoardmageLoader extends ImageLoader {
    @Override
    public void displayImage(Context context, Object path, ARPinchImageView imageView) {
        RequestManager requestManager= Glide.with(context);
        requestManager.load(path).
                into(imageView);
    }
}

arBoardView.setImageLoader(new BoardmageLoader());
```
> 新建一个类，继承ImageLoader，在重载方法displayImage()里用您项目中加载网络图片的方式加载即可，上述代码以Glide为例

#### 1.5 初始化画板房间

**示例代码**

```
arBoardView.initWithRoomId(String roomId, String fileId, String userId, List<String> imageList);
```
> 注:用户如果第一次进入则创建，第二次进入则拉取白板数据；用户可以根据自己的实际情况来使用,**所有参数不能为空**，imageList可以传入图片URL地址或者颜色代码（例：#FF0000）

#### 1.6 设置画笔类型撤销等操作
**示例代码**

```
//设置画笔类型
arBoardView.getARBoardConfig().setBrushModel(ARBoardConfig.BrushModel.Line);
//撤销
arBoardView.undo()
```
> 更多请参考API介绍

## 三、API文档

**主要类文件概览** 

类名                | 主要功能
---                 | ---
ARBoardEngine | 开发者配置信息、获取版本号、是否打印日志
ARBoardTextureView  | 白板视图类，白板操作类，包含了所有白板相关的操作接口
ARBoardConfig   | 白板配置类，包含设置画笔，颜色，粗细等相关操作


### 1.配置开发者 

方法 | 说明
---|---
initEngine() | 配置开发者信息

参数名 | 说明
---|---
String strAppId | AppId
String strToken | Token

> 注:使用ARBoardSDK必须先配置开发者信息，可从www.anyrtc.cc管理中心获取。

### 2. 是否打印日志

```
ARBoardEngine.Inst().setDebugLog(false);//true

```

### 3.获取SDK版本号 

```
 ARBoardEngine.Inst().getSdkVersion();

```

### 4.注册回调监听

```
class YourActivity extends AppCompatActivity implements ARBoardListener{

    ARBoardView arBoardView
    
    arBoardView.setWhiteBoardListener(this);
}
```
### 5.设置图片加载器（安卓特有）

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

### 6.初始化画板
接口 | 说明
---|---
initWithRoomId() | ARBoardView


ARBoardView 的指定初始化方法，需传入一个房间 ID,文件ID,用户ID,图片地址集合, 参数说明如下：

参数名 | 说明
---|---
String roomId |房间ID **不能为空**
String fileID | 文件ID:每个画板保持唯一,自己业务维持；**不能为空**
String userID | 用户ID:用户平台用户ID;为空:则不能操作画板；**不能为空**
List<String> imageList | 背景集合:画板根据集合元素个数来创建多少个面板；**不能为空**

>注:用户如果第一次进入则创建，第二次进入则拉取白板数据；用户可以根据自己的实际情况来使用

### 7.白板配置类操作


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

### 8.白板基本操作

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



### 9.回调信息

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
