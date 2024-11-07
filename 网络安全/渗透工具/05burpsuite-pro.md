<center>burpsuite-pro</center>







[toc]







## burpsuite-pro

> Burp Suite 是一个用于执行 Web 应用程序安全测试的集成平台。其各种工具无缝协作，支持整个测试过程，从初始映射和分析应用程序的攻击面，到查找和利用安全漏洞。
>
> [wiki](https://wiki.archlinux.org/title/Burp_Suite)









### 1. 安装

>  openjdk: [wiki](https://wiki.archlinuxcn.org/wiki/Java#OpenJDK)

```shell
# java 环境
yay -S extra/jdk-openjdk

# burp pro
yay -S burpsuite-pro

# free 
yay -S burpsuite
```





### 2. 激活

> [github](https://github.com/gt0day/Burp-Suite)

- [1] - 在 /etc/opt/Burpsuite 中创建一个文件夹

  > sudo mkdir /etc/opt/Burpsuite

- [2] - 将 BurpLoaderKeygen.jar 和 burpsuite_pro_vxxxx.x.jar 移动到同一个文件夹中

  > sudo mv BurpLoaderKeygen.jar burpsuite_pro_v2023.7.jar /etc/opt/Burpsuite && cd /etc/opt/Burpsuite

- [3] -java -jar BurpLoaderKeygen.jar

- [4] - 复制我们需要使用的快捷方式的加载器命令并按 R...

- [5] - 从 burpsuite 加载程序复制许可证并将许可证密钥粘贴到 burpsuite 中，然后按下一步

- [6] - 点击手动激活，复制请求，然后粘贴到 burpsuite loeader 中的激活请求中

- [7] - 从 burpsuite 加载器复制激活响应并粘贴到 burpsuite 中（粘贴响应）

  > 最后，从步骤 4 中的加载程序命令创建一个快捷方式。