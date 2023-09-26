<center>Redis 主从模式</center>



[toc]









## 主从模式

> Redis虽然读取写入的速度都特别快，但是也会产生读压力特别大的情况。为了分担读压力，Redis支持主从复制，Redis的主从结构可以采用一主多从或者级联结构，Redis主从复制可以根据是否是全量分为全量同步和增量同步。 ==主负责写，从负责读。==
>
> [介绍](https://www.cnblogs.com/h--d/p/11406581.html) , [腾讯云](https://cloud.tencent.com/developer/article/1343837) , [csdn](https://blog.csdn.net/m_nanle_xiaobudiu/article/details/104814617)



