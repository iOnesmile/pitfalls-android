#  Android Studio

## 1.Android Studio 导入项目不显示正常的结构。

原因分析：可能原有的项目是早期 Eclipse 开发的。目录结构没有和 Android Studio 结构对应上。

解决方法：通过 Android Studio 导入 Eclipse 工程，进行自动转换。

```
Import project (Gradle,Eclipse ADT,etc.)
```

## 2.AndroidManifest.xml 文件中声明 Activity 出现：is not a concrete class.

原因分析：一般情况下这个 Activity 是一个抽象类：abstract

解决方法：移除此类 Activity 的声明即可。
