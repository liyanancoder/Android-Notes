### App 启动流程

App 启动时，AMS 会检查这个应用程序所需要的进程是否存在，不存在就会请求 Zygote 进程启动需要的应用程序进程，Zygote 进程接收到 AMS 请求并通过 fock 自身创建应用程序进程，这样应用程序进程就会获取虚拟机的实例，还会创建 Binder 线程池（ProcessState.startThreadPool()）和消息循环（ActivityThread looper.loop），然后 App 进程，通过 Binder IPC 向 system_server 进程发起 attachApplication 请求；system_server 进程在收到请求后，进行一系列准备工作后，再通过 Binder IPC 向 App 进程发送 scheduleLaunchActivity 请求；App 进程的 binder 线程（ApplicationThread）在收到请求后，通过反射机制创建目标 Activity，并回调 Activity.onCreate() 等方法，到此，App 便正式启动，开始进入 Activity 生命周期，执行完 onCreate/onStart/onResume 方法，UI渲染结束后便可以看到 App 的主界面。
