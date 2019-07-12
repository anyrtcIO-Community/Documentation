## 一、快速开始

### 集成指南

#### 适用范围

本集成文档适用于Web ARWhiteboard SDK 3.0.0版本。

#### 兼容情况

- Chrome、Firefox、safari 11(以上)等，具体使用[webRTC检测工具](https://docs.anyrtc.io/v1/tools/%E6%B5%8F%E8%A7%88%E5%99%A8WebRTC%E6%A3%80%E6%B5%8B.html)
- H5支持chrome内核。

#### 导入SDK

##### npm 市场

* 安装或更新至最新版本：

```
npm install ar-whiteboard@latest --save-dev

import Board from 'ar-whiteboard';
import 'ar-whiteboard/lib/index.css';
```

##### js 引用

- 前往[SDK 下载页面](https://docs.anyrtc.io/download/js/ArWhiteBoard.3.0.0.js)，`ctrl+s`或`command+s`保存到本地
- 引用

```
<script src="yourAssetsPath/ArWhiteBoard.3.0.0.js"></script>
```

### 开发指南
#### 1. 初始化SDK
集成SDK后，还需对SDK在页面进行初始化操作。
##### 1.1 导入头文件
```
import Board from 'ar-whiteboard';
import 'ar-whiteboard/lib/index.css';
```
##### 1.2 实例化对象
`DomId`div容器的id属性;
```
let Board = new Board(DomId);
```

##### 1.3 配置开发者信息
配置开发者信息，开发者信息可在anyRTC管理后台中获得，详见[创建anyRTC账号](https://docs.anyrtc.io)。
```
//配置开发者信息
Board.initEngineWithARInfo(APP_ID, APP_TOKEN);

//配置私有云(默认无需配置)
//Board.configServer(SERVE_URL);
```
#### 2. 创建白板
##### 2.1 实例化白板对象
```
Board.initWithRoomID(anyRTCId, fileId, userId, backgroundList);
```
#### 3. 白板操作        
详细操作请看ArBoard 接口类。
#### 4.退出画板
退出。
```
Board.leave();
```
## 二、API接口文档
### ArBoard 接口类
#### 1.初始化实例

##### 示例
```
let Board = new Board(DomId);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
DomId | Element | Div容器的id属性

#### 2. 连接画板服务
##### 示例
```
Board.initEngineWithARInfo(appId, appToken); 

```

##### 参数
参数名 | 类型 | 描述 
---|---|---
appId | String | anyRTC云平台的应用id
appToken | String | anyRTC云平台的应用的appToken

##### 说明
该方法为配置开发者信息，上述参数均可在[https://www.anyrtc.io/ ](https://www.anyrtc.io/)应用管理中获得。

#### 3. 初始化画板
##### 示例
```
Board.initWithRoomID(anyRTCId, fileId, userId, backgroundList);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
anyRTCId | String | 房间号ID
fileId |String | 文件ID
userId | String | 用户id
backgroundList | Array | 画板背景图URL(保证图片源允许跨域),第一次创建房间时必生效，后续初始化不会覆盖掉之前服务端的背景图片
`backgroundList`
参数名 | 类型 | 描述
---|:---:|---
board_background | String | 背景图片地址
board_number |Number | 画板的页数

##### 说明
注意：该参数为画板背景图片的队列，存储着每一页的背景图片。第一次初始化时backgroundList参数为必填。当第一次初始化之后，再次初始化时不会清除上一次的画板数据，而是读取上次的数据进行渲染。

#### 4. 设置画笔的类型
##### 示例
```
Board.setBrushModel(type);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
type |Number | 画笔的类型 `0`不可编辑`1`涂鸦(默认)`2`箭头`3`直线`4`矩形

##### 说明
设置画笔的类型

#### 5. 预览本地音视频
##### 示例
```
Board.getBrushModel();
```

##### 说明
获取画笔的类型。

#### 6. 设置画笔的粗细
##### 示例
```
Board.setBrushWidth(width);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
width | Number | 例如10，该值均自动转换为px。

#### 7. 获取画笔的粗细
##### 示例
```
Board.getBrushWidth();
```

##### 说明
获取画笔的类型。

#### 8. 设置画笔的颜色
##### 示例
```
Board.setBrushColor(color);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
width | Number | 颜色的十六进制码。例如#000000,为了和移动平台的兼容性，请勿缩写为#000

##### 说明
设置画笔的颜色。 

#### 9. 获取画笔的颜色
##### 示例
```
Board.getBrushColor();
```

##### 说明
获取画笔的颜色。 

#### 10. 更新当前画板背景图片
##### 示例
```
Board.updateCurrentBgImage(BGUrl);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
strBGUrl | String | 图片url

#### 11. 获取当前画板的背景图片URL
##### 示例
```
Board.getCurrentBgImageURL();
```

##### 说明
获取当前画板的背景图片URL

#### 12. 在当前画板之前或之后插入一个新的画板
##### 示例
```
Board.addBoard(withFront, boardBGUrl);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
withFront | Number | `0`表示在当前画板之前插入，`1`表示在当前画板之后插入
boardBGUrl | String | 插入新画板的背景图片URL(保证图片源允许跨域)

#### 13. 删除当前页的画板
##### 示例
```
Board.deleteCurrentBoard();
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
type | Number | 自定义共享通道标识id

#### 14. 切换到上一页
##### 示例
```
Board.prePage(needSync);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
needSync | Boolean | `true`其他端的画板同步滑动到上一页，false`仅本地滑动到上一页

#### 15. 切换到下一页
##### 示例
```
Board.nextPage(needSync);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
needSync | Boolean | `true`其他端的画板同步滑动到下一页，false`仅本地滑动到上一页

#### 16. 滑动画板到第几页
##### 示例
```
Board.switchPage(needSync, page);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
needSync | Boolean | `true`其他端的画板同步滑动到指定页，`false`仅本地滑动到指定页
page | Number | 滑动画板到第几页

#### 17. 发送用户实时消息
##### 示例
```
Board.sendMessage(message);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
message | String | 消息文本，推荐json字符串拓展性极佳

#### 18. 设置画板大小
##### 示例
```
Board.setCanvasSize(width, heigh);
```
##### 参数
参数名 | 类型 | 描述
---|:---:|---
width | Number | 画板将要设置的宽度
heigh | Number | 画板将要设置的高度

##### 说明
设置画板大小，当尺寸变化，或者放大缩小时调用。

#### 19. 设置画板比例
##### 示例
```
Board.setBoardScale(scale);
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
scale | Number | 画板放大比例，1~3倍

##### 说明
设置画板大小，当尺寸变化，或者放大缩小时调用。

#### 20. 撤销画笔
##### 示例
```
Board.undo();
```

##### 说明
撤销画笔,撤销当前画板上自己的画笔,逐条撤销。

#### 21. 销毁画板
##### 示例
```
Board.destoryBoard();
```

##### 说明
清除所有画板的笔迹以及背景图片。

#### 22. 清除所有画板笔迹
##### 示例
```
Board.clearAllDraws();
```

##### 说明
清除所有画板的笔迹。

#### 23. 清除当前画板所有笔迹
##### 示例
```
Board.clearCurrentDraw();
```

##### 说明
清除当前画板所有笔迹。

#### 24. 离开房间
##### 示例
```
Board.leave();
```

##### 说明
离开画板（房间）。

### ArBoard 回调接口

#### 1. 画板断开连接
##### 示例
```
Board.on('onBoardServerDisconnect', () => {});
```

#### 2. 监听画板变化
##### 示例
```
Board.on('onBoardPageChange', (index, totalIndex, currentBgUrl) => {});
```
##### 参数

参数名 | 类型 | 描述
---|:---:|---
index | Number | -
totalIndex | Number | 画板总页数
currentBgUrl | String | 当前画板背景图片URL

##### 说明
监听画板变化，当画板发生变化，将会收到该回调（例如翻页、添加一页、删除一页、背景图片更新等等。

#### 3. 收到广播消息
##### 示例
```
Board.on('onBoardMessage', (message) => {});
```

##### 参数

参数名 | 类型 | 描述
---|:---:|---
message | String | 广播消息主体

##### 说明
收到广播的消息，消息主体为sendMessage时发送的字符串，此处推荐json字符串，可以和业务系统高效的配合。例如：指定用户接收消息、踢人（判断字段中是否有自己的userid即可）等等。

#### 4. 画板实时变化时间戳回调
##### 示例
```
Board.on('onBoardDrawsChangeTimestamp', (timestamp) => {});
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
timestamp | String | 时间戳

##### 说明
主持人监听并实现录制，非主持人不做处理。

#### 5. 画板断开连接
##### 示例
```
Board.on('onBoardServerDisconnect', (error) => {});
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
error | String | 错误码，详情请看

##### 说明
主持人监听并实现录制，非主持人不做处理。

#### 6. switch_board
##### 示例
```
Board.on('switch_board', () => {});
```

##### 参数
参数名 | 类型 | 描述
---|:---:|---
 |  | 

##### 说明

#### 7. 画板被摧毁
##### 示例
```
Board.on('onBoardDestroy', () => {});
```

##### 说明
画板已被摧毁。

#### 8. 更改画板背景
##### 示例
```
Board.on('update_board_background', (res) => {});
```

##### 说明
更改画板背景, `res.code` 为0说明更改成功。


#### 9. 监听画板错误
##### 示例
```
Board.on('onBoardError', (error) => {});
```

##### 参数

参数名 | 类型 | 描述
---|:---:|---
error | String | 错误码

##### 说明
监听画板错误详情参考错误码对照表。


## 三、更新日志

**Version 3.0.0 （2019-04-11）**

* SDK版本升级3.0，API接口变更，更加简洁规范