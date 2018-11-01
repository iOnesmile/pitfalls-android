# AndroidManifest.xml


### 1.`build.gradle` 文件中包含 `$applicationId` 的引用，但是却没有配置这个值。



#### 环境参数：

```
Android:8.0.0
MIUI:9.5.6.0(ODECNFA)
```

#### 问题分析：

应用安装导致冲突，可以通过 `adb install` 命令来排查安装错误。

```
ifeegoo:Downloads ifeegoo$ adb install android-ilight-v1.90-20180928-huawei_enc-by-ijiami.apk
adb: failed to install android-ilight-v1.90-20180928-huawei_enc-by-ijiami.apk: Failure [INSTALL_FAILED_CONFLICTING_PROVIDER: Package couldn't be installed in /data/app/com.chipsguide.app.colorbluetoothlamp.v2-1HhOmh5j0_MvKSeLlxwylg==: Can't install because provider name com.fmxos.platform.fmxos.fileProvider (in package com.chipsguide.app.colorbluetoothlamp.v2) is already used by com.chipsguide.app.colorbluetoothlamp.v3.changda]
```

原本这个问题是通过应用的 `applicationId` 来进行隔离的，后来发现 `applicationId` 的值并没有配置。只放到了 `AndroidManifest.xml` 文件中。

#### 解决方法：

在 `build.gradle` 文件中通过 `applicationId` 来声明包名。
