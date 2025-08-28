<center>Nacos</center>



[toc]







## Nacos

> Nacos（全称：Dynamic Naming and Configuration Service）是阿里巴巴开源的一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。
>
> 一个 `Nacos` 的 `PHP` 协程客户端，与 `Hyperf` 的配置中心、微服务治理完美结合。







### 1. docker nacos

> 容器化

```shell
mkdir -p ./nacos/data ./nacos/logs

sudo chown -R 1000:1000 ./nacos
sudo chmod -R 777 ./nacos
```

```shell
# 启动
docker pull nacos/nacos-server:latest

docker run -d --name nacos-standalone \
  -e MODE=standalone \
  -e NACOS_AUTH_TOKEN=Q2FzZV9Zb3VfTmVlZF9Bbl9FeHRyYV9Mb25nX1NlY3JldA== \
  -e NACOS_AUTH_IDENTITY_KEY=serverIdentity \
  -e NACOS_AUTH_IDENTITY_VALUE=security \
  -p 8848:8848 \
  -p 8080:8080 \
  -v ./nacos/data:/home/nacos/data \
  -v ./nacos/logs:/home/nacos/logs \
  nacos/nacos-server:latest
```

> 访问： http://localhost:8080/

> 牛皮。



