
# 4. Retrieval过程

Retrieval的过程是如何根据用户提出的不同问题，在偌大的文本中找到与该问题最相关的片段。这就涉及到两个`Embedding`向量之间是如何衡量它们是不是相似的。

计算两个向量之间的相似度通常可以通过几种不同的方法来实现，比如点积、欧氏距离和余弦相似度（`Cosine Similarity`），每种方法侧重于不同的相似度特征。比较常用的一种方法是余弦相似度，它是衡量两个向量之间角度的一种度量，不受向量长度的影响。余弦相似度广泛用于文本分析、推荐系统等领域，计算公式如下：

> 分子部分是两个向量 `A` 和 `B` 的点积，也称为内积，计算是对应元素相乘后的结果相加。

> 分母是两个向量的欧几里得范数（或称为欧氏长度）的乘积。向量的范数是向量从原点到其终点的距离。

转化成代码如下：

```
import numpy as np  

def cosine_similarity(A, B):
    # 计使用np.dot函数计算向量A和B的点积
    dot_product = np.dot(A, B)  
    
    # 使用np.linalg.norm函数计算向量A的欧几里得范数
    norm_A = np.linalg.norm(A)  
    norm_B = np.linalg.norm(B) 

    # 计算余弦相似度
    return dot_product / (norm_A * norm_B)  
```

```
query = get_embeddings("我喜欢打篮球。")
answer_1 = get_embeddings("NBA是篮球的最高赛事。")
answer_2 = get_embeddings("中国的乒乓球放眼世界也是最强的。")
answer_3 = get_embeddings("今天的天气真好")
```

```
answer_1
```
