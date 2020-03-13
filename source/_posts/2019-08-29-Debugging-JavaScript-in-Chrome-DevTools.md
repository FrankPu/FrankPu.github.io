---
title: Debugging JavaScript in Chrome DevTools
date: 2019-08-29 17:27:27
tags: Debug
categories: 其他前端
---

如何使用 Chrome DevTools Sources 来进行断点调试

<!-- more -->

## 什么是断点

通常，您可能希望停止执行代码，以便您可以逐行地查看特定的上下文。

一旦代码在断点处停止，我们就可以通过访问作用域，查看调用堆栈，甚至在运行时更改代码来进行调试。

## 如何设置断点

1. 打开**Sources**选项卡
2. `Ctrl + P`打开我们要调试的文件
3. 点击左侧的代码行数，来设置断点

![设置断点](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-16-4-14.gif)

如上图所示，我们可以在一行代码上**更深入**地设置断点，例如在一行代码里的不同语句。

我们设置了3个断点：

1. 第一个断点在代码定义时停止执行
2. 第二个断点将在 `priceReceived` **函数执行之前**停止
3. 第三个断点将在 `priceReceived` **被调用后**立即停止，因此我们就**可以检查箭头函数的返回值**

当**调用**箭头函数时，执行**停止**，右侧面板 `Scope` 将显示**当前的上下文**，并允许我们访问所有我们想查看的值。

如下图所示，我们可以看到变量 `price` 的值：

![Scope查看上下文](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-16-10-47.png)

在下图中，一旦 `priceReceived` 函数执行，**第三个**断点就会被使用。

在右侧面板中您可以使用 `Return value` 查看匿名函数的返回值：

![Return value](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-16-13-7.png)

## 临时取消断点

场景：有时候你在代码中设置了多个断点，当刷新页面的时候，就会逐个执行每个断点，浪费很多时间。

这时，你可以临时**暂停所有断点**的执行，通过切换下图中的图标来操作：

![暂停所有断点](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-16-16-55.png)

## 错误捕捉

场景：你的代码执行产生了错误，但你不知道何时会抛出错误。

这时，你可以设置**执行错误时停止**，通过切换下图中的图标来操作：

![错误捕捉](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-16-21-27.gif)

## 条件断点

就是仅在条件为**真**时触发的断点。

例如上面的示例中，用户可以在文本区域中输入**非数字**。由于 `JS` 的兼容性只会显示 `NaN` 而不会抛出错误。

场景：您的代码比上面的代码更复杂，并且无法确定何时出现 `NaN` ，不能及时复现错误。

操作步骤如下：

![设置条件断点](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-16-26-40.gif)

1. **右键单击**要添加断点的代码行
2. 单击`Add conditional breakpoint…`
3. 添加有效的**JS表达式**。当然，在调用表达式时，您可以引用参数 x 和 y
4. 当表达式为**真**时，断点将被触发

## 逐步执行代码

使用 `Dev Tools` 中的 `navigator` 可以**按顺序逐行**执行代码，包含**4种**方式：

1. `Step`按钮
2. `Step over next function call`按钮
3. `Step Into Next function call`按钮
4. `Step Out of function call`按钮

### 单步执行代码

若点击 `Step` 按钮将按时间顺序移动到下一行：

![Step](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-16-32-37.gif)

### 跳过下一个函数调用

若点击 `Step over next function call` 按钮，也会顺序执行代码，但**不会进入函数调用**。也就是说，函数调用将被跳过，除非您在函数中设置了断点，否则调试器将不会在该函数中停止。

![Step over next function call](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-16-36-4.gif)

如上图所示：`multiplyBy` 和 `renderToDOM` 这两个方法执行时没有像 `Step` 那样进入函数内部。

### 进入下一个函数调用

若点击 `Step Into Next function call` 按钮，它类似于`Step`，不同之处在于，**当进入异步代码时，它将停止在异步代码中**，而不是按时间顺序运行的代码：

![Step Into Next function call](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-16-39-41.gif)

如上图所示：如果按时间顺序，第`32`行应该已经运行，但事实并非如此。调试器在等待2秒后才移动到第`29`行。

### 退出函数调用

假设调试代码时，您不想进入某个函数的内部，`Step Out of function call` 允许您**退出函数**并在**函数调用后的下一行**停止。

![Step Out of function call](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-16-43-2.gif)

如上图所示：

1. 代码在第`36`行的断点停了下来
2. 然后**退出**了函数 `renderToDOM`
3. 调试器**直接移到**第`29`行并**跳过** `renderToDOM` 函数的**剩余**部分

## 全局存储变量

有时，将某些值（例如组件类，大型数组或复杂对象）在**全局范围**内存储，会非常有用。

例如，当您想要传入不同的参数调到某个组件的方法时，在调试过程中将这些参数添加到全局范围可以节省大量时间。

![全局存储变量](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-17-4-57.gif)

如上图所示：我将数组 `[previous, current]` 存为**全局变量**。开发者工具会**自动分配**一个名为 `temp{n}` 的变量，`n` 基于先前保存的变量的数目。

事例中变量被命名`temp2`，您可以在控制台中使用它，因为它现在已是一个全局变量了！

如果您仔细观察会发现，当我将保存的变量映射到字符串数组时，我没有按下 `Enter` 键，但结果立即显示在下一行。

## 查看调用堆栈

1. 在调用它们的函数中来回跳转
2. 在每个步骤检查它们的作用域

假设我们有一个简单页面和一个输入数字的脚本，并在页面上呈现数字乘以10。

我们将调用两个函数：一个用来做乘法，一个用来将结果渲染到页面中。

![查看调用堆栈](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-17-11-19.gif)

如上图所示：只需单击 `Call Stack` 窗格中的函数名称，我们就可以浏览它们的作用域。

如果您仔细观察会发现，每次我们从一个函数调用跳到另一个函数调用时，作用域都会保留，我们可以在这里对每一步进行分析！

## `Blackbox`脚本用于展平堆栈

`Blackboxing`脚本将通过从堆栈中排除特定的脚本或某些匹配模式的脚本来过滤调用堆栈。

例如，如果我有99%的时间只调试 `userland` 中的代码感兴趣，我可以在 `Blackbox` 中添加一个模式，将 `node_modules` 文件夹下的所有脚本过滤掉。

要通过 Blackbox 过滤一个脚本，有两种方法：

1. 右键单击 `Sources` 选项卡中的 `JS` 脚本，然后单击`Blackbox Script`
2. 转到Chrome设置页面，然后转到 `Blackboxing` 并单击 `Add Pattern…` 并输入您想要加入 `Blackbox` 的正则，在您想要过滤大量脚本时很有用。

![Blackbox](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-17-14-39.png)

## 监视表达式

通过监视表达式，您可以定义一些 `JS` 语句，在开发者工具运行显示这些语句的结果。这是一个特别有趣的工具，因为您可以写任何您想要的虚拟情况，只要它是一个有效的 `JS` 表达式。

例如，您可以编写一个结果始终为 `true` 的表达式，当表达式结果为 `false` 时 ，您就可以发现当前的运行状态存在问题。

需要注意地方：

- 当我们使用断点进行调试时，监视表达式将被立刻执行，不需要刷新页面
- 如果代码在正常运行时，则需要手动单击刷新按钮

![监视表达式](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-29-17-19-41.png)

## 参考地址

- [Get Started with Debugging JavaScript in Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools/javascript/)
- [Console Overview](https://developers.google.com/web/tools/chrome-devtools/console/)
- [Debugging Javascript Like a Pro](https://blog.bitsrc.io/debugging-javascript-like-a-pro-a2e0f6c53c2e)
- [掘金](https://juejin.im/post/5d18d6eb6fb9a07edc0b6cc4)
