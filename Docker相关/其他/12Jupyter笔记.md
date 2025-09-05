<center>Jupyter笔记</center>





[toc]







## Jupyter笔记

> jupyter-docker-stacks 是 Jupyter 官方维护的一组基于 Docker 的镜像，内置了 Jupyter Notebook/Lab 及常用的科学计算、数据分析、机器学习环境
>
> [github](https://github.com/jupyter/docker-stacks)







### 1. 安装

```shell
mkdir jupyter
sudo chown -R 1001:1001 ./jupyter
sudo chmod -R 777 ./jupyter

```

```yaml
services:
  jupyter:
    image: jupyter/datascience-notebook:latest
    container_name: jupyter
    restart: always
    ports:
      - 8888:8888
    volumes:
      - ./work:/home/jovyan/work
      - ./data:/home/jovyan/data
    environment:
      - JUPYTER_ENABLE_LAB=yes
      # 可选：设置密码（推荐用 token 登录更安全）
      # - JUPYTER_TOKEN=你的token
```

> 访问 Jupyter

浏览器访问：

http://localhost:8888

首次启动时，控制台会输出 token，复制 token 用于登录。