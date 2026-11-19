
```python
import random
import math


class BiasSVD():
    def __init__(self, rating_data, F=5, alpha=0.1, lmbda=0.1, max_iter=100):
        self.F = F          # 这个表示隐向量的维度
        self.P = dict()     # 用户矩阵P  大小是[users_num, F]
        self.Q = dict()     # 物品矩阵Q  大小是[item_nums, F]
        self.bu = dict()    # 用户偏置系数
        self.bi = dict()    # 物品偏置系数
        self.mu = 0         # 全局偏置系数
        self.alpha = alpha  # 学习率
        self.lmbda = lmbda  # 正则项系数
        self.max_iter = max_iter        # 最大迭代次数
        self.rating_data = rating_data  # 评分矩阵

        for user, items in self.rating_data.items():
            # 初始化矩阵P和Q, 随机数需要和1/sqrt(F)成正比
            self.P[user] = [random.random() / math.sqrt(self.F) for x in range(0, F)]
            self.bu[user] = 0
            for item, rating in items.items():
                if item not in self.Q:
                    self.Q[item] = [random.random() / math.sqrt(self.F) for x in range(0, F)]
                    self.bi[item] = 0

    # 采用随机梯度下降的方式训练模型参数
    def train(self):
        cnt, mu_sum = 0, 0
        for user, items in self.rating_data.items():
            for item, rui in items.items():
                mu_sum, cnt = mu_sum + rui, cnt + 1
        self.mu = mu_sum / cnt

        for step in range(self.max_iter):
            # 遍历所有的用户及历史交互物品
            for user, items in self.rating_data.items():
                # 遍历历史交互物品
                for item, rui in items.items():
                    rhat_ui = self.predict(user, item)  # 评分预测
                    e_ui = rui - rhat_ui                                  # 评分预测偏差

                    # 参数更新
                    self.bu[user] += self.alpha * (e_ui - self.lmbda * self.bu[user])
                    self.bi[item] += self.alpha * (e_ui - self.lmbda * self.bi[item])
                    for k in range(0, self.F):
                        self.P[user][k] += self.alpha * (e_ui * self.Q[item][k] - self.lmbda * self.P[user][k])
                        self.Q[item][k] += self.alpha * (e_ui * self.P[user][k] - self.lmbda * self.Q[item][k])
            # 逐步降低学习率
            self.alpha *= 0.1


    # 评分预测
    def predict(self, user, item):
        return sum(self.P[user][f] * self.Q[item][f] for f in range(0, self.F)) + self.bu[user] + self.bi[
            item] + self.mu


# 通过字典初始化训练样本，分别表示不同用户（1-5）对不同物品（A-E)的真实评分
def loadData():
    rating_data={1: {'A': 5, 'B': 3, 'C': 4, 'D': 4},
           2: {'A': 3, 'B': 1, 'C': 2, 'D': 3, 'E': 3},
           3: {'A': 4, 'B': 3, 'C': 4, 'D': 3, 'E': 5},
           4: {'A': 3, 'B': 3, 'C': 1, 'D': 5, 'E': 4},
           5: {'A': 1, 'B': 5, 'C': 5, 'D': 2, 'E': 1}
          }
    return rating_data

# 加载数据
rating_data = loadData()
# 建立模型
basicsvd = BiasSVD(rating_data, F=10)
# 参数训练
basicsvd.train()
# 预测用户1对物品E的评分
for item in ['E']:
    print(item, basicsvd.predict(1, item))

# 预测结果：E 3.685084274454321
```

# 课后思考

## 算法改进探索

矩阵分解算法后续有哪些改进呢?针对这些改进，是为了解决什么的问题呢？请大家自行探索 `RSVD`，消除用户和物品打分偏差等。

## 矩阵分解的优缺点分析

### 优点

- 泛化能力强：一定程度上解决了稀疏问题
- 空间复杂度低：由于用户和物品都用隐向量的形式存放，少了用户和物品相似度矩阵，空间复杂度由 $n^2$ 降到了
- 更好的扩展性和灵活性：矩阵分解的最终产物是用户和物品隐向量，这个深度学习的 embedding 思想不谋而合，因此矩阵分解的结果非常便于与其他特征进行组合和拼接，并可以与深度学习无缝结合。

### 缺点

- 矩阵分解算法依然是只用到了评分矩阵，没有考虑到用户特征，物品特征和上下文特征，这使得矩阵分解丧失了利用很多有效信息的机会。
- 同时在缺乏用户历史行为的时候，无法进行有效的推荐。
- 为了解决这个问题，`逻辑回归模型及后续的因子分解机模型`，凭借其天然的融合不同特征的能力，逐渐在推荐系统领域得到了更广泛的应用。
