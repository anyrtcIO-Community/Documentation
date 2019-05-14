# iOS常见问题

#### iOS进入后台后，听不到其他端说话了？
打开TARGETS->Capabilities->Background Models 选中Audio,AirPlay,and Picture in Picture
![](https://upload-images.jianshu.io/upload_images/2557494-11cc7c9988142c7b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 手动集成SDK，运行时出现Reason: image not found的错误怎么回事？
打开TARGETS->General->Embedded Binaries 点击+号把anyRTC SDK添加进来即可

#### 集成SDK后，运行时出现This app has crashed because it attempted to access privacy-sensitive data without a usage description.的错误怎么解决？
打开TARGETS->Info 添加权限
 <table>
      <tr>
       <th>权限</th>
       <th>方法</th>
      </tr>
      <tr>
       <th>相机权限</th>
       <th>NSCameraUsageDescription</th>
      </tr>
      <tr>
       <th>麦克风权限</th>
       <th>NSPhotoLibraryUsageDescription</th>
      </tr>
     </table>

#### 集成SDK后，运行时出现The resource could not be loaded because the App Transport Security policy requires the use of a secure connection.的错误怎么解决？

打开TARGETS->Info 添加权限
 <table>
      <tr>
       <th>权限</th>
       <th>方法</th>
       <th>值</th>
      </tr>
      <tr>
       <th>网络权限</th>
       <th>NSAllowsArbitraryLoads</th>
       <th>YES</th>
      </tr>
     </table>