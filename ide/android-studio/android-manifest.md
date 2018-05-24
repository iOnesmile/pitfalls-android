# AndroidManifest.xml

1.MetaData 获取的数据为空：

```
String channel = context.getPackageManager().getApplicationInfo(context.getPackageName(), PackageManager.GET_META_DATA).metaData.getString(name);
```

原因分析：  
1.metadata 标签所在的位置层级不对。

解决方法：  
1.以上代码对应的 metadata 的层级是在 application 标签下面，而不是在 manifest 标签下面。
