---
title: ReactNative环境搭建问题集
date: 2020-04-13 15:27:52
tags: 环境变量
categories: ReactNative
---

## 环境变量配置

**系统变量新增：**

1. key: `ANDROID_HOME`, value: `D:\Android\Sdk`(你的sdk所在目录)
2. key: `JAVA_HOME`, value: `C:\Program Files\Java\jdk1.8.0_241`(你的jdk所在目录)

**系统变量Path中新增：**

1. `%ANDROID_HOME%\platform-tools`
2. `%ANDROID_HOME%\emulator`
3. `%ANDROID_HOME%\tools`
4. `%ANDROID_HOME%\tools\bin`
5. `%JAVA_HOME%\bin`
6. `%JAVA_HOME%\jre\bin`

至此，你应该可以使用`javac`, `adb`, `keytool`等相关命令了。

<!-- more -->

## SDK Platforms

![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-15-10-06-51.png)
![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-15-10-07-49.png)
![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-15-10-08-11.png)
![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-15-10-08-46.png)
![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-15-10-09-07.png)
![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-15-10-09-20.png)

## SDK Tools

![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-15-10-10-08.png)
![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-15-10-11-01.png)

## 生成keystore

1. 进入`C:\Program Files\Java\jdk1.8.0_241\bin`，因为没有配置环境变量，不然可以直接在项目目录执行`keytool`操作，[环境变量配置参考]([http://pushishuang.top/2020/04/13/ReactNative%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA%E9%97%AE%E9%A2%98%E9%9B%86/](http://pushishuang.top/2020/04/13/ReactNative环境搭建问题集/))，**配置过的忽略本条**
2. **管理员身份**运行cmd，执行默认语句`keytool -genkey -v -keystore my-release-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000`
3. 将生成的文件改名为`my-release-key.keystore`，并拷贝至项目目录的`android/app/`下，这里的名字`my-release-key.keystore`和`android\gradle.properties`中的`MYAPP_RELEASE_STORE_FILE`对应，名字`my-key-alias`和`android\gradle.properties`中的`MYAPP_RELEASE_KEY_ALIAS`对应，都可以自定义
4. 执行打包命令即可

[参考地址](https://reactnative.dev/docs/signed-apk-android#generating-an-upload-key)

## metro-config sharedBlacklist regexp without scape "\"

> npx react-native run-android 时，启动报错

### 错误信息

```
error Invalid regular expression: /(.*\\__fixtures__\\.*|node_modules[\\\]react[\\\]dist[\\\].*|website\\node_modules\\.*|heapCapture\\bundle\.js|.*\\__tests__\\.*)$/: Unterminated character class. Run CLI with --verbose flag for more details.
SyntaxError: Invalid regular expression: /(.*\\__fixtures__\\.*|node_modules[\\\]react[\\\]dist[\\\].*|website\\node_modules\\.*|heapCapture\\bundle\.js|.*\\__tests__\\.*)$/: Unterminated character class
    at new RegExp (<anonymous>)
    at blacklist (C:\Users\***\react\myApp\node_modules\metro-config\src\defaults\blacklist.js:34:10)
    at getBlacklistRE (C:\Users\***\react\myApp\node_modules\@react-native-community\cli\build\tools\loadMetroConfig.js:69:59)
    at getDefaultConfig (C:\Users\***\react\myApp\node_modules\@react-native-community\cli\build\tools\loadMetroConfig.js:85:20)
    at load (C:\Users\***\react\myApp\node_modules\@react-native-community\cli\build\tools\loadMetroConfig.js:121:25)
    at Object.runServer [as func] (C:\Users\***\react\myApp\node_modules\@react-native-community\cli\build\commands\server\runServer.js:82:58)
    at Command.handleAction (C:\Users\***\react\myApp\node_modules\@react-native-community\cli\build\cliEntry.js:160:21)
    at Command.listener (C:\Users\***\react\myApp\node_modules\commander\index.js:315:8)
    at Command.emit (events.js:210:5)
    at Command.parseArgs (C:\Users\***\react\myApp\node_modules\commander\index.js:651:12)
```

### 原因

This bug is caused due to Node's parsing of regular expressions in Node versions >=12.11.0.

### 解决方法

使用[node-v12.0.0-x64.msi](https://npm.taobao.org/mirrors/node/v12.0.0/node-v12.0.0-x64.msi)

[参考地址](https://github.com/facebook/metro/issues/453)
