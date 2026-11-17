
# 腾讯混元、北大发现Scaling law「浪涌现象」，解决学习率调参难题

> 来源：[https://agijuejin.feishu.cn/docx/OQPyddxvkoSkY3xwLRdcj3a7nDe](https://agijuejin.feishu.cn/docx/OQPyddxvkoSkY3xwLRdcj3a7nDe)

> 机器之心｜[阅读原文](https://www.jiqizhixin.com/articles/2024-06-06)

> 转载请联系原作者取得授权

过去十年间，基于随机梯度下降（`SGD`）的深度学习模型在许多领域都取得了极大的成功。与此同时各式各样的`SGD`替代品也如雨后春笋般涌现。在这些众多替代品中，`Adam`及其变种最受追捧。无论是`SGD`，还是`Adam`，亦或是其他优化器，最核心的超参数非`Learning rate`莫属。因此如何调整好`Leanring rate`是炼丹师们从一开始就必学的技能。

从直觉上讲，影响`Learning rate`取值的重要因素是`Batch size`。不知你在学习炼丹术时，是否遇到或者思考过入如下问题：

+ 我的`Batch size`增加一倍，`Learning rate`该怎么调整？

+ 网上有说`Batch size`和`Learning rate`是线性放缩，也有说是平方根放缩，到底该按照哪个调整？

+ 为什么我按照网上说的经验关系调整之后效果反而变差了？

针对上述问题，`腾讯 混元`联合北京大学基于现有科研基础和实际业务需求，在进行了大量理论分析和实验验证后发布了关于`Batch size`和`Learning rate`放缩关系的调参指南：

![](img/95968d426f9e0d07121d6afcb3da8090.png)

+ 论文：Surge Phenomenon in Optimal Learning Rate and Batch Size Scaling

+ 论文地址：https://arxiv.org/pdf/2405.14578

1. 当使用`SGD`风格的优化器时，应当采用OpenAI 2018 年给出的结论（https://arxiv.org/pdf/1812.06162）：

![](img/f008a1cb4ef4d831a2dd0e6a1171c30b.png)

2. 但是当使用`Adam`风格的优化器时，需要按照如下放缩规律：

![](img/084f186a9febe6df020831a29c79081c.png)

![](img/2993c3f6fc84d5845548eeb680cce5cd.png)

## 一、居然要降低 学习率 ？

![](img/0f61b6a9415fbab2d9f2161055231c6b.png)

![](img/7c04dcac876f93644b870268ec8898c0.png)

![](img/0fdef4600b1bfd602523afe29d92ff4f.png)

## 二、观察到的下降区间

![](img/f2a94a4c0530e0b953d0a3dcaeeca63d.png)

![](img/0dd58e02bf4a6e7e6b9cdc90655b9523.png)

![](img/65118bd9f26a90520519637c80992b09.png)

![](img/0b20a699f4325810f7d1506e6c72e6ec.png)

![](img/7b50f4d1e90897d713cd79e0613bf283.png)

以及`Resnet18`在`TinyImagenet`，和`DistilGPT2`在`Eli5Category`上也观察到了类似现象。

## 三、浪涌现象

![](img/c4d4da57674c70843d4dbfbd3d2645bf.png)

![](img/a9be7fbfd6174e5f33094cd52c7da5b1.png)

## 四、理论发现

![](img/76faa2b52028d4c31b12091c71ff9a13.png)

![](img/674bea2c46b3b844ab30e498885de1e3.png)

表明，`Batch size`无限大时最优学习率趋于一个饱和值。

## 五、应用

我们在`腾讯 Angel`大模型训练框架中集成了上述理论成果，并在`腾讯 混元`大模型训练任务中对理论进行进一步验证，未来将服务于各种大模型训练场景。

感谢阅读，更多详细内容，请参考原文。
