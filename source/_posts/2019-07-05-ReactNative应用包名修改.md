---
title: ReactNative应用包名修改
date: 2019-07-05 14:45:16
tags: ReactNative
categories: ReactNative
---

包名是Android唯一的应用ID。

修改包名后，之前上传到应用商店的APP也就不是同一款了。



<!-- more -->



假设新的包名为`com.oc.objective`，下面几处地方是需要同步修改的：

## 两个Java文件

`android/app/src/main/java/com/PROJECT_NAME/MainActivity.java`

`android/app/src/main/java/com/PROJECT_NAME/MainApplication.java`

修改第一行的`package`为：

`package com.oc.objective;`



## 安卓描述文件

`android/app/src/main/AndroidManifest.xml`

修改第二行`package`为：

`package="com.oc.objective"`



### 两个打包脚本

1. `**android/app/BUCK**`

   里面有两处`package`，都修改为新的包名

   ```
   android_build_config(
       ...
       package = "com.oc.objective",
   )
   
   android_resource(
       ...
       package = "com.oc.objective",
   ...
   )
   ```

   

2. `**android/app/build.gradle**`

   修改`applicationId`

   ```
   defaultConfig {
       applicationId "com.oc.objective"
       ...
   }
   ```

   

修改完成后，命令行进入android目录，执行./gradlew clean清除缓存即可（windows上是 gradlew.bat）



## 更新Java文件目录

至此能够打包出正确包名的apk，不过在开发过程中我们都需要自动link原生模块，现在Java文件目录不正确会导致无法link成功，所以还需要按照Java的规范把Java文件目录放入包名匹配的目录中。

把`MainActivity.java`和`MainApplication.java`两个文件移到新创建的目录下：`android/app/src/main/java/com/oc/objective/`。

修改完成的目录结构为：

```
android/app/src/main/java/com/PROJECT_NAME/MainActivity.java
android/app/src/main/java/com/PROJECT_NAME/MainApplication.java
```

现在就可以自动link了。