---
title: ReactNative集成TypeScript
date: 2019-06-26 17:21:51
tags: 
- ReactNative
- TypeScript
categories: ReactNative
---

初始化项目的时候没有使用TypeScript，之后集成TS的笔记

<!-- more -->

1. 安装
`npm i --dev typescript react-native-typescript-transformer`
2. 在项目的根目录下创建一个文件`rn-cli.config.js`

```
module.exports = {  
      getTransformModulePath() {
        return require.resolve('react-native-typescript-transformer')
      },
      getSourceExts() {
        return ['ts', 'tsx'];
      }
}
```
3. 在项目根目录下创建一个文件`tsconfig.json`

```
{
      "compilerOptions": {
        "target": "es2015",
        "module": "es2015",
        "jsx": "react-native",
        "moduleResolution": "node",
        "allowSyntheticDefaultImports": true
      }
}
```
4. 修改`App.js`和`__tests__/App.js`文件名为`App.tsx`，并且修改文件第一行的引入：

```
// 将：
-import React, { Component } from 'react';
// 修改为：
+import React from 'react'
+import { Component } from 'react';
```

5. 添加类型声明依赖
`npm i --dev @types/react @types/react-native`


至此，结束！