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
