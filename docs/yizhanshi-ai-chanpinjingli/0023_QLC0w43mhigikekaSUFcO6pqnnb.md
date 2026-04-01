# 进阶之路

> 来源：[https://v11enp9ok1h.feishu.cn/docx/AyErdkBHGo0nzVxHG2mcBKi3nLe](https://v11enp9ok1h.feishu.cn/docx/AyErdkBHGo0nzVxHG2mcBKi3nLe)

<https://v11enp9ok1h.feishu.cn/sync/VztedzIoUstVPDb6RNMctK8Unie>

本文档将带领开发者更深入的使用 GLM-4-9B开源模型，您可以通过子文档列表进行查看。

### GLM结构

再深入了解LLM之前，下面的内容你需要逐个了解，这里只做简单介绍。链接也许不会随着技术迭代更新，请关注最新的技术生态，假如在这里你止步不前，很抱歉，这里需要有机器学习基础，也许你需要关注下这部分内容。

**历史工作**：GLM系列LLMs建立在Transformer (Attention is all you need)之上, GLM模型迭代的过程中在transformer框架上的尝试了众多技术选型，目前 GLM-4 的技术仍在更新，关注下面的基础内容或许对你理解历史工作有些帮助

#### tokenization&tokenizers：分词算法与分词器

分词算法[与分词器作为LLM基](https://huggingface.co/docs/trl/main/en/sft_trainer)本组件， 对主流的分词算法和分词器全面的了解或许可以免去不少认知上的问题，GLM的语言模型拥有一些特殊令牌，这些令牌在对话中的不同角色（例如用户、助手和系统）之间添加，并帮助模型识别对话的结构和流程。此设置对于使模型能够在聊天环境中生成连贯且上下文适当的响应至关重要。了解这个之前你可能需要掌握模型结构，请查阅有监督训练的基础内容

#### 位置编码与attention机制

对于经过word embedding并加入RoPE的输入序列 ，获得其虚数空间中的空间距离参数，cos, sin = self.rotary_emb(value_states, seq_len=kv_seq_len)。获得带有空间距离的计算矩阵Q,矩阵K，query_states, key_states = apply_rotary_pos_emb(query_states, key_states, cos, sin, position_ids)

**基本的计算公式**如下**,这**里你可能需要注意矩阵计算过程，是行列依次相乘，所以是某行乘以某列，自然其它行和参考的是其他列是这里的  是 query_states在 权重x 特征的结果，点积的结果方向不能变换，自然如果你写成 也许没问题，也没必要纠结

这一步的计算量为O(2D_kND_x+D_vND_x)=O(ND_x(2D_k+D_v)))=O(N)。

GLM训练阶段删除了所有偏差项，但注意力层的查询、键和值 （QKV） 矩阵中的偏差除外。

#### 基础架构与attention mask

GLM将多头注意力 （MHA） 替换为组查询注意力 （GQA），改变了一部分参数,详细查询论文

GQA将所有Head分为个组 （ 可以整除 ），每组共享同一对K、V，

为便于理解上下两个公式

请注意   =  ,且  。也就是说，这里的   等于上面的

同时，你需要了解上下两个公式依然是抽象的公式，并不完整

用数学公式表示为

#### 归一化

Layer Normalization 在训练**过程中矩阵转换的两**个指标起着决定的作用，平移不变和缩放不变，这个公式一直没有被替换的原因是在整个数据流转过程中，矩阵A到A'的过程需要是相似而不是变形。

Root Mean Square Layer Normalization认为矩阵中参数在矩阵空间中位置不同，不影响矩阵的A到A'的形状。

GLM模型基于此原则将线性层转换为RMSNorm。

#### 激活函数

激活函数采用SwiGLU的替换带来了更好的模型性能。

#### 预训练系列

#### PEFT

PEFT（Parameter-Eff[icient Fine](https://huggingface.co/docs/peft/index)-Tuning）由huggingface组织的一个微调适配框架。通过PEFT框架对模型参数进行微调，可以显著降低了计算和存储成本，同时产生的性能与全量微调的模型相当。这使得在消费硬件上大型语言模型 （LLMs）训练变得更加容易。

#### RLHF

### 文章引用

+   [H](https://mp.weixin.qq.com/s?__biz=MzU5MzcwODE3OQ==&mid=2247485594&idx=1&sn=f3de0f45800d1705aad1173a33330d62&chksm=fe0d1d66c97a94702c3d02335c763a14e6cf8009cff1670bd3d52208e6bb9c796b94d3da8856&cur_album_id=1343689864403042305&scene=189#wechat_redirect)uggingface🤗NLP笔记4：Models，Tokenizers，以及如何做Subword tokenization

+   [What are special to](https://www.linkedin.com/pulse/what-special-tokens-tokenization-farhan-naqvi-uxxsf)kens in Tokenization

+   Training with PEFT: Memory efficient RLHF training using adapters with PEFT

+   [LoRA](https://huggingface.co/papers/2106.09685): Low-Rank Adaptation of Large Language Models

+   [缓存与效果的极限拉扯](https://spaces.ac.cn/archives/10091)：从MHA、MQA、GQA到MLA