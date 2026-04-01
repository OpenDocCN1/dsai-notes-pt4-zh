# 推荐系统之重排

> 来源：[https://v11enp9ok1h.feishu.cn/docx/OFWuddJzYocO9qxr2SjcWRyunLg](https://v11enp9ok1h.feishu.cn/docx/OFWuddJzYocO9qxr2SjcWRyunLg)

## 推荐系统之重排

### 相似性的度量

+   基于物品属性标签

![](img/a63e6117ddd6b121391453f713cee00a.png)

基于物品向量表征⽤召回的双塔模型的物品向量（不好）

基于内容的向量表征（好）

![](img/b39a341a903f4df203a2e975dafb39d9.png)

![](img/8865199cfee0875e513f552193caeb90.png)

![](img/572915a8632a2aa1050e5dbb7bbe60d9.png)

## Maximal Marginal Relevance (MMR)

![](img/c28b65c96380fbbf29ad754afe39e162.png)

![](img/1ae387cfdd957ad6384c5cf30669c03e.png)

![](img/9d500cb2a3b9f969a20c2a447b662dc7.png)

![](img/dc6a79fa484238ec6b7301ea75cc9c7d.png)

## 重排的规则

+   最多连续出现 篇某种笔记

+   每篇笔记最多出现1篇某种笔记

+   前篇笔记最多出现 篇某种笔记

**MMR + 重排规则**

![](img/62e8305cad33ed3d37a71ef35b33348f.png)

**DPP：多样性算法**

![](img/f79def85da04f867ae02b5ac34c59506.png)

![](img/c9176d19fd0872632ed4888b125eb398.png)

![](img/abcaf4a9919dce892d2b31bd977cde67.png)