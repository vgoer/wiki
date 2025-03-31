<center>Yolov12</center>



[toc]









## Yolov12

> YOLOv12：以注意力为中心的实时物体检测器.
>
> [github](https://github.com/sunsmarterjie/yolov12)







### 1. 安装环境

> install 
>
> flash-attention: 快速且内存高效的精确注意力. [github](https://github.com/Dao-AILab/flash-attention)
>
> windows: 注意力机制： [github](https://huggingface.co/lldacing/flash-attention-windows-wheel/tree/main)

```shell
# 可以不要这个注意力，在requirements.txt删除对应行即可
wget https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.3/flash_attn-2.7.3+cu11torch2.2cxx11abiFALSE-cp311-cp311-linux_x86_64.whl

https://huggingface.co/lldacing/flash-attention-windows-wheel/blob/main/flash_attn-2.7.0.post2%2Bcu124torch2.4.0cxx11abiFALSE-cp310-cp310-win_amd64.whl


conda create -n yolov12 python=3.11
conda activate yolov12

git clone git@github.com:sunsmarterjie/yolov12.git

pip install -r requirements.txt
pip install -e .
```

