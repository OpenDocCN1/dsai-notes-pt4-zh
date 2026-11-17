
# 一个产品经理的大模型观察、反思与预测

> 来源：[https://agijuejin.feishu.cn/docx/RRYMdTzPkoowHLxE9lkcrdqFnLb](https://agijuejin.feishu.cn/docx/RRYMdTzPkoowHLxE9lkcrdqFnLb)

> 张海庚｜[阅读原文](https://mp.weixin.qq.com/s/CRZlXIOkLEfTUaklfq9yCQ)

> 转载请联系原作者取得授权

> 最近在字节内部做了一次大模型分享，主要从产品经理和战略的视角聊大模型。应粉丝要求，分享下文档（PPT），内容有点长， 欢迎收藏阅读。

> 另外，因为主要内容是现场讲，所以很多地方只放图、没有过多文字解释，如果你读完想找我深入聊聊，欢迎文末扫码入群或找我讨论，enjoy：

---

## 本文目录

1. LLM 技术原理
2. LLM 产品 Landscape
3. 一个产品经理的 LLM 暴论
4. LLM 学习方法/媒介素养

---

## 关于我

“你个大忽悠，去知识星球捞钱吧！”

“没干过产品经理的战略不是好AI布道师”

战略、产品经理、运营

抖音百科、今日头条、飞书、识区、类Notion文档、Simeji日文输入法、头条国际化TopBuzz

toB/toC、内容/工具/社区、国内/出海

0-1/1-100、MVP/PMF/GTM/UG/商业化

为什么复合视角对这轮AI浪潮很重要？：

[做产品、投资，与借假修真](http://mp.weixin.qq.com/s?__biz=MzIwODA3MDUwOQ==&mid=2652772397&idx=1&sn=702e5bb1d81fb3a2bac410f5335434e4&chksm=8ce20a2fbb958339ac9d4cb718f98d326e2abef39abd7841049667d830a78e95b1394d8a3afa&scene=21#wechat_redirect)

---

## 大模型时代：ChatGPT 引发的狂欢

2022年8月，由Jason M. Allen使用Midjourney生成的《太空歌剧院》获美国科罗拉多艺术博览会数字艺术类别冠军

![](img/e568e111c1acd12dd1818bbbfcf7a2a3.png)

2022年11月30日，ChatGPT上线，两个月月活突破1亿，成为史上增速最快的App

![](img/01307bf8c1dfb0841471af985f7892a6.png)

2023年全球生成式AI融资规模暴涨

![](img/d7bc9c2681e4785e62997eaaa4c69776.png)

中国百模大战

![](img/83554a3e13b48a754488bc63c0ccca49.png)

---

## 一、LLM 技术原理

> “不了解大模型的技术原理的话，要么踩雷、要么被忽悠，没法讨论大模型创新”

### LLM 是什么？

> 大模型是棵分叉无数的大树，先搞清楚是什么，才不会鸡同鸭讲

![](img/58a09a6c9a93723b6ed6fe733137899a.png)

![](img/26d88746adaa670a253bfbe63c953cdf.png)

![](img/cd71dbdb9e3dadd112d3f4711da06d5f.png)

![](img/d1b456e2a2d61db3ac58e6c6fbf99d36.png)

![](img/949d8c5b8f0a670347ffa2ce53a66d46.png)

![](img/8794346d6b6bdf2f692a6d3365bfe943.png)

![](img/61b339fc63eaf4eef5678d98dec6f1ff.png)

![](img/8a0be9538fbcead9870c7e7024551acf.png)

### LLM 为什么现在爆发？

> 多年以后，面对行刑队，奥雷里亚诺·布恩迪亚上校将会回想起父亲带他去见识冰块的那个遥远的下午

> —— 《百年孤独》 加西亚·马尔克斯

自然语言处理：人工智能皇冠上的明珠

![](img/eec6980a124dd6787d209b8658180241.png)

### 神经网络历史：深度学习框架演进

+ 80年代末
+ Yann LeCunn 卷积神经网络 CNN（Convolutional Neural Network）
+ 2000年代
+ Geoff Hinton 深度学习 DL（Deep Learning）
+ 2012-2017年
+ 卷积神经网络 AlexNet
+ 循环神经网络 RNN （Recurrent Neural Network）
+ 长短期记忆网络 LSTM（Long Short-Term Memory Networks）
+ 残差网络 ResNets（Residual Networks）
+ 生成对抗网络 GANs（Generative Adversarial Networks）
+ 2017年~
+ 2017年：Transformers 横空出世 《Attention is All you Need》
+ 2018年：Google BERT（Bidirectional Encoder Representations from Transformers / 双向编码器表征模型）
+ 2018年：OpenAI GPT（Generative Pre-trained Transformer / 生成式预训练转换器）

![](img/df5ce2815ce9be296966ac8abd315625.png)

### Scaling Laws：神经网络的大力出奇迹

+ 随着模型大小、数据集大小和用于训练的计算浮点数增加，模型性能会提高
+ 为了获得最佳性能，所有三个因素必须同时放大
+ 当不受其他两个因素的制约时，模型性能与每个单独的因素都有幂律关系

![](img/c72d26b42fd22c8fd7c5b8baa9b4b13d.png)

![](img/2c7b04bd7231c56fd69dc39ccc3d185a.png)

### GPT 进化之路

![](img/d279c6fe0b746e74933e9c32d0f77daa.png)

### LLM 进化树

![](img/996df0193328e994ed64372107762391.png)

### GPT-4：AGI 的火花

鉴于GPT-4能力的广度和深度，我们相信它可以被合理地视为 通用 人工智能 ( AGI )系统的早期版本(但仍不完整) ……GPT-4实现了一种形式的通用智能，确实显示出了 通用人工智能的火花 。

——微软 Sparks of Artifificial General Intelligence: Early experiments with GPT-4

![](img/63620d324daf6092845e2ab577bea863.png)

### 大语言模型的技术原理与训练过程：以 GPT 为例

State of GPT： OpenAI 联合创始人 Andrej Karpathy GPT模型技术原理和训练过程

![](img/b76c4e47c7ba217e75a5214e35629782.png)

GPT训练的四个主要阶段：

1. Pretrain 预训练
1. Supervised Finetuning 自监督微调（SFT ）
1. Reward Modeling 奖励建模（RM ）
1. Reinforcement Learning 强化学习（RL）

+ 每个阶段：数据集、算法、模型、注释
+ LLM 为什么难？ 算法、算力、数据
+ 预训练：最主要工作，相当于训练计算时间的99%，数千 GPU、数月训练时间
+ 其他三个为微调阶段：少量 GPU 、数小时或数天训练时间

### 1、Pretrain 预训练

![](img/62914e1811ebdd044093538ecdf5a4df.png)

数据集、tokenization、参数

+ 预训练时要处理的数量级：上下文长度通常是 2000、4000 甚至是 100,000（决定GPT在尝试预测序列中的下一个token时将查看的最大token数）
+ 不应该仅仅通过模型包含的参数数量来判断模型的能力， 还要看训练token数
+ 预训练参数粗略数量级： Meta 650亿模型，2000 个 GPU 、训练21 天、花费500万美元

![](img/6a18d78405d8b63de9c2406c8cd7e452.png)

![](img/b4df248e0e690709d7f341c9cc0e26fc.png)

![](img/4dca4e3f1dd6461ed949ad63b0114e73.png)

### 2、Supervised Finetuning 自监督微调

![](img/329b12ac21652a0333046bce2b68387b.png)

数据集：

+ 少量但高质量（数万）
+ QA格式

例子：关于垄断一词的简短介绍

![](img/f4ecb9bb84c6385089a8e49d5f42055e.png)

### 3、Reward Modeling 奖励建模

![](img/a0d4cff4751f4588e370ed41b5d65093.png)

在奖励建模步骤中，要做的是 将数据收集转变为比较形式， 然后 让Transformer对每个补全的质量进行了猜测 ——这就是训练奖励模型的方式，能够对提示的完成程度进行评分

![](img/7eac739b8d3294342711568de5b43045.png)

![](img/21fd190c500108191863c04b3b5cc642.png)

### 4、Reinforcement Learning 强化学习

强化学习期间所做的基本上是 再次获得大量提示，然后针对 奖励模型 进行强化学习

![](img/55e3f00c971252b2eec9d5bf2c7c73b8.png)

为什么用RLHF？因为效果好

Reinforcement Learning from Human Feedback/基于人类反馈的强化学习

利用人类的偏好作为奖励信号来微调模型

![](img/753ba65515e9478cca853c07a1a57d71.png)

---

## 二、LLM 产品 Landscape

> AI应用哪家强？你看好哪个方向？ 你用的最多的AI产品是什么？

> 先有信息，才有观点。

> 极少数有价值的观点，只能产生于充分的、甚至冗余的信息之中。

### 用户的狂欢

ChatGPT成史上用户增长最快产品

![](img/ae6e2c4f3758c08648f84d254f2ffdd3.png)

Midjourney Discord社区用户数遥遥领先，2023年5月数据

![](img/ca60dc5494edf82d55b062ab8010d887.png)

中国情侣 By Midjourney V5，2023年3月

![](img/2d15677cb678b05504246155a1b0b5d0.png)

### 资本的狂欢

![](img/29d4d4d10907dff86ad2332fb04331ee.png)

![](img/8e49c95f6ba6c3091f708bc6de889323.png)

![](img/c27011fbb854f340f91eb1f686e3dec6.png)

![](img/de1f7688dee03405aef6ba26b430b5af.png)

### 各细分方向模型与应用

![](img/9c4beab1ae6bbad21a64d22fef8677bf.png)

### 生成式AI应用全景图

![](img/d4994a2fde3ac5309d28169d2377f251.png)

![](img/cc0f7953936f4333b093b060cc074de9.png)

![](img/ebb0944fa5a50f6f73b1bbe0c411458e.png)

![](img/55f9857b028422d46746c34e2aa5aedd.png)

### 2023中国AIGC产业图谱

![](img/8274754318efb8462cda8b563d6d82b6.png)

### The 2023 MAD (ML/AI/Data) Landscape

![](img/93497f1b65e38b69818a277c501ea0d6.png)

### 全球5000+AI产品盘点

完整榜单见 AI龙虎榜：全球5000+AI产品盘点（2023年8月）

![](img/f5930f7f6b6be3b1fab3e043fd8116e3.png)

### 信息爆炸的时代

信息的角度，比信息更重要

体验产品的角度，比产品更重要

---

## 三、一个产品经理的 LLM 暴论

### 1、为什么还没有LLM的杀手级应用跑出来？

首先是技术周期：模型层还没ready，中间层蓬勃发展， 应用层为时尚早

对创业公司、新产品而言：只有应用层没有模型层的公司，因为 壁垒不够强，边跑边被吃掉了 。比如Jasper.ai

![](img/2f4b016cc0c195f4b470e6fb4e8b243e.png)

对大厂而言： 监管 、现阶段 基础模型 开发高优于应用 ；大厂尤其是产品经理 缺少对AI的独到/深刻认知 ，包括苹果。

![](img/ebea9db08ce370950c8bdc29d3d401dc.png)

总而言之，无论是大厂还是小厂、还是创业者，大家都还在 消化底层模型的能力、提升认知

认知迭代的阶段，如何学习？

### 2、LLM 时代，了解技术原理，非常重要

（1）了解 大模型 产业链，认清自己的生态位

在「只有发令枪，没有地图」的生成式AI大航海时代里， 清楚自己在船队中的位置，能让你更可能发现新大陆。

![](img/c4598b8d3bd8a45e65749bcf413d6193.png)

（2）大模型 产品经理必须了解技术

互联网和移动互联网时代，产品经理完全不懂TCP/IP协议、HTML、Swift语言等技术栈，照样设计产品。

但LLM时代不同了：

+ 由于LLM如此强大到几乎无所不能，以至于 看起来 能无差别地满足所有需求
+ 今天如果不理解LLM的技术原理、局限性，产品经理就只是个 调模型（拉天线）的 ， 无法提出有价值的产品洞察和解决方案：
+ 还有什么是 LLM 不会的？还有什么是我可以做的？怎么做？
+ “LLM 产品经理可以不写 PRD，但一定要去搞数据：给 LLM 训练的数据集”

![](img/69005c790c5bb5fc69ac56e54585b7f6.png)

### 3、LLM时代的产品设计革命：大模型带来的人机交互范式的转换如何影响产品设计？

大模型带来的人机交互范式的转换将彻底改变产品设计思路：

1. LLM改变人机交互： 从 图形用户界面 转向自然语言用户界面
1. 最早转变过思路的产品经理和他们的产品将赢得巨大的先机
1. 最根本的东西是不变的： 用户需求洞察以及你对用户痛点的独特理解和创造性满足
1. 图形交互界面的本质： 预测用户可能的需求，并通过用户可以理解的方式提供满足方案
1. 自然语言用户界面对产品经理的新挑战： 大 语言模型 已经这么强了，产品还需要做什么？怎么做？

![](img/5f371910b36d11c561881e9acf05fa64.png)

MS-DOS

![](img/54521ed9ae45fb3a3fb8ac39ea0d8d5d.png)

乔布斯与Macintosh

![](img/1458d17f39a1bd28a412ff0f6ee189cc.png)

Window XP

![](img/aecb944ae0f2cc035ec414d5e92020a3.png)

![](img/a08c4c06cbee22051c346c438cb878af.png)

ChatGPT

### 4、大语言模型产品&系统应该怎么做？

![](img/af511a704ab4e47b7e88506ef20e1678.png)

![](img/d7f6e52f9251917bf451756f5cc38307.png)

Midjourney 四选一

![](img/8d213a9f7bbddf8c2fee8c3f1f0f4efd.png)

Midjourney作图，我外甥涂色😁

### 5、为什么 AGI 大航海时代，战略视角很重要？

「做什么」的沉没成本、维护成本很高

「因为做了A而没做B」的机会成本更高

陆奇： 不蹭热点、勤于学习、行动导向： 要想明白再做、果断行动、不进则退

### 6、不要被竞争视角蒙蔽了双眼

任何理论都有适用范围和条件

竞争视角更适合红海市场，蓝海市场应该更用户视角

### 7、AGI大航海时代：新世界不能看旧船票

AGI只有发令枪，没有地图

为什么新世界不能看旧船票？

旧世界的经验往往是新世界的桎梏，旧世界的 Legacy ，是新世界的 Burden

为什么是Google走出来的 OpenAI 做出了 ChatGPT 、而不是Google？

“小创新靠大厂，大创新靠小厂”（王小川）

怎么办？

要总结过去的经验，也要勇于跳出自己总结的经验

![](img/b407328d8a7fb9abb773b7639ccab37c.png)

### 8、行业大模型是伪命题

“行业大模型解决什么需求？”“融资需求：行业数据 is all you have”

“行业大模型是中国特色词汇，海外没人讲，都是讲通用大模型、AGI”

用行业数据、从零到一预训练一个垂类大模型——技术上不可能成立

基于开源的基座大模型，用行业数据去微调——那就是通用大模型，同时门槛/成本也没那么低
