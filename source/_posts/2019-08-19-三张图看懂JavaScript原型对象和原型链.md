---
title: 三张图看懂JavaScript原型对象和原型链
date: 2019-08-19 09:24:20
tags: 
- prototype
- __proto__
- 原型链
categories: JS/ES6
---

本文介绍了`prototype`和`__proto__`
> 参考地址：[水乙](https://www.cnblogs.com/shuiyi/p/5305435.html)

<!-- more -->

## `prototype`和`__proto__`的区别

![prototype和__proto__的区别](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-19-9-29-26.png)

```javascript
var a = {};
console.log(a.prototype);  //undefined
console.log(a.__proto__);  //Object {}

var b = function(){}
console.log(b.prototype);  //b {}
console.log(b.__proto__);  //function() {}
```

## `__proto__`的指向

![__proto__指向](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-19-9-30-24.png)

```javascript
/*1、字面量方式*/
var a = {};
console.log(a.__proto__);  //Object {}

console.log(a.__proto__ === a.constructor.prototype); //true

/*2、构造器方式*/
var A = function(){};
var a = new A();
console.log(a.__proto__); //A {}

console.log(a.__proto__ === a.constructor.prototype); //true

/*3、Object.create()方式*/
var a1 = {a:1}
var a2 = Object.create(a1);
console.log(a2.__proto__); //Object {a: 1}

console.log(a.__proto__ === a.constructor.prototype); //false（此处即为图1中的例外情况）
```

## 什么是原型链

![原型链](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2019-8-19-9-31-8.png)

```javascript
var A = function(){};
var a = new A();
console.log(a.__proto__); //A {}（即构造器function A 的原型对象）
console.log(a.__proto__.__proto__); //Object {}（即构造器function Object 的原型对象）
console.log(a.__proto__.__proto__.__proto__); //null
```
