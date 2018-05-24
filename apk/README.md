# APK 安装包


### 1.手机下载安装 APK 安装包提示：安装失败，未知原因。

#### 环境参数：

```
System:Xiaomi MIX 2/MIUI 9.5.6.0(ODECNFA)
Android Version:Android 8.0.0
```

#### 问题分析：

手机上显示 APK 安装包安装失败，我们通过其他方式要找到具体安装失败的原因，否则无从查证。一般手机端对普通用户来说，不会提供太过详细的原因，因为过于详细会涉及到很多专业技术细节，给用户带来理解压力，用户即使知道了这些技术细节，他也解决不了。

#### 解决方法：

将手机通过 USB 线连接电脑，打开调试模式，然后用 adb 命令来安装 APK 安装包，如果安装失败，就会有相关的详细提醒，可以在对应 APK 安装包的根目录底下，执行以下安装命令：

```
adb install test.apk
```

出现的错误提醒举例：  

```
adb: failed to install test.apk: Failure [INSTALL_FAILED_DUPLICATE_PERMISSION: Package *.v2n attempting to redeclare permission *.v2.permission.MIPUSH_RECEIVE already owned by *.v2]
```

以上安装失败给我们提供了更多细节的提醒，便于开发者查找和解决相关问题。

### 2.手机下载安装 APK 安装包提示：Failure [INSTALL_FAILED_DUPLICATE_PERMISSION: Package *.v2n attempting to redeclare permission *.v2.permission.MIPUSH_RECEIVE already owned by *.v2]

#### 环境参数：

```
System:Xiaomi MIX 2/MIUI 9.5.6.0(ODECNFA)
Android Version:Android 8.0.0
```

#### 问题分析：


当在配置文件中设置了自定义权限的保护级别为：`signature ` 的时候，如果你多个应用同时声明这个自定义权限，而这多个应用的签名不一样的话，后面的应用是安装不上去的。

```
<permission
    android:name="com.ifeegoo.permission.*"
    android:protectionLevel="signature" />
```

#### 解决方法：

确保相同的应用签名即可，一般会出现在你先直接用 Android Studio 直接安装一个应用，然后又用发布包的形式直接安装，建议在 `build.gradle` 文件中将 `debug` 和 `release` 签名文件同时设置成正式的签名文件，这样平时调试和测试也方便。
