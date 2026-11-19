
# Memristor 忆阻器 - talks 笔记

> 来源：[https://learning.feishu.cn/docx/IO3jdupXAothIuxuwp6c2K9wn0g](https://learning.feishu.cn/docx/IO3jdupXAothIuxuwp6c2K9wn0g)

中国台湾的台积电和中国大陆的昕原半导体，成为唯二实现28nm制程 ReRAM 量产的公司。

ReRAM 生产工艺与 CMOS 完全兼容，可以通过制程工艺的升级迭代持续提升性能和密度。

几种存算的路线：

+ 查存计算（Processing With Memory）：GPU 中落地多年。复杂函数的计算，由存储芯片内的存储单元查表，存储单元和计算单元完全融合，没有一个独立的计算单元。

+ 近存计算（Computing Near Memory）：带 HBM 的 GPGPU。

+ 存内计算（Computing In Memory）：典型代表是 Mythic、闪忆、知存、九天睿芯。计算操作由位于存储芯片/区域内部的独立计算单元完成，存储和计算可以是模拟的也可以是数字的。这种路线适合算法固定的场景算法计算，目前主要用于语音等轻算力场景。

![](img/31dce995fcc7da47217c60d31febf672.png)

## Paper List

| Paper | Year | Author | Notes |
| --- | --- | --- | --- |
| Memristor-The missing circuit element | 1971.09 | L. Chua | 根据理论推导提出，在电阻、电容、电感之外，可能存在第四种基本元器件，取名忆阻器，其基本特性是电阻随着电流改变，电流消失了，电阻停留不动，直到反向电流将电阻推回原值。 |
| The missing memristor found | 2008 | R. Stanley Williams | 首次找到了有忆阻器性质的物质：二氧化钛。 |
| Training and operation of an integrated neuromorphic network based on metal-oxide memristors | 2015 | IBM | 用 12*12 的阵列，实现了 3 个字母的识别 |
| Face classification using electronic synapses | 2017 | 清华 吴华强 | 用 128 * 8 的阵列，实现了人脸识别 |
| Fully hardware-implemented memristor convolutional neural network | 2020 |  | CNN 模型 |
| Edge learning using a fully integrated neuro-inspired memristor chip | 2003.09 | 清华 吴华强 | https://mp.weixin.qq.com/s/ZYcos8vEszvWGkKmvgXc_A |
| In-memory Computing with Memristors and Memtransistors - Daniele Ielmini | 2023.02 |  |  |

第 4 种元器件，体现电荷和磁通之间的关系 -- 忆阻器。

From 忆阻器芯片 - 新石器公园

https://www.bilibili.com/video/BV1nN411g7HC

![](img/c74f6743df6f96fee3c098b041da5483.png)

![](img/1052768f4efb17a2962e89cfda551576.png)

三条线同时加压

![](img/364bb9322b163c0ca1299a95502fe40b.png)

## 6-Minute Memristor Guide

https://www.youtube.com/watch?v=rvA5r4LtVnc

中文字幕: https://www.bilibili.com/video/BV1k44y1474h/

R. Stanley Williams

Dec 11, 2008

2008 年，R. Stanley Williams 的团队首次造出了忆阻器。

a quick whiteboard talk

要点：

1. crossbar 结构的网络拓扑。如果，每对导线之间有一个开关，就可以表示惊人的信息量。

1. 13 年过去了，我们仍然认为 crossbar 是未来电路的基础结构。

1. 1 个忆阻器，可以代替 10 个晶体管。

![](img/479a2620c9a0e177087f975740e1a67c.png)

开关的设计思路

![](img/e76662878a11c71d9a0a8cd942fce75d.png)

![](img/f483c6beaa753c9044a5104924119823.png)

## R. Stanley Williams 2010

https://www.youtube.com/watch?v=bKGhvKyjgLY

Finding the Missing Memristor

Jan 22, 2010

R. Stanley Williams

HP lab 从 1997 年就开始研究，前后可能超过  200 多人参与。

passive device

无源器件。指在电子电路中不需要外部电源供电的器件，如电阻、电容、电感等。

![](img/1538a7281d3cfb7a09771b453616b883.png)

Leon Chua 蔡少棠 https://en.wikipedia.org/wiki/Leon_O._Chua

非线性电路理论之父，就像爱因斯坦之于相对论。当时的人还都不太能理解他的理论的价值。

预测忆阻器，就像狄拉克预测正电子一样。

![](img/5cb3995ef4148741db30732609c132a7.png)

![](img/642511fa6ce05f96df7f27e9f6e8884c.png)

大家都去尝试找这种硬件，但犯了一个错误：大家在找因果关系，什么物理现象能把磁通 flux 和电荷 charge 耦合起来。

R. Stanley Williams 意识到，公式并没有说什么引起什么，只说了 2 个东西是 equal 的。

几年后，Leon 和他的学生意识到，memristor 是一个有状态的无源器件。

即，memoristor 里面有个 memory，可以表征器件的 1 个或多个状态。

因此，无法用 1 个 equation 来表征 memristor，实际上，需要 2 个 equation 才能完全表征。

1. quasi-static 准静态方程描述 `U` 和 `I` 的关系。

1. 电阻则取决于电路此时的状态。一个时间相关的方程，描述器件状态与时间的关系。

这两个 equation 都没有 flux 磁通。

`IV` 曲线像数字 8。在 Leon Chua 1976 年的 paper 里，叫 收缩磁滞回线 pinched hysteresis loop。

在 1976 年的 paper 中指出，找到 pinched hysteresis loop 曲线就找到了 memristor。

Why memristor fundamental？

只用电阻、电容和电感无法做出这种 `IV` 曲线，Leon Chua 从数学上证明了这一点。

实际上，1971 年之前，很多 paper 都观察到了 收缩磁滞回线，但大家都不知道这是什么。

![](img/5973eab732922b03562fa5c4c044cb13.png)

![](img/8bb15a43e733c01fd635aba92e86c7f8.png)

器件的状态就是 doped(掺杂）和 undoped（未掺杂）器件的分界面。

用电压可以控制和改变这个分界面。

![](img/696a8e85d4679e70140b54daf7f163be.png)

![](img/62c0970f50b750a35fbae99c2fe0d142.png)

![](img/dee39385390b76b5ec5e7e95f3dac8a7.png)

![](img/f7ed8ab1ea86a58b3a2512996b873a6a.png)

![](img/05c2a0571111920b9a2cbc28806463e7.png)

![](img/b144eddace3e111a2e9ddfcc27efbb13.png)

![](img/adc1db29177e7fdffd228d4cc7091be7.png)

![](img/44f42a65839ba5cbe791d3ff8a23c548.png)

![](img/f92ff7aec1448e78b3c43a144050ec53.png)

![](img/b9339e9e643f406e554c162aa2fc2458.png)

## Leon Chua 2017 - UC Berkeley

https://www.youtube.com/watch?v=ieMdXnjQTXc

Leon Chua, UC Berkeley - 10 Things You Didn't Know About Memristors

看不懂

![](img/9890bac16a3ad23a134df487ac44b118.png)

1. All non-volatile solid-state memories are memristors.

1. Not all memristors are non-volatile

## Memristors: Where fact meets fantasy

https://www.youtube.com/watch?v=Y6ab32SRRoE

Alex Nugent

2018 年

![](img/b920058407d07b5190e202d8d92924d7.png)

![](img/2e449a723364d27222676004c5625523.png)

颠覆性的技术，的定义。

![](img/4aeb1fd93b2fade356da8d2a97aef394.png)

![](img/f830e80eb01ab966242339a4a16b9404.png)

![](img/b61a5f068d0ad06553a14114465e7bfe.png)

![](img/06adfc1d7eb95354ef9d46c7db93c03e.png)

## 清华 吴华强 2023

https://mp.weixin.qq.com/s/ZYcos8vEszvWGkKmvgXc_A

Edge learning using a fully integrated neuro-inspired memristor chip

基于完全集成神经形态忆阻器芯片的边缘学习

![](img/6dac396d8e751e1ca0fad05229ed56c7.png)

几点突破

1. 器件层面。用千层饼结构，降低离子扩散导致的精度问题。

1. 架构优化。器件的电压控制等

1. 算法层面，引入误差模型，通过一个压力训练，让 AI 寻找误差模型。

以清华团队论文中用的，MINST数据集中的28*28的手写数字图像识别为例，神经网络节点是3层的，784*100*10，输入层是784（它等于28*28）个节点，中间层是100个节点，输出层是10个节点，对应0-9。应用时是前向推导，784个输入节点的数值乘上连接权重，中间层某结点的所有784个连接乘数相加，就是这个节点的状态数值。这相当于用784维的向量乘以784*100的矩阵，得到100维的向量。再用中间层的向量，乘以100*10的权重矩阵，得到10维的向量。

一个重大缺陷是，电阻的写入需要验证（write verification)，反复地读写，因为忆阻器作为元器件并非完美。这让训练过程不是那么顺畅，额外开支很大，能耗不低，并没有摆脱“冯·诺依曼瓶颈”。

![](img/01b9d9879cdf0bf2a0655948b225aa0d.png)

要理解清华忆阻器芯片的元器件与架构，需要搞清楚一个现实问题：矩阵权重是有正有负的，忆阻器的电阻（论文中用电导，就是电阻的倒数）都是正的，负权重如何表示？办法是用一对忆阻器，看它们电导的差值，就能表示负数了。如上图左（上下排列是向量的意思），红的是正Cell，蓝的是负Cell（里面的电导也是正值），相减得到权重，就能表示负数了。这个图要好好理解，清华团队的创新原理与之紧密相关。

而清华作者们就是具体实现了一个基于忆阻器交叉阵列的新架构芯片，能算是完全集成（fully integrated）的忆阻器芯片，并通过一系列人工智能任务上展现了芯片架构的优点与潜力。

![](img/cdee1b7fc8c3afd0bf926e0ee3083eff.png)

清华忆阻器芯片的光学显微影像，核心模块标出来了

芯片架构叫 STELLAR，是“sign- and threshold-based learning”的简称，指的是训练中的创新。

有两个忆阻器交叉阵列数组，大一些的是2T2R的，有1568*100个忆阻器，在神经网络模型中代表784*100的权重矩阵。小一些的是1T1R的，有100*20个忆阻器，代表100*10的权重矩阵（注意不是100*20）。两个忆阻器交叉阵列组合成了784*100*10的一个三层神经网络结构，用于完成一些小型的人工智能算法任务。

这两个Crossbar各有特性。大的是2T2R的，里面的权重是“off-chip”离线训练好的，然后上传到交叉阵列里面，它的特点是可以并行计算矩阵乘法，节点数多，展现了“存算一体”的优良特性。小的是1T1R的，规模小，但是后面附带权重更新逻辑，可以在芯片内训练更新网络权重，展现了“片上学习”（on-chip learning）的功能。

![](img/96734cbf875f31d284ee01bbb55a7903.png)

2T2R Cells的电子显微镜（TEM）切片图

![](img/1aea7c2160a57eb5b3b1b486d5748d82.png)

`W1`是大的交叉阵列，输入向量`X`进来，乘以矩阵`W1`，再用激活函数（Activation function）`ReLU`变换——其实很简单，负数变0，正数不变——变成向量`Y1`。`Y1`再乘以矩阵`W2`，再`ReLU`变换，成为输出向量`Y2`。`Y2`与训练样本`T`比较，生成误差向量`E`。

![](img/2aad47de4180506d63ea9e6df93636e4.png)

清华团队的创新是，对`W2`的训练，先将`Y1`、`Y2`、`E`三个向量的符号抽出来，实施更好的权重更新办法。其出发点是，一个权重是用一正一负两个忆阻器存储的，可以选择性根据符号，一列一起更新。这是深入思考反向传播的权重更新算法，结合元器件架构，实现了忆阻器潜在的并行功能。如图，权重更新分为`SET`和`RESET`两个步骤，`SET`步骤只对正的符号更新正Cell，对负的符号更新负Cell，而`RESET`正相反。这样`SET`和`RESET`分别都可以按列并行加速了。

另一个创新是，在误差向量`E`上加一个`threshold`，预先将一些微小的误差过滤掉，这样给出的符号向量，训练性能更优。这个`threshold`是可配置的，对不同的网络模型可以设置相应的过滤门限值，这应该是结合`SET`和`RESET`特性的进一步优化。

在字符识别测试时，清华团队还用48天进行了重复测试，结果是稳定的。这说明忆阻器芯片里的权重是稳定的，这是芯片能实用的重要特性。

130 nm 的是 Memristor

![](img/d0c33b3da00d58137725d20fc3672ea0.png)

## 清华 高斌 2023 - 忆阻器与类脑计算

https://www.youtube.com/watch?v=9U-5jshTuwg

Oct 23, 2023

![](img/7e2b292a48bb34785098dd4ee02a58cb.png)

### 存算一体技术

![](img/872500654b26870c2d7e36a0bd769c74.png)

![](img/1584bec99d18f2bd072ede1a1dab808e.png)

大脑是存算一体的

![](img/21039cac697dbcfc8205a789b09ef6ca.png)

### 忆阻器 RRAM

外加的脉冲，可以影响电阻值。且可以长时间保持。

![](img/f61191eccafaedf23fff77aeda2024e8.png)

基于物理定律的模拟计算。

忆阻器记住的电阻值。给电压，出电流，完成了乘法运算。

一个忆阻器如果有 64 个状态，相当于 6bit 的器件。相当于数字电路的 6 bit 乘法器 + 6 bit 加法器 + 6 bit SRAM。

![](img/87dba06b1f355ae80f17208b94b786f2.png)

基于物理定律的模拟计算 - 非冯架构

![](img/a72052d25f5600ef60e2e8b6bf5e088f.png)

![](img/a9fbdbcf52b2db88d31f6eeca36012bb.png)

![](img/488087e7236ffb553088bcd523fc0afc.png)

![](img/813fd3bde281cd7d9c11f20661895a62.png)

### 研究进展

![](img/cb6eaad54e77382721f5bd296450c36c.png)

![](img/f99278219f9fae4163b617257de270d3.png)

![](img/41cac13e7cf4fc5e9321fc32a0786b69.png)

![](img/4e1caed000934d6afddcb225942ce41d.png)

![](img/8eaea1855c320fde69a31376dd9d03b2.png)

![](img/9402eb907f2d069cd987bfa70f99b02a.png)

![](img/a6d854969aad3024e6bd5f2daf9dd9e9.png)



![](img/58630b29669496d7f419f61ccefc5bfe.png)

# 器件测试的新挑战

![](img/62bdb6a8e1e0c0a3777e7e258845a424.png)

![](img/ce470927b6dc2eee4cc16ea2da67743f.png)

![](img/abcae274a0ef03eef2b98620a17be6ae.png)

![](img/9873a92b16c3b53a9ad00f9ab27762c2.png)

![](img/35e4a612c96333d81a6ec44109b3c725.png)

![](img/69a00135ff4fca73324c0a94aba5b90b.png)

![](img/b3f93b398522d67eca267dadbfabc1fb.png)

## In-memory Computing with Memristors and Memtransistors - Daniele Ielmini

https://www.youtube.com/watch?v=RBviTZAM-7Y

讲材料的，看不懂。

![](img/84d26a78fd5caa96a305f8319a30ed1a.png)

![](img/d768b67154c9cad4376a4c6c2e49a774.png)

![](img/2294f40fc0db57c596581f47cbfc7544.png)

![](img/d2e0a11bee64bfac884635ca8d5c775d.png)

![](img/a2ed5f37baf3c51d4c49ef3fc1ca6a90.png)

![](img/255bfac0851a3de982523c2e7f466b88.png)

## 北大 杨玉超 - 基于忆阻器的高效AI芯片与学习系统

https://www.youtube.com/watch?v=lrrtM4ngzv0

北京大学集成电路学院

May 4, 2022

![](img/dd18b5a27a74ec493c58fde1572673b9.png)

### 背景 & 动机

近存计算

![](img/02ebca027b71ba2542e8383d49f5f079.png)

存算一体

![](img/d6657689a2f4221df7e8cae3021d1891.png)

类脑计算

![](img/fb3492b2585ea3627985522774889147.png)

![](img/3664408b5ad547f74037f60b21aaeda2.png)

![](img/9e1ed4f3f9692659298855c2670c7b8b.png)

![](img/23cb2f5fa4230fc6d5a7815ace15a100.png)

![](img/06b9bcc837a48e65ae8d909927f138a9.png)

### 3 个例子

![](img/164707a7214172e3d48443095f6f430c.png)

#### 深度学习

![](img/9639bb43fcd30e1828d8cfff062f0767.png)

用忆阻器实现高效的随机数发生矩阵。代价比数字电路更低。

![](img/ea2d854df5419082ce1e1ea74cbba05f.png)

时间和空间上，都是很好的高斯分布。

![](img/4ce41b02635628fe58662edbc01a3b47.png)

在 2 个网络上，达到了与 BP 类似的效果。硬件代价明显下降。

![](img/035215be3223fab069a5908cec318221.png)

![](img/5aec88bc256d2911257296c36b8ba0bc.png)

#### 强化学习

用相变忆阻器来实现 ET。

![](img/0b68a8f4b15173e11ea165604f051b77.png)

![](img/cabda0cd4f84ddfd302c4be846e392be.png)

![](img/0ecda21a1611cbf0661ecef73e1ea78b.png)

### 高效芯片设计

比 GPU 至少 3 个数量级的能效提升

![](img/c61b85a7193313e027d09f3c1863ab5e.png)

流片的芯片

![](img/9b31efe6c7d5b26485a47af985862400.png)

![](img/24ef62c8fae71620c2d3ffacef9a33a3.png)

![](img/24b765929d693280acfbfea54fa08a80.png)

## 什麼是憶阻器 - 江佩勳 教授

https://www.youtube.com/watch?v=x9E5QOhjGtE

May 27, 2022

![](img/144838f7320afb823a7c398d21fb51c1.png)

![](img/52d78e5809477228dfd21af76dc3c3b8.png)

![](img/7d83183d580dff20599d1d865b9a641a.png)

电阻状态是靠原子的重新排列

白色球是 氧空缺，跑到了钛里面。

![](img/8213e5a7fda18cefa450ad8f08e066b6.png)

![](img/33fb7c4e78c93f578a868f6d2bd21f76.png)

加电压，吸引负价的氧离子，实现导通。

![](img/c61bc937438f8cfe3f3cb6db1c2f7c4d.png)

![](img/49ffeb1ebff7a3323fb3444a3b9c1af2.png)

绕着蓝色的 8 字形循环。

![](img/3008638176ac6a5cc0cf5b6738a59bb2.png)

不断的加小脉冲，直到氧空缺进入似断非断的状态，有了量子态。

![](img/ad62b65292bf3739c9a1ca3a7d063c7c.png)

电导是电阻值的导数/倒数

电导是半个半个的掉

![](img/07c19308d63a55c6d5eed669234d6773.png)
