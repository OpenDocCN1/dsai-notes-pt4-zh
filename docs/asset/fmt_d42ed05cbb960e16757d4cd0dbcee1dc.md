
# Task3：进阶上分方向讨论

> 来源：`https://datawhaler.feishu.cn/docx/Z2QIddwEaoISRfxOooWcDSJQnyG`

> **教程贡献者说：**

> Task2使用探索性数据分析详细挖掘了赛题电力数据背后的模式，并给出了一系列的上分点（节假日、外部数据……）

> 在本节中，我们将探讨电力现货市场价格预测的前沿方法，并补充相关的知识，帮助大家更好地找到自己迭代上分的方向。

> 本节设置了三个主要的上分方向，大家可以自由尝试 []~(￣▽￣)~：

+ 时间序列挖掘（较容易）

+ ABM报价策略优化（难度中等偏上）

+ 强化学习（难度高）

## 进阶上分方向

### 时间序列挖掘

Task2中我们对时间序列做了详细的EDA，在这里我们将提供一个简单的时间序列`Baseline`[从零入门AI for Science（AI+经济）](https://datawhaler.feishu.cn/wiki/MkHkwQKEZiylVHkmFu8c17xgnbc?table=tblu7vcq7MqL7GC1&view=vewPZpUsYn#doxcnrr97CJxnZ2hHQ5w0ZIxDnv)作为参考，并提供详细的流程解释。可以下载下方的文件按类似Task1中的流程 就可以在`Modelscope`平台中跑通。

![](img/5456976ae2672e9472e54d200a38d25b.png)

[`Task3_Baseline.ipynb`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FaQkbYj3VoOme1xZr4PcxBewnSg)

[`sample_submit.csv`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZOegbsyJ8oadClxI4Xvcehnynyd)

![](img/a098d9ab853836af812c8bc865699f34.png)

[`electricity_price_parsed.csv`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WgqWbHp3WowUnvxbjvycBt4Fnic)

接下来我们来详细解读一下`Baseline2`

#### 导入包和读取数据（同`Baseline1`）

#### 特征工程

特征工程是数据预处理过程的一部分，涉及从原始数据中提取和创建新特征，以提高机器学习模型的性能。特征工程的目标是通过选择和构建合适的特征，使模型能够更好地理解数据和预测目标变量。

一般而言，我们可以构造如下时间序列特征：

+ 时间戳特征：年、月、日……

+ 历史信息特征：过去n期同日期的均值、标准差……

+ 滞后特征：将数据平移n期，例如平移7期就是用上周同时间的出清价作为参考（但本案例中预测区间过长，无法用到该特征，除非使用迭代预测）

+ 滑窗特征：历史7天、14天……均值、标准差、最大值、最小值、变异系数、峰度、偏度……

+ 周期特征：将年月日等时间特征转为正余弦函数

+ 异常值特征：例如在一定滑窗下用3标准差原则发现总负荷的异常值作为特征

+ 节假日特征与外部数据：节假日与天气数据在本案例中有很大的影响

详细的时间序列特征工程构造方法可以参考[Task3：尝试使用深度学习方案](https://datawhaler.feishu.cn/wiki/G8WywyV8iiYtuGkVjoOchdjxnib)二期Datawhale电力预测夏令营：

```
train_data["hour"] = electricity_price.index.hour
train_data["day"] = electricity_price.index.day
train_data["month"] = electricity_price.index.month
train_data["year"] = electricity_price.index.year
train_data["weekday"] = electricity_price.index.weekday
# 根据月份信息，判断是否为风季（1-5月和9-12月），创建布尔型 "is_windy_season" 列
train_data["is_windy_season"] = electricity_price.index.month.isin([1, 2, 3, 4, 5, 9, 10, 11, 12])
# 根据小时信息，判断是否为低谷时段（10-15点），创建布尔型 "is_valley" 列
train_data["is_valley"] = electricity_price.index.hour.isin([10, 11, 12, 13, 14, 15])
train_data["quarter"] = electricity_price.index.quarter
# 对时间特征进行独热编码（One-Hot Encoding），删除第一列以避免多重共线性
train_data = pd.get_dummies(
    data=train_data,        # 需要进行独热编码的 DataFrame
    columns=["hour", "day", "month", "year", "weekday"],  # 需要独热编码的列
    drop_first=True         # 删除第一列以避免多重共线性
)
```

基于Task2中的EDA，我们发现节假日中电力价格更容易为负值，因此我们构造典型的春节和劳动节的节假日特征。当然你也可以尝试构造其他的节假日特征（端午节、清明节、国庆节……）

> 可以使用`python`的`holidays`库来构造节假日特征，但考虑到其给出的日期范围不够准确，所以这里进行了手动构造

```
def generate_holiday_dates(start_dates, duration):
    holidays = []  # 初始化一个空列表，用于存储节假日日期
    for start_date in start_dates:  # 遍历每个节假日的开始日期
        # 生成从 start_date 开始的日期范围，持续时间为 duration 天
        holidays.extend(pd.date_range(start=start_date, periods=duration).tolist())
    return holidays  # 返回所有节假日日期的列表

spring_festival_start_dates = ["2022-01-31", "2023-01-21", "2024-02-10"]
labor_start_dates = ["2022-04-30", "2023-04-29"]
spring_festivals = generate_holiday_dates(spring_festival_start_dates, 7)
labor = generate_holiday_dates(labor_start_dates, 5)

# 判断训练数据的索引是否在春节日期列表中，生成布尔型列 "is_spring_festival"
train_data["is_spring_festival"] = train_data.index.isin(spring_festivals)

# 判断训练数据的索引是否在劳动节日期列表中，生成布尔型列 "is_labor"
train_data["is_labor"] = train_data.index.isin(labor)

```

可以发现一段时间内，总负荷如果有下降趋势，则下一个出清价格也可能下降。因此联想到构造基于总需求的窗口特征。

![](img/feea8baaa2e4e7da77ac4ffa800211a9.png)

在这里我们利用到了`pandas`非常好用的`agg`聚合函数，其可以将一列根据选择的函数转换为统计值（例如取一列标准差、均值），这里由于是对每个窗口`agg`，相当于取每个窗口的特定统计值。

在`agg`函数定义中，参数接受一列值（为`pandas`的`Series`），做某种计算后返回单个值。

```
from tqdm import tqdm  # 导入 tqdm 库用于显示进度条

# 定义滚动窗口大小的列表
window_sizes = [4, 12, 24]

# 遍历每个窗口大小
with tqdm(window_sizes) as pbar:
    for window_size in pbar:
        # 定义要应用的聚合函数列表
        functions = ["mean", "std", "min", "max", cal_range, increase_num,
                     decrease_num, increase_mean, decrease_mean, increase_std, decrease_std]

        # 遍历每个聚合函数
        for func in functions:
            # 获取函数名称，如果是字符串则直接使用，否则使用函数的 __name__ 属性
            func_name = func if type(func) == str else func.__name__

            # 生成新列名，格式为 demand_rolling_{window_size}_{func_name}
            column_name = f"demand_rolling_{window_size}_{func_name}"

            # 计算滚动窗口的聚合值，并将结果添加到 train_data 中
            train_data[column_name] = train_data["demand"].rolling(
                window=window_size,        # 滚动窗口大小
                min_periods=window_size//2,  # 最小观测值数
                closed="left"         # 滚动窗口在左侧闭合
            ).agg(func)              # 应用聚合函数

            pbar.set_postfix({"window_size": window_size, "func": func_name})

```

我们还可以加入一些其他的时序特征，例如`demand`的滞后特征，差分特征，百分比特征等。通常我们会尝试构造尽可能多的特征，随后在特征筛选中留下对结果有显著影响的特征。

```
# 添加新的特征列：demand_shift_1，表示将 demand 列中的值向后移动一位
train_data["demand_shift_1"] = train_data["demand"].shift(1)
# 添加新的特征列：demand_diff_1，表示 demand 列中相邻值的差
train_data["demand_diff_1"] = train_data["demand"].diff(1)
# 添加新的特征列：demand_pct_1，表示 demand 列中相邻值的百分比变化
train_data["demand_pct_1"] = train_data["demand"].pct_change(1)
```

### 模型训练

一般数据科学比赛中，线性模型和树模型是比较常用的两种回归模型，在这里我们将线性回归和`LightGBM`作为Baseline的两个模型，并不进行特别的超参数调优（可以参考如下链接学习如何调整`LightGBM`的超参数）。

可以尝试其他模型例如：

+ 基于深度学习的方法：`LSTM`、`NBeats`、`Transformer`……

+ 基于时序模型的方法：`ARIMA`、`Prophet`、`VARMAX`……

+ 其他线性模型：`Ridge`、`ElasticNet`、`Lasso`……

+ 其他树模型：`CatBoost`、`XGBoost`、`LightGBM`

```
X_train = train_data.iloc[:train_length].drop(columns=["price"]).bfill().ffill()

# 创建测试集特征数据 X_test
X_test = train_data.iloc[train_length:].drop(columns=["price"]).bfill().ffill()

# 创建训练集目标数据 y_train
y_train = train_data.iloc[:train_length][["price"]]

from sklearn.linear_model import LinearRegression
from lightgbm import LGBMRegressor
lgb_model = LGBMRegressor(num_leaves=2**5-1, n_estimators=300, verbose=-1)
linear_model = LinearRegression()
# 使用训练集数据训练 LGBMRegressor 模型
lgb_model.fit(X_train, y_train)
# 使用训练集数据中的 "demand" 特征训练线性回归模型
linear_model.fit(X_train[["demand"]], y_train)
lgb_pred = lgb_model.predict(X_test)
# 返回预测的目标值，并将结果展平为一维数组
linear_pred = linear_model.predict(X_test[["demand"]]).flatten()
```

### 模型融合

考虑到`demand`对价格的线性程度高，但直接线性回归无法捕捉到时序中的一些非线性特征，因此我们融合树模型和线性模型的结果，由于我们没有其他先验信息，选择直接取均值。

> 还记得我们在Task2中提到，由于鸭子曲线，火电厂在低谷期时的价格会越来越低，并且整体火电价格也会下降，因此我们可以对最终预测结果进行一个整体上的缩放，保证符合未来的趋势。

> ps：在本次赛题数据中，线下验证的效果和线上验证效果差别非常大，数据科学比赛常用的trust cv反而起反效果。思考当测试集分布和训练集分布不一致时，应该如何优化自己的模型。

成功后就可以提交到官网啦：`http://competition.sais.com.cn/competitionDetail/532232/mySubmissions`

最终结果应该是`11302.0459`左右，当然你可以尝试加入线下验证来寻找更优的模型融合和系数修正方法。

```
# 简单求均值
y_pred = (lgb_pred+linear_pred)/2
y_pred *= 0.95  # 进行少量修正
sample_submit["clearing price (CNY/MWh)"] = y_pred
sample_submit.to_csv("submit.csv", index=False)
```

## ABM报价策略优化

在Task1中，我们提到了智能体可以采用“边际成本”来作为自己的报价，但在现实中，火电机组会有更加复杂的报价策略，在这里我们进行详细的讨论。

### 报价机制

`Baseline1`中我们假定了智能体的报价只有一个值，即其最大出力（容量），但思考这样一个场景，假如某机组最大出力是600MW，都报350元，而市场以345元出清了，机组因为5元之差没能中标，损失了全部电力。

而现在这个机组学聪明了，为了降低风险，他选择前300MW出力报300元，后300W出力报400元，这样即使最终价格在300-400之间，他保底能卖出300MW的出力。

这就是现实中的机组报价方式：分段报价。可以类比股票中的金字塔加仓策略，机组不会把所有出力都报一样的价格，而是随着出力升高报更高的价格

![](img/13de6e646946050424570c6eed325db2.png)

![](img/df3e9aca7653c7d92fdd68fe54509020.png)

### 报价策略

在`Baseline1`中，机组总是以边际成本（即边际煤耗）报价，即

+ ：`unit`数据中的`power consumption rate (%)`

+ ：单位标煤价，需要利用外部数据获取

+ ：边际煤耗量，即数据中的`coal consumption (g coal/KWh)`
