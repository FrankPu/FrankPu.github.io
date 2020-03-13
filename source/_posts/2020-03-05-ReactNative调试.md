---
title: ReactNative调试
date: 2020-03-05 09:53:59
tags: 
categories: ReactNative
---

> ReactNative调试技巧

<!-- more -->

## 查看网络请求

在**项目根目录**，找到`index.js`，添加`GLOBAL.XMLHttpRequest = GLOBAL.originalXMLHttpRequest || GLOBAL.XMLHttpRequest;`，然后就可以在chrome Network查看网络请求了。
