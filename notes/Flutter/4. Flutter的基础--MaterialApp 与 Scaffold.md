### 一. 前言
   
在我们创建第一个 Flutter Demo 中，可以看到 MaterialApp 和 Scaffold 的使用。下面我们就详细地介绍一下他们。

### 二. MaterialApp
   
MaterialApp 是 Android 开发者所熟悉的 Material Design 设计风格。如果我们想使用其他已经封装好的 Material Design 风格的 Widget，就必须使用 MaterialApp。MaterialApp 也是 Flutter Widget 树里的第一个元素，即 Root WIdget。  
   
**使用**  
MaterialApp 共有 25 个属性，本文我们只简单介绍几个，如 title、color、theme、home。  
如我们创建的第一个 demo 中的代码：  
```
class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        // This is the theme of your application.
        //
        // Try running your application with "flutter run". You'll see the
        // application has a blue toolbar. Then, without quitting the app, try
        // changing the primarySwatch below to Colors.green and then invoke
        // "hot reload" (press "r" in the console where you ran "flutter run",
        // or simply save your changes to "hot reload" in a Flutter IDE).
        // Notice that the counter didn't reset back to zero; the application
        // is not restarted.
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```
- title  
String 类型  
- color  
Color 类型  
- theme  
ThemeData 类型，设置 Flutter App 的主题，比如颜色、字体等。  
- home  
Widget 类型，主界面  
### 三. Scaffold
   
Scaffold 实现了 Material Design 的基本布局结构，例如 AppBar、Drawer、SnackBar 等，它经常会作为 MaterialApp 的子Widget，Scaffold 会自动填充可用的空间，这通常意味着它将占据整个窗口或屏幕，并且 Scaffold 会自动适配不同的屏幕。  
   
**使用**  
Scaffold 总共有 16 个属性，本文简单介绍几个属性：  
- appBar  
AppBar 类型，就是顶部的标题栏，不设置的话就不会显示  
- backgroundColor  
Color 类型，背景颜色  
- body   
Widget 类型，是实际要显示的 UI 内容  
   
### 总结  
本文我们主要了解了 MaterialApp 和 Scaffold，MaterialApp 大部分情况下要作为 Flutter 的 Root Widget，然后MaterialApp 的子Widget 就是 Scaffold，然后在 Scaffold 的子Widget 中写UI。

