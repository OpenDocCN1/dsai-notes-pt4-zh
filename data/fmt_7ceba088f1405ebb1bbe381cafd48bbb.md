
## 权重初始化

修定好你的网络并尝试不同的初始化方案。记住，权重是你一直想要找到的模型的实际参数。有许多套权重能给出好的表现，但你需要的是更好的表现。

* 尝试所有的初始化方法，看有没一个是最好的。
* 尝试用自编码器（autoencoder）这样的无监督方法进行预学习。
* 为了解决你的问题，尝试使用已有的方法重复训练新的输入和输出层（迁移学习）

记住，改变权重初始化方法会影响到激活函数，甚至是优化函数。

## 3）学习率

调整学习率总会有所收获。下面是一些可以探索的方法：

* 用超大或超小的学习率进行试验
* 从文献中找到常用的学习率值，看你能将网络改进到什么地步
* 尝试随着 `epoch` 降低学习率。
* 尝试经过一定量的 `epoch` 训练后，就按一定概率降低学习率
* 尝试增加一个动量项，然后同时对学习率和动量进行网格搜索

越大的网络需要越多的训练，反之亦然。如果你增加更多的神经元或更多的层，请增加你的学习率。学习率与训练 `epoch` 的数量、`batch` 的大小、优化方法是紧密相关的。

相关推荐：

* 在 `Python` 中对深度学习模式使用学习率方案：`http://machinelearningmastery.com/using-learning-rate-schedules-deep-learning-models-python-keras/`
* 反向传播应该使用什么样的学习率？：`ftp://ftp.sas.com/pub/neural/FAQ2.html#A_learn_rate`

## 4）激活函数

你或许应该使用 `rectifier` 激活函数。

它们用起来更好。

在那之前，在输出层上，一开始是 `sigmoid` 和 `tanh` 函数，然后是一个 `softmax` 函数、线性函数或者 `sigmoid` 函数。我不推荐做更多的尝试，除非你知道你在做什么。

尝试这三个函数并且调整你的数据以满足这些函数的边界。

很明显，你想选择适合输出的形式的传递函数（transfer function），但是要考虑利用不同的表征。

例如，从二元分类（binary classification）的 `sigmoid` 函数切换到解决回归问题的线性函数，然后后处理（`post-process`）你的输出。

这或许也需要将损失函数换成某些更加适合的东西。下面是关于数据转换的更多的想法。

相关推荐：

* 为什么要使用激活函数：`ftp://ftp.sas.com/pub/neural/FAQ2.html#A_act`

## 5）网络拓扑

改变你的网络结构会有回报。

你需要多少层和多少个神经元？

没人只知道，所以别问。

你必须为你的问题开发出好配置。试验。

* 尝试一个隐藏层包含很多个神经元（宽）
* 尝试每层只有少量神经元的深度网络（深）
* 尝试将以上结合起来
* 从最新的论文中找出与你类似的架构并尝试它们
* 尝试拓扑模式（扇出然后扇入）和书与论文中的好的经验规则（见下面链接）

后面的网络需要更多的训练，在 `epochs` 和学习率上都需要。做相应的调整。

相关推荐：

下面的链接可以给你很多尝试的想法，对我很有用。

* 我应该用多少个隐藏层？：`ftp://ftp.sas.com/pub/neural/FAQ3.html#A_hl`
* 我应该用多少个隐藏单元？：`ftp://ftp.sas.com/pub/neural/FAQ3.html#A_hu`

## 6）Batches 和 Epochs

`Batch` 的大小限定了梯度以及多久更新权重。一个 `epoch` 是分批（`batch-by-batch`）暴露给网络的整个训练数据。你试验过不同的 `batch` 大小和 `epochs` 量吗？

上面我已经谈过学习率、网络大小和 `epochs` 之间的关系了。带有大 `epoch` 的小 `batch` 和大量的训练 `epoch` 在现在的深度学习部署中很常见。以下这些方法可能不符合你的问题：

* 尝试将 `batch` 大小与训练数据的大小对等，这依赖于内存（`batch learning`）
* 尝试大小为 1 的 `batch`（在线学习）
* 尝试不同 `mini-batch` 大小（8、16、32...）的网格搜索
* 尝试分别训练一些 `epoch` 以及大量的 `epoch`

考虑下接近无限量的 `epoch`，并设立抽查点捕捉最好的表现模型。一些网络架构要比其他架构对 `batch` 的大小更敏感。我认为多层感知机对 `batch` 大小比较稳健，`LSTM` 和 `CNN` 比较敏感，但这只是传闻。

相关推荐：

* What are batch, incremental, on-line, off-line, deterministic, stochastic, adaptive, instantaneous, pattern, constructive, and sequential learning?：`ftp://ftp.sas.com/pub/neural/FAQ2.html#A_styles`
* 直观上，`mini-batch` 的大小如何影响（随机）梯度下降的性能？：`https://www.quora.com/Intuitively-how-does-mini-batch-size-affect-the-performance-of-stochastic-gradient-descent`

## 7）正则化

正则化是遏制过拟合训练数据的很好的方法。最新的热门正则化技术是 `dropout`，你试过吗？`Dropout` 在训练期间随机跳过神经元，并强迫层内其他算法重拾这些神经元。简单而有效，开始 `dropout` 吧！

网格搜索不同的 `dropout` 百分比。

在输入层、隐藏层和输出中试验 `dropout`。

关于 `dropout` 的想法还有一些扩展，可以像 `drop connect`（`http://cs.nyu.edu/~wanli/dropc/`）那样尝试它们。

你也可以考虑其他更传统的神经网络正则化技术，比如：

* 权重衰减以惩罚最大的权重
* 激活约束，以惩罚最大激活

在可被惩罚的不同方面和可以应用的不同类型的惩罚（`L1`，`L2`，`L1` 和 `L2` 同时使用）上进行试验。

相关推荐：

* `Keras` 的深度学习模型中的 `dropout` 正则化：`http://machinelearningmastery.com/dropout-regularization-deep-learning-models-keras/`
* 什么是权重衰减：`ftp://ftp.sas.com/pub/neural/FAQ3.html#A_decay`

## 8）优化和损失

曾经的方法是随机梯度下降，但现在有很多可以优化的方式。你有试过不同的优化程序吗？随机梯度下降是默认的。首先用不同的学习率、动量和学习率计划充分利用它。许多更高级的优化方法会提供更多的参数，更多的复杂性以及更快的收敛性。这是好是坏，取决于你的问题。

为了最大化给定的方法，你真的需要深入到每一个参数，然后根据你的问题网格搜索不同的值。这困难，且耗费时间。但也可能有回报。

我发现新的/流行的方法可以收敛得更快并且能对于一个给定的网络拓扑结构的能力给出一个很快的想法，例如：

* `ADAM`（论文请点击「阅读原文」下载）
* `RMSprop`

你还可以探索其他的优化算法，比如更传统的（`Levenberg-Marquardt`）和不太传统的（遗传算法）。其他的方法可以为随机梯度下降法提供很好的起点和优化的方式。要优化的损失函数和你将要解决的问题是密切相关的。不过，你会有一些回旋的余地（用于回归的 `MSE` 和 `MAE` 度量，等等），你也可能会通过换算你问题的损失函数得到一个小的凸点。这也可能与输入数据的规模和正在使用的激活函数的规模紧密相关。

相关推荐：

* 梯度下降优化算法概述：`http://sebastianruder.com/optimizing-gradient-descent/`
* 什么是共轭梯度，`Levenberg-Marquardt` 等？`ftp://ftp.sas.com/pub/neural/FAQ2.html#A_numanal`
* 深度学习的优化算法，点击「阅读原文」下载

## 9）早停

一旦性能开始下降，你可以终止学习。这可以节省大量的时间，甚至可能让你使用更复杂的重采样方法来评估模型的性能。早停是一种遏制训练数据过拟合的正则化手段，要求你在每一个 `epoch` 中监控训练模型的表现并验证数据集。一旦验证数据集的表现开始下降，训练就可以停止。如果这一条件得到满足（测量精度损失），你还可以设置检查点来保存模型，并允许模型继续学习。检查点可以让你在没有停止的情况下早停，给你几个模型在运行结束时进行选择。

相关推荐：

* 如何在 `Keras` 的深度学习模型中设置检查点：`http://machinelearningmastery.com/check-point-deep-learning-models-keras/`
* 什么是早停？：`ftp://ftp.sas.com/pub/neural/FAQ3.html#A_stop`

## 模型组合（Ensemble）

你可以将多个模型的预测相结合。算法调试后，这是需要改进的下一个大区域。事实上，你可以从多个足够好的模型的预测结合中获取好的表现，而不是多个高度调整（脆弱）的模型。我们会看看你可能要考虑的模型组合的三大领域：

* Combine Models. 模型结合
* Combine Views. 视角结合
* Stacking. 堆

### 1）模型结合

不要选择一个模型，把它们结合起来。如果你有多个不同的深度学习模型，每个模型都在这个问题上的表现良好，那么通过取均值来结合它们的预测。模型越不相同，效果越佳。例如，你可以使用完全不同的网络拓扑结构或者不同的技术。如果每个模型都很灵巧，但方式不同，那么集成预测将更为强劲。或者，你可以用相反的位置进行试验。每次训练网络的时候，你要用不同的权重对这个网络进行初始化，该网络会收敛成一组不同的最终权重。将此过程重复多次，生成许多的网络，然后结合这些网络的预测。

它们的预测将是高度相关的，但它可能会在这些模式上给你一个更难预测的小凸点（`bump`）。

相关推荐：

* 在 `Python` 中用 `scikit-learn` 组合机器学习算法：`http://machinelearningmastery.com/ensemble-machine-learning-algorithms-python-scikit-learn/`
* 如何提高机器学习算法的结果：`http://machinelearningmastery.com/how-to-improve-machine-learning-results/`

### 2）视角结合

正如上文所述，但是以你的问题的一个不同视角或框架来训练每个网络。再一遍，目标是得到熟练的的模型，但是用不同的方式（比如不相关的预测）。你可以依靠非常不同的缩放（`scaling`）和上文中提到的转换技巧。用于训练不同模型的问题的转换和框架越多，就越有可能改善你的结果。运用预测的简单平均将是一个良好的开端。

### 3）层叠

你还可以了解如何最好地结合多种模型的预测。这就是所谓的层叠泛化，简称层叠（`stacking`）。通常情况下，你可以利用像正则回归这样学习如何为不同模型的预测加权的简单线性方法来取得更好均值结果。基准结果使用多个子模型的预测的平均，但是会用学到的模型权重提升表现。

相关推荐：

* 层叠泛化（层叠）：`http://machine-learning.martinsewell.com/ensembles/stacking/`

## 附加资源

有很多很好的资源，但很少能将所有的想法都联系在一起。我将列出一些资源和相关的发布信息，如果你想深入了解，你会发现这很有趣。

相关推荐：

* 神经网络常见问题解答：`ftp://ftp.sas.com/pub/neural/FAQ.html`
* 如何在 `Python` 中使用 `Keras` 网格搜索深度学习模型的超参数：`http://machinelearningmastery.com/grid-search-hyperparameters-deep-learning-models-python-keras/`
* 必须知道的深度神经网络提示/技巧：`http://lamda.nju.edu.cn/weixs/project/CNNTricks/CNNTricks.html`
* 如何增加深度神经网络验证的准确性：`http://stackoverflow.com/questions/37020754/how-to-increase-validation-accuracy-with-deep-neural-net`

©本文由机器之心编译，转载请联系本👸号获得授权。

✄------------------------------------------------

**加入机器之心（全职记者/实习生）：hr@almosthuman.cn**

**投稿或寻求报道：editor@almosthuman.cn**

**广告&商务合作：bd@almosthuman.cn**

![](img/a573ff7d72f49f8fe283857b964d06fd.jpg)
