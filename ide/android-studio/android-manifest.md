# AndroidManifest.xml

### 1.`MetaData` 获取的数据为空

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


### 2.声明 `Activity` 出现警告提示：is not a concrete class

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


