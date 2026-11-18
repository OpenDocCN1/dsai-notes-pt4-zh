
# Task3：进阶baseline详解

> 来源：[https://datawhaler.feishu.cn/docx/A6sad8CO4oFXPGxVKeqcCjvEnlb](https://datawhaler.feishu.cn/docx/A6sad8CO4oFXPGxVKeqcCjvEnlb)

## 导入模块

导入Python中用于数据分析、机器学习建模和化学信息学所需的库。以下是每个库的简要说明：

+ `numpy`: 提供强大的多维数组对象和相应的操作。

+ `pandas`: 提供高性能、易用的数据结构和数据分析工具。

+ `catboost`: 一个用于机器学习的库，特别是分类和回归任务。

+ `sklearn.model_selection`: 包含模型选择的多种方法，如交叉验证。

+ `sklearn.metrics`: 包含评估模型性能的多种指标。

+ `rdkit`: 一个化学信息学和机器学习软件，用于处理化学结构。

+ `sklearn.feature_extraction.text`: 提供将文本转换为特征向量的Tf-idf向量化器。

+ `tqdm`: 用于在长循环中添加进度条的库。

+ `sys`: 与Python解释器密切相关的模块，提供访问由解释器使用或维护的变量和函数。

+ `os`: 提供与操作系统交互的功能。

+ `gc`: 垃圾收集器接口，用于手动标记对象为可删除。

+ `re`: 正则表达式库，用于字符串搜索和替换。

+ `argparse`: 用于编写用户友好的命令行接口。

+ `warnings`: 用于发出警告的库，这里用来忽略警告信息。

```
import numpy as np
import pandas as pd
from catboost import CatBoostClassifier
from sklearn.model_selection import StratifiedKFold, KFold, GroupKFold
from sklearn.metrics import f1_score
from rdkit import Chem
from rdkit.Chem import Descriptors
from sklearn.feature_extraction.text import TfidfVectorizer
import tqdm, sys, os, gc, re, argparse, warnings
warnings.filterwarnings('ignore')
```

## 数据预处理

```
train = pd.read_excel('./dataset-new/traindata-new.xlsx')
test = pd.read_excel('./dataset-new/testdata-new.xlsx')

# test数据不包含 DC50 (nM) 和 Dmax (%)
train = train.drop(['DC50 (nM)', 'Dmax (%)'], axis=1)

# 定义了一个空列表drop_cols，用于存储在测试数据集中非空值小于10个的列名。
drop_cols = []
for f in test.columns:
    if test[f].notnull().sum() < 10:
        drop_cols.append(f)
        
# 使用drop方法从训练集和测试集中删除了这些列，以避免在后续的分析或建模中使用这些包含大量缺失值的列
train = train.drop(drop_cols, axis=1)
test = test.drop(drop_cols, axis=1)

# 使用pd.concat将清洗后的训练集和测试集合并成一个名为data的DataFrame，便于进行统一的特征工程处理
data = pd.concat([train, test], axis=0, ignore_index=True)
cols = data.columns[2:]
```

## 特征工程

特征工程指的是把原始数据转变为模型训练数据的过程，目的是获取更好的训练数据特征。特征工程能使得模型的性能得到提升，有时甚至在简单的模型上也能取得不错的效果。

实践代码：

```
# 将SMILES转换为分子对象列表,并转换为SMILES字符串列表
data['smiles_list'] = data['Smiles'].apply(lambda x:[Chem.MolToSmiles(mol, isomericSmiles=True) for mol in [Chem.MolFromSmiles(x)]])
data['smiles_list'] = data['smiles_list'].map(lambda x: ' '.join(x))  

# 使用TfidfVectorizer计算TF-IDF
tfidf = TfidfVectorizer(max_df = 0.9, min_df = 1, sublinear_tf = True)
res = tfidf.fit_transform(data['smiles_list'])

# 将结果转为dataframe格式
tfidf_df = pd.DataFrame(res.toarray())
tfidf_df.columns = [f'smiles_tfidf_{i}' for i in range(tfidf_df.shape[1])]

# 按列合并到data数据
data = pd.concat([data, tfidf_df], axis=1)

# 自然数编码
def label_encode(series):
    unique = list(series.unique())
    return series.map(dict(zip(
        unique, range(series.nunique())
    )))

for col in cols:
    if data[col].dtype == 'object':
        data[col]  = label_encode(data[col])
        
train = data[data.Label.notnull()].reset_index(drop=True)
test = data[data.Label.isnull()].reset_index(drop=True)

# 特征筛选
features = [f for f in train.columns if f not in ['uuid','Label','smiles_list']]

# 构建训练集和测试集
x_train = train[features]
x_test = test[features]

# 训练集标签
y_train = train['Label'].astype(int)
```

代码执行了以下步骤：

1. **SMILES转换**：使用`RDKit`库将数据集中的`SMILES`字符串转换回`SMILES`字符串的列表。这里看起来有些冗余，因为您已经拥有`SMILES`字符串，但可能您想确保所有`SMILES`都是以相同的方式（例如，考虑异构体信息）处理的。

1. **字符串处理**：将`SMILES`字符串列表转换为单个字符串，每个`SMILES`之间用空格分隔。

1. **TF-IDF计算**：使用`TfidfVectorizer`从处理后的`SMILES`字符串创建`TF-IDF`特征矩阵。

1. **转换为DataFrame**：将`TF-IDF`矩阵转换为`DataFrame`，以便与原始数据集结合。

1. **自然数编码**：定义了一个函数`label_encode`，用于将分类特征（对象类型）转换为整数编码。

1. **特征和标签准备**：

+ 对于所有的特征列（`cols`），如果它们的数据类型是对象（通常表示为字符串），则应用自然数编码。

+ 从合并后的数据集中分离出训练集和测试集，其中训练集包含标签（`Label`），测试集不包含。

1. **特征和标签的筛选**：从训练集和测试集中筛选出特征列（不包括`uuid`、`Label`和`smiles_list`），并从训练集中提取标签列。

1. **数据类型转换**：将标签列`Label`转换为整数类型，以便于模型训练。

## 模型训练与预测

```
def cv_model(clf, train_x, train_y, test_x, clf_name, seed=2022):
    
    kf = KFold(n_splits=5, shuffle=True, random_state=seed)

    train = np.zeros(train_x.shape[0])
    test = np.zeros(test_x.shape[0])

    cv_scores = []
    # 100， 1 2 3 4 5
    # 1 2 3 4    5
    # 1 2 3 5。  4
    # 1
    for i, (train_index, valid_index) in enumerate(kf.split(train_x, train_y)):
        print('************************************ {} {}************************************'.format(str(i+1), str(seed)))
        trn_x, trn_y, val_x, val_y = train_x.iloc[train_index], train_y[train_index], train_x.iloc[valid_index], train_y[valid_index]
               
        params = {'learning_rate': 0.1, 'depth': 6, 'l2_leaf_reg': 10, 'bootstrap_type':'Bernoulli','random_seed':seed,
                  'od_type': 'Iter', 'od_wait': 100, 'allow_writing_files': False, 'task_type':'CPU'}

        model = clf(iterations=20000, **params, eval_metric='AUC')
        model.fit(trn_x, trn_y, eval_set=(val_x, val_y),
                  metric_period=100,
                  cat_features=[], 
                  use_best_model=True, 
                  verbose=1)

        val_pred  = model.predict_proba(val_x)[:,1]
        test_pred = model.predict_proba(test_x)[:,1]
            
        train[valid_index] = val_pred
        test += test_pred / kf.n_splits
        cv_scores.append(f1_score(val_y, np.where(val_pred>0.5, 1, 0)))
        
        print(cv_scores)
       
    print("%s_score_list:" % clf_name, cv_scores)
    print("%s_score_mean:" % clf_name, np.mean(cv_scores))
    print("%s_score_std:" % clf_name, np.std(cv_scores))
    return train, test
    
cat_train, cat_test = cv_model(CatBoostClassifier, x_train, y_train, x_test, "cat")

pd.DataFrame(
    {
        'uuid': test['uuid'],
        'Label': np.where(cat_test>0.5, 1, 0)
    }
).to_csv('submit.csv', index=None)
```

代码定义了一个名为 `cv_model` 的函数，用于使用交叉验证训练分类模型，并对测试集进行预测。以下是代码的详细步骤和说明：

1. **函数定义**：`cv_model` 函数接受分类器对象 `clf`，训练特征集 `train_x` 和标签集 `train_y`，测试特征集 `test_x`，分类器名称 `clf_name`，以及可选的随机种子 `seed`。

1. **交叉验证设置**：使用 `KFold` 进行5折交叉验证，`shuffle=True` 表示在分折前打乱数据。

1. **初始化变量**：创建两个数组 `train` 和 `test` 来存储交叉验证过程中的训练集预测和测试集预测。

1. **循环执行交叉验证**：

+ 对每一折数据，使用训练索引 `train_index` 和验证索引 `valid_index` 分割训练集和验证集。

+ 打印当前折数和随机种子。

+ 设置 `CatBoost` 分类器的参数 `params`。

1. **模型训练**：使用 `CatBoost` 分类器训练模型，`iterations=20000` 表示最大迭代次数，`eval_metric='AUC'` 表示使用 `AUC` 作为评估指标。

1. **模型评估**：

+ 使用验证集 `val_x` 和 `val_y` 对模型进行评估，获取预测概率 `val_pred`。

+ 使用测试集 `test_x` 获取测试集预测概率 `test_pred`。

1. **保存结果**：将验证集的预测结果存储在 `train` 数组中，将测试集的预测结果累加到 `test` 数组中，并计算当前折的 `F1` 分数。

1. **输出结果**：打印所有折的 `F1` 分数、平均值和标准差。

1. **返回结果**：返回训练集预测结果 `train` 和测试集预测结果 `test`。

1. **模型应用**：

+ 使用 `cv_model` 函数训练 `CatBoost` 分类器，并将返回的测试集预测结果 `cat_test` 用于生成提交文件。

+ 根据预测概率 `cat_test` 生成二元标签，概率大于0.5的预测为1，否则为0。

1. **生成提交文件**：创建一个包含 `uuid` 和预测标签 `Label` 的 `DataFrame`，并将其保存为 `CSV` 文件。

## 附加：TFIDF学习

TF-IDF（Term Frequency-Inverse Document Frequency）是一种用于信息检索与文本挖掘的常用加权技术。它是一种统计方法，用以评估一个词语对于一个文件集或一个语料库中的其中一份文件的重要程度。它的主要思想是：如果某个词语在一篇文章中出现的频率高（Term Frequency，TF），并且在其他文章中很少出现（Inverse Document Frequency，IDF），则认为这个词语具有很好的类别区分能力，对这篇文章的内容有很好的指示作用。

### 基本使用

TF-IDF通常用于以下方面：

+ **文本挖掘**：帮助识别文档中的关键词条。

+ **信息检索**：评估词条在文档中的重要性，用于搜索引擎中对搜索结果进行排序。

+ **特征提取**：在机器学习算法中，将文本转换为TF-IDF向量，作为输入特征。

### 使用步骤

1. **文本预处理**：包括去除标点符号、转换为小写、去除停用词等。

1. **分词**：将文本分割成词条（关键字）。

1. **计算TF**：统计每个词条在文档中出现的频率，并进行归一化。

1. **计算IDF**：统计词条在所有文档中出现的频率，计算其IDF值。

1. **计算TF-IDF**：将TF和IDF相乘，得到每个词条在一个文档中的重要性得分。

### 工具和库

在Python中，可以使用`scikit-learn`库中的`TfidfVectorizer`来方便地实现TF-IDF向量化：

```
from sklearn.feature_extraction.text import TfidfVectorizer

# 假设我们有以下文档集
documents = [
    "这是一个例子文档。",
    "这是第二个文档，其中包含一些相同的词。",
    "第三个文档完全不同。"
]

# 初始化TF-IDF向量化器
vectorizer = TfidfVectorizer()

# 转换文档集
tfidf_matrix = vectorizer.fit_transform(documents)

# 查看TF-IDF矩阵
print(tfidf_matrix)
```

这段代码会输出一个稀疏矩阵，其中包含了每个词条在每个文档中的TF-IDF得分。

TF-IDF是一种简单而强大的文本表示方法，尤其适用于处理大量文本数据。

## 附加：交叉验证学习



# 交叉验证（Cross-Validation）

交叉验证（Cross-Validation）是一种统计方法，用于评估并提高模型的预测性能，特别是在样本数量有限的情况下。它可以帮助我们评估模型的泛化能力，即在未知数据上的表现能力，从而减少模型过拟合的风险。

## 基本思想

将数据集分成几个子集，每次用一个子集作为测试集，其余子集联合起来作为训练集。这个过程重复进行多次，每次选择不同的子集作为测试集，最终得到多个模型评估结果，然后对这些结果进行平均，得到模型性能的综合评估。

## 主要类型

### `K`-折交叉验证（`K`-Fold Cross-Validation）

- 将数据集随机分成`K`个子集（或“折”）。
- 每次迭代中，选择其中一个子集作为测试集，其余`K-1`个子集联合作为训练集。
- 重复`K`次，每次选择不同的子集作为测试集。

### 留一交叉验证（Leave-One-Out Cross-Validation, LOOCV）

- 当数据集较小时使用，每次只留下一个样本作为测试集，其余所有样本作为训练集。

### 分层交叉验证（Stratified Cross-Validation）

- 特别适用于分类问题，确保每个折中类别的比例与整个数据集保持一致。

### 时间序列交叉验证（Time Series Cross-Validation）

- 适用于时间序列数据，确保测试集总是来自时间序列的末端。

## 核心步骤

1. **分割数据**：根据选择的交叉验证类型，将数据集分割成多个子集。
1. **训练和测试**：对每个子集进行训练和测试，记录模型在测试集上的性能。
1. **性能评估**：计算所有迭代中模型性能的平均值和标准差。
1. **选择模型**：根据交叉验证的结果选择表现最好的模型或参数。

## 使用优点

- **减少过拟合**：通过在多个训练集上训练模型，可以减少模型对特定训练数据的依赖。
- **更准确的性能评估**：提供了模型在不同数据集上表现的更全面视图。
- **利用所有数据**：每个样本都被用作训练和测试，充分利用了可用数据。

## 局限性

- **计算成本**：需要多次训练和测试模型，这可能会增加计算时间。
- **随机性**：`K`-折交叉验证中的随机分割可能导致结果的随机性。

# 附加：CatBoost学习

`CatBoost`是一个开源的梯度提升库，由俄罗斯的搜索引擎公司Yandex开发。它专为处理分类和回归任务而设计，尤其擅长处理具有大量类别特征（categorical features）的数据集。`CatBoost`的名称来源于“Categorical Boosting”，即对类别特征进行增强的算法。

## 核心特点

1. **自动处理类别特征**：无需对类别特征进行独热编码（One-Hot Encoding），`CatBoost`可以直接处理类别特征。
1. **处理缺失值**：`CatBoost`能够自动处理数据中的缺失值。
1. **提升算法**：`CatBoost`使用梯度提升决策树（GBDT）作为基学习器。
1. **模型的可解释性**：`CatBoost`提供了特征重要性评估和模型的可解释性工具。
1. **多任务学习**：`CatBoost`支持多任务学习，即同时学习多个输出。
1. **支持大数据**：`CatBoost`使用基于GPU和CPU的并行计算，可以高效处理大规模数据集。

## 基本使用步骤

1. **数据准备**：准备训练数据和测试数据，数据可以是数值型或类别型。
1. **创建`CatBoost`模型**：使用`CatBoost`的`CatBoostClassifier`或`CatBoostRegressor`类创建模型实例。
1. **设置参数**：根据需要设置模型参数，如迭代次数、`learning_rate`、`depth`等。
1. **训练模型**：使用训练数据训练模型。
1. **模型评估**：使用测试数据评估模型性能。
1. **应用模型**：使用训练好的模型进行预测。

## 示例代码

```
from catboost import CatBoostClassifier, Pool
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
# 假设`X`是特征数据，`y`是目标变量
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
# 创建`CatBoost`模型
model = CatBoostClassifier(
    iterations=100,
    learning_rate=0.1,
    depth=6,
    loss_function='Logloss',
    random_seed=42,
    verbose=False
)
# 训练模型
model.fit(X_train, y_train)
# 预测测试数据
y_pred = model.predict(X_test)
# 评估模型
accuracy = accuracy_score(y_test, y_pred)
print(f'Accuracy: {accuracy:.2f}')
```

## 注意事项

- 在使用`CatBoost`之前，需要确保安装了`CatBoost`库，可以通过`pip install catboost`命令安装。
- `CatBoost`的参数众多，可以根据具体问题进行调整以优化模型性能。
- `CatBoost`的模型训练速度和内存消耗可能比其他一些梯度提升库要高，特别是在使用大量数据或复杂模型时。

`CatBoost`是一个强大的机器学习工具，尤其适合处理包含类别特征的数据集，并且提供了许多高级功能来提高模型的性能和可解释性。

> 本教程主要由 [`@鱼遇雨欲语与余`](https://www.zhihu.com/people/wang-he-13-93) 写作贡献，期待你的点赞和评论～
