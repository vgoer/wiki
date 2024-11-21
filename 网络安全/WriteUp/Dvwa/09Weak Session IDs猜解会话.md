<center>Weak Session IDs猜解会话</center>







[toc]









## Weak Session IDs猜解会话

> Weak Session IDs猜解会话











### 1. low

> 低级
>
> 代码

```php

<?php

$html = "";

if ($_SERVER['REQUEST_METHOD'] == "POST") {
    if (!isset ($_SESSION['last_session_id'])) {
        $_SESSION['last_session_id'] = 0;
    }
    $_SESSION['last_session_id']++;
    $cookie_value = $_SESSION['last_session_id'];
    setcookie("dvwaSession", $cookie_value);
}
```

> burp 抓包

```shell
Cookie: dvwaSession=9; PHPSESSID=5skbhhtisu1kf4vfe8jvh4a2l5; security=low
```

> 每次cookie++, 很不安全，容易被猜解。









### 2. medium

> 中级

```php

<?php

$html = "";

if ($_SERVER['REQUEST_METHOD'] == "POST") {
    $cookie_value = time();
    setcookie("dvwaSession", $cookie_value);
}
```

> 时间戳，也容易被猜解。

```shell
Cookie: dvwaSession=1732164337; PHPSESSID=5skbhhtisu1kf4vfe8jvh4a2l5; security=medium
```







### 3. high

> 高级

```php
<?php

$html = "";

if ($_SERVER['REQUEST_METHOD'] == "POST") {
    if (!isset ($_SESSION['last_session_id_high'])) {
        $_SESSION['last_session_id_high'] = 0;
    }
    $_SESSION['last_session_id_high']++;
    $cookie_value = md5($_SESSION['last_session_id_high']);
    setcookie("dvwaSession", $cookie_value, time()+3600, "/vulnerabilities/weak_id/", $_SERVER['HTTP_HOST'], false, false);
}

```

> 用了一个md5加密，还是不安全的。[md5](https://www.runoob.com/php/func-string-md5.html)

````shell
# 还是时间戳1732164416  应该是36位字符。
Cookie: dvwaSession=1732164416; PHPSESSID=5skbhhtisu1kf4vfe8jvh4a2l5; security=high
````











