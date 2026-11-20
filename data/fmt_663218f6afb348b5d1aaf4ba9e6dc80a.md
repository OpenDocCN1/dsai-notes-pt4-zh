
# AI夏令营第三期 - 脑PET图像分析和疾病预测挑战赛教程

> 来源：https://datawhaler.feishu.cn/docx/K6RndJlW5ocYfZxJ02ucGIwmnqd

> 本教程会带领大家项目制学习，由浅入深，逐渐进阶。从跑通最简的Baseline，精读Baseline，到底层算法原理与进阶实践技巧的学习，千里之行，始于足下，从这里，开启你的 AI 学习之旅吧

> —— Datawhale贡献者团队

# 赛题解析

> 脑PET图像分析和疾病预测挑战赛

> https://challenge.xfyun.cn/topic/info?type=pet-2023&ch=ymfk4uU

## 赛题背景

脑PET全称为脑部正电子发射计算机断层显像(brain positron emission tomography PET)，是反映脑部病变的基因、分子、代谢及功能状态的显像。它是利用正电子核素标记葡萄糖等人体代谢物作为显像剂，通过病灶对显像剂的摄取来反映其代谢变化，从而为临床提供疾病的生物代谢信息，为脑癫痫病、脑肿瘤、帕金森病、阿尔茨海默综合征等提供了有效的检测手段。可利用脑PET图像检测出轻度认知障碍病灶，并提前介入治疗，从而延缓发病，对后续患者康复治疗有着积极的意义。因此本赛题以轻度认知障碍为例对脑PET图像进行分析与疾病预测。

## 赛事任务

为研究基于脑PET图像的疾病预测，本次大赛提供了海量脑PET数据集作为脑PET图像检测数据库的训练样本，参赛者需根据提供的样本构建模型，对轻度认知障碍进行分析和预测。

脑PET图像检测数据库，记录了老年人受试志愿者的脑PET影像资料，其中包括确诊为轻度认知障碍（MCI）患者的脑部影像数据和健康人（NC）的脑部影像数据。

被试者按医学诊断分为两类：

NC：健康

MCI：轻度认知障碍

## 赛题数据集

本次大赛所用脑PET图像检测数据库，图像格式为nii。

![](img/70c73a7f4e2d39d08c1ba78f6cd03926.png)

## 评价指标

本次竞赛的评价标准采用`F1_score`，分数越高，效果越好。

# 解题思路

使用计算机视觉方法进行脑PET图像分类可以帮助提取脑部疾病的特征并进行疾病预测。下面是一个使用计算机视觉思路进行脑PET图像分类的一般步骤：

![](img/64129713958c928b83df452878c78f89.png)

# 任务1：统计方法Baseline

> Baseline是一份简易的入门教程，可以帮助同学们迈出 AI 训练大师之路的第一步。建议入门的同学可以暂时不用着急去弄懂各个代码的原理，先跑通代码，动手实践，看到成绩。

## 快速跑通

快速跑通全流程，我们基于百度AI Studio，将本教程Baseline部署在线上平台，可一键fork运行代码，看到成绩。

一键运行：https://aistudio.baidu.com/aistudio/projectdetail/6554002?sUid=377372&shared=1&ts=1689768420818

+ 运行时，选择CPU2核8G的配置
+ 总运行时间大约需要3分钟，请耐心等待。
+ 视频演示：AI夏令营：开营仪式|实践全流程基础演示_哔哩哔哩_bilibili，对应的CV章节

![](img/ee6206bd5384f90036af1d8e292c4ca8.png)

![](img/a853571e37ebdca26e3c906ff219bead.png)

输出`submit.csv`文件后，右键可下载到本地，在[赛题提交结果](https://challenge.xfyun.cn/topic/info?type=pet-2023&ch=vWxQGFU)提交文件，获取分数。

![](img/12a645c093a841a344f677a2abc3ce30.png)

![](img/a378e79274e4985115fa13af3b42fca2.png)

## 实践步骤

> 视频讲解：https://www.bilibili.com/video/BV1pm4y1L7H4

### 图像特征统计（传统方法）

图像特征统计是指对图像中的像素或图像区域进行统计分析，以提取和描述图像的特征信息。

1. 占比特征：通过统计图像中不同像素值或颜色通道的像素数量，计算其在整个图像中的比例或占比。
1. 边缘特征：边缘是图像中像素值或颜色发生剧烈变化的区域，通常表示物体的边界或纹理的边界。
1. 纹理特征：描述了图像中的纹理信息，反映了图像的细节和结构。

### 方法优缺点（运行时间5分钟，不要GPU）

+ 思路简单，不需要GPU就可以运行完成。
+ 精度较差，且容易受到形态比较形似的影响。

### 代码流程图

![](img/758fa476141a81343c310ccbfe31c10a.png)

### 完整代码如下

```python
import glob                # 获取文件路径
import numpy as np
import pandas as pd
import nibabel as nib      # 处理医学图像数据
from nibabel.viewers import OrthoSlicer3D    # 图像可视化
from collections import Counter              # 计数统计

# 读取训练集文件路径
train_path = glob.glob('./脑PET图像分析和疾病预测挑战赛公开数据/Train/*/*')
test_path = glob.glob('./脑PET图像分析和疾病预测挑战赛公开数据/Test/*')

# 打乱训练集和测试集的顺序
np.random.shuffle(train_path)
np.random.shuffle(test_path)

# 对PET文件提取特征
def extract_feature(path):
    # 加载PET图像数据
    img = nib.load(path)
    # 获取第一个通道的数据
    img = img.dataobj[:, :, :, 0]
    
    # 随机筛选其中的10个通道提取特征
    random_img = img[:, :, np.random.choice(range(img.shape[2]), 10)]
    
    # 对图片计算统计值
    feat = [
        (random_img != 0).sum(),               # 非零像素的数量
        (random_img == 0).sum(),               # 零像素的数量
        random_img.mean(),                     # 平均值
        random_img.std(),                      # 标准差
        len(np.where(random_img.mean(0))[0]),  # 在列方向上平均值不为零的数量
        len(np.where(random_img.mean(1))[0]),  # 在行方向上平均值不为零的数量
        random_img.mean(0).max(),              # 列方向上的最大平均值
        random_img.mean(1).max()               # 行方向上的最大平均值
    ]
    
    # 根据路径判断样本类别（'NC'表示正常，'MCI'表示异常）
    if 'NC' in path:
        return feat + ['NC']
    else:
        return feat + ['MCI']

# 对训练集进行30次特征提取，每次提取后的特征以及类别（'NC'表示正常，'MCI'表示异常）被添加到train_feat列表中。
train_feat = []
for _ in range(30):
    for path in train_path:
        train_feat.append(extract_feature(path))
     
# 对测试集进行30次特征提取   
test_feat = []
for _ in range(30):
    for path in test_path:
        test_feat.append(extract_feature(path))
        
# 使用训练集的特征作为输入，训练集的类别作为输出，对逻辑回归模型进行训练。
from sklearn.linear_model import LogisticRegression
m = LogisticRegression(max_iter=1000)
m.fit(
    np.array(train_feat)[:, :-1].astype(np.float32),  # 特征
    np.array(train_feat)[:, -1]                       # 类别
)

# 对测试集进行预测并进行转置操作，使得每个样本有30次预测结果。
test_pred = m.predict(np.array(test_feat)[:, :-1].astype(np.float32))
test_pred = test_pred.reshape(30, -1).T

# 对每个样本的30次预测结果进行投票，选出最多的类别作为该样本的最终预测类别，存储在test_pred_label列表中。
test_pred_label = [Counter(x).most_common(1)[0][0] for x in test_pred]

# 生成提交结果的DataFrame，其中包括样本ID和预测类别。
submit = pd.DataFrame(
    {
        'uuid': [int(x.split('/')[-1][:-4]) for x in test_path],  # 提取测试集文件名中的ID
        'label': test_pred_label                                  # 预测的类别
    }
)

# 按照ID对结果排序并保存为CSV文件
submit = submit.sort_values(by='uuid')
submit.to_csv('submit1.csv', index=None)
```

在提交结果处提交，提交`submit.csv`文件，在排名结果页查看自己的成绩排名。

# 任务2：深度学习Baseline

## Pytorch CNN

> 视频讲解：https://www.bilibili.com/video/BV18h4y1V7as/

卷积神经网络（Convolutional Neural Network，CNN）是一种深度学习模型，广泛用于图像识别、计算机视觉和模式识别任务中。CNN 在处理具有网格结构数据（如图像）时表现出色，它能够自动学习和提取图像中的特征，并在分类、定位和分割等任务中取得优秀的性能。

方法优缺点（运行时间20分钟，如果有GPU可以更快）：

+ CNN带来的精度更好，但需要训练更长的时间
+ CNN模型调优需要GPU

### Step1：自定义数据集
