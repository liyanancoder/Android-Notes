## 对 Context 的理解
Context 是一个抽象类，翻译为上下文，也可以理解为环境，是提供一些程序的运行环境基础信息，Context 下有两个子类，ContextWrapper 是上下文功能的封装类，而 ContextImpl 则是上下文功能的实现类。其中 ContextWrapper 又有三个直接的子类，ContextThemeWrapper、Service 和 Application，其中，ContextThemeWrapper 是一个带主题的封装类，它有一个直接子类就是 Activity，所以 Activity 和 Service 以及 Application 的 Context 是不一样的，只有 Activity 需要主题，Service 不需要主题。Context 一共有三种类型，分别是 Application、Activity 和 Service，这三个类虽然分别承担着不同的作用，但是它们都属于 Context 的一种，而它们具体 Context 的功能则是由 ContextImpl 类去实现的，因此在绝大多数场景下，Activity、Service 和 Application 这三种类型的 Context 都是可以通用的，不过有几种场景比较特殊，比如启动 Activity，还有弹出 Dialog，出于安全原因的考虑，Android 是不允许 Activity 或 Dialog 凭空出现的，一个 Activity 的启动必须要建立在另一个 Activity 的基础之上，也就是以此形成的返回栈，而 Dialog 则必须在一个 Activity 上面弹出（除非是 System Alert 类型的 Dialog），因此在这种场景下，我们只能使用 Activity 类型的 Context，否则将会出错。  
getApplicationContext() 和 getApplication() 方法得到的对象都是同一个 application 对象，只是对象的类型不一样。

Context 数量 = Activity 数量 + Service 数量 + 1 （1 为 Application）
