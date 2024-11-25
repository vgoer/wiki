<center>Redeemer</center>







[toc]







## Redeemer

> Redeemer靶机







### 1. task

1. Which TCP port is open on the machine?

```shell
# 全端口扫描
nmap -p- 10.129.136.187 -A

PORT     STATE SERVICE VERSION
6379/tcp open  redis   Redis key-value store 5.0.7
```

2. Which service is running on the port that is open on the machine?

```shell
redis
```

3. What type of database is Redis? Choose from the following options:

    (i) In-memory Database,

    (ii) Traditional Database

```shell
In-memory Database
```

4. Which command-line utility is used to interact with the Redis server? Enter the program name you would enter into the terminal without any arguments.

```shell
redis-cli
```

5. Which flag is used with the Redis command-line utility to specify the hostname?

```shelll
-h
```

6. Once connected to a Redis server, which command is used to obtain the information and statistics about the Redis server?

```shell
info
```

7. What is the version of the Redis server being used on the target machine?

```shell
5.0.7
```

8. Which command is used to select the desired database in Redis?

```shell
# 连接数据库
redis-cli -h 10.129.136.187 -p 6379

select
```

9. How many keys are present inside the database with index 0?

```shell
# redis-cli
keys *
4
```

10. Which command is used to obtain all the keys in a database?

```shelll
keys *
```





### 2. redis

> *Redis (Remote Dictionary Server) 是一个开源的内存数据库，支持多种数据结构：`port: 6379`。* [runoob](https://www.runoob.com/redis/redis-tutorial.html)

```shell
字符串 (String)
哈希 (Hash)
列表 (List)
集合 (Set)
有序集合 (Sorted Set)
位图 (Bitmap)
HyperLogLog
```

> redis服务端和客户端搭建。

```shell
1. 服务端安装

# 安装
sudo apt update
sudo apt install redis-server

# 安装 EPEL 源
sudo dnf install epel-release
# 安装 Redis
sudo dnf install redis

# 启动服务
sudo systemctl start redis
sudo systemctl enable redis

# 源码安装
# 下载并编译
wget https://download.redis.io/redis-stable.tar.gz
tar xzf redis-stable.tar.gz
cd redis-stable
make

# 安装
sudo make install

# 创建配置目录
sudo mkdir /etc/redis
sudo cp redis.conf /etc/redis/

# 配置
/etc/redis/redis.conf
```

```shell
# 网络
bind 127.0.0.1         # 监听地址
port 6379              # 端口
protected-mode yes     # 保护模式

# 常规
daemonize yes         # 守护进程模式
pidfile /var/run/redis_6379.pid
dir /var/lib/redis    # 工作目录

# 内存
maxmemory 256mb      # 最大内存使用
maxmemory-policy allkeys-lru  # 内存策略

# 持久化
save 900 1           # 900秒内有1个改动则保存
save 300 10          # 300秒内有10个改动则保存
save 60 10000        # 60秒内有10000个改动则保存

# 安全
requirepass yourpassword  # 设置密码
```

```shell
# 启用密码
# 方法1：通过配置文件
sudo nano /etc/redis/redis.conf
# 添加或修改：
requirepass your_strong_password

# 方法2：通过 redis-cli
redis-cli
> CONFIG SET requirepass "your_strong_password"
> CONFIG REWRITE
```

```shell
# 客户端
# 基本连接
redis-cli

# 带密码连接
redis-cli -a your_password

# 连接远程服务器
redis-cli -h host -p port -a password
```

```shell
# 常用命令
# 字符串操作
SET key value
GET key

# 列表操作
LPUSH mylist value
RPUSH mylist value
LRANGE mylist 0 -1

# 哈希操作
HSET user name "John"
HGET user name

# 集合操作
SADD myset value
SMEMBERS myset

# 有序集合操作
ZADD scores 89 "Tom"
ZRANGE scores 0 -1
```

> 高级： Redis 集群配置

```shell
1. 主从配置
# 在从服务器配置文件中添加
replicaof 主服务器IP 主服务器端口
masterauth 主服务器密码

2. 哨兵模式
/etc/redis/sentinel.conf
sentinel monitor mymaster 127.0.0.1 6379 2
sentinel auth-pass mymaster your_password
sentinel down-after-milliseconds mymaster 5000
sentinel failover-timeout mymaster 60000
```

> 监控与备份

```shell
# 监控信息
redis-cli info

# 内存使用
redis-cli info memory

# 客户端连接
redis-cli client list

# 慢查询日志
redis-cli slowlog get 10

# 备份
redis-cli SAVE
# 或
redis-cli BGSAVE

# 恢复
# 停止 Redis
sudo systemctl stop redis
# 复制 dump.rdb 到 Redis 工作目录
sudo cp dump.rdb /var/lib/redis/
# 启动 Redis
sudo systemctl start redis
```







### 3. flag

> 获取flag

```shell
nmap -sV -sC -v -p 1000-9999 10.129.136.187 -A

PORT     STATE SERVICE VERSION
6379/tcp open  redis   Redis key-value store 5.0.7

# 连接redis
redis-cli -h 10.129.136.187 -p 6379

# 查看所有key
keys *

# 查看flag
get flag
"03e1d2b376c37ab3f5319922053953eb"

```

