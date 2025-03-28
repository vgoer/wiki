<center>PaddleOCR搭建</center>





[toc]







## PaddleOCR搭建

> PaddleOCR 旨在打造一套丰富、领先、且实用的 OCR 工具库，助力开发者训练出更好的模型，并应用落地。
>
> [github](https://github.com/PaddlePaddle/PaddleOCR) [doc](https://paddlepaddle.github.io/PaddleOCR/latest/)





### 1. 环境搭建

```shell
# 创建
conda create -n paddle python=3.9
# 激活
conda activate paddle

# github
git clone https://github.com/PaddlePaddle/PaddleOCR.git

# 安装依赖
cd PaddleOCR
pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```

