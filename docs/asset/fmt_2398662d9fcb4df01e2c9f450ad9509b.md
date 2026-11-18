
# 从零入门 AI for Science（AI+气象）

> 来源：[https://datawhaler.feishu.cn/docx/R9nZdwOwzoAkhix4MAscGa6knjh](https://datawhaler.feishu.cn/docx/R9nZdwOwzoAkhix4MAscGa6knjh)

> 从零入门 AI for Science（AI+气象） 是 Datawhale 2024 年 AI 夏令营第三期的学习活动（“深度学习（极端降水预测）”方向），基于上海科学智能研究院和天池的“第二届世界科学智能大赛地球科学赛道：AI极端降水预报”开展的实践学习——

+   适合想入门并实践深度学习、用AI解决时序问题、气象预测的学习者参与

> 学习内容提要：从深度学习RNN方案逐步进阶，教程会偏重介绍`torch`和数据处理、特征工程和模型搭建思路，最后会简单介绍一些往期赛事的方案案例(选学)

> 📢重点提示：

> 希望选手们由`baseline`中获取解题灵感，争取更好的成绩；如果直接使用`baseline`提交，后续审核出代码雷同情况，将被取消成绩与晋级资格。

# 小白零基础 10 分钟 速通指南

## Step1：报名赛事！（点击即可跳转）

> 赛事链接：`http://competition.sais.com.cn/competitionDetail/532234/competitionData`

> 按照官网指示报名比赛即可

![](img/ae24c8ce618c5b9bc989738409f4fc7e.png)

![](img/e72005d2c32f079c8b16011dfded5b41.png)

<https://datawhaler.feishu.cn/sync/E8g9dSVhBshgxUbmbRHc4c8BnKd>

## Step2：启动魔搭Notebook！（点击即可跳转）

> 链接：`https://www.modelscope.cn/my/mynotebook/preset`

> 选择方式二 GPU环境，启动！

![](img/4c41c60b46ee49d0fecf828b0d8a1f10.png)

![](img/86ef77c9e7c608f3c3f45806759b09f7.png)

<https://datawhaler.feishu.cn/sync/H03Edp1Ozs86WsbAivYcxgRlnLe>

## Step3：10分钟一键跑通`baseline`！

1.  下载`baseline`文件（大约需要2分钟）

> 在终端输入如下命令，回车运行！

```
git lfs install
git clone https://www.modelscope.cn/datasets/Datawhale/AICamp_earth_baseline.git
```

![](img/1d00ef0ac20d689394b9cd9dd469ad97.png)

![](img/6cf060a355f3287a30c1235c53dd0a60.png)

1.  打开文件，一键运行！（大约需要20分钟）

![](img/1b16244235605525ad2bc2b9bbca6901.png)

![](img/217a8581b7ba58252c18003dbb63b7ba.png)

1.  等待运行完成，下载`output.zip`文件！

![](img/3fe41133b4d14754843b9c7c169255ee.png)

![](img/f969356fff9a8d03dab65b37a35735a4.png)

## Step4：提交文件，拿下第一个分数！（点击即可跳转官网进行提交）

> 链接：`http://competition.sais.com.cn/competitionDetail/532234/format`

> 将`output.zip`文件上传即可！

![](img/74100818fba8caf795d73fb343fd0ef7.png)

![](img/452b55029bc8585a9381115c421cddf2.png)

<https://datawhaler.feishu.cn/sync/V52pduThssIeu3bDFk0cvvIvnDh>

> 评分大概需要 2 分钟～评分结束后可以在【我的成绩】处看到评分结果哟！

![](img/44e74e08f0a4fa93a91cde25c7ac7cac.png)

# 🎉 恭喜你拿下了属于自己的第一个分数！

## 👍🏻 快来给自己点赞！

## 🛎️ 别忘记打卡&在群里接龙！

> PS：没有报名AI夏令营第三期的话是无法看到打卡界面的哟~

> 可点击 `https://linklearner.com/activity/12?inviteId=272` 进行报名

![](img/0b964067b1ea31003c95b6ab4beadcfc.png)

## 📅 然后迎接后续的挑战吧！

> 教程贡献者说：

> 恭喜大家成功提交了第一个比赛结果，想对上分有更多了解吗，那就快来参与我们接下来的学习活动吧！

> 接下来我们会基于这个赛题进行更多的知识点学习，会涉及到：

1.  `torch`的基础使用

1.  深度学习的相关知识

1.  特征工程

1.  时间序列任务分析

1.  上分的小技巧

> 更多详情见如下学习规划表：

| 从零入门 AI for Science(AI+气象) |  |  |  |

| --- | --- | --- | --- |

| Task | 学习任务 | 学习内容 | 学习安排 |

| Task 1 |  | 1. 机器学习概念入门 | 开营直播 |

| Task 2 |  | 1. 赛题深入解析 | `baseline`精读直播 |

| Task 3 |  | 1. 数据处理进阶 | 上分思路交流会 |

| Task 4 |  | 1. 以往赛题案例 |  |

|  |  |  |  |

> 打怪难度：简单

1.  跑通`baseline`，拿下第一个分数！

1.  写笔记，完成`Task1`打卡

> 打怪难度：⭐️

1.  精读`baseline`和领域背景知识

1.  了解伏羲气象大模型

1.  写笔记，完成`Task2`打卡

> 打怪难度：⭐️⭐️

1.  精读`baseline2`

1.  参与上分思路交流会

1.  写笔记，完成`Task3`打卡

> 打怪难度：⭐️⭐️

1.  研究以往赛题，进行相关分析

1.  此部分为选学，没有打卡任务

### 祝大家上分顺利，学习愉快~ 多在群里提问和交流哟！

+   赛事链接：[第二届世界科学智能大赛 地球科学赛道：AI极端降水预报](http://competition.sais.com.cn/competitionDetail/532234/format)（点击跳转赛事官网）

+   提问前建议先查看[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/CGrEwDgKHinjmPkEBK3c5mxpnEg)是否早已解决并收录你的问题~（QA文档可任意编辑，助教和教程贡献者会不定时查看）

+   更多学习活动问题可见：[AI+气象学习者手册](https://datawhaler.feishu.cn/wiki/FK8cwI6TjiSZOIkzWcdcWIuEnhs)

# 🍰 下面正式来了解一下赛题！

## 你知道刚刚发生了什么吗？

> 刚刚我们跑通了`baseline`，想必你一定很好奇这背后发生了什么吧？

> 在学习的过程中，不单单要知道如何做，更要知道为什么要这样做。这样我们在遇到同类型的问题时，才能举一反三。

> 一般情况下，拿到一个赛题之后，我们需要明确：

> 1）这是一个什么场景下的赛题；

> 2）这个赛题要解决什么问题。

> 深度思考这些问题，既能帮助我们更好地解决相关问题，更能让我们在后续遇到相关问题时，更好地完成迁移。

## 本次学习活动的赛题“AI极端降水预报挑战赛”是一个典型的时间序列预测问题

> 时间序列预测问题是指涉及使用历史数据来预测未来值。这种问题在许多领域都有应用，如金融市场预测、销售预测、天气预报等。此类问题是典型的有监督的回归问题。解决此类问题, 我们要善于发现其中内在数据的相关性，尤其是在时间、周期上呈现出的特征，加以特征工程为我们所用。

## 来了解一下`baseline`代码的大致结构

> 看看我们是如何构建的`baseline`~

1.  **导入库**：首先，代码导入了需要用到的库，包括 `torch` 和处理气象数据必要的 `xarray`，用于处理结构数据的 `pandas` 和常用操作系统库 `os`

1.  **数据集构建**：代码通过使用 `Feature` 类和 `GT` 类定义了从气象`.nc`文件中读取数据，同时通过 `Dataset` 类将训练数据和对应标签建立起对应关系，最后使用 `torch.utils.data` 中的 `DataLoader` 定义数据加载工具，方便我们在训练过程中获取数据。

1.  **定义模型和使用的损失函数**：

- 定义了只含有一层卷积的简单网络，使用 `MSE` 作为损失函数，特别注意模型的输入输出要根据赛题要求设计。

1.  **模型训练**：

- 完成优化器和训练周期的定义后，我们就可以开始训练模型以便在数据上得到一个拟合程度最好的训练模型，模型训练时不要忘了保存模型参数文件`.pth`

1.  **加载训练好的模型进行预测输出**：

- 加载第4步中的训练参数以后，需要用 `model.eval()` 将模型置于推理模式，然后我们就有了一个拟合程度相对不错的降水预测模型，再把测试数据输入，就可以生成预测结果~

伪代码在这里：

```
# 1. 导入需要用到的相关库
import os
import torch
import pandas as pd
import xarray as xr
import torch.nn as nn
from torch.utils.data import Dataset, DataLoader

# 2. 定义数据集
path = "" # 配置路径的设置..
class Feature:
    pass
class GT:
    pass
class mydataset(Dataset):
    def __init__(self):
        ...
    def __getitem__(self, index):
        # 获取训练数据的方法， 同时将训练数据和真值建立联系
        ...
    def __len__(self):
        #获取数据集长度的方法
        ...
my_data = mydataset() # 初始化dataset
train_loader = DataLoader(my_data, batch_size=1, shuffle=True) # 定义dataloader         
 # 3. 定义模型和损失函数
 class Model(nn.Module):
     # 模型初始化
     def __init__(self, *args):
         ...
     # 定义前向传播函数
     def forward(self, *args):
         ...
         
 # 模型具体输入输出定义
 input_chnl = ...
 # loss定义
 loss = nn.MSELoss()
 
 # 4. 模型训练
num_epochs = 1 # 定义模型训练轮数
optimizer = torch.optim.Adam(model.parameters(), lr=0.001) # 定义优化器
# 从dataloader中取数据训练
for epoch in range(num_epochs):
    for index, (ft_item, gt_item) in enumerate(train_loader):
        ...

torch.save(model.state_dict(), "xxx.pth") # 保存模型参数

 # 5. 模型推理
model.load_state_dict(torch.load('model_weights.pth')) # 加载模型
model.eval() # 将模型置于推理状态
test_data_path = "xxx"
# 模型推理
for index, test_data_file in enumerate(os.listdir(test_data_path)):
    ...
 

```

## 接下来，大家可以思考，以上代码为何能用来完成赛事要求？

> 有任何想法都可以在学习群里交流哟~

> 如果大家等不及想提分的话，还可以直接开始的学习哟~

## 关于PyTorch库和深度学习

### Pandas学习推荐

> Datawhale有一个开源教程项目叫 深入浅出`PyTorch`，链接如下

> 在线内容链接：`https://datawhalechina.github.io/thorough-pytorch/`

> （如果大家想仔细研读的话，可以在学习过程中，依照其中的内容进行实践；还可以多多在此处留言，也许夏令营后就会展开组队学习哟~）

### 深度学习学习材料推荐

> Datawhale也有一个针对深度学习的开源导学项目叫 水很深的深度学习

> 开源项目地址: `https://github.com/datawhalechina/unusual-deep-learning`

> 大家在本次学习过程中，可以一边将上面推荐的材料作为字典，遇到问题时通过这些材料、大模型、搜索引擎，去翻阅查看，做到理论和实践的结合，真正理解这些理论知识的价值，以及究竟要如何在真实的问题场景中使用。

![](img/a0a5cc4dfa2e8b2700c6cc391f59e190.png)

## 进入到[Task1 知识点文档](https://datawhaler.feishu.cn/wiki/OdXjwwUNYifP76ktaBLcPYPonJc) 学习更多吧！

## 更多QA详见[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/CGrEwDgKHinjmPkEBK3c5mxpnEg)！

> [ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/CGrEwDgKHinjmPkEBK3c5mxpnEg)任何想法都可以在学习群里交流或在进行填写哟~

> 别忘了打开[Task1：赛事初探——降水预测模型搭建过程概述](https://datawhaler.feishu.cn/wiki/OdXjwwUNYifP76ktaBLcPYPonJc)，了解赛题！

> [Task2知识点文档](https://datawhaler.feishu.cn/wiki/AloVwLxJ3ipNg2kWsKTcyGapnNe)如果大家等不及想提分的话，还可以直接开始的学习！

>

> 本教程主要由  、  写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

> 注：本文采用 署名—非商业性使用—相同方式共享 4.0 协议国际版许可协议进行许可。
