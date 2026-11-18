
# Task2：建模方案解读与进阶

> 来源：[https://datawhaler.feishu.cn/docx/XiMVdKRU9oNqFpxaQbvcxp31nGf](https://datawhaler.feishu.cn/docx/XiMVdKRU9oNqFpxaQbvcxp31nGf)

> **教程贡献者说：**

> 继我们在Task1中完成了YOLO基础实践之后，现在我们将进一步探索YOLO算法。我们将掌握YOLO在实际项目中的应用，包括数据集准备、模型训练、评估和部署。

> 需注意：本文档主要提供YOLO模型介绍，具体如何使用在咱们的赛题中就欢迎大家在评论区和学习群里积极讨论啦～

> PS：示例代码就在https://www.modelscope.cn/datasets/Datawhale/AI_Camp5_baseline_CV.git

## YOLO 模型介绍

物体检测是计算机视觉领域的一个重要任务，它的目标是在图像或视频帧中识别和定位感兴趣的物体。物体检测算法不仅要识别图像中的对象属于哪个类别，还要确定它们在图像中的具体位置，通常以边界框（bounding box）的形式表示。以下是物体检测的一些关键概念和步骤：

1. **输入**：物体检测算法的输入通常是一张图像或视频帧。

1. **特征提取**：算法使用深度学习模型（如卷积神经网络CNN）来提取图像的特征。这些特征捕捉了图像中的视觉信息，为后续的物体识别和定位提供基础。

1. **候选区域生成**：在某些检测算法中，如基于区域的卷积神经网络（R-CNN）及其变体，首先需要生成图像中的候选区域，这些区域可能包含感兴趣的物体。

1. **区域分类和边界框回归**：对于每个候选区域，算法需要判断它是否包含特定类别的物体，并预测物体的边界框。这通常涉及到分类任务和回归任务的结合。

1. **非极大值抑制（NMS）**：在检测过程中，可能会产生多个重叠的边界框，用于表示同一物体。NMS是一种常用的技术，用于选择最佳的边界框并去除多余的框。

![](img/7b87baba6c641d293a41f536a094c890.png)

物体检测算法主要分为两类：One-Stage（一阶段）和Two-Stage（两阶段）模型。

![](img/7e3f113761a59a17f16dc977911b5091.png)

+ **One-Stage模型**，如YOLO（You Only Look Once）和SSD（Single Shot Detection），直接在单次网络评估中预测图像中所有物体的类别和位置信息。这种方法的优点是速度快，适合实时应用，但可能在精度上不如Two-Stage模型。

+ **Two-Stage模型**，如Faster R-CNN，首先使用区域提议网络（Region Proposal Network, RPN）生成候选区域，然后对这些区域进行分类和边界框的精细调整。这种方法的优点是精度高，但速度相对较慢。

One-Stage模型通常在单个卷积网络中同时预测类别和位置，而Two-Stage模型则将检测任务分解为两个阶段：区域提议和候选区域的分类与定位。One-Stage模型因为省略了区域提议步骤，所以能够实现更快的检测速度，但这可能会以牺牲一些精度为代价。相比之下，Two-Stage模型通过两步过程提高了检测的准确性，但同时也增加了计算的复杂性和时间消耗。

> 在实际应用中，选择哪种模型取决于特定场景的需求。如果对速度有较高要求，如视频流处理或实时监控，One-Stage模型可能更合适。如果对精度有更高要求，如在需要高精度识别的科研或专业领域，Two-Stage模型可能更加适用。

YOLO，全称为"You Only Look Once"，是一种流行的实时目标检测算法，由Joseph Redmon等人于2015年首次提出。YOLO的核心思想是将目标检测任务视为一个单一的回归问题，直接从图像像素到边界框坐标和类别概率的映射。这种设计使得YOLO能够以非常快的速度进行目标检测，同时保持较高的精度，特别适合需要实时处理的应用场景。

![](img/da5b897dd8e38685651daeb33378e4a3.png)

1. YOLO算法的一个显著特点是它在单个网络评估中同时预测多个边界框和类别概率，而不是像传统的滑动窗口方法那样多次评估。

1. YOLO使用一个卷积神经网络（CNN）来提取图像特征，然后使用这些特征来预测边界框和类别概率。YOLO的网络结构通常包括多个卷积层和池化层。

1. YOLO为每个边界框预测一个置信度，这个置信度反映了边界框包含目标的概率以及预测的类别。置信度的计算公式是：`Pr(Object) * IOU(pred, truth)`，其中`Pr(Object)`表示格子中存在目标的概率，`IOU(pred, truth)`表示预测框和真实框的交并比。

YOLO（You Only Look Once）是一种革命性的目标检测算法，以其快速和高效的性能而闻名。自2015年YOLOv1的首次推出以来，YOLO系列已经经历了多次迭代，每一次迭代都在速度、准确性和计算效率方面做出了显著的贡献。

| 版本号 | 年份 | 主要贡献与特点 |

| --- | --- | --- |

| YOLOv1 | 2015 | 实时端到端物体检测，将检测视为回归问题，单次网络评估预测位置和类别。 |

| YOLOv2 | 2016 | 引入批量归一化，高分辨率分类器，全卷积网络，能检测超过9000个类别。 |

| YOLOv3 | 2018 | 使用更深的Darknet-53网络，引入特征金字塔网络提高多尺度目标检测能力。 |

| YOLOv4 | 2020 | 结合CSPNet、PANet、SAM等技术，提高特征提取和检测效率。 |

| YOLOv5 | 2020 | 使用Pytorch框架，不同大小模型版本适应不同环境，易用性和性能显著改进。 |

| YOLOv6 | 2021 | 多种不同尺寸模型适应工业应用，继续在YOLO系列基础上改进。 |

| YOLOv7 | 2022 | 架构变化和一系列免费包提高准确率，保持实时性。 |

| YOLOv8 | 2023 | 新功能和改进，包括新的骨干网络、Anchor-Free检测头和新损失函数，提升性能和灵活性。 |

| YOLOv9 | 2023 | 引入可编程梯度信息（PGI）和基于梯度路径规划的通用高效层聚合网络（GELAN）架构。 |

| YOLOv10 | 2024 | 通过消除非最大抑制（NMS-Free）和优化各种模型组件，实现了最先进的性能。 |

![](img/85abbbb44c41213304ccff51525cb80a.png)

> YOLOv1至YOLOv3主要由Joseph Redmon及其合作者提出，而后续版本则由不同的研究者和团队负责开发。YOLOv4之后的版本，尤其是YOLOv5和YOLOv8，由Ultralytics公司开发和维护。YOLOv5和YOLOv8也是现在比较流行的版本。

## YOLO 数据集格式

YOLO算法的标注格式主要使用`.txt`文件来存储图像中物体的标注信息。每个图像都有一个对应的`.txt`文件，文件中的每行表示一个物体的标注，包括物体的类别索引和边界框（bounding box）的坐标。以下是YOLO标注格式的详细介绍：

1. **类别索引**：每个物体的类别由一个整数索引表示，索引对应于预先定义的类别列表。

1. **边界框坐标**：边界框由其中心点坐标(`x_center`, `y_center`)和宽度`width`、高度`height`组成。这些值通常是归一化到图像宽度和高度的比例值，范围在0到1之间。

1. **坐标格式**：边界框坐标通常按照[`class_index` `x_center` `y_center` `width` `height`]的格式记录，其中`class_index`是类别索引，`x_center`和`y_center`是边界框中心点的x和y坐标，`width`和`height`是边界框的宽度和高度。

![](img/bda1f14996ab956e193f72413bed09f0.png)

在YOLO的训练过程中，这样的配置文件允许用户轻松地指定数据集的位置和类别信息，从而无需硬编码在训练脚本中。具体来说，这段配置的含义如下：

+ `path`: 指定了数据集的根目录路径，即所有数据子文件夹的上级目录。这里的路径是相对于当前配置文件的路径或者相对于执行训练脚本的工作目录。

+ `train`: 定义了训练集图像的相对路径。在训练模型时，程序会在指定的路径下查找图像文件。

+ `val`: 定义了验证集图像的相对路径。验证集用于在训练过程中评估模型性能，避免过拟合。

+ `nc`: 表示类别的数量，这里设置为2，意味着数据集中有两类物体需要被识别。

+ `names`: 是一个列表，包含了每个类别的名称。这里有两个类别，名称分别是"0"和"1"。这些名称在训练和测试过程中用于引用特定的类别。

```
# Train/val/test sets as 1) dir: path/to/imgs, 2) file: path/to/imgs.txt, or 3) list: [path/to/imgs1, path/to/imgs2, ..]
path: ../dataset/  # dataset root dir
train: images/train/  # train images (relative to &#x27;path&#x27;) 128 images
val: images/val/  # train images (relative to &#x27;path&#x27;) 128 images

# Classes
nc: 2  # number of classes
names: [&quot;0&quot;, &#x27;1&#x27;]  # class names
```

## YOLO 训练日志

在使用YOLO进行训练时，生成的`exp/detect/train`类型的文件夹是训练过程中的一个关键组成部分。

+ **模型权重** (`.pt` 或 `.pth` 文件): 训练过程中保存的模型权重，可以用于后续的测试或继续训练。

+ **日志文件** (`.log` 文件): 包含训练过程中的所有输出信息，如损失值、精度、速度等。

+ **配置文件** (`.yaml` 或 `.cfg` 文件): 训练时使用的配置文件副本，记录了数据路径、类别名、模型架构等设置。

+ **图表和可视化**: 有时YOLO会生成训练过程中的性能图表，如损失曲线、精度曲线等。

+ **测试结果**: 如果训练过程中包括了测试阶段，可能会有测试结果的保存，如检测结果的图片或统计数据。

![](img/a2c5c48ca842cc1a3a5dba20473aa38b.png)

1. `F1_curve.png`, `PR_curve.png`, `P_curve.png`, `R_curve.png`: 包含模型性能的不同评估指标曲线图，如F1分数`F1_curve.png`、精确率-召回率曲线`PR_curve.png`、精确率`P_curve.png`和召回率`R_curve.png`。

1. `results.csv`: 一个CSV文件，包含模型训练或测试的结果数据。

1. `results.png`: 包含训练结果的汇总图表或图像。

1. `train_batch*****.jpg`: 包含训练过程中不同批次的图像和它们的标注。

1. `val_batch0_labels.jpg`, `val_batch0_pred.jpg`: 验证集批次的图像，可能包含真实标签(labels)和模型预测(pred)的可视化。

1. `weights/`: 一个目录，通常包含模型训练过程中保存的权重文件，如`.pt`或`.pth`文件。

在训练过程中和训练完成后，都可以查看训练日志。可以优先查看`results.png`，图像的内容类似如下。从验证集上的损失 (`val/box_loss`, `val/cls_loss`, `val/dfl_loss`) 和性能指标可以评估模型在未见数据上的泛化能力。在下面的训练日志中，我们发现模型在验证集发生了过拟合。

![](img/fb3f5a19451ffb1c277acafba1d24cfb.png)

---

## 赛题再剖析

上面介绍了那么多关于YOLO的知识点，那我们为什么要选择YOLO这个模型而不用其他的模型呢？

1. **速度快**：YOLO的设计目标是实现快速的对象检测，它在保持相对高准确度的同时，能够实现高帧率的实时检测。

1. **易于实现**：YOLO的架构相对简单，易于理解和实现，这使得它在学术和工业界都得到了广泛的应用。

1. **版本迭代**：YOLO自2016年首次发布以来，已经经历了多个版本的迭代（如YOLOv5、YOLOv6、YOLOv7等），每个版本都在性能和效率上有所提升。

1. **社区支持**：YOLO拥有一个活跃的开发者社区，提供了大量的教程、工具和预训练模型，这使得小白也可以快速上手并应用YOLO。

1. **灵活性**：YOLO的架构允许用户根据特定需求调整网络结构，例如改变输入图像的尺寸或调整网络的深度。

我们回归赛题，赛题最终目标是开发一套智能识别系统，能够自动检测和分类城市管理中的违规行为。该系统应利用先进的图像处理和计算机视觉技术，通过对摄像头捕获的视频进行分析，自动准确识别违规行为，并及时向管理部门发出告警，以实现更高效的城市管理。

![](img/37eaaacaed10ebc0aef04e04c5da9d4c.png)

赛事数据集截图（一） 垃圾桶满溢

![](img/8b10fef5da82457922d7a3a7ba075025.png)

赛事数据集截图（二） 机动车违停

仔细分析，赛题的本质是其实就是通过选择合适的目标检测模型进行训练，并使用训练后的模型对城市内违规行为进行检测。由于实际应用场景中，需要模型对行为进行快速分析，因此我们抛弃了二阶段目标检测模型（Faster R-CNN、R-CNN等）选择了一阶段目标检测模型（YOLO、SSD等），同时我们不能一味的追求速度而忽略精度以及易于实践的实际需求，最终我们在众多一阶段目标检测模型中选择了YOLO模型。

---

## Baseline 进阶思路

如果想要在原有代码上提高模型思路，可以有如下操作：

1、增加训练的数据集，可以增加更多的视频到训练集中进行训练。

![](img/1c5e2f7eebfb1399bb35a5972cf6a155.png)

2、切换不同的模型预训练权重。

![](img/279a0e63b96457d4d041722cbd68524f.png)

需要在原始 notebook 中修改，下载预训练模型部分，以及代码中加载模型部分。

```
原始为
!wget http://mirror.coggle.club/yolo/yolov8n-v8.2.0.pt -O yolov8n.pt

下面任意一个
!wget http://mirror.coggle.club/yolo/yolov8s-v8.2.0.pt -O yolov8s.pt
!wget http://mirror.coggle.club/yolo/yolov8m-v8.2.0.pt -O yolov8m.pt
!wget http://mirror.coggle.club/yolo/yolov8l-v8.2.0.pt -O yolov8l.pt
!wget http://mirror.coggle.club/yolo/yolov8x-v8.2.0.pt -O yolov8x.pt
```

![](img/c13eccfd28028829714cd1cf80d2226f.png)

## 参考资料

https://docs.ultralytics.com/datasets/detect/#ultralytics-yolo-format

https://docs.ultralytics.com/guides/yolo-performance-metrics/

https://docs.ultralytics.com/models/yolov8/#performance-metrics

## 来迎接更多挑战吧~

> 学有余力还可以往前学习哟~

<https://datawhaler.feishu.cn/sync/MkNxdOIq2sO0HKbv1A4cZr7JnSh>

> 本教程主要由  Datawhale的小伙伴们 写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用 署名—非商业性使用—相同方式共享 4.0 协议国际版许可协议进行许可。
