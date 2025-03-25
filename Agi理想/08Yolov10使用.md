<center>Yolov10使用</center>





[toc]





## Yolov10使用



### 1. 安装

> 安装，使用，训练。[blog](https://blog.csdn.net/qq_37344058/article/details/139568413)
>
> [模型](https://github.com/THU-MIG/yolov10/releases)

```shell
# 1.在conda创建python3.9环境
conda create -n yolov10 python=3.9
# 2.激活切换到创建的python3.9环境
conda activate yolov10

# 克隆
git clone https://github.com/THU-MIG/yolov10.git
cd yolov10

# 1.切换到yolov10源码根目录下，安装依赖
# 注意：会自动根据你是否有GPU自动选择pytorch版本进行按照，这里不需要自己去选择pytorch和cuda按照，非常良心
pip install -r requirements.txt -i https://pypi.doubanio.com/simple
# 2.运行下面的命令，才可以在命令行使用yolo等命令
pip install -e .

```

> 图片案例

```python
import cv2
from ultralytics import YOLOv10

# 加载模型
model = YOLOv10("yolov10m.pt")

# 批量运算
results = model(["./datasets/group1/images/train/9597185011003UY_20240610_092234_555.png"], stream=True)

for result in results:
    boxes_cls_len = len(result.boxes.cls)
    if not boxes_cls_len:
        # 没有检测到内容
        continue
    for boxes_cls_index in range(boxes_cls_len):
        # 获取类别id
        class_id = int(result.boxes.cls[boxes_cls_index].item())
        # 获取类别名称
        class_name = result.names[class_id]

        # 获取相似度
        similarity = result.boxes.conf[boxes_cls_index].item()

        # 获取坐标值，左上角 和 右下角：lt_rb的值：[1145.1351318359375, 432.6763000488281, 1275.398681640625, 749.5224609375]
        lt_rb = result.boxes.xyxy[boxes_cls_index].tolist()
        # 转为：[[1145.1351318359375, 432.6763000488281], [1275.398681640625, 749.5224609375]]
        lt_rb = [[lt_rb[0], lt_rb[1]], [lt_rb[0], lt_rb[1]]]

        print("类别：", class_name, "相似度：", similarity, "坐标：", lt_rb)

    # 图片展示
    annotated_image = result.plot()
    annotated_image = annotated_image[:, :, ::-1]
    if annotated_image is not None:
        cv2.imshow("Annotated Image", annotated_image)
        cv2.waitKey(0)
        cv2.destroyAllWindows()

```



### 2. 使用gpu

> 使用cuda训练
>
> [pytorch](hhttps://pytorch.org/get-started/locally/)

```shell
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126
```

> 一定要下载对应版本的。

```python
import torch
print('CUDA版本:',torch.version.cuda)
print('Pytorch版本:',torch.__version__)
print('显卡是否可用:','可用' if(torch.cuda.is_available()) else '不可用')
print('显卡数量:',torch.cuda.device_count())
print('当前显卡型号:',torch.cuda.get_device_name())
print('当前显卡的CUDA算力:',torch.cuda.get_device_capability())
print('当前显卡的总显存:',torch.cuda.get_device_properties(0).total_memory/1024/1024/1024,'GB')
print('是否支持TensorCore:','支持' if (torch.cuda.get_device_properties(0).major >= 7) else '不支持')
print('当前显卡的显存使用率:',torch.cuda.memory_allocated(0)/torch.cuda.get_device_properties(0).total_memory*100,'%')
```





### 2. 值作数据集

> - 一般会将所有图片放到一个文件夹，打完标注后，从总的文件夹中，分别分不同的图片到训练集和数据集

数据目录结构：

![image-20250324170932676](.\assets\image-20250324170932676.png)

> - Labelimg是一款开源的数据标注工具，可以标注三种格式。
>   - VOC标签格式，保存为xml文件。
>   - yolo标签格式，保存为txt文件。
>   - createML标签格式，保存为json格式。

```shell
pip install labelimg -i https://pypi.doubanio.com/simple
```

