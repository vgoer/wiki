<center>pytest测试库</center>





[toc]







## pytest测试库

> pythest： [pytest](https://github.com/pytest-dev/pytest) 测试库。









### 1. 安装

> install

```shell
pip install pytest
```









### 2. 使用

> 编写测试函数
>
> 在 pytest 中，可以使用 `def` 关键字定义测试函数，并使用 `assert` 断言来验证代码的行为是否符合预期。

```python
# main.py

def add(a,b):
    return a+b
```

```python
# test_main.py
import main

def test_add():
    assert main.add(2,3) == 5
```

> 运行测试。`pytest`

```shell
pytest
```





### 3. 参数化测试

> pytest 支持参数化测试，可以通过 `@pytest.mark.parametrize` 装饰器来定义多组输入参数，并运行测试。

```python
import pytest


def multiply(a,b):
    return a * b


@pytest.mark.parametrize("a,b,expected",[(2, 3, 6), (0, 0, 0), (-1, 1, -1)])
def test_multiply(a,b,expected):
    assert multiply(a,b) == expected
```

