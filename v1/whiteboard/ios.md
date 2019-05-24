# iOS

### 简介

anyRTC平台画板SDK是一款跨平台轻量级的白板SDK，易用、实时, 提供了包括画笔、背景设置、标准图像、框选等基本功能，同时还支持文档展示和多段互动。

### Demo体验

请根据需求选择渠道安装，安装完白板Demo后，可体验在线会议功能。

- [iOS Demo下载](https://www.pgyer.com/t1Dy)

- [Android Demo下载](https://www.pgyer.com/yhUN)

- [Web Demo 体验](https://demos.anyrtc.io/ar-whiteboard)

### 源码GitHub

源码仅供开发者参考，适用于SDK调试，便于快速集成。

- [iOS Demo 源码下载](https://github.com/anyRTC/anyRTC-Whiteboard-iOS)

- [Android Demo 源码下载](https://github.com/anyRTC/anyRTC-Whiteboard-Android)

- [Web Demo 源码下载](https://github.com/anyRTC/anyRTC-Whiteboard-Web)

## 二、集成指南

### 适用范围

本集成文档适用于iOS ARBoardEngine SDK 2.0.0 ~ 3.0.0版本

### 准备环境

- Xcode 9.0+
- iOS 8.0+ 真机（iPhone 或 iPad）
- 请确保你的项目已设置有效的开发者签名

### 导入SDK

**CocoaPods导入**

* 通过 Cocoapods 下载地址：

```
pod 'ARBoardEngine'
```
* 如果需要安装指定版本则使用以下方式（以 3.0.0 版本为例）：

```
pod 'ARBoardEngine', '3.0.0'
```

**手动导入**

* 前往GitHub[下载Demo](https://github.com/anyRTC/anyRTC-WhiteBoard-iOS)，找到ARBoardEngine.framework；

* 在Xcode中选择“Add files to 'Your project name'...”，将ARBoardEngine.framework添加到你的工程目录中
![ios_board_01](/assets/images/ios_board_01.png)

* 打开General->Embedded Binaries中添加ARBoardEngine.framework

![ios_board_02](/assets/images/ios_board_02.png)

### 权限说明

使用ARBoardEngine SDK 前，需要对设备进行授权。打开 info.plist ，点击 + 图标开始添加：

* 添加设备使用「网络」的权限
```
<key>NSAppTransportSecurity</key>
<dict>
<key>NSAllowsArbitraryLoads</key>
<true/>
</dict>
```

## 三、API接口文档

### ARBoardConfig 接口类

#### 1. 配置开发者信息

**定义**

```
+ (void)initEngine:(NSString *)appId token:(NSString *)token;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
appId | NSString | appId
token | NSString | token

**说明**

该方法为配置开发者信息，上述参数均可在[https://www.anyrtc.io/ ](https://www.anyrtc.io/)应用管理中获得，建议在AppDelegate.m调用。

#### 2. 是否打印日志

**定义**

```
+ (void)enableConsoleLog:(BOOL)isEnable;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
isEnable | BOOL | YES为打印，NO为不打印，默认YES

#### 3. 获取SDK版本号

**定义**

```
+ (NSString*)getSdkVersion;
```

### ARBoardView 接口类

#### 1. 初始化画板

**定义**

```
- (id)initWithRoomID:(NSString*)roomID
withFileId:(NSString*)fileID
withUserId:(NSString*)userID
withUrlArray:(nullable NSArray*)urlArray;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
roomID | NSString | 房间号
fileID | NSString | 文件ID
userID | NSString | 用户ID
urlArray | NSArray | 图片数组，可为空,如果第一次，就是初始化一个白板

**说明**

用户如果第一次进入则创建，第二次进入则拉取白板数据；用户可以根据自己的实际情况来使用。

#### 2. 设置画笔类型

**定义**

```
- (void)setBrushModel:(ARBoardBrushModel)brushModel;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
brushModel | ARBoardBrushModel | 画笔类型

#### 3. 获取画笔类型

**定义**

```
- (ARBoardBrushModel)getBrushModel;
```
**返回值**

当前画笔类型。

#### 4. 设置画笔颜色

**定义**

```
- (void)setBrushColor:(NSString*)brushColor;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
brushColor | NSString | 画笔颜色，16进制字符串

#### 5. 获取画笔颜色

**定义**

```
- (NSString*)getBrushColor;
```
**返回值**

当前画笔颜色，16进制字符串。

#### 6. 设置画笔粗细

**定义**

```
- (void)setBrushWidth:(int)brushWidth;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
brushWidth | int | 画笔粗细，默认2

#### 7. 获取画笔粗细

**定义**

```
- (int)getBrushWidth;
```
**返回值**

当前画笔粗细。

#### 8. 撤销一笔

**定义**

```
- (BOOL)undo;
```

#### 9. 更改背景

**定义**

```
- (void)updateCurrentBgImageWithURL:(NSString*)imageURL;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
imageURL | NSString | 背景url

#### 10. 当前画板截图

**定义**

```
- (UIImage*)getCurrentSnapShotImage;
```

#### 11. 添加一页

**定义**

```
- (void)addBoard:(nullable NSString*)imageURL withFont:(BOOL)isFont;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
imageURL | NSString | 图片url
isFont | BOOL | YES向前加一页，NO向后加一页

#### 12. 删除当前画板

**定义**

```
- (BOOL)deleteCurrentBoard;
```

#### 13. 向前翻一页

**定义**

```
- (BOOL)prePageWithSync:(BOOL)sync;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
sync | BOOL | 是否同步

#### 14. 向后翻一页

**定义**

```
- (BOOL)nextPageWithSync:(BOOL)sync;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
sync | BOOL | 是否同步

#### 15. 跳到某一页

**定义**

```
- (BOOL)switchPage:(int)page withSync:(BOOL)sync;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
page | int | 页数
sync | BOOL | 是否同步

#### 16. 群发消息

**定义**

```
- (BOOL)sendMessage:(NSString*)message;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
message | NSString | 消息内容

#### 17. 设置画板的背景颜色

**定义**

```
- (void)setBoardBgColor:(UIColor *)color;

```
**参数**

参数名 | 类型 | 描述
---|:---:|---
color | UIColor | 背景颜色，默认白色

#### 18. 清空所有内容

**定义**

```
- (void)destoryBoard;
```

#### 19. 清空涂鸦内容

**定义**

```
- (void)clearAllDraws;
```

#### 20. 清空当前涂鸦内容

**定义**

```
- (void)clearCurrentDraw;
```

#### 21. 离开白板

**定义**

```
- (void)leave;
```

### ARBoardViewDelegate 接口类

#### 1. 初始化画板成功

**定义**

```
- (void)initBoardScuess;
```

#### 2. 初始化画板失败

**定义**

```
- (void)onBoardError:(ARBoardCode)nCode;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
nCode | ARBoardCode | 错误码

#### 3. 与服务器断开连接

**定义**

```
- (void)onBoardServerDisconnect;
```

#### 4. 白板页码改变

**定义**

```
- (void)onBoardPageChange:(NSString*)imageUrl withCurrentPage:(int)currentPage withTotalPage:(int)totalPage;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
imageUrl | NSString | 图片url
currentPage | int | 当前页数
totalPage | int | 总页数

#### 5. 消息回调

**定义**

```
- (void)onBoardMessage:(NSString*)message;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
message | NSString | 消息内容

#### 6. 白板数据改变的时间

**定义**

```
- (void)onBoardDrawsChangeTimestamp:(uint32_t)timestamp;
```
**参数**

参数名 | 类型 | 描述
---|:---:|---
timestamp | uint32_t | 时间

#### 7. 画板被销毁

**定义**

```
- (void)onBoardDestory;
```

**说明**

收到该回调，退出白板。

## 四、更新日志

**Version 3.0.0 （2019-05-15）**

* SDK版本升级3.0，API接口变更

**Version 2.0.0 （2017-09-30）**

* SDK版本升级2.0，梳理、完善SDK

## 五、错误码对照表

以下为介绍 iOS ARBoardEngine SDK 的错误码。

名称 | 值            | 备注
---|------------------------------|----
ARBoardCodeParameterError | 3000 | 初始化错误
ARBoardCodeNoNet | 3001 | 初始化无网络
ARBoardCodeUserIdIsNull | 3002 | 用户ID为空
ARBoardCodeSessionPastDue | 201 | session过期
ARBoardCodeDeveloperInfoError | 202 | 开发者信息错误
ARBoardCodeDeveloperArrearage | 206 | 欠费
ARBoardCodeDeveloperNotOpen | 207 | 用户未开通该功能
ARBoardCodeDatabaseError | 301 | 数据库异常
