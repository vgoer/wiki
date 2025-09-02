<center>Dockge监控</center>





[toc]









## Dockge监控

> 一个精美、易用、反应灵敏的自托管 docker compose.yaml 堆栈导向管理器 
>
> [github](https://github.com/louislam/dockge)









### 1. 安装

```shell
# Create directories that store your stacks and stores Dockge's stack
mkdir -p /opt/stacks /opt/dockge
cd /opt/dockge

# Download the compose.yaml
curl https://raw.githubusercontent.com/louislam/dockge/master/compose.yaml --output compose.yaml

# Start the server
docker compose up -d

# If you are using docker-compose V1 or Podman
# docker-compose up -d
```

```yaml
services:
  dockge:
    image: louislam/dockge:1
    restart: unless-stopped
    ports:
      # Host Port : Container Port
      - 1006:5001
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./data:/app/data

      # If you want to use private registries, you need to share the auth file with Dockge:
      # - /root/.docker/:/root/.docker

      # Stacks Directory
      #   READ IT CAREFULLY. If you did it wrong, your data could end up writing into a WRONG PATH.
      #   1. FULL path only. No relative path (MUST)
      #   2. Left Stacks Path === Right Stacks Path (MUST)
      # docker 目录
      - /home/dockerdata:/opt/stacks
    environment:
      # Tell Dockge where is your stacks directory
      - DOCKGE_STACKS_DIR=/home/dockerdata
```

