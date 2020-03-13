---
title: 写给自己看的react-native-pushy使用笔记
date: 2019-06-21 16:35:43
tags: 
- ReactNative
- 环境配置
categories: ReactNative
---

本文是根据[react-native-pushy](https://github.com/reactnativecn/react-native-pushy)文档，使用后的一些笔记整理

<!-- more -->

## 环境配置

[环境配置](https://reactnative.cn/docs/getting-started/)

> 环境：React Native 版本0.58，react-native-update 版本5.1.8
>
> 手机系统：Android

## 安装

```javascript
npm i -g react-native-update-cli
npm i react-native-update
react-native link react-native-update
```

然后在目录`android\app\src\main\java\app\ytd\dg\MainApplication.java`中，添加代码：

```javascript
// ... 其它代码

// 添加这个import
import cn.reactnative.modules.update.UpdateContext;

public class MainApplication extends Application implements ReactApplication {

  private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
  	// 添加下面这段
    @Override
    protected String getJSBundleFile() {
        return UpdateContext.getBundleUrl(MainApplication.this);
    }
    // ... 其它代码
  }
}
```

## 配置

在[reactnative](https://update.reactnative.cn/home)创建一个账号。
然后在项目根目录，执行：

```javascript
pushy login
email: <输入你的注册邮箱>
password: <输入你的密码>
```

这会在项目文件夹下创建一个`.update`文件，注意不要把这个文件上传到Git等CVS系统上。
你可以在`.gitignore`末尾增加一行`.update`来忽略这个文件。

然后创建一个我们的应用（也可以在网页上创建）：

```bash
pushy createApp --platform android
App Name: <输入应用名字>
```

如果是在网页上创建的，就执行选择APP的命令：

```bash
pushy selectApp --platform android
1)     App Name 1
2)     App Name 2

Total 2 android apps
Enter appId: <输入)前面的编号>
```

执行完上面的创建或者选择APP的操作以后，你可以在根目录看到一个新文件`update.json`，它不包含任何敏感信息。

## 添加热更新功能

[添加热更新功能](https://github.com/reactnativecn/react-native-pushy/blob/master/docs/guide2.md)
完整代码示例：

```javascript
import React, {
  Component,
} from 'react';

import {
  StyleSheet,
  Platform,
  Text,
  View,
  Alert,
  TouchableOpacity,
  Linking,
} from 'react-native';

import {
  isFirstTime,
  isRolledBack,
  packageVersion,
  currentVersion,
  checkUpdate,
  downloadUpdate,
  switchVersion,
  switchVersionLater,
  markSuccess,
} from 'react-native-update';

import _updateConfig from './update.json';
const {appKey} = _updateConfig[Platform.OS];

class MyProject extends Component {
  componentWillMount(){
    if (isFirstTime) {
      Alert.alert('提示', '这是当前版本第一次启动,是否要模拟启动失败?失败将回滚到上一版本', [
        {text: '是', onPress: ()=>{throw new Error('模拟启动失败,请重启应用')}},
        {text: '否', onPress: ()=>{markSuccess()}},
      ]);
    } else if (isRolledBack) {
      Alert.alert('提示', '刚刚更新失败了,版本被回滚.');
    }
  }
  doUpdate = info => {
    downloadUpdate(info).then(hash => {
      Alert.alert('提示', '下载完毕,是否重启应用?', [
        {text: '是', onPress: ()=>{switchVersion(hash);}},
        {text: '否',},
        {text: '下次启动时', onPress: ()=>{switchVersionLater(hash);}},
      ]);
    }).catch(err => {
      Alert.alert('提示', '更新失败.');
    });
  };
  checkUpdate = () => {
    checkUpdate(appKey).then(info => {
      if (info.expired) {
        Alert.alert('提示', '您的应用版本已更新,请前往应用商店下载新的版本', [
          {text: '确定', onPress: ()=>{info.downloadUrl && Linking.openURL(info.downloadUrl)}},
        ]);
      } else if (info.upToDate) {
        Alert.alert('提示', '您的应用版本已是最新.');
      } else {
        Alert.alert('提示', '检查到新的版本'+info.name+',是否下载?\n'+ info.description, [
          {text: '是', onPress: ()=>{this.doUpdate(info)}},
          {text: '否',},
        ]);
      }
    }).catch(err => {
      Alert.alert('提示', '更新失败.');
    });
  };
  render() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          欢迎使用热更新服务
        </Text>
        <Text style={styles.instructions}>
          这是版本一 {'\n'}
          当前包版本号: {packageVersion}{'\n'}
          当前版本Hash: {currentVersion||'(空)'}{'\n'}
        </Text>
        <TouchableOpacity onPress={this.checkUpdate}>
          <Text style={styles.instructions}>
            点击这里检查更新
          </Text>
        </TouchableOpacity>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});
```

## 发布安卓应用

[发布安卓应用](https://github.com/reactnativecn/react-native-pushy/blob/master/docs/guide3.md#%E5%8F%91%E5%B8%83%E5%AE%89%E5%8D%93%E5%BA%94%E7%94%A8)

> 这里发布的包文件，是上传到应用商店的基础版本，无法通过热更新来获得

```javascript

// 1. 进入android目录
`cd android`

// 2. 生成APK
`./gradlew assembleRelease`

// 3. 返回根目录
`cd ..`

// 4. 将生成的APK发布到Pushy服务器上
`pushy uploadApk android/app/build/outputs/apk/release/app-release.apk`

```

## 发布新的热更新版本

[发布新的热更新版本](https://github.com/reactnativecn/react-native-pushy/blob/master/docs/guide3.md#%E5%8F%91%E5%B8%83%E6%96%B0%E7%9A%84%E7%83%AD%E6%9B%B4%E6%96%B0%E7%89%88%E6%9C%AC)

[语义化版本规范](https://semver.org/lang/zh-CN/)

1. 修改你的业务代码

2. `pushy bundle --platform android`, 之后输入`Y`立即发布（此过程是生成新的热更新版本）

3. ```bash
   version name 热更新版本号, 请遵循版本规范
   description, 描述更新内容，会展示给用户，不支持`\n`换行
   meta info, 传输的元信息，例如`{'ok': 1}`（JSON的key必须带引号）
   ```

   此时版本已经提交到update服务，但用户暂时看不到此更新，你还需要将此此**热更新版本**绑定到指定的**包版本**上。
   有3种方法：
    - 输入`Y`立即绑定
    - 或者以后`pushy update --platform android`来让对应包版本用户得以更新
      - 输入`Y`或者上述命令以后，会让你再输入*热更新版本号*`versionId`，就是`)`前面的数字
      - （可以使用`U`上一页，`D`下一页，`B`回到开始）
      - 输入完`versionId`以后，会让你输入*包版本号*`packageId`，就是U/D/B提示下面的列表前的数字
    - 还可以直接在网页端拖拽操作
