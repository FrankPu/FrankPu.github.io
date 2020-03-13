---
title: react-native-file-selector安装遇到的问题
date: 2019-06-24 17:36:24
tags: 
- ReactNative
- 环境配置
categories: ReactNative
---

在安装react-native-file-selector时遇到的一些问题，特此记录

<!-- more -->

## 步骤

1. 安装`npm install react-native-file-selector --save`，之后千万不要link，不然会出错！

2. 在`android\settings.gradle`添加
  ```bash
  include ':react-native-file-selector'
  project(':react-native-file-selector').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-file-selector/android')
  ```

3. 在`android\build.gradle`添加
  ```bash
  maven { url "http://dl.bintray.com/lukaville/maven" }
  ```

4. 在`android\app\build.gradle`中添加
  ```bash
  implementation project(':react-native-file-selector')
  ```

5. 在`MainApplication.java`中引用jar包
  ```bash
  import ui.fileselector.RNFileSelectorPackage;
  ```
  在下面的`getPackages()`中使用
  ```bash
  new RNFileSelectorPackage(),
  ```

6. 新增主题文件`android/app/src/main/res/values/colors.xml`，可以自定义
  ```bash
  	<resources>
      <color name="colorPrimary">#3F51B5</color>
      <color name="colorPrimaryDark">#303F9F</color>
      <color name="colorAccent">#FF4081</color>
  </resources>
  ```

7. 在`android\app\src\main\AndroidManifest.xml`添加权限
  ```bash
  <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
  ```

8. 到这里算是完成了自动`link`的步骤，但是还有**bug**！！！错误提示：
  ```bash
  Android dependency 'com.android.support:support-v4' has different version for the compile (27.0.2) and runtime (28.0.0) classpath. You should manually set the same version via DependencyResolution
  ```
解决办法，修改我们第4步为：
  ```bash
  implementation (project(':react-native-file-selector')) {
       exclude group: 'com.android.support'
   }
  ```
  至此，结束！
