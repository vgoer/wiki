---
title: 01.Node简介
description: Node简介
published: 1
date: 2023-04-22T18:24:30.309Z
tags: node
editor: markdown
dateCreated: 2023-04-22T18:24:30.309Z
---

<center>node</center>

[toc]

## node

> Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行时。



### 1. nvm

> nodejs的`版本管理工具`,为了解决node.js各种版本存在不兼容现象

> 首先卸载你的nodejs版本，不然后期很多问题

下载： [nvm](https://github.com/coreybutler/nvm-windows/releases)

```
nvm-noinstall.zip：   绿色免安装版，但使用时需进行配置。
nvm-setup.zip：    安装版，推荐使用。
Source code(zip)：zip  压缩的源码。
Sourc code(tar.gz)：tar.gz的源码，  一般用于*nix系统。
```



修改源

> nvm 目录下的    settings.txt

```
root: F:\nvm      //你的nvm目录
path: F:\node\no  //你的nodejs目录

//淘宝源
node_mirror: https://npm.taobao.org/mirrors/node/   
npm_mirror: https://npm.taobao.org/mirrors/npm/
```



环境变量 --> 一般安装配置好了

```
NVM_HOME   ->   安装路径  F:\nvm
NVM_SYMLINK ->  node.js目录 F:\node\node.js

//加入到path
%NVM_HOME%
%NVM_SYMLINK%
```

> 测试

```
nvm arch 显示node是运行在32位还是64位。

nvm install <version> [arch]  安装node， version是特定版本也可以是最新稳定版本latest。可选参数arch指定安装32位还是64位版本，默认是系统位数。可以添加--insecure绕过远程服务器的SSL。

nvm list [available]  显示已安装的列表。可选参数available，显示可安装的所有版本。list可简化为ls。

nvm on  开启node.js版本管理。

nvm off  关闭node.js版本管理。

nvm proxy [url]  设置下载代理。不加可选参数url，显示当前代理。将url设置为none则移除代理。

nvm node_mirror [url]  设置node镜像。默认是https://nodejs.org/dist/。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。

nvm npm_mirror [url]  设置npm镜像。https://github.com/npm/cli/archive/。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。

nvm uninstall <version>  卸载指定版本node。

nvm use [version] [arch]  使用制定版本node。可指定32/64位。

nvm root [path]  设置存储不同版本node的目录。如果未设置，默认使用当前目录。

nvm version  显示nvm版本。version可简化为v。
```

```
nvm ls 
nvm list available  //列出了可以下载的版本
nvm install 版本  //下载版本
nvm use node版本
node -v; node版本
npm -v;  npm版本
```



### 2. npm

> NPM是随同NodeJS一起安装的`包管理工具`
>
> 设置`npm`全局

在nvm 目录下新建文件npm文件

```
npm config set prefix 'D://D://新建的npm文件位置'  //你的位置
```



设置环境变量

```
// 和上面一样 都要设置用户和系统变量
NPM_HOMe  --> F:\nvmnode\nvm\npm  //你的位置

// 添加到path
%NPM_HOME%
```



> 全局安装

```
npm install -g npm   //全局安装npm 
npm -v  //查看版本
```



#### nrm

> npm的`镜像源管理工具`

```
// 安装
npm install -g nrm   //全局安装

nrm ls  //查看可选源

nrm use taobao  //切换源

nrm add reigstry url //添加
reigstry：源名
url:地址

nrm del <registry> //删除

nrm test  //测速
```

> 报错：nrm ls 报错internal/validators.js:124 throw new ERR_INVALID_ARG_TYPE(name, ‘string‘, value)

error:[nrm ls](https://www.jianshu.com/p/94d084ce6834)



nodemon

> 开启后  保存js文件，就直接运行

```
npm install -g nodemon

nodemon js文件   //开启监听
```

