---
title: ReactNative之使用Stetho调试Android项目
date: 2019-08-30 15:24:28
tags: 环境配置
categories: ReactNative
---

在RN上使用[Stetho](https://github.com/facebook/stetho)调试安卓项目

<!-- more -->

## 环境配置步骤

1. 打开`android/app/build.gradle`文件，在`dependencies`中添加：

   ```java
    implementation 'com.facebook.stetho:stetho:1.3.1'
    implementation 'com.facebook.stetho:stetho-okhttp3:1.3.1'
    ```

2. 在`android/app/src/main/java/com/{yourAppName}/MainApplication.java`文件中添加：

   ```java
   import com.facebook.react.modules.network.ReactCookieJarContainer;
   import com.facebook.stetho.Stetho;
   import okhttp3.OkHttpClient;
   import com.facebook.react.modules.network.OkHttpClientProvider;
   import com.facebook.stetho.okhttp3.StethoInterceptor;
   import java.util.concurrent.TimeUnit;
   ```

3. 依旧在`android/app/src/main/java/com/{yourAppName}/MainApplication.java`文件中，找到`onCreate()`，添加内容：

   ```java
   public void onCreate() {
      super.onCreate();
      Stetho.initializeWithDefaults(this);
      OkHttpClient client = new OkHttpClient.Builder()
      .connectTimeout(0, TimeUnit.MILLISECONDS)
      .readTimeout(0, TimeUnit.MILLISECONDS)
      .writeTimeout(0, TimeUnit.MILLISECONDS)
      .cookieJar(new ReactCookieJarContainer())
      .addNetworkInterceptor(new StethoInterceptor())
      .build();
      OkHttpClientProvider.replaceOkHttpClient(client);
   }
   ```

4. 运行命令`react-native run-android`
5. 打开一个新的Chrome选项卡，在地址栏中输入`chrome://inspect`并回车。
6. 稍等几秒，在页面列表中找到你要调试的设备，点击`Inspect`。

至此结束！
