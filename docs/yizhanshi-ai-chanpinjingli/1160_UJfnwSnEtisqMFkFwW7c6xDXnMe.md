# 推荐系统之召回

> 来源：[https://v11enp9ok1h.feishu.cn/docx/MrgydfsLBoe3grxKoFIcmmydnkc](https://v11enp9ok1h.feishu.cn/docx/MrgydfsLBoe3grxKoFIcmmydnkc)

## 推荐系统之召回

![](img/5eac375949b7935195cf162bcf42a9eb.png)

⽤索引，离线计算量⼤，线上计算量⼩

![](img/6ce275bde78c091beb74e7e5a6a199ef.png)

![](img/eee6a3c7d421aa1f41b919c50f2e1a2f.png)

> **Swing**额外考虑重合的⽤户是否来⾃⼀个⼩圈⼦，两个⽤户重合度⼤，则可能来⾃⼀个⼩圈⼦，权重降低。

###

### **基于用户的协同过滤**

![](img/53347c7bb7c7a226102f4fe335c90317.png)

![](img/b9894eef15a3b8844493c862120ce24b.png)

同样是离线计算索引，在线召回的流程


![](img/4799b69acccf790cc918457faae5986e.png)

离散特征处理

![](img/892b26b5472e0ab9ea9e5ba588b7183f.png)

Embedding 层参数数量=向量维度× 类别数量

![](img/f09746d82933ddc8428f53b41bee39b7.png)

### **矩阵补充模型**

![](img/9d433a45a074530cc6848f83f0b6baca.png)

![](img/2b11a888922888f8ae08941dc03504f8.png)

矩阵补充模型有很多缺点，效果不好

![](img/2f683a885a113fc3f70b4d5bc0ec57fb.png)

![](img/d0aac086d4e5f6927fb9f8048aeab1ce.png)

**训练好后线上召回**

![](img/e1ff235553b9fefcffb654cb110c49a9.png)

### **双塔模型**

除了ID还要考虑一些离散特征、连续特征；离散特征可以用Embedding Layers，连续特征可以归一化、分桶等处理

![](img/b49d7f382733d36aa95af6ab4cd82af4.png)

训练方式主要有pointwise、pairwise、listwise

![](img/a791fbdc1de00b0963544cda6f3b0e38.png)

pointwise就是常规的分类的思想

![](img/e2a4d3d4cd5b47aeeaf257559ed7ea6a.png)

pairwise希望拉开用户与正负样本的距离

![](img/a312f68e9c107b3ec8d6f2cb250eb42f.png)

![](img/dd28d8ccc510fd50da40904ff4a819c4.png)

listwise考虑更多的负样本

![](img/86edc3c25051de09849dba6c27ad493f.png)

正样本的选择需要考虑冷门、热门物品

![](img/6999d01c96ffe01fc6341864212173dd.png)

负样本可以是简单和困难样本的混合

![](img/875664afa6c7ca18ce37c763e65964bc.png)

简单负样本可以是全体物品（考虑非均匀采样打压热门物品）或者Batch内负样本

困难负样本主要考虑被召回，但是被排序淘汰的样本

![](img/5aa118268803d0ee5be130ae5bd81943.png)

**一个常见的错误！**


![](img/e99d0b0dafffe88c611858f4949442e2.png)

**双塔模型的线上召回和更新**

+   ⽤户兴趣动态变化，⽽物品特征相对稳定，事先存储物品向量 ，线上现算⽤户向量

![](img/f19583b570e364d5585a6f40bf17c919.png)

![](img/d167c3c6f057257ac584f6a900c067a1.png)

+   为了适应用户兴趣变化需要更新模型

![](img/d13d92800b7edfda259b204f550197ef.png)

双塔模型的问题

![](img/e6a8fa63e762e97e77eddca3e5418422.png)

可以通过减去对数抽样概率来进行纠偏

![](img/102ee96c840dc5b5e6a661d7fa069291.png)

**自监督学习**

![](img/1453cb58e0813c78859525182a631382.png)

一个物品的两个向量可以通过一些特征变换得到

![](img/60f2578b1e2c5385eb33b01ba8f5d783.png)

![](img/a5dc3301d13feacc41866bdcab4c29a9.png)

![](img/34d9685fd049d1893517fb7c6ee815df.png)

![](img/660813861162fc77c66097494e5ffe69.png)

**训练模型**

![](img/24ee3f0b8f76b7d6db67f2b2b39799e1.png)

最终的损失函数包含两部分

![](img/4ea2031f2c5aadde530833d99bb29a60.png)

**Deep Retrieval** 把物品表征为路径（path），线上查找⽤户最匹配的路径

![](img/c4aabf0b32139cae0f511d1779399637.png)

![](img/15198effe1b0e7e5ad04aa75cdd8ad35.png)

其他召回通道

⽤户可能对附近、同城发⽣的事感兴趣

+   索引：GeoHash优质笔记列表（按时间倒排）

+   索引：城市优质笔记列表（按时间倒排）

关注作者召回

![](img/03fee19165f8e50fced34c85d384c19b.png)

有交互的作者召回

![](img/3a18ce898cadd1bc4e72acbb6ea7e247.png)

相似作者召回

![](img/039d130b0df48cf693a6839879dddade.png)

缓存召回，缓存⼤⼩固定，需要退场机制。

![](img/7b6f9260cd2653dfdc45bc1e7fd6cd90.png)

曝光过滤问题

![](img/b36d25808208fa57357163f8a712806a.png)

Bloom filter 判断⼀个物品ID是否在已曝光的物品集合中

![](img/e6a3f9c3ca49f9dd34af38ebd30a22cb.png)

![](img/a098518e6ab47b79d6f03fc3d7c5de53.png)

<https://v11enp9ok1h.feishu.cn/sync/Ipw1d27gNsv3OAbo3N7cks4ZnJf>