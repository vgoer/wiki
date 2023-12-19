<center>python便利</center>





[toc]







## python便利

> python便利地方







### 1. 开启服务

> 快速开启一个服务

```shell
 python3 -m http.server 8111
```

> 浏览器访问： `ip:8881`









### 2. 开启venv环境

> 安装依赖不会污染全局

```shell
# 2. 创建虚拟环境
python3 -m venv venv

# 3. 进入虚拟环境
# On Windows:
.\venv\Scripts\activate
# On macOS and Linux:
source venv/bin/activate

#4. 安装依赖
pip install -r requirements.txt
```







