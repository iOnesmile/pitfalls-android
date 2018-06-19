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


### 4.编译提示：android-apt plugin is incompatible with the Android Gradle plugin. Please use 'annotationProcessor' configuration instead.


#### 环境参数：

```  
System:macOS High Sierra Version 10.13
IDE:Android Studio 3.1
Gradle:4.4
Gradle Plugin:3.1.0
```

#### 问题分析：

Gradle 版本不匹配，可以通过修改 Gradle 版本和 Gradle 插件版本来解决问题。

#### 解决方法：


修改以下两个文件：

1.`gradle-wrapper.properties`

`distributionUrl=https\://services.gradle.org/distributions/gradle-4.4-all.zip`

**to**

`distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip`

2.`build.gradle(Module: app)`

`classpath 'com.android.tools.build:gradle:3.1.0'`

**to**

`classpath 'com.android.tools.build:gradle:2.1.2'`


### 5.编译 Release 包提示：Attribute manifest@versionCode value=(183) from AndroidManifest.xml:5:5-30 is also present at AndroidManifest.xml:5:5-30 value=(182).Attributes of <manifest> elements are not merged.


#### 环境参数：

```  
System:macOS High Sierra Version 10.13
IDE:Android Studio 3.1
Gradle:2.10
Gradle Plugin:2.1.2
```

#### 问题分析：

Android 项目在编译的过程中，会将项目内部所有的 `AndroidManifest.xml` 文件进行合并，如果发现有相同元素不同的值，就会出现合并失败的情况。

#### 解决方法：

以上问题出现是为了尝试多渠道不同版本的应用生成，应该在 `build.gradle` 文件中采用 `flavor` 方式来处理，移除掉不同 `AndroidManifest.xml` 文件中的不同 versionCode 和 versionName。


### 6.配置多渠道，meta-data 字符串数据类型被转化成了 Float 类型。

#### 环境参数：

```  
System:macOS High Sierra Version 10.13
IDE:Android Studio 3.1
Gradle:2.10
Gradle Plugin:2.1.2
```

#### 问题分析：

`AndroidManifest.xml`

```
        <meta-data
            android:name="mi_push_appid"
            android:value="${mi_push_appid_value}" />

        <meta-data
            android:name="mi_push_appkey"
            android:value="${mi_push_appkey_value}" />
```

`build.gradle`

``` 
self {
      		manifestPlaceholders = [mi_push_appid_value : "1717000", mi_push_appkey_value:"1313131313"]
     }
```

像以上这种情况，Android Studio 编译一周，就会把上面的字符串类型的值转换成 Float 类型，数据就会发生变化。

#### 解决方法：

对 `meta-data` 的 value 值进行 `\0` 标记。

```AndroidManifest.xml
        <meta-data
            android:name="mi_push_appid"
            android:value="${mi_push_appid_value}\0" />

        <meta-data
            android:name="mi_push_appkey"
            android:value="${mi_push_appkey_value}\0" />
```


### 7.编译提示错误：Plugin with id 'com.android.application' not found

#### 环境参数：

```  
System:macOS High Sierra Version 10.13
IDE:Android Studio 3.1
Gradle:2.10
Gradle Plugin:2.1.2
```

#### 问题分析：

编译工具存在一定的问题。

#### 解决方法：

在 `build.gradle` 文件的最顶层追加以下编译代码：  

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


### 8.编译提示错误：Gradle sync failed: Could not find method google() for arguments [] on repository container.

#### 环境参数：

```  
System:macOS High Sierra Version 10.13
IDE:Android Studio 3.1
Gradle:2.10
Gradle Plugin:2.1.2
```

#### 问题分析：

Gradle 1.7 里面追加 `jcenter()` ，在之前的版本都会有这样的异常。

```
Gradle sync failed: Could not find method google() for arguments [] on repository container.
Consult IDE log for more details (Help | Show Log) (1m 2s 468ms)
```

#### 解决方法：

可以通过以下方式追加 `jcenter()`：


```
repositories {
	maven {
		url "https://jcenter.bintray.com"
	}
}
```

### 9.Gradle 编译警告：WARNING: Configuration '*' is obsolete and has been replaced with '**'.


#### 环境参数：

```  
System:macOS High Sierra Version 10.13
IDE:Android Studio 3.1
Gradle:2.10
Gradle Plugin:2.1.2
```

#### 问题分析：

使用 Android Studio 3.1 之后编译项目就有此种提示。

```
WARNING: Configuration 'compile' is obsolete and has been replaced with 'implementation'.
WARNING: Configuration 'androidTestCompile' is obsolete and has been replaced with 'androidTestImplementation'.
WARNING: Configuration 'androidTestApi' is obsolete and has been replaced with 'androidTestImplementation'.
WARNING: Configuration 'testCompile' is obsolete and has been replaced with 'testImplementation'.
WARNING: Configuration 'testApi' is obsolete and has been replaced with 'testImplementation'.
```

#### 解决方法：

将相关的 Gradle 文件中的关键词修改成提示的词就行了。

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

### 10.编译提示错误：Could not find com.android.support:multidex:1.0.3

#### 环境参数：

```  
System:macOS High Sierra Version 10.13
IDE:Android Studio 3.1
Gradle:2.10
Gradle Plugin:2.1.2
```

#### 问题分析：

```
Could not find com.android.support:multidex:1.0.3
```

#### 解决方法：

解决方法

```
repositories {
	maven {
		url 'https://maven.google.com'
	}
}
```

### 11.`MetaData` 获取的数据为空

#### 环境参数：

```  
System:macOS High Sierra Version 10.13
IDE:Android Studio 3.1
compileSdkVersion:22
```

#### 问题分析：

```
String channel = context.getPackageManager().getApplicationInfo(context.getPackageName(), PackageManager.GET_META_DATA).metaData.getString(name);
```

`metadata` 标签所在的位置层级不对。代码对应的 `metadata` 的层级是在 `application` 标签下面，而不是在 `manifest` 标签下面。

#### 解决方法：

将处在 `manifest` 标签下面的 `metadata` 移动到 `application` 标签底下。


### 12.声明 `Activity` 出现警告提示：is not a concrete class

#### 环境参数：

```  
System:macOS High Sierra Version 10.13
IDE:Android Studio 3.1
compileSdkVersion:22
```

#### 问题分析：

虽然提示此类警告，但是项目还是可以编译通过的，没有太大问题。但是对于有追求的程序员，要避免所有的警告才是能力的体现。出现这种警告，是这个 `Activity` 为抽象类导致的。

#### 解决方法：

移除此抽象类 `Activity` 的声明即可。