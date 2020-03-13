---
title: 在Linux服务器下配置node和pm2
date: 2019-07-24 17:30:54
tags: 
- 环境配置
- Linux
- node
- pm2
categories: 服务器
---

Linux配置Node和pm2的笔记



<!-- more -->



## Node

1. 转到根目录`cd /`

2. 下载node包到服务器

   `wget https://npm.taobao.org/mirrors/node/v6.10.3/node-v6.10.3-linux-x64.tar.xz`

3. 把xz格式解压成tar格式

   `xz -d node-v6.10.3-linux-x64.tar.xz`

   或者

   `tar -xzvf node-v6.10.3-linux-x64.tar.xz`

4. 把tar格式包解压成node文件夹

   `tar -xvf node-v6.10.3-linux-x64.tar`

5. 建立软连接`link`

   建立了软连接才能全局使用

   ```
   ln -s /node-v6.10.3-linux-x64/bin/npm /usr/local/bin/npm
   ln -s /node-v6.10.3-linux-x64/bin/node /usr/local/bin/node
   ```

6. 使用定制的[cnpm](https://github.com/cnpm/cnpm) (gzip 压缩支持) 命令行工具代替默认的npm

   `$ npm install -g cnpm --registry=[https://registry.npm.taobao.org](https://registry.npm.taobao.org/)`

7. 建立npm的软连接

   `ln -s /node-v6.10.3-linux-x64/bin/cnpm /usr/local/bin/cnpm`



## PM2

1. 安装`cnpm install pm2 -g`

2. 转到根目录`cd /`

3. 建立软连接

   `ln -s /node-v6.10.3-linux-x64/bin/pm2 /usr/local/bin/pm2`

4. pm2启动node文件

   `pm2 start app.js`

5. 显示服务列表

   `pm2 list`

6. 重启服务

   `pm2 show <id|nam>`