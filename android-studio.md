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

