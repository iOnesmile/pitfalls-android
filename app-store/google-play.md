# Google Play


### 1.Google Play 应用被拒提示：不得采用 Google Play 更新机制以外的其他任何方式修改、替换或更新应用本身的 APK 二进制代码

#### 环境参数：

```
https://play.google.com
```

#### 问题分析：

目前 Google Play 市场上严禁一切形式的应用自更新机制。

#### 解决方法：

去除一切更新机制，然后重新提交版本上传。


### 2.Google Play 搜索不到上线的应用

#### 环境参数：

```
https://play.google.com
```

#### 问题分析：

当用户在 Google Play 上搜索或浏览应用时，会根据哪些应用与其设备兼容来过滤搜索结果。例如，如果应用需要摄像头，Google Play 不会在无摄像头的设备上显示该应用。这种过滤帮助开发者管理其应用的分发，并且有助于确保为用户提供最佳的体验。

参考资料：  
1°.[过滤规则](https://developer.android.com/google/play/filters.html?hl=zh-cn])  
2°.[权限声明](https://developer.android.com/guide/topics/manifest/uses-feature-element.html?hl=zh-cn#permissions-features)

#### 解决方法：

去掉设备兼容过滤 ：例如 Nexus 7 平板电脑没有电话功能，然而在应用的 `AndroidManifest.xml` 文件里声明了电话相关的权限，希望不支持电话功能的设备也能搜索该应用， 就在 `AndroidManifest.xml` 文件添加如下代码 ：

```
<uses-feature 
	android:name="android.hardware.telephony" 
	android:required="false"/>
```

通过显式声明某项功能并加入 `android:required="false"` 属性，可以在 Google Play 上有效停用所有针对指定功能的过滤。

### 3.Google Play 应用被拒提示：您上传了可调式的APK或Android App Bundle。出于安全考虑，您需要先停用APK或Android App Bundle的调试功能，然后才能在Google Play 中进行发布。

#### 环境参数：

```
https://play.google.com
```

#### 问题分析：

在打包 APK 时，没有把 Log 日志关掉。或者是 `buildTypes` 中 `release` 的配置出现问题。

#### 解决方法：

1、在打包 `release` 版本APK的情况下，需要关闭日志。

2、如果在 `buildTypes` 中使用了多渠道分包技术，需要设置如：

```
buildTypes{
		
	demo.initWith(buildTypes.release)//打包前面时需要设置release,调试时设置为debug
	demo{	
			
	}
}
```




