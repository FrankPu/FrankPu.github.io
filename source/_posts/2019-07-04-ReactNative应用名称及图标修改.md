---
title: ReactNative应用名称及图标修改
date: 2019-07-04 15:03:58
tags: ReactNative
categories: ReactNative
---

APP名称和图标的修改方法



<!-- more -->



## 修改APP名称

1. 在`android/app/src/main/AndroidManifest.xml`里，找到`android:label="@string/app_name"`，这是一个类似于定义好的变量，它调用的地方在`android\app\src\main\AndroidManifest.xml`里。

2. 修改`android/app/src/main/res/valuse/strings.xml`中的文本，就会改变APP在手机上显示的名称

   ```javascript
   <resources>
   	<string name="app_name">巡检系统</string>
   </resources>
   ```

   

## 修改APP图标

1. 进入目录`android/app/src/main/AndroidManifest.xml`，找到`android:icon="@mipmap/ic_launcher"`，它调用的地方在`android\app\src\main\res\mipmap-XXX`里。
2. 替换几个`mipmap-XXX`目录中的`png`图片即可，分别对应的分辨率为：48x48，72x72，96x96，144x144，192x192。

如果你`AndroidManifest.xml`文件里是`android:icon="@drawable/ic_launcher"`，那么修改`drawable-XXX`文件图片即可。