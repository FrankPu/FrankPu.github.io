---
title: 写给自己看的ReactNative在AMD锐龙平台下的初始化步骤
date: 2020-04-13 16:24:10
tags: 环境配置
categories: ReactNative
---

> 最近换了华为MagicBook Pro锐龙版，配置RN环境遇到一些问题

<!-- more -->

首先网上说的要开SVM MODE，但是我的笔记本进入BOIS并没有这个选项，我没有打开后来也成功了，可能是默认已经打开。

其次，很多提到WIN10的Hyper-V，我是家庭版，打开window功能也并没有此选项，我没有打开后来也成功了。

> 环境
>
> WIN10：家庭版，系统版本18363.752，版本号1909
>
> node：11.12.0
>
> android studio：3.61
>
> java 1.8.0_241
>
> python 2.7.15

下面是解决步骤：

1. 打开任务管理器，性能，如果虚拟化已经开启，即已开启SVM MODE。
2. 关闭Win10的`Hyper-V`，`Windows Hypervisor Platform（虚拟机平台、windows 虚拟机监控程序平台）`，`Windows Sandbox（windows 沙盒）`。（没有就跳过）
3. 进入：`$ANDROID_SDK_ROOT\extras\google\Android_Emulator_Hypervisor_Driver`，`$ANDROID_SDK_ROOT\`是Android SDK的安装路径，可以在**android studio**的**SDK Manager**中查看位置。
4. 运行目录中的**silent_install.bat**，如果执行结果返回是：STATE: 4 RUNNING，说明成功。

至此，就可以使用Android针对AMD处理器的高性能模拟器了。

[参考地址1](https://androidstudio.googleblog.com/2019/10/android-emulator-hypervisor-driver-for.html) [参考地址2](https://blog.csdn.net/a497785609/article/details/103832368)
