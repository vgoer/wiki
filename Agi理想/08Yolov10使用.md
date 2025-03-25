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





### 2. 值作数据集

> - 一般会将所有图片放到一个文件夹，打完标注后，从总的文件夹中，分别分不同的图片到训练集和数据集

数据目录结构：

![image-20250324170932676](.\assets\image-20250324170932676.png)