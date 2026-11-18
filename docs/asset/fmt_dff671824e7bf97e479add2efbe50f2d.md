
```
import numpy as np
from collections import Counter
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
import graphviz

# 1. 辅助函数：计算熵、信息增益、固有值、信息增益率
def entropy(y):
    """计算熵：H(S) = - Σ (p * log2(p))"""
    counts = Counter(y)
    total = len(y)
    ent = 0.0
    for count in counts.values():
        p = count / total
        ent -= p * np.log2(p + 1e-9)
    return ent

def split_dataset(X, y, feature_index, threshold):
    """
    对连续型特征按照阈值进行划分，返回左右子集：
    left: 特征值 <= threshold; right: 特征值 > threshold
    """
    mask = X[:, feature_index] <= threshold
    return X[mask], y[mask], X[~mask], y[~mask]

def info_gain_ratio(X, y, feature_index, threshold):
    """
    计算给定特征与阈值的分裂的信息增益率：
    1. 计算划分前的熵 H(S)
    2. 按阈值划分，计算条件熵 H(S|A)
    3. 计算信息增益 IG = H(S) - H(S|A)
    4. 计算固有值 SI
    5. 返回增益率 GR = IG / SI
    """
    # 1. 总熵
    H = entropy(y)
    # 划分数据集
    X_left, y_left, X_right, y_right = split_dataset(X, y, feature_index, threshold)
    n = len(y)
    n_left, n_right = len(y_left), len(y_right)
    if n_left == 0 or n_right == 0:
        # 如果某一边为空，则无法划分，返回增益率为0
        return 0, None, None, None
    # 2. 条件熵
    H_cond = (n_left / n) * entropy(y_left) + (n_right / n) * entropy(y_right)
    # 3. 信息增益
    ig = H - H_cond
    # 4. 固有值
    si = - (n_left / n) * np.log2(n_left / n + 1e-9) - (n_right / n) * np.log2(n_right / n + 1e-9)
    # 5. 增益率（防止 si 为 0）
    gr = ig / si if si > 1e-9 else 0
    return gr, ig, threshold, (X_left, y_left, X_right, y_right)

# 2. 定义决策树结构（C4.5 树）
class TreeNode:
    def __init__(self, feature_index=None, threshold=None, left=None, right=None, *, value=None):
        """
        如果是叶节点，则 value 不为 None，表示预测的类别；
        如果是非叶节点，则记录划分的特征索引和阈值，并链接左右子树。
        """
        self.feature_index = feature_index
        self.threshold = threshold
        self.left = left
        self.right = right
        self.value = value  # 叶节点类别

def majority_class(y):
    """返回 y 中出现最多的类别"""
    return Counter(y).most_common(1)[0][0]

# 3. 构建 C4.5 决策树（递归生成树结构）
def build_tree(X, y, feature_indices, min_samples_split=10, max_depth=10, depth=0):
    """
    参数：
        X: 特征矩阵
        y: 标签
        feature_indices: 待选特征下标（本例中所有特征均可用）
        min_samples_split: 如果样本数小于该值则停止划分（预剪枝策略）
        max_depth: 树最大深度
        depth: 当前递归深度
    返回：
        TreeNode 对象（决策树节点）
    """
    # 如果样本完全属于同一类别或满足停止条件
    if len(set(y)) == 1 or len(y) < min_samples_split or depth >= max_depth:
        return TreeNode(value=majority_class(y))

    best_feature = None
    best_threshold = None
    best_gain_ratio = -1
    best_split = None

    # 遍历每个特征，寻找最优分裂（对于连续值特征，遍历所有候选阈值）
    for feature_index in feature_indices:
        feature_values = X[:, feature_index]
        # 获取排序后的唯一值作为候选划分点（采用相邻中点法）
        sorted_vals = np.sort(np.unique(feature_values))
        if len(sorted_vals) == 1:
            continue
        candidate_thresholds = (sorted_vals[:-1] + sorted_vals[1:]) / 2.0
        for threshold in candidate_thresholds:
            gr, ig, th, split = info_gain_ratio(X, y, feature_index, threshold)
            if gr > best_gain_ratio:
                best_gain_ratio = gr
                best_feature = feature_index
                best_threshold = threshold
                best_split = split

    # 如果没有有效划分，则返回叶节点
    if best_gain_ratio <= 0 or best_split is None:
        return TreeNode(value=majority_class(y))

    # 分裂数据集
    X_left, y_left, X_right, y_right = best_split
    # 递归构造左右子树
    left_subtree = build_tree(X_left, y_left, feature_indices, min_samples_split, max_depth, depth + 1)
    right_subtree = build_tree(X_right, y_right, feature_indices, min_samples_split, max_depth, depth + 1)

    return TreeNode(feature_index=best_feature, threshold=best_threshold,
                    left=left_subtree, right=right_subtree)

# 4. 预测函数

def predict_sample(node, sample):
    """对单一样本递归预测类别"""
    if node.value is not None:
        return node.value
    if sample[node.feature_index] <= node.threshold:
        return predict_sample(node.left, sample)
    else:
        return predict_sample(node.right, sample)

def predict(tree, X):
    """对样本矩阵 X 进行预测"""
    return np.array([predict_sample(tree, sample) for sample in X])

# 5. 可视化决策树

def visualize_tree(node, feature_names, class_names, dot=None, node_id=0):
    """
    递归构造 graphviz 的 dot 对象，用于可视化树结构。
    """
    if dot is None:
        dot = graphviz.Digraph()

    # 当前节点标签
    if node.value is not None:
        label = f"Leaf\nClass: {class_names[node.value]}"
        dot.node(str(node_id), label, shape='box', style='filled', color='lightgrey')
    else:
        label = f"{feature_names[node.feature_index]} <= {node.threshold:.2f}?"
        dot.node(str(node_id), label)
        # 递归构造左右子树
        left_id = node_id * 2 + 1
        right_id = node_id * 2 + 2
        dot.edge(str(node_id), str(left_id), label="Yes")
        dot.edge(str(node_id), str(right_id), label="No")
        visualize_tree(node.left, feature_names, class_names, dot, left_id)
        visualize_tree(node.right, feature_names, class_names, dot, right_id)
    return dot

# 6. 案例演示：使用 Iris 数据集
if __name__ == "__main__":
    # 加载数据集
    iris = load_iris()
    X = iris.data
    y = iris.target
    feature_names = iris.feature_names
    class_names = iris.target_names

    # 划分训练集和测试集（例如 70%/30%）
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

    # 构造 C4.5 决策树
    # 使用预剪枝参数：最小样本数 5，最大深度 5（实际可调）
    tree = build_tree(X_train, y_train, feature_indices=list(range(X.shape[1])),
                      min_samples_split=5, max_depth=5)

    # 对测试集进行预测，并计算准确率
    y_pred = predict(tree, X_test)
    accuracy = np.mean(y_pred == y_test)
    print("Test Accuracy: {:.2f}%".format(accuracy * 100))

    # 7. 可视化决策树
    dot = visualize_tree(tree, feature_names, class_names)
    dot.render("c45_tree", view=True)
```

## 算法原理与分裂选择

代码通过计算熵、条件熵及信息增益率来确定最佳划分点。对于连续特征，采用候选阈值（相邻数值中点）进行遍历，并选择信息增益率最大的作为划分依据。这样避免了单纯依赖信息增益而产生的多值偏好问题。

## 预剪枝优化

我们设置了 `min_samples_split` 和 `max_depth` 参数，限制了树的生长。当样本数过少或树的深度超过预设值时，直接生成叶节点，从而防止过拟合。

![](img/db0978acc86986665e8320687fc4c213.png)

利用 `graphviz` 库将构建的决策树生成 PDF 文件，直观展示各节点的决策规则（特征与阈值）和叶节点的分类结果，便于分析模型结构。

## 进一步优化方向

+ **排序缓存**：对于连续特征，每次递归时可保存排序结果，提高分裂效率。

+ **熵计算缓存**：对于重复出现的子集可考虑缓存熵计算结果。

+ **交叉验证**：通过交叉验证来选择剪枝参数，进一步提高模型泛化能力。

+ **并行计算**：当数据集较大时，可以利用并行计算提升分裂候选阈值的搜索速度。

这个示例从数据加载、模型构建、预测评估到树结构可视化，展示了一个较为完整的 C4.5 算法实现案例，并通过预剪枝和代码结构提示了算法优化的方向，适合初学者和有一定基础的读者深入理解该算法。

## 模型分析

### 1. C4.5 模型的优缺点

#### 优点

+ **处理连续变量和离散变量**：C4.5 能够有效地处理连续型特征（通过候选划分点）以及离散型特征，使其在实际问题中具有更强的适应性。

+ **处理缺失值**：模型在构建树的过程中能够利用概率分布对缺失数据进行处理，减少了因缺失值导致的信息丢失。

+ **信息增益率选择划分特征**：通过引入固有值，避免了 ID3 算法中偏向取值较多特征的问题，使得特征选择更加平衡。

+ **剪枝机制**：通过后剪枝或者预剪枝策略，有助于降低模型复杂度，避免过拟合，提高泛化能力。

#### 缺点

+ **计算复杂度较高**：尤其在处理连续变量时，需要遍历候选阈值，对于大数据量或者高维数据，计算量会显著增大。

+ **对噪声数据敏感**：像大多数决策树模型一样，C4.5 对于异常值和噪声数据可能会受到一定影响，导致决策边界偏差。

+ **生成的树较大、结构复杂**：在实际应用中，树的深度和节点数可能会较多，模型可解释性下降，且存储和运算成本上升。

### 2. 与其他决策树算法的对比

下面的表格对比了 C4.5 与 ID3 和 CART 三种常见决策树算法的主要特点：

| 特性 | ID3 | C4.5 | CART |
| --- | --- | --- | --- |
| **处理数据类型** | 仅能处理离散型数据 | 能同时处理连续型和离散型数据 | 同样能处理连续型和离散型数据 |
| **特征选择准则** | 信息增益 | 信息增益率（考虑固有值） | 基尼指数（Gini Index） |
| **缺失值处理** | 不支持缺失值 | 能够处理缺失值，通过概率分布进行处理 | 部分实现，可采用数据预处理方式 |
| **剪枝策略** | 无（容易过拟合） | 后剪枝或预剪枝 | 后剪枝，通常使用代价复杂度剪枝（Cost Complexity Pruning） |
| **计算复杂度** | 较低 | 较高（尤其在连续值候选分割上） | 较高，但通常针对二叉树分裂 |
| **模型输出** | 多叉树 | 多叉树 | 二叉树 |

### 3. 应用场景讨论

#### 优选情况

+ **数据特征多样化**：当数据集中既包含连续型变量又有离散型变量，且数据中可能存在缺失值时，C4.5 能够较好地处理这些情况，因此较为合适。
