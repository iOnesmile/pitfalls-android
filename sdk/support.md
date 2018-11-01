# Support 包


### 1.项目 Support 包的冲突问题。



#### 环境参数：

```
Android:8.0.0
MIUI:9.5.6.0(ODECNFA)
```

#### 问题分析：

问题出现在 Support 包冲突上，我们需要想办法保证 Support 包的独立性和唯一性。

#### 解决方法：

在 `build.gradle` 文件中通过以下声明来制定 Support 包的唯一性。

```
configurations.all {
    resolutionStrategy {
        force "com.android.support:support-v4:28.0.0"
    }
}
```
