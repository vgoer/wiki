<center>Yolov10</center>





[toc]









## Yolov10

> YOLOv10: 实时端到端物体检测ai Real-Time End-to-End Object Detection [NeurIPS 2024]
>
> [github](https://github.com/THU-MIG/yolov10)







### 1. 安装

```shell
# 环境
conda create -n yolov10 python=3.9
conda activate yolov10


# 克隆
git clone https://github.com/THU-MIG/yolov10.git
cd yolov10

# 依赖
pip install -r requirements.txt
pip install -e .

# 验证
python app.py
```

> 然后访问 http://127.0.0.1:7860 查看 Gradio 界面，进行简单的推理测试。







### 2. 模型

> 模型： [model url](https://github.com/THU-MIG/yolov10/releases)

| 模型                                                   | 测试规模 | #参数    | 失败次数 | AP值   | 延迟      |
| ------------------------------------------------------ | -------- | -------- | -------- | ------ | --------- |
| [YOLOv10-N](https://huggingface.co/jameslahm/yolov10n) | 640      | 2.3百万  | 6.7G     | 38.5％ | 1.84毫秒  |
| [YOLOv10-S](https://huggingface.co/jameslahm/yolov10s) | 640      | 7.2百万  | 21.6克   | 46.3％ | 2.49毫秒  |
| [YOLOv10-M](https://huggingface.co/jameslahm/yolov10m) | 640      | 15.4百万 | 59.1G    | 51.1％ | 4.74毫秒  |
| [YOLOv10-B](https://huggingface.co/jameslahm/yolov10b) | 640      | 19.1百万 | 92.0克   | 52.5％ | 5.74毫秒  |
| [YOLOv10-L](https://huggingface.co/jameslahm/yolov10l) | 640      | 2440万   | 120.3G   | 53.2％ | 7.28毫秒  |
| [YOLOv10-X](https://huggingface.co/jameslahm/yolov10x) | 640      | 2950万   | 160.4克  | 54.4％ | 10.70毫秒 |

```shell
YOLOv10-N：用于资源极其有限环境的纳米版本。
YOLOv10-S：兼顾速度和精度的小型版本。
YOLOv10-M：通用中型版本。
YOLOv10-B：平衡型，宽度增加，精度更高。
YOLOv10-L：大型版本，精度更高，但计算资源增加。
YOLOv10-X：超大型版本可实现最高精度和性能。
```



```shell
# 直接安装使用
pip install git+https://github.com/THU-MIG/yolov10.git
```

> 1. 推理： 

```shell
# 任意模型
yolo predict model=jameslahm/yolov10{n/s/m/b/l/x}

# 或者python api
from ultralytics import YOLOv10

model = YOLOv10.from_pretrained('jameslahm/yolov10{n/s/m/b/l/x}')
model.predict()
```

> 实列：

```python
from ultralytics import YOLOv10

model = YOLOv10.from_pretrained('jameslahm/yolov10n')
source = 'http://images.cocodataset.org/val2017/000000039769.jpg'
model.predict(source=source, save=True)
```

> 2. 训练： coco数据
>
> 你需要从官方仓库下载预训练的 YOLOv10 模型权重文件。你可以选择不同规模的模型（如 `yolov10n.pt`、`yolov10s.pt` 等）。 [url](https://github.com/THU-MIG/yolov10/releases/download/v1.1/yolov10n.pt)

```shell
yolo detect train data=coco.yaml model=yolov10n/s/m/b/l/x.yaml epochs=500 batch=256 imgsz=640 device=0,1,2,3,4,5,6,7

# python API
from ultralytics import YOLOv10

# 指定预训练模型的路径
model = YOLOv10("yolov10n.pt")  # 替换为实际的模型路径

# 开始训练
model.train(data='coco.yaml', epochs=500, batch=256, imgsz=640)
```





### 3. 应用到生活

> 
