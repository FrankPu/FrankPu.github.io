---
title: Hexo博客使用TravisCI实现自动部署
date: 2020-04-21 09:55:28
tags: 
- Hexo
- TravisCI
- 环境配置
categories: 其他前端
---

> Hexo博客使用TravisCI实现自动部署
>
> 博客分支：master
>
> 博客源码分支：dev

<!-- more -->

## 生成Personal Access Token

在Github的`settings`页面，左侧面板选择`Developer settings`，然后选择`Personal access tokens`, 右上角点击`Generate new token`。勾选第一个`repo`即可。

**生成的token只有第一次可见，一定要保存下来备用。**

![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-21-10-02-27.png)

## 开启博客仓库

使用github账号登陆[TravisCI](https://www.travis-ci.org/)，因为是第一次登陆，所以左侧仓库列表还没有东西，需要手动添加自己博客所在的仓库。

![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-21-10-08-23.png)
![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-21-10-09-32.png)

## 配置环境变量

开启后，点仓库`settings`，在下面的`Environment Variables`输入第一步在github生成的`token`。
![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-21-10-11-51.png)

## 配置`.travis.yml`

配置博客源码分支根目录下的`.travis.yml`，没有则新建。

```yml
# 指定语言环境
language: node_js

node_js:
  - lts/*

# 指定缓存模块，可选。缓存可加快编译速度。
cache:
  apt: true
  directories:
    - node_modules

before_install:
  - export TZ='Asia/Shanghai' # 更改时区

install:
  - npm install

script:
  # 执行清缓存，生成网页操作
  - hexo clean
  - hexo generate

# 设置git提交名，邮箱，生成的博客分支(master, 因人而异)；替换真实token到_config.yml文件，最后deploy部署
after_script:
  - git clone https://${GH_REF} .deploy_git
  - cd .deploy_git
  - git checkout master
  - cd ../
  - mv .deploy_git/.git/ ./public/   # 这一步之前的操作是为了保留dev分支的提交记录，不然每次git push都只有1次commit
  - cd ./public
  - git config user.name dick3741
  - git config user.email 233955487@qq.com
  - git add .
  - git commit -m "Travis CI Auto Builder at `date +"%Y-%m-%d %H:%M"`"  # 提交记录包含时间 跟上面更改时区配合
  - git push --force --quiet "https://${TOKEN}@${GH_REF}" master:master  # TOKEN是在Travis中配置环境变量的名称
# End: Build LifeCycle

# 指定博客源码分支(dev, 因人而异)。博客源码托管在独立的仓库则不用设置此项
branches:
  only:
    - dev

env:
  global:
    - GH_REF: github.com/dick3741/dick3741.github.io.git  #设置GH_REF

notifications:
  email:
    - 233955487@qq.com
  on_success: change
  on_failure: always
```

## 集成`build icon`

在TravisCI控制台里，点击顶部那个`build icon`，选择博客源代码分支、markdown的样式，然后把生成的地址放到项目`README.md`里。

![](https://frank-database.oss-cn-hangzhou.aliyuncs.com/img/2020-04-21-10-27-37.png)

## 完成

至此，完成了所有配置项，以后就只需要在博客源码分支进行如下操作即可：

1. `hexo new 文章名`
2. 编写文章
3. `git add .`
4. `git commit -am 提交信息`
5. `git push`
