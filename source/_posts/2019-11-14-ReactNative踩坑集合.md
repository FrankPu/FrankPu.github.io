---
title: ReactNative踩坑集合
date: 2019-11-14 16:57:22
tags:
categories: ReactNative
---

RN过程中遇到的一些问题。

<!-- more -->

## `Execution failed for task ':xxxx:compileDebugJavaWithJavac`

遇到这种跟`compileDebugJavaWithJavac`相关的问题，是因为AndroidX引起的。

**解决办法：**

使用[jetifier](https://github.com/mikehardy/jetifier)：

执行命令：

```
npm i jetifier
npx jetify -r
```

然后更新`package.json`中`scripts`：

```
"scripts": {
  ....
  "postinstall": "jetify",
  "jetify": "npx jetify -r"
}
```

以后如果重装node_modules后，记得`npm run jetify`一下。

> [无限期的延迟AndroidX的迁移](https://github.com/mikehardy/jetifier#to-reverse-jetify--convert-node_modules-dependencies-to-support-libraries)

## `Warning: setState(...): Can only update a mounted or mounting component. This usually means you called setState() on an unmounted component. This is a no-op.`

在切换路由时报以上错误，是因为在组件挂载（mounted）之后进行了**异步**操作，比如`ajax`请求或者设置了**定时器**等，而你在`callback`中进行了`setState`操作。当你切换路由时，组件已经被**卸载**（unmounted）了，此时**异步**操作中`callback`还在执行，因此setState没有得到值。

**解决办法：**

1. 在卸载的时候对所有的操作进行清除（例如：abort你的ajax请求或者清除定时器）

    ```javascript
      componentDidMount = () => {
        //1.ajax请求
        $.ajax('你的请求',{})
            .then(res => {
                this.setState({
                    aa:true
                })
            })
            .catch(err => {})
        //2.定时器
        timer = setTimeout(() => {
            //dosomething
        },1000)
      }
      componentWillUnMount = () => {
          //1.ajax请求
          $.ajax.abort()
          //2.定时器
          clearTimeout(timer)
      }
    ```

2. 设置一个flag，当unmount的时候重置这个flag

    ```javascript
    componentDidMount = () => {
        this._isMounted = true;
        $.ajax('你的请求',{})
            .then(res => {
                if(this._isMounted){
                    this.setState({
                        aa:true
                    })
                }
            })
            .catch(err => {})
    }
    componentWillUnMount = () => {
        this._isMounted = false;
    }
    ```

3. 最简单的方式（万金油）

    ```javascript
      componentDidMount = () => {
        $.ajax('你的请求',{})
        .then(res => {
            this.setState({
                data: datas,
            });
        })
        .catch(error => {

        });
      }
      componentWillUnmount = () => {
          this.setState = (state,callback)=>{
            return;
          };
      }
    ```

## Unable to load script from assets 'index.android.bundle'. Make sure your bundle is packaged correctly or you're running a packager server

**解决办法：**

1. 创建assets目录
   `mkdir android/app/src/main/assets`
2. 执行命令
   `react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res/`
