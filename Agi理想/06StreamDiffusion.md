<center>StreamDiffusion</center>





[toc]







## StreamDiffusion

> StreamDiffusion：实时交互生成的管道级解决方案 [github](https://github.com/cumulo-autumn/StreamDiffusion)
>
> 看仓库：很多内容。









### 1. 安装

> realtime-txt2img: docker安装

```shell
# 启动镜像：
docker run -it --name sd --network host --gpus all bucess/streamdiffusion:1 /bin/bash
# 启动服务：
cd demo/realtime-txt2img/server/
python main.py
```

> img2img 演示