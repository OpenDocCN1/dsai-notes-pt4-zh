
``` 
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import StackingRegressor
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error
import lightgbm as lgb
from xgboost import XGBRegressor
from sklearn.neural_network import MLPRegressor

# 假设已有数据集 df
X = df.drop('target', axis=1)  # 特征列
y = df['target']  # 目标列

# 划分训练集和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 定义基模型
estimators = [
    ('lgb', lgb.LGBMRegressor(objective='regression')),
    ('xgb', XGBRegressor(objective='reg:squarederror')),
    ('mlp', MLPRegressor(hidden_layer_sizes=(50, 30), max_iter=500))
]

# 定义最终的meta-regressor
final_estimator = LinearRegression()

# 创建Stacking模型
stacking_regressor = StackingRegressor(estimators=estimators, final_estimator=final_estimator, cv=5)

# 模型训练
stacking_regressor.fit(X_train, y_train)

# 模型预测
y_pred = stacking_regressor.predict(X_test)

# 评估模型
mse = mean_squared_error(y_test, y_pred)
print(f'Test MSE: {mse:.4f}')

# 看每个单独模型的性能
for name, est in stacking_regressor.named_estimators_.items():
    y_pred_individual = est.predict(X_test)
    mse_individual = mean_squared_error(y_test, y_pred_individual)
    print(f'{name} Test MSE: {mse_individual:.4f}')
```

## 3. 混合学习

在解决复杂的生物信息学问题，如siRNA效果的预测中，机器学习与深度学习的混合方法可以提供强大的工具。这种混合方法可以结合深度学习的特征学习能力和传统机器学习模型的效率与解释性。以下是一种结合机器学习和深度学习的策略，包括两个主要部分：一个用于特征提取的深度学习模型（如卷积神经网络），以及一个传统的机器学习模型（如`LightGBM`）来进行最终的决策。这里提供一个示例，展示如何使用`PyTorch`构建深度学习部分，然后将输出特征传递给`LightGBM`进行分类或回归。

### 1. 构建`PyTorch`模型

首先，我们定义一个简单的卷积神经网络（CNN）来处理序列数据。这个模型将用于提取有用的特征。

``` 
import torch
import torch.nn as nn
import torch.optim as optim
from torch.utils.data import DataLoader, TensorDataset
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
import lightgbm as lgb
from sklearn.metrics import mean_squared_error

class RegressionCNN(nn.Module):
    def __init__(self, sequence_length):
        super(RegressionCNN, self).__init__()
        self.conv1 = nn.Conv1d(in_channels=1, out_channels=32, kernel_size=3, stride=1)
        self.relu = nn.ReLU()
        self.pool = nn.MaxPool1d(kernel_size=2)
        self.flatten = nn.Flatten()
        self.fc = nn.Linear(32 * ((sequence_length // 2) - 1), 100)  # Adjust size accordingly
        self.regressor = nn.Linear(100, 1)

    def forward(self, x):
        x = self.conv1(x)
        x = self.relu(x)
        x = self.pool(x)
        x = self.flatten(x)
        x = self.fc(x)
        x = self.regressor(x)
        return x

# 假设df是包含序列和目标值的DataFrame
sequence_length = 100  # 假定每个序列的长度
X = np.array([np.array(list(map(float, list(seq)))) for seq in df['sequence']])
X = X.reshape(X.shape[0], 1, sequence_length)
y = df['target'].values

# 划分数据集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
train_dataset = TensorDataset(torch.tensor(X_train, dtype=torch.float32), torch.tensor(y_train, dtype=torch.float32))
test_dataset = TensorDataset(torch.tensor(X_test, dtype=torch.float32), torch.tensor(y_test, dtype=torch.float32))

# 创建DataLoader
train_loader = DataLoader(train_dataset, batch_size=32, shuffle=True)
test_loader = DataLoader(test_dataset, batch_size=32, shuffle=False)

# 模型和优化器
model = RegressionCNN(sequence_length)
optimizer = optim.Adam(model.parameters(), lr=0.001)
criterion = nn.MSELoss()

# 训练模型
def train_model(model, train_loader):
    model.train()
    for batch_idx, (data, target) in enumerate(train_loader):
        optimizer.zero_grad()
        output = model(data)
        loss = criterion(output.view(-1), target)
        loss.backward()
        optimizer.step()

# 提取特征
def extract_features(model, loader):
    model.eval()
    features = []
    labels = []
    with torch.no_grad():
        for data, target in loader:
            output = model(data)
            features.extend(output.view(-1).numpy())
            labels.extend(target.numpy())
    return np.array(features), np.array(labels)

train_model(model, train_loader)
X_train_features, y_train = extract_features(model, train_loader)
X_test_features, y_test = extract_features(model, test_loader)
```

### 2. 使用`LightGBM`进行回归

在获取特征后，我们可以使用`LightGBM`进行回归预测。

``` 
# 使用LightGBM进行最终的预测
lgb_regressor = lgb.LGBMRegressor(n_estimators=100, learning_rate=0.05, max_depth=5)
lgb_regressor.fit(X_train_features.reshape(-1, 1), y_train)

# 预测和评估
y_pred = lgb_regressor.predict(X_test_features.reshape(-1, 1))
mse = mean_squared_error(y_test, y_pred)
print(f"Test MSE: {mse:.4f}")
```

## 4. 其他思路

+ **外部数据集**： 集成额外的生物信息学数据库来丰富特征，这可能有助于提高模型的泛化能力。（比赛可以使用，但是需要说明出处）

+ **特征选择**：实施自动化的特征选择流程，以剔除不重要的特征，减少模型的复杂性并避免过拟合。

+ **动态学习率调整**：除了基于性能衰退调整学习率外，还可以尝试使用学习率预热（`warm-up`）和循环学习率等策略，这可能有助于改进模型收敛速度和质量。

+ **多目标优化**：考虑将多个评价指标整合到模型的优化过程中，如同时优化`MAE`和`F1`，或者设计一个更全面的评价函数，以更好地反映siRNA的实验效果。

## 新模型构建

### 使用`attention`原理进行`end2end`的建模

1. 上一部分所述的通过特征工程进行上分的思路，都是从序列出发，基于生物学研究者在长期实践过程中积累的经验，对siRNA序列中的某些特征进行提取，以达到提高模型表现的目的。

2. 这种思路依然是传统的机器学习建模方法，在深度学习时代，AI研究者发现，基于大量的训练数据与端到端的数据模式提取算法，可以在大大减少建模过程中特征工程的复杂度的同时，提高模型的表现。最典型的代表案例就是卷积神经网络在图像领域的应用，以及循环神经网络和`attention`机制在自然语言领域的应用。

3. 由于本赛题中序列特征为信息量最丰富的模型特征，并且siRNA与target gene的序列信息关联对siRNA的沉默效率有直接的影响，因此可以将siRNA序列与target gene序列进行拼接，然后使用`attention`机制捕捉这两段序列内部，以及这两段序列之间与沉默效率相关的pattern。这种方法有望从数据驱动的视角，获得超出生物学家经验的新的siRNA序列模式以提高siRNA的设计能力，这才是AI for science的真正魅力所在。

### 使用基础模型辅助建模

1. 近年来，在图像与自然语言领域，大模型正在如火如荼的发展与应用。大模型的主要思路就是通过深度学习算法的大规模参数，从大规模数据中提取有效信息，从而获得超越人类专家的智能。

2. 生物学序列数据也有大量的数据积累，计算生物学家们也希望在生物领域做出能够理解生物语言的`chatGPT`，现在已经有一些工作对大规模DNA、RNA序列进行预训练，获得了生物学序列大模型，以支持下游的一系列应用，比如`LucaOne`、`hyenaDNA`、`caduceus`等。

3. 我们难以在比赛期间自己训练出一个生物序列大模型，但是可以站在巨人的肩膀上，充分利用已有的生物序列基础模型，生成siRNA和target gene序列的表征向量，然后这些包含更多生物学信息的表征向量输入模型，以提高预测效果。

4. 由于生物序列大模型的研发仍处于学术研究的阶段，并没有一个公认最有效的模型，所以建议大家多多进行文献搜索与尝试，挑选出最适合该任务的基础模型

# ❀ 恭喜看完全部知识教程！

<https://datawhaler.feishu.cn/sync/TdhvdKG8psUuznbIsOzcJsSon56>

## 📟 你可以随时回顾我们这次的学习~

<https://datawhaler.feishu.cn/sync/VzINd7TdxstuUkbAqWJcLhV6nMN>

## 🎉 合影留念墙

感谢很棒的你一路的坚持，让我们在这里给自己点个赞！

期待下期夏令营还能与你再次相遇~

一起合个影 :)

> 本教程主要由 、 、 写作贡献，
> 融合了多位助教的内测体验和宝贵建议。
> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~
>
> **注：** 本文采用 *署名—非商业性使用*—相同方式共享 4.0 协议国际版许可协议进行许可。

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！
