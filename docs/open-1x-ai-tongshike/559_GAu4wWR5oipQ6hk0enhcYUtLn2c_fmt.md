
# AI+大气科学：天气预报

> 来源：[https://datawhaler.feishu.cn/docx/SRkydw1ufoAclmxeIfAct4uGnAb](https://datawhaler.feishu.cn/docx/SRkydw1ufoAclmxeIfAct4uGnAb)

## 首届世界科学智能大赛 - AI + 大气科学

> 首届世界科学智能大赛：大气科学赛道——华东区域AI中期天气预报大赛：

> [h](https://tianchi.aliyun.com/competition/entrance/532111/introduction)ttps://tianchi.aliyun.com/s/e09ba4f624e4678219c563c15f1bb5a2

> **举办方：****复旦大学，****阿里云****，中国信通院**

> 本内容由 @王贺、@宋志学 写作，欢迎大家点赞&评论~

| 学习任务 |
| --- |
| **任务一：** |
| **任务二：** |
| **任务三：** |

## 赛题背景

本次比赛是基于AI的区域天气预测大赛，提供10年的再分析数据，通过输入历史70个大气变量数据，预测华东区域未来1-5天的5个地面变量。区域天气预报既有较大的挑战性，也与人们的生活息息相关，具有非常好的实际价值。期望通过本次大赛推动人工智能技术在气象领域的应用和研究，加速深度学习算法在气象领域的发展和推广，同时促进气象行业的数字化、智能化和科技进步。

## 赛事任务

提供10年的再分析数据，通过输入历史70个大气变量数据，预测华东区域未来1-5天的5个地面变量。

## 赛题数据集

华东区域AI中期天气预报测大赛：输入历史2个时刻的多个大气变量，输出未来1-5天逐6小时的5个地面变量。相关变量定义如下：

![](img/c240d8e682fa8bc93b0b2261ec0e9ccc.png)

比赛数据来自欧洲中期天气预报中心(ECMWF)的再分析数据ERA5，使用了ERA5的子集，包含5个气压层变量(每个变量有13个气压层)，5个地面变量，共计70个变量，时间间隔6小时，空间分辨率0.25度，覆盖区域(N10 ~ N50, E100 ~ E140)。

![](img/19b2786c1ce0aa4b9ab9a0524f2f1bf2.png)

推理文件的输入`Input: (2 x 70 x H x W)`，输出`Output: (20 x 5 x H x W)`，其数据类型是`float16`，存储为`pt`格式。参赛队伍可以不使用全部的70个变量做为输入，但是输出必须包含5个地面变量，且顺序固定按照：`[T2M, U10, V10, MSL, TP]`，必须包含20个step对应未来时刻如下：

`[6,12,18,24,30,36,42,48,54,60,66,72,78,84,90,96,102,108,114,120]`（小时）

输入和输出文件组织如下:

![](img/caef6e523056bdc5477b1b48c52f15f6.png)

通过上面的介绍，对于数据方面，训练集和测试集并没有时序联系的，测试集部分所能使用的历史数据仅为2个时刻的数据，然后输出未来20个时刻的结果。可以看到，能够使用的历时信息非常少，需要预测的时刻比较多，相应提升了赛题的难度。

## 评价指标

![](img/07eb59e13f10c84c2e225d4f866f316a.png)

## 解题思路

本题任务是预测未来1-5天逐6小时的5个地面变量，属于典型的回归问题，输入数据为历史大气变量信息，输出5个变量结果，分别为2米温度、10米纬向风、10米经向风、平均海平面气压、6小时累积降水。针对这类时间序列预测问题方法比较灵活，传统的时序模型、机器学习、深度学习方法均可以使用。

**1、统计策略：**使用最近时刻的结果进行均值、中位数、时间衰减等方式直接统计得到未来结果，这种方式比较简单，可以快速得到结果；

**2、时序模型：**比较常用的方法有指数平滑法、灰色预测模型、`ARIMA`预测、季节`Sarima`模型、`VAR`模型等，仅能刻画序列信息，无法加入其他信息进行训练，比如离散类特征；

**3、机器学习模型：**常见的为`lightgbm`、`xgboost`、`catboost`，需要构建大量时序相关特征；

**4、深度学习模型：**常见为`rnn`、`lstm`、`cnn`、`transformer`这类模型，可以直接输入序列信息，不需要构建大量的人工特征；

因为整体数据处理起来还是比较复杂的，为了快速的得到一个结果，所以我们选择使用统计策略作为`baseline`方案，后续进阶实践部分则使用机器学习模型。不过我们会在baseline实践部分花些时间进行数据解读，为进阶部分做铺垫。

# 任务1：统计方法Baseline实践

## 云环境配置与数据下载

介于本赛事的数据规模比较庞大，推荐大家使用赛事官方推荐的阿里云的云环境，提供免费V100与A10算力

[Ai for science 大气科学赛道阿里云DSW部署教程](https://qwosdw576oc.feishu.cn/docx/GsusdtwYFoFW8FxJVkzcc1HhnAf)

![](img/681963140dcf5dd1c65dba9cc81f8235.png)

[weather_stats_baseline_update.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SHfKbAR7EoQL4rx72LRciJqOnUf)

> **如何运行**`Baseline`视频讲解 @王子丞 助教贡献

<https://player.bilibili.com/player.html?bvid=1PF41117Zp>

刚刚我们完成了基本的问题分析，接下来会带领大家跑通数据读取、数据探索、统计策略、结果输出的全部竞赛实践流程。

## 导入模块

导入我们本次`Baseline`代码所需的模块

```
#import 相关库
import os
import numpy as np
import pandas as pd
import xarray as xr

import torch
torch.random.seed()
np.random.seed(0)
```

## 数据探索

数据探索性分析，是通过了解数据集，了解变量间的相互关系以及变量与预测值之间的关系，对已有数据在尽量少的先验假设下通过作图、制表、方程拟合、计算特征量等手段探索数据的结构和规律的一种数据分析方法，从而帮助我们后期更好地进行特征工程和建立模型，是机器学习中十分重要的一步。

可参考下图流程制作内容

![](img/5ce1ed0658875fa5efa8e338debaaa1c.png)

加载数据及基本的预处理，需要特别注意：

1. 数据分为两部分，训练数据和测试数据，并且读取方式是不一样的；

1. 训练数据非常大，不能直接展开，可以选择一部分数据进行展开，了解数据基本情况；

1. [与常规读取数据的方式不](https://blog.csdn.net/mihuanchengbao/article/details/126641111?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-126641111-blog-80790874.235^v38^pc_relevant_default_base3&spm=1001.2101.3001.4242.1&utm_relevant_index=3)同，这里使用`xarray`进行数据加载；`xarray` 使用教程

具体代码如下：

```
data_dir = 'weather' # 你自己存储数据的目录

# 按时间进行数据块划分
def chunk_time(ds):
    dims = {k:v for k, v in ds.dims.items()}
    dims['time'] = 1
    ds = ds.chunk(dims)
    return ds

# 加载训练数据
def load_dataset():
    ds = []
    for y in range(2007, 2012):
        data_name = os.path.join(data_dir, f'weather_round1_train_{y}')
        x = xr.open_zarr(data_name, consolidated=True)
        print(f'{data_name}, {x.time.values[0]} ~ {x.time.values[-1]}')
        ds.append(x)
    ds = xr.concat(ds, 'time')
    ds = chunk_time(ds)
    return ds

ds = load_dataset().x

num_step = 20 # 5天的数据，每6小时一个时刻
shape = ds.shape # 四维数据，time x channel x lat x lon 
times = ds.time.values # 提取对应的所有time值
init_times = times[slice(1, -num_step)] 
num_data = len(init_times) # 
names = list(ds.channel.values) # 提取所有变量名
test_names = names[-5:] # 最后5个变量为目标变量，即我们需要预测的目标

print(f'\n shape: {shape}')
print('\n times: {} ~ {}'.format(times[0], times[-1]))
print('\n init_times: {} ~ {}'.format(init_times[0], init_times[-1]))
print(f'\n names: {names}')
print(f'\n test_names: {test_names}\n')    
```

![](img/2fea30dcdaece727b4bbf3e61a26b74a.png)

```
ds
```

![](img/44d1ac2887c0403f62a3d0253eba0252.png)

加载出来的数据格式是`xarray.DataArray`，如果是气象领域的同学可能接触过这样数据格式类型，直接打印出来的是数据的结构，以及不同维度对应的数据类型和部分对应数值。通过如下代码可以直接对应维度的值

```
print(ds.time.values)
print(ds.channel.values)
print(ds.lat.values)
print(ds.lon.values)
```

![](img/c4d613fb092d30adfb6a05a6018df67a.png)

如果想要看具体的变量值，则可以通过如下代码查看，因为数据比较大，所以仅看第一个time值（2007-01-01T00:00:00.000000000），对应的`z50`，`lat`和`lon`下的所有`z50`值：

```
ds[0,0,:,:].values
# 对应维度
# ds[0,0,:,:].shape
# (161, 161)
```

![](img/309f3e12d563f787323d020b565ebce2.png)

可以看到一个时刻对应某个变量的值应该是161*161个，即不同经纬度下的值。所以需要预测的结果是20个时刻对应5个目标变量下不同经纬度下的值，即20*5*161*161。

已经分析完了训练数据，接下来看看测试数据，加载代码如下：

```
file_lists = os.listdir(f'{path}/weather_round1_test/input')
for file in file_lists:
    input_data = torch.load(f'{path}/weather_round1_test/input/{file}')
    break
print(file_lists) # 打印所有输入数据，每个pt文件为一个测试集输入数据，pt文件直接没有任何相关性
```

![](img/8a1116ac1fba8e3bb292895b58f7e586.png)

接下来看看数据长什么样子

```
input_data.shape, input_data
# shape为（2,70,161,161)，即输入数据为两个时刻的数据
```

![](img/f6800aae04e25d5d855d3176b370e4a8.png)

一般时间序列预测问题都有较长的历史数据，而本次赛题却只有历史两个时刻单位的数据，这就导致常见的时间序列特征提取方式在这个赛题中无法使用，或者极具困难。

## 数据清洗

数据和特征决定了机器学习的上限，而模型和算法只是逼近这个上限而已。俗话说：garbage in, garbage out。分析完数据后，特征工程前，必不可少的步骤是对数据清洗。

数据清洗的作用是利用有关技术如数理统计、数据挖掘或预定义的清理规则将脏数据转化为满足数据质量要求的数据。主要包括缺失值处理、异常值处理、数据分桶、特征归一化/标准化等流程。

![](img/8e38ce22e82663879a75060867422fda.png)

数据非常干净，不需要进行数据清洗工作。

## 统计策略

作为`baseline`方案使用历史均值统计策略，方案简单，可以快速获得结果。

具体代码如下：

```
import torch,os
path = 'weather' 
file_lists = os.listdir(f'{path}/weather_round1_test/input')
# 遍历读取每个测试集输入数据文件
for file in file_lists:
    input_data = torch.load(f'{path}/weather_round1_test/input/{file}')
    # 计算两个时刻对应目标变量的均值，并复制20份，即20个时刻
    output_data = torch.tile(torch.mean(input_data, axis=0)[-5:,:,:], (20,1,1,1))
    # 计算结果保存到output文件作为输出结果
    torch.save(output_data.half(), f'output/{file}', ) # 精度转为半精度浮点类型
```

## 结果输出

提交需要符合提交样例结果

```
# 现在就可以得到答案的压缩包啦~~~
_ = !zip -r output.zip output/

```

```
#如果上面代码无响应，运行下面这段代码
import zipfile,os
path='./output/' #压缩当前路径所有文件，输出zip文件
zipName = 'output.zip' #压缩后文件的位置及名称
f = zipfile.ZipFile( zipName, 'w', zipfile.ZIP_DEFLATED )
for dirpath, dirnames, filenames in os.walk(path):
    for filename in filenames:
        print(filename)
        f.write(os.path.join(dirpath,filename))
f.close()
```

把`output.zip`下载到本地，打开[赛事主页](https://tianchi.aliyun.com/s/e09ba4f624e4678219c563c15f1bb5a2)，在**提交结果**初提交`output.zip`，稍等片刻评测成功，在**我的成绩**处可以看到成绩

![](img/6f184cda621e1e0168f68e8808ec66a3.png)

![](img/b39d62bb8174fc0cbdfe1addb4ea2154.png)

成功出分，不过分数并不高，这也不见得是件糟糕的事情，起码我们有很多提升空间。具体怎么提升优化呢，在进阶实践部分将使用机器学习方法来构建我们的优化方案，一起拭目以待吧。

## 完整代码

![](img/a22463e87bbd3fe94f0f56769684ba07.png)

[weather_stats_baseline_update.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CmlnbpBtOo1Wa6xPuHJc25fJnpe)

# 任务2：机器学习Baseline实践

## 云环境配置与数据下载

介于本赛事的数据规模比较庞大，推荐大家使用赛事官方推荐的阿里云的云环境，提供免费V100与A10算力

[Ai for science 大气科学赛道阿里云DSW部署教程](https://qwosdw576oc.feishu.cn/docx/GsusdtwYFoFW8FxJVkzcc1HhnAf)

进阶实践部分，我们将使用机器学习的方法搭建方案，模型选择`catboost`，在`baseline`部分已经进行了数据探索和数据清洗两个部分，进阶部分直接跳过。主要围绕数据预处理、特征工程、模型训练、模型验证进展开实践。

**实践难点：**



## 数据格式与以往的不同

在数据挖掘比赛中，数据是四维的形式的呈现（`time`、`channl`、`lat`、`lon`），与以往直接转为`dataframe`格式完全不同，需要我们自行转换，并且自行构建训练样本和测试样本。

## 可用的历史数据非常少

每份训练数据只提供历史2个时刻的输入文件，这也表明仅能使用2个时刻的数据提取特征，如何使用2个时刻数据刻画未来20个时刻的目标变量信息能为难点。

## 数据量非常大

每份测试数据需要预测的结果为（20，5，161，161），对应20*5*161*161条样本，有300份测试数据，那就是777630000条样本，就单单进行样本构建、特征转换和结果预测就非常耗时了。训练数据是远大于测试数据，不过可以提取部分数据输入到模型进行训练，并不需要全部使用。

> 以下代码已全部更新，增加了逐行代码注释，@刘星，助教贡献

![](img/2232472f489c119c729e03f4e1cd6817.png)

[weather_ml_baseline_update代码注释.ipynb](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LHwoboi5OoTZvtxEzF8c761Wn9b)

### 导入模块

相较`baseline`部分，这里需要导入更多的模块。

```
import numpy as np  # 导入NumPy库，用于数值计算和数组操作
import pandas as pd  # 导入Pandas库，用于数据处理和分析
import xarray as xr  # 导入Xarray库，用于处理标签化的多维数据
from collections import defaultdict, Counter  # 导入defaultdict和Counter类，用于创建默认字典和计数器
import xgboost as xgb  # 导入XGBoost库，用于梯度提升树模型
import lightgbm as lgb  # 导入LightGBM库，也用于梯度提升树模型
from catboost import CatBoostRegressor  # 导入CatBoostRegressor类，用于CatBoost回归模型
from sklearn.model_selection import StratifiedKFold, KFold, GroupKFold  # 导入三种交叉验证类，分别是分层K折交叉验证、K折交叉验证和分组K折交叉验证
from sklearn.metrics import mean_squared_error  # 导入均方误差函数
from joblib import Parallel, delayed  # 导入Parallel和delayed类，用于并行化计算任务
from tqdm.auto import tqdm  # 导入tqdm类，用于在循环中显示进度条
import sys, os, gc, argparse, warnings, torch, glob  # 导入其他常用模块，如sys、os、gc、argparse、warnings、torch、glob

warnings.filterwarnings('ignore')  # 设置警告过滤器，将忽略警告信息

# pip install importlib-metadata==4.13.0
# pip install zarr lightgbm catboost
# pip install xarray[complete]
```

### 加载数据

加载数据部分与`baseline`处操作方式基本一致，这里仅为训练数据的加载，代码如下：

```
path = './weather'  # 数据集文件路径

def chunk_time(ds):
    # 获取数据集维度的字典并赋值给dims变量
    dims = {k: v for k, v in ds.dims.items()}
    # 在'time'维度上将数据集进行分块处理，每块包含一个时间步
    dims['time'] = 1
    ds = ds.chunk(dims)
    return ds

def load_dataset():
    ds = []  
    for y in range(2007, 2012):  # 遍历年份2007到2011
        data_name = os.path.join(path, f'weather_round1_train_{y}')  # 构建数据文件名
        x = xr.open_zarr(data_name, consolidated=True)  # 打开Zarr格式的数据集文件
        print(f'{data_name}, {x.time.values[0]} ~ {x.time.values[-1]}')  # 输出数据文件名和时间范围
        ds.append(x)  # 将打开的数据集添加到ds列表中
        
    ds = xr.concat(ds, 'time')  # 在'time'维度上连接所有数据集
    ds = chunk_time(ds)  # 对数据集进行时间分块处理
    return ds

ds = load_dataset().x  # 调用load_dataset函数加载数据集，并获取其中的'x'变量
```

### 构建训练样本

因为数据量比较大，所以仅提取10份训练数据，每份数据大小应该为(22,70,161,161)，共22个时刻的数据，前面2个时刻用来提取特征，后面20个时刻用来提取目标变量。另外需要特别注意的是：

1. 整个流程涉及很多数组的操作，如提取数据、维度转换、数组合并等，需要仔细操作；
2. 分别提取时间、经纬度、第一个时刻的70个变量、第二个时刻的70个变量、两个时刻70个变量的差值。需要特别注意，对于未来20个时刻，除了时间信息是不一样的外，其余特征变量完全一致，主要刻画相同特征不同时间的情况下，目标变量的变化；

```
lats = ds.lat.values.tolist()  # 将数据集中的纬度值转换为Python列表，并赋值给lats变量
lons = ds.lon.values.tolist()  # 将数据集中的经度值转换为Python列表，并赋值给lons变量

# 对齐测试数据，每份数据应该是22个时刻
# 获取多份训练数据，滑动5次，每次24个时刻（6天），每次滑动提取最后22个时刻作为一份训练数据
train_data = []
for i in range(5):
    if i == 0:  # 如果是第一份训练数据
        print(-22, 0)  # 输出起始和结束索引
        train_data.append(ds[-22:, :, :, :].values)  # 提取最后22个时刻的数据，添加到训练数据列表中
    else:
        idx = i * 24  # 计算索引偏移量
        train_data.append(ds[-22-idx:-idx, :, :, :].values)  # 提取最后22个时刻的数据，添加到训练数据列表中
        print(-22-idx, -idx)  # 输出起始和结束索引

# 经纬度标识字段
latlon_vals = []
for i in range(161):  # 遍历纬度索引
    for j in range(161):  # 遍历经度索引
        latlon_vals.append([lats[i], lons[j]])  # 将每个纬度和经度对应的值添加到列表中
latlon_vals = np.array(latlon_vals)  # 将列表转换为NumPy数组，并赋值给latlon_vals变量

# 确定好20份训练数据后，接下来需要提取特征
for flag, dat in tqdm(enumerate(train_data)):  # 遍历每份训练数据，并显示进度条
    # 第一时刻特征
    first_feat = dat[0,:,:,:].reshape(70,161*161).transpose()  # 将第一时刻的数据转换为特征向量，reshape后的形状为(70, 161*161)，然后进行转置得到(161*161, 70)
    # 第二时刻特征
    second_feat = dat[1,:,:,:].reshape(70,161*161).transpose()  # 将第二时刻的数据转换为特征向量，reshape后的形状为(70, 161*161)，然后进行转置得到(161*161, 70)
    # 两个时刻差分特征
    diff_feat = (dat[1,:,:,:] - dat[0,:,:,:]).reshape(70,161*161).transpose()  # 将两个时刻的数据差分并转换为特征向量，reshape后的形状为(70, 161*161)，然后进行转置得到(161*161, 70)
    
    # 构建训练样本
    tmp_dat = dat[2:,-5:,:,:]  # 从第3个时刻开始获取最后5个channels的数据，形状为(20, 5, 161, 161)
    for i in range(20):  # 遍历20个时刻
        # 时间特征、经纬特征
        time_vals = np.array([i]*161*161).reshape(161*161,1)  # 创建一个维度为(161*161,1)的数组，每个元素为当前时刻的时间特征i
        sub_vals = np.concatenate((time_vals, latlon_vals), axis=1)  # 将时间特征和经纬度特征拼接在一起，形状为(161*161,3)
        
        # 添加历史特征，第一时刻特征、第二时刻特征、两个时刻差分特征
        sub_vals = np.concatenate((sub_vals, first_feat), axis=1)  # 将第一时刻特征拼接到子特征向量中，形状变为(161*161,73)
        sub_vals = np.concatenate((sub_vals, second_feat), axis=1)  # 将第二时刻特征拼接到子特征向量中，形状变为(161*161,143)
        sub_vals = np.concatenate((sub_vals, diff_feat), axis=1)  # 将两个时刻差分特征拼接到子特征向量中，形状变为(161*161,213)
        
        # 添加5个目标变量，形状变为(161*161,218)
        for j in range(5):
            var_vals = tmp_dat[i,j,:,:].reshape(161*161, 1)  # 将目标变量的数据转换为特征向量，形状为(161*161,1)
            sub_vals = np.concatenate((sub_vals, var_vals), axis=1)  # 将目标变量拼接到子特征向量中
        
        if i == 0 :
            all_vals = sub_vals  # 如果是第一个时刻，则将子特征向量赋值给all_vals
        else:
            all_vals = np.concatenate((all_vals, sub_vals), axis=0)  # 如果不是第一个时刻，则将子特征向量与all_vals进行垂直拼接
        
    if flag == 0:
        final_vals = all_vals  # 如果是第一份训练数据，则将all_vals赋值给final_vals
    else:
        final_vals = np.concatenate((final_vals, all_vals), axis=0)  # 如果不是第一份训练数据，则将all_vals与final_vals进行垂直拼接
```

将二维数组转为`dataframe`格式，并设置新的列名，然后选择入模特征，代码如下：

```
train_df = pd.DataFrame(final_vals)  # 将提取的特征final_vals转换为DataFrame格式，并赋值给train_df
train_df.columns = ['time','lat','lon'] + [f'feat_{i}' for i in range(210)] + ['t2m','u10','v10','msl','tp']
# 添加列名，分别为时间特征、经度特征、纬度特征、210个特征向量、目标变量t2m、u10、v10、msl和tp

cols = ['time','lat','lon'] + [f'feat_{i}' for i in range(210)]
# 创建列名列表，包括时间特征、经度特征、纬度特征和210个特征向量列名
```

### 训练模型

模型选择使用`catboost`模型，并且将模型训练和模型推理单独构建，主要是方便对300份测试数据进行单独推理。另外需要注意，模型学习率`learning_rate`设置的比较大、迭代次数`iterations`设置的比较少，主要为了快速完成模型训练，在进行优化的时候可以适当的调整模型参数，让模型充分训练。

代码如下：



# 模型训练

```python
def train_model(train_x, train_y, label, seed=2023):
    # 初始化交叉验证的变量和参数
    oof = np.zeros(train_x.shape[0])  # 存储交叉验证的预测结果
    kf = KFold(n_splits=5, shuffle=True, random_state=seed)  # 5折交叉验证
    cv_scores = []  # 存储每一折的验证集分数

    for i, (train_index, valid_index) in enumerate(kf.split(train_x, train_y)):
        print('************************************ {} {}************************************'.format(str(i+1), str(seed)))
        trn_x, trn_y, val_x, val_y = train_x.iloc[train_index], train_y[train_index], train_x.iloc[valid_index], train_y[valid_index]
        #trn_x, trn_y, val_x, val_y：根据索引划分出当前折的训练集和验证集。

        # 设置CatBoost模型的参数
        params = {'learning_rate': 0.5,
                  'depth': 5,
                  'bootstrap_type':'Bernoulli',
                  'random_seed':2023,
                  'od_type': 'Iter',
                  'od_wait': 100,
                  'random_seed': 11,
                  'allow_writing_files': False,
                  'task_type':"GPU",
                  'devices':'0:1'}

        model = CatBoostRegressor(iterations=100, **params)  # 创建CatBoost回归模型
        model.fit(trn_x, trn_y, eval_set=(val_x, val_y),
                  cat_features=[],
                  use_best_model=True,
                  verbose=1)  # 在训练集上拟合模型并在验证集上进行评估
        # model.save_model(f'catboost_{label}_fold{i}.model')  # 保存模型
        model.save_model(f'model/catboost_{label}_fold{i}.model')  # 保存模型到指定路径
        val_pred  = model.predict(val_x)  # 在验证集上进行预测
        oof[valid_index] = val_pred  # 将验证集的预测结果填充到oof数组中

        cv_scores.append(np.sqrt(mean_squared_error(val_y, val_pred)))  # 计算并存储验证集的均方根误差
        
        if i == 0: #如果是第一折（i==0），计算特征重要性并保存到CSV文件。
            fea_ = model.feature_importances_
            fea_name = model.feature_names_
            fea_score = pd.DataFrame({'fea_name':fea_name, 'score':fea_})
            fea_score = fea_score.sort_values('score', ascending=False)
            fea_score.to_csv('feature_importances.csv', index=False)

        print(cv_scores) #打印当前折的验证集分数。
    return oof
    
oofs = []  # 存储所有特征的交叉验证预测结果

# 对每个特征进行训练和预测
for label in ['t2m', 'u10', 'v10', 'msl', 'tp']:
    print(f'==================== {label} ====================')
    
    # 调用train_model函数进行训练，并将返回的交叉验证预测结果存储到cat_oof变量中
    cat_oof = train_model(train_df[cols], train_df[label], label)
    
    # 将交叉验证预测结果添加到oofs列表中
    oofs.append(cat_oof)
```

![](img/30c5a91b44ae2704d59e02b57e9bd889.png)

特征重要性：可以看到`time`特征的重要性最高，如果没有`time`特征，那么20个时刻的预测结果将完全一样

![](img/e6cfb3937a98e85a6192d0ed4d0849e2.png)

## 测试集预测

在训练模型阶段提前将训练好的模型进行保存，在模型推理函数部分直接加载对应目标变量的模型即可，代码如下：

```python
def inter_model(test_x, label, seed=2023):
    kf = KFold(n_splits=5, shuffle=True, random_state=seed)  # 5折交叉验证
    test = np.zeros(test_x.shape[0])  # 存储测试集的预测结果

    params = {'learning_rate': 0.5, 'depth': 6, 'bootstrap_type':'Bernoulli','random_seed':seed, 'metric_period': 300,
              'od_type': 'Iter', 'od_wait': 100, 'random_seed': 11, 'allow_writing_files': False, 'task_type': 'CPU',
              'task_type':"GPU", 'devices':'0:1'}

    for i in range(0, 5):
        # print('************************************ {} {}************************************'.format(str(i+1), str(seed)))
        model = CatBoostRegressor(**params)
        # model.load_model(f'catboost_{label}_fold{i}.model')
        model.load_model(f'model/catboost_{label}_fold{i}.model')  # 加载训练好的模型
        test_pred = model.predict(test_x)  # 在测试集上进行预测
        test += test_pred / kf.n_splits  # 将每一折的预测结果加权平均得到最终的测试集预测结果

    return test
```

构建测试样本，及特征提取函数，这里的构建流程与训练集部分基本一致，除了没有5个目标变量外。代码如下：

```python
def get_test_data(dat):
    # 第一时刻特征
    first_feat = dat[0,:,:,:].reshape(70,161*161).transpose() # 将第一时刻的特征数据从(70, 161, 161)转换为(161*161, 70)

    # 第二时刻特征
    second_feat = dat[1,:,:,:].reshape(70,161*161).transpose() # 将第二时刻的特征数据从(70, 161, 161)转换为(161*161, 70)

    # 两个时刻差分特征
    diff_feat = (dat[1,:,:,:] - dat[0,:,:,:]).reshape(70,161*161).transpose() # 计算两个时刻特征之间的差分，并将结果从(70, 161, 161)转换为(161*161, 70)
    
    # 构建测试样本
    for i in range(20): # 20个时刻
        # 时间特征、经纬特征
        time_vals = np.array([i]*161*161).reshape(161*161,1) # 创建一个形状为(161*161, 1)的数组，每个元素都是i
        sub_vals = np.concatenate((time_vals, latlon_vals), axis=1) # 将时间特征和经纬度特征按列进行拼接，形状为(161*161, 3)
        
        # 添加历史特征，第一时刻特征、第二时刻特征、两个时刻差分特征
        sub_vals = np.concatenate((sub_vals, first_feat), axis=1) # 将第一时刻特征拼接到sub_vals中，形状为(161*161, 73)
        sub_vals = np.concatenate((sub_vals, second_feat), axis=1) # 将第二时刻特征拼接到sub_vals中，形状为(161*161, 143)
        sub_vals = np.concatenate((sub_vals, diff_feat), axis=1) # 将两个时刻差分特征拼接到sub_vals中，形状为(161*161, 213)
        
        if i == 0 :
            all_vals = sub_vals
        else:
            all_vals = np.concatenate((all_vals, sub_vals), axis=0) # 将sub_vals沿着行方向进行拼接
            
    df = pd.DataFrame(all_vals) # 将结果转换为DataFrame格式
    df.columns = ['time','lat','lon'] + [f'feat_{i}' for i in range(210)] # 给DataFrame的列命名
    return df
```

结果生成函数，代码流程：

1. 生成测试样本，并构建特征；
2. 调用推理模型函数，分别预测5个目标变量的结果；
3. 将结果转为可提交格式(20,5,161,161)；
4. Array转为Tensor；
5. 保存为pt文件，精度为半精度浮点类型。

代码如下：

```python
def get_parallel_result(file):
    # 加载输入数据
    input_data = torch.load(file)  # 加载保存在文件中的输入数据

    # 生成测试样本，并构建特征
    test_df = get_test_data(np.array(input_data))  # 根据输入数据生成测试样本并构建特征

    # 保存结果
    res = test_df[['time']]  # 创建一个只包含时间列的DataFrame用于保存结果

    # 模型推理，预测测试集结果
    for label in ['t2m', 'u10', 'v10', 'msl', 'tp']:
        # 通过模型推理预测测试集结果，inter_model是一个未给出的模型推理函数
        cat_pred = inter_model(test_df[cols], label)
        res[label] = cat_pred  # 将预测结果保存到res中

    # 转为提交格式
    for label in ['t2m', 'u10', 'v10', 'msl', 'tp']:
        for i in range(20):
            sub_vals = res[res['time'] == i][label].values.reshape(1, 161, 161)
            # 将时间为i的某个标签的预测结果转换为形状为(1, 161, 161)的数组

            if i == 0:
                all_vals = sub_vals
            else:
                all_vals = np.concatenate((all_vals, sub_vals), axis=0)
                # 将所有时间步的预测结果按时间序列连接起来，形状为(20, 161, 161)

        all_vals = all_vals.reshape(20, 1, 161, 161)

        if label == 't2m':
            final_vals = all_vals
        else:
            final_vals = np.concatenate((final_vals, all_vals), axis=1)
            # 将不同标签的预测结果按照通道连接起来，形状为(20, 5, 161, 161)

    final_vals = torch.tensor(final_vals)  # 将最终的结果转换为PyTorch张量
    save_file = file.split('/')[-1]  # 获取保存结果的文件名

    # torch.save(final_vals.half(), f'./{save_file}')  # 将结果以半精度浮点类型保存到指定路径中
    torch.save(final_vals.half(), f'output/{save_file}')  # 将结果以半精度浮点类型保存到指定路径中
```

为了加快运行速度，这里使用`Joblib`中的`Parallel`和`delayed`实现并行化处理，执行`get_parallel_result`函数，代码如下：

注：`n_jobs`为开启的进程数，即是用`n_jobs`个CPU来计算。

```python
# 并行处理指定目录下的文件
Parallel(n_jobs=8)( #创建一个并行处理的上下文，指定使用 8 个工作线程。

    # 对每个文件路径调用 get_parallel_result 函数进行处理
    delayed(get_parallel_result)(file_path)
    #使用 delayed 函数将 get_parallel_result 函数及其参数 file_path 封装起来，表示要对每个文件路径调用 get_parallel_result(file_path) 进行处理
    
    # 遍历指定目录下以 .pt 为后缀的文件路径
    for file_path in tqdm(glob.glob(f'{path}/weather_round1_test/input/*.pt'))
    #使用glob.glob函数获取指定目录下所有以 .pt 结尾的文件路径，并使用 tqdm 函数在进度条中显示遍历的进度。
)

```

![](img/a12f37de254f1bcab99ee2737dd13a0c.png)

并行化处理提高了运行效率，试想下常规`for`循环大概得16*32分钟吧。

## 结果输出



```
#将output文件夹压缩，如果这段代码无法生成output.zip，运行下面的代码
_ = !zip -r output.zip output/
```

```
#如果上面代码无响应，运行下面这段代码
import zipfile, os
path = './output/'  # 要压缩的文件路径
zipName = 'output.zip'  # 压缩后的zip文件路径及名称

# 创建一个新的zip文件
f = zipfile.ZipFile(zipName, 'w', zipfile.ZIP_DEFLATED)
#使用zipfile模块创建一个新的zip文件对象，指定为写模式('w')并采用ZIP_DEFLATED压缩算法。

# 遍历指定路径下的所有文件和文件夹
for dirpath, dirnames, filenames in os.walk(path): #使用os.walk函数遍历指定路径下的所有文件和文件夹，包括子文件夹
    for filename in filenames: #遍历每个文件夹中的文件名。
        print(filename)
        # 将文件添加到zip文件中
        f.write(os.path.join(dirpath, filename))

# 关闭zip文件
f.close()
```

把`output.zip`下载到本地，打开[赛事主页](https://tianchi.aliyun.com/s/e09ba4f624e4678219c563c15f1bb5a2)，在**提交结果**初提交`output.zip`，稍等片刻评测成功，在**我的成绩**处可以看到成绩

![](img/42633bfcfba21958e484847ca54eb99d.png)

![](img/cd1bdf9d1c72bdbc274f238aa244f914.png)

## 完整代码

![](img/6c33bb7d5eb42857289d34a9a7d8be31.png)

[`weather_ml_baseline_update代码注释.ipynb`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/T6YubVgvAo50k5xQ9efcWgiqnuh)

# 任务3：更多优化方向

可以看到前面所使用的训练数据和模型参数设置，都是有所保留的，以及特征提取也没挖掘太多，还有很大的优化空间，具体可以是：

1. 扩大使用的训练数据，从10份扩展到20份、30份、50份；
2. 参数优化，学习率和迭代次数；
3. 特征优化，经纬度对应的变量可以进行聚合统计，比如横向、纵向，又或者相邻经纬的变量，统计方法如最大、最小、均值、中位数、方差等。

# 附录

## [QA文档](https://xqbbve5hpvs.feishu.cn/docx/DiooduxPHoCbv4xk5atc9Td5npe#EgbkdQYRhon3KwxMb5Xc1kuVnBh)

[Ai for science 赛道问题库](https://xqbbve5hpvs.feishu.cn/docx/DiooduxPHoCbv4xk5atc9Td5npe#EgbkdQYRhon3KwxMb5Xc1kuVnBh)

## 实践环境配置

### AI环境配置：

+ **视频讲解：**AI夏令营：手把手带你配置AI环境_哔哩哔哩_bilibili

+ **图文材料：**https://gitee.com/anine09/learn-python-the-smart-way-v2/blob/main/slides/chapter_0-Installation.ipynb

## 工具使用与提分思路

+ 竞赛实践路线：[竞赛实践路线分享](https://datawhaler.feishu.cn/docx/EJ2Edl0hXoIWwuxO15CcEj9Wnxn)

+ 时间序列预测方法总结：https://zhuanlan.zhihu.com/p/67832773
