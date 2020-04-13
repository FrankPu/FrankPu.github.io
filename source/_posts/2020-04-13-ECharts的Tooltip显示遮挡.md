---
title: ECharts的Tooltip显示遮挡
date: 2020-04-13 15:34:54
tags: ECharts
categories: JS/ES6
---

> Echarts提示框（tooltip）浮层位置，不设置时，默认位置会跟随鼠标的位置。但是，当提示框位置超出图表所在区域时，就可能出现提示框显示不全的问题。

<!-- more -->

## 设置提示框位置的方法

### 方法一：固定position的位置

数组第一个元素设置距离父元素**左边**的距离，数组第二个元素设置距离父元素**上边**的距离。

这种方法设置的提示框位置固定不变，不会随鼠标移动而位置变化。

（1）数字设置绝对位置 position: [20, 20]

        tooltip: {
          trigger: 'axis',
          position: [20, 20] // 等价于 position: ['20px', '20px']
        },
（2）百分比设置相对位置 position: ['50%', '50%']

        tooltip: {
          trigger: 'axis',
          // 相对位置
          position: ['50%', '30%']
        },

### 方法二：设置position的值为：'inside'|'top'|'bottom'|'left'|'right'

这种方法只在设置`trigger: 'item'`的时候才有效。

`inside`  鼠标所在图形的内部中心位置

`top`  鼠标所在图形上侧

`left`  鼠标所在图形左侧

`right`  鼠标所在图形右侧

`bottom`  鼠标所在图形底侧

        tooltip: {
          trigger: 'item',
          position: 'top'
        },

### 方法三：通过回调函数设置提示框位置

回调函数中有**五个参数**和**一个返回值**：

**参数：**

1. `point`：鼠标位置，是一个数组，如 [20, 50]

2. `params`：Object|Array.<Object>  是需要的数据集

3. `dom`：tooltip 的 dom 对象。

4. `rect`：一个对象，只有鼠标在图形上时有效，是一个用  x, y, width, height   四个属性表达的图形包围盒。

5. `size`：一个对象，包括 dom 的尺寸和 echarts 容器的当前尺寸，例如：{contentSize: [width, height], viewSize: [width, height]}。

   size中有两个属性：viewSize为外层div的大小，contentSize为tooltip提示框的大小

**返回值：**

（1）返回值可以是一个表示 tooltip 位置的数组，数组值可以是绝对像素值，也可以是相对百分比。

        tooltip: {
          trigger: 'axis',
          position: function (point, params, dom, rect, size) {
            return ['40%', 30];
          }
        },
（2）返回值也可以是一个对象

对象的属性有：top、bottom、left、right

          position: function (point, params, dom, rect, size) {
            return {left: '40%', bottom: 20};
          }

## 设置提示框位置随鼠标移动，并解决提示框显示不全的问题

```
  position: function (point, params, dom, rect, size) {
    // 鼠标坐标和提示框位置的参考坐标系是：以外层div的左上角那一点为原点，x轴向右，y轴向下
    // 提示框位置
    var x = 0; // x坐标位置
    var y = 0; // y坐标位置

    // 当前鼠标位置
    var pointX = point[0];
    var pointY = point[1];

    // 外层div大小
    // var viewWidth = size.viewSize[0];
    // var viewHeight = size.viewSize[1];

    // 提示框大小
    var boxWidth = size.contentSize[0];
    var boxHeight = size.contentSize[1];

    // boxWidth > pointX 说明鼠标左边放不下提示框
    if (boxWidth > pointX) {
      x = 5;
    } else { // 左边放的下
      x = pointX - boxWidth;
    }

    // boxHeight > pointY 说明鼠标上边放不下提示框
    if (boxHeight > pointY) {
      y = 5;
    } else { // 上边放得下
      y = pointY - boxHeight;
    }

    return [x, y];
  }
```

[参考链接](https://blog.csdn.net/sleepwalker_1992/article/details/83023546)
