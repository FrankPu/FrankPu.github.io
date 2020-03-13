---
title: 'call,aplly,bind的区别'
date: 2020-02-06 15:56:34
tags: call apply bind
categories: JS/ES6
---

> 在JS中，这三者都是用来**改变函数的this指向**，那他们有什么样的区别呢？

<!-- more -->

## 相同点

1. 都是用来改变函数的this对象的指向的。

2. 第一个参数都是this要指向的对象。

3. 都可以利用后续参数传参。

## 区别

```javascript
var xw = {
    name : "小王",
    gender : "男",
    age : 24,
    say : function() {
        alert(this.name + " , " + this.gender + " ,今年" + this.age);
    }
}

var xh = {
     name : "小红",
     gender : "女",
     age : 18
 }

xw.say();	// 输出结果为 “小王 ， 男 ， 今年24”
```

**那么如何用xw的`say()`方法来显示xh的数据呢？**

1. 对于`call`可以这样：`xw.say.call(xh);`

2. 对于`apply`可以这样：`xw.say.apply(xh);`

3. 对于`bind`可以这样：`xw.say.bind(xh)();`

   如果直接写`xw.say.bind(xh)`是不会有任何结果的，看到区别了吗？`call`和`apply`都是对函数的**直接**调用，而`bind`方法返回的仍然是一个函数，因此后面还需要()来进行调用才可以。

**那么`call`和`apply`有什么区别呢？**我们把例子稍微改写一下：

```javascript
var xw = {
    name : "小王",
    gender : "男",
    age : 24,
    say(school,grade) {
        alert(this.name + " , " + this.gender + " ,今年" + this.age + " ,在" + school + "上" + grade);
    }
}

var xh = {
     name : "小红",
     gender : "女",
     age : 18
}
```

可以看到`say()`方法多了两个**参数**，我们通过call/apply的参数进行传参：

1. 对于call来说是这样的：`xw.say.call(xh,"实验小学","六年级");`

2. 而对于apply来说是这样的：`xw.say.apply(xh,["实验小学","六年级"]);`

看到区别了吗，call后面的参数与say方法中是一一对应的，而apply的第二个参数是一个**数组**，数组中的元素是和say方法中一一对应的，这就是两者最大的区别。

**那么bind怎么传参呢？**它可以像call那样传参：

`xw.say.bind(xh,"实验小学","六年级")();`

但是由于bind返回的仍然是一个函数，所以我们也可以在调用的时候进行传参：

`xw.say.bind(xh)("实验小学","六年级");`

## 手写代码

```javascript
Function.prototype.myCall = function (obj) {
  obj.fn = this
  let args = [...arguments].splice(1)
  let result = obj.fn(...args)
  delete obj.fn
  return result
}

Function.prototype.myApply = function (obj) {
  obj.fn = this
  let args = arguments[1]
  let result
  if (args) {
    result = obj.fn(...args)
  } else {
    result = obj.fn()
  }

  delete obj.fn

  return result
}

Function.prototype.myBind = function (obj) {
  let context = obj || window
  let _this = this
  let _args = [...arguments].splice(1)

  return function () {
    let args = arguments
    // 产生副作用
    // return obj.fn(..._args, ...args)
    return _this.apply(context, [..._args, ...args])
  }
}

function myFun (argumentA, argumentB) {
  console.log(this.value)
  console.log(argumentA)
  console.log(argumentB)
  return this.value
}

let obj = {
  value: 'ziyi2'
}
console.log(myFun.myCall(obj, 11, 22))
console.log(myFun.myApply(obj, [11, 22]))
console.log(myFun.myBind(obj, 33)(11, 22))
```
