# Exception

#### 1.java.lang.RuntimeException: Parcelable encountered IOException writing serializable


#### 环境参数：

*

#### 问题分析：

Android 中的 `Activity` 传递数据时，为了方便往往将很多数据封装成对象，然后将整个对象传递过去。传对象的时候有两种情况，一种是实现 `Parcelable` 接口，一种是实现 `Serializable` 接口。可以用 `bundle.putSerializable（Key，Object）` 传递数据或者直接用 `intent putExtrr（Key，Object）` 传递数据。但当我们调用 `bundle.putSerializable（Key，Object）` 时如抛出此异常可用以下方法解决。

#### 解决方法：

1、抛出 `Java.io.NotSerializableException` 异常

抛出这个异常是因为你的对象没有实现 `Serializable` 接口，只要实现该接口就好了。

2、抛出 `java.lang.RuntimeException` 异常

抛出这个异常是因为传递的对象里面的对象也要实现 `Serializable` 接口。

### 2.android.util.SuperNotCalledException: Activity {*} did not call through to super.onCreate()

#### 环境参数：

*

#### 问题分析：

在 super.onCreate() 方法调用之前，有其他的代码终止，导致此方法不能执行。

#### 解决方法：

1.不要在 super.onCreate() 方法之前调用任何代码。同理其他的声明周期方法也是一样。



### 2.Unable to start activity ComponentInfo{*}: android.util.AndroidRuntimeException: requestFeature() must be called before adding content

#### 环境参数：

[参数描述]

#### 问题分析：

在 `setContentView` 之后调用 `requestWindowFeature` 会报错。这个是 Android UI 绘制的先后顺序问题。

#### 解决方法：

在 `setContentView` 之前调用 `requestWindowFeature` 即可。


