<center>moriarty-corp</center>







[toc]









## moriarty-corp

> 靶场：[hub](https://www.vulnhub.com/entry/boredhackerblog-moriarty-corp,456/)







### 1. 安装

> 下载主机到本地，用vmware或者vxbox打开靶机。
>
> 网络都应该设置为`桥接`





### 2. 主机发现

```shell
# 发现主机ip
sudo arp-scan -l

192.168.0.1     34:96:72:f7:a8:ca       (Unknown)
192.168.0.105   c0:a5:e8:7b:e7:1f       (Unknown)
192.168.0.102   ba:79:4d:d7:be:07       (Unknown: locally administered)

# 发现端口
nmap -p- 192.168.0.102


```





