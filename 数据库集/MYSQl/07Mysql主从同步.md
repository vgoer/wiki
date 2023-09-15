<center>Mysql 主从同步</center>



[toc]







## 主从同步

> 问题： 在开发工作中，有时候会遇见某个`sql 语句`需要锁表，导致暂时不能使用读的服务，这样就会影响现有业务，使用主从复制，让主库负责写，从库负责读
>
> **主服务器负责写的操作，也就是增删改，从服务器负责读的操作**
>
> [blog1](https://blog.csdn.net/qq_38225558/article/details/121025633) [blog2](https://blog.csdn.net/qq_58804301/article/details/130061985)





### 1. 流程

1. 主库db的更新事件(update、insert、delete)被写到binlog
2. 主库创建一个binlog dump thread，把binlog的内容发送到从库
3. 从库启动并发起连接，连接到主库
4. 从库启动之后，创建一个I/O线程，读取主库传过来的binlog内容并写入到relay log
5. 从库启动之后，创建一个SQL线程，从relay log里面读取内容，从Exec_Master_Log_Pos位置开始执行读取到的更新事件，将更新内容写入到slave的db

> 注：上述流程为相对流程，并非绝对流程