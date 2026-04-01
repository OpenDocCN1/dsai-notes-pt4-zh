# 推荐系统之序列建模

> 来源：[https://v11enp9ok1h.feishu.cn/docx/W0uMdSmvWoQOYTxYSY5cRb2pnBf](https://v11enp9ok1h.feishu.cn/docx/W0uMdSmvWoQOYTxYSY5cRb2pnBf)

## 推荐系统之序列建模

## LastN特征

+   LastN：⽤户最近的 次交互（点击、点赞等）的物品ID。

+   对LastN物品ID做embedding，得到 个向量。

+   把 个向量取平均，作为⽤户的⼀种特征。

+   适⽤于召回双塔模型、粗排三塔模型、精排模型。

![](img/d6ee4f6ca670d20dce8be3e2b1e4e7a1.png)

比如在小红书中可以同时考虑点击、点赞、收藏等多种交互序列

![](img/43df70fd187973f794c56ae1651a2ad9.png)

## DIN模型

DIN ⽤加权平均代替平均，即注意⼒机制（attention），其中权重为候选物品与⽤户LastN物品的相似度。

![](img/3312b89d82e5bab55135ad93782c83f7.png)

以相似度为权重，求⽤户LastN物品向量的加权和，结果是⼀个向量

![](img/dbbb408b399ac36214d8ca457666ad59.png)

**简单平均** v.s**注意力机制**

![](img/a56907e1a88595a431881e79438c2950.png)

**DIN模型的缺点**

**SIM模型**

![](img/a2dd0c3d81cf092a6c99384bbd496d3f.png)

![](img/ac91013792d022ddd1ff5d804bde973c.png)

![](img/7e56b39a6ba354f06b1aa9a51936ff30.png)

![](img/f220d5c4c3c8a7f60c443d1f1ad3f8e9.png)

使⽤时间信息。SIM的序列长，记录⽤户长期⾏为。时间越久远，重要性越低。

![](img/b85b8ddab664fbab5e2f326beb4c9833.png)

![](img/21e4b1861e6ea1e940cdbf8def9afd63.png)