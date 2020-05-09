### 一、 Activity 的生命周期
1. 正常情况下的生命周期  
![屏幕快照 2018-09-20 下午3.45.48.png](https://upload-images.jianshu.io/upload_images/2353568-959559f4d7321d22.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

正常情况下，一个 Activity 从启动到结束经历的生命周期如下：  
onCreate() -> onStart() -> onResume() -> onPause() -> onStop() -> onDestory()。  

（1）onCreate()：   
当 Activity 第一次创建时会被调用，这是生命周期的第一个方法。在这个方法中，可以做一些初始化工作。      

（2）onRestart()：  
表示 Activity 正在重新启动，一般情况下，当当前 Activity 从不可见重新变为可见状态时，onRestart 就会被调用。这种情形一般是用户行为导致的，比如用户按 Home 键切换到桌面或打开了另一个新的 Activity，接着用户又回到这个 Activity。  


（3）onStart()：  
表示 Activity 正在启动，即将开始，这时 Activity 已经出现了，但是还没有出现在前台，无法与用户交互。这个时候可以理解为 Activity 已经显示出来，但是我们还看不到。    

（4）onResume()：    
表示 Activity 已经可见了，并且出现在前台并开始活动。而 onStart 的时候 Activity 还在后台，onResume 的时候 Activity 才显示到前台。    

（5）onPause()：  
表示 Activity 正在停止，仍可见，正常情况下，紧接着 onStop 就会被调用。onPause 中不能进行耗时操作，会影响到新 Activity 的显示。因为 onPause 必须执行完，新的 Activity 的 onResume 才会执行。    

（6）onStop()：    
表示 Activity 即将停止，不可见，位于后台。同样不能太耗时。    

（7）onDestory()：    
表示 Activity 即将销毁，这是 Activity 生命周期的最后一个回调，可以做一些回收工作和最终的资源回收。 

2. 生命周期的几种普通情况   
（1）针对一个特定的 Activity，第一次启动，回调如下：onCreate() -> onStart() -> onResume()     
（2）用户打开新的 Activity 的时候，上述 Activity 的回调如下：onPause() -> onStop()    
（3）再次回到原 Activity 时，回调如下：onRestart() -> onStart() -> onResume()     
（4）按 back 键回退时，回调如下：onPause() -> onStop() -> onDestory()     
（5）按 Home 键切换到桌面后又回到该 Activity，回调如下：onPause() -> onStop() -> onRestart() -> onStart() -> onResume()      
（6）调用 finish() 方法后，回调如下：onDestory()（以在 onCreate() 方法中调用为例，不同方法中回调不同，通常都是在 onCreate() 方法中调用）

3. 特殊情况下的生命周期  
两种特殊情况：  
（1）横竖屏切换    
在横竖屏切换的过程中，会发生 Activity 被销毁并重建的过程。    

**onSaveInstanceState 和 onRestoreInstanceState：**      
在 Activity 由于异常情况下终止时，系统会调用 onSaveInstanceState 来保存当前 Activity 的状态。这个方法的调用是在 onStop 之前，它和 onPause 没有既定的时序关系，该方法只在 Activity 被异常终止的情况下调用。当异常终止的 Activity 被重建以后，系统会调用 onRestoreInstanceState，并且把 Activity 销毁时 onSaveInstanceState 方法所保存的 Bundle 对象参数同时传递给 onRestoreInstanceState 和 onCreate 方法。因此，可以通过 onRestoreInstanceState 方法来恢复 Activity 的状态，该方法的调用时机是在 onStart 之后。其中 onCreate 和 onRestoreInstanceState 方法来恢复 Activity 的状态的区别：onRestoreInstanceState 回调则表明其中 Bundle 对象非空，不用加非空判断。onCreate 需要非空判断。建议使用 onRestoreInstanceState。  
横竖屏切换的生命周期：    
onPause() -> onSaveInstanceState() -> onStop() -> onDestory() -> onCreate() -> onStart() -> onRestoreInstanceState() -> onResume()    

可以通过在 AndroidManifest 文件的 Activity 中指定如下属性：    
```
android:configChanges = "orientation | screenSize" 
```  
来避免横竖屏切换时，Activity 的销毁和重建，添加这个属性后，会调用  
```
@Override
public void onConfigurationChanged(Configuration newConfig){
	super.onConfigurationChanged(newConfig);
}
```  

（2）资源内存不足导致优先级低的 Activity 被杀死  
Activity 优先级的划分和下面的 Activity 的三种运行状态是对应的。     
* 前台 Activity --- 正在和用户交互的 Activity，优先级最高    
* 可见但非前台 Activity --- 比如 Activity 中弹出了一个对话框，导致 Activity 可见但是位于后台无法和用户交互。    
* 后台 Activity --- 已经被暂停的 Activity，比如执行了 onStop，优先级最低。    
当系统内存不足时，会按照上述优先级从低到高去杀死目标 Activity 所在的进程。  

4. Activity 的三种运行状态  
（1）Resumed（活动状态）  
又叫 Running 状态，这个 Activity 正在屏幕上显示，并且有用户焦点。    
（2）Paused（暂停状态）    
这时一个比较不常见的状态，这个 Activity 在屏幕上是可见的，但是并不是在屏幕最前端的那个 Activity。比如有另一个非全屏或者透明的 Activity 是Resumed 状态，没有完全遮盖这个 Activity。  
（3）Stopped（停止状态）    
当 Activity 完全不可见时，此时 Activity 还在后台运行，仍然在内存中保留 Activity 的状态，并不是完全销毁。比如当跳转到另一个界面时，之前的界面还在后台，按回退按钮还会恢复原来的状态，大部分软件在打开的时候，直接按 Home 键，并不会关闭它，此时的 Activity 就是 Stopped 状态。  


### 二、Activity 的启动模式
1. 启动模式的类别  
Android 提供了四种 Activity 启动方式：    
标准模式（standard）    
栈顶复用模式（singleTop）    
栈内服用模式（singleTask）    
单例模式（singleInstance）    

2. 启动模式的结构--栈  
Activity 的管理是采用任务栈的形式，任务栈采用“后进先出”的栈结构。    

3. Activity 的 LaunchMode  
（1）标准模式（standard）  
每启动一次 Activity，就会创建一个新的 Activity 实例并置于栈顶，谁启动了这个 Activity，那么这个 Activity 就运行在启动它的那个 Activity 所在的栈中。    
例如：Activity A 启动了 Activity B，则就会在 A 所在的栈顶压入一个新的 Activity。    


特殊情况，如果在 Service 或 Application 中启动一个 Activity，其并没有所谓的任务栈，可以使用标记位 Flag 来解决。解决办法：为待启动的 Activity 指定 FLAG_ACTIVITY_NEW_TASK 标记位，创建一个新栈。  


应用场景：绝大多数 Activity，如果以这种方式启动的 Activity 被跨进程调用，在 5.0 之前新启动的 Activity 实例会放入发送 Intent 的 Task 的栈的顶部，尽管它们属于不同的程序，这似乎有点费解看起来也不是那么合理，所以在 5.0 之后，上述情景会创建一个新的 Task，新启动的 Activity 就会放入刚创建的 Task 中，这样就合理的多了。    

（2）栈顶复用模式（singleTop）  
如果需要新建的 Activity 位于任务栈栈顶，那么此 Activity 的实例就不会重建，而是重用栈顶的实例。并回调如下方法：    
```
@Override
	protected void onNewIntent(Intent intent) {
		super.onNewIntent(intent);
	}
```    
由于不会重建一个 Activity 实例，则不会回调其他生命周期方法。如果栈顶不是新建的 Activity，就会创建该 Activity 新的实例，并放入栈顶。    

应用场景：在通知栏点击收到的通知，然后需要启动一个 Activity，这个 Activity 就可以用 singleTop，否则每次点击都会新建一个 Activity。同 standard 模式，如果是外部程序启动 singleTop 的 Activity，在 Android 5.0 之前新创建的 Activity 会位于调用者的 Task 中，5.0 及以后会放入新的 Task 中。    

（3）栈内复用模式（singleTask）  
该模式是一种单例模式，即一个栈内只有一个该 Activity 实例。该模式，可以通过在 AndroidManifest 文件的 Activity 中指定该 Activity 需要加载到那个栈中，即 singleTask 的 Activity 可以指定想要加载的 目标栈。singleTask 和 taskAffinity 配合使用，指定开启的 Activity 加入到哪个栈中。    
```
<activity
            android:name=".Activity1"
            android:launchMode="singleTask"
            android:taskAffinity="com.test.task" />
```  

关于 taskAffinity 的值：每个 Activity 都有 taskAffinity 属性，这个属性指出了它希望进入的 Task。如果一个 Activity 没有显示的指明该 Activity 的 taskAffinity，那么它的这个属性就等于 Application 指明的 taskAffinity，如果 Application 也没有指明，那么该 taskAffinity 的值就等于包名。    

执行逻辑：在这种模式下，如果 Activity 指定的栈不存在，则创建一个栈，并把创建的 Activity 压入栈内。如果 Activity 指定的栈存在，如果其中没有该 Activity 实例，则会创建 Activity 并压入栈顶，如果其中有该 Activity 实例，则把该 Activity 实例之上的 Activity 杀死清除出栈，重用并让该 Activity 实例处在栈顶，然后调用 onNewIntent() 方法。    

应用场景：大多数 App 的主页。对于大部分应用，当我们在主界面点击回退按钮的时候都是退出应用，那么当我们第一次进入主界面之后，主界面位于栈底，以后不管我们打开了多少个 Activity，只要我们再次回到主界面，都应该将主界面 Activity 上所有的 Activity 移除的方式来让主界面 Activity 出于栈顶，而不是往栈顶新加一个主界面 Activity 的实例，通过这种方式能够保证退出应用时所有的 Activity 都能被销毁。在跨应用 Intent 传递时，如果系统中不存在 singleTask Activity 的实例，那么将创建一个新的Task，然后创建 SingleTask Activity 的实例，将其放入新的 Task 中。  

（4）单例模式（singleInstance）   
作为栈内复用模式（singleTask）的加强版，打开该 Activity 时，直接创建一个新的任务栈，并创建该Activity 实例放入新栈中。一旦该模式的 Activity 实例已经存在于某个栈中，任何应用再激活该 Activity 时都会重用该栈中的实例。  

应用场景：呼叫来电界面。这种模式的使用情况比较罕见，在 Launcher 中可能使用。或者你确定你需要使 Activity 只有一个实例。  

3. 特殊情况：前台栈和后台栈的交互  
假如目前有两个任务栈，前台任务栈为 AB，后台任务栈为 CD，这里假设 CD 的启动模式均为 singleTask，现在请求启动 D，那么这个后台的任务栈都会被切换到前台，这个时候整个后退列表就变成了 ABCD。当用户按 back 返回时，列表中的 activity 会一一出栈。    


如果不是请求启动D而是启动 C，那么情况又不一样，  

调用 SingleTask 模式的后台任务栈中的 Activity，会把整个栈的 Activity 压入当前栈的栈顶。singleTask 会具有 clearTop 特性，把之上的栈内 Activity 清除。    

4. Activity 的 Flags    
Activity 的 Flags 很多，在启动 Activity 时，通过 Intent 的 addFlags() 方法设置 Activity的启动模式。    
（1）FLAG_ACTIVITY_NEW_TASK 其效果与指定 Activity 为 singleTask 模式一致。    
（2）FLAG_ACTIVITY_SINGLE_TOP 其效果与指定 Activity 为 singleTop 模式一致。    
（3）FLAG_ACTIVITY_CLEAR_TOP 具有此标记位的 Activity，当它启动时，在同一个任务栈中所有位于它上面的 Activity 都要出栈。如果和 singleTask 模式一起出现，若被启动的 Activity 已经存在栈中，则清除其之上的 Activity，并调用该 Activity 的 onNewIntent 方法。如果被启动的 Activity 采用 standard 模式，那么该 Activity 连同之上的所有的 Activity 出栈，然后创建新的 Activity 实例并压入栈中。  
  

