<center>Debug调试</center>


[toc]


## Debug调试
> 
> 调试器的使用必不可少，本文分享一下PHP调试器的使用。 [搭建](https://juejin.cn/post/7090895599968452621)
>  [2.0](https://juejin.cn/post/7163225953245036552)




### 1.Xdebug下载
> 
> Xdebug下载链接：[xdebug](https://xdebug.org/download/historical)
> 
> 下载对应的PHP版本。



### 2. 配置
1、将xDebug.dll 文件放到php 目录下ext 文件夹下；

2、打开php.ini配置文件，在文件最下面添加如下信息：
```php
 zend_extension = D:\phpstudy\PHPTutorial\php\php-5.4.45-nts\ext\php_xdebug.dll
 ;xdebug的dll文件路径
 xdebug.remote_enable = 1
 ;是否开启远程调试
 xdebug.remote_autostart = 1
 ;是否自动开启远程调试
 xdebug.remote_port = 10001
 ;指定远程调试的端口号
 xdebug.remote_handler = "dbgp"
 ;指定远程调试的处理协议
 xdebug.remote_mode = "req"
 ;可选req或jit，req表示脚本一开始运行就连接远程客户端，jit表示脚本出错时才连接远程客户端

```


重启Apache服务。
> 重启后打开phpinfo(),查看是否有xDebug 字样，如果有，那么恭喜你安装成功。
>



### 2. vscode配置


1.安装PHP Xdebug插件
在vscode直接搜索PHP Xdebug就可以安装：


2.添加 php.exe 文件路径
在file-首选项-setting中选择扩展



