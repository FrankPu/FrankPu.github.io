---
title: 写给自己看的Chameleon笔记
date: 2019-07-19 09:53:06
tags: 多端开发
categories: 其他前端
---

[Chameleon](https://cmljs.org/#/)框架是由滴滴开源的多端统一开发解决方案



<!-- more -->



## 视图层

> 数据展示层，是“身体”

### CMSS

#### 单位

使用cpx，规格以屏幕 750px视觉稿为标准，不支持%

#### 布局

默认flex布局，不需要手动为元素添加`display: flex;`属性，勿使用`float`

#### 盒模型

`box-sizing`默认为`border-box`，即宽度包含内容、内边距盒边框

#### 样式

**class：**静态、动态（勿传对象）
**内联样式：**

- 静态：

  ```
  <view style="border: 1px solid red"></view>
  ```

  

- 动态：

  ```
  <template>
    <view style="{{inlineStyle}}">
    </view>
  </template>
  <script>
  class Index {
    data = {
      inlineStyle: 'border: 1px solid red;'
    }
  }
  export default new Index();
  </script>
  ```

  

- 样式多态：

  ```
  <style lang="less">
  @media cml-type (web) {
    @import "./style1.less";
  }
  @media cml-type (weex) {
    @import "./style2.less";
  
  }
  @media cml-type (wx,alipay,baidu) {
    @import "./style3.less";
  
  }
  </style>
  ```

  

### CML

#### 组件属性：

```
<view id="item-{{id}}"> </view>
```



#### 运算：

```
<view hidden="{{flag ? true : false}}"> <text>Hidden </text> </view>

<view> <text>{{a + b}} + {{c}} + d </text></view>

<view c-if="{{length > 5}}"> </view>

class Index {
  data = {
    a: 1,
    b: 2,
    c: 3,
  }
}
export default new Index();

//view中的内容为 3 + 3 + d
```



为了提高微信端的渲染效率，在组件上加上`shrinkComponents`：

```
<component is="{{currentComp}}" shrinkComponents="comp,comp1"></component>
```



#### 指令

1. c-if
   如果c-if需要判断多个组件，则用block包起来

   ```
   <block c-if="{{true}}">
    <view> <text>view1 </text></view>
    <view> <text>view2 </text></view>
   </block>
   ```

2. c-for

   ```
   <view c-for="{{array}}" c-for-item="item" c-for-index="index" c-key="index"></view>
   ```



#### 事件

默认点击事件`c-bind:tap`

```
<view id="tapTest" data-hi="WeChat" c-bind:tap="tapName">

tapName() {
    typeof event.currentTarget.dataset.hi == "WeChat" // true
}

```



#### 组件通信

自定义事件名称不能存在大写字母
父组件：`c-bind:事件名=“methods”`
子组件：`this.$cmlEmit("事件名”, [参数])` ，与vue的emit类似



#### 类vue语法

为了降低学习成本，独立支持了vue的指令子集，你可以在模板添加一个lang属性`<template lang="vue">`即可使用。 注意必须在组件根元素上 template上加`lang='vue'`

## 逻辑层

> 用户操作层，是“灵魂”

### 生命周期

`beforeCreate` 实例初始化完成
`created` 数据和方法挂载完成
`beforeMount` 开始挂载已经编译完成的cml到对应节点
`mounted` cml模板编译完成，且渲已完成DOM染到
`beforeDestroy` 实例销毁之前
`destroyed` 实例销毁厚

### 配置

1. 组件配置

   ```
   <script cml-type="json">
   {
     "base":{
       "usingComponents": {
       //不要包含后缀名
         "navi": "/components/navi/navi",
         "c-cell": "/components/c-cell/c-cell",
         "c-list": "/components/c-list/c-list",
         "navi-npm": "cml-test-ui/navi/navi"
       }
     },
     "wx": {
       "navigationBarTitleText": "index",
        "backgroundTextStyle": "dark",
        "backgroundColor": "#E2E2E2",
        "component": true
     }
     "web": {
     },
     "weex": {
     }
   }
   </script>
   
   ```

2. 路由配置



## 组件

#### 基础内容

view（类div）, text（类p）, page（含titleBar的页面容器，只在页面使用），block（外层包装）, list/cell（类list下的li）

#### 布局容器

scroller（滑动器），container（类element的container），row/col（类element的row），carousel（走马灯）

#### 表单元素

- button: 
  open-type: getUserInfo, getPhoneNumber, openSetting
- input:
  type输入框类型: text, password, number
  return-key-type: 右下角回车键文字
- textarea: 和input差不多
- switch开关，radio单选，checkbox（和radio方法一致）

