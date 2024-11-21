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

