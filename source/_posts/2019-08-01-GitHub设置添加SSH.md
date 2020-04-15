---
title: GitHub设置添加SSH
date: 2019-08-01 11:49:05
tags: 
- 环境配置
- git
categories: 其他前端
---

在[搭建Hexo博客](https://pushishuang.top/2019/07/15/从零开始搭建Hexo博客/)的时候也会需要配置SSH



## `HTTPS`和`SSH`的区别

1. 我们可以在github上可以使用**HTTPS**随意`clone`别人的项目;

   而使用**SSH**来`clone`的话你必须是要克隆的项目的拥有者或管理员，且必须先添加`SSH key`，否则无法操作。

2. **HTTPS**在`push`的时候需要验证用户名和密码；

   而使用`SSH`来`push`的话，是不用输入用户名的，如果配置`SSH key`的时候设置了密码，则需要输入密码的，否则连密码都不需要输入。



<!-- more -->



## 配置`SSH`

1. 先检查你电脑是否已经有`SSH key`

   打开**git bash**，输入：

   ```
   $ cd ~/.ssh
   $ ls
   ```

   这是在检查是否已经存在`id_rsa.pub`或`id_dsa.pub`两个文件，如果文件已经存在，那么你可以跳过步骤2，直接进入步骤3。

2. 创建一个`SSH key`

   ```
   $ ssh-keygen -t rsa -C "your_email@example.com"
   ```

   - -t 指定密钥类型，默认是 `rsa` ，可以省略。
   - -C 设置注释文字，比如邮箱。
   - -f 指定密钥文件存储文件名。

   上述代码省略了 `-f` 参数，因此，运行上面那条命令后会让你输入一个文件名，用于保存刚才生成的 `SSH key` 代码，如：

   ```
   Generating public/private rsa key pair.
   # Enter file in which to save the key (/c/Users/you/.ssh/id_rsa): [Press enter]
   ```

   当然，你也可以不输入文件名，**使用默认文件名（推荐）**，那么就会生成 `id_rsa` 和 `id_rsa.pub` 两个秘钥文件。

   紧接着又会提示你输入两次密码（该密码是你`push`文件的时候要输入的密码，而不是github管理者的密码）！

   当然，你也可以不输入密码，直接按回车。那么push的时候就不需要输入密码，直接提交到github上了（电脑私用的时候才比较安全），如：

   ```
   Enter passphrase (empty for no passphrase): 
   # Enter same passphrase again:
   ```

   接下来，就会显示如下代码提示，如：

   ```
   Your identification has been saved in /c/Users/you/.ssh/id_rsa.
   # Your public key has been saved in /c/Users/you/.ssh/id_rsa.pub.
   # The key fingerprint is:
   # 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
   ```

   当你看到上面这段代码的收，那就说明你的 `SSH key` 已经创建成功，你只需要**添加到github**的`SSH key`上就可以了。

3. 添加`SSH key`到github

   1. 先拷贝 `id_rsa.pub` 文件的内容，你可以用编辑器打开文件复制，也可以用git命令复制该文件的内容，如：

      `$ clip < ~/.ssh/id_rsa.pub`

   2. 登录你的github账号，在设置中找到**SSH and GPG keys**

   3. 新建一个`SSH key`

      `Title`可以这条`SSH key`的作用，默认是邮箱;

      `Key`就是第一步复制`id_rsa.pub`的内容，**代码的前后不要留有空格或者回车**。

4. 测试一下该`SSH key`

   在**git bash**中输入：

   ```
   $ ssh -T git@github.com
   ```

   然后会出现一段警告代码，如：

   ```
   The authenticity of host 'github.com (207.97.227.239)' can't be established.
   # RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
   # Are you sure you want to continue connecting (yes/no)?
   ```

   **这是正常的！你输入 yes 回车既可。**如果你创建 SSH key 的时候设置了密码，接下来就会提示你输入密码，密码正确后你会看到下面这段话，如：

   ```
   Hi username! You've successfully authenticated, but GitHub does not
   # provide shell access.
   ```

   如果用户名也是正确的，你就已经成功设置SSH密钥。如果你看到 “access denied” ，者表示拒绝访问，那么你就需要使用 HTTPS 去访问，而不是 SSH 。