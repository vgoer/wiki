<center>Hilton渗透流程</center>



[toc]









## Hilton渗透流程

> 探索*希尔顿*酒店和遍布全球的独特品牌组合。[hilton](https://www.hilton.com/en/)
>
> [blog](https://blog.csdn.net/qq_45894840/article/details/130360123)







### 1. 信息收集

> 子域名收集

```shell
subfinder -d  hilton.com  -all > hilton.txt

# 目录扫描
python3 dirsearch.py -l hilton.txt --deep-recursive -o outfile.txt
```

