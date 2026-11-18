
# Stacking 模型融合与实践

将结果取平均进行融合是比较基础的融合的方式，另外一种经典融合方式为`stacking`，`stacking`是一种分层模型集成框架。以两层为例，第一层由多个基学习器组成，其输入为原始训练集，第二层的模型则是以第一层基学习器的输出作为特征加入训练集进行再训练，从而得到完整的`stacking`模型。

![](img/08eaa2ecbbcb2b1e1462addf112b4883.png)

## 第一层：基学习器训练

（类比`cv_model`函数）

1. 划分训练数据为`K`折（5折为例，每次选择其中四份作为训练集，一份作为验证集）；
2. 针对各个模型`RF`、`ET`、`GBDT`、`XGB`，分别进行5次训练，每次训练保留一份样本用作训练时的验证，训练完成后分别对`Validation set`、`Test set`进行预测，对于`Test set`一个模型会对应5个预测结果，将这5个结果取平均；对于`Validation set`一个模型经过5次交叉验证后，所有验证集数据都含有一个标签。此步骤结束后：5个验证集（总数相当于训练集全部）在每个模型下分别有一个预测标签，每行数据共有4个标签（4个算法模型），测试集每行数据也拥有四个标签（4个模型分别预测得到的）

## 第二层：元学习器训练

（类比`stack_model`函数）

1. 将训练集中的四个标签外加真实标签当作五列新的特征作为新的训练集，选取一个训练模型，根据新的训练集进行训练，然后应用测试集的四个标签组成的测试集进行预测作为最终的`result`。

## Stacking 参考代码

需要特别注意，代码给出的是解决二分类问题的流程，对于多分类问题大家可以自行尝试。

```
def stack_model(oof_1, oof_2, oof_3, predictions_1, predictions_2, predictions_3, y):
    &#x27;&#x27;&#x27;
    输入的oof_1, oof_2, oof_3可以对应lgb_oof，xgb_oof，cat_oof
    predictions_1, predictions_2, predictions_3对应lgb_test，xgb_test，cat_test
    &#x27;&#x27;&#x27;
    train_stack = pd.concat([oof_1, oof_2, oof_3], axis=1)
    test_stack = pd.concat([predictions_1, predictions_2, predictions_3], axis=1)
    
    oof = np.zeros((train_stack.shape[0],))
    predictions = np.zeros((test_stack.shape[0],))
    scores = []
    
    from sklearn.model_selection import RepeatedKFold
    folds = RepeatedKFold(n_splits=5, n_repeats=2, random_state=2021)
    
    for fold_, (trn_idx, val_idx) in enumerate(folds.split(train_stack, train_stack)): 
        print(&quot;fold n°{}&quot;.format(fold_+1))
        trn_data, trn_y = train_stack.loc[trn_idx], y[trn_idx]
        val_data, val_y = train_stack.loc[val_idx], y[val_idx]
        
        clf = Ridge(random_state=2024)
        clf.fit(trn_data, trn_y)

        oof[val_idx] = clf.predict(val_data)
        predictions += clf.predict(test_stack) / (5 * 2)
        
        score_single = roc_auc_score(val_y, oof[val_idx])
        scores.append(score_single)
        print(f&#x27;{fold_+1}/{5}&#x27;, score_single)
    print(&#x27;mean: &#x27;,np.mean(scores))
   
    return oof, predictions
```

## 附录

### 实践环境配置

#### AI 环境配置

视频讲解：AI夏令营：手把手带你配置AI环境_哔哩哔哩_bilibili

图文材料：`https://gitee.com/anine09/learn-python-the-smart-way-v2/blob/main/slides/chapter_0-Installation.ipynb`

### 工具使用与提分思路

+ 数据竞赛入门讲义：[《数据竞赛入门讲义》.pdf](https://datawhaler.feishu.cn/file/IXdDbOBgIoZbtvxLukqc2432nQd?from=from_copylink)

+ `Scikit-Learn` 保姆教程：`https://mp.weixin.qq.com/s/4NSVh1HniNT4CGakzHxm1w`

> 本教程主要由 [@鱼遇雨欲语与余](https://www.zhihu.com/people/wang-he-13-93) 写作贡献，期待你的点赞和评论～
