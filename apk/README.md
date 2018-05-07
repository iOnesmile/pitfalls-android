# APK

#### 1.下载安装 apk 文件发现提示：安装失败，未知原因。

原因分析：  
1.我们要找到安装失败的详细原因，需要采用特殊方法安装。  

解决方法：  
1.通过 adb 命令来安装 apk 文件，如果安装失败，就会提示安装失败的详细原因。

```
adb install test.apk
```

出现的错误提醒举例：  
```
adb: failed to install test.apk: Failure [INSTALL_FAILED_DUPLICATE_PERMISSION: Package *.v2n attempting to redeclare permission *.v2.permission.MIPUSH_RECEIVE already owned by *.v2]
```

#### 2.安装 apk 文件提示：重复声明自定义权限。

原因分析：  
1.自定义权限只能在同一个应用中声明，如果别的应用也声明了，就需要修改自己的这个，或者修改自己的那个应用的权限。这个是用到小米推送的权限，我们新的应用必须对应新的推送权限命名。

解决方法：  
1.修改与包名结构对应的命名权限。
