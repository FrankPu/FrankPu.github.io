---
title: ReactNative环境搭建问题集
date: 2020-04-13 15:27:52
tags: 
categories: ReactNative
---

# metro-config sharedBlacklist regexp without scape "\"

> npx react-native run-android 时，启动报错

<!-- more -->

## 错误信息

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

## 原因

This bug is caused due to Node's parsing of regular expressions in Node versions >=12.11.0.

## 解决方法

使用[node-v12.0.0-x64.msi ](https://npm.taobao.org/mirrors/node/v12.0.0/node-v12.0.0-x64.msi)

[Github参考地址](https://github.com/facebook/metro/issues/453)
