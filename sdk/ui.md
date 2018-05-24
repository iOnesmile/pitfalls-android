# UI 

### 1.小米 MIX 2 全面屏应用出现底部黑边情况。

![](/resources/Screenshot_2018-04-10-15-59-03-755_com.chipsguide.png)

原因分析：小米 MIX 2 全面屏是 18:9 比例，我们需要在 Application 标签中做特殊声明适配。

解决方法：

```
    <application
        android:resizeableActivity="true">
```
