<center>moriarty-corp</center>







[toc]









## moriarty-corp

> 靶场：[hub](https://www.vulnhub.com/entry/boredhackerblog-moriarty-corp,456/)
>
> bilibili: [bili](https://www.bilibili.com/video/BV1Vh411z7s8?spm_id_from=333.788.videopod.sections&vd_source=86b829d6caeffc65037786a84ec2cb17)







### 1. 安装

> 下载主机到本地，用vmware或者vxbox打开靶机。
>
> 网络都应该设置为`桥接`
>
> 介绍: 

```txt
描述
您好，代理。

你来这里是为了一个特殊的使命。

任务是打倒最大的武器供应商之一——莫里亚蒂公司。

在 Web 应用程序中输入 flag{start} 即可开始！

笔记：

Web 面板位于端口 8000（不在范围内。请勿攻击）
标志以 #_flag.txt 格式存储。标志以 flag{} 格式输入。它们通常存储在 / 目录中，但可以位于不同位置。
要暂时停止播放，请暂停虚拟机。不要关闭它。
添加标志后，Web 应用会在后台启动 docker 容器。关闭并重启会造成混乱。
（故事很糟糕，缺乏创意，抱歉）
```







### 2. 主机发现

```shell
# 发现主机ip
sudo arp-scan -l

192.168.0.1     34:96:72:f7:a8:ca       (Unknown)
192.168.0.105   c0:a5:e8:7b:e7:1f       (Unknown)
192.168.0.102   ba:79:4d:d7:be:07       (Unknown: locally administered)

# 发现端口
sudo nmap -p- 192.168.0.102




```





