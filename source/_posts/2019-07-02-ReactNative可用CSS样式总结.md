---
title: ReactNative可用CSS样式总结
date: 2019-07-02 17:52:35
tags: 
- ReactNative
- CSS
categories: ReactNative
---

`ReactNative`的样式跟原生CSS样式有一定区别，且不支持部分原生CSS样式，下面是对RN常用样式的总结：

- ReactNative中样式采用驼峰写法

- View组件类似Div标签，包装容器，默认占用100%宽度，是最常用的块状元素
- Text组件类似Span标签，在被View包裹时候可设置padding、margin像块状

- 绝对定位和相对定位父元素不需要设置position和zindex
- 默认是Flex布局，方向是自上而下



<!-- more -->



## 常用属性



### Text 文本

| 属性名                      | 取值                                                         | 描述                                                         |
| :-------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| color                       | 颜色                                                         | 对应 `CSS` 中的 [color](http://css.doyoe.com/properties/color/color.htm) 属性 |
| fontFamily                  | string                                                       | 对应 `CSS` 中的 [font-family](http://css.doyoe.com/properties/font/font-family.htm) 属性 |
| fontSize                    | number                                                       | 对应 `CSS` 中的 [font-size](http://css.doyoe.com/properties/font/font-size.htm) 属性 |
| fontStyle                   | `normal`, `italic`                                           | 对应 `CSS` 中的 [font-style](http://css.doyoe.com/properties/font/font-style.htm) 属性，但阉割了 `oblique` 取值 |
| fontWeight                  | `normal`, `bold` `100~900`                                   | 对应 `CSS` 中的 [font-weight](http://css.doyoe.com/properties/font/font-weight.htm) 属性，但阉割了 `bolder, lighter` 取值 |
| lineHeight                  | number                                                       | 对应 `CSS` 中的 [line-height](http://css.doyoe.com/properties/text/line-height.htm) 属性 |
| textAlign                   | `auto`, `left`, `right`, `center`, `justify`                 | 对应 `CSS` 中的 [text-align](http://css.doyoe.com/properties/text/text-align.htm) 属性，增加了 `auto` 取值，当取值为 `justify` 时，在 `Android` 上会变为 `left` |
| textAlignVertical`Android`  | `auto`, `top`, `bottom`, `center`                            | 对应 `CSS` 中的 [vertical-align](http://css.doyoe.com/properties/text/vertical-align.htm) 属性，增加了 `auto` 取值，`center` 取代了 `middle`，并阉割了 `baseline, sub`等值 |
| includeFontPadding`Android` | boolean                                                      | Android在默认情况下会为文字额外保留一些padding，以便留出空间摆放上标或是下标的文字。对于某些字体来说，这些额外的padding可能会导致文字难以垂直居中。如果你把textAlignVertical设置为center之后，文字看起来依然不在正中间，那么可以尝试将本属性设置为false |
| textShadowColor             | 颜色                                                         | 对应 `CSS` 中的 [text-shadow](http://css.doyoe.com/properties/text-decoration/text-shadow.htm) 属性中的颜色定义 |
| textShadowOffset            | { width: number,  height: number }                           | 对应 `CSS` 中的 [text-shadow](http://css.doyoe.com/properties/text-decoration/text-shadow.htm) 属性中的阴影偏移定义 |
| textShadowRadius            | number                                                       | 在 `CSS` 中，阴影的圆角大小取决于元素的圆角定义，不需要额外定义 |
| letterSpacing`iOS`          | number                                                       | 对应 `CSS` 中的 [letter-spacing](http://css.doyoe.com/properties/text/letter-spacing.htm) 属性，但取值不同 |
| textDecorationColor`iOS`    | 颜色                                                         | 对应 `CSS` 中的 [text-decoration-color](http://css.doyoe.com/properties/text-decoration/text-decoration-color.htm) 属性 |
| textDecorationLine`iOS`     | `none`, `underline`, `line-through`, `underline line-through` | 对应 `CSS` 中的 [text-decoration-line](http://css.doyoe.com/properties/text-decoration/text-decoration-line.htm) 属性，但阉割了 `overline`, `blink` 取值 |
| textDecorationStyle`iOS`    | `solid`, `double`, `dotted`, `dashed`                        | 对应 `CSS` 中的 [text-decoration-style](http://css.doyoe.com/properties/text-decoration/text-decoration-style.htm) 属性，但阉割了 `wavy` 取值 |
| writingDirection`iOS`       | `auto`, `ltr`, `rtl`                                         | 对应 `CSS` 中的 [direction](http://css.doyoe.com/properties/writing-modes/direction.htm) 属性，增加了 `auto` 取值 |



### Dimension 宽高

| 属性名 | 取值   | 描述                          |
| :----- | :----- | :---------------------------- |
| width  | number | 对应 `CSS` 中的 `width` 属性  |
| height | number | 对应 `CSS` 中的 `height` 属性 |



### Positioning 定位

| 属性名   | 取值                   | 描述                                                         |
| :------- | :--------------------- | :----------------------------------------------------------- |
| position | `absolute`, `relative` | 对应 `CSS` 中的 `position` 属性，但阉割了 `static, fixed` 取值 |
| top      | number                 | 对应 `CSS` 中的 `top` 属性                                   |
| right    | number                 | 对应 `CSS` 中的 `right` 属性                                 |
| bottom   | number                 | 对应 `CSS` 中的 `bottom` 属性                                |
| left     | number                 | 对应 `CSS` 中的 `left` 属性                                  |



### Margin

| 属性名           | 取值   | 描述                                                         |
| :--------------- | :----- | :----------------------------------------------------------- |
| margin           | number | 对应 `CSS` 中的 `margin` 属性，不同的是，只能定义一个参数，用以表示`上、右、下、左`4个方位的外补白 |
| marginHorizontal | number | `CSS`中没有对应的属性，相当于同时设置marginRight和marginLeft |
| marginVertical   | number | `CSS`中没有对应的属性，相当于同时设置marginTop和marginBottom |
| marginTop        | number | 对应 `CSS` 中的 `margin-top` 属性                            |
| marginRight      | number | 对应 `CSS` 中的 `margin-right` 属性                          |
| marginBottom     | number | 对应 `CSS` 中的 `margin-bottom` 属性                         |
| marginLeft       | number | 对应 `CSS` 中的 `margin-left` 属性                           |



### Padding

| 属性名            | 取值   | 描述                                                         |
| :---------------- | :----- | :----------------------------------------------------------- |
| padding           | number | 对应 `CSS` 中的 `padding` 属性，不同的是，只能定义一个参数，用以表示`上、右、下、左`4个方位的内补白 |
| paddingHorizontal | number | `CSS`中没有对应的属性，相当于同时设置paddingRight和paddingLeft |
| paddingVertical   | number | `CSS`中没有对应的属性，相当于同时设置paddingTop和paddingBottom |
| paddingTop        | number | 对应 `CSS` 中的 `padding-top` 属性                           |
| paddingRight      | number | 对应 `CSS` 中的 `padding-right` 属性                         |
| paddingBottom     | number | 对应 `CSS` 中的 `padding-bottom` 属性                        |
| paddingLeft       | number | 对应 `CSS` 中的 `padding-left` 属性                          |



### Border

| 属性名                  | 取值                        | 描述                                                         |
| :---------------------- | :-------------------------- | :----------------------------------------------------------- |
| borderStyle             | `solid`, `dotted`, `dashed` | 对应 `CSS` 中的 `border-style` 属性，但阉割了 `none, hidden, double, groove, ridge, inset, outset` 取值，且无方向分拆属性 |
| borderWidth             | number                      | 对应 `CSS` 中的 `border-width` 属性                          |
| borderTopWidth          | number                      | 对应 `CSS` 中的 `border-top-width` 属性                      |
| borderRightWidth        | number                      | 对应 `CSS` 中的 `border-right-width` 属性                    |
| borderBottomWidth       | number                      | 对应 `CSS` 中的 `border-bottom-width` 属性                   |
| borderLeftWidth         | number                      | 对应 `CSS` 中的 `border-left-width` 属性                     |
| borderColor             | 颜色                        | 对应 `CSS` 中的 `border-color` 属性                          |
| borderTopColor          | 颜色                        | 对应 `CSS` 中的 `border-top-color` 属性                      |
| borderRightColor        | 颜色                        | 对应 `CSS` 中的 `border-right-color` 属性                    |
| borderBottomColor       | 颜色                        | 对应 `CSS` 中的 `border-bottom-color` 属性                   |
| borderLeftColor         | 颜色                        | 对应 `CSS` 中的 `border-left-color` 属性                     |
| borderRadius            | number                      | 对应 `CSS` 中的 `border-radius` 属性                         |
| borderTopLeftRadius     | number                      | 对应 `CSS` 中的 `border-top-left-radius` 属性                |
| borderTopRightRadius    | number                      | 对应 `CSS` 中的 `border-top-right-radius` 属性               |
| borderBottomLeftRadius  | number                      | 对应 `CSS` 中的 `border-bottom-left-radius` 属性             |
| borderBottomRightRadius | number                      | 对应 `CSS` 中的 `border-bottom-right-radius` 属性            |



### Shadow

| 属性名        | 取值                               | 描述                                                         |
| :------------ | :--------------------------------- | :----------------------------------------------------------- |
| shadowColor   | 颜色                               | 对应 `CSS` 中的 [box-shadow](http://css.doyoe.com/properties/border/box-shadow.htm) 属性中的颜色定义 |
| shadowOffset  | { width: number,  height: number } | 对应 `CSS` 中的 [box-shadow](http://css.doyoe.com/properties/border/box-shadow.htm) 属性中的阴影偏移定义 |
| shadowRadius  | number                             | 在 `CSS` 中，阴影的圆角大小取决于元素的圆角定义，不需要额外定义 |
| shadowOpacity | number                             | 对应 `CSS` 中的 [box-shadow](http://css.doyoe.com/properties/border/box-shadow.htm) 属性中的阴影透明度定义 |



### Background 背景

| 属性名          | 取值 | 描述                                    |
| :-------------- | :--- | :-------------------------------------- |
| backgroundColor | 颜色 | 对应 `CSS` 中的 `background-color` 属性 |



### Transform

> 这里要注意，transform的取值是对象组成的数组，且没有合并写法，translateX和translateY需要分开写。

| 属性名             | 取值                                                         | 描述                                                         |
| :----------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| transform          | `[{perspective: number}, {rotate: string}, {rotateX: string}, {rotateY: string}, {rotateZ: string}, {scale: number}, {scaleX: number}, {scaleY: number}, {translateX: number}, {translateY: number}, {skewX: string}, {skewY: string}]` | 对应 `CSS` 中的 `transform` 属性                             |
| transformMatrix    | `TransformMatrixPropType`                                    | 类似于 `CSS` 中 `transform` 属性的 `matrix()` 和 `matrix3d()` 函数 |
| backfaceVisibility | `visible`, `hidden`                                          | 对应 `CSS` 中的 `backface-visibility` 属性                   |



### Flex

| 属性名         | 取值                                                         | 描述                                                         |
| :------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| flex           | number                                                       | 对应 `CSS` 中的 `flex` 属性                                  |
| flexGrow       | number                                                       | 设置或检索弹性盒的扩展比率                                   |
| flexShrink     | number                                                       | 将子元素宽度之和与父元素宽度的差值按照子元素 flex-shrink 的值分配给各个子元素，每个子元素原本宽度减去按比例分配的值，其剩余值为实际宽度。 |
| flexBasis      | number                                                       | 设置或检索弹性盒伸缩基准值                                   |
| flexDirection  | `row`, `column`                                              | 对应 `CSS` 中的 `flex-direction` 属性，但阉割了 `row-reverse, column-reverse` 取值 |
| flexWrap       | `wrap`, `nowrap`                                             | 对应 `CSS` 中的 `flex-wrap` 属性，但阉割了 `wrap-reverse` 取值 |
| justifyContent | `flex-start`, `flex-end`, `center`, `space-between`, `space-around` | 对应 `CSS` 中的 `justify-content` 属性，但阉割了 `stretch` 取值。 |
| alignItems     | `flex-start`, `flex-end`, `center`, `stretch`                | 对应 `CSS` 中的 `align-items` 属性，但阉割了 `baseline` 取值。 |
| alignSelf      | `auto`, `flex-start`, `flex-end`, `center`, `stretch`        | 对应 `CSS` 中的 `align-self` 属性，但阉割了 `baseline` 取值  |



### 其他

| 属性名                | 取值                          | 描述                                                         |
| :-------------------- | :---------------------------- | :----------------------------------------------------------- |
| opacity               | number                        | 对应 `CSS` 中的 `opacity` 属性                               |
| overflow              | `visible`, `hidden`           | 对应 `CSS` 中的 `overflow` 属性，但阉割了 `scroll, auto` 取值 |
|                       | number                        | 对应 `CSS` 中的 `z-index` 属性                               |
| elevation`Android`    | number                        | `CSS`中没有对应的属性，只在 `Android5.0+` 上有效             |
| resizeMode            | `cover`, `contain`, `stretch` | `CSS`中没有对应的属性，可以参考 `background-size` 属性       |
| overlayColor`Android` | string                        | `CSS`中没有对应的属性，当图像有圆角时，将角落都充满一种颜色  |
| tintColor`iOS`        | 颜色                          | `CSS`中没有对应的属性，`iOS` 图像上特殊的色彩，改变不透明像素的颜色 |



## 颜色取值

`React-Native` 支持了 `CSS` 中大部分的颜色类型：

- `#f00` (#rgb)
- `#f00c` (#rgba)：`CSS` 中无对应的值
- `#ff0000` (#rrggbb)
- `#ff0000cc` (#rrggbbaa)：`CSS` 中无对应的值
- `rgb(255, 0, 0)`
- `rgba(255, 0, 0, 0.9)`
- `hsl(360, 100%, 100%)`
- `hsla(360, 100%, 100%, 0.9)`
- `transparent`
- `Color Name`：支持了 [基本颜色关键字](http://css.doyoe.com/appendix/color-keywords.htm#basic) 和 [拓展颜色关键字](http://css.doyoe.com/appendix/color-keywords.htm#extended)，但不支持 [28个系统颜色](http://css.doyoe.com/appendix/color-keywords.htm#system)；

## Units 单位

在 `React-Native` 中，并不支持百分比单位，目前只支持一种单位，即 `pt` 绝对长度单位，同时，你在定义时不需要加单位，例如：

```javascript
<View style={{width: 100, height: 50}}></View>
var styles = StyleSheet.create({
    box: {
        width: 100,
        height: 50
    }
});
```
