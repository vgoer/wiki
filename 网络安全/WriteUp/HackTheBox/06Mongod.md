<center>Mongod</center>







[toc]







## Mongod

> Mongod









### 1. task

1. How many TCP ports are open on the machine?

```shell
nmap -Pn -p- -sT -T4 --min-rate=1000 10.129.167.185

22 ssh
27017 mongod
```

2. Which service is running on port 27017 of the remote host?

```shell
nmap -sV -p27017 10.129.167.185
MongoDB 3.6.8
```

3. What type of database is MongoDB? (Choose: SQL or NoSQL)

```shell
NoSQL
```

4. What is the command name for the Mongo shell that is installed with the mongodb-clients package?

```shell
sudo apt install mongodb-clients
mongo
```

5. What is the command used for listing all the databases present on the MongoDB server? (No need to include a trailing ;)

```shell
mongo 10.129.167.185:27017

# 查看所有数据库
show dbs
```

6. What is the command used for listing out the collections in a database? (No need to include a trailing ;)

```shell
# 使用数据库
use admin

# 查看所有表(集合)
show collections
```

7. What is the command used for dumping the content of all the documents within the collection named flag in a format that is easy to read?

```shell
db.flag.find().pretty()
```







### 2. mongodb

> MongoDB 是一个开源的文档型数据库。 
>
> [blog](https://www.runoob.com/mongodb/mongodb-tutorial.html)

```shell
文档型存储（类 JSON）
高性能
高可用性
易扩展性
支持动态查询
支持完全索引
```

> `默认端口：27017`

> 安装客户端和服务端

````shell
1. 安装服务端

#### Ubuntu/Debian 安装
```bash
# 导入公钥
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -

# 添加源
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

# 安装
sudo apt update
sudo apt install mongodb-org

# 启动服务
sudo systemctl start mongod
sudo systemctl enable mongod
```


#### CentOS/RHEL 安装
```bash
# 创建 repo 文件
cat <<EOF | sudo tee /etc/yum.repos.d/mongodb-org-6.0.repo
[mongodb-org-6.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/\$releasever/mongodb-org/6.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-6.0.asc
EOF

# 安装
sudo dnf install mongodb-org

# 启动服务
sudo systemctl start mongod
sudo systemctl enable mongod
```


#### 基本配置
```yaml:/etc/mongod.conf
# 网络设置
net:
  port: 27017
  bindIp: 127.0.0.1

# 存储设置
storage:
  dbPath: /var/lib/mongodb
  journal:
    enabled: true

# 系统日志
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log

# 安全设置
security:
  authorization: enabled
```


#### 创建管理员用户
```javascript
// 连接 MongoDB
mongosh

// 切换到 admin 数据库
use admin

// 创建管理员用户
db.createUser({
  user: "adminUser",
  pwd: "securePassword",
  roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
})
```


2. 客户端

#### 命令行客户端 (mongosh)
```bash
# 安装 mongosh
sudo apt install mongodb-mongosh  # Ubuntu
sudo dnf install mongodb-mongosh  # CentOS

# 基本连接
mongosh

# 带认证连接
mongosh --username adminUser --password --authenticationDatabase admin

# 连接远程服务器
mongosh "mongodb://username:password@host:port/database"
```


````

> 基本操作

````shell

#### 基本操作示例
```javascript
// 显示数据库
show dbs

// 创建/切换数据库
use mydb

// 创建集合
db.createCollection("users")

// 插入文档
db.users.insertOne({
    name: "John",
    age: 30,
    email: "john@example.com"
})

// 查询文档
db.users.find()

// 更新文档
db.users.updateOne(
    { name: "John" },
    { $set: { age: 31 } }
)

// 删除文档
db.users.deleteOne({ name: "John" })
```


#### 启用认证
```yaml:/etc/mongod.conf
security:
  authorization: enabled
```


#### 网络安全
```yaml:/etc/mongod.conf
net:
  bindIp: 127.0.0.1
  ssl:
    mode: requireSSL
    PEMKeyFile: /path/to/mongodb.pem
```


### 备份和恢复

```bash
# 备份
mongodump --uri="mongodb://username:password@localhost:27017" --out=/backup/path

# 恢复
mongorestore --uri="mongodb://username:password@localhost:27017" /backup/path
```

````







### 3. flag

> flag

```shell
# navicat 连接数据库

1b6e6fb359e7c40241b6d431427ba6ea
```

