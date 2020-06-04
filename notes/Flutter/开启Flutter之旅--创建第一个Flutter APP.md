### 一. 前言
在本篇文章中，我们使用Android Studio创建一个简单的demo，来揭开Flutter的神秘面纱。（PS：基于studio3.2版本开发的）

### 二. 创建Flutter项目
在Android Studio中，选择“**start a new Flutter project**”或者选择**File > New Flutter Project**，会出现下图的面板，  
![屏幕快照 2019-05-22 下午7.06.31.png](https://upload-images.jianshu.io/upload_images/2353568-441608bcaab9e2e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
选择Flutter Application，然后 **Next** ，出现下图的面板，  
![屏幕快照 2019-05-22 下午7.09.30.png](https://upload-images.jianshu.io/upload_images/2353568-aafcf6751ac1a2d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
这里填写应用的信息。  
- Project name：工程的名字。这里我们就填写first_flutter_app，需要注意的是Project name必须符合 Dart 包名命名规则（小写+下划线，可以有数字）。
- Flutter SDK path：Flutter sdk 的路径。
- Project location：工程的路径。
- Description：工程的描述。

填完之后，点击 **Next**，会出现下图的面板，  
![屏幕快照 2019-05-22 下午7.18.33.png](https://upload-images.jianshu.io/upload_images/2353568-242d76297d70dd01.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

这里是设置我们项目的包名，下面有两个选项：  
- Include Kotlin support for Android code（使用Kotlin开发）
- Include Swift support for iOS code（使用Swift开发）
   
如果使用 Kotlin 开发 Andorid 或者用 Swift 开发 iOS 的话就选上，如果不选 Android 默认使用 Java 开发，iOS 默认使用 Objective-C 开发。  
    
然后点击 **Finish**，我们的项目就创建完成了。

### 三. 运行Flutter APP
点击下图中的按钮运行APP，  
![屏幕快照 2019-05-22 下午7.28.43.png](https://upload-images.jianshu.io/upload_images/2353568-d2479cee0a509fdc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  


### 四. 运行效果
ps：第一次运行时间可能会有点久。  
   
Android 模拟器：  
![Screenshot_1558530168.png](https://upload-images.jianshu.io/upload_images/2353568-3965d548b66a5d54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
   
   
iOS 模拟器：  
![屏幕快照 2019-05-22 下午8.58.15.png](https://upload-images.jianshu.io/upload_images/2353568-344a47770ea63cfd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

### 五. Flutter 代码的目录结构
在创建完Flutter APP之后，我们要看一下它的目录结构。  
![屏幕快照 2019-05-23 下午4.47.48.png](https://upload-images.jianshu.io/upload_images/2353568-65689cea49c4f66f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
- Android 目录  
这个目录下是一个完整的 Android APP 工程的代码。可以理解为Flutter在Android上的壳子，这个目录里的代码都会被打包进 Flutter 的Android 安装包里。  
- ios 目录  
这个目录下是一个完整的 iOS APP 工程的代码。可以理解成 Flutter 在 iOS 上的壳子，这个目录里的代码都会被打包进 Flutter 的 iOS 安装包中。  
- lib 目录  
这里是 Flutter 的代码，使用 Dart 语言编写。main.dart 是 Flutter 的入口文件。  
- test 目录  
这里是 Flutter 的测试代码，使用 Dart 语言编写。  
- pubspec.yaml 文件  
这个是 Flutter 的配置文件，声明了 Flutter APP 的名称、版本、作者等的元数据文件，还有声明的依赖库，和指定的本地资源（图片、字体、音频、视频等）。  
**注意：pubspec.yaml 是 Flutter 的配置，是 Flutter 里的重要部分。**  

然后我们再看一下，从哪里开始编写代码。打开 main.dart :  
![屏幕快照 2019-05-23 下午5.03.25.png](https://upload-images.jianshu.io/upload_images/2353568-06ed50766523c42e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
这里的 main() 函数就是 Flutter 的入口函数。  
在 main() 函数里要运行 runApp() 函数，runApp() 函数的参数类型是Widget 类型。使用的方法如下：  
```
void main() => runApp(MyApp());
```
这个使用方法是固定的。后面文章我们再接着在 main.dart 文件中编写代码。
### 总结
到此，我们就可以创建一个 Flutter APP 并在 Android 和 iOS 设备上运行了。





