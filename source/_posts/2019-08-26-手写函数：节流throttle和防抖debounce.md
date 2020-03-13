---
title: 手写函数：节流throttle和防抖debounce
date: 2019-08-26 14:16:24
tags: 
- 节流
- 防抖
categories: JS/ES6
---
节流与防抖是**高频触发优化**方式

<!-- more -->

## 节流

当达到了一定的时间间隔就会执行一次

```javascript
function throttle (fn, delay) {
  let prev = Date.now();
  return function () {
    let context = this;
    let arg = arguments;
    let now = Date.now();
    if (now - prev >= delay) {
      fn.apply(context, arg)
      prev = Date.now();
    }
  }
}

function fn () {
  console.log('节流')
}

addEventListener('scroll', throttle(fn, 1000));
```

### 加上立即触发参数

常见使用场景：**页面滚动**和**resize**

```html
<body>
  <input oninput="handel(111)"></input>
</body>
<script>
function throttle(fn, delay, immediate) {
  let timer = null;
  retrun function () {
    let context = this;
    let arg = arguments;
    if (immediate) {
      fn.apply(context, args);
    } else {
      if (!timer) {
        timer = setTimeout(() => {
          fn.apply(context, args);
          timer = null;
        }, delay)
      }
    }
  }
}

let handel = throttle(checkVal, 1000, false)
function checkVal() {
  console.log('sss');
  console.log(arguments);
}
</script>
```

## 防抖

将若干函数调用合成为一次

```javascript
function debounce (fn, delay) {
  let timer = null;
  return function() {
    let context = this;
    let arg = arguments;
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(context, arg)
    }, delay)
  }
}

function fn () {
  console.log('防抖')
}

addEventListener('scroll', debounce(fn, 1000));
```

### 加上立即触发方参数

常见使用场景：**用户输入校验**

```html
<body>
  <input oninput="handel(111)"></input>
</body>
<script>
function debounce(fn, delay, immediate) {
  let timer = null
  return function () {
    let context = this;
    let args = arguments;
    if (immediate) {
      fn.apply(context, args);
    } else {
      if (timer) {
        clearTimeout(timer);
      }
      timer = setTimeout(() => {
        fn.apply(context, args);
      }, delay)
    }
  }
}

let handel = debounce(checkVal, 1000, false)
function checkVal() {
  console.log('sss');
  console.log(arguments);
}
</script>
```
