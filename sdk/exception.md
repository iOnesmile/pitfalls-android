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



### 3.Unable to start activity ComponentInfo{*}: android.util.AndroidRuntimeException: requestFeature() must be called before adding content

#### 环境参数：

[参数描述]

#### 问题分析：

在 `setContentView` 之后调用 `requestWindowFeature` 会报错。这个是 Android UI 绘制的先后顺序问题。

#### 解决方法：

在 `setContentView` 之前调用 `requestWindowFeature` 即可。


### 4. java.lang.IllegalStateException: Fatal Exception thrown on Scheduler.Worker thread.

#### 环境参数：

使用 RxAndroid + Retrofit 请求网络。打开新的 Fragment 后开始网络请求，当请求还没返回时（网络慢、手快等原因）就退出了页面，一会后出现此异常。

#### 问题分析：

页面退出后数据才返回，但是会继续回调更新页面，此时 `getContext()` 结果为 `null`，导致异常发生，并抛出在最顶层就变成了如上错误。   

#### 解决方法：

在页面关闭即 `onDestroy()` 时注销页面更新监听。   
最初开发时由于在回调更新接口并没有调用类似于 `getContext()` 的方法，即使退出也不会引发异常而忽视了注销操作。然而即使不出错，在页面退出后确不能即使回收也有内存泄露风险。

```java
Subscription subscription = observable.subscribeOn(Schedulers.io()).observeOn(AndroidSchedulers.mainThread())
            				.subscribe(...);

// 取消订阅，有多个 subscription 时，可以先添加到 CompositeSubscription 对象中
subscription.unsubscribe();
```
