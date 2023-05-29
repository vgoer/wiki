---
title: 01.jenkins
description: jenkins
published: 1
date: 2023-05-29T04:56:35.047Z
tags: jenkins, k8s
editor: markdown
dateCreated: 2023-05-29T04:56:35.047Z
---

<center>Jenkins</center>





[toc]





## Jenkins

> ==devops==开发和运维结合起来 `自动化运维`
>
> ==CI/CD==持续集成/持续交互

> **构建伟大，无所不能** [github](https://www.jenkins.io/zh/)





### 1.安装

> [download](https://www.jenkins.io/zh/download/)

```shell
# docker方式
docker pull jenkins/jenkins:lts-jdk11


docker run -itd -p 8086:8080 jenkins/jenkins:lts-jdk11

# 管理原密码
docker logs jenkins/jenkins:lts-jdk11 
```

```yaml
version: '3'
services:
  jenkins:
    image: jenkins/jenkins:lts
    container_name: jenkins
    ports:
      - "8085:8080"
      - "50000:50000"
    volumes:
      - ./jenkins_home:/var/jenkins_home
```
