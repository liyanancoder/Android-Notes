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


### 二、Activity的启动模式
1. 启动模式的类别

