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
2.修改 build.gradle 里面的应用包名。


