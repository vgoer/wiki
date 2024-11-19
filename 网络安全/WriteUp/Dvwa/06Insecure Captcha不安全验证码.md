<center>Insecure Captcha不安全验证码</center>





[toc]







## Insecure Captcha不安全验证码

> 不安全验证码









### 0. 设置

> *`reCAPTCHA API key missing* from config file: /var/www/html/config/config.inc.php` 随便填入值

```shell
sed -i -e "s/\$_DVWA\[ 'recaptcha_public_key' \].*= '';/\$_DVWA[ 'recaptcha_public_key' ] = '6LdK7xITAAAAAHUZYMKSU2LvPAJm-aNJoFlZuxGV';/" -e "s/\$_DVWA\[ 'recaptcha_private_key' \].*= '';/\$_DVWA[ 'recaptcha_private_key' ] = '6LdK7xITAAAAAMBaAE-QQzKKCZX4KlQXE7XkzqV2';/" /var/www/html/config/config.inc.php
```



