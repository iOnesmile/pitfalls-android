### 1.Android Gradle Plugin 与 Android SDK Build Tools 版本不兼容。

```
The specified Android SDK Build Tools version (25.0.0) is ignored, as it is below the minimum supported version (26.0.2) for Android Gradle Plugin 3.0.0.
Android SDK Build Tools 26.0.2 will be used.
To suppress this warning, remove "buildToolsVersion '25.0.0'" from your build.gradle file, as each version of the Android Gradle Plugin now has a default version of the build tools.
Message{kind=WARNING, text=The specified Android SDK Build Tools version (25.0.0) is ignored, as it is below the minimum supported version (26.0.2) for Android Gradle Plugin 3.0.0.
Android SDK Build Tools 26.0.2 will be used.
To suppress this warning, remove "buildToolsVersion '25.0.0'" from your build.gradle file, as each version of the Android Gradle Plugin now has a default version of the build tools., sources=[Unknown source file, Unknown source file]}
```

解决方法：  
1.按照相关提示，移除 build.gradle 文件中的 "buildToolsVersion '25.0.0'"。

### 2.修改应用包名不成功。

问题分析：  
1.可能是你在 build.gradle 文件中声明了包名，AndroidManifest.xml 文件中也声明了包名，只是在 AndroidManifest.xml 文件修改了。build.gradle 里面的包名优先。

解决方法：  
1.修改 build.gradle 里面的应用包名。

### 3.运行 gradlew 命令，提示权限被拒绝。

```
ifeegoo:android-bluetooth-color-lamp-chipsguide-ilight ifeegoo$ ./gradlew assembleDebug --info
-bash: ./gradlew: Permission denied
```

问题分析：  
1.没有给出相关的读写权限。

解决方法：  
1.通过命令行修改相关权限。

```
ifeegoo:android-bluetooth-color-lamp-chipsguide-ilight ifeegoo$ chmod +x gradlew
```

### 4.编译出现无法解析模块

```
Caused by: org.gradle.api.UnknownProjectException: Cannot evaluate module UpdateLibrary : Configuration with name 'default' not found.
```

问题分析：  
1.之前删除的模块，在 settings.gradle 文件中并没有移除掉：

```
include ':app', ':BaseCloudMusicResource', ':UpdateLibrary', ':BaiduSDK_LibProject', ':bdSpeechLibrary'
```

解决方法：  
1.移除掉已经删除的模块即可。

```
include ':app', ':BaseCloudMusicResource', ':bdSpeechLibrary'
```

### 5.提示 apt 版本不适配。

```
Error:android-apt plugin is incompatible with the Android Gradle plugin. Please use 'annotationProcessor' configuration instead.
```

问题分析：  
1.Gradle 版本的问题。

解决方法：  
1.更改 Gradle 对应的版本。

 Android Studio 3.1 + macOS 10.13.

修改以下两个文件：

1.`gradle-wrapper.properties`

`distributionUrl=https\://services.gradle.org/distributions/gradle-4.4-all.zip`

**to**

`distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip`

2.`build.gradle(Module: app)`

`classpath 'com.android.tools.build:gradle:3.1.0'`

**to**

`classpath 'com.android.tools.build:gradle:2.1.2'`

### 6.编译 Release 包的时候，提示版本冲突。

```
	Element uses-permission#android.permission.WRITE_EXTERNAL_STORAGE at AndroidManifest.xml:38:5-81 duplicated with element declared at AndroidManifest.xml:24:5-81
/Users/ifeegoo/workspace/android/android-bluetooth-color-lamp-chipsguide-ilight/app/src/none/AndroidManifest.xml:5:5-30 Error:
	Attribute manifest@versionCode value=(183) from AndroidManifest.xml:5:5-30
	is also present at AndroidManifest.xml:5:5-30 value=(182).
	Attributes of <manifest> elements are not merged.
/Users/ifeegoo/workspace/android/android-bluetooth-color-lamp-chipsguide-ilight/app/src/none/AndroidManifest.xml:6:5-31 Error:
	Attribute manifest@versionName value=(1.83) from AndroidManifest.xml:6:5-31
	is also present at AndroidManifest.xml:6:5-31 value=(1.82).
	Attributes of <manifest> elements are not merged.

```

原因分析：  
1.如果你的项目中包含多个 AndroidManifest.xml，无论是多渠道的目录底下，还是其他第三方模块的目录底下的，编译的时候，会合并 各个 AndroidManifest.xml 文件中的各个元素，当发现相同元素的时候，如果数值有冲突，就会编译冲突。

解决方法：  
1.相同元素，保持相同值。相同元素，多渠道不同值，请采用多渠道来避开。

### 7.配置多渠道，meta-data 数据类型问题。

```AndroidManifest.xml
        <meta-data
            android:name="mi_push_appid"
            android:value="${mi_push_appid_value}" />

        <meta-data
            android:name="mi_push_appkey"
            android:value="${mi_push_appkey_value}" />
```

``` build.gradle
self {
      manifestPlaceholders = [mi_push_appid_value : "1717000", mi_push_appkey_value:"33333333"]
     }
```

原因分析  
1.以上内容会报错，实际编译成功之后，mi_push_appid_value 和 mi_push_appkey_value 的值会被当成 float 类型。代码中获取相关参数时就会报错。

解决方法：  
1.对 meta-data value 值进行 `\0` 标记。

```AndroidManifest.xml
        <meta-data
            android:name="mi_push_appid"
            android:value="${mi_push_appid_value}\0" />

        <meta-data
            android:name="mi_push_appkey"
            android:value="${mi_push_appkey_value}\0" />
```

```


