# UI 


### 1.小米 MIX 2 全面屏应用出现底部黑边情况。

![](/img/mix2-black-edge.png)


#### 环境参数：

```
Android:8.0.0
MIUI:9.5.6.0(ODECNFA)
```

#### 问题分析：

小米 MIX 2 全面屏是 18:9 比例，我们需要做特殊适配。

#### 解决方法：

在 `Application` 标签中做特殊声明适配。

解决方法：

```
    <application
        android:resizeableActivity="true">
```





