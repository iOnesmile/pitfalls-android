# SDK

#### 1.super.onCreate() 没有被调用。

```
android.util.SuperNotCalledException: Activity {*} did not call through to super.onCreate()
```

问题分析：  
1.在 super.onCreate() 方法调用之前，有其他的代码终止，导致此方法不能执行。

解决方法：  
1.不要在 super.onCreate() 方法之前调用任何代码。同理其他的声明周期方法也是一样。

#### 2.requestWindowFeature(Window.FEATURE_NO_TITLE) 调用错误。

```
Unable to start activity ComponentInfo{*}: android.util.AndroidRuntimeException: requestFeature() must be called before adding content
```

问题分析：  
1.在 setContentView 之后调用 requestWindowFeature 会报错。这个是 Android UI 绘制的先后顺序问题。

解决方法：  
1.在 setContentView 之前调用 requestWindowFeature 即可。
