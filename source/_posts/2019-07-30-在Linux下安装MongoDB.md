---
title: 在Linux下安装MongoDB
date: 2019-07-30 17:12:38
tags: 
- 环境配置
- Linux
- MongoDB
categories: 服务器
---

> 参考地址：[linuxidc](https://www.linuxidc.com/Linux/2016-06/132675.htm)



<!-- more -->



## 安装步骤

1. 下载你的服务器系统版本的mongodb，这里是`mongodb-linux-x86_64-3.4.9.tgz`

2. 传输到服务器，我这里使用**putty的pscp传输**（mac用户直接在shell使用scp传输）

   格式为：

   `pscp.exe 文件路径 服务器登录名@服务器ip:传输到的服务器路径`

   具体命令如下：

   `C:\Program Files\PuTTY>pscp.exe d:/mongodb-linux-x86_64-3.4.9.tgz root@120.78.82.59:/`

   我这里是传到了**根目录**，所以直接是`/`

3. `wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.4.9.tgz`

   `wget  https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-rhel70-3.4.9.tgz`

   解压mongodb：

   `tar -zxvf mongodb-linux-x86_64-rhel70-3.4.9.tgz`

4. 新建mongodb文件夹：

   `mkdir mongodb`

   移动刚才解压缩完的文件夹到**新建的mongodb目录**下：

   `mv mongodb-linux-x86_64-rhel70-3.4.9 mongodb`

5. 进入新建的mongodb目录下，新建3个文件夹：

   ```
   cd mongodb
   mkdir data
   mkdir logs
   mkdir etc
   ```

6. 进入logs创建

   ```
   cd logs
   touch mongo.log
   ```

7. 进入etc创建

   ```
   cd ..
   cd etc
   // 进入vim编辑模式
   vi mongo.conf
   
   // 键入 I 进入编辑模式
   // 输入：
   dbpath=/mongodb/data
   logpath=/mongodb/logs/mongo.log
   logappend=true
   journal=true
   quiet=true
   port=27017
   
   // 完成后按esc退出编辑模式
   // 再输入 :wq 保存退出（有一个冒号哦！）
    
   ```

8. 建mongodb软连接

   ```
   cd /
   ln -s /mongodb/mongodb-linux-x86_64-rhel70-3.4.9/bin/mongo /usr/local/bin/mongo
   ln -s /mongodb/mongodb-linux-x86_64-rhel70-3.4.9/bin/mongod /usr/local/bin/mongod
   ```

9. 打开第一个putty窗口，启动服务器（在服务器ssh上执行）

   `mongod -f /mongodb/etc/mongo.conf`

10. 双开第二个putty窗口，连接数据库，输入：`mongo`

11. 此时，我们已经可以进入数据库，我们插入一条数据（mongodb的语法）：

    `db.goods.insert({ id:1000, name:test })`

    再执行`db.goods.find()`就可以看到我们刚才插入的数据了，但是这种做法我们不常用，一般使用可视化软件**robot 3t**

12. 打开第三个putty窗口，执行：`node imooc/server/bin/www`，可以看到`mongodb connected success`

13. 将mongodb加入系统自启动

    编辑文件`vi /etc/rc.loacl`，在最底部输入：

    `/mongodb/mongodb-linux-x86_64-rhel70-3.4.9/bin/mongod --dbpath=/mongodb/data --fork --port 27017 --logpath=/mongodb/logs/mongo.log --logappend --auth`

14. **注意：**此处有一个超级大坑！阿里云默认是没有开27017和3000端口的，我之前服务器虽然配置成功了，但是在浏览器中却一直无法访问。

    操作方法：

    `阿里云控制台--云计算基础服务--云服务器CES--网络安全（安全组）--选到你的服务器所在安全组列表--管理实例`

    ![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/20190730170634.png)

    然后添加27017和3000端口即可：

    ![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/20190730170703.png)

