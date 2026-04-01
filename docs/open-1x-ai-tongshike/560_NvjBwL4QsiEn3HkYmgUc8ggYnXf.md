# AI+生命科学：年龄评价与相关疾病风险预测

> 来源：[https://datawhaler.feishu.cn/docx/Sgc4dmz6yoOnyTxZQgvclU10nsg](https://datawhaler.feishu.cn/docx/Sgc4dmz6yoOnyTxZQgvclU10nsg)

| 学习任务 |
| --- |
| **任务一：** |
| **任务二：** |
| **任务三：** |

1.  阅读[特征优化章节](https://datawhaler.feishu.cn/docx/Sgc4dmz6yoOnyTxZQgvclU10nsg#ZglPdiyDSoD902xjUS8c3Fgqneh)，尝试对比实验

1.  阅读[模型融合章节](https://datawhaler.feishu.cn/docx/Sgc4dmz6yoOnyTxZQgvclU10nsg#XtW6dDE0ioUl7pxYVKfcCWBXnHb)，尝试对比实验

# **首届世界科学智能大赛 - **AI + 生命科学

> **首届世界科学智能大赛：生命科学赛道—**—生物学年龄评价与年龄相关疾病风险预测：

> https://tianchi.aliyun.com/s/6a1351ecd2a3987995a7bda7f62542d2

> **举办方：**复旦大学，阿里云，中国电信，中国信通院

## **赛题背景**

生物学年龄评价是一种通过测量和分析生物体内特定指标或生理过程的状态，以评估个体的生理年龄和健康状况的方法。与传统的日历年龄相比，生物学年龄可以提供更准确的健康评估和疾病风险预测。随着 AI 技术的不断发展，计算科学与生命科学的整合与发展将为健康管理应用开发、衰老机制研究、抗衰老药物研发提供前沿的思路与方法。基于上述背景，我们举办基于甲基化测量数据预测生物学年龄的比赛。

这项比赛将结合计算科学和生命科学的发展，为健康管理应用的开发、衰老机制的研究以及抗衰老药物的研发提供前沿的思路和方法。传统的日历年龄只提供了时间上的度量，而生物学年龄则更加关注个体的生理状态和功能。通过测量和分析生物体内的特定指标或生理过程的状态，我们能够更准确地评估个体的生理年龄和健康状况，这对于健康管理和疾病风险预测至关重要。参赛者可以利用甲基化测量数据来预测生物学年龄。本次比赛将为研究人员和开发人员提供机会，合作解决生物学年龄评估中的挑战，并推动相关领域的进步。最终的目标是提供更精确、可靠和个性化的生物学年龄评估工具，为人们的健康管理和抗衰老策略提供有力支持。

## **赛事任务**

本次比赛提供健康人和年龄相关疾病患者的甲基化数据，期待选手通过分析甲基化数据的模式和特征建立预测模型，可以根据某个人的甲基化数据来预测其生物学年龄。

## **赛题数据集**

公开数据包含10296个样本，其中7833个样本为健康样本。每一个样本提供485512个位点的甲基化数据、年龄与患病情况。抽取80%作为训练样本，20%作为测试样本。

以训练集为例，一共包括8233样本，其中健康样本6266个，其余为患病样本，共涉及Alzheimer's disease，schizophrenia，Parkinson's disease，rheumatoid arthritis，stroke，Huntington's disease，Graves' disease，type 2 diabetes，Sjogren's syndrome等类型。

## **评价指标**

本次任务采用多个指标来进行评测，初赛和复赛评价指标有差异，作为本次教程仅介绍初赛评价指标。初赛是将两个指标（健康样本MAE和患病样本MAE）进行计算，取平均得到最终分数。

![](img/17b48919ae43a820f1952551b0f95388.png)

## 解题思路

本题初赛任务是预测样本年龄，属于典型的回归问题，输入数据为每一个样本对应的485512个位点的甲基化数据与患病情况，输出为样本对应年龄。

可以看到所提供的特征纬度非常高（485512维），可以考虑基本的特征选择，比如考虑覆盖率、相关性、特征重要性等。或者仅使用部分特征，快速跑通流程，后续在考虑如何添加更多特征，也是一个不错的选择。

模型方面可以选择机器学习模型或深度学习模型，如果使用类似xgboost这类机器学习模型，是不需要进行缺失值填充和数据标准化操作的，效果比较稳定。如果选择深度学习模型，则要进行缺失值填充和数据标准化操作，网络搭建也需要更多的尝试。

鉴于上面的对比，我们Baseline选择使用机器学习方法，在解决机器学习问题时，一般会遵循以下流程：

# Baseline实践

刚刚我们完成了基本的问题分析，接下来会带领大家跑通数据探索、数据清洗、特征工程、模型训练、模型验证、结果输出的全部竞赛实践流程。

## 任务1：云环境配置与数据下载

介于本赛事的数据规模比较庞大，推荐大家使用赛事官方推荐的阿里云的云环境，提供免费V100与A10算力，可以快速跑通全流程

[阿里云环境快速跑通](https://datawhaler.feishu.cn/docx/GIr5dWijEoGWRJxzSeCcZFmgnAe?from=from_copylink)

> **D****S**W环境配置与Baseline视频讲解， 助教贡献

![](img/a76882fb31519da33448ce18ff37c3b7.png)

[3d7d6bc86e39716424afb1964f2e2cbe.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/D5HgbRVoyouDwPxaWqDcahlznyh)

> **以下代码**全部更新，补充了代码逐行解析@刘志文 助教贡献

https://blog.csdn.net/weixin_51038524/article/details/132331242

## 任务2：精读Baseline

## **导入模块**

导入我们本次Baseline代码所需的模块

```
# 导入numpy库，用于进行数值计算
import numpy as np
# 导入pandas库，用于数据处理和分析
import pandas as pd
# 导入polars库，用于处理大规模数据集
import polars as pl
# 导入collections库中的defaultdict和Counter，用于统计
from collections import defaultdict, Counter
# 导入xgboost库，用于梯度提升树模型
import xgb
# 导入lightgbm库，用于梯度提升树模型
import lgb
# 导入CatBoostRegressor库，用于梯度提升树模型
from catboost import CatBoostRegressor
# 导入StratifiedKFold、KFold和GroupKFold，用于交叉验证
from sklearn.model_selection import StratifiedKFold, KFold, GroupKFold
# 导入mean_squared_log_error，用于评估模型性能
from sklearn.metrics import mean_squared_log_error
# 导入sys、os、gc、argparse和warnings库，用于处理命令行参数和警告信息
import sys, os, gc, argparse, warnings
# 均绝对误差）是一个用于回归问题评估模型性能的指标之一，它衡量了预测值与实际观测值之间的平均绝对差异。
from sklearn.metrics import mean_absolute_error
# 忽略警告信息
warnings.filterwarnings(&#x27;ignore&#x27;)

```

## **数据探索**

数据探索性分析，是通过了解数据集，了解变量间的相互关系以及变量与预测值之间的关系，对已有数据在尽量少的先验假设下通过作图、制表、方程拟合、计算特征量等手段探索数据的结构和规律的一种数据分析方法，从而帮助我们后期更好地进行特征工程和建立模型，是机器学习中十分重要的一步。

加载数据及基本的预处理，需要特别注意：

训练集压缩包**7.22G**，测试集压缩包**2.01G**

解压缩后traindata.csv**56.7G**，testdata.csv压缩包**14.2G**

本地算力不够的同学，推荐使用赛事云端环境PAI-DSW

![](img/d5f5c4fd9690aaecc3e1935355d315f8.png)

1、使用reduce_mem_usage_pl函数对数据进行类型转换，及时减少内存；下面代码中使用压缩函数，内存可以压缩一半；

![](img/ff98c5684ff27b75d246b0742a324ece.png)

2、对数据进行预处理，主要是进行数据表的转置操作，因为原本数据为每一列代表一个样本，需要调整为每一行为一个样本；

3、处理完的数据然后保存为pickle文件，之后使用数据时可以直接加载pickle文件即可，减少重复预处理工作。

```
# 定义一个函数，用于减少DataFrame的内存使用
def reduce_mem_usage(df, verbose=True):
    numerics = [&#x27;int16&#x27;, &#x27;int32&#x27;, &#x27;int64&#x27;, &#x27;float16&#x27;, &#x27;float32&#x27;, &#x27;float64&#x27;] # 定义数值类型列表
    start_mem = df.memory_usage().sum() / 1024**2    # 计算初始内存使用量
    for col in df.columns: # 遍历DataFrame的每一列
        col_type = df[col].dtypes # 获取该列的数据类型
        if col_type in numerics: # 如果该列是数值类型
            c_min = df[col].min() # 获取该列的最小值
            c_max = df[col].max() # 获取该列的最大值
            if str(col_type)[:3] == &#x27;int&#x27;: # 如果该列是整数类型
                if c_min &gt; np.iinfo(np.int8).min and c_max &lt; np.iinfo(np.int8).max: # 如果该列的数值范围在int8的范围内
                    df[col] = df[col].astype(np.int8) # 将该列的数据类型转换为int8
                elif c_min &gt; np.iinfo(np.int16).min and c_max &lt; np.iinfo(np.int16).max: # 如果该列的数值范围在int16的范围内
                    df[col] = df[col].astype(np.int16) # 将该列的数据类型转换为int16
                elif c_min &gt; np.iinfo(np.int32).min and c_max &lt; np.iinfo(np.int32).max: # 如果该列的数值范围在int32的范围内
                    df[col] = df[col].astype(np.int32) # 将该列的数据类型转换为int32
                elif c_min &gt; np.iinfo(np.int64).min and c_max &lt; np.iinfo(np.int64).max: # 如果该列的数值范围在int64的范围内
                    df[col] = df[col].astype(np.int64) # 将该列的数据类型转换为int64  
            else: # 如果该列不是整数类型
                if c_min &gt; np.finfo(np.float16).min and c_max &lt; np.finfo(np.float16).max: # 如果该列的数值范围在float16的范围内
                    df[col] = df[col].astype(np.float16) # 将该列的数据类型转换为float16
                elif c_min &gt; np.finfo(np.float32).min and c_max &lt; np.finfo(np.float32).max: # 如果该列的数值范围在float32的范围内
                    df[col] = df[col].astype(np.float32) # 将该列的数据类型转换为float32
                else: # 如果该列的数值范围不在上述三种类型的范围内
                    df[col] = df[col].astype(np.float64) # 将该列的数据类型转换为float64
    end_mem = df.memory_usage().sum() / 1024**2 # 计算结束时的内存使用量
    if verbose: print(&#x27;Mem. usage decreased to {:5.2f} Mb ({:.1f}% reduction)&#x27;.format(end_mem, 100 * (start_mem - end_mem) / start_mem)) # 输出内存使用量的减少情况
    return df # 返回处理后的DataFrame

# 读取数据
path = &#x27;ai4bio&#x27; # 定义数据集路径
# 可能因为内存问题所导致数据读取困难，可以选择放弃部分特征，仅读取部分行，baseline仅读取前10000行
#根据自己的算力情况，适当读取数据
traindata = pd.read_csv(f&#x27;{path}/traindata.csv&#x27;, nrows=10000) # 读取训练数据
trainmap = pd.read_csv(f&#x27;{path}/trainmap.csv&#x27;) # 读取训练数据的映射信息

testdata = pd.read_csv(f&#x27;{path}/ai4bio_testset_final/testdata.csv&#x27;, nrows=10000) # 读取测试数据
testmap = pd.read_csv(f&#x27;{path}/ai4bio_testset_final/testmap.csv&#x27;) # 读取测试数据的映射信息

# 压缩内存，因时间较长，所以暂时注释掉，按自身情况选择使用
# traindata = reduce_mem_usage(traindata)
# testdata = reduce_mem_usage(testdata)

# 数据预处理
traindata = traindata.set_index(&#x27;cpgsite&#x27;) # 将训练数据的索引设置为&#x27;cpgsite&#x27;列
traindata = traindata.T # 转置训练数据
traindata = traindata.reset_index() # 重置训练数据的索引
traindata = traindata.rename(columns={&#x27;index&#x27;:&#x27;sample_id&#x27;}) # 重命名训练数据的列名
traindata.columns = [&#x27;sample_id&#x27;] + [i for i in range(10000)] # 设置训练数据的列名为&#x27;sample_id&#x27;加上一列自增的数字
traindata.to_pickle(f&#x27;{path}/traindata.pkl&#x27;) # 将处理后的训练数据保存为pickle文件

testdata = testdata.set_index(&#x27;cpgsite&#x27;) # 将测试数据的索引设置为&#x27;cpgsite&#x27;列
testdata = testdata.T # 转置测试数据
testdata = testdata.reset_index() # 重置测试数据的索引
testdata = testdata.rename(columns={&#x27;index&#x27;:&#x27;sample_id&#x27;}) # 重命名测试数据的列名
testdata.columns = [&#x27;sample_id&#x27;] + [i for i in range(10000)] # 设置测试数据的列名为&#x27;sample_id&#x27;加上一列自增的数字
testdata.to_pickle(f&#x27;{path}/testdata.pkl&#x27;) # 将处理后的测试数据保存为pickle文件
```

基本信息展示

```
# 使用 head() 函数查看前几行数据
trainmap.head()
```

![](img/195118eb7c9452a9109415427ed8d8e2.png)

```
# 使用 head() 函数查看前几行数据
traindata.head()
```

![](img/c61620f958c125fe57bdb33d391624c4.png)

```
# info()的方法，用于显示有关数据集的信息，例如列名、数据类型、非空值数量和内存使用情况等。
traindata.info()
```

![](img/54de5f16539e1924a8f7cc5a0d4a25e8.png)

经过上面简单的信息展示，可以发现trainmap中的gender、sample_type、disease为字符串类型的特征，需要进行转换为数值类型。traindata数据已经调整为每行代表一个样本的结构，每列表示一个特征。

下面展示部分特征字段地缺失比例，可以看出部分特征缺失率还是蛮高的。

```
# 使用循环来遍历数据集traindata的前10个特征，并计算每个特征的缺失率。
for i in range(10):
        # 使用traindata[i]获取第i个特征的数据,调用isnull()方法来判断数据是否为空值，返回一个布尔类型的数组,使用sum()方法对布尔数组求和，统计每行中空值的数量,将空值数量除以数据集的总行数，得到该特征的缺失率。
    null_cnt = traindata[i].isnull().sum() / traindata.shape[0] 
    # 使用print()函数输出特征名称和对应的缺失率。
    print(f&#x27;特征{i}，对应的缺失率为{null_cnt}&#x27;)

```

![](img/756916588b46cfb2a86bc231974b2a6b.png)

下面使用DataFrame自带的corr()方法展示前1000个特征之间的相关性分数，大于0为正相关，小于0为负相关。

```
# 代码使用列表推导式生成一个包含前1000个特征的列表，即[i for i in range(1000)]，这个列表作为索引传递给traindata，以获取这些特征对应的列数据，调用corr()方法计算这些列之间的相关系数矩阵。
traindata[[i for i in range(1000)]].corr()

```

![](img/f5519a0f6c6560aedac6553742963af4.png)

## **数据清洗**

数据和特征决定了机器学习的上限，而模型和算法只是逼近这个上限而已。俗话说：garbage in, garbage out。分析完数据后，特征工程前，必不可少的步骤是对数据清洗。

数据清洗的作用是利用有关技术如数理统计、数据挖掘或预定义的清理规则将脏数据转化为满足数据质量要求的数据。主要包括缺失值处理、异常值处理、数据分桶、特征归一化/标准化等流程。

经过数据探索部分的分析，需要进行处理的部分包括缺失值、数据类型转换，其中对于缺失值的处理，鉴于baseline方案使用的是catboost模型，并不需要进行缺失值处理。

下面仅需要进行数据类型的转换操作：

```
# 数据拼接
# 将trainmap数据集中的&#x27;sample_id&#x27;, &#x27;age&#x27;, &#x27;gender&#x27;, &#x27;sample_type&#x27;, &#x27;disease&#x27;这几列数据合并到traindata数据集中，合并的依据是两个数据集中的&#x27;sample_id&#x27;列相同，合并后的结果存储在traindata中。
traindata = traindata.merge(trainmap[[&#x27;sample_id&#x27;, &#x27;age&#x27;, &#x27;gender&#x27;, &#x27;sample_type&#x27;, &#x27;disease&#x27;]],on=&#x27;sample_id&#x27;,how=&#x27;left&#x27;)
# 将testmap数据集中的&#x27;sample_id&#x27;, &#x27;gender&#x27;这两列数据合并到testdata数据集中，合并的依据是两个数据集中的&#x27;sample_id&#x27;列相同，合并后的结果存储在testdata中，由于使用了左连接（how=&#x27;left&#x27;），如果testmap中没有与testdata匹配的&#x27;sample_id&#x27;，那么对应的&#x27;gender&#x27;值将会被填充为NaN。
testdata = testdata.merge(testmap[[&#x27;sample_id&#x27;, &#x27;gender&#x27;]],on=&#x27;sample_id&#x27;,how=&#x27;left&#x27;)

# 定义了一个名为disease_mapping的字典，它将疾病名称映射为对应的数值。例如，&#x27;Alzheimer&#x27;s disease&#x27;被映射为1，&#x27;Parkinson&#x27;s disease&#x27;被映射为4，以此类推。这样的映射通常用于机器学习模型中的特征编码，以便将文本形式的类别标签转换为可以输入到模型的数字形式。
disease_mapping = {
    &#x27;control&#x27;: 0,
    &quot;Alzheimer&#x27;s disease&quot;: 1,
    &quot;Graves&#x27; disease&quot;: 2,
    &quot;Huntington&#x27;s disease&quot;: 3,
    &quot;Parkinson&#x27;s disease&quot;: 4,
    &#x27;rheumatoid arthritis&#x27;: 5,
    &#x27;schizophrenia&#x27;: 6,
    &quot;Sjogren&#x27;s syndrome&quot;: 7,
    &#x27;stroke&#x27;: 8,
    &#x27;type 2 diabetes&#x27;: 9
}
sample_type_mapping = {&#x27;control&#x27;: 0, &#x27;disease tissue&#x27;: 1}
gender_mapping = {&#x27;F&#x27;: 0, &#x27;M&#x27;: 1}

# 将traindata数据集中&#x27;disease&#x27;列中的疾病名称使用disease_mapping字典进行映射，将疾病名称替换为对应的数值。这样处理后，&#x27;disease&#x27;列中的值将变为0到9之间的整数。
traindata[&#x27;disease&#x27;] = traindata[&#x27;disease&#x27;].map(disease_mapping)
# 将traindata数据集中&#x27;sample_type&#x27;列中的样本类型使用sample_type_mapping字典进行映射，将样本类型替换为对应的数值，处理后，&#x27;sample_type&#x27;列中的值将变为0或1，表示不同的样本类型。
traindata[&#x27;sample_type&#x27;] = traindata[&#x27;sample_type&#x27;].map(sample_type_mapping)
# 将traindata和testdata数据集中&#x27;gender&#x27;列中的性别使用gender_mapping字典进行映射，将性别替换为对应的数值。这样处理后，&#x27;gender&#x27;列中的值将变为0或1，表示不同的性别。
traindata[&#x27;gender&#x27;] = traindata[&#x27;gender&#x27;].map(gender_mapping)
testdata[&#x27;gender&#x27;] = testdata[&#x27;gender&#x27;].map(gender_mapping)

```

## **特征工程**

特征工程指的是把原始数据转变为模型训练数据的过程，目的是获取更好的训练数据特征。特征工程能使得模型的性能得到提升，有时甚至在简单的模型上也能取得不错的效果。

原始字段特征以连续型数值为主，可以按行进行聚合统计操作，如构建max、min、mean、std、skew等常见的数值统计特征。

```
# 特征工程
# 计算traindata和testdata数据集中前10000行每一列的最大值、最小值、标准差、方差、偏度、均值和中位数，并将结果分别存储在traindata和testdata的&#x27;max&#x27;、&#x27;min&#x27;、&#x27;std&#x27;、&#x27;var&#x27;、&#x27;skew&#x27;、&#x27;mean&#x27;和&#x27;median&#x27;列中，这些统计量可以用于描述数据集的特征和分布情况。
traindata[&#x27;max&#x27;] = traindata[[i for i in range(10000)]].max(axis=1)
traindata[&#x27;min&#x27;] = traindata[[i for i in range(10000)]].min(axis=1)
traindata[&#x27;std&#x27;] = traindata[[i for i in range(10000)]].std(axis=1)
traindata[&#x27;var&#x27;] = traindata[[i for i in range(10000)]].var(axis=1)
traindata[&#x27;skew&#x27;] = traindata[[i for i in range(10000)]].skew(axis=1)
traindata[&#x27;mean&#x27;] = traindata[[i for i in range(10000)]].mean(axis=1)
traindata[&#x27;median&#x27;] = traindata[[i for i in range(10000)]].median(axis=1)

testdata[&#x27;max&#x27;] = testdata[[i for i in range(10000)]].max(axis=1)
testdata[&#x27;min&#x27;] = testdata[[i for i in range(10000)]].min(axis=1)
testdata[&#x27;std&#x27;] = testdata[[i for i in range(10000)]].std(axis=1)
testdata[&#x27;var&#x27;] = testdata[[i for i in range(10000)]].var(axis=1)
testdata[&#x27;skew&#x27;] = testdata[[i for i in range(10000)]].skew(axis=1)
testdata[&#x27;mean&#x27;] = testdata[[i for i in range(10000)]].mean(axis=1)
testdata[&#x27;median&#x27;] = testdata[[i for i in range(10000)]].median(axis=1)

# 入模特征选择
cols = [i for i in range(10000)] + [&#x27;gender&#x27;,&#x27;max&#x27;,&#x27;min&#x27;,&#x27;std&#x27;,&#x27;var&#x27;,&#x27;skew&#x27;,&#x27;mean&#x27;,&#x27;median&#x27;]
```

## 模型训练与验证

特征工程也好，数据清洗也罢，都是为最终的模型来服务的，模型的建立和调参决定了最终的结果。模型的选择决定结果的上限， 如何更好的去达到模型上限取决于模型的调参。

建模的过程需要我们对常见的线性模型、非线性模型有基础的了解。模型构建完成后，需要掌握一定的模型性能验证的方法和技巧

作为baseline方案使用catboost模型，并选择经典的K折交叉验证方法进行离线评估，大体流程如下：

1、K折交叉验证会把样本数据随机的分成K份；

2、每次随机的选择K-1份作为训练集，剩下的1份做验证集；

3、当这一轮完成后，重新随机选择K-1份来训练数据；

4、最后将K折预测结果取平均作为最终提交结果。

![](img/8bb75878ba4b4fb4480259a300b009bb.png)

具体代码如下：

```

# 模型训练与验证
# 定义一个名为catboost_model的函数，接收四个参数：train_x, train_y, test_x和seed
def catboost_model(train_x, train_y, test_x, seed = 2023):
    folds = 5  # 设置K折交叉验证折数为5
    kf = KFold(n_splits=folds, shuffle=True, random_state=seed) # 使用KFold方法创建一个交叉验证对象kf，设置折数、是否打乱顺序和随机种子
    oof = np.zeros(train_x.shape[0]) # 初始化一个全零数组oof，长度为train_x的长度
    test_predict = np.zeros(test_x.shape[0]) # 初始化一个全零数组test_predict，长度为test_x的长度
    cv_scores = [] # 初始化一个空列表cv_scores，用于存储交叉验证得分
    # 使用for循环遍历kf的每个折叠
    for i, (train_index, valid_index) in enumerate(kf.split(train_x, train_y)):
               # 打印当前折数的序号
        print(&#x27;************************************ {} ************************************&#x27;.format(str(i+1)))
        # 获取当前折叠的训练集索引和验证集索引，根据索引获取训练集和验证集的特征和标签
        trn_x, trn_y, val_x, val_y = train_x.iloc[train_index], train_y[train_index], train_x.iloc[valid_index], train_y[valid_index]
        # 定义CatBoostRegressor模型的参数
        params = {&#x27;learning_rate&#x27;: 0.1, # 学习率，控制模型参数更新的速度。值越大，模型更新越快，但可能陷入局部最优解；值越小，模型更新越慢，但可能收敛到更好的解。
          &#x27;depth&#x27;: 5,  # 树的深度，即决策树的最大层数。树的深度越深，模型的复杂度越高，可能导致过拟合；树的深度越浅，模型的复杂度越低，可能导致欠拟合。
          &#x27;bootstrap_type&#x27;:&#x27;Bernoulli&#x27;, # 自助法的类型，用于有放回地抽样。&#x27;Bernoulli&#x27;表示使用伯努利分布进行抽样，每次抽样后将结果反馈到训练集中。
          &#x27;random_seed&#x27;:2023, # 随机种子，用于控制随机过程。设置相同的随机种子可以保证每次运行代码时得到相同的结果。
          &#x27;od_type&#x27;: &#x27;Iter&#x27;,  # 迭代次数优化方法的类型。&#x27;Iter&#x27;表示使用迭代次数优化方法，通过多次迭代来寻找最优的迭代次数。
          &#x27;od_wait&#x27;: 100,  # 迭代次数优化方法的等待时间，即两次迭代之间的最小间隔。设置较长的等待时间可以加快收敛速度，但可能导致过拟合；设置较短的等待时间可以加快收敛速度，但可能导致欠拟合。
          &#x27;allow_writing_files&#x27;: False, # 是否允许写入文件。设置为False表示不保存模型参数，只返回模型对象。
          &#x27;task_type&#x27;:&quot;GPU&quot;,  # 任务类型，表示模型运行在GPU还是CPU上。设置为&quot;GPU&quot;表示模型运行在GPU上，如果计算机没有GPU，可以设置为&quot;CPU&quot;。
          &#x27;devices&#x27;:&#x27;0:1&#x27; }# 设备列表，表示使用哪些GPU设备。&quot;0:1&quot;表示只使用第一个GPU设备。
        
        # 创建CatBoostRegressor模型实例
        # 根据自己的算力与精力，调整iterations，V100环境iterations=500需要跑10min
        model = CatBoostRegressor(iterations=500, **params)
        # 使用训练集和验证集拟合模型
        model.fit(trn_x, trn_y, # 训练集的特征和标签，用于模型的训练。
                  eval_set=(val_x, val_y), # 验证集的特征和标签，用于在训练过程中评估模型性能。
                  metric_period=500, # 定评估指标的计算周期，即每隔多少次迭代计算一次评估指标。
                  use_best_model=True, # 设置为True表示在训练过程中使用验证集上性能最好的模型参数。
                  cat_features=[], # 包含需要转换为类别特征的特征名称，没有需要转换的特征，所以为空列表。
                  verbose=1) # 设置日志输出的详细程度，1表示输出详细信息。
                  
        # 使用模型对测试集进行预测
        val_pred  = model.predict(val_x)
        test_pred = model.predict(test_x)
        # 将验证集预测结果存储到oof数组中
        oof[valid_index] = val_pred
        # 计算K折测试集预测结果的平均值并累加到test_predict数组中
        test_predict += test_pred / kf.n_splits
        
        # 暂时忽略健康样本和患病样本在计算MAE上的差异，仅使用常规的MAE指标
        # 计算验证集预测结果与真实值之间的平均绝对误差（MAE）
        score = mean_absolute_error(val_y, val_pred)
        # 将MAE添加到cv_scores列表中
        cv_scores.append(score)
        print(cv_scores) # 打印cv_scores列表
        
        # 获取特征重要性打分，便于评估特征
        if i == 0:
                # 将特征名称和打分存储到DataFrame中
            fea_ = model.feature_importances_
            fea_name = model.feature_names_
            fea_score = pd.DataFrame({&#x27;fea_name&#x27;:fea_name, &#x27;score&#x27;:fea_})
            # 按照打分降序排列DataFrame
            fea_score = fea_score.sort_values(&#x27;score&#x27;, ascending=False)
            # 将排序后的DataFrame保存为CSV文件（命名为feature_importances.csv）
            fea_score.to_csv(&#x27;feature_importances.csv&#x27;, index=False)
        
    return oof, test_predict # 返回oof和test_predict数组

# 调用catboost_model函数，进行模型训练与结果预测
cat_oof, cat_test = catboost_model(traindata[cols], traindata[&#x27;age&#x27;], testdata[cols])
```

![](img/01c26bec6652dc3af388508f1d57c174.png)

**特征重要性结果展示：**

![](img/bb6340fe3d1dcf98950eba17aaaba748.png)

细致的观察特征重要性排序，我们手动构建的max、min、mean这类特征并没有排到前面，这也大概率说明这类特征可能没有太多作用。

## **结果输出**

[提交需要符合提交样例结果](https://tianchi.aliyun.com/competition/entrance/532114/submission/1117)，线上提交分数**3****.8****840**，能优化的空间还是非常大的。

```
# 输出赛题提交格式的结果
testdata[&#x27;age&#x27;] = cat_test # 将testdata数据框中的age列赋值为cat_test。
testdata[&#x27;age&#x27;] = testdata[&#x27;age&#x27;].astype(float) # 将age列的数据类型转换为浮点数。
testdata[&#x27;age&#x27;] = testdata[&#x27;age&#x27;].apply(lambda x: x if x&gt;0 else 0.0) # 使用lambda函数对age列中的每个元素进行判断，如果大于0，则保持不变，否则将其替换为0.0。
testdata[&#x27;age&#x27;] = testdata[&#x27;age&#x27;].apply(lambda x: &#x27;%.2f&#x27; % x) # 使用lambda函数将age列中的每个元素格式化为保留两位小数的字符串。
testdata[&#x27;age&#x27;] = testdata[&#x27;age&#x27;].astype(str) # 将age列的数据类型转换为字符串。
testdata[[&#x27;sample_id&#x27;,&#x27;age&#x27;]].to_csv(&#x27;submit.txt&#x27;,index=False) # 将sample_id和age两列保存到名为submit.txt的文件中，不包含索引。
```

**nrow**：读取数据行数，**iterations****：模型迭**代次数，阿里云PAI-DSW提供的最高配免费算力V100

![](img/f06f92cb2b6ce78dce290b658479d483.png)

## 完整代码

![](img/c5fc579406cab2e4bb126cc20a03f099.png)

[life-baseline.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/GAevbBQvvo66PxxBpqkcvBEun5f)

# 任务3：**进阶实践**

在进阶实践部分，将在原有Baseline基础上做更多优化，一般优化思路，从特征工程与模型中来思考。

**优化方法建议：**

1.  **读取所有特征：**因为特征字段过大，所以baseline仅使用部分特征，不免导致很多数据信息丢失，可以加载所有原始特征并进行特征筛选进行优化。

1.  **提取更多特征：**在数据挖掘比赛中，**特征**总是最终制胜法宝，去思考什么信息可以帮助我们提高预测精准度，然后将其转化为特征输入到模型。

1.  **尝试不同的模型：****模型**间存在很大的差异，预测结果也会不一样，比赛的过程就是不断的实验和试错的过程，通过不断的实验寻找最佳模型，同时帮助自身加强模型的理解能力。

## 特征优化

1、原始特征进行特征选择，过滤熟悉唯一、缺失率过高、高相关特征；

2、提取PCA、LDA、SVD这类降维特征。因为特征纬度过大，读取全部特征或者直接使用全部特征训练要求很大的内存，可以将数据逐组进行降维操作，比如将特征分10组，每组进行PCA、LDA、SVD操作，然后将10组降维后的结果合并起来作为入模特征。

**特征选择参考代码如下：**

```
drop_cols = []
print(&#x27;过滤异常特征...&#x27;)
for col in cols:
    if testdata[col].nunique()==1: # 属性值唯一
        drop_cols.append(col)
    if testdata[col].isnull().sum() / test_df.shape[0] &gt; 0.95: # 缺失率大于0.95
        drop_cols.append(col)

print(&#x27;过滤高相关特征...&#x27;)
def correlation(data, threshold):
    col_corr = []
    corr_matrix = data.corr()
    for i in range(len(corr_matrix)):
        for j in range(i):
            if abs(corr_matrix.iloc[i,j]) &gt; threshold: # 相关性大于0.98
                colname = corr_matrix.columns[i]
                col_corr.append(colname)
    return list(set(col_corr))

drop_cols += correlation(testdata[cols], 0.98)
```

**特征提取参考代码如下：**

```
# 合并训练数据和测试数据
df = pd.concat([traindata[[i for i in range(10000)]], testdata[[i for i in range(10000)]]], axis=0, ignore_index=True)

# 导入TruncatedSVD
from sklearn.decomposition import TruncatedSVD
# 调用TruncatedSVD
svd = TruncatedSVD(n_components = 16, random_state = 2023)
svd_res = svd.fit_transform(df.values)
for i in range(n_components):
    df[f&#x27;svd_{i}&#x27;] = svd_res[:,i]
```

## 模型融合

进行模型融合的前提是有多个模型的输出结果，比如使用catboost、xgboost和lightgbm三个模型分别输出三个结果，这时就可以将三个结果进行融合，最常见的是将结果直接进行加权平均融合。

下面我们构建了cv_model函数，内部可以选择使用lightgbm、xgboost和catboost模型，可以依次跑完这三个模型，然后将三个模型的结果进行取平均进行融合。

```
def cv_model(clf, train_x, train_y, test_x, clf_name, seed = 2023):
    &#x27;&#x27;&#x27;
    clf：调用模型
    train_x：训练数据
    train_y：训练数据对应标签
    test_x：测试数据
    clf_name：选择使用模型名
    seed：随机种子
    &#x27;&#x27;&#x27;
    folds = 5
    kf = KFold(n_splits=folds, shuffle=True, random_state=seed)
    oof = np.zeros(train_x.shape[0])
    test_predict = np.zeros(test_x.shape[0])
    cv_scores = []
    
    for i, (train_index, valid_index) in enumerate(kf.split(train_x, train_y)):
        print(&#x27;************************************ {} ************************************&#x27;.format(str(i+1)))
        trn_x, trn_y, val_x, val_y = train_x.iloc[train_index], train_y[train_index], train_x.iloc[valid_index], train_y[valid_index]
        
        if clf_name == &quot;lgb&quot;:
            train_matrix = clf.Dataset(trn_x, label=trn_y)
            valid_matrix = clf.Dataset(val_x, label=val_y)
            params = {
                &#x27;boosting_type&#x27;: &#x27;gbdt&#x27;,
                &#x27;objective&#x27;: &#x27;regression&#x27;,
                &#x27;metric&#x27;: &#x27;mae&#x27;,
                &#x27;min_child_weight&#x27;: 6,
                &#x27;num_leaves&#x27;: 2 ** 6,
                &#x27;lambda_l2&#x27;: 10,
                &#x27;feature_fraction&#x27;: 0.8,
                &#x27;bagging_fraction&#x27;: 0.8,
                &#x27;bagging_freq&#x27;: 4,
                &#x27;learning_rate&#x27;: 0.1,
                &#x27;seed&#x27;: 2023,
                &#x27;nthread&#x27; : 16,
                &#x27;verbose&#x27; : -1,
            }
            model = clf.train(params, train_matrix, 2000, valid_sets=[train_matrix, valid_matrix],
                              categorical_feature=[], verbose_eval=200, early_stopping_rounds=100)
            val_pred = model.predict(val_x, num_iteration=model.best_iteration)
            test_pred = model.predict(test_x, num_iteration=model.best_iteration)
        
        if clf_name == &quot;xgb&quot;:
            xgb_params = {
              &#x27;booster&#x27;: &#x27;gbtree&#x27;, 
              &#x27;objective&#x27;: &#x27;reg:squarederror&#x27;,
              &#x27;eval_metric&#x27;: &#x27;mae&#x27;,
              &#x27;max_depth&#x27;: 5,
              &#x27;lambda&#x27;: 10,
              &#x27;subsample&#x27;: 0.7,
              &#x27;colsample_bytree&#x27;: 0.7,
              &#x27;colsample_bylevel&#x27;: 0.7,
              &#x27;eta&#x27;: 0.1,
              &#x27;tree_method&#x27;: &#x27;hist&#x27;,
              &#x27;seed&#x27;: 520,
              &#x27;nthread&#x27;: 16
              }
            train_matrix = clf.DMatrix(trn_x , label=trn_y)
            valid_matrix = clf.DMatrix(val_x , label=val_y)
            test_matrix = clf.DMatrix(test_x)
            
            watchlist = [(train_matrix, &#x27;train&#x27;),(valid_matrix, &#x27;eval&#x27;)]
            
            model = clf.train(xgb_params, train_matrix, num_boost_round=2000, evals=watchlist, verbose_eval=200, early_stopping_rounds=100)
            val_pred  = model.predict(valid_matrix)
            test_pred = model.predict(test_matrix)
            
        if clf_name == &quot;cat&quot;:
            params = {&#x27;learning_rate&#x27;: 0.1, &#x27;depth&#x27;: 5, &#x27;bootstrap_type&#x27;:&#x27;Bernoulli&#x27;,&#x27;random_seed&#x27;:2023,
                      &#x27;od_type&#x27;: &#x27;Iter&#x27;, &#x27;od_wait&#x27;: 100, &#x27;random_seed&#x27;: 11, &#x27;allow_writing_files&#x27;: False}
            
            model = clf(iterations=2000, **params)
            model.fit(trn_x, trn_y, eval_set=(val_x, val_y),
                      metric_period=200,
                      use_best_model=True, 
                      cat_features=[],
                      verbose=1)
            
            val_pred  = model.predict(val_x)
            test_pred = model.predict(test_x)
        
        oof[valid_index] = val_pred
        test_predict += test_pred / kf.n_splits
        
        score = mean_absolute_error(val_y, val_pred)
        cv_scores.append(score)
        print(cv_scores)
        
    return oof, test_predict

# 选择lightgbm模型
lgb_oof, lgb_test = cv_model(lgb, traindata[cols], traindata[&#x27;label&#x27;], testdata[cols], &#x27;lgb&#x27;)
# 选择xgboost模型
xgb_oof, xgb_test = cv_model(xgb, traindata[cols], traindata[&#x27;label&#x27;], testdata[cols], &#x27;xgb&#x27;)
# 选择catboost模型
cat_oof, cat_test = cv_model(CatBoostRegressor, traindata[cols], traindata[&#x27;label&#x27;], testdata[cols], &#x27;cat&#x27;)

# 进行取平均融合
final_test = (lgb_test + xgb_test + cat_test) / 3
```

另外一种就是stacking融合，stacking是一种分层模型集成框架。以两层为例，第一层由多个基学习器组成，其输入为原始训练集，第二层的模型则是以第一层基学习器的输出作为特征加入训练集进行再训练，从而得到完整的stacking模型。

![](img/0c66439e7d9edeaf916cf1389192d313.png)

第一层：（类比cv_model函数）

1.  划分训练数据为K折（5折为例，每次选择其中四份作为训练集，一份作为验证集）；

1.  针对各个**模型RF、ET、GBDT、XGB，分别进行5次训练，每次训练保留一份样本用作训**练时的验证，训练完成后分别对Validation set，Test set进行预测，对于Test set一个模型会对应5个预测结果，将这5个结果取平均；对于Validation set一个模型经过5次交叉验证后，所有验证集数据都含有一个标签。此步骤结束后：5个验证集（总数相当于训练集全部）在每个模型下分别有一个预测标签，每行数据共有4个标签（4个算法模型），测试集每行数据也拥有四个标签（4个模型分别预测得到的）

第二层：（类比stack_model函数）

1.  **将训练集中的四个标签外加真****实标签当作五列新的特征作为新**的训练集，选取一个训练模型，根据新的训练集进行训练，然后应用测试集的四个标签组成的测试集进行预测作为最终的result。

**Stacking参考代码：**

```
def stack_model(oof_1, oof_2, oof_3, predictions_1, predictions_2, predictions_3, y):
    &#x27;&#x27;&#x27;
    输入的oof_1, oof_2, oof_3可以对应lgb_oof，xgb_oof，cat_oof
    predictions_1, predictions_2, predictions_3对应lgb_test，xgb_test，cat_test
    &#x27;&#x27;&#x27;
    train_stack = pd.concat([oof_1, oof_2, oof_3], axis=1)
    test_stack = pd.concat([predictions_1, predictions_2, predictions_3], axis=1)
    
    oof = np.zeros((train_stack.shape[0],))
    predictions = np.zeros((test_stack.shape[0],))
    scores = []
    
    from sklearn.model_selection import RepeatedKFold
    folds = RepeatedKFold(n_splits=5, n_repeats=2, random_state=2021)
    
    for fold_, (trn_idx, val_idx) in enumerate(folds.split(train_stack, train_stack)): 
        print(&quot;fold n°{}&quot;.format(fold_+1))
        trn_data, trn_y = train_stack.loc[trn_idx], y[trn_idx]
        val_data, val_y = train_stack.loc[val_idx], y[val_idx]
        
        clf = Ridge(random_state=2021)
        clf.fit(trn_data, trn_y)

        oof[val_idx] = clf.predict(val_data)
        predictions += clf.predict(test_stack) / (5 * 2)
        
        score_single = mean_absolute_error(val_y, oof[val_idx])
        scores.append(score_single)
        print(f&#x27;{fold_+1}/{5}&#x27;, score_single)
    print(&#x27;mean: &#x27;,np.mean(scores))
   
    return oof, predictions
    
stack_oof, stack_pred = stack_model(lgb_oof, xgb_oof, cat_oof, lgb_test, xgb_test, cat_test, traindata[&#x27;age&#x27;])
```

## 完整代码

![](img/ba11fb0d4adb64c36fc09f042769d154.png)

[baseline.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/P8NKbTLWQozk0gxWHlwcBCRNnJg)

# 附录

## [QA文档](https://xqbbve5hpvs.feishu.cn/docx/DiooduxPHoCbv4xk5atc9Td5npe#NdHsdB72Jo9sZhxem5mcDnrXnGd)

[Ai for science 赛道问题库](https://xqbbve5hpvs.feishu.cn/docx/DiooduxPHoCbv4xk5atc9Td5npe#NdHsdB72Jo9sZhxem5mcDnrXnGd)

## 实践环境配置

**AI环境配置：**

+   **视频讲解：**AI夏令营：手把手带你配置AI环境_哔哩哔哩_bilibili

+   **图文材料：**https://gitee.com/anine09/learn-python-the-smart-way-v2/blob/main/slides/chapter_0-Installation.ipynb

## 工具使用与提分思路

+   竞赛实践路线：[竞赛实践路线分享](https://datawhaler.feishu.cn/docx/EJ2Edl0hXoIWwuxO15CcEj9Wnxn)

+   机器学习特征工程实用技巧大全：https://zhuanlan.zhihu.com/p/67832773