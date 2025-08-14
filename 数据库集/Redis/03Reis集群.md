<center>Reis集群</center>





[toc]









## Reis集群

> 搭建 Redis 集群通常需要至少 3 个主节点，为了高可用，建议配置 3 个主节点和 3 个从节点，共 6 个节点。





### 1. 安装

> docker-compose.yaml

```yaml
services:
  redis-node-1:
    image: redis:latest
    container_name: redis-node-1
    # 运行 Redis 服务器，开启 AOF 持久化，开启集群模式
    command: redis-server --appendonly yes --cluster-enabled yes --cluster-config-file nodes.conf --port 6379
    volumes:
      # 持久化数据到命名卷
      - ./redis_data_1:/data
    ports:
      # 将容器内部的 6379 端口映射到宿主机的不同端口，方便后续创建集群和客户端连接
      - 6001:6379
    networks:
      # 将所有节点放在同一个自定义网络中
      - redis-cluster-network

  redis-node-2:
    image: redis:latest
    container_name: redis-node-2
    command: redis-server --appendonly yes --cluster-enabled yes --cluster-config-file nodes.conf --port 6379
    volumes:
      - ./redis_data_2:/data
    ports:
      - 6002:6379
    networks:
      - redis-cluster-network

  redis-node-3:
    image: redis:latest
    container_name: redis-node-3
    command: redis-server --appendonly yes --cluster-enabled yes --cluster-config-file nodes.conf --port 6379
    volumes:
      - ./redis_data_3:/data
    ports:
      - 6003:6379
    networks:
      - redis-cluster-network

  redis-node-4:
    image: redis:latest
    container_name: redis-node-4
    command: redis-server --appendonly yes --cluster-enabled yes --cluster-config-file nodes.conf --port 6379
    volumes:
      - ./redis_data_4:/data
    ports:
      - 6004:6379
    networks:
      - redis-cluster-network

  redis-node-5:
    image: redis:latest
    container_name: redis-node-5
    command: redis-server --appendonly yes --cluster-enabled yes --cluster-config-file nodes.conf --port 6379
    volumes:
      - ./redis_data_5:/data
    ports:
      - 6005:6379
    networks:
      - redis-cluster-network

  redis-node-6:
    image: redis:latest
    container_name: redis-node-6
    command: redis-server --appendonly yes --cluster-enabled yes --cluster-config-file nodes.conf --port 6379
    volumes:
      - ./redis_data_6:/data
    ports:
      - 6006:6379
    networks:
      - redis-cluster-network


# 定义一个自定义网络，方便容器间通信
networks:
  redis-cluster-network:
    driver: bridge
```

> 启动

```shell
docker-compose up -d
```







### 2. 测试

> 本机安装redis_cli

```shell
# ubuntu
sudo apt install redis-tools

# 连接 集群
 redis-cli --cluster create 127.0.0.1:6001 127.0.0.1:6002 127.0.0.1:6003 127.0.0.1:6004 127.0.0.1:6005 127.0.0.1:6006 --cluster-replicas 1
 
 # 日志
 >>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 127.0.0.1:6005 to 127.0.0.1:6001
Adding replica 127.0.0.1:6006 to 127.0.0.1:6002
Adding replica 127.0.0.1:6004 to 127.0.0.1:6003
>>> Trying to optimize slaves allocation for anti-affinity
[WARNING] Some slaves are in the same host as their master
M: 0c1d2fafecdaa13d01683b5e6c481870362c246a 127.0.0.1:6001
   slots:[0-5460] (5461 slots) master
M: 3e2c9b540c06fb328dcc3794b992b5c21c799d3b 127.0.0.1:6002
   slots:[5461-10922] (5462 slots) master
M: 8ca422f157ce9b9dcc23fd236a16e1e6babe2230 127.0.0.1:6003
   slots:[10923-16383] (5461 slots) master
S: 56529ea8dd3498ebb276047331eeba92749e052a 127.0.0.1:6004
   replicates 8ca422f157ce9b9dcc23fd236a16e1e6babe2230
S: 258d54cfcb55a8f2539271332ea8925c842f7f87 127.0.0.1:6005
   replicates 0c1d2fafecdaa13d01683b5e6c481870362c246a
S: 406b39a770d7f4b64d25ae4d308ee675faf4c56e 127.0.0.1:6006
   replicates 3e2c9b540c06fb328dcc3794b992b5c21c799d3b
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
```

> 查看状态： 

```shell
redis-cli -p 6001 cluster info
redis-cli -p 6001 cluster nodes
```

