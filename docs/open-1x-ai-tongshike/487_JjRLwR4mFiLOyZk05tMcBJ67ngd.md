# Task3：特征工程进阶，持续上分

> 来源：[https://datawhaler.feishu.cn/docx/UaB6dGnVjow2Adx9cZicVD1wnkf](https://datawhaler.feishu.cn/docx/UaB6dGnVjow2Adx9cZicVD1wnkf)

> **教程贡献者说：**

> 本节主要基于task2的lgm模型，从特征和模型训练两个角度优化，并提供一些其它上分思路。

# 引导思考

## 生物学角度新特征

1.  使用反义链与target gene序列的序列匹配结果作为特征来增强模型表现。在siRNA沉默target gene的过程中，RNA酶III家族的Dicer与TAR RNA结合蛋白和Argonaute（Ago）共同作用，产生长度为21–25个核苷酸的siRNA。siRNA做为RNA诱导沉默复合体（RISC）的组成部分，能够识别靶mRNA，从而达到沉默目标基因的作用。但是siRNA与靶mRNA的互补方式的不同意味着沉默机制的不同：siRNA与mRNA的3′非翻译区（UTR）结合，在Ago1、Ago3和Ago4的帮助下会导致翻译抑制；但如果siRNA与编码序列（CDS）完全互补，靶mRNA将被Ago2的内切核酸酶活性切割。而本比赛的预测目标是mRNA的保留水平，因此siRNA反义链与target gene的匹配程度，以及匹配位置，都会对预测目标产生影响。

参考文献：

Paddison PJ. RNA interference in mammalian cell systems. Curr Top Microbiol Immunol 2008; 320: 1–19. 12 Ji X.

The mechanism of RNase III action: how dicer dices. Curr Top Microbiol Immunol 2008; 320: 99–116. 13

Rivas FV, Tolia NH, Song J-J, Aragon JP, Liu J, Hannon GJ et al. Purified Argonaute2 and an siRNA form recombinant human RISC. Nat Struct Mol Biol 2005; 12: 340–349. 14

Su H, Trombly MI, Chen J, Wang X. Essential and overlapping functions for mammalian Argonautes in microRNA silencing. Genes Dev 2009; 23: 304–317.

1.  GC含量是siRNA效率中的一个重要且基本的参数，可以作为模型预测的特征。这是因为低GC含量会导致非特异性和较弱的结合，而高GC含量可能会阻碍siRNA双链在解旋酶和RISC复合体作用下的解旋。生物学研究人员曾经通过大量实验确定了siRNA的GC含量的可接受区间，有人提出过31.6%到57.9%这个区间，也有人提出过36%到52%这个区间。此外，也有研究者更深入地地探讨了这个问题，他们发现在反义链中，第2到第7个核苷酸和第8到第18个核苷酸的GC百分比分别为19%和52%是理想的。此外，功能性siRNA在第9到第14个核苷酸之间有一个不稳定区域（GC含量低于其他区域），被称为能量谷，这是选择siRNA的重要标准。这种内部的不稳定性通过在mRNA剪切过程中诱导最理想的构象，从而提高了RISC复合体的功能性。从上述生物学研究中我们可以发现，siRNA的GC含量，或者片段中特定位置的GC含量，对其沉默效率至关重要。

参考文献：

Reynolds A, Leake D, Boese Q, Scaringe S, Marshall WS, Khvorova A. Rational siRNA design for RNA interference. Nat Biotechnol 2004; 22: 326–330. 40.

Ui‐Tei K, Naito Y, Takahashi F, Haraguchi T, Ohki‐Hamazaki H, Juni A et al. Guidelines for the selection of highly effective siRNA sequences for mammalian and chick RNA interference. Nucleic Acids Res 2004; 32: 936–948. 41

Amarzguioui M, Prydz H. An algorithm for selection of functional siRNA sequences. Biochem Biophys Res Commun 2004; 316: 1050–1058.

Khvorova A, Reynolds A, Jayasena SD. Functional siRNAs and miRNAs exhibit strand bias. Cell 2003; 115: 209–216.

1.  **将修饰过的碱基序列也进行编码**，简单的编码方式是将带有修饰的核苷酸编码为和普通核苷酸不一样的输入向量，复杂的编码方式是将不同修饰在化学上的差异也加入模型中。siRNA上核苷酸的化学修饰对于siRNA发挥其功能至关重要。在siRNA治疗技术研发的早期阶段，siRNA药物都是未加修饰的，siRNA可以在体内介导基因沉默，但是可能会出现较差治疗效果和潜在的非靶向效应。化学修饰的siRNA，例如用2′-O-甲基（2′-OMe）或2′-甲氧乙基（2′-MOE）取代2′-OH，或用locked nucleic acid、unlocked nucleic acid或glycol nucleic acid取代某些核苷酸，可以有效抑制由siRNA引发的免疫刺激性内源免疫激活，增强活性和特异性，并减少非靶向诱导的毒性。根据核苷酸的自然结构，化学修饰可以施加在磷酸骨架、核糖部分或碱基上。通常，这些修饰会同时引入到siRNA中。例如，2′-OMe和磷硫酸酯（PS）修饰的结合有助于胆固醇结合的siRNA的系统性给药，并实现体内高效的基因沉默。从siRNA药物的研发经验来说，对siRNA的精确修饰可以提高其沉默效率、特异性和稳定性，并减少其毒性和免疫原性。

参考文献：

Ju¨rgen Soutschek, A. A. et al. Therapeutic silencing of an endogenou**s g**ene by systemic administration of modified siRNAs. nature432, 173–178 (2004).

Kh**vo**rova, A. & Watts, J. K. The chemical evolution of oligonucleotide therapies of clinical utility. Nat. Biotechnol.35, 238–248 (2017).

# 正文内容

## lgm上分实现

> 本task不会直接给出所有代码，但同学们可以尝试根据上分提示进行代码实现（包括但不限于补充完成代码块中省略号的部分）和分数提交。

### 引入新特征

#### 对task2特征再刻画

对task2引入的长度、GC含量等特征细节刻画，引入生物知识先验

```
def siRNA_feat_builder3(s: pd.Series, anti: bool = False):
    name = &quot;anti&quot; if anti else &quot;sense&quot;
    df = s.to_frame()

    # 长度分组
    df[f&quot;feat_siRNA_{name}_len21&quot;] = (s.str.len() == 21)
    # 省略号标识以此类推构造特征
    ...

    # GC含量
    GC_frac = (s.str.count(&quot;G&quot;) + s.str.count(&quot;C&quot;))/s.str.len()
    df[f&quot;feat_siRNA_{name}_GC_in&quot;] = (GC_frac &gt;= 0.36) &amp; (GC_frac &lt;= 0.52)

    # 局部GC含量
    GC_frac1 = (s.str[1:7].str.count(&quot;G&quot;) + s.str[1:7].str.count(&quot;C&quot;))/s.str[1:7].str.len()
    ...
    
    df[f&quot;feat_siRNA_{name}_GC_in1&quot;] = GC_frac1
    ...

    return df.iloc[:, 1:]
```

#### 修饰siRNA构建特征

```
def siRNA_feat_builder3_mod(s: pd.Series, anti: bool = False):
    name = &quot;anti&quot; if anti else &quot;sense&quot;
    df = s.to_frame()
    
    # 修饰RNA的起始、终止位置单元类别
    for pos in [0, -1]:
        for c in voc_ls:
            ...
    for pos in [1, -2]:
        for c in voc_ls:
            ...

    return df.iloc[:, 1:]
```

修饰siRNA序列进行n-gram的词频统计，同时也可对未修饰序列进行相同的操作

```
class GenomicTokenizer:
    def __init__(self, ngram=5, stride=2):
        # 初始化分词器，设置n-gram长度和步幅
        self.ngram = ngram
        self.stride = stride
        
    def tokenize(self, t):

        # 字符串变list
        if isinstance(t, str):
            t = list(t)

        if self.ngram == 1:
            # 如果n-gram长度为1，直接将序列转换为字符列表
            toks = t
        else:
            # 否则，按照步幅对序列进行n-gram分词
            toks = [t[i:i+self.ngram] for i in range(0, len(t), self.stride) if len(t[i:i+self.ngram]) == self.ngram]
        
            # 如果最后一个分词长度小于n-gram，移除最后一个分词
            if len(toks[-1]) &lt; self.ngram:
                toks = toks[:-1]

            # sub list to str
            toks = [&#x27;&#x27;.join(x) for x in toks]

        # 返回分词结果
        return toks

class GenomicVocab:
    def __init__(self, itos):
        # 初始化词汇表，itos是一个词汇表列表
        self.itos = itos
        # 创建从词汇到索引的映射
        self.stoi = {v: k for k, v in enumerate(self.itos)}
        
    @classmethod
    def create(cls, tokens, max_vocab, min_freq):
        # 创建词汇表类方法
        # 统计每个token出现的频率
        freq = Counter(tokens)
        # 选择出现频率大于等于min_freq的token，并且最多保留max_vocab个token
        # itos = [&#x27;&lt;pad&gt;&#x27;] + [o for o, c in freq.most_common(max_vocab - 1) if c &gt;= min_freq]
        itos = [o for o, c in freq.most_common(max_vocab - 1) if c &gt;= min_freq]
        # 返回包含词汇表的类实例
        return cls(itos)
    
def siRNA_feat_builder_substr(se, name, patterns):
    
    # 创建一个空字典来存储特征
    features = {}

    for pattern in patterns:
        try:
            # escaped_pattern = re.escape(pattern)  # 转义模式中的特殊字符
            escaped_pattern = pattern
            features[f&quot;feat_{name}_seq_pattern_{escaped_pattern}&quot;] = se.str.count(escaped_pattern)
        except re.error as e:
            print(f&quot;Error in pattern {pattern}: {e}&quot;)

    # 将字典转换为DataFrame
    feature_df = pd.DataFrame(features)

    return feature_df
```

```
# 处理序列特征
seq_features_df = pd.DataFrame()

tokenizer1 = GenomicTokenizer(ngram=1, stride=1) # 1gram
tokenizer2 = GenomicTokenizer(ngram=2, stride=1) # 2gram
tokenizer3 = GenomicTokenizer(ngram=3, stride=1) # 3gram

# 子串词频统计，未修饰序列
cols_nomod = [&quot;siRNA_sense_seq&quot;, &quot;siRNA_antisense_seq&quot;]
all_tokens_nomod = []
for col in cols_nomod:
    for seq in df[col]:
        if pd.isna(seq):
            continue
        ...
print(&#x27;#all_tokens_nomod: &#x27;, len(all_tokens_nomod))

vocab_nomod = GenomicVocab.create(all_tokens_nomod, max_vocab=100000, min_freq=1)
print(&#x27;#vocab_nomod: &#x27;, len(vocab_nomod.itos))

for col in cols_nomod:
    ...
```

```
# 子串词频统计，修饰序列
cols_mod = [&quot;modified_siRNA_sense_seq&quot;, &quot;modified_siRNA_antisense_seq&quot;]
cols_mod_ls = [&quot;modified_siRNA_sense_seq_list&quot;, &quot;modified_siRNA_antisense_seq_list&quot;]
all_tokens_mod = []
for col in cols_mod_ls:
    for seq_ls in df[col]:
        if pd.isna(seq_ls):
            continue
        ...
print(&#x27;#all_tokens_mod: &#x27;, len(all_tokens_mod))

vocab_mod = GenomicVocab.create(all_tokens_mod, max_vocab=100000, min_freq=1)
print(&#x27;#vocab_mod: &#x27;, len(vocab_mod.itos))

for col in cols_mod:
    ...
```

#### siRNA序列与target序列对比

siRNA与target序列的结合能力是一个很重要的特征，其中两者的序列比对score可作为对这个特征的定量化描述

```
def get_feat_align(df, anti: bool = False):
    # 提示：https://biopython.org/docs/1.76/api/Bio.pairwise2.html
    # 使用pairwise2.align.localxx
    ...
```

### lgm优化

#### 低Remaining范围样本高权重

```
weight_ls = np.array(feats[&#x27;mRNA_remaining_pct&#x27;].apply(lambda x:2 if ((x&lt;=30)and(x&gt;=0)) else 1))
```

#### 使用**官方评价指标作为损失函数**

由原来的root_mean_squared_error评价指标被替换为更加复杂的官方评价分数，具体公式为:

```
# calculate_metrics函数用于计算评估指标
def calculate_metrics(preds, data, threshold=30):
    y_pred = preds
    y_true = data.get_label()
    mae = np.mean(np.abs(y_true - y_pred))
    # if mae &lt; 0: mae = 0
    # elif mae &gt;100: mae = 100

    y_true_binary = ((y_true &lt;= threshold) &amp; (y_true &gt;= 0)).astype(int)
    y_pred_binary = ((y_pred &lt;= threshold) &amp; (y_pred &gt;= 0)).astype(int)

    mask = (y_pred &gt;= 0) &amp; (y_pred &lt;= threshold)
    range_mae = (
        mean_absolute_error(y_true[mask], y_pred[mask]) if np.sum(mask) &gt; 0 else 100
    )
    # if range_mae &lt; 0: range_mae = 0
    # elif range_mae &gt;100: range_mae = 100

    # precision = precision_score(y_true_binary, y_pred_binary, average=&quot;binary&quot;)
    # recall = recall_score(y_true_binary, y_pred_binary, average=&quot;binary&quot;)

    if np.sum(y_pred_binary) &gt; 0:
        precision = (np.array(y_pred_binary) &amp; y_true_binary).sum()/np.sum(y_pred_binary)
    else:
        precision = 0
    if np.sum(y_true_binary) &gt; 0:
        recall = (np.array(y_pred_binary) &amp; y_true_binary).sum()/np.sum(y_true_binary)
    else:
        recall = 0

    if precision + recall == 0:
        f1 = 0
    else:
        f1 = 2 * precision * recall / (precision + recall)
    score = (1 - mae / 100) * 0.5 + (1 - range_mae / 100) * f1 * 0.5
    return &quot;custom_score&quot;, score, True  # True表示分数越高越好

```

#### **自适应学习率**

```
# adaptive_learning_rate函数用于自适应学习率
def adaptive_learning_rate(decay_rate=0.8, patience=50):
    best_score = float(&quot;-inf&quot;)  # 初始化为负无穷,因为分数越高越好
    wait = 0

    def callback(env):
        nonlocal best_score, wait
        current_score = env.evaluation_result_list[-1][2]  # 假设使用的是最后一个评估指标
        current_lr =  env.model.params.get(&#x27;learning_rate&#x27;)

        if current_score &gt; best_score: 
            best_score = current_score
            # wait = 0 # 需要连续的score没有上升
        else:
            wait += 1

        if wait &gt;= patience:
            new_lr = float(current_lr) * decay_rate
            wait = 0
            env.model.params[&#x27;learning_rate&#x27;] = new_lr
            print(f&quot;Learning rate adjusted to {env.model.params.get(&#x27;learning_rate&#x27;)}&quot;)

    return callback

```

#### 多折交叉训练

```
# train函数用于训练模型
def train(feats, n_original):
    # 定义k折交叉验证
    n_splits = 10
    kf = KFold(n_splits=n_splits, shuffle=True, random_state=42)
    # 开始k折交叉验证
    gbms = []
    for fold, (train_idx, val_idx) in enumerate(
        kf.split(feats.iloc[:n_original, :]), 1
    ):
        # 准备训练集和验证集
        X_train, X_val = feats.iloc[train_idx, :-1], feats.iloc[val_idx, :-1]
        y_train, y_val = feats.iloc[train_idx, -1], feats.iloc[val_idx, -1]
        w_train = weight_ls[train_idx]
        

        # 创建LightGBM数据集
        train_data = lgb.Dataset(X_train, label=y_train, weight=w_train)
        val_data = lgb.Dataset(X_val, label=y_val, reference=train_data)

        boost_round = 25000
        early_stop_rounds = int(boost_round*0.1)

        # 显示metric
        lgb_log = lgb.log_evaluation(period=200, show_stdv=True)
        lgb_stop = lgb.early_stopping(stopping_rounds=early_stop_rounds, first_metric_only=True, verbose=True, min_delta=0.00001)

        # 设置LightGBM参数
        params = {
            &quot;boosting_type&quot;: &quot;gbdt&quot;,
            &quot;objective&quot;: &quot;regression&quot;,
            &quot;metric&quot;: &quot;None&quot;,
            # &quot;metric&quot;: &quot;root_mean_squared_error&quot;,
            &quot;max_depth&quot;: 8,
            &quot;num_leaves&quot;: 63,
            &quot;min_data_in_leaf&quot;: 2,
            &quot;learning_rate&quot;: 0.05,
            &quot;feature_fraction&quot;: 0.9,
            &quot;lambda_l1&quot;: 0.1,
            &quot;lambda_l2&quot;: 0.2,
            &quot;verbose&quot;: -1, # -1时不输出
            &quot;early_stopping_round&quot;: early_stop_rounds,
            &quot;num_threads&quot;: 8,
        }

        # 在训练时使用自适应学习率回调函数
        adaptive_lr = adaptive_learning_rate(decay_rate=0.9, patience=1000)
        gbm = lgb.train(
            params,
            train_data,
            num_boost_round=boost_round,
            valid_sets=[val_data],
            feval=calculate_metrics,  # 将自定义指标函数作为feval参数传入
            # callbacks=[print_validation_result, adaptive_lr, lgb_log, lgb_stop],
            callbacks=[adaptive_lr, lgb_log, lgb_stop],
        )
        valid_score = gbm.best_score[&quot;valid_0&quot;][&quot;custom_score&quot;]
        print(f&quot;best_valid_score: {valid_score}&quot;)
        gbms.append(gbm)

    return gbms
```

## 其它上分思路

### lgm模型继续优化

新的Baseline提供是一个很好的起点。下面是几个模型结构和参数调整的上分思路，感兴趣的同学可以根据思路自行尝试：

**!!!!!! 注意****：**代码只是思路展示，下面代码没法直接用于当前任务！！！！！

#### 1. 优化超参数

a. 网格搜索（Grid Search）：

参考代码如下：

```
from sklearn.model_selection import GridSearchCV
from lightgbm import LGBMRegressor

param_grid = {
    &#x27;max_depth&#x27;: [3, 5, 7, 9],
    &#x27;learning_rate&#x27;: [0.01, 0.02, 0.05, 0.1],
    &#x27;n_estimators&#x27;: [100, 500, 1000, 2000],
    &#x27;min_child_samples&#x27;: [20, 30, 50, 100]
}

lgbm_model = LGBMRegressor(objective=&#x27;regression&#x27;, metric=&#x27;rmse&#x27;)
grid_search = GridSearchCV(estimator=lgbm_model, param_grid=param_grid, cv=5, n_jobs=-1, verbose=2)
grid_search.fit(X_train, y_train)

print(&quot;Best parameters found: &quot;, grid_search.best_params_)
```

b. 随机搜索（Random Search）：

参考代码如下：

```
from sklearn.model_selection import RandomizedSearchCV
from scipy.stats import randint as sp_randint

param_dist = {
    &#x27;max_depth&#x27;: sp_randint(3, 10),
    &#x27;learning_rate&#x27;: [0.01, 0.02, 0.05, 0.1],
    &#x27;n_estimators&#x27;: sp_randint(100, 2000),
    &#x27;min_child_samples&#x27;: sp_randint(20, 100)
}

random_search = RandomizedSearchCV(estimator=lgbm_model, param_distributions=param_dist, n_iter=100, cv=5, n_jobs=-1, verbose=2)
random_search.fit(X_train, y_train)

print(&quot;Best parameters found: &quot;, random_search.best_params_)
```

**c. 贝叶斯优化：**

您可以使用如optuna这样的库来执行贝叶斯优化, 参考代码如下：

```
import optuna

def objective(trial):
    params = {
        &#x27;max_depth&#x27;: trial.suggest_int(&#x27;max_depth&#x27;, 3, 10),
        &#x27;learning_rate&#x27;: trial.suggest_loguniform(&#x27;learning_rate&#x27;, 1e-3, 1e-1),
        &#x27;n_estimators&#x27;: trial.suggest_int(&#x27;n_estimators&#x27;, 100, 2000),
        &#x27;min_child_samples&#x27;: trial.suggest_int(&#x27;min_child_samples&#x27;, 20, 100)
    }
    
    model = LGBMRegressor(**params)
    model.fit(X_train, y_train)
    return model.score(X_val, y_val)

study = optuna.create_study(direction=&#x27;maximize&#x27;)
study.optimize(objective, n_trials=100)

print(&#x27;Best trial:&#x27;)
trial = study.best_trial
print(&#x27;Value: {}&#x27;.format(trial.value))
print(&#x27;Params: &#x27;)
for key, value in trial.params.items():
    print(&#x27;    {}: {}&#x27;.format(key, value))
```

这些方法可以帮助您找到更好的超参数组合。请注意，超参数优化可能会花费大量时间，特别是对于大型数据集。您可能需要根据自己的计算资源和时间限制来调整搜索范围和迭代次数。 在实际应用中，通常会结合使用早停和超参数优化，以获得最佳的模型性能和训练效率。

#### 2. 集成学习

集成学习是一种机器学习范式，通过结合多个模型来改善预测性能，通常比单个模型的表现更优。在你的项目中，可以采用以下几种常见的集成学习方法：

1.  **Bagging（自助聚合）**：通过在原始数据集上进行多次重采样来创建多个子集，分别训练多个模型，最后进行平均或多数投票决策。

1.  **Boosting**：训练多个模型，每个模型都尝试纠正前一个模型的错误，通常是序列处理。

1.  **Stacking**：训练多个不同的模型，然后再训练一个新的模型来综合这些模型的输出。

a. Stacking 示例代码

假设已有LightGBM、XGBoost和一个简单的神经网络模型，下面是一个使用Stacking方法的Python示例代码：

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
X = df.drop(&#x27;target&#x27;, axis=1)  # 特征列
y = df[&#x27;target&#x27;]  # 目标列

# 划分训练集和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 定义基模型
estimators = [
    (&#x27;lgb&#x27;, lgb.LGBMRegressor(objective=&#x27;regression&#x27;)),
    (&#x27;xgb&#x27;, XGBRegressor(objective=&#x27;reg:squarederror&#x27;)),
    (&#x27;mlp&#x27;, MLPRegressor(hidden_layer_sizes=(50, 30), max_iter=500))
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
print(f&#x27;Test MSE: {mse:.4f}&#x27;)

# 看每个单独模型的性能
for name, est in stacking_regressor.named_estimators_.items():
    y_pred_individual = est.predict(X_test)
    mse_individual = mean_squared_error(y_test, y_pred_individual)
    print(f&#x27;{name} Test MSE: {mse_individual:.4f}&#x27;)
```

#### 3. 混合学习

在解决复杂的生物信息学问题，如siRNA效果的预测中，机器学习与深度学习的混合方法可以提供强大的工具。这种混合方法可以结合深度学习的特征学习能力和传统机器学习模型的效率与解释性。以下是一种结合机器学习和深度学习的策略，包括两个主要部分：一个用于特征提取的深度学习模型（如卷积神经网络），以及一个传统的机器学习模型（如LightGBM）来进行最终的决策。这里提供一个示例，展示如何使用PyTorch构建深度学习部分，然后将输出特征传递给LightGBM进行分类或回归。

**1. 构建PyTorch模型**

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
X = np.array([np.array(list(map(float, list(seq)))) for seq in df[&#x27;sequence&#x27;]])
X = X.reshape(X.shape[0], 1, sequence_length)
y = df[&#x27;target&#x27;].values

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

2. 使用LightGBM进行回归

在获取特征后，我们可以使用LightGBM进行回归预测。

```
# 使用LightGBM进行最终的预测
lgb_regressor = lgb.LGBMRegressor(n_estimators=100, learning_rate=0.05, max_depth=5)
lgb_regressor.fit(X_train_features.reshape(-1, 1), y_train)

# 预测和评估
y_pred = lgb_regressor.predict(X_test_features.reshape(-1, 1))
mse = mean_squared_error(y_test, y_pred)
print(f&quot;Test MSE: {mse:.4f}&quot;)
```

#### 4. 其他思路

+   **外部数据集**： 集成额外的生物信息学数据库来丰富特征，这可能有助于提高模型的泛化能力。（比赛可以使用，但是需要说明出处）

+   **特征选择**：实施自动化的特征选择流程，以剔除不重要的特征，减少模型的复杂性并避免过拟合。

+   **动态学习率调整**：除了基于性能衰退调整学习率外，还可以尝试使用学习率预热（warm-up）和循环学习率等策略，这可能有助于改进模型收敛速度和质量。

+   **多目标优化**：考虑将多个评价指标整合到模型的优化过程中，如同时优化MAE和F1，或者设计一个更全面的评价函数，以更好地反映siRNA的实验效果。

### 新模型构建

#### 使用attention原理进行end2end的建模

1.  上一部分所述的通过特征工程进行上分的思路，都是从序列出发，基于生物学研究者在长期实践过程中积累的经验，对siRNA序列中的某些特征进行提取，以达到提高模型表现的目的。

1.  这种思路依然是传统的机器学习建模方法，在深度学习时代，AI研究者发现，基于大量的训练数据与端到端的数据模式提取算法，可以在大大减少建模过程中特征工程的复杂度的同时，提高模型的表现。最典型的代表案例就是卷积神经网络在图像领域的应用，以及循环神经网络和attention机制在自然语言领域的应用。

1.  由于本赛题中序列特征为信息量最丰富的模型特征，并且siRNA与target gene的序列信息关联对siRNA的沉默效率有直接的影响，因此可以将siRNA序列与target gene序列进行拼接，然后使用attention机制捕捉这两段序列内部，以及这两段序列之间与沉默效率相关的pattern。这种方法有望从数据驱动的视角，获得超出生物学家经验的新的siRNA序列模式以提高siRNA的设计能力，这才是AI for science的真正魅力所在。

#### 使用基础模型辅助建模

1.  近年来，在图像与自然语言领域，大模型正在如火如荼的发展与应用。大模型的主要思路就是通过深度学习算法的大规模参数，从大规模数据中提取有效信息，从而获得超越人类专家的智能。

1.  生物学序列数据也有大量的数据积累，计算生物学家们也希望在生物领域做出能够理解生物语言的chatGPT，现在已经有一些工作对大规模DNA、RNA序列进行预训练，获得了生物学序列大模型，以支持下游的一系列应用，比如LucaOne、hyenaDNA、caduceus等。

1.  我们难以在比赛期间自己训练出一个生物序列大模型，但是可以站在巨人的肩膀上，充分利用已有的生物序列基础模型，生成siRNA和target gene序列的表征向量，然后这些包含更多生物学信息的表征向量输入模型，以提高预测效果。

1.  由于生物序列大模型的研发仍处于学术研究的阶段，并没有一个公认最有效的模型，所以建议大家多多进行文献搜索与尝试，挑选出最适合该任务的基础模型

# ❀ 恭喜看完全部知识教程！

<https://datawhaler.feishu.cn/sync/TdhvdKG8psUuznbIsOzcJsSon56>

## 📟 你可以随时回顾我们这次的学习~

<https://datawhaler.feishu.cn/sync/VzINd7TdxstuUkbAqWJcLhV6nMN>

## 🎉合影留念墙

感谢很棒的你一路的坚持，让我们在这里给自己点个赞！

期待下期夏令营还能与你再次相遇~

一起合个影 :)

#

> 本教程主要由 、 、   写作贡献，

> 融合了多位助教的内测体验和宝贵建议。

> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

>

### 另外别忘了写笔记打卡呀！！~

### 及时思考、沉淀和产出能让你的学习事半功倍！

>

> **注：本文采用 ****署名—非商业性使用**—相同方式共享 4.0 协议国际版许可协议进行许可。