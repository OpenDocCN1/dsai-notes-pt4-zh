
```py
  (ggplot(df_train_set, aes(x = "Race", fill = "Income"))+ geom_bar(position="fill")+ theme(axis_text_x = element_text(angle = -90, hjust = 1))+ facet_wrap('~Sex'))

```

![](img/ba82da2edb61b221ad4d0c8577333007.jpg)

直到现在，我们仅关注了非数值特征（`non-numeric`）的相互关系。现在我们看一下资本收益（`CapitalGain`）和资本损失（`CapitalLoss`）对收入的影响。

```py
  (ggplot(df_train_set, aes(x="Income", y="CapitalGain"))+ geom_jitter(position=position_jitter(0.1)))

```

![](img/992e1a9eb752b0f793ae0152eca67034.jpg)

```py
  (ggplot(df_train_set, aes(x="Income", y="CapitalLoss"))+ geom_jitter(position=position_jitter(0.1)))

```

![](img/b5c26da235879527228f67e31fd3bbdc.jpg)

## 树分类器

现在我们理解了我们数据中的一些关系，所以就可以使用 `sklearn.tree.DecisionTreeClassifier` 创建一个简单的树分类器模型。然而，为了使用这一模型，我们需要把所有我们的非数值数据转化成数值型数据。我们可以直接在 `Pandas` 数据框架中使用 `sklearn.preprocessing.LabeEncoder` 模块和 `sklearn_pandas` 模块就可以轻松地完成这一步骤。

```py
  mapper = DataFrameMapper([('AgeGroup', LabelEncoder()),('Education', LabelEncoder()),('Workclass', LabelEncoder()),('MaritalStatus', LabelEncoder()),('Occupation', LabelEncoder()),('Relationship', LabelEncoder()),('Race', LabelEncoder()),('Sex', LabelEncoder()),('Income', LabelEncoder())], df_out=True, default=None) cols = list(df_train_set.columns) cols.remove("Income") cols = cols[:-3] + ["Income"] + cols[-3:] df_train = mapper.fit_transform(df_train_set.copy()) df_train.columns = cols df_test = mapper.transform(df_test_set.copy()) df_test.columns = cols cols.remove("Income") x_train, y_train = df_train[cols].values, df_train["Income"].values x_test, y_test = df_test[cols].values, df_test["Income"].values

```

现在我们用正确的形式对数据进行了训练和测试，已创建了我们的第一个模型！

```py
  treeClassifier = DecisionTreeClassifier() treeClassifier.fit(x_train, y_train) treeClassifier.score(x_test, y_test)

```

最简单的且没有优化的概率分类器模型可以达到 `83.5%` 的精度。在分类问题中，混淆矩阵（confusion matrix）是衡量模型精度的好方法。使用下列代码我们可以绘制任意基于树的模型的混淆矩阵。

```py
  import itertoolsfrom sklearn.metrics import confusion_matrixdef plot_confusion_matrix(cm, classes, normalize=False):"""    This function prints and plots the confusion matrix.    Normalization can be applied by setting `normalize=True`.    """    cmap = plt.cm.Blues    title = "Confusion Matrix"if normalize:        cm = cm.astype('float') / cm.sum(axis=1)[:, np.newaxis]        cm = np.around(cm, decimals=3)    plt.imshow(cm, interpolation='nearest', cmap=cmap)    plt.title(title)    plt.colorbar()    tick_marks = np.arange(len(classes))    plt.xticks(tick_marks, classes, rotation=45)    plt.yticks(tick_marks, classes)    thresh = cm.max() / 2.for i, j in itertools.product(range(cm.shape[0]), range(cm.shape[1])):        plt.text(j, i, cm[i, j],                 horizontalalignment="center",                 color="white" if cm[i, j] > thresh else "black")    plt.tight_layout()    plt.ylabel('True label')    plt.xlabel('Predicted label')

```

现在，我们可以看到第一个模型的混淆矩阵：

```py
  y_pred = treeClassifier.predict(x_test) cfm = confusion_matrix(y_test, y_pred, labels=[0, 1]) plt.figure(figsize=(10,6)) plot_confusion_matrix(cfm, classes=["<=50K", ">50K"], normalize=True)

```

我们发现多数类别（`<=50K`）的精度为 `90.5%`，少数类别（`>50K`）的精度只有 `60.8%`。

让我们看一下调校此简单分类器的方法。我们能使用带有 `5` 折交叉验证的 `GridSearchCV()` 来调校树分类器的各种重要参数。

```py
  from sklearn.model_selection import GridSearchCV
parameters = {'max_features':(None, 9, 6),'max_depth':(None, 24, 16),'min_samples_split': (2, 4, 8),'min_samples_leaf': (16, 4, 12)}

clf = GridSearchCV(treeClassifier, parameters, cv=5, n_jobs=4)
clf.fit(x_train, y_train)
clf.best_score_, clf.score(x_test, y_test), clf.best_params_
(0.85934092933263717,
 0.85897672133161351,
 {'max_depth': 16,
  'max_features': 9,
  'min_samples_leaf': 16,
  'min_samples_split': 8})

```

经过优化，我们发现精度上升到了 `85.9%`。在上方，我们也可以看见最优模型的参数。现在，让我们看一下 已优化模型的混淆矩阵（confusion matrix）：

```py
  y_pred = clf.predict(x_test) cfm = confusion_matrix(y_test, y_pred, labels=[0, 1]) plt.figure(figsize=(10,6)) plot_confusion_matrix(cfm, classes=["<=50K", ">50K"], normalize=True)

```

![](img/65d90408d895c0dbd258696acb7d289e.jpg)

经过优化，我们发现在两种类别下，预测精度都有所提升。

## 决策树的局限性

决策树有很多优点，比如：

*   易于理解、易于解释
*   可视化
*   无需大量数据准备。不过要注意，`sklearn.tree` 模块不支持缺失值。
*   使用决策树（预测数据）的成本是训练决策时所用数据的对数量级。

但这些模型往往不直接使用，决策树一些常见的缺陷是：

*   构建的树过于复杂，无法很好地在数据上实现泛化。
*   数据的微小变动可能导致生成的树完全不同，因此决策树不够稳定。
*   决策树学习算法在实践中通常基于启发式算法，如贪婪算法，在每一个结点作出局部最优决策。此类算法无法确保返回全局最优决策树。
*   如果某些类别占据主导地位，则决策树学习器构建的决策树会有偏差。因此推荐做法是在数据集与决策树拟合之前先使数据集保持均衡。
*   某些类别的函数很难使用决策树模型来建模，如 `XOR`、奇偶校验函数（parity）和数据选择器函数（multiplexer）。

大部分限制可以通过改善决策树轻易解决。在下面的内容中，我们将介绍相关的几个概念，重点介绍袋装和随机森林。

## 剪枝

由于决策树容易对数据产生过拟合，因此分支更少（即减少区域 `R_1`, … , `R_J`）的小树虽然偏差略微高一点，但其产生的方差更低，可解释性更强。处理上述问题的一种方法是构建一棵树，每个分支超过某个（高）阈值造成叶结点误差率 `Qm` 下降，则结束构建。但是，由于分裂算法的贪婪本质，它其实很短视。决策树早期看似无用的一次分裂有可能会导致之后一次优秀的分裂，并使得 `Qm` 大幅下降。

因此，更好的策略是构建一个非常大的树 `T_0`，然后再剪枝，得到一棵子树。剪枝可以使用多种策略。代价复杂度剪枝（Cost complexity pruning），又叫最弱连接剪枝（weakest link pruning），就是其中一种行之有效的策略。除了考虑每一个可能的子树之外，还需要考虑由非负调参（nonnegative tuning parameter）`α` 索引的树序列。每一个 `α` 值都对应一个尽可能小的子树 `T⊂T_0`。

![](img/4bd732bbaee38603e143e24a7effe365.jpg)

这里 `∣T∣` 代表树 `T` 中叶结点的数量，`R_m` 代表第 `m` 个叶结点对应的矩形（预测器空间的子集），`yhat_Rm` 是 `Rm` 的预测值，即 `Rm` 中训练样本预测值的均值（或分类树中的模式响应）。调整参数 `α` 控制子树复杂度之间的权衡，对训练数据进行拟合。当 `α= 0` 的时候，子树 `T` 等同于 `T_0`。当 `α` 的值增长时，构建具备多个子结点的树需要付出代价，这样，要想得到更小的子树，上述公式将达到最小化。我们可以使用某种交叉验证方法选择剪枝参数 `α` 。

注意，目前 `sklearn.tree` 决策树分类器（和回归器）不支持剪枝。

## 袋装（Bootstrap Aggregating——Bagging）

在统计学中，`Bootstrap` 是依靠替换随机采样的任意试验或度量。我们从上文可以看见，决策树会受到高方差的困扰。这意味着如果我们把训练数据随机分成两部分，并且给二者都安置一个决策树，我们得到的结果可能就会相当不同。`Bootstrap` 聚集，或者叫做袋装，是减少统计学习方法的方差的通用过程。

给定一组 `n` 个独立的样本观测值 `Z_1`，`Z_2`，...，`Z_n`，每一个值的方差均为 `σ*2`，样本观测值的均值方差为 `σ*2`/`n`。换句话说，对一组观测值取平均会减小方差。因此一种减小方差的自然方式，也就是增加统计学习方法预测精度的方式，就是从总体中取出很多训练集，使用每一个训练集创建一个分离的预测模型，并且对预测结果求取平均值。

这里有一个问题，即我们不能获取多个训练数据集。相反，我们可以通过从（单一）训练数据集提取重复样本进行自助法（bootstrap）操作。在这种方法中，我们生成了 `B` 个不同的自助训练数据集。我们随后在第 `b` 个自助训练数据集得到了一个预测结果![](img/867e03da187d19f313bb9af52e2df969.jpg)，从而获得一个聚集预测（aggregate prediction）。

![](img/6f05f84cdc0cc0897669b525e4f00d66.jpg)

这就叫做袋装（bagging）。注意，聚集（aggregating）在回归和分类问题中可能有不同的均值。当平均预测值在回归问题中的效果很好时，我们将会需要使用多数票决（majority vote）：由于分类问题中的聚集机制，整体预测就是在 `B` 个预测值中最常出现的那个主要类别。

## Out-of-Bag（OOB）误差

`Bagging` 方法最大的优势是我们可以不通过交叉验证而求得测试误差。回想一下，`Bagging` 方法的精髓是多棵树可以重复地拟合观察样本的自助子集。平均而言，每一个袋装树可以利用 `2/3` 的观察样本。而剩下的 `1/3` 观察样本就可以称为 out-of-bag (`OOB`) 观察样本，它们并不会拟合一一棵给定袋装树。我们可以使用每一棵树的 `OOB` 观察样本而计算第 `i` 个观察样本的预测值，这将会导致大约有 `B/3` 的预测值可以预测第 `i` 个观察样本。现在我们可以使用和 `Bagging`（平均回归和大多数投票分类）类似的聚集技术，我们能获得第 `i` 个观察样本的单一预测值。我们可以用这种方式获得 `n` 个观察样本的 `OOB` 预测，因此总体的 `OOB` `MSE`（回归问题）和分类误差率（分类问题）就能计算出来。`OOB` 误差结果是 `Bagging` 模型测试误差的有效估计，因为每一个样本的预测值都是仅仅使用不会进行拟合训练模型的样本。

## 特征重要性度量

通过使用单一树，`Bagging` 通常会提升预测的精确度。但是，解释最终的模型可能很困难。当我们袋装大量的树时，就不再可能使用单一的树表征最终的统计学习流程，因此，`Bagging` 是以牺牲阐释性能力为代价来提升预测精确度的。有趣的是，一个人可使用 `RSS`（用于 `bagging` 回归树）或者基尼指数（用于 `bagging` 分类树）得到每一个预测器的整体总结。在 `bagging` 回归树的情况中，我们可以记录由于所有的 `B` 树上平均的给定预测分子分裂而造成的 `RSS` 减少的所有数量。一个大的值表示一个重要的预测器。相似地，在 `bagging` 分类树的情况下，我们可以添加由于所有的 `B` 树上平均的给定预测分子分裂而造成的基尼系数降低的所有数量。一旦训练完成，`sklearn` 模块的不同袋装树（bagged tree）学习方法可直接访问特征的重要性数据作为属性。

## 随机森林模型

虽然袋装技术（`Bagging`）通过降低方差而提高了一般决策树的预测性能，但它还遇到了其他缺点：`Bagging` 要求我们在自助样本上生成整棵树，这就增加了 `B` 倍计算复杂度。此外，因为基于 `Bagging` 的树是相关联的，预测精度会根据 `B` 而饱和。

随机森林通过随机扰动而令所有的树去相关，因此随机森林要比 `Bagging` 性能更好。随机森林不像 `Bagging`，在构建每一棵树时，每一个结点分割前都是采用随机样本预测器。因为在核心思想上，随机森林还是和 `Bagging` 树一样，因此其在方差上有所减少。此外，随机森林可以考虑使用大量预测器，不仅因为这种方法减少了偏差，同时局部特征预测器在树型结构中充当重要的决策。

随机森林可以使用巨量的预测器，甚至预测器的数量比观察样本的数量还多。采用随机森林方法最显著的优势是它能获得更多的信息以减少拟合数值和估计分割的偏差。

通常我们会有一些预测器能主导决策树的拟合过程，因为它们的平均性能始终要比其他一些竞争预测器更好。因此，其它许多对局部数据特征有用的预测器并不会选定作为分割变量。随着随机森林计算了足够多的决策树模型，每一个预测器都至少有几次机会能成为定义分割的预测器。大多数情况下，我们不仅仅只有主导预测器，特征预测器也有机会定义数据集的分割。

随机森林有三个主要的超参数调整：

*   结点规模：随机森林不像决策树，每一棵树叶结点所包含的观察样本数量可能十分少。该超参数的目标是生成树的时候尽可能保持小偏差。
*   树的数量：在实践中选择数百棵树一般是比较好的选择。
*   预测器采样的数量：一般来说，如果我们一共有 `D` 个预测器，那么我们可以在回归任务中使用 `D/3` 个预测器数作为采样数，在分类任务中使用 `D^(1/2)` 个预测器作为抽样。

## 随机森林模型案例

使用和上文一样的收入数据，现在我们构建一个包含 500 棵树的简单随机森林分类器模型：

```py
  rclf = RandomForestClassifier(n_estimators=500) rclf.fit(x_train, y_train) rclf.score(x_test, y_test)

```

即使没有任何优化，我们仍然发现模型性能可以和已优化决策树分类器相媲美，并且测试分达到了 85.1%。按照下面的混淆矩阵，我们发现简单的随机森林和经过优化的树型分类器表现差不多，其在主要类别（`<=50K` 收入）的预测精度达到了 92.1%，而在少数类别（`>50K` 收入）上达到了 62.6%。

```py
  rclf = RandomForestClassifier(n_estimators=500) rclf.fit(x_train, y_train) rclf.score(x_test, y_test)

```
