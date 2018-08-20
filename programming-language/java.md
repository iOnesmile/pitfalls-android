# Java


### 1. Android 中 ProGuard 混淆代码的问题

1. Retrofit 定义的 Service 接口参数被清空，导致网络请求失败
2. 大多数包名都未被混淆，例如 utils、bean 的包名都存在
3. 打包 SDK 时，入口 API 被删除
4. 被 Gson 实例的 bean 类无法混淆

#### 环境参数：

> Gradle: gradle-4.1-all   
> android build: gradle:3.0.1

#### 问题分析：

1. 因为 Service 接口方法中的参数是被反射获取，ProGuard 的压缩（Shrinking）算法判断接口参数未被使用即会删除该参数。
2. 说明包下肯定有被 keep 的类，例如 Parcelable 子类，导致它的包名也被 keep。
3. 对外暴露的 API 类没有使用 keep 去避免混淆和压缩
4. Gson 的反序列化是通过成员变量名实现，混淆将导致反序列失败

#### 解决方法：

1. 使用 -keepclassmembers 关键字来保持方法不被混淆，例如：

	```proguard
	-keepclassmembers class com.fmxos.platform.http.api.** {<methods>;}
	```

2. 将不能被混淆的类放到指定不被混淆的包下，避免因为个别类影响整个库的混淆程度。  
	另外个别的类例如 View，想保持他们的关联性不移动包名，可以在混淆包下创建子类只重写构造函数，在其它创建的地方引用子类（如：layout）
	
3. 使用 `-keep` 来保持要暴露的 API 类，如果类中有太多实现细节不想被暴露，可以定义一个包装类来保留细节。

4. 使用 `-keepclassmembernames` 保持 `<fields>;` 不混淆。或通过 @SerializedName() 注解的别名不影响反序列化

	```proguard
	-keepclassmembernames class com.fmxos.platform.http.bean.** {<fields>;}	
	```
	
	或使用注解：
	
	```java
	@SerializedName("id")
	private int id; 
	```
	
