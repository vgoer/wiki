<center>deepseekR3搭建</center>



[toc]







## deepseekR3搭建

> 我们提出了 DeepSeek-V3，这是一个强大的混合专家 (MoE) 语言模型，总共有 671B 个参数，每个 token 激活 37B。
>
> [github](https://github.com/deepseek-ai/DeepSeek-V3)
>
> [blog](https://deepseek.csdn.net/67ab1a3e79aaf67875cb91fd.html)





### 1. 环境准备

在开始部署之前，确保你的本地环境满足以下要求：

- **操作系统**: Ubuntu 20.04 或更高版本（推荐）
- **Python**: 3.8 或更高版本
- **GPU**: 支持 CUDA 的 NVIDIA GPU（可选，但推荐用于加速）

> Linux 仅适用于 Python 3.10。不支持 Mac 和 Windows。

### 2. 安装依赖

> 更新你的系统并安装必要的依赖包：

```shell
sudo apt-get update
sudo apt-get install -y python3-pip python3-venv git
```





### 3. 创建虚拟环境

```shell
# venv
python3 -m venv deepseek-env
source deepseek-env/bin/activate

# conda
conda create -n deepseek python=3.11
conda activate deepseek
```



### 4. 下载模型

```shell
# 克隆仓库
git clone https://github.com/deepseek-ai/DeepSeek-V3.git
cd DeepSeek-V3

cd DeepSeek-V3/inference
pip install -r requirements.txt -i https://pypi.doubanio.com/simple

# 安装gpu
# 测试gpu
import torch
torch.cuda.is_available()

# pytorch
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126

# 下载预训练模型
# 安装Hugging Face库
pip install transformers accelerate sentencepiece
```

