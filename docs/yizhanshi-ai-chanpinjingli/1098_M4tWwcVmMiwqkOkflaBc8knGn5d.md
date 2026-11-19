
# 热门技术相关

> 来源：[https://v11enp9ok1h.feishu.cn/docx/DveUdYHe4o5xDvxO4zBctw4tn0f](https://v11enp9ok1h.feishu.cn/docx/DveUdYHe4o5xDvxO4zBctw4tn0f)

## 3.3.1 Embedding

+ 介绍下`item2vec`模型
+ `embedding`冷启动怎么做
+ `w2v`的原理了解嘛，详细讲
+ `w2v`的原理，用的什么方法，`skip-gram`怎么做
+ `Deepwork`的思想
+ `Node2vec`随机游走策略
+ `word2vec`的`loss`怎么设计的
+ `word2vec`的工具
+ 用户`embedding`了解嘛
+ `word2vec`对顺序敏感的原因，窗口大小选用多少
+ `DeepWalk`相比序列`embedding`优缺点，对那一部分`item`影响最大
+ `w2v`用的哪一个，是google13年的那篇嘛

## 3.3.2 多任务学习

+ 多任务学习模型的发展历史详细介绍
+ 为什么要用多任务学习
+ 介绍`MMOE`、`PLE`、`ESMM`，`PLE`相对`MMOE`的改进
+ `ESSM`算法原理和解决的两个问题
+ `ESMM`中如何解决`CVR`样本过于稀疏的问题，实际上解决了吗
+ `ESMM`训练是否使用全量样本
+ 介绍`PLE`模型
+ `PLE`里面`loss`如何平衡
+ `PLE`模型中，是否有尝试对不同的`gate`用不同的特征，是否有尝试不同业务用不同的特征组合
+ `gradnorm`介绍
+ 介绍关于多任务权重设置的相关模型或者策略
+ 如何平衡不同任务的`loss`
+ 如果一个特征对任务`a`是正相关，对任务`b`是负相关，如何处理这个特征
+ `CTR`和`CVR`任务放在`ESMM`（都是曝光空间）里和放在`PLE`（`CTR`点击空间，`CVR`曝光空间）里哪种效果好

## 3.3.3 图神经网络

+ `GNN`在推荐系统中有哪些用法
+ `GCN`、`GraphSAGE`、`GAT`的区别与联系
+ `node2vec`对比`deepwalk`的改进
+ `GraphSAGE`对比`GCN`的优势
+ `transductive`与`inductive`的区别
+ 训练图模型的`loss`有哪些
+ `graph embedding`的作用
+ `GCN`图卷积
+ 怎么减少感受野
+ 讲一下`GCN`，为什么用拉普拉斯矩阵，优缺点
+ `GCN`图谱了解嘛
+ 同质图？异质图？
+ 边类型不一样是异质图么
+ 什么是`local structure`和`global structure`
