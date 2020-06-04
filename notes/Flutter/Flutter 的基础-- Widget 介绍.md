### 一. 前言
上一篇我们简单地了解了 Dart 语言，接着我们就开始学习 Flutter 的基础 Widget 吧。
   
本篇文章的目录结构：  
![](https://upload-images.jianshu.io/upload_images/2353568-961074e6a2813a8a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

### 二. 什么是 Widget
在 Flutter 中要用 Widget 构建 UI，Widget 相当于 Android 里的 View，iOS 里的 UIView。但与 View 不同的是，Widget 具有不同的生命周期：它是不可变的，每当 Widget 或者其状态发生变化时，Flutter 的框架都会创建一个新的 Widget 实例树，会先计算从上一个状态转换到下一个状态所需的最小更改，然后再去刷新界面。而 Aandroid 中的 View 会被绘制一次，并且在 invalidate 调用之前不会重绘。

### 三. Widget 的结构：Widget 树
Widget 组合的结构是树，所以叫做 Widget 树。  
Widget 树结构如下图：![](https://upload-images.jianshu.io/upload_images/2353568-5ca55e759ce65884.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
1. 父 Widget 和 子 Widget  
在 Widget 树里，Widget 有包含和被包含的关系：  
- 父 Widget：包含其他 Widget 的就叫父 Widget。  
- 子 Widget：被父 Widget 包含的 Widget 就叫子 Widget。   

2. 根 Widget  
根 Widget 也叫 RootWidget。  
我们之前创建的 Flutter demo 的入口文件 main.dart 里面有一个 main() 方法，是 Flutter 的入口方法：  
```
void main() => runApp(MyApp());
```
 runApp(MyApp()) 里的参数 MyApp() 就是一个 Widget，MyApp 的作用只是封装了一下，实际使用的 Widget 是 MaterialApp，这里的 MaterialApp 就是 RootWidget，Flutter 默认会把 根Widget 充满屏幕。
   
在 Flutter 中，根 Widget 只能是以下三个：  
- WidgetsApp：可以自定义风格的根 Widget。  
- MaterialApp：是在 WidgetsApp 上添加了很多 material-design 的功能，是 Material Design 风格的根 Widget。  
- CupertinoApp：也是基于 WidgetsApp 实现的 iOS 风格的根 Widget。  
这三个中最常用的是 MaterialApp，因为 MaterialApp 的功能最完善。MaterialApp 经常与 Scaffold 一起使用，下一篇文章我们再介绍。  

### 四. Widget 的标识符：Key  
因为 Flutter 采用的是 react-style 的框架，每次刷新 UI 的时候，都会重新构建新的 Widget 树，然后和之前的 Widget 树进行对比，计算出变化的部分，这个计算过程叫做 diff，在 diff 过程中，如果能提前知道哪些 Widget 没有变化，无疑会提高 diff 的性能，这时候就需要使用标识符。
   
**在 diff 过程中，如何知道哪些 Widget 没有变化呢？**  
给 Widget 添加一个唯一的标识符，然后在 Widget 树的 diff 过程中，查看刷新前后的 Widget 树有没有相同标识符的 Widget，如果标识符相同，则说明 Widget 没有变化，否则说明 Widget 有变化。
>注意：这个标识符在 Flutter 中就是 Key，所有 Widget 都有 Key 这一个属性。
   
那么 Flutter 中是如何在 diff 过程中判断哪些 Widget 没有变化呢？   
- 默认情况下（Widget 没有设置 Key）  
当没有给 Widget 设置 Key 时，Flutter 会根据 Widget 的 runtimeType 和显示顺序是否相同来判断 Widget 是否有变化。  
runtimeType 是 Widget 的 类型。
- Widget 有 Key  
当给 Widget 设置了 Key 时，Flutter 是根据 Key 和 runtimeType 是否相同来判断 Widget 是否有变化。  
>注意：Key的使用：  
一般情况下我们不需要使用 Key，但是当页面比较复杂时，就需要使用 Key 去提升渲染性能。    

### 五. Widget 大全  
Widget 有很多，Flutter官网（https://flutter.dev/docs/development/ui/widgets）上将 Widget 分为14类：  
1. Accessibility：辅助功能Widget。
2. Animation and Motion：动画和动作Widget。
3. Assets, Images, and Icons：资源和图片Widget。
4. Async：Flutter应用程序的异步Widget。
5. Basics：在构建第一个Flutter应用程序之前，需要知道的Basics Widget。
6. Cupertino (iOS-style widgets)：iOS风格的Widget。
7. Input：除了在Material Components和Cupertino中的输入Widget外，还可以接受用户输入的Widget。
8. Interaction Models：响应触摸事件并将用户路由到不同的视图中。
9. Layout：用于布局的Widget。
10. Material Components：Material Design风格的Widget。
11. Painting and effects：不改变布局、大小、位置的情况下为子Widget应用视觉效果。
12. Scrolling：滚动相关的Widget。
13. Styling：主题、填充相关Widget。
14. Text：显示文本和文本样式。
  
Widget 几乎实现了所有的功能，除了 UI、布局之外，还有交互、动画等，所以掌握 Widget 是很重要的。  
### 六. Widget的分类
因为渲染是很耗性能的，为了提高 Flutter 的帧率，就要尽量减少不必要的 UI 渲染，所以 Flutter 根据 UI 是否有变化，将 Widget 分为：  
- StatefulWidget：是 UI 可以变化的 Widget，创建完后 UI 还可以再更改。  
- StatelessWidget：是 UI 不可以变化的 Widget，创建完后 UI 就不可以再更改。 
   
### StatefulWidget  
   
StatefulWidget 是 UI 可以变化的Widget。  
   
**代码举例：**
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp("Hello World"));

class MyApp extends StatefulWidget {
  // This widget is the root of your application.

  String content;

  MyApp(this.content);

  @override
  State<StatefulWidget> createState() {
    // TODO: implement createState
    return MyAppState();
  }
}

class MyAppState extends State<MyApp> {

  bool isShowText =true;

  void increment(){
    setState(() {
      widget.content += "d";
    });
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'Flutter Demo',
        theme: ThemeData(
          primarySwatch: Colors.blue,
        ),
        home: Scaffold(
          appBar: AppBar(title: Text("Widget -- StatefulWidget及State"),),
          body: Center(
              child: GestureDetector(
                child: isShowText? Text(widget.content) :null,
                onTap: increment,
              )
          ),
        )
    );
  }
}
```  
当点击 Hello World 文本框的时候，内容会变。  
   
实现自定义的 StatefulWidget，需要两部分：  
1. StatefulWidget：主要功能创建 State。  
2. State：状态。  
   
其中 StatefulWidget 需要实现以下两个步骤：  
1. 首先继承 StatefulWidget  
2. 实现 createState() 的方法，返回一个 State  
    
**State**：    
   
实现步骤：  
1. 首先继承 State，State 的泛型类型是上面定义的 Widget 的类型
2. 实现 build() 的方法，返回一个 Widget
3. 需要刷新 UI 的话，调用 setState() 方法
   
State 的功能：  
- build()
- setState()
1. build()：创建 Widget  
State 的 build() 函数创建 Widget，用于显示 UI。  
   
2. setState()：刷新UI  
当我们想要刷新UI的时候，在 setState() 方法里更改数据，然后  setState() 会触发 State 的 build() 方法，引起强制重建 Widget，重建 Widget 的时候会重新绑定数据，而这时数据已经更新了，从而达到刷新 UI 的目的。  
接下来我们看一下 setState() 方法的源码：![](https://upload-images.jianshu.io/upload_images/2353568-51a7923e88805119.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)1117行，执行无参函数 fn()，并把结果转换成 dynamic 类型，然后赋值给 result。1133行，才是真正的 Widget 创建，感兴趣的可以去看 markNeedsBuild() 方法的源码，这里不过多介绍了。
>注意：刷新 UI 的代码必须在调用 setState() 之前或者在 setState() 函数里面写，才能刷新UI。

>**为什么 StatefulWidget 被分成 StatefulWidget 和 State 两部分？**
一方面是为了保存当前的 APP 状态，另一方面是为了性能。  
当 UI 需要更新的时候，假设 Widget 和 State 都重建，由于 State 里保存了 UI 显示的数据，State 重建就会创建新的实例，UI 之前的状态就会丢失，导致 UI 显示异常，所以要分成两部分StatefulWidget 部分重建，而 State 部分不重建。  
Widget 重建的成本比较低，但 State 的重建成本很高，分成两部分就可以让 State 不会被频繁重建，这样就提高了性能。  
   
**生命周期**：  
   
StatefulWidget 的生命周期：  
- createState  
   
State的生命周期：![](https://upload-images.jianshu.io/upload_images/2353568-16fc9c3b222be85a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- moundted is true  
mounted 是 boolean，只有当 mounted 为 true 时，才能调用 setState()  
- initState  
createState 创建 State 对象后调用的，只会调用一次。一旦这个方法完成，State 对象就初始化完成了。  
- didChangeDependencied  
state依赖的对象发生变化时调用，在initState() 方法运行完后调用  
- didUpdateWidget  
Widget状态改变时候调用，可能会调用多次  
- build   
在 didChangeDependencies()（或者 didUpdateWidget() ）之后调用。 这是构建Widget的地方。  
- deactivate  
当 State 从树中移除时，就会触发 deactivate。但是如果在这帧结束前，如果其他地方使用到了这个 Widget，就会重新把 Widget 插入到树里，这就涉及到了 Widget 的重用，Widget 的重用和 Key 有关。  
- dispose  
当 StaefulWidget 从树中移除时调用 dispose() 方法  

#### StatelessWidget  
   
StatelessWidget 是没有 State（状态）的 Widget，当 Widget 在运行时不需要改变时，就用 StatelessWidget。  

**代码举例：**
```
import 'package:flutter/material.dart';

void main() => runApp(MyApp("Hello World"));

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  final String content;

  MyApp(this.content);
  
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: Scaffold(
        body: Center(
          child: Text(content),
        ),
      )
    );
  }
}
```
上面代码中的 MyApp 是 StatelessWidget，其实看一下 Text 的源码，发现它也是 StatelessWidget。![](https://upload-images.jianshu.io/upload_images/2353568-2eae0ff49ef437f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/620)  
要实现自定义的 StatelessWidget，需要下面两步：
1. 首先继承 StatelessWidget
2. 必须要实现 build 函数，返回一个 Widget。

**生命周期：**
StatelessWidget 的生命周期只有一个，即 build 函数。

**使用注意事项：**
StatelessWidget 只能在它初始化的时候，通过构造函数传递一些额外的参数，这些参数在之后的阶段都不会变化。因为 StatelessWidget 是 immutable的，只能在加载/构建 Widget 时才绘制一次。
### 总结 
本文我们主要介绍了什么是 Widget、Widget树结构、Widget标识符、Widget大全和Widget的状态分类，后面会详细介绍具体的Widget。
