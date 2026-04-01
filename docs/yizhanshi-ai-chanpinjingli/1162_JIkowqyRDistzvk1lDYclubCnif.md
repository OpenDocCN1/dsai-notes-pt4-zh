# 推荐系统之交叉

> 来源：[https://v11enp9ok1h.feishu.cn/docx/SDYZdeohgoUf2rxLxg7cQ7OVnwe](https://v11enp9ok1h.feishu.cn/docx/SDYZdeohgoUf2rxLxg7cQ7OVnwe)

Factorized Machine

线性模型预测是特征的加权和。（只有加，没有乘。）

![](img/8a310e2bff1896488257e4b32e55342a.png)

二阶特征交叉

![](img/28984ee69a677c6aa08c3ba95d2ec521.png)

可以通过矩阵分解减少模型参数量

![](img/93357b5daaa6c0b5606cee0278ffe405.png)

![](img/ef044dbfc7f5e0d4d7c958b3fdd447ae.png)

深度交叉网络（DCN）

之前提到过的召回、排序模型中的神经网络可以用任意网络结构；常见的交叉层

![](img/184f1f387833e77866d516969ac01d2c.png)

交叉网络就是多个交叉层串起来的网络

![](img/8b9d9e3e42ced895c9333cc8065c0338.png)

深度交叉网络就是两个分支，一边是全连接，一边是交叉网络

![](img/7b10a6e1f5901979f4efc1fe743fd492.png)

LHUC网络结构

LHUC起源于语⾳识别，快⼿将LHUC应⽤在推荐精排，称作PPNet

![](img/58b8879827801bd760616ff6ec2d0ff8.png)

SENet & Bilinear Cross

SENet 对离散特征做field-wise加权，如果有 个fields，那么权重向量是 维。

![](img/b91c19d7a6fd0e569e475f45893541e3.png)

Field 间特征交叉

![](img/b5223bb5a7b6301150a7c8838fe114d6.png)

![](img/21e42c24212543576eadcdd5f9336927.png)

![](img/28f98bcb0b414f20db365dbc962c6221.png)

FiBiNet可以理解为同时考虑了SENet 结合 Field 间特征交叉

![](img/5505b640d7ad7b5f4c2db8f29f92e38b.png)