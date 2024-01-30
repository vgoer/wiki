<center>FastApi</center>





[toc]







## FastApi

> FastAPI 是一个现代、快速（高性能）的 Web 框架，用于基于标准 Python 类型提示使用 Python 3.8+ 构建 API。
>
> [fastapi](https://fastapi.tiangolo.com/)









### 1. 安装

> 安装

```shell
python3 -m venv venv

# 安装
pip install fastapi

# asgi安装，支持异步调用
pip install uvicorn
```







### 2. 简单使用

> 列子

```python
from typing import Optional
from fastapi import FastAPI


app = FastAPI()

@app.get("/")
def read_root():
    return {"hello:" "world"}

@app.get("/items/{item_id}")
def read_item(item_id: int, q: Optional[str] = None):
    return { "id为:" : item_id, "q:":q}
```

> 启动： 命令

```shell
# 异步             热更新    端口
uvicorn  main:app --reload --port 8001
```

> 访问网站： `ip:8001` wc，牛皮
>
> 等等，接口文档，自带。 
>
> `ip:8001/docs`:     wc，wc。





