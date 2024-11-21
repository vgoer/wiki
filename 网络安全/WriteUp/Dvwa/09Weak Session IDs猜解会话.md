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











