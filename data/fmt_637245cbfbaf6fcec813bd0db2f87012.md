
```python
from __future__ import annotations

import matplotlib.pyplot as plt
import numpy as np
from vizier.service.clients import environment_variables
from vizier.service import clients, resources, vizier_client
from vizier.service import pyvizier as vz

def main() -> None:
    clients.environment_variables.server_endpoint = 'ip:port'
    # study_config = vz.StudyConfig()  # Search space, metrics, and algorithm.
    # root = study_config.search_space.root
    owner = 'e'
    study_id = 'gmnet'
    study_resource_name = resources.StudyResource(
        owner_id=owner, study_id=study_id
    ).name
    client = vizier_client.VizierClient(
        vizier_client.create_vizier_servicer_or_stub(
            environment_variables.server_endpoint
        ),
        study_resource_name,
        '4',
    )
    trial_filter = vz.TrialFilter(status=[vz.TrialStatus.COMPLETED])

    all_trials = client.list_trials()
    mIoU = []
    lr = []
    for t in filter(trial_filter, all_trials):
        print('trail:')
        print(t)
        # trials.append(t)
        iou = t.final_measurement.metrics['mIoU'].value
        l = t.parameters['backbone_lr'].value
        mIoU.append(iou)
        lr.append(l)
    mIoU = np.asarray(mIoU).transpose()
    lr = np.asarray(lr).transpose()
    # x = np.linspace(0, 2, 100)
    plt.scatter(lr, mIoU)
    plt.show()

if __name__ == '__main__':
    main()
```

## 输出为

```
[Trial(parameters=ParameterDict(_items={'backbone_lr': 0.00013287588294651868}), metadata=Metadata((namespace:, items: {}), current_namespace=), id=23, is_requested=False, assigned_worker='3', stopping_reason=None, _infeasibility_reason=None, description='owners/e/studies/gmnet/trials/23', related_links={}, final_measurement=Measurement(metrics={'mIoU': Metric(value=0.5949919441762046, std=None)}, elapsed_secs=0.0, steps=0), measurements=[], creation_time=02/25/23 18:15:47, completion_time=None)]
Pareto-optimal trial found so far has parameters ParameterDict(_items={'backbone_lr': 0.00013287588294651868}) and metrics {'mIoU': Metric(value=0.5949919441762046, std=None)}
```

## 得到 `mIoU` 和学习率 `lr` 的关系图

![](img/64e3528fb8041dbd361e6c572e0ed323.png)

![](img/c688efeef6306607766d21295821fb80.png)

按照上面左图可以看出，最优的区间在 0.00012 到 0.00018 之间。同步如右图绘制验证集损失和初始学习率的关系后可以看出最优的学习率的区间应该在 0.00012 到 0.00015 之间。

> （需要注意的是 `vizier` 0.1.3 之前的版本在做伪随机搜索的时候，存在不均匀的问题，这个问题在 0.1.3 以后得版本已经修改）

使用贝叶斯优化对模型进行进一步优化。

## 只要将算法改成

`GAUSSIAN_PROCESS_BANDIT`

然后直接进行实验。

## 发现报错

`Jax transforms and Flax models cannot be mixed.`

重新安装 `ossVizier` 到 0.1.2 版本。解决该问题。

## 使用贝叶斯优化后得到

![](img/b1989970c33bf86bcc30f8d335ddfbb0.png)

发现在 0.0001 的地方，训练轮次之间的偏差过大。这个直接影响了最终判断。

使用 `SGD` 优化器进行伪随机搜索。

## 使用的学习率的更新策略为

```
lr_policy: warmup_cosine
lr_step_per_batch: False
warm_up_iter: 10
T_max: 40
lr_max: 1.33e-4
lr_min: 1e-8
gamma: 0.99
```

## `vizier` 的配置为

```
base_cfg: './config/tokyo_gmnet_train.config'
search_space:
  lr_max:
    type: 'float'
    min: 0.0001
    max: 0.1
    scale_type: 'LINEAR'
```

## 伪随机搜索得到的结果

![](img/aaef1561276af8fe9b913f575eaaa914.png)

验证集上，`miou` 超过 0.56 的采样点如下：

> `lr is 0.008992095967644217 iou is 0.6097086748570127`
> `lr is 0.006673528863708799 iou is 0.5629044428652298`
> `lr is 0.010705819479248656 iou is 0.5962651373131723`
> `lr is 0.005665456209823834 iou is 0.5874034043911056`
> `lr is 0.011814699398522118 iou is 0.5878719203422849`
> `lr is 0.00576626347521233 iou is 0.5699906138827684`
> `lr is 0.006975950659874287 iou is 0.579933796565994`

同时查看学习率为这些值的时候。整个模型的收敛程度：

![](img/f63f667ecf51fdce73c12437ced3171e.png)

可以看出模型基本是收敛的。

可以看到最优的区间应该在 [0.005，0.012] 之间，我们在这个区间内，使用贝叶斯优化搜索最优的学习率。使用贝叶斯优化的时候，0.1.3 的版本中又出现了问题：

> `AttributeError: module 'tensorflow_probability.substrates.jax.experimental' has no attribute 'bayesopt'`

之前是 `v0.1.1` 版本有这个问题，但是 `v0.1.2` 的版本中没有这个问题了。`v0.1.3` 的版本中又出现了。看了应该是 `tensorflow_probability` 版本的问题。

```
pip install -Uq tfp-nightly[jax]
```

## 优化之后得到

![](img/ad6ad38ff2cc66a32da7149ba22bb1ad.png)

![](img/90246a68bedc9d833f12e19de8bf4621.png)

从上面左图可以看到最优点为 0.012 在边界，因此需要放宽边界，重新搜索，我们将边界设为 [0.005，0.2] 继续搜索。

如上面右图，输出结果，由于训练之间的误差过大，只能简单的说明大概的趋势。

所以怎么减少训练之间的偏差，非常重要。此外对比 `sgd` 和 `adamw` 优化器，发现最后优化出来的效果相似。这也和谷歌调参手册中的描述实现了一致。

## 附：谷歌深度调参手册原文

### 适用场景

> `1.图像生成:` `GANS` 可以用于生成新的图像，这在艺术和设计中有很大的应用。例如，DeepArt 和 Prisma 这样的应用就使用了 `GANS` 来将用户的照片转换成艺术品。

> `2.超分辨率:` `GANS` 可以用于提高图像的分辨率，这在图像处理和视频流领域非常有用。例如，有些电视和电脑显示器使用 `GANS` 技术来提高低分辨率内容的质量。

> `3.虚拟现实:` 在虚拟现实 (`VR`) 和增强现实 (`AR`) 中，`GANS` 可以用于生成逼真的虚拟环境和对象。3.

> `4.医疗图像分析:` `GANS` 可以用于生成医疗图像，如 `CT` 扫描或 `MRI` 图像，这对于训练医疗图像分析 `4.` 算法非常有用。

> `5.语音生成:` 在语音识别和生成领域，`GANS` 可以用于生成逼真的语音，这在智能助手和语音合成中有很大的应用。

> `6.数据增强:` 在机器学习中，`GANS` 可以用于生成额外的训练数据，这可以帮助改善模型的性能。`7.深度伪造:` `GANS` 可以用于生成逼真的假视频和音频，这被称为“深度伪造”。虽然这在一些领域 (如电影制作) 有其用途，但也引发了一些关于隐私和安全的问题。

### 12条经验分享

1. 调参一定要做好笔记，每次只调 1 个参数。

2. 在 `Validation` 筛选模型参数时，可以除 `loss` 函数“外，设置某种规则引导模型向某个想要的方向去更新参数。

3. 关于 `Activation function`:

   (1) `ReLu`、`Sigmoid`、`Softmax`、`Tanh` 是最常用的 4 个激活函数°

   (2) 对于输出层，常用 `sigmoid` 和 `softMax` 激活函数，中间层常用 `ReLu` 激活函数，`RNN` 常用 `Tanh` 激活函数。

4. 关于 `Weights initialization`:

   预训练参数是最好的参数初始化方式，其次是 `Xavir`。

5. 关于 `Regularization`:

   `Dropout` 虽然思想很简单，但效果出奇的好，首选 `0.5`。

6. `Batch normalization` 很好用。

7. 如果在 `train` 模型时，`loss` 可以降到非常低，那么模型的学习能力是够的；如果 `train` 能达到，`validation` 达不到，则说明模型泛化能力不强。

8. 关于调参:

   有哪些参数可以调呢?

   `Loss function`、`Learning rate`、`Batch size`、`Epoch number`、`Optimizer`、`Activationfunction`、`Weights initialization`、使用 `Regularization`、`Validation`、使用的 `GPU` 个数。

9. 关于 `Loss function`:

   `Loss function` 是 `Model` 和数据之外，第三重要的参数。具体使用 `MSE`、`Cross entropy`、`Focal` 还是其他自定义，需要具体问题具体分析。

10. 关于 `Epoch number` 和 `early stopping`:

    (1) `Epoch number` 和 `Early stopping` 是息息相关的，需要输出 `loss` 看一下，到底是什么 `epoch` 时效果最好，及时 `early stopping`。

    (2) `Epoch` 越大，会浪费计算资源；`epoch` 太小，则训练模型提取特征没到极致。

    (3) 此外，也要明白 `Epoch`、`lteration`、`batchsize` 的关系。

11. 关于 `Optimizer`:

    (1) `Adam` 和 `SGDM` 是最常用的两个，前者能快速收敛°，后者收敛慢但最终精度更高。现在大家会先使用 `Adam` 快速收敛，后面再用 `SGDM` 提升精度。

    (2) 如果必须二选一的话，我会推荐 `Adam`。

12. 关于 `Learning rate` 和 `batch size`:

    (1) `Learningrate` 和 `batch size` “是两个重要的参数，而且二者也是相互影响的，在反向传播时直接影响梯度。一般情况下，先调 `batchsize`，再调 `learning rate`

    (2) `batchsize` 不能太大，也不能太小；太小会浪费计算资源，太大则会浪费内存；一般设置为 `16` 的倍数。

    (3) 如果使用微调，则 `learningrate` 设置为 `0.0001` 较好。`learningrate` 设置上有很多 `trick`，包括 `cosing learning rate` 等。
