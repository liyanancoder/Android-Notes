## 对 Context 的理解

### 1. Context的作用：
Context 是应用组件的上下文，有了 Context 才可以方便地访问系统资源，调用系统服务。
```
public abstract class Context {
    ...
    public abstract Resources getResources();
    public abstract @Nullable Object getSystemService(String name);
    public abstract void startActivity(@RequiresPermission Intent intent);
    public abstract void sendBroadcast(@RequiresPermission Intent intent);
    ...
}
```

### 2. Context 的继承关系：
Context 下有两个子类，ContextWrapper 是上下文功能的封装类，而 ContextImpl 则是上下文功能的实现类。  

其中 ContextWrapper 又有三个直接的子类，ContextThemeWrapper、Service 和 Application，其中 ContextThemeWrapper 是一个带主题的封装类，它有一个直接子类就是 Activity，所以 Activity 和 Service 以及 Application 的 Context 是不一样的，只有 Activity 需要主题，Service 不需要主题。  

Context 一共有三种类型，分别是 Application、Activity 和 Service，这三个类虽然分别承担着不同的作用，但是它们都属于 Context 的一种，而它们具体 Context 的功能则是由 ContextImpl 类去实现的，因此在绝大多数场景下，Activity、Service 和 Application 这三种类型的 Context 都是可以通用的，不过有几种场景比较特殊，比如启动 Activity，还有弹出 Dialog，出于安全原因的考虑，Android 是不允许 Activity 或 Dialog 凭空出现的，一个 Activity 的启动必须要建立在另一个 Activity 的基础之上，也就是以此形成的返回栈，而 Dialog 则必须在一个 Activity 上面弹出（除非是 System Alert 类型的 Dialog），因此在这种场景下，我们只能使用 Activity 类型的 Context，否则将会出错。

### 3. Context的初始化流程
（1）Application 的初始化流程：  
ApplicationContext 是跟随 Application 一起初始化的，而 Application 又是跟随应用进程的启动初始化的，过程是创建应用进程，然后执行一个 java 类的入口函数，就是 ActivityThread 的 main 函数，这个函数会通知 AMS，应用进程启动了，然后 AMS 就会通知创建 Application，调用的是 handleBindApplication() 函数，这里首先要创建一个 Application 对象，然后执行它的 onCreate() 回调，application 对象的创建过程是调用 makeApplication() 函数，先通过 createAppContext() 函数创建一个 Context，然后调用 newApplication() 函数并将该 context 传进去，生成 application 对象。
```
private void handleBindApplication(AppBindData data) {
	...
	Application app = data.info.makeApplication(data.restrictedBackupMode, null);
	mInstrumentation.callApplicationOnCreate(app);
	...
}
```
```
public void callApplicationOnCreate(Application app) {
        app.onCreate();
    }
```
```
 public Application makeApplication(boolean forceDefaultAppClass,
            Instrumentation instrumentation) {
    ...
    ContextImpl appContext = ContextImpl.createAppContext(mActivityThread, this);
    app = mActivityThread.mInstrumentation.newApplication(
                    cl, appClass, appContext);
    ...
    return app;
  }
```
在 newApplication() 函数中先用 ClassLoader 加载 Application 类，然后调用 newInstance() 函数得到 application 对象，并通过 attachBaseContext() 函数给该对象赋予上下文。
```
 public Application newApplication(ClassLoader cl, String className, Context context)
            throws InstantiationException, IllegalAccessException, 
            ClassNotFoundException {
        return newApplication(cl.loadClass(className), context);
    }
    
static public Application newApplication(Class<?> clazz, Context context)
            throws InstantiationException, IllegalAccessException, 
            ClassNotFoundException {
        Application app = (Application)clazz.newInstance();
        app.attach(context);
        return app;
    }
```
另外 attach() 调用的是 attachBaseContext() 函数。  

其中 application 是继承 ContextWrapper的，而 ContextWrapper 又是继承 Context，在 attachBaseContext 函数中就是设置了 mBase 对象。
```
public class ContextWrapper extends Context {
    Context mBase;

    public ContextWrapper(Context base) {
        mBase = base;
    }
    
    protected void attachBaseContext(Context base) {
        if (mBase != null) {
            throw new IllegalStateException("Base context already set");
        }
        mBase = base;
    }

    public Context getBaseContext() {
        return mBase;
    }

    @Override
    public AssetManager getAssets() {
        return mBase.getAssets();
    }

    @Override
    public Resources getResources() {
        return mBase.getResources();
    }
    ···
 }
```

> Application 初始化的结论：  
继承关系，Application <- ContextWrapper <- Context
调用顺序，<init> -> attachBaseContext -> onCreate
ContextWrapper 里包含一个 Context，调用都委托给他了。

（2）Activity 的初始化流程：  
Activity 启动调用的就是 performLaunchActivity() 函数，首先就是 newActivity 创建 Activity 对象，也是通过 ClassLoader 加载类然后调用 newInstance() 创建 activity 对象。接着是调用 makeApplication() 函数返回 application 对象，然后调用 createBaseContextForActivity() 函数创建 Context，并将 context 赋予 activity，最后调用 onCreate() 生命周期。
```
private Activity performLaunchActivity(ActivityClientRecord r, Intent customIntent) {
    ...
    Activity activity = null;
    activity = mInstrumentation.newActivity(
                    cl, component.getClassName(), r.intent);

    Application app = r.packageInfo.makeApplication(false, mInstrumentation);
    ContextImpl appContext = createBaseContextForActivity(r);

    activity.attach();

    activity.onCreate();
    
    ...

    return activity;
}

```

Activity 继承自 ContextThemeWrapper，这点和 Application 不一样。
```
public class Activity extends ContextThemeWrapper{...}
```

> Activity的结论：
继承关系，Application <- ContextThemeWrapper <- ContextWrapper <- Context
调用顺序，<init> -> attachBaseContext -> onCreate

（3）Service 的初始化流程：  
Service 的创建也是通过 ClassLoader 加载类，然后调用 newInstance() 创建，然后调用 createAppContext() 函数创建 Context 对象，接着调用 makeApplication() 函数准备 application，然后调用 attach 将 context 和 application 赋予 service，然后调用 onCreate() 方法执行生命周期。
```
public abstract class Service extends ContextWrapper{...}
```
```
private void handleCreateService(CreateServiceData data) {
    ...
    Service service = null;
    service = (Service) cl.loadClass(data.info.name).newInstance();

    ContextImpl context = ContextImpl.createAppContext(this, packageInfo);
    context.setOuterContext(service);

    Application app = packageInfo.makeApplication(false, mInstrumentation);
    service.attach(context, this, data.info.name, data.token, app,
                    ActivityManager.getService());
    service.onCreate();
	...
}
```

其中 createAppContext() 函数中 new ContextImpl() 创建了 ContextImpl 对象。  
attach() 函数调用的是 attachBaseContext(context) 函数。

### 4. 与 Context 相关的问题
（1）应用里面有多少个 Context？ 不同的 Context 之间有什么区别？  
Context 的数量 = activity 数量 + service 数量 + application 数量    
因为 Activity 要显示 UI，继承的是 ContextThemeWrapper，而 Service 和 Application 继承自 ContextWrapper。  

（2）Activity 里的 “this” 和 getBaseContext 有什么区别？  
Activity 最终继承的是 Context，所以 this 是 Activity 自己。而 getBaseContext 返回的是 ContextWrapper 里面的 mBase。  

（3）getApplication 和 getApplicationContext 有什么区别？  
返回的都是 application 对象，但是 getApplication 是 Activity 和 Service 特有的。  

（4）应用组件的构造、onCreate()、attachBaseContext() 调用顺序？  
应用组件的构造函数 -> attachBaseContext() -> onCreate()  

### 5. Context 会引起的相关问题
**（1）Context 引起的内存泄漏**  
Context 用的不好有可能会引起内存泄漏的问题。  

示例1:     
**错误的单例模式：**    
```
public class Singleton { 
    private static Singleton instance; 
    private Context mContext;

    private Singleton(Context context) {
    	this.mContext = context;
    }

    public static Singleton getInstance(Context context) {
    	if (instance == null) {
           instance = new Singleton(context);
    }

    return instance;
    }
}
```

这是一个非线程安全的单例模式，instance 作为静态对象，其生命周期要长于普通的对象，其中也包含 Activity A 去 getInstance 获得 instance 对象，传入 this，常驻内存的 Singleton 保存了传入的 Activity A 对象，并一直持有，即使 Activity 被销毁掉，但因为它的引用还存在于一个 Singleton 中，就不可能被 GC 掉，这样就导致了内存泄漏。  

示例2:   
**View 持有 Activity 引用**    
```
public class MainActivity extends Activity { 
    private static Drawable mDrawable;

    @Override
    protected void onCreate(Bundle saveInstanceState) {
        super.onCreate(saveInstanceState);
    	setContentView(R.layout.activity_main);
    	ImageView iv = new ImageView(this);
    	mDrawable = getResources().getDrawable(R.drawable.ic_launcher);
    	iv.setImageDrawable(mDrawable);
    }

}
```

有一个静态的 Drawable 对象当 ImageView 设置这个 Drawable 时，ImageView 保存了 mDrawable 的引用，而 ImageView 传入的 this 是 MainActivity 的 mContext，因为被 static 修饰的 mDrawable 是常驻内存的，MainActivity 是它的间接引用，MainActivity 被销毁时，也不能被 GC 掉，所以造成内存泄漏。  

**（2）正确使用 Context**  
一般 Context 造成的内存泄漏，几乎都是当 Context 销毁的时候，却因为被引用导致销毁失败，而 Application 的 Context 对象可以理解为随着进程存在的，所以我们总结出使用 Context 的正确用法：  
- 当 Application 的 Context 能满足的情况下，并且生命周期长的对象，优先使用 Application 的 Context。
- 不要让生命周期长于 Activity 的对象持有到 Activity 的引用。
- 尽量不要在 Activity 中使用非晶态内部类，因为非静态内部类会隐式持有外部类实例的引用，如果使用静态内部类，将外部实例引用作为弱引用持有。


