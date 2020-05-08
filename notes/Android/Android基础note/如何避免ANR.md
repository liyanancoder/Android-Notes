### 1. 什么是ANR
在 Android 上，如果你的应用程序有一段时间响应不够灵敏，系统会向用户显示一个对话框，这个对话框称作应用程序无响应（ANR：Application Not Responding）对话框。用户可以选择让程序继续运行。因此，在程序里对响应性能的设计很重要，这样系统就不会显示 ANR 给用户。  

### 2. ANR 的时间
不同的组件发生 ANR 的时间不一样，Activity 是 5 秒，BroadCastReceiver 是 10 秒，Service 是 20 秒（均为前台）。  

如果出现 ANR，可以通过查看 /data/anr/traces.txt。

### 3. 出现 ANR 的情况
（1）主线程被 IO 操作（从 4.0 之后网络 IO 不允许在主线程中）阻塞。  
（2）主线程中存在耗时的计算。  
（3）主线程中错误的操作，比如 Thread.wait 或者 Thread.sleep 等 Android 系统会监控程序等响应状况，一旦出现下面两种情况，则弹出 ANR 对话框。  
	* 应用在 5 秒内未响应用户的输入事件（如按键或者触摸）  
	* BroadcastReceiver 未在 10 秒内完成相关的处理  
	* Service 在特定的时间（20秒）内无法处理完成

### 4. 如何避免 ANR
（1）使用 AsyncTask 处理耗时 IO 操作  
（2）使用 Thread 或者 HandlerThread时，调用 Process.setThreadPriority(Process.THREAD_PRIORITY_BACKGROUND) 设置优先级，否则仍然会降低程序响应，因为默认 Thread 的优先级和主线程相同。  
（3）使用 Handler 处理工作线程结果，而不是使用 Thread.wait() 或者 Thread.sleep() 来阻塞主线程。
（4）Activity 的 onCreate() 和 onResume() 回调中尽量避免耗时的代码。BroadcastReceiver 中 onReceive 代码也要尽量减少耗时，建议使用 IntentService 处理。  
	将所有耗时操作都放到子线程中去，然后通过 handler.sendMessage 或者 runOnUIThread 或者 AsyncTask 或者 Rxjava 等方式更新 UI。


> [深入分析 ANR](https://mp.weixin.qq.com/s?__biz=MzIwMTAzMTMxMg==&mid=2649493643&idx=1&sn=34b51d1f61bd2ecaa8fd0a2d39c4d1d1&chksm=8eec9b74b99b126246acc4547597dfe55c836b8f689b2d1a65bdf1ee2054ced2fc070bfa2678&mpshare=1&scene=24&srcid=0116vzNfMMv2dLizhAT8mEYq)
