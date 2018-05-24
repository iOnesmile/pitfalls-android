# Android Studio

## IDE 配置问题

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

### 2. 提示文件资源文件名智能包含小写字母 a-z，数字 0-9 或者下划线。

```
/Users/user/Desktop/代码备份180305/A-haiyu翻译机/Android/translatormachinehy/app/src/main/res/mipmap-xhdpi/ic_language_Russia.png: Error: 'R' is not a valid file-based resource name character: File-based resource names must contain only lowercase a-z, 0-9, or underscore
```

问题原因：  
资源文件名称包含大写字母。

解决方法：  
文件名称修改成要求的命名规范。

### 3.提示没有引入 Gradle 管理。

问题原因：  
有的时候是由于没有在正确的根目录底下打开相关项目导致的。

解决方法：  
在包含 build.gradle 文件的根目录中打开项目。


### 4.提示无法导入某个模块，因为 .iml 文件缺失。

问题原因：  
1.有的时候因为之前移除掉了某个模块。

解决方法：  
1.Android Studio 会提示你是否移除 .iml 文件，如果觉得这个模块没有用到，可以点击移除。

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

