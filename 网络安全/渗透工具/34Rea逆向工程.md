<center>Rea逆向工程</center>



[toc]





## Rea逆向工程

>  利用代理对任何事物进行逆向工程，从应用程序行为到原生二进制文件。
>
> [github](https://github.com/morluto/rea)



### 1. 安装

> 安装

```shell
npx rea-agents setup

# 指定ai代理 Claude Code、Codex、Cursor
```





### 2. 使用

```shell
示例 1：分析 Electron / Web 应用的逻辑

“帮我弄清楚 Notes 应用（路径 /path/to/app）中的搜索功能是如何工作的，给出分析证据，并为我的项目写一个类似的实现。”

示例 2：反编译/恢复原生应用代码

“分析 /path/to/binary 这个二进制文件的音频处理算法，找出它在调整音量时的函数调用关系，并用 C 语言重构该函数。”
```

