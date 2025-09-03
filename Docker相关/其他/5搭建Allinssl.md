<center>Allinssl</center>





[toc]











## Allinssl

> AllinSSL 是一个集证书申请、管理、部署和监控于一体的SSL证书全生命周期管理工具。AllinSSL is an all-in-one SSL
>
> [github](https://github.com/allinssl/allinssl)







### 1. 安装

```shell
docker pull allinssl/allinssl:latest

sudo mkdir -p ./allinssl/data
sudo chown -R 1001:1001 ./allinssl/data
sudo chmod -R 777 ./allinssl/data

docker run -itd \
  --name allinssl \
  -p 7979:8888 \
  -v ./allinssl/data:/www/allinssl/data \
  -e ALLINSSL_USER=allinssl \
  -e ALLINSSL_PWD=allinssldocker \
  -e ALLINSSL_URL=allinssl \
  -e TZ=Asia/Shanghai \
  allinssl/allinssl:latest
```

> 访问： `ip:7979/allinssl`  账号密码： allinssl/allinssldocker

> 牛皮。安装好勒。





### 2. 使用

> 使用 [文档](https://allinssl.com/guide/getting-started.html)

