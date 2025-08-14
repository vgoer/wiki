<center>Wrk压力测试</center>





[toc]







## Wrk压力测试

> 现代 HTTP 基准测试工具
>
> [github ](https://github.com/wg/wrk)  [blog](https://www.cnblogs.com/quanxiaoha/p/10661650.html)







### 1. 安装

> 被面试官经常问到之前开发的系统接口 QPS 能达到多少，经常给不出一个数值，支支吾吾，导致整体面试效果降低？

```shell
# ubuntu
sudo apt install wrk

# arch
yay -S wrk
```







### 2. 使用

> 常用命令

```shell
wrk -t12 -c400 -d30s http://www.baidu.com
```

> 这条命令表示，利用 wrk 对 www.baidu.com 发起压力测试，线程数为 12，模拟 400 个并发请求，持续 30 秒。

```shell
使用方法: wrk <选项> <被测HTTP服务的URL>                            
  Options:                                            
    -c, --connections <N>  跟服务器建立并保持的TCP连接数量  
    -d, --duration    <T>  压测时间           
    -t, --threads     <N>  使用多少个线程进行压测   
                                                      
    -s, --script      <S>  指定Lua脚本路径       
    -H, --header      <H>  为每一个HTTP请求添加HTTP头      
        --latency          在压测结束后，打印延迟统计信息   
        --timeout     <T>  超时时间     
    -v, --version          打印正在使用的wrk的详细版本信息
                                                      
  <N>代表数字参数，支持国际单位 (1k, 1M, 1G)
  <T>代表时间参数，支持时间单位 (2s, 2m, 2h)
```

> PS: 关于线程数，并不是设置的越大，压测效果越好，线程设置过大，反而会导致线程切换过于频繁，效果降低，一般来说，推荐设置成压测机器 CPU 核心数的 2 倍到 4 倍就行了。





### 3. 测试报告

执行压测命令:

```cmd
wrk -t12 -c400 -d30s --latency http://www.baidu.com
```

```shell
Running 30s test @ http://www.baidu.com （压测时间30s）
  12 threads and 400 connections （共12个测试线程，400个连接）
			  （平均值） （标准差）  （最大值）（正负一个标准差所占比例）
  Thread Stats   Avg      Stdev     Max   +/- Stdev
    （延迟）
    Latency   386.32ms  380.75ms   2.00s    86.66%
    (每秒请求数)
    Req/Sec    17.06     13.91   252.00     87.89%
  Latency Distribution （延迟分布）
     50%  218.31ms
     75%  520.60ms
     90%  955.08ms
     99%    1.93s 
  4922 requests in 30.06s, 73.86MB read (30.06s内处理了4922个请求，耗费流量73.86MB)
  Socket errors: connect 0, read 0, write 0, timeout 311 (发生错误数)
Requests/sec:    163.76 (QPS 163.76,即平均每秒处理请求数为163.76)
Transfer/sec:      2.46MB (平均每秒流量2.46MB)
```

> *(QPS 163.76,即平均每秒处理请求数为163.76)*