---
title: 01.vue开发环境
description: vue
published: 1
date: 2023-04-24T10:28:10.133Z
tags: docker, docker env
editor: markdown
dateCreated: 2023-04-21T05:24:42.986Z
---

<center>Vue</center>



[toc]





## vue

> docker 搭建vue环境
>
> 找你要的镜像：[hub](https://hub.docker.com/)

```shell
# 装备好docker和docker-compose

# 1.node
docker pull node:16-buster

# 2.新建文件映射到容器内
mkdir -p docker/vue
nano Dockerfile
mkdir src # 映射文件
```

```bash
FROM node:16-buster  
ADD ./src /app 
WORKDIR /app
ENV DEBCONF_NOWARNINGS yes
RUN apt-get update -y && \
	apt-get upgrade -y && \
	apt-get install -y \
	build-essential -y \ 
	curl \
	git \
	nano \
	&& rm -rf /var/lib/apt/lists/*
RUN npm install -g @vue/cli@4.4.6
```

> 打包

```shell
docker image build -t self/vue4.0 .
# 查看
docker images
# 运行容器
docker run -it --name vue4cli -v `pwd`/src:/app -p 8081:8080 -d self/vue4.0
# 进入容器内
docker exec -it vue4cli /bin/bash
```

```shell
# vue默认配置 可有可无
nano .vuerc

{
	"useTaobaoRegistry":false,
	"packageManager":"npm"
}
```

> 新建项目

```shell
vue create myWeb
cd myWeb 
npm run serve
# 启动8081端口
ip:8081

# linux后台运行命令
nohub npm run server > /dev/null & 
# 查看
jobs
```

> 进入容器执行命令

```shell
docker run --rm -it vue4cli cat /etc/passwd
```





> 扩展，linux添加别名

```shell
vim ~/.bashrc

export LS_OPTIONS='--color=auto'
alias ls='ls $LS_OPTIONS'
alias ll='ls $LS_OPTIONS -l'
alias l='ls $LS_OPTIONS -lA'
```

