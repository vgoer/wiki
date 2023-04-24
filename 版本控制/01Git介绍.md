---
title: 01.Git介绍
description: Git介绍
published: 1
date: 2023-04-24T10:27:51.039Z
tags: git
editor: markdown
dateCreated: 2023-04-22T18:33:28.681Z
---

<center>Git简单使用</center>

[toc]

### GIT简单使用



#### 1. git是什么？

> git: 分布式版本控制系统  ，是一个工具软件
>
> > 主要：**版本控制**

```tex	
Git是一个版本控制系统（Version Control System，VCS）。版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。有了版本控制系统，就可以不用担心文件丢失，不小心误修改文件等等“事故”，而且你可以随便回到历史记录的某个时刻。
SVN, CVS这类早期的集中式版本控制系统
```

借鉴：[博客园](https://www.cnblogs.com/schaepher/p/5561193.html)

#### 2. 下载安装

[官网](https://git-scm.com/ 'git')

> 官网： Downloads --> 选择系统

> 安装：下一步就行, 添加到环境变量，不然不能在命令行使用



#### 3.常用命令

> 命令前都要加 `git`，如表中的`init`是指 `git init`。

###### a. 个人本地使用

| 行为                        | 命令                 | 备注                                                         |
| --------------------------- | -------------------- | ------------------------------------------------------------ |
| 初始话                      | init                 | 本地的当前目录里初始化git仓库                                |
| 克隆                        | clone+地址           | 网络上拷贝仓库(repository)到本地                             |
| 查看当前状态                | **status**           | 查看当前仓库的状态,使用频率和高                              |
| 查看不同                    | diff                 | 查看当前状态和最新的commit之间不同的地方                     |
| 比较版本差别                | diff 版本号1 版本号2 | 查看两个指定的版本之间不同的地方。这里的版本号**指的是commit的hash值** |
| 添加文件                    | add -A               | 特别常用，在commit之前要先add                                |
| 撤回修改的且还未stage的内容 | checkout --  .       | 小数点表示撤回所有修改，在`--`的前后都有空格                 |
| 提交信息                    | commint -m"提交”     | 提交信息最好能体现更改了什么                                 |
| 删除未tracked               | clean -xf            | 删除当前目录下所有没有track过的文件。不管它是否是.gitignore文件里面指定的文件夹和文件 |
| 查看提交记录                | log                  | 查看当前版本及之前的commit记录                               |
|                             | reflog               | HEAD的变更记录                                               |
| 版本回退                    | seset --hard 版本号  | 回退到指定版本号的版本，该版本之后的修改都被删除。同时也是通过这个命令回到最新版本。需要reflog配合 |

###### b. 个人使用远程仓库

| 行为       | 命令                               | 备注   |
| ---------- | ---------------------------------- | ------ |
| 设置用户名 | config --global user.name "用户名" | 用户名 |
| 设置邮箱   | config --global user.email "邮箱"  | 邮箱   |
|            |                                    |        |
|            |                                    |        |
|            |                                    |        |
|            |                                    |        |















#### 4. 本地git使用

> 打开命令行（cmd）或者在想要空白文件右键鼠标并点击 `Git Bash Here` 打开窗口。

```git
git init  #初始化
```









#### 开源协议的区别

![开源区别](https://img-blog.csdnimg.cn/20200429155422430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzIzMjc0NzE1,size_16,color_FFFFFF,t_70#pic_center)





#### 3. git和github

> 想和其他人分享你的代码，或者合作开发，就要使用开源仓库。

1. github:全球最大的软件仓库。
2. gitee:我们国家的仓库，码云。



##### a. 注册账号

> 一般有邮箱就行咯,记住邮箱和密码

我们的：[gitee](https://gitee.com)

###### b. 创建仓库

> 命名好，开源即可



###### c. 克隆

> 进入仓库，点击克隆/下载复制地址



###### e. 拉取到本地

> 新建文件，用于存放仓库，点击鼠标右键，`Git Bash Here`

```git	
git clone + 复制地址		 #这样就初始化了	
```



###### f. 设置用户名和邮箱

>就在克隆下来的仓库内打开`git bash` 

```git
git config user.name "username"    # 用户名

git config user.email "email@123"  # 邮件
```



###### g.推送到开源仓库

> 把项目做好后

1. ==拉取代码==，更新到本地**重要**

```git
git pull   # 很重要啊
```

2. 添加文件，到缓存区

```git
git add --all     # 添加所有文件
git add .         # 添加所有文件
git add -A 文件名  # 添加单个文件
```

3. 本次提交的注释(可以中文)

```git
git commit -m"本次提交注释“
```

4. 提交代码

```git
git push origin master
# 输入用户名和密码就可以在网站上看到了
```

