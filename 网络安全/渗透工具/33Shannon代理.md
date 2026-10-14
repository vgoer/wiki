<center>Shannon代理</center>





[toc]





## Shannon代理

> Shannon 是一个自主运行的 AI 渗透测试工具，用于测试 Web 应用程序和 API。[github](https://github.com/KeygraphHQ/shannon)



### 1. 安装和使用

> 安装

```shell
# linux mac wsl
npx @keygraph/shannon@latest


# 指定代理
npx @keygraph/shannon@latest setup

# 目标和 代码路径
npx @keygraph/shannon@latest start \
  -u https://your-app.com \
  -r /path/to/your/repo
```

