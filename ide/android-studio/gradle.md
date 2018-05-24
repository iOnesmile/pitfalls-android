# Gradle 

### 1.修改应用包名失败

#### 环境参数：

```  
System:macOS High Sierra Version 10.13
IDE:Android Studio 3.1
Gradle:2.10
Gradle Plugin:2.1.2
```

#### 问题分析：

在 `build.gradle` 文件中声明了包名，`AndroidManifest.xml` 文件中也声明了包名，只在 `AndroidManifest.xml` 文件修改了包名，这种情况是不行的。`build.gradle` 里面的包名优先，`AndroidManifest.xml` 里面的包名和资源文件和包隔离有比较大关系。

#### 解决方法：

包名放到 `build.gradle` 里面声明，修改包名在 `build.gradle` 里面修改即可。`AndroidManifest.xml` 里面一般不用修改。

### 2.运行 `gradlew` 命令，提示：`./gradlew: Permission denied`

#### 环境参数：

```  
System:macOS High Sierra Version 10.13
IDE:Android Studio 3.1
Gradle:2.10
Gradle Plugin:2.1.2
```

#### 问题分析：

```
ifeegoo:android-bluetooth-color-lamp-chipsguide-ilight ifeegoo$ ./gradlew assembleDebug --info
-bash: ./gradlew: Permission denied
```

没有针对 `gradlew` 命令给出读写权限。

#### 解决方法：

在终端中输入：`chmod +x gradlew` 来调整 `gradlew` 命令的权限：

```
ifeegoo:android-bluetooth-color-lamp-chipsguide-ilight ifeegoo$ chmod +x gradlew
```


### 3.编译提示：Cannot evaluate module

#### 环境参数：

```  
System:macOS High Sierra Version 10.13
IDE:Android Studio 3.1
Gradle:2.10
Gradle Plugin:2.1.2
```

#### 问题分析：

```
Caused by: org.gradle.api.UnknownProjectException: Cannot evaluate module UpdateLibrary : Configuration with name 'default' not found.
```

之前删除的模块，在 settings.gradle 文件中并没有移除掉：

```
include ':app', ':BaseCloudMusicResource', ':UpdateLibrary', ':BaiduSDK_LibProject', ':bdSpeechLibrary'
```

#### 解决方法：

移除掉已经删除的模块即可。

```
include ':app', ':BaseCloudMusicResource', ':bdSpeechLibrary'
```


### 4.编译提示：

#### 环境参数：

[参数描述]

#### 问题分析：

[分析描述]

#### 解决方法：

[方法描述]

### 5.[问题关键内容描述]

#### 环境参数：

[参数描述]

#### 问题分析：

[分析描述]

#### 解决方法：

[方法描述]





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

### 8.编译提示错误：Plugin with id 'com.android.application' not found


原因分析：  
1.编译工具存在一定的问题。

解决方法：  
1.在 build.gradle 文件的最顶层追加以下编译代码：  

```
buildscript {
    repositories {
        google() // Gradle 4.0+
        maven { url "https://maven.google.com" } // Gradle < 4.0
    }
    dependencies {
        classpath "com.android.tools.build:gradle:3.1.2"
    }
}
```

参考资料：https://stackoverflow.com/questions/24795079/error1-0-plugin-with-id-com-android-application-not-found/25232725#25232725


### 1. Gradle 提示 google() 无法找到。

```
Gradle sync failed: Could not find method google() for arguments [] on repository container.
Consult IDE log for more details (Help | Show Log) (1m 2s 468ms)
```

问题原因：
Gradle 1.7 里面追加  jcenter() ，在之前的版本都会有这样的异常。

解决方法：
可以通过以下方式追加 jcenter()

```
repositories {
maven {
url "https://jcenter.bintray.com"
}
....
}
```


### 3.提示没有引入 Gradle 管理。

问题原因：
有的时候是由于没有在正确的根目录底下打开相关项目导致的。

解决方法：
在包含 build.gradle 文件的根目录中打开项目。


### 5.Gradle 编译关键词替换警告。

```
WARNING: Configuration 'compile' is obsolete and has been replaced with 'implementation'.
WARNING: Configuration 'androidTestCompile' is obsolete and has been replaced with 'androidTestImplementation'.
WARNING: Configuration 'androidTestApi' is obsolete and has been replaced with 'androidTestImplementation'.
WARNING: Configuration 'testCompile' is obsolete and has been replaced with 'testImplementation'.
WARNING: Configuration 'testApi' is obsolete and has been replaced with 'testImplementation'.
```

问题原因：
1.使用 Android Studio 3.1 之后编译项目就有此种提示。

解决方法：
1.将相关的 Gradle 文件中的关键词修改成提示的词就行了。

示例：

```
dependencies {
compile fileTree(dir: 'libs', include: ['*.jar'])
compile 'com.android.support:appcompat-v7:27.1.0'
compile 'com.android.support.constraint:constraint-layout:1.0.2'
testImplementation 'junit:junit:4.12'
androidTestImplementation 'com.android.support.test:runner:1.0.1'
androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.1'
}

```

修改成：

```
dependencies {
implementation fileTree(dir: 'libs', include: ['*.jar'])
implementation 'com.android.support:appcompat-v7:27.1.0'
implementation 'com.android.support.constraint:constraint-layout:1.0.2'
testImplementation 'junit:junit:4.12'
androidTestImplementation 'com.android.support.test:runner:1.0.1'
androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.1'

}
```

### 6.无法找到 Could not find com.android.support:multidex:1.0.3


```
Could not find com.android.support:multidex:1.0.3
```

解决方法

```
repositories {
maven {
url 'https://maven.google.com'
}
}
```
