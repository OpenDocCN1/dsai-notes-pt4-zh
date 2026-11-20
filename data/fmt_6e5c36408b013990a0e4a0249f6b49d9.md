
# 将知识图谱引入推荐系统？推荐系统

> 来源：`https://v11enp9ok1h.feishu.cn/docx/UE7VdQkR6oRJAUx3z8bclLn3nHf`

`https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie`

推荐系统的目标是满足用户个性化需求，降低用户筛选信息的难度。推荐系统有着广泛地应用，在MDB的电影推荐中可以推荐更多人喜欢的电影，在Amazon的图书推荐会推荐与当前书相似的书，Booking.com会推荐可能会感兴趣的旅游的目的地，Quora上对用户可能感兴趣问答的推荐，Tik tok上对用户可能感兴趣的短视频推荐，以及音乐网站对用户可能感兴趣的音乐推荐。

![](img/2a2b2181b219f4a7d052da513d6fd82c.png)

推荐系统有两大任务，评分预测和点击率预测：

+   评分预测：是给定用户对物体的打分，可以直接地反应用户喜好程度，因此称为显式反馈（Explicit feedback）

+   点击预测：是给定用户对物体的点击情况，并不能显式地反应出用户偏好，因此称为隐式反馈（Implicit feedback），隐式反馈收集难度更低，因此真实线上环境中常用Implicit feedback

## 协同过滤 collaborative filtering

协同过滤（Collaborative Filtering, `CF`）假定相似用户有相似偏好，根据用户历史行为算出用户相似度，并用相似度加权平均其他用户的评分来预测该用户未打分物体的评分。

![](img/0c8f8d1dcd31a7a44bc66de827e0653f.png)

### CF存在的缺点：

+   用户、物体矩阵存在稀疏性，用少量数据预测大量缺失的数据存在过拟合的风险。

+   冷启动问题，难以解决新用户和新物品没有历史数据的情况。

## CF + Side Information

通过引入辅助信息（Side Information）来解决协同过滤存在的问题，常用的辅助信息包括：社交网络比如🧣间关注的关系、用户物品的属性信息、图片文本音频视频等多媒体信息、上下文信息比如购买时的位置时间信息。

## 知识图谱

> 知识图谱（Knowledge Graph, `KG`）也可以看作一种辅助信息，`KG`是一种有向异构图，它的节点表示实体，边表示实体间关系。一个`KG`通常包含多个三元组，形如（`head`, `relation`, `tail`），表示头实体与尾实体存在某种关系。我们假定推荐系统中的物品也是一个`KG`中的节点，因此`KG`提供了物品和物品之间的关系。

![](img/4b72eed69b33817d58e63f0c00740f05.png)

### 知识图谱实例

> 以电影推荐为例：一个用户看过的电影可以靠`KG`中的实体连接到其他电影，通过合理推断，可以认为用户也会喜欢与该电影紧密连接的电影，因此从电影的属性和特征出发，一个`KG`可以帮助我们合理地推测用户的兴趣。

![](img/87cebf1ece8aed68e9d7357100c2d15f.png)

![](img/837a687c54ff5e7d399feec3314075cf.png)

> 以新闻推荐为例：可以通过用户阅读过的新闻实体，利用`KG`重复扩展并连接到另一条新闻。虽然下图中上下两条新闻字面上没有重合，但可以通过常识知识图谱进行判断关联非常紧密。

## 知识图谱嵌入 Knowledge Graph Embedding

> 知识图谱嵌入（Knowledge Graph Embedding, `KGE`）可以解决知识图谱作为复杂图结构难以直接利用的问题，`KGE`可以学习`KG`中实体关系的低维向量表示。

### 常用转移距离模型（Translate Distance Model）

+   `TransE`目标是使`head`实体embedding加上`relation` embedding接近`tail`实体embedding。

![](img/ed77d0b6e9002f8cf3c840ea77b7895d.png)

+   `TransH`解决一对多多对多关系，通过计算`head`和`tail`实体embedding在关系embedding上的投影，计算投影之间的关系。

![](img/0619bc7fd686c947bf3e2835f574c40d.png)

+   `TransR`通过将`head`、`tail` embedding转换到`relation` embedding空间，是转换后的投影满足三元组的关系。

![](img/b21aa8d319dff508d333d2204006a843.png)

### 知识图谱辅助推荐系统

**问题定义** (Problemı Formulation):

+   给定用户集合、物品集合、用户参与标签，以及相关知识图谱`G`，目标为预测用户点击概率。

**知识图谱嵌入方法分为两类：**

#### 基于嵌入方法（Embedding-based methods）

+   首先用`KGE`的方法处理`KG`，得到实体、关系的embedding，利用推荐系统的方法得到物品、用户的embedding，设计一种模型来融合四类embedding。

![](img/50ab29f3e07fd11b5e739a6042dfcad1.png)

#### 基于结构方法（Structure-based methods）

+   将`KG`和用户物品交互的图结构概括在一个统一的框架下，挖掘图结构信息。

+   将`KG`和用户物品交互的图结构概括在一个统一的框架下，挖掘图结构信息。

![](img/585e3c373401940f06caeeb23c0cd091.png)

## Deep Knowledge-aware Networks

### embedding-based 方法

#### ① 框架

> 给定新闻数据，使用实体链接提取出相对应的实体，利用给定实体取出完整知识图谱的子图，用`KGE`的方法处理子图，得到实体embedding。

> 将邻居实体embedding平均得到该实体的embedding表示。

![](img/657a66530edd756aba0199019e5f7cbc.png)

![](img/0fed16316cc3f09bce12d67c882ef145.png)

### Kim CNN

Kim CNN用来学习句子的embedding。给定句子中有`n`个词语，词嵌入维度为`d`，使用3个长度为`d`宽度为2的1维卷积核处理词嵌入矩阵，再用4个宽度为3长度为`d`的一维卷积核处理词嵌入矩阵，对最后一维进行max-pooling，将池化结果concat后得到的向量作为sentence embedding。

![](img/c4e94599b5fa9de145a238b8fd9a8edd.png)

### Knowledge-Aware CNN (KCNN)

`KCNN`是对Kim CNN的扩展，输入为三个embedding，包括word embedding、entity embedding和context embedding。如果word embedding中单词对应一个实体entity embedding中对应位置为学到的实体embedding，如果不对应实体则用0来填充。此时的卷积核高度变为3。

![](img/494f96151f0555b7a245fbd59456a812.png)

### Attention-Based User Interest Extraction

> 给定用户历史新闻数据和候选新闻，判断用户是否对候选新闻感兴趣。先使用`KCNN`的方法学习新闻embedding，并用attention net来判断以前读过的新闻对候选新闻的重要程度，

attention network

通过将向量拼接并经过dnn来计算最后的权重，利用attention net的权重加权用户历史数据可以得到用户的embedding，最后将用户embedding和候选新闻embedding拼接并通过dnn得到点击候选新闻的概率。

![](img/b6b6056b092c617d93bdcecce5db4ed5.png)

#### ② 实验

使用Bing News作为数据集，`KG`使用微软Satori。Bing News数据集中标题平均词数量为7.9个，平均实体数量为3.7个。

![](img/af7138a44be94c80b0a4909036d55b92.png)

实验结果如下图

（-）代表没有使用`KG`的方法，可以看到`DKN`方法显著地好于其他baseline的方法。

![](img/148a9ab6bc16fefd3e9dc2e7b9e610f3.png)

## Multi-Task Feature Learning for KG-Enhanced RS

### ① 框架

多任务学习框架，下图左为推荐系统的框架，下图右为`KGE`的框架。

![](img/954560529025ee8b102ad21086d79f0e.png)

> 由于知识图谱中的实体就是推荐系统中的`item`，可以认为它们的embedding存在特定的关系，因此使用cross & compress units来建模实体和物品embedding之间的关联。其中cross操作将计算向量内积得到矩阵，compress操作通过将矩阵压缩为向量。

![](img/13102a2b10dfc389a0015a83728e4008.png)

推荐系统用`MLP`处理用户embedding，并使用cross & compress计算物品的embedding，最后拼接来计算点击概率。

![](img/5a496d2e2732fc2c7e4dcecaedaed963.png)

`KGE`模块中，使用cross & compress处理`head`实体embedding，使用`MLP`处理关系embedding，拼接后通过`MLP`计算`tail`实体的预测值，并以embedding预测值和真实值之间的差距作为损失函数。

因此，整体的损失函数分为三块：推荐系统的损失、`KGE`的损失和正则损失。

![](img/5496e8b1808a471bb3cbaa2f985aac18.png)

### ② 实验

分别在四个数据集上进行了实验，实验结果好于其他方法。

![](img/4db8918660f32f8a7505533ba7f3b9b2.png)

![](img/cd119821738a1a569271959a196cf454.png)

## RippleNet: Propagating User Preference in KGs

### ① 框架

![](img/e78a84cbcc80f715ed58bd4c9f570ce2.png)

> 输入为用户和物品，输出为用户点击物品的概率。利用用户的历史点击数据`v(u)`，将`v(u)`沿着`KG`向外传播，得到历史点的邻节点`t`和关系`r`，并利用下式计算邻节点`t`与候选物品`v`的关联概率。

![](img/a856b731904ea02b206e3dfba7b7e018.png)

利用关联概率加权平均得到用户的一阶表示，不断重复传播过程，得到多个用户embedding，将其相加作为最终用户embedding，最后使用用户embedding和item embedding内积的sigmoid激活结果作为最终的点击概率。

在给定`KG`和用户物品历史数据时，基于最大化后验概率来计算模型参数，利用贝叶斯方法将概率公式拆为三项。

第一项为模型参数的先验分布，采用高斯分布作为先验。

![](img/d30d8006b224120ba4ca3ae569b28238.png)

第二项为给定模型参数判断观测知识图谱的可能性函数，即通过头实体、尾实体、关系embedding的乘积来模拟出现的概率，如下图公式。

![](img/55a02203d0ccfd46729c776cdfb197b8.png)

第三项为给定`KG`和模型参数判断用户历史行为的物品点击概率函数，为伯努利分布，如下图公式。

![](img/ccf5533fb17089b0d6dbbdb63b9e1e4f.png)

将概率公式取负对数后得到最终损失函数。

![](img/271279e8e30a34f47e3271a45db1abac.png)

### ② 实验

实验结果好于其他方法。

![](img/b6bb59d12bbbd7b7d96f0995441411f9.png)

## Knowledge Graph Convolutional Networks

Structure-Based Methods (H. Wang, et al. Knowledge Graph Convolutional Networks for Recommender Systems. WWW 2019.

H. Wang, et al. Knowledge-aware Graph Neural Networks with Label Smoothness Regularization for Recommender Systems. KDD 2019)

### ① 方法

`KG`中的边不存在权值，因此引入

![](img/5af065bca8eae2cf7bcb9cabfee50230.png)

，通过用户embedding和关系embedding的内积，将权值引入`KG`。

逐层传播的函数如下图，其中`A(u)`为每个用户`u`的`KG`邻接矩阵，`D(u)`为`A(u)`的对角矩阵，`H(l)`为第`l`层实体embedding，`W(l)`为参数矩阵。

![](img/138de42d6dff61c7cbb86c395402860f.png)

最终点击概率如下图公式计算。

![](img/587d282efd25e9fbe993134110aae5ed.png)

### ② 实验

实验结果如下图，一般使用`KGCN-sum`的方法效果最好。

![](img/e9996cc889638dc27675c1db3ef2ee5b.png)

## 比较

在性能方面，`KGCN`性能最好，`DKN`表现最差。

在可扩展性方面，embedding-based方法可扩展性更好，因为embedding方法可以复用，而structure-based方法在面对新的物品、用户时需要对整个方法需要重新训练。

在可解释性方面，我们认为structure-based方法的图结构比embedding更直观。

![](img/c1a7fbe04a09b8499ddb6006c738b33a.png)

`https://v11enp9ok1h.feishu.cn/sync/Ipw1d27gNsv3OAbo3N7cks4ZnJf`
