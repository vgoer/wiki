<center>FastApi</center>



[toc]







## FastApi

> Python 高性能 web 框架 - FastApi 全面指南
>
> FastAPI 是一个用于构建 API 的现代、快速（高性能）的 web 框架. 
>
> [blog](https://zhuanlan.zhihu.com/p/397029492)



### 1. 优点 

- **快速**：可与 **NodeJS** 和 **Go** 比肩的极高性能（归功于 Starlette 和 Pydantic）
- **高效编码**：提高功能开发速度约 200％ 至 300％
- **更少 bug**：减少约 40％ 的人为（开发者）导致错误。
- **智能**：极佳的编辑器支持。处处皆可自动补全，减少调试时间
- **简单**：设计的易于使用和学习，阅读文档的时间更短
- **简短**：使代码重复最小化。通过不同的参数声明实现丰富功能。bug 更少
- **健壮**：生产可用级别的代码。还有自动生成的交互式文档
- **标准化**：基于（并完全兼容）API 的相关开放标准：[OpenAPI](https://link.zhihu.com/?target=https%3A//github.com/OAI/OpenAPI-Specification) (以前被称为 Swagger) 和 [JSON Schema](https://link.zhihu.com/?target=https%3A//json-schema.org/)。







## 2. 安装

```python
pip install fastapi

# ASGI 服务器可以使用uvicorn：
pip install uvicorn
```





## 3. 简单示例

> `main.py` 

```php
from typing import Optional

from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def read_root():
    return {"Hello": "World"}


@app.get("/items/{item_id}")
def read_item(item_id: int, q: Optional[str] = None):
    return {"item_id": item_id, "q": q}
```

> 启动服务

```shell
uvicorn main:app --reload
```

> 访问： `http://127.0.0.1:8000/docs`  自动生成的文档
>
> `http://127.0.0.1:8000/items/10?q=100` 查看参数

























