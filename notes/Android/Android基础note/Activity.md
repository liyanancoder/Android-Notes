### 1. 横竖屏切换时候 Activity 的生命周期
不设置 Activity 的 android:configChanges 时，切屏会重新回调各个生命周期，切横屏时会执行一次，切竖屏时会执行两次。  
设置 Activity 的 android:configChanges = "orientation" 时，切屏还是会调用各个生命周期，切换横竖屏只会执行一次，  
设置 Activity 的 android:configChanges = "orientation|keyboardHidden" 时，切屏不会重新调用各个生命周期，只会执行 o nConfigurationChanged 方法
