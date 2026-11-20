
# 学习率动态调整策略

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/NKcNdrGoyovGjvxCTcac9mn2n6f](https://qqs7y1hozd1.feishu.cn/docx/NKcNdrGoyovGjvxCTcac9mn2n6f)

![](img/0d892b76463cd2016c0c170d600607ea.png)

## 1 什么是学习率

深度网络模型是在一个未知且参数量级很大的复杂函数上进行训练的，训练的目的是找到一套最优的参数，使得网络对应在该函数的最小值处。现在的训练方式多采用批量（`Batch`）训练法，即每次丢进网络一个`Batch`，每个`Batch`中包含一个数据集的子集，`Batch`的大小由自己根据显存大小进行设置（一般越大越好）。在训练的过程中，模型每次输入输出一个`Batch`，就要调整一次参数。当前深度学习中参数的调整基本都是基于梯度下降原理，即输入一个`Batch`，计算模型输出与真实分布的差异对于参数的梯度，基于当前梯度更新参数。梯度下降算法大全传送门。函数在一点沿梯度方向的变化率最大，最大值为该梯度的模。

如图1所示，其中  是第`t + 1`次迭代后的参数值，  是第`t`次迭代后的参数值，`lr`是学习率， 是参数θ 在第`t`次迭代时计算得到的梯度值。从公式中我们可以看出，学习率`lr`控制了参数更新的速度，及模型学习的速度。

![](img/5d0e269c097e78f51af1c31e69f63e95.png)

## 2 为什么要动态调整学习率

拿打高尔夫球举例子，因为把球从起点打进洞和函数优化到最低点很像。把学习率比喻成击球的力气，整个高尔夫球场比喻成待优化的函数，球洞比作函数最低点。在起点的时候，球洞离我们很远，这时候为了减少击球的杆数，需要用大力把球朝着洞口的方向击打；在球离洞口很近的时候，则不需要使用那么大力气，而是需要精准地使用小力气把球朝着洞口的方向击打，不然使大力气很容易让球在洞口的周围来回变换位置但就是不进洞。通过上边的例子就说明了我们为什么要动态调整学习率，根本上还是要让网络朝着正确的方向加速收敛。不至于在最低点附近震荡，也不至于一直缓慢地朝着最低点的方向前进。

## 3 动态调整学习率的几种常见方法

一般地，模型训练和人刻意练习一项技能一样，需要反复地学习才能加固认知。模型也是一样，需要反复学习多次给定的数据集，即要训练多个`epoch`。在`Pytorch`中，给我们提供了很多种动态调整学习率的策略，这些调整策略多是基于`epoch`进行的。下面一一进行讲解。

### 3.1 `lr_scheduler.LambdaLR`

目的：将每个参数组的学习率设置为初始 `lr` 乘以给定的Lambda函数根据`epoch`计算出来的值。

```
CLASS torch.optim.lr_scheduler.LambdaLR(optimizer, lr_lambda, last_epoch=- 1, verbose=False)
```

**参数详情：**

`optimizer` (`Optimizer`)：关联的优化器

`lr_lambda` (`function` or `list`)：给定当前`epoch`计算乘法因子的函数，如果是函数列表，则每一个函数对应一个参数组，这样就可以为不同的参数组设置不同的学习率。

`last_epoch` (`int`)：最后一个`epoch`的索引，默认值为-1.

`verbose` (`bool`)：如果为`True`，为每次更新打印输出，默认值`False`

**例子：**

```
# 假设优化器有两个参数组.
lambda1 = lambda epoch: epoch // 30
lambda2 = lambda epoch: 0.95 ** epoch
scheduler = LambdaLR(optimizer, lr_lambda=[lambda1, lambda2])
for epoch in range(100):
    train(...)
    validate(...)
    scheduler.step()
```

### 3.2 `lr_scheduler.StepLR`

目的：每`step_size`个`epochs`通过`γ`降低每个参数组的学习率。

```
CLASS torch.optim.lr_scheduler.StepLR(optimizer, step_size, gamma=0.1, last_epoch=- 1, verbose=False)
```

**参数详情：**

`optimizer` (`Optimizer`)：关联的优化器

`step_size` (`int`)：学习率更新的周期，每`step_size`个`epochs`更新一次

`gamma` (`float`)：降低学习率的乘法因子，默认值0.1

`last_epoch` (`int`)：最后一个`epoch`的索引，默认值-1

`verbose` (`bool`)：如果为`True`，为每次更新打印输出，默认值`False`

**例子：**

```
# 假设所有参数组的初始学习率为0.05，γ为0.1
# lr = 0.05     if epoch &lt; 30
# lr = 0.005    if 30 &lt;= epoch &lt; 60
# lr = 0.0005   if 60 &lt;= epoch &lt; 90
# ...
scheduler = StepLR(optimizer, step_size=30, gamma=0.1)
for epoch in range(100):
    train(...)
    validate(...)
    scheduler.step()
```

### 3.3 `lr_scheduler.MultiStepLR`

目的：当`epoch`到达设定的标志位时，通过`γ`降低每个参数组的学习率。请注意，这种衰减可能与此调度程序外部对学习率的其他更改同时发生。

```
CLASS torch.optim.lr_scheduler.MultiStepLR(optimizer, milestones, gamma=0.1, last_epoch=- 1, verbose=False)
```

**参数详情：**

`optimizer` (`Optimizer`)：关联的优化器

`milestones` (`list`)：`epoch`索引的列表，必须是升序的，因为`epoch`是越来越大的

`gamma` (`float`)：降低学习率的乘法因子，默认值0.1

`last_epoch` (`int`)：最后一个`epoch`的索引，默认值-1

`verbose` (`bool`)：如果为`True`，为每次更新打印输出，默认值`False`

**例子：**

```
# 假设所有参数组的初始学习率为0.05，γ为0.1
# lr = 0.05     if epoch &lt; 30
# lr = 0.005    if 30 &lt;= epoch &lt; 80
# lr = 0.0005   if epoch &gt;= 80
scheduler = MultiStepLR(optimizer, milestones=[30,80], gamma=0.1)
for epoch in range(100):
    train(...)
    validate(...)
    scheduler.step()
```

### 3.4 `lr_scheduler.ExponentialLR`

目的：对于每个`epoch`，使用`γ`降低每个参数组的学习率。以`γ`为底，`epoch`为指数，

**参数详情：**

`optimizer` (`Optimizer`)：关联的优化器

`gamma` (`float`)：降低学习率的乘法因子，默认值0.1

`last_epoch` (`int`)：最后一个`epoch`的索引，默认值-1

`verbose` (`bool`)：如果为`True`，为每次更新打印输出，默认值`False`

### 3.5 `lr_scheduler.CosineAnnealingLR`

目的：使用余弦退火策略动态调整每个`epoch`的学习率。

```
CLASS torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max, eta_min=0, last_epoch=- 1, verbose=False)
```

**参数详情：**

`optimizer` (`Optimizer`)：关联的优化器

`T_max` (`int`)：最大的迭代次数

`eta_min` (`float`)：最小的学习率，默认值0

`last_epoch` (`int`)：最后一个`epoch`的索引，默认值-1

`verbose` (`bool`)：如果为`True`，为每次更新打印输出，默认值`False`

计算公式如下：

### 3.6 `lr_scheduler.ReduceLROnPlateau`

目的：当指标停止改进时便开始降低学习率。一旦学习停滞，将学习率降低 2-10 倍之后，模型通常会继续学习。 该调度程序读取一个指标数量，如果一定数量的 `epoch` 后仍没有改善，则学习率会降低。

```
CLASS torch.optim.lr_scheduler.ReduceLROnPlateau(optimizer, mode='min', factor=0.1, patience=10, threshold=0.0001, threshold_mode='rel', cooldown=0, min_lr=0, eps=1e-08, verbose=False)
```

**参数详情：**

`optimizer` (`Optimizer`)：关联的优化器

`mode` (`str`)：有`min`和`max`两种可选。在`min`模式下，当模型指标停止下降时开始调整学习率；在`max`模式下，当模型指标停止上升时开始调整学习率

`factor` (`float`)：降低学习率的乘法因子，`lr_{new} = lr * factor`

`patience` (`int`)：耐心值，不是指标停止改变后立马就调整学习率，而是`patience`个`epoch`之后指标仍没有改变，便开始调整学习率，即该策略对指标的变化有一定的容忍度。默认值10

`threshold` (`float`)：衡量新的最佳阈值，只关注重大变化。默认值：1e-4

`cooldown` (`int`)：在 `lr` 减少后恢复正常操作之前要等待的 `epoch` 数

`min_lr` (`float` or `list`)：每个参数组设定的最小学习率

`eps` (`float`)：如果调整之后的学习率和调整之前的学习率差距小于`eps`，则忽略此次调整

`verbose` (`bool`)：如果为`True`，为每次更新打印输出，默认值`False`

**例子：**

```
optimizer = torch.optim.SGD(model.parameters(), lr=0.1, momentum=0.9)
scheduler = ReduceLROnPlateau(optimizer, 'min')
for epoch in range(10):
    train(...)
    val_loss = validate(...)
    # Note that step should be called after validate()
    scheduler.step(val_loss)
```

### 3.7 `lr_scheduler.CyclicLR`

目的：根据周期性学习率策略调整每个参数组的学习率。该策略以恒定频率在两个边界之间循环调整学习率，详见论文 Cyclical Learning Rates for Training Neural Networks。 两个边界之间的距离可以在每次迭代或每个周期的基础上进行缩放。循环学习率策略在每个`Batch`之后改变学习率。 `step()`方法应该在一个`Batch`用于训练后调用。

该方法有三种循环策略：

`triangular`：没有幅度缩放的基本三角循环

`triangular2`：一个基本的三角循环，每个循环将初始幅度缩放一半

`exp_range`：每个循环之后，将初始幅度按照 `γ` 进行降低

**参数详情：**

`optimizer` (`Optimizer`)：关联的优化器

`base_lr` (`float` or `list`)：初始学习率，它是每个参数组循环中的下边界

`max_lr` (`float` or `list`)：每个参数组在循环中的上层学习率边界。 从功能上讲，它定义了周期幅度 (`max_lr` - `base_lr`)。 任何周期的 `lr` 是 `base_lr` 和一些幅度缩放的总和； 因此 `max_lr` 实际上可能无法达到，具体取决于缩放函数

`step_size_up` (`int`)：在一个周期中，学习率上升阶段中的训练迭代次数。 默认值：2000

`step_size_down` (`int`)：在一个周期中，学习率下降阶段中的训练迭代次数。如果设置为`None`，则等于`step_size_up`

`mode` (`str`)：{`triangular`, `triangular2`, `exp_range`}三选一。如果`scale_fn`不是`None`，忽略此参数。默认值`‘triangular’`

`gamma` (`float`)：`‘exp_range’`模式中的缩放常数。缩放公式：`γ^(cycle-iterations)`，默认值1.0

`scale_fn` (`function`)：由单个参数 `lambda` 函数定义的自定义缩放策略，其中对于任意`x >= 0`，`0 <= scale_fn(x) <= 1`。如果指定，则忽略“`mode`”。 默认值`None`

`scale_mode` (`str`)：{`'cycle'`, `'iterations'`}二选一。定义是否在循环数或循环迭代（自循环开始以来的训练迭代）上评估 `scale_fn`。 默认值“`cycle`”

`cycle_momentum` (`bool`)：动量与`"base_momentum"`和`"max_momentum"`之间的学习率成反比。 默认值`True`

`base_momentum` (`float` or `list`)：每个参数组的循环中较低的动量边界。 请注意，动量循环与学习率成反比； 在一个周期的峰值，动量是`"base_momentum"`，学习率是`"max_lr"`。 默认值0.8

`max_momentum` (`float` or `list`)：每个参数组在循环中的上动量边界。在功能上，它定义了周期幅度(`max_momentum` - `base_momentum`)。 任何周期的动量是 `max_momentum` 和一些幅度缩放的差值； 因此 `base_momentum` 实际上可能无法达到，具体取决于缩放函数。请注意，动量循环与学习率成反比；在循环开始时，动量为`"max_momentum"`，学习率为`"base_lr"` 默认值0.9

`last_epoch` (`int`)：最后一个`Batch`的索引，恢复训练时使用此参数。 因为 `step()` 方法应该在每个`Batch`之后而不是每个 `epoch` 之后调用，所以这个数字表示计算的`Batch`总数，而不是计算的 `epoch` 总数。 当 `last_epoch` = -1 时，学习率调度从头开始。 默认值-1

**例子：**

```
optimizer = torch.optim.SGD(model.parameters(), lr=0.1, momentum=0.9)
scheduler = torch.optim.lr_scheduler.CyclicLR(optimizer, base_lr=0.01, max_lr=0.1)
data_loader = torch.utils.data.DataLoader(...)
for epoch in range(10):
    for batch in data_loader:
        train_batch(...)
        scheduler.step()
```
