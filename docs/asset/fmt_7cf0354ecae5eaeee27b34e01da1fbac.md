
# Task2：入门`lightgbm`，开始特征工程

> 来源：[https://datawhaler.feishu.cn/docx/JCfpdZHL7oIxO4xPFYcc8wVGnch](https://datawhaler.feishu.cn/docx/JCfpdZHL7oIxO4xPFYcc8wVGnch)

> **教程贡献者说：**

> 前面我们在[ 从零入门机器学习竞赛](https://datawhaler.feishu.cn/wiki/CuhBw9vBaiG1nJklIPkcRhqVnmk)**（即`Task1`）**中介绍了赛题，并给出基于经验模型的`baseline`完成了预测任务。

> 本`Task`中，我们的任务是使用进阶的**机器学习模型**`lightgbm`解决本次问题，以达到更好的预测效果。

> 你可以从中学会

+   **使用数据集绘制柱状图和折线图**，
+   使用时间序列数据构建历史平移特征和窗口统计特征，
+   使用`lightgbm`模型进行训练并预测。

> 特征工程是参与机器学习竞赛的重要环节，可以通过**观察数据**并**结合专业背景知识**改善特征或者构建新的**特征**。

> 机器学习中的一个经典理论是：**数**据和特征决定了机器学习的上限，而模型和算法只是逼近这个上限。

## 进阶思路Q&A

### 解决该问题常见的几种思路是什么？

本次问题是回归预测，能够取得不错效果的话，常规思路一般为使用机器学习模型，如`LightGBM`、`XGBoost`，或者使用深度学习模型（神经网络等）进行实践，在模型的搭建上就比较复杂，需要自己构建模型结构，对于数值数据需要进行标准化处理；

### Task2版本的教程使用什么思路？

`task1` 的`baseline`我们是基于经验模型（使用均值作为结果数据）来解决的问题

Task2 版本教程将使用机器学习模型解决本次问题，模型使用简单，数据不需要过多预处理；

**使用机器学习方法**一般主要需要从**获取数据&增强**、**特征提取**和**模型**三个方面下手。

### 使用机器学习方法有哪几个步骤？

一般的使用机器学习模型解决问题的主要步骤为探索性数据分析、数据预处理、提取特征、切分训练集与验证集、训练模型、预测结果。

## 基础概念入门

### GBDT

`GBDT`（`Gradient Boosting Decision Tree`）是机器学习中一个长盛不衰的模型，其主要思想是利用弱分类器（决策树）迭代训练以得到最优模型，该模型具有训练效果好、不易过拟合等优点。

`GBDT`不仅在工业界应用广泛，通常被用于多分类、点击率预测、搜索排序等任务；在各种数据挖掘竞赛中也是致命武器，据统计Kaggle上的比赛有一半以上的冠军方案都是基于`GBDT`。

### LightGBM

`LightGBM`（`Light Gradient Boosting Machine`）是一个实现`GBDT`算法的框架，支持高效率的并行训练，并且具有更快的训练速度、更低的内存消耗、更好的准确率、支持分布式可以快速处理海量数据等优点。

`LightGBM` 框架中还包括随机森林和逻辑回归等模型。通常应用于二分类、多分类和排序等场景。

例如：在个性化商品推荐场景中，通常需要做点击预估模型。使用用户过往的行为（点击、曝光未点击、购买等）作为训练数据，来预测用户点击或购买的概率。根据用户行为和用户属性提取一些特征，包括：

+   类别特征（`Categorical Feature`）：字符串类型，如性别（男/女）。
+   物品类型：服饰、玩具和电子等。
+   数值特征（`Numrical Feature`）：整型或浮点型，如用户活跃度或商品价格等。

更多内容可见 [`LightGBM 中文文档`](https://lightgbm.cn/)、[`LightGBM`英文文档](https://lightgbm.readthedocs.io/en/latest/index.html)

## 进阶代码详解

完整代码如下：

### （1）导入模块

此部分包含代码所需的模块

```python
import numpy as np
import pandas as pd
import lightgbm as lgb
from sklearn.metrics import mean_squared_log_error, mean_absolute_error, mean_squared_error
import tqdm
import sys
import os
import gc
import argparse
import warnings
warnings.filterwarnings('ignore')
```

### （2）探索性数据分析（EDA）

在数据准备阶段，主要读取训练数据和测试数据，并进行基本的数据展示。

```python
train = pd.read_csv('./data/train.csv')
test = pd.read_csv('./data/test.csv')
```

![](img/dceffaba4cfc3aea842c10d05b79adb0.png)

**数据简单介绍：**

+   其中`id`为房屋`id`，
+   `dt`为日标识，训练数据`dt`最小为11，不同`id`对应序列长度不同；
+   `type`为房屋类型，通常而言不同类型的房屋整体消耗存在比较大的差异；
+   `target`为实际电力消耗，也是我们的本次比赛的预测目标。

下面进行简单的可视化分析，帮助我们对数据有个简单的了解。

+   不同`type`类型对应`target`的柱状图

```python
import matplotlib.pyplot as plt
# 不同type类型对应target的柱状图
type_target_df = train.groupby('type')['target'].mean().reset_index()
plt.figure(figsize=(8, 4))
plt.bar(type_target_df['type'], type_target_df['target'], color=['blue', 'green'])
plt.xlabel('Type')
plt.ylabel('Average Target Value')
plt.title('Bar Chart of Target by Type')
plt.show()
```

![](img/bdf417ed4206438e343fb9d77ee3fd5f.png)

+   `id`为`00037f39cf`的按`dt`为序列关于`target`的折线图

```python
specific_id_df = train[train['id'] == '00037f39cf']
plt.figure(figsize=(10, 5))
plt.plot(specific_id_df['dt'], specific_id_df['target'], marker='o', linestyle='-')
plt.xlabel('DateTime')
plt.ylabel('Target Value')
plt.title("Line Chart of Target for ID '00037f39cf'")
plt.show()
```

![](img/a2443084466b5964f5f06c2ba8a2d6ff.png)

### （3）特征工程

这里主要构建了 **历史平移特征** 和 **窗口统计特征**；每种特征都是有理可据的，具体说明如下：

+   **历史平移特征：**通过历史平移获取上个阶段的信息；如下图所示，可以将`d-1`时间的信息给到`d`时间，`d`时间信息给到`d+1`时间，这样就实现了平移一个单位的特征构建。

![](img/8f2768628f280e3e3cd5428702126775.png)

+   **窗口统计特征：**窗口统计可以构建不同的窗口大小，然后基于窗口范围进统计均值、最大值、最小值、中位数、方差的信息，可以反映最近阶段数据的变化情况。如下图所示，可以将`d`时刻之前的三个时间单位的信息进行统计构建特征给`d`时刻。

![](img/b9f70008b87ee0ed336d2ee450be1d49.png)

**完整代码如下：**

```python
# 合并训练数据和测试数据，并进行排序
data = pd.concat([test, train], axis=0, ignore_index=True)
data = data.sort_values(['id', 'dt'], ascending=False).reset_index(drop=True)

# 历史平移
for i in range(10, 30):
    data[f'last{i}_target'] = data.groupby(['id'])['target'].shift(i)
    
# 窗口统计
data[f'win3_mean_target'] = (data['last10_target'] + data['last11_target'] + data['last12_target']) / 3

# 进行数据切分
train = data[data.target.notnull()].reset_index(drop=True)
test = data[data.target.isnull()].reset_index(drop=True)

# 确定输入特征
train_cols = [f for f in data.columns if f not in ['id', 'target']]
```

### （4）模型训练与测试集预测

这里选择使用`LightGBM`模型，也是通常作为数据挖掘比赛的基线模型，在不需要过程调参的情况的也能得到比较稳定的分数。

另外需要注意的训练集和验证集的构建：因为数据存在时序关系，所以需要严格按照时序进行切分，

+   这里选择原始给出训练数据集中`dt`为30之后的数据作为训练数据，**之前的数据作为验证数据**，
+   这样保证了数据不存在穿越问题（不使用未来数据预测历史数据）。

```python
def time_model(lgb, train_df, test_df, cols):
    # 训练集和验证集切分
    trn_x, trn_y = train_df[train_df.dt >= 31][cols], train_df[train_df.dt >= 31]['target']
    val_x, val_y = train_df[train_df.dt <= 30][cols], train_df[train_df.dt <= 30]['target']
    # 构建模型输入数据
    train_matrix = lgb.Dataset(trn_x, label=trn_y)
    valid_matrix = lgb.Dataset(val_x, label=val_y)
    # lightgbm参数
    lgb_params = {
        'boosting_type': 'gbdt',
        'objective': 'regression',
        'metric': 'mse',
        'min_child_weight': 5,
        'num_leaves': 2 ** 5,
        'lambda_l2': 10,
        'feature_fraction': 0.8,
        'bagging_fraction': 0.8,
        'bagging_freq': 4,
        'learning_rate': 0.05,
        'seed': 2024,
        'nthread': 16,
        'verbose': -1,
    }
    # 训练模型
    model = lgb.train(lgb_params, train_matrix, 50000, valid_sets=[train_matrix, valid_matrix], 
                      categorical_feature=[], verbose_eval=500, early_stopping_rounds=500)
    # 验证集和测试集结果预测
    val_pred = model.predict(val_x, num_iteration=model.best_iteration)
    test_pred = model.predict(test_df[cols], num_iteration=model.best_iteration)
    # 离线分数评估
    score = mean_squared_error(val_pred, val_y)
    print(score)
       
    return val_pred, test_pred
    
lgb_oof, lgb_test = time_model(lgb, train, test, train_cols)

# 保存结果文件到本地
test['target'] = lgb_test
test[['id', 'dt', 'target']].to_csv('submit.csv', index=None)
```

![](img/b8797191c3c5cf3dbd68ef14fc345cf8.png)

![](img/5929e29b016b21269828bea7dfb531fd.png)

## 复盘总结

在目前阶段，我们使用`lightgbm`完成了基本的模型训练，并且添加了时序问题中常见的特征提取方式，

通过**特征工程**挖掘特征可以很快的提升模型预测效果，这也是数据挖掘比赛中的主要优化方向，很多情况下会决定最终的成绩。

总之，就是在现有方案的基础上**不断的改造与尝试**，通过不断的实践来提升自己的数据挖掘能力，

正所谓【**纸上得来终觉浅，绝知此事要躬行**】，也许你熟练掌握机器学习的相关算法，能熟练推导各种公式，

但如何将学习到的方法应用到实践工程中，需要我们不断的尝试与改进，

没有一个模型是一步所得，持续努力、向最后的冠军冲击~

## 参考资料

1.  [下一站，向冠军冲击！](https://mp.weixin.qq.com/s/d7dXGYnF4NZuuazktK4SXQ)

1.  [我的机器学习之路](https://mp.weixin.qq.com/s/2-V1kFbSzi3Z5UJ7GV_WBw)

1.  [我的机器学习入门清单及路线！](https://mp.weixin.qq.com/s/KeD9kPG8PowlKrz69zSHNQ)

1.  机器学习神器Scikit-Learn保姆教程！

1.  《Datawhale人工智能培养方案》发布！

1.  [`《机器学习算法竞赛实战》书籍！`](https://book.douban.com/subject/35595757/)

1.  [时间序列预测方法总结](https://zhuanlan.zhihu.com/p/67832773)

1.  [`《数据竞赛入门讲义》.pdf`](https://datawhaler.feishu.cn/file/IXdDbOBgIoZbtvxLukqc2432nQd)

1.  双节棍「大师」鱼佬亲传武功秘籍：如何进行一场数据挖掘算法竞赛？

学习了这些武林秘籍后，你可以充分发挥自己的想象力和创新力，思考各种可能的方案，尝试、分享、互相进阶，有任何想法或新发现都可以在群里一起分享哟~

## 学习规划表

> 学有余力还可以提前看看后续的内容哟~

> 迫不及待的话，快来瞧瞧[`Task3`：尝试使用深度学习方案](https://datawhaler.feishu.cn/wiki/G8WywyV8iiYtuGkVjoOchdjxnib)

<https://datawhaler.feishu.cn/sync/KeQvdtCR6sr0nIb8UTRc1dmfneb>

> 本教程主要由 [**@鱼遇雨欲语与余**](https://www.zhihu.com/people/wang-he-13-93) 、 写作贡献，融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~
