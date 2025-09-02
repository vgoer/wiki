<center>Yolov11</center>





[toc]









## Yolov11

> 介绍 [Ultralytics](https://www.ultralytics.com/)[YOLO11](https://github.com/ultralytics/ultralytics)YOLO11 基于[深度学习](https://www.ultralytics.com/glossary/deep-learning-dl)和[计算机视觉](https://www.ultralytics.com/blog/everything-you-need-to-know-about-computer-vision-in-2025)领域的尖端技术，在速度和[准确性](https://www.ultralytics.com/glossary/accuracy)方面具有无与伦比的性能。其流线型设计使其适用于各种应用，并可轻松适应从边缘设备到云 API 等不同硬件平台。
>
> [github](https://github.com/ultralytics/ultralytics)
>
> [doc](https://docs.ultralytics.com/zh)







### 1. 安装

> 官方文档：

```shell
git clone git@github.com:ultralytics/ultralytics.git yolov11

# 安装
pip install ultralytics

# 测试安装成功
import ultralytics

# 测试gpu
import torch
torch.cuda.is_available()

# pytorch
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126
```







### 2. 模型下载

> 下载模型

![image-20250330205228673](.\assets\image-20250330205228673.png)

> 准备好需要训练的数据集。 图片和视频

```python
from ultralytics import YOLO

if __name__ == '__main__':

    # Load a model
    model = YOLO(model=r'D:\Agi\ultralytics\yolo11n-seg.pt')
    # 图片和视频
    model.predict(source=r'D:\Agi\ultralytics\28382923430-1-192.mp4',
                  save=True,
                  show=True,
                  )
```









### 3. 数据集准备

> **工具推荐**：使用 **LabelImg** 或 **Labelme** 进行目标检测标注。LabelImg适合矩形框标注，Labelme支持多边形标注（适合分割任务）

> 目录结构

```shell
datasets/
├── images/
│   ├── train/  # 训练集图片
│   └── val/    # 验证集图片
├── labels/
│   ├── train/  # 训练集标签（与图片同名.txt文件）
│   └── val/    # 验证集标签
```

```shell
pip install labelimg
labelimg
```

- 打开图片文件夹（`images`），设置标签保存路径（`labels`）。
- 切换标注格式为 **YOLO**，使用快捷键 `W` 开始框选目标，输入类别名称（英文），完成后保存为 `.txt` 文件46。

![image-20250330211629176](.\assets\image-20250330211629176.png)

> 开启自动生成标注文件

![image-20250330212001665](.\assets\image-20250330212001665.png)

> 数据集划分： 
>
> 训练自己的yolov11检测模型，数据集需要划分为训练集、验证集和测试集，这里提供一个参考代码,划分比例为8：1：1，也可以按照自己的比例划分，获取的数据集划分过了则不用重复划分。

```python
# 作者：CSDN-笑脸惹桃花 https://blog.csdn.net/qq_67105081?type=blog
# github:peng-xiaobai https://github.com/peng-xiaobai/Dataset-Conversion
import os
import shutil
import random
 
# random.seed(0)  #随机种子，可自选开启
def split_data(file_path, label_path, new_file_path, train_rate, val_rate, test_rate):
	images = os.listdir(file_path)
	labels = os.listdir(label_path)
	images_no_ext = {os.path.splitext(image)[0]: image for image in images}
	labels_no_ext = {os.path.splitext(label)[0]: label for label in labels}
	matched_data = [(img, images_no_ext[img], labels_no_ext[img]) for img in images_no_ext if img in labels_no_ext]
 
	unmatched_images = [img for img in images_no_ext if img not in labels_no_ext]
	unmatched_labels = [label for label in labels_no_ext if label not in images_no_ext]
	if unmatched_images:
		print("未匹配的图片文件:")
		for img in unmatched_images:
			print(images_no_ext[img])
	if unmatched_labels:
		print("未匹配的标签文件:")
		for label in unmatched_labels:
			print(labels_no_ext[label])
 
	random.shuffle(matched_data)
	total = len(matched_data)
	train_data = matched_data[:int(train_rate * total)]
	val_data = matched_data[int(train_rate * total):int((train_rate + val_rate) * total)]
	test_data = matched_data[int((train_rate + val_rate) * total):]
 
	# 处理训练集
	for img_name, img_file, label_file in train_data:
		old_img_path = os.path.join(file_path, img_file)
		old_label_path = os.path.join(label_path, label_file)
		new_img_dir = os.path.join(new_file_path, 'train', 'images')
		new_label_dir = os.path.join(new_file_path, 'train', 'labels')
		os.makedirs(new_img_dir, exist_ok=True)
		os.makedirs(new_label_dir, exist_ok=True)
		shutil.copy(old_img_path, os.path.join(new_img_dir, img_file))
		shutil.copy(old_label_path, os.path.join(new_label_dir, label_file))
	# 处理验证集
	for img_name, img_file, label_file in val_data:
		old_img_path = os.path.join(file_path, img_file)
		old_label_path = os.path.join(label_path, label_file)
		new_img_dir = os.path.join(new_file_path, 'val', 'images')
		new_label_dir = os.path.join(new_file_path, 'val', 'labels')
		os.makedirs(new_img_dir, exist_ok=True)
		os.makedirs(new_label_dir, exist_ok=True)
		shutil.copy(old_img_path, os.path.join(new_img_dir, img_file))
		shutil.copy(old_label_path, os.path.join(new_label_dir, label_file))
	# 处理测试集
	for img_name, img_file, label_file in test_data:
		old_img_path = os.path.join(file_path, img_file)
		old_label_path = os.path.join(label_path, label_file)
		new_img_dir = os.path.join(new_file_path, 'test', 'images')
		new_label_dir = os.path.join(new_file_path, 'test', 'labels')
		os.makedirs(new_img_dir, exist_ok=True)
		os.makedirs(new_label_dir, exist_ok=True)
		shutil.copy(old_img_path, os.path.join(new_img_dir, img_file))
		shutil.copy(old_label_path, os.path.join(new_label_dir, label_file))
	print("数据集已划分完成")
 
if __name__ == '__main__':
	file_path = r"f:\data\JPEGImages"  # 图片文件夹
	label_path = r'f:\data\labels'  # 标签文件夹
	new_file_path = r"f:\VOCdevkit"  # 新数据存放位置
	split_data(file_path, label_path, new_file_path, train_rate=0.8, val_rate=0.1, test_rate=0.1)
```

> #### **划分训练集与验证集**
>
> - 手动或使用脚本按比例（如8:2）划分图片到`train`和`val`文件夹。
> - 示例Python脚本自动划分：

```shell
pip install scikit-learn
```

```python
import os
import shutil
from sklearn.model_selection import train_test_split

image_dir = "D:\Agi\\ultralytics\databases\Person1\images"
all_images = os.listdir(image_dir)
train, val = train_test_split(all_images, test_size=0.2, random_state=42)

# 将图片和对应标签复制到train/val文件夹
for img in train:
    shutil.copy(os.path.join(image_dir, img), "D:\Agi\\ultralytics\databases\Person1\images\\train")
    shutil.copy(os.path.join("D:\Agi\\ultralytics\databases\Person1\labels", img.replace(".jpg", ".txt")), "D:\Agi\\ultralytics\databases\Person1\labels\\train")
# 重复同样步骤处理val集

```

> 自己总结的目录： 

![image-20250331003304674](.\assets\image-20250331003304674.png)



* 标注工具2

```shell
pip install labelme
```





### 4. 训练

> data.yaml

```yaml
train: D:\Agi\ultralytics\databases\Person1\images\train # train images (relative to 'path') 128 images
val: D:\Agi\ultralytics\databases\Person1\images\val  # val images (relative to 'path') 128 images

nc: 1

# Classes
names: ['person']
```



> `train.py`

```python
import warnings
warnings.filterwarnings('ignore')
from ultralytics import YOLO

if __name__ == '__main__':
    # model.load('yolo11n.pt') # 加载预训练权重,改进或者做对比实验时候不建议打开，因为用预训练模型整体精度没有很明显的提升
    model = YOLO(model=r'D:\Agi\ultralytics\ultralytics\cfg\models\11\yolo11.yaml')
    model.train(data=r'D:\Agi\ultralytics\databases\Person1\data.yaml',
                imgsz=640,
                epochs=100,
                batch=4,
                workers=0,
                device='',
                optimizer='SGD',
                close_mosaic=10,
                resume=False,
                project='runs/train',
                name='exp',
                single_cls=False,
                cache=False,
                )

```

> 训练好的模型使用

```python
from ultralytics import YOLO

if __name__ == '__main__':

    # Load a model
    model = YOLO(model=r'D:\Agi\ultralytics\databases\Person1\runs\train\exp\weights\best.pt')
    # 图片和视频
    model.predict(source=r'D:\Agi\ultralytics\28382923430-1-192.mp4',
                  save=True,
                  show=True,
                  )
```

