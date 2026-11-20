
# Task4：持续尝试，上分！

> 来源：[https://datawhaler.feishu.cn/docx/R8fXdASOYob6cYxKkA3c0pOWnle](https://datawhaler.feishu.cn/docx/R8fXdASOYob6cYxKkA3c0pOWnle)

在`baseline`阶段，我们使用`CatBoost`完成了解决机器学习问题的全部流程，得到了基础的分数。在进阶实践部分，将在原有`Baseline`基础上做更多优化，一般优化思路，从特征工程与模型中来思考。

## 优化方法建议

提取更多特征：在数据挖掘比赛中，特征总是最终制胜法宝，去思考什么信息可以帮助我们提高预测精准度，然后将其转化为特征输入到模型。对于本次赛题可以从专业角度构建特征，除了`Smiles`特征外，还有很多特征可以提取有价值的信息，比如`InChI`是由一系列部分组成，提供了关于分子结构的详细信息。比如开头标识、分子式、连接表、氢原子计数、多可旋转键计数、立体化学信息、同分异构体信息、混合物或互变异构体信息、电荷和自旋多重度信息等。

尝试不同的模型：模型间存在很大的差异，预测结果也会不一样，比赛的过程就是不断的实验和试错的过程，通过不断的实验寻找最佳模型，同时帮助自身加强模型的理解能力。

## 特征优化

这里主要对`InChI`进行展开构建特征，具体说明如下：

### (1) 提取分子式

从`InChI`字符串中，我们可以看到分子式直接给出在`/C47H61N7O6S`部分。这意味着分子由47个碳原子、61个氢原子、7个氮原子、6个氧原子和1个硫原子组成；

### (2) 计算分子量

分子量可以通过将每种原子的原子质量乘以其数量然后相加得到。例如：

+ 碳（C）的原子质量约为12.01 g/mol
+ 氢（H）的原子质量约为1.008 g/mol
+ 氮（N）的原子质量约为14.01 g/mol
+ 氧（O）的原子质量约为16.00 g/mol
+ 硫（S）的原子质量约为32.07 g/mol

分子量计算公式如下：

`分子量=(47×12.01)+(61×1.008)+(7×14.01)+(6×16.00)+(1×32.07)`

### (3) 原子计数

直接计算不同原子的个数，并进行展开。

### 参考代码如下

```python
import pandas as pd
import re

atomic_masses = {
    'H': 1.008, 'He': 4.002602, 'Li': 6.94, 'Be': 9.0122, 'B': 10.81, 'C': 12.01,
    'N': 14.01, 'O': 16.00, 'F': 19.00, 'Ne': 20.180, 'Na': 22.990, 'Mg': 24.305,
    'Al': 26.982, 'Si': 28.085, 'P': 30.97, 'S': 32.07, 'Cl': 35.45, 'Ar': 39.95,
    'K': 39.10, 'Ca': 40.08, 'Sc': 44.956, 'Ti': 47.867, 'V': 50.942, 'Cr': 52.00,
    'Mn': 54.938, 'Fe': 55.845, 'Co': 58.933, 'Ni': 58.69, 'Cu': 63.55, 'Zn': 65.38
}

# 函数用于解析单个InChI字符串
def parse_inchi(row):
    inchi_str = row['InChI']
    formula = ''
    molecular_weight = 0
    element_counts = {}
    
    # 提取分子式
    formula_match = re.search(r"InChI=1S/([^/]+)/c", inchi_str)
    if formula_match:
        formula = formula_match.group(1)
    
    # 计算分子量和原子计数
    for element, count in re.findall(r"([A-Z][a-z]*)([0-9]*)", formula):
        count = int(count) if count else 1
        element_mass = atomic_masses.get(element.upper(), 0)
        molecular_weight += element_mass * count
        element_counts[element.upper()] = count
    
    return pd.Series({
        'Formula': formula,
        'MolecularWeight': molecular_weight,
        'ElementCounts': element_counts
    })

# 应用函数到DataFrame的每一行
train[['Formula', 'MolecularWeight', 'ElementCounts']] = train.apply(parse_inchi, axis=1)

# 定义存在的key
keys = ['H', 'He', 'Li', 'Be', 'B', 'C', 'N', 'O', 'F', 'Ne', 'Na', 'Mg', 'Al', 'Si', 'P', 'S', 'Cl', 'Ar', 'K', 'Ca', 'Sc', 'Ti', 'V', 'Cr', 'Mn', 'Fe', 'Co', 'Ni', 'Cu', 'Zn']

# 创建一个空的DataFrame，列名为keys
df_expanded = pd.DataFrame({key: pd.Series() for key in keys})

# 遍历数据，填充DataFrame
for index, item in enumerate(train['ElementCounts'].values):
    for key in keys:
        # 将字典中的值填充到相应的列中
        df_expanded.at[index, key] = item.get(key, 0)
        
df_expanded = pd.DataFrame(df_expanded)
```

## 模型融合

模型融合代码参考：

定义`cv_model`函数，内部可以选择使用`lightgbm`、`xgboost`和`catboost`模型，可以依次跑完这三个模型，然后将三个模型的结果进行取平均进行融合。

```python
def cv_model(clf, train_x, train_y, test_x, clf_name, seed = 2023):
    folds = 5
    kf = KFold(n_splits=folds, shuffle=True, random_state=seed)
    oof = np.zeros(train_x.shape[0])
    test_predict = np.zeros(test_x.shape[0])
    cv_scores = []
    for i, (train_index, valid_index) in enumerate(kf.split(train_x, train_y)):
        print('************************************ {} ************************************'.format(str(i+1)))
        trn_x, trn_y, val_x, val_y = train_x.iloc[train_index], train_y[train_index], train_x.iloc[valid_index], train_y[valid_index]
        
        if clf_name == "lgb":
            train_matrix = clf.Dataset(trn_x, label=trn_y)
            valid_matrix = clf.Dataset(val_x, label=val_y)
            params = {
                'boosting_type': 'gbdt',
                'objective': 'binary',
                'min_child_weight': 6,
                'num_leaves': 2 ** 6,
                'lambda_l2': 10,
                'feature_fraction': 0.8,
                'bagging_fraction': 0.8,
                'bagging_freq': 4,
                'learning_rate': 0.35,
                'seed': 2024,
                'nthread' : 16,
                'verbose' : -1,
            }
            model = clf.train(params, train_matrix, 2000, valid_sets=[train_matrix, valid_matrix],
                              categorical_feature=[], verbose_eval=1000, early_stopping_rounds=100)
            val_pred = model.predict(val_x, num_iteration=model.best_iteration)
            test_pred = model.predict(test_x, num_iteration=model.best_iteration)
        
        if clf_name == "xgb":
            xgb_params = {
              'booster': 'gbtree', 
              'objective': 'binary:logistic',
              'num_class':3,
              'max_depth': 5,
              'lambda': 10,
              'subsample': 0.7,
              'colsample_bytree': 0.7,
              'colsample_bylevel': 0.7,
              'eta': 0.35,
              'tree_method': 'hist',
              'seed': 520,
              'nthread': 16
              }
            train_matrix = clf.DMatrix(trn_x , label=trn_y)
            valid_matrix = clf.DMatrix(val_x , label=val_y)
            test_matrix = clf.DMatrix(test_x)
            
            watchlist = [(train_matrix, 'train'),(valid_matrix, 'eval')]
            
            model = clf.train(xgb_params, train_matrix, num_boost_round=2000, evals=watchlist, verbose_eval=1000, early_stopping_rounds=100)
            val_pred  = model.predict(valid_matrix)
            test_pred = model.predict(test_matrix)
            
        if clf_name == "cat":
            params = {'learning_rate': 0.35, 'depth': 5, 'bootstrap_type':'Bernoulli','random_seed':2024,
                      'od_type': 'Iter', 'od_wait': 100, 'random_seed': 11, 'allow_writing_files': False}
            
            model = clf(iterations=2000, **params)
            model.fit(trn_x, trn_y, eval_set=(val_x, val_y),
                      metric_period=1000,
                      use_best_model=True, 
                      cat_features=[],
                      verbose=1)
            
            val_pred  = model.predict_proba(val_x)
            test_pred = model.predict_proba(test_x)
        
        oof[valid_index] = val_pred
        test_predict += test_pred / kf.n_splits
        
        F1_score = f1_score(val_y, np.where(val_pred>0.5, 1, 0))
        cv_scores.append(F1_score)
        print(cv_scores)
        
    return oof, test_predict

# 参考demo,具体对照baseline实践部分调用cv_model函数
# 选择lightgbm模型
lgb_oof, lgb_test = cv_model(lgb, x_train, y_train, x_test, 'lgb')
# 选择xgboost模型
xgb_oof, xgb_test = cv_model(xgb, x_train, y_train, x_test, 'xgb')
# 选择catboost模型
cat_oof, cat_test = cv_model(CatBoostClassifier, x_train, y_train, x_test, 'cat')

# 进行取平均融合
final_test = (lgb_test + xgb_test + cat_test) / 3
```
