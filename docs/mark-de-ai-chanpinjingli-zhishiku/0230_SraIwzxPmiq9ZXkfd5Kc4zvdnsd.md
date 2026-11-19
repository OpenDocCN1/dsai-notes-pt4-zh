
# 10-06损失函数-均方根误差

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/ZZZ7dDDdGooSY2xjbVUc4rTunxd](https://qqs7y1hozd1.feishu.cn/docx/ZZZ7dDDdGooSY2xjbVUc4rTunxd)

假设你做了一个模型，预测明天的气温（单位：℃）：

| 天数 | 实际温度 | 模型预测温度 |
| --- | --- | --- |
| 1 | 25 | 26 |
| 2 | 27 | 25 |
| 3 | 30 | 29 |

我们希望知道模型到底“准不准”，也就是预测值和真实值之间差了多少。你可以：

1. 把每个预测值和实际值之间的“误差”（预测值减去真实值）算出来；
2. 为了不让正负误差互相抵消，我们把每个误差平方；
3. 把这些平方后的误差平均一下（就是均方误差 `MSE`）；
4. 然后再开平方（就是均方根误差 `RMSE`）。

## 均方误差（MSE）

假设有：

+ 一组样本数据，共有   个样本；
+ 对每个样本，模型的预测值为  ，真实值为  。

### 步骤

#### 第一步：误差（Error）

误差就是预测值和真实值之间的差距：

#### 第二步：平方误差（Squared Error）

为了避免正负抵消，把误差平方：

#### 第三步：均方误差 `MSE`（Mean Squared Error）

所有样本的平方误差求平均：

## 均方根误差

`RMSE` 是在 `MSE` 的基础上开平方根：

为什么要开平方？

因为平方误差单位是“平方单位”（比如温度是℃，那 `MSE` 单位是 ℃²），不好理解。

`RMSE` 再开平方，单位就和原始数据一样（比如℃），更直观。

## `RMSE` 和 `MSE` 本质含义

| 项目 | `MSE` | `RMSE` |
| --- | --- | --- |
| 是否平方？ | 是 | 是（然后再开平方） |
| 是否平均？ | 是 | 是 |
| 单位 | 原单位的平方（比如 m²） | 原单位（比如 m） |
| 敏感度 | 对大误差更敏感（平方放大效果） | 同样敏感，但更直观可解释 |

## 算法流程

我们用 `RMSE` 来作为损失函数，也就是模型训练中优化的目标：

训练流程简述：

1. 模型初始化：模型开始训练，有一组初始参数。
2. 预测输出：模型对训练集样本输出预测值  。
3. 计算损失：对每个样本，计算  ，然后求平均，开根号，得到 `RMSE`。
4. 反向传播：根据 `RMSE` 对模型参数进行微分（求导），用梯度下降方法来更新参数，使得 `RMSE` 越来越小。
5. 迭代训练：重复以上步骤，直到损失函数收敛，或满足某个停止条件。

均方根误差（`RMSE`）就是衡量预测值和真实值之间差距大小的一个工具，它先算误差的平方平均值，再开平方，把误差大小转换成跟原数据一样的单位，直观又精准。

## 完整案例

我们以一个预测“房价”（简化为一维特征）为例，生成合成数据，使用梯度下降算法最小化均方根误差（`RMSE`），并可视化训练曲线和预测效果。

```
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split

# 1. 生成合成数据
np.random.seed(42)
X = 2 * np.random.rand(200, 1)
y = 4 + 3 * X + np.random.randn(200, 1) * 0.5  # 添加噪声

# 拆分训练集和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 2. 初始化参数
theta = np.random.randn(2, 1)  # θ0 和 θ1
learning_rate = 0.1
n_epochs = 1000
m = len(X_train)

# 3. 添加 X0 = 1 列
X_b = np.c_[np.ones((m, 1)), X_train]

# 记录每次迭代的损失
rmse_history = []

# 4. 梯度下降
for epoch in range(n_epochs):
    y_pred = X_b.dot(theta)
    error = y_pred - y_train
    rmse = np.sqrt(np.mean(error**2))
    rmse_history.append(rmse)
    
    gradients = 2/m * X_b.T.dot(error)
    theta = theta - learning_rate * gradients

# 5. 在测试集上评估
X_test_b = np.c_[np.ones((len(X_test), 1)), X_test]
y_test_pred = X_test_b.dot(theta)
test_rmse = np.sqrt(np.mean((y_test_pred - y_test)**2))

# 可视化：训练过程中的 RMSE 变化
plt.figure(figsize=(8, 5))
plt.plot(rmse_history, label='Training RMSE', color='crimson', linewidth=2)
plt.xlabel('Epoch', fontsize=12)
plt.ylabel('RMSE', fontsize=12)
plt.title('RMSE vs Epochs', fontsize=14)
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

# 可视化：测试集预测 vs 真实值
plt.figure(figsize=(8, 5))
plt.scatter(X_test, y_test, label='实际值', color='teal', s=50, alpha=0.7)
plt.plot(X_test, y_test_pred, label='预测值', color='orange', linewidth=2)
plt.xlabel('Feature X', fontsize=12)
plt.ylabel('Target y', fontsize=12)
plt.title('Actual vs Predicted', fontsize=14)
plt.legend()
plt.grid(True)
plt.tight_layout()
plt.show()

# 输出参数和测试 RMSE
print(f"训练得到参数 θ0 = {theta[0,0]:.4f}, θ1 = {theta[1,0]:.4f}")
print(f"测试集 RMSE = {test_rmse:.4f}")
```

### 1. 生成合成数据

+ `np.random.seed(42)`：固定随机种子，保证可重复性。
+ 构造 200 个一维特征 `X`，范围在   上均匀分布。
+ 目标值  ，其中噪声服从标准正态分布并放大 0.5。

### 2. 拆分训练集和测试集

+ 使用 `train_test_split` 按 8:2 的比例随机划分训练集和测试集。

### 3. 初始化参数

+ `theta` 是一个   的列向量，对应截距   和斜率  。
+ `learning_rate`（学习率）设为 0.1，`n_epochs`（迭代次数）设为 1000。
+ `m` 是训练集样本数，用于计算梯度和损失。

### 4. 添加偏置项

+ 在训练特征矩阵最左侧拼接一列全 1，得到  ，这样矩阵运算时就能同时更新截距和斜率。

### 5. 梯度下降迭代

每次迭代中：

+ 预测值  。
+ 计算误差  ，再求 `RMSE`：

+ 将当前 `RMSE` 存入 `rmse_history`，以便后续可视化。

+ 计算梯度：

+ 更新参数：

### 6. 在测试集上评估

+ 同样在测试集拼接偏置项，计算预测值，并计算最终的 `test_rmse`。

### 7. 可视化

+ `RMSE` vs Epochs：展示训练过程中 `RMSE` 随迭代次数的变化趋势。

![](img/137247ad0642b0fd068de84503076188.png)

+ `Actual vs Predicted`：散点图展示实际值（teal 颜色）和回归直线（orange 颜色）的关系。

![](img/1e371000eec5b5b97970d5c614030007.png)

### 8. 输出结果

+ 打印最终训练得到的参数   以及测试集上的 `RMSE`。

## 模型分析

### 优缺点分析

优点：

+ 凸函数：`MSE` 对应的 `RMSE` 依然是可导的凸函数，梯度下降能收敛到全局最优。
+ 强调大误差：平方操作对大误差“惩罚”更大，能促使模型尽可能减小大偏差。
+ 易于数学推导：在解析解（正规方程）或梯度推导时非常简洁。

缺点：

+ 对离群值敏感：因为平方放大了大误差，若数据中存在离群点，会严重影响模型。
+ 可解释性：`MSE` 的单位是原单位的平方，虽然 `RMSE` 解决单位问题，但仍不如 `MAE`（绝对误差）直观。
+ 梯度较大：在极端误差下，梯度过大可能导致梯度爆炸，需要更谨慎地选择学习率。

### 与相似算法的对比

| 损失函数 | 优点 | 缺点 |
| --- | --- | --- |
| `MAE`（Mean Absolute Error） | 对离群点不敏感；线性惩罚，单位直观 | 不可导点处（误差为零点）导致优化稍复杂 |
| `MSE` / `RMSE` | 数学推导简单；凸且可导；强调大误差 | 对离群点敏感；单位为平方（需开根号） |
| `Huber Loss` | 对小误差表现同 `MSE`，对大误差同 `MAE`；鲁棒性 | 需要调节阈值参数 |
| `Log-Cosh Loss` | 平滑近似 `MAE`；对离群点适度惩罚 | 计算较 `MSE` 稍复杂 |

### 何时优选 `RMSE` / `MSE`，何时考虑其他算法

适用场景：

+ 数据分布大致满足高斯分布，离群值较少。
+ 需要对大误差进行强惩罚，以提升整体预测精度。
+ 模型训练需解析解或梯度下降，享受凸优化带来的全局最优保证。

考虑其他算法的场景：

+ 数据中包含显著离群点：可选 `MAE` 或 `Huber Loss`，以减少离群点影响。
+ 需要对所有误差线性处理，不想过度放大大偏差：`MAE` 更直观。
+ 任务对小误差和大误差的惩罚策略要分段控制：`Huber Loss` 按需设置阈值。
