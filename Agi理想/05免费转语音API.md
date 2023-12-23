<center>转语音API</center>







[toc]







### 转语音API

> 免费的在线文本转语音API  [api](https://github.com/wxxxcxx/ms-ra-forwarder)
>
> 感谢开源大佬









### 1. 安装

> 推荐docker

> 创建 `docker-compose.yml` 写入以下内容并保存。

```shell
version: '3'

services:
  ms-ra-forwarder:
    container_name: ms-ra-forwarder
    image: wxxxcxx/ms-ra-forwarder:latest
    restart: unless-stopped
    ports:
      - 3000:3000
    environment:
      # 不需要可以不用设置环境变量
      - TOKEN=自定义TOKEN
```

> 在 `docker-compose.yml` 目录下执行 `docker compose up -d`。



### 2. 直接运行

> git nodejs

```shell
# 获取代码
git clone https://github.com/wxxxcxx/ms-ra-forwarder.git

cd ms-ra-forwarder
# 安装依赖
npm install 
# 运行
npm run start
```







### 3. 手动调用的话

> api

```shell
POST /api/ra
FORMAT: audio-16khz-128kbitrate-mono-mp3
Content-Type: text/plain

<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
  <voice name="zh-CN-XiaoxiaoNeural">
    如果喜欢这个项目的话请点个 Star 吧。
  </voice>
</speak>
```





