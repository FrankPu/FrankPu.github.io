---
title: 在Windows下安装MongoDB
date: 2019-07-31 17:29:45
tags: 
- 环境配置
- MongoDB
categories: 服务器
---

> 参考地址：[河畔一角](https://www.cnblogs.com/jacksoft/p/6910709.html)



<!-- more -->



## 下载

[下载地址](https://www.mongodb.com/download-center/community)，下载后双击安装即可，所在目录在`C:\Program Files\MongoDB\`。

![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/20190731105751.png)



## 配置

1. 在`c:\MongoDB`（可随意起）下面建一个data文件夹`c:\MongoDB\data`

2. 在`c:\MongoDB`下面建一个logs文件夹`c:\MongoDB\logs`，在里面建一个文件`mongo.log`

3. 在`c:\MongoDB`下面建一个etc文件夹（随意起，放配置文件）`c:\MongoDB\etc` ，在里面建一个文件`mongo.conf`

4. 打开`mongo.conf`文件，修改如下：

   ```
   #数据库路径
   dbpath=c:\MongoDB\data\
   #日志输出文件路径
   logpath=c:\MongoDB\logs\mongodb.log
   #错误日志采用追加模式，配置这个选项后mongodb的日志会追加到现有的日志文件，而不是从新创建一个新文件
   logappend=true
   #启用日志文件，默认启用
   journal=true
   #这个选项可以过滤掉一些无用的日志信息，若需要调试使用请设置为false
   quiet=false
   #端口号 默认为27017
   port=27017
   #指定存储引擎（默认先不加此引擎，如果报错了，大家在加进去）
   storageEngine=mmapv1
   ```

   **注意：**在上面，我指定了一个存储引擎，这个通常是不需要的，但是，我的电脑报错`storage engine 'wiredTiger'`，查找博客后，需要添加指定引擎为：`storageEngine=mmapv1`。如果大家是win 64位，则可能不需要指定，如果是其它平台，比如linux平台或者osx平台，可能也不需要指定。

完成以上操作后，我们就可以启动我们的mongo数据库了。



备注：

以上是通过配置的形式，来启动我们的MongoDB，也可以把参数直接在启动的时候，传递进去如下：

`mongod --dbpath c:\MongoDB\data --logpath c:\MongoDB\log\mongo.log  --journal`



## 在命令行中启动mongo

1. 管理员身份运行`cmd`。

2. 进入到安装的mongo文件夹中，一直进入到bin目录（存放命令的目录，里面有mongod.exe）。

   ![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/20190731110836.png)

3. 先简单启动一下mongo（这种方式是通过命令的形式，同时把参数传进去，实际上，我们只需要启动我们上面那个配置文件就可以了），执行：

   `mongod --dbpath c:\MongoDB\data`

   ![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/20190731110927.png)

   看到`waiting for connections on port 27017`等，说明启动成功，我们可以测试一下。

   ![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/20190731171517.png)

   看到此图，说明启动成功。

4. 再次打开命令行，然后进入到mongo的`bin`目录（因为还没有配置环境变量，所以需要进到bin下面执行），输入`mongo`后回车。

   ![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/20190731171638.png)

   如上图所示，已经进入到了mongo的命令中，现在就可以通过mongo的命令进行一系列关于数据库的操作了。



## 配置mongo到Windows服务中

1. 进入到mongodb的安装目录下面的`bin`目录中：

   `cd c:\Program Files\MongoDB\Server\3.2\bin>`

2. 输入命令，启动mongo：

   `mongod --config c:\MongoDB\etc\mongo.conf --install --serviceName "MongoDB"`

   ![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/20190731172336.png)

3. 安装成功后，打开window服务，我们可以看到里面已经安装了MongoDB：

   ![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/20190731172413.png)

   下次我们用的时候，只需要启动服务即可，如果大家不安装此服务也可以，每次启动的时候，手动进入到安装目录里面，输入命令启动，两种方式都可以。

   删除服务命令：`mongod --config c:\MongoDB\etc\mongo.conf --remove `，然后在服务里面刷新一下，就会发现已经删掉了。

4. 另外如果不想进入到安装目录，可以配置一下Mongo的环境变量，这样就不需要每次进入到安装目录启动了，打开`path`，在里面输入：

   `;C:\Program Files\MongoDB\Server\3.2\bin`

   （注意，前面有个分号）

   ![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/20190731173614.png)

到此，我们已经成功安装MongoDB。