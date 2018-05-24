#  Android Studio

## 1.Android Studio 导入项目不显示正常的结构。

原因分析：可能原有的项目是早期 Eclipse 开发的。目录结构没有和 Android Studio 结构对应上。

解决方法：通过 Android Studio 导入 Eclipse 工程，进行自动转换。

```
Import project (Gradle,Eclipse ADT,etc.)
```






# Android Studio

## IDE 配置问题



### 2. 提示文件资源文件名智能包含小写字母 a-z，数字 0-9 或者下划线。

```
/Users/user/Desktop/代码备份180305/A-haiyu翻译机/Android/translatormachinehy/app/src/main/res/mipmap-xhdpi/ic_language_Russia.png: Error: 'R' is not a valid file-based resource name character: File-based resource names must contain only lowercase a-z, 0-9, or underscore
```

问题原因：
资源文件名称包含大写字母。

解决方法：
文件名称修改成要求的命名规范。




### 4.提示无法导入某个模块，因为 .iml 文件缺失。

问题原因：
1.有的时候因为之前移除掉了某个模块。

解决方法：
1.Android Studio 会提示你是否移除 .iml 文件，如果觉得这个模块没有用到，可以点击移除。




