<center>Portainer容器监控</center>



[toc]







## Portainer容器监控

> 使 Docker 和 Kubernetes 管理变得简单。
>
> [github](使 Docker 和 Kubernetes 管理变得简单。)







### 1. 安装

```shell
mkdir portainer
sudo chown -R 1001:1001 ./portainer
sudo chmod -R 777 ./portainer
```

> docker-compose.yaml	

```yaml
services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    restart: always
    ports:
      - 9000:9000      # Web UI 端口
      - 9443:9443      # HTTPS 端口（可选）
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock   # 让 Portainer 管理宿主机 Docker
      - ./portainer_data:/data                     # 持久化 Portainer 数据
```

> 启动： ip：9000







### 2. 监控k8s服务

> 将 Kubernetes 环境连接到 Portainer 时，您可以根据具体需求使用几种不同的方法。您可以在 Kubernetes 集群上安装 Portainer Agent 并通过代理进行连接，也可以以标准或异步模式部署 Portainer Edge Agent，或者选择使用 kubeconfig 文件导入现有的 Kubernetes 环境。
>
> [doc](https://docs.portainer.io/admin/environments/add/kubernetes)

> 菜单-环境—>添加环境—>k8s

> 启动k8s的Portainer代理

```shell
    kubectl apply -f https://downloads.portainer.io/ce2-27/portainer-agent-k8s-lb.yaml
```

