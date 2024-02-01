<center>Trio异步框架</center>

[toc]







## Trio异步框架

> Trio [trio](https://trio.readthedocs.io/en/stable/) 项目的目标是为 Python 生成生产质量、 [许可许可的](https://github.com/python-trio/trio/blob/master/LICENSE)异步/等待本机 I/O 库。







### 1. 安装

> 安装

```shell
pip install -U trio
```







### 2. 使用

> 多个线程。

```python
import trio

import numpy as numpy

async def main():
    async with trio.open_nursery() as nursery:

        # 创建通道
        send_channel, receive_channel = trio.open_memory_channel(0)

        # 执行线程，传入通道对象
        nursery.start_soon(thread1, send_channel)
        nursery.start_soon(thread2, receive_channel)

async def thread1(send_channel):
    print("我是线程1，我是公司老板。")
    async with send_channel:
        for i in range(10):
            # 休息一会
            await trio.sleep(1.)
            # 给秘书发指示
            cmd = numpy.random.randint(10)
            print(">老板发出指示：{!r}".format(cmd))
            await send_channel.send(cmd)

async def thread2(receive_channel):
    print("我是线程2，我是个小秘书。")
    async with receive_channel:
        # 等待老板的指示
        async for value in receive_channel:
            print("+秘书收到指示：{!r}".format(value))

trio.run(main)
```

