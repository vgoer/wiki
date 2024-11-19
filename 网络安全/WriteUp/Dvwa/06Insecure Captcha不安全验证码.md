<center>Insecure Captcha不安全验证码</center>





[toc]







## Insecure Captcha不安全验证码

> 不安全验证码









### 0. 设置

> *`reCAPTCHA API key missing* from config file: /var/www/html/config/config.inc.php` 随便填入值
>
> google验证码。

```shell
sed -i -e "s/\$_DVWA\[ 'recaptcha_public_key' \].*= '';/\$_DVWA[ 'recaptcha_public_key' ] = '6LdK7xITAAzzAAJQTfL7fu6I-0aPl8KHHieAT_yJg';/" -e "s/\$_DVWA\[ 'recaptcha_private_key' \].*= '';/\$_DVWA[ 'recaptcha_private_key' ] = '6LdK7xITAzzAAL_uw9YXVUOPoIHPZLfw2K1n5NVQ';/" /var/www/html/config/config.inc.php
```







### 1. low

> 代码

```php
```

