## Tencent Tinker

### 1.Could not get unknown property 'apkVariantData' for object of type

问题原因：Android Studio 3.0 以上的版本容易出现此问题，Tinker 出现不兼容。

问题解决：修改 Tinker Support 的版本：·`classpath "com.tencent.bugly:tinker-support:1.1.1"`

```
buildscript {
    repositories {
        mavenCentral()
        jcenter()
        google()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.0.1'

        classpath "io.realm:realm-gradle-plugin:4.3.4"

        classpath "com.tencent.bugly:tinker-support:1.0.8"
    }
}

repositories {
    google()
}
```

