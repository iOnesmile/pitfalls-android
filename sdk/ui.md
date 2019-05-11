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



### 2. RecyclerView 中添加多个 HeaderView，刷新后顺序错乱

#### 环境参数：

网上的某个万能适配器，封装了添加多个 HeaderView 和 多个 FooterView 的 Adapter。相关代码如下：

```java
private SparseArray<View> mHeaderViews;
    
@Override
public int getItemViewType(int position) {
    if (isHeader(position)) {
        return TYPE_HEADER;
    }
    ...
    return TYPE_NORMAL;
}
```

#### 问题分析：

多个 HeaderView 共用一个 `TYPE_HEADER` 的 `ItemViewType` 值，页面刷新后根据 ItemViewType 从多个 HeaderView 中随机拿取一个，导致显示顺序错乱。

#### 解决方法：

使每一个 HeaderView 都对应唯一的 `ItemViewType`。避免与普通类型冲突，设置较大的基数与 `position` 计算出唯一的 Type，如：

```java
private static final int TYPE_HEADER = 0x10000;

@Override
public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
    if (viewType >= TYPE_HEADER) {
        return new SimpleViewHolder(mHeaderViews.get(viewType - TYPE_HEADER));
    }
    ...
}

@Override
public int getItemViewType(int position) {
    if (isHeader(position)) {
        return TYPE_HEADER + position;
    }
    ...
}
```



### 3. RecyclerView 滑动卡顿

#### 环境参数：

*

#### 问题分析：

内嵌了比较复杂的 ItemView，加载较耗时导致卡顿，如 Banner，RecyclerView 等

#### 解决方法：

将较费时的 ItemView 设置为不被回收属性，在 `onBindViewHolder()` 时如果数据未变不要重新渲染，如改成 HeaderView 类型（参考问题2的 Adapter 处理，不局限只显示在头部）。



### 4. RecyclerView 无法滑动，滑动事件异常或向上缩进显示不完整

#### 环境参数：

*

#### 问题分析：

内嵌了 RecyclerView，焦点被强占

#### 解决方法：

给该 RecyclerView 最外层的根布局加上两个属性:

```xml
android:focusableInTouchMode="true"
android:focusable="true"
```



### 5. 开启 Fragment 的动画无效，或卡顿

#### 环境参数：

示例代码如下：

```java
getFragmentManager().beginTransaction()
        .setCustomAnimations(enterAnim, exitAnim)
        ...
        .commit();
```

enterAnim.xml

```xml
<translate xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="300"
    android:fromXDelta="100.0%p"
    android:interpolator="@android:anim/decelerate_interpolator"
    android:toXDelta="0.0" />
```

#### 问题分析：

新开启的 Fragment 在创建时执行了耗时操作。   
如上 xml 代码所示，若执行代码的时长达到 300ms 则完全看不到动画效果，当小于 300ms 时则会看到卡顿

#### 解决方法：

1. 耗时非 UI 操作在子线程中执行，推荐使用 RxAndroid（如：Json解析、数据库查询、图片处理等）
2. 耗时 UI 相关操作，尽量延时到 300ms 后再执行（如：显示复杂的RecyclerView）



### 6. 带透明度的色值（ARGB）转换成不透明色值（RGB）

#### 环境参数：

*

#### 问题分析：

想象用一个白色透明的塑料袋遮住不同颜色的物体，看到背景泛白、塑料袋也有物体颜色，因此最终不透明色受背景色影响。这样就有一个简单的计算公式 `RGB = 前景色*透明度 + 背景色*(1-透明度)`，当然应先分解出 R、G、B 值，分别对每一项相乘，最后再组合。

#### 解决方法：

示例转换代码如下：

```java
// alpha 的最大值为 0xFF 表示完全不透明
public static int convertColorAlpha(int color, int bgColor, int alpha) {
    int red = Color.red(color)*alpha/0xFF + (Color.red(bgColor)*(0xFF-alpha))/0xFF;
    int green = Color.green(color)*alpha/0xFF + (Color.green(bgColor)*(0xFF-alpha))/0xFF;
    int blue = Color.blue(color)*alpha/0xFF + (Color.blue(bgColor)*(0xFF-alpha))/0xFF;
    return Color.argb(0xFF, red, green, blue);
}
```
### 7. textview 跑马灯失效和出现抖动的解决方法

#### 环境参数：

TextView的直接父布局为约束布局，当TextView进行跑马灯效果的时候会抖动以及显示不全文字

#### 问题分析：

可能是约束布局与TextView存在焦点冲突

#### 解决方法：

将TextView外面包裹一层其他布局，比如帧布局等