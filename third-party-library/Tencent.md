# 腾讯叮当SDK导入

### 1.java.lang.UnsatisfiedLinkError: No implementation found for void com.tencent.ai.sdk.jni.CommonInterface.aisdkSetQUA(java.lang.String, java.lang.String, java.lang.String, java.lang.String, java.lang.String, java.lang.String, java.lang.String, java.lang.String, int) (tried Java_com_tencent_ai_sdk_jni_CommonInterface_aisdkSetQUA and Java_com_tencent_ai_sdk_jni_CommonInterface_aisdkSetQUA__Ljava_lang_String_2Ljava_lang_String_2Ljava_lang_String_2Ljava_lang_String_2Ljava_lang_String_2Ljava_lang_String_2Ljava_lang_String_2Ljava_lang_String_2I)

#### 环境参数：

#### 问题分析：
不同结构的工程导致初始化语音对象失败

#### 解决办法：
问题关键点在于可根据 com_tencent_ai_sdk_jni 突破，因为包结构的不同，所以导致初始化失败，所以重新创建一个包,方法为： java 目录下创建com.tencent.ai.jni,然后把相应的类放入就可以了。

2.so 库编译时出现问题： 如果放在 jniLibs 下，要在 build.gradle 下添加如下代码： 
```
sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
        }
    }

如果还是编译不通过，可尝试把 so 文件直接放到libs目录下，记得在 build.gradle 添加如下代码：

     ndk {
            //选择要添加的对应cpu类型的.so库。
            abiFilters 'armeabi',"armeabi-v7a"
            // 还可以添加 'x86', 'x86_64', 'mips', 'mips64', 'armeabi-v7a', 'armeabi-v8a'
        }
        
3.科大讯飞 AIUI 导入 so 文件时出现 armeabi 文件夹少了某个 so 文件，解决办法为：直接到 armeabi-v7a 文件夹下找到提示少了的 so 文件，复制一份到 armeabi 下，此时编译就可以了
