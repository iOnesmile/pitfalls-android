# Error

### 1.Java.lang.NoClassDefFoundError

#### 环境参数：

```
Android Version: below 5.0
```

#### 问题分析：

随着 Android 版本的推移，网络上已经出现了越来越多的第三方库，在我们开发项目的过程中，由于大量的引用第三方库，在 Android 5.0 以下的手机运行就闪退，甚至有的机型无法安装，例如引用了okhttp，Retrofit，环信，融云等都有可能会发现此异常。如果你同样也出现在 Android 5.0 以上可运行，Android 5.0 以下运行崩溃，可用此方法解决。

出现这个问题的主要原因是：方法数超 65536 限制。由于实际开发当中的需求不断变更,开源框架越来越多，大多都用第三方 SDK，导致方法数很容易超出 65536 限制。出现错误`Java.lang.NoClassDefFoundError`。

例如:

```
java.lang.NoClassDefFoundError:NoClassDefFoundError: okhttp3.OkHttpClient$Builder
```

测试 Android 6.0 设备没问题，Android 4.4 上就有问题了。导致出现以上错误崩溃。

当我查看源码并用 Debug 调试后，发现此类并没有什么问题。

这个错误是 Android 应用的方法总数限制造成的。Android 平台的 Java 虚拟机 Dalvik 在执行 DEX 格式的 Java 应用程序时，使用原生类型 short 来索引 DEX 文件中的方法。这意味着单个 DEX 文件可被引用的方法总数被限制为 65536。通常 APK 包含一个 classes.dex 文件，因此 Android 应用的方法总数不能超过这个数量，这包括 Android 框架、类库和你自己开发的代码。而 Android 5.0 和更高版本使用名为 ART 的运行时，它原生支持从 APK 文件加载多个 DEX 文件。在应用安装时，它会执行预编译，扫描 classes(..N).dex 文件然后将其编译成单个 .oat 文件用于执行. 通俗的讲，就是分包。


#### 解决方法：

使用自定义的 `Application` 继承 `MultiDexApplication` 这个类，或者重写 `Application` 的方法 `attachBaseContext()`，并调用 `MultiDex.install()`；

```
	@Override
    protected void attachBaseContext(Context base) {
        super.attachBaseContext(base);
        MultiDex.install(this);
    }
```

使用以上方法即可解决，如果无法编译请将你的 Gradle 版本配置如下：

```
	compileSdkVersion 23以上

    buildToolsVersion "23.0.0以上"

	defaultConfig {

          minSdkVersion 15

          targetSdkVersion 22

          // Enabling multidex support. 开关

              multiDexEnabled true

	}
```







