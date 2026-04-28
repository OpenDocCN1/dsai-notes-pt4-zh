# 03-09集成算法-梯度提升

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/M87ldnncLoKLm1x90v7cEdVmnSd](https://qqs7y1hozd1.feishu.cn/docx/M87ldnncLoKLm1x90v7cEdVmnSd)

当你在做选择题，不确定答案，你就去问一群同学，**然后“听**大多数人的意见”来选答案。这就是集成算法。它的核心思路就是：多个模型联合起来投票、协作，做出更好的决策。

梯度提升就像你一个人做题做得不太好，但你不放弃，每次找出错在哪，然后请“更聪明的小助手”专门来帮你改正错误，下一次做得更好。你一次次改进，最后整体的结果就越来越棒了！

#### 梯度提升的工作方式是这样的

1.  先训练一个很简单的模型（比如一个弱小的决策树），它尝试去预测结果。

1.  这个模型肯定不完美，所以我们就看它“错在哪了”（就是它预测值和实际值的差距）。

1.  然后训练第二个模型，目标是“专门去修补前一个模型的错误”。

1.  不断重复这个过程：每次都让新的模型去“补前面所有模型犯的错”。

1.  最终，把所有这些模型的预测“加起来”，就能得到一个超强预测结果！

这就好像你每次考试都反思错题，然后请不同老师来专门教你不会的部分。

经过很多轮，你的整体成绩就突飞猛进了。

## 原理详解

### 数学原理

#### 问题设定（监督学习）

我们希望通过一个模型  去拟合输入  与输出  之间的关系。

目标是最小化损失函数（Loss Function）：

在实际中我们是给定样本  ，变成经验风险最小化：

### 模型形式

我们使用加法模型（Additive Model）：

：最终的模型

：第  个“弱学习器”模型（通常是决策树）

：每个学习器的权重（学习率）

：总的迭代轮数（学习器数量）

模型是“逐步构建的”，每一步往模型里加一个新的学习器。

### 逐步优化思想

我们一步一步地构建模型，每一步都选择一个新的弱学习器  和对应系数  ，使得损失尽可能下降：

**第 ** **步优化目标：**

解释：

+    是前  个弱学习器构成的模型

+   新的  是为了进一步降低总损失

**但这个优化不容**易直接做，我们引入 梯度下降的思想！

### 引入梯度下降

**我们对损**失函数进行一阶泰勒展开（也就是用梯度近似）：

设：

那么我们就可以近似成：最小化负梯度方向的误差

> 梯度提升的核心思想：每一步都用一个新模型去拟合损失函数在当前模型下的负梯度！

### 梯度提升算法流程（细化步骤）

我们以回归问题为例（损失函数是平方误差）：

#### **初始化：**

对平方误差来说，就是：

#### 对于每一轮  到  ：

**第一步：计算残差（负梯度）**

对于平方误差  ，有：

**第二步：拟合残差**

用一个弱学习器  去拟合这些残差：

也可以理解为“向残差方向修正”。

**第三步：确定步长 **

对于平方误差来说，这就是最小二乘问题。

**第四步：更新模型**

其中  是学习率（通常很小，如 0.1 或 0.01），用于控制每一步更新的幅度，防止过拟合。

#### 举例：平方误差下的推导过程

平方误差：

梯度是：

+   残差就是当前模型与真实值的差值

+   每轮就是“拟合残差 + 加入模型”

#### 扩展：分类任务中的梯度提升

对于二分类，损失函数可以是 **对数损失（Log Loss）**：

这时每一轮我们依然计算负梯度：

后续的流程相同，只是损失函数和梯度不同。

## 完整案例

咱们这里使用Scikit-learn内置的“Breast Cancer Wisconsin”数据集，该数据集包含了乳腺癌患者的各项特征数据，通过分类任务预测患者是否患有恶性肿瘤。

我们利用梯度提升算法构建模型，并通过GridSearchCV进行超参数调优，最终使用多种指标对模型进行评估和可视化。

### 数据预处理

首先，我们导入所需的库，并对数据集进行加载、预处理和初步探索。数据预处理包括数据标准化、数据集拆分等。探索性数据分析（EDA）能够帮助我们了解数据分布、特征相关性，为后续模型调参提供依据。

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.datasets import load_breast_cancer
from sklearn.model_selection import train_test_split, GridSearchCV
from sklearn.preprocessing import StandardScaler
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.metrics import confusion_matrix, classification_report, roc_curve, auc

import warnings
warnings.filterwarnings(&#x27;ignore&#x27;)

# 加载数据
data = load_breast_cancer()
df = pd.DataFrame(data.data, columns=data.feature_names)
df[&#x27;target&#x27;] = data.target

# 输出数据集的基本信息
print(&quot;数据集样本数：&quot;, df.shape[0])
print(&quot;特征数量：&quot;, df.shape[1]-1)
print(&quot;类别分布：\n&quot;, df[&#x27;target&#x27;].value_counts())

# 分析各特征的分布情况
plt.figure(figsize=(12, 6))
sns.histplot(df[&#x27;mean radius&#x27;], kde=True, color=&#x27;darkorange&#x27;)
plt.xlabel(&#x27;mean radius&#x27;, fontsize=12)
plt.title(&#x27;Distribution of Mean Radius&#x27;, fontsize=14)
plt.show()

# 分析特征间的相关性，绘制热力图
plt.figure(figsize=(14, 10))
corr_matrix = df.drop(&#x27;target&#x27;, axis=1).corr()
sns.heatmap(corr_matrix, cmap=&#x27;viridis&#x27;, annot=False)
plt.xlabel(&#x27;Features&#x27;, fontsize=12)
plt.ylabel(&#x27;Features&#x27;, fontsize=12)
plt.title(&#x27;Feature Correlation Matrix&#x27;, fontsize=14)
plt.show()
```

1.  数据加载后可见共有569个样本，30个特征以及1个目标变量。目标变量0代表恶性肿瘤，1代表良性肿瘤（数据集的具体含义请参考数据说明）。

1.  绘制“mean radius”的分布图有助于观察该特征是否呈现正态分布，便于判断是否需要进行数据转换或标准化。

![](img/16d20c5f7ed3127b4788292fa8dadd2a.png)

1.  相关性热力图展示了各个特征之间的相关关系，能够帮助发现高度相关的特征，以便于后续特征选择或降维。

![](img/be41934209cbd7448301a8e89cb7df39.png)

### 数据集拆分与标准化

为了保证模型能够泛化，我们将数据集划分为训练集和测试集，并进行标准化处理，使得各特征处于相同的尺度上，从而提高梯度提升算法的收敛速度和性能。

```
# 特征和标签分离
X = df.drop(&#x27;target&#x27;, axis=1)
y = df[&#x27;target&#x27;]

# 拆分数据集为训练集和测试集，测试集占比30%
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42, stratify=y)

# 对特征进行标准化处理
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

print(&quot;训练集样本数：&quot;, X_train.shape[0])
print(&quot;测试集样本数：&quot;, X_test.shape[0])
```

1.  采用stratify参数保证训练集和测试集中类别分布一致，避免因数据不平衡带来的偏差。

1.  标准化处理对大多数机器学习算法都有益处，尤其是梯度下降类算法，有助于提高算法的稳定性和收敛速度。

### 模型训练：梯度提升分类器

接下来，我们构建梯度提升分类器（GradientBoostingClassifier），利用默认参数进行初步训练，并对模型性能进行初步评估。

```
# 利用默认参数构建梯度提升分类器
gbc = GradientBoostingClassifier(random_state=42)
gbc.fit(X_train_scaled, y_train)

# 在测试集上进行预测
y_pred = gbc.predict(X_test_scaled)

# 输出分类报告
print(&quot;分类报告（Default Parameters）:&quot;)
print(classification_report(y_test, y_pred))

# 绘制混淆矩阵
cm = confusion_matrix(y_test, y_pred)
plt.figure(figsize=(6, 5))
sns.heatmap(cm, annot=True, fmt=&#x27;d&#x27;, cmap=&#x27;YlGnBu&#x27;)
plt.xlabel(&#x27;Predicted Label&#x27;, fontsize=12)
plt.ylabel(&#x27;True Label&#x27;, fontsize=12)
plt.title(&#x27;Confusion Matrix&#x27;, fontsize=14)
plt.show()
```

1.  使用默认参数构建模型后，通过classification_report可以直观地看到模型在精度、召回率、F1-score等指标上的表现。

1.  混淆矩阵有助于发现模型在预测过程中是否存在偏向某一类别的情况，为后续的调参和优化提供依据。

![](img/e74d7021268b3ef7cd7ccd307ea2b6b8.png)

### 模型超参数调优

模型的性能高度依赖于超参数的设置。我们使用GridSearchCV来自动搜索最佳超参数组合。主要调参参数包括：

+   n_estimators：弱学习器（决策树）的数量

+   learning_rate：每个弱学习器贡献的权重

+   max_depth：树的最大深度，用于控制过拟合

```
# 定义超参数网格
param_grid = {
    &#x27;n_estimators&#x27;: [50, 100, 150],
    &#x27;learning_rate&#x27;: [0.01, 0.1, 0.2],
    &#x27;max_depth&#x27;: [3, 4, 5]
}

# 利用GridSearchCV进行超参数搜索，采用5折交叉验证
grid_search = GridSearchCV(GradientBoostingClassifier(random_state=42), 
                           param_grid, 
                           cv=5, 
                           n_jobs=-1,
                           scoring=&#x27;accuracy&#x27;)
grid_search.fit(X_train_scaled, y_train)

print(&quot;最佳参数：&quot;, grid_search.best_params_)
print(&quot;最佳交叉验证得分：&quot;, grid_search.best_score_)

# 使用最佳参数构建新的模型
best_gbc = grid_search.best_estimator_
y_pred_best = best_gbc.predict(X_test_scaled)

print(&quot;分类报告（Optimized Parameters）:&quot;)
print(classification_report(y_test, y_pred_best))

# 绘制优化后模型的混淆矩阵
cm_best = confusion_matrix(y_test, y_pred_best)
plt.figure(figsize=(6, 5))
sns.heatmap(cm_best, annot=True, fmt=&#x27;d&#x27;, cmap=&#x27;coolwarm&#x27;)
plt.xlabel(&#x27;Predicted Label&#x27;, fontsize=12)
plt.ylabel(&#x27;True Label&#x27;, fontsize=12)
plt.title(&#x27;Confusion Matrix (Optimized)&#x27;, fontsize=14)
plt.show()
```

1.  通过GridSearchCV，我们系统地遍历了超参数的组合，找到在交叉验证中表现最好的参数。调参过程不仅提高了模型的准确性，同时也能降低过拟合的风险。

1.  调参后模型的分类报告和混淆矩阵较默认参数模型有明显改善，说明超参数对模型性能具有关键影响。

1.  通过对比优化前后结果，可以验证调参过程的有效性。

![](img/da9b7a1dbc15adf2010ea9b9bebc108c.png)

### 模型评估与结果可视化

为了更全面地评估模型，我们除了查看混淆矩阵和分类报告外，还绘制了ROC曲线来分析模型在不同阈值下的分类性能。

```
# 计算预测概率
y_proba = best_gbc.predict_proba(X_test_scaled)[:, 1]

# 计算ROC曲线
fpr, tpr, thresholds = roc_curve(y_test, y_proba)
roc_auc = auc(fpr, tpr)

plt.figure(figsize=(8, 6))
plt.plot(fpr, tpr, color=&#x27;darkred&#x27;, lw=2, label=&#x27;ROC curve (area = %0.2f)&#x27; % roc_auc)
plt.plot([0, 1], [0, 1], color=&#x27;navy&#x27;, lw=2, linestyle=&#x27;--&#x27;)
plt.xlabel(&#x27;False Positive Rate&#x27;, fontsize=12)
plt.ylabel(&#x27;True Positive Rate&#x27;, fontsize=12)
plt.title(&#x27;Receiver Operating Characteristic&#x27;, fontsize=14)
plt.legend(loc=&quot;lower right&quot;)
plt.show()
```

1.  ROC曲线可以全面反映模型在各种分类阈值下的表现，曲线下面积（AUC）是衡量模型整体判别能力的重要指标。

1.  理想的ROC曲线应尽可能靠近左上角，AUC值越高表示模型的区分能力越强。

1.  在本例中，通过优化后的模型得到较高的AUC，证明调参优化带来的提升具有统计学意义。

![](img/cbcd395d9d83bb32142396138f971a80.png)

### 特征重要性分析

梯度提升算法在训练过程中会自动评估各特征的重要性，帮助我们理解哪些特征对最终预测贡献最大。下面我们绘制特征重要性柱状图，并对结果进行解释。

```
# 获取各特征的重要性
feature_importances = best_gbc.feature_importances_
features = X.columns

# 构造DataFrame，方便排序和展示
importance_df = pd.DataFrame({
    &#x27;Feature&#x27;: features,
    &#x27;Importance&#x27;: feature_importances
}).sort_values(by=&#x27;Importance&#x27;, ascending=False)

plt.figure(figsize=(12, 6))
sns.barplot(x=&#x27;Importance&#x27;, y=&#x27;Feature&#x27;, data=importance_df, palette=&#x27;Spectral&#x27;)
plt.xlabel(&#x27;Feature Importance&#x27;, fontsize=12)
plt.ylabel(&#x27;Feature&#x27;, fontsize=12)
plt.title(&#x27;Feature Importance Ranking&#x27;, fontsize=14)
plt.show()
```

1.  特征重要性分析可以揭示哪些变量对模型的预测贡献最大，从而为领域专家提供有价值的信息（例如哪些生物指标更值得关注）。

1.  如果存在部分特征贡献很低，可以考虑进行特征选择，进一步减少模型复杂度，加快训练速度，同时可能提升泛化能力。

![](img/acfbf018233799d3fb3fe39940873b7f.png)

### 算法优化策略

在实际应用中，为了进一步提高模型的效果，我们可以从几个方面对梯度提升算法进行优化：

**1. 调整超参数**

+   通过调节n_estimators、learning_rate和max_depth等参数，找到性能与模型复杂度之间的平衡点。过高的树数量和过深的树容易导致过拟合，而较低的学习率则需要更多的迭代次数来收敛。

+   此外，还可以尝试引入min_samples_split、min_samples_leaf等参数，进一步控制树的分支条件，从而提高模型鲁棒性。

**2. 特征工程**

+   针对数据集内可能存在的共线性问题，可以采用PCA、LDA等降维方法进行特征转换，降低模型冗余度。

+   针对不同特征的分布差异，考虑进行对数变换、Box-Cox变换等处理，使得数据更符合模型假设。

**3. 集成多个模型**

+   可考虑结合其他集成算法，如随机森林、XGBoost、LightGBM等，进行模型融合（Ensemble），充分利用不同模型的优势，提高最终预测性能。

**4. 交叉验证与早停法**

+   在模型训练过程中，可以采用更为复杂的交叉验证方法，并引入early stopping策略。当验证集性能不再提升时，提前终止训练，避免过拟合。

**5. 调参工具自动化**

+   除了GridSearchCV，还可以使用RandomizedSearchCV或Bayesian Optimization（例如Optuna、Hyperopt）进行更高效的超参数搜索，这些方法在高维参数空间中表现更好，能够更快找到较优解。

```
# 进一步示例：利用早停法进行优化
from sklearn.metrics import log_loss

# 构造一个梯度提升分类器，设置较多的迭代次数，并使用early stopping
gbc_early_stop = GradientBoostingClassifier(
    n_estimators=300,  # 设置较多的迭代次数
    learning_rate=grid_search.best_params_[&#x27;learning_rate&#x27;],
    max_depth=grid_search.best_params_[&#x27;max_depth&#x27;],
    random_state=42,
    validation_fraction=0.1,  # 使用部分训练数据作为验证集
    n_iter_no_change=10  # 如果连续10次迭代验证集损失没有改善，则提前停止训练
)

gbc_early_stop.fit(X_train_scaled, y_train)

# 训练过程中记录验证集损失（如果可用），此处可通过staged_predict_proba观察
losses = []
for y_proba in gbc_early_stop.staged_predict_proba(X_test_scaled):
    loss = log_loss(y_test, y_proba)
    losses.append(loss)

plt.figure(figsize=(8, 6))
plt.plot(losses, color=&#x27;purple&#x27;, lw=2)
plt.xlabel(&#x27;Iteration&#x27;, fontsize=12)
plt.ylabel(&#x27;Log Loss&#x27;, fontsize=12)
plt.title(&#x27;Validation Log Loss over Iterations&#x27;, fontsize=14)
plt.show()

print(&quot;Early stopping iterations:&quot;, gbc_early_stop.n_estimators_)
print(&quot;Final Test Accuracy:&quot;, gbc_early_stop.score(X_test_scaled, y_test))
```

1.  通过设置较高的最大迭代次数结合early stopping策略，可以使模型在达到最优性能后自动停止训练，避免不必要的计算浪费和过拟合风险。

1.  通过绘制验证集Log Loss随迭代次数的变化曲线，可以直观地观察到模型何时开始出现过拟合现象，帮助确定最优迭代次数。

1.  这种优化方法在实际工程中非常实用，特别是当数据量较大或者训练时间较长时，early stopping能显著提高效率。

![](img/ba75236847291b3494524ca2b9c350c5.png)

咱们整个代码展示了利用梯度提升算法进行乳腺癌分类任务的整个流程。代码中不仅涉及数据加载、预处理、模型训练、调参和评估，还特别加入了多种可视化手段，确保每一步骤大家都有一个清晰的理解。

## 模型分析

### 梯度提升模型的优缺点

**优点：**

+   **高预测性能**：梯度提升模型通过不断迭代地构建和优化多个弱分类器，可以捕捉复杂的数据模式，在许多任务中能取得较高的准确率和鲁棒性。

+   **灵活性强**：模型可以通过调整诸如弱学习器数量、学习率、树的深度等超参数来平衡偏差与方差，适用于多种数据分布和问题场景。

+   **内置特征选择**：在迭代过程中，梯度提升会自动对各特征进行重要性评估，有助于了解哪些变量对预测贡献较大，同时可辅助进行特征工程。

+   **稳健性**：对异常值和部分噪声具有一定的鲁棒性，能较好地处理非线性关系和特征交互。

**缺点：**

+   **训练时间较长**：由于模型需要逐步构建多个弱学习器，尤其在超参数调优时计算代价较高，不适合实时或超大数据量的场景。

+   **容易过拟合**：若超参数设置不当（如学习率过高或树的深度过深），模型可能出现过拟合问题，因此需要采用交叉验证、early stopping等策略。

+   **参数调优复杂**：模型的性能对参数设置较为敏感，需要较多经验和计算资源来搜索最优参数组合。

+   **解释性较弱**：虽然可以输出特征重要性，但整体模型结构相对复杂，不如单棵决策树直观，模型解释性相对较低。

### 梯度提升与相似算法对比

咱们从模型性能、训练速度、调参难度等方面进行比较~

| 模型 | Strengths | Weaknesses |
| --- | --- | --- |
| Gradient Boosting | - 高精度  - 能捕捉复杂特征交互  - 内置特征重要性评估。 | - 训练时间长  - 对超参数敏感  - 可能过拟合。 |
| **Random Forest** | - 易于实现和调参  - 训练速度较快  - 不容易过拟合  - 能处理大规模数据。 | - 模型预测精度可能低于梯度提升  - 可解释性较弱  - 对噪声数据可能存在冗余信息。 |
| **XGBoost** | - 高效且准确  - 支持正则化减少过拟合  - 并行计算优化速度。 | - 参数调优复杂  - 内存占用较大  - 学习曲线较陡峭。 |
| **LightGBM** | - 速度快，内存消耗低  - 能处理大规模数据  -适合高维特征数据 | - 对小数据集效果不如其他算法  - 参数调优要求较高  -对类别特征需要额外处理。 |
| **AdaBoost** | - 实现简单  - 对异常值有一定鲁棒性  - 与其他算法组合时能提升效果。 | - 对噪声和离群点敏感  - 通常性能不如梯度提升  - 在复杂数据上表现有限。 |

### 适用场景

**优选梯度提升的情况：**

+   **预测精度要求高的场景**：当问题对预测准确性要求非常严格时，梯度提升算法由于其不断迭代优化的特性往往能获得更高的精度。

+   **数据特征交互复杂**：当数据中存在大量非线性和复杂特征交互时，梯度提升能更好地捕捉这些信息，从而提高模型表现。

+   **数据量适中**：对于中小规模的数据集，梯度提升的训练时间较为可控，且能充分发挥其模型优化能力。

+   **特征选择需求**：当需要借助模型了解各特征的重要性时，梯度提升的内置特征评估能提供有价值的信息。

**考虑其他算法的情况：**

+   **实时或大规模数据场景**：当数据量极大或需要实时预测时，随机森林、LightGBM等在训练和预测速度上可能更有优势。

+   **对训练时间要求较高的场景**：如果计算资源有限或调参时间紧张，随机森林或AdaBoost通常调参相对简单，训练速度更快。

+   **解释性要求高的场景**：对于需要简单直观解释模型决策的场合，单棵决策树或简单的集成模型可能更合适，而梯度提升模型整体结构较复杂。

+   **内存资源有限**：在资源受限的环境下，LightGBM由于内存消耗较低，可以作为梯度提升的替代选择。

通过以上文字描述和表格对比，可以看出梯度提升模型在精度和复杂特征建模方面具有明显优势，但也存在训练时间长、超参数调优复杂等问题。

因此，在选择算法时需要根据具体任务的数据规模、计算资源、实时性要求以及模型解释性的需求来做出合理的选择。