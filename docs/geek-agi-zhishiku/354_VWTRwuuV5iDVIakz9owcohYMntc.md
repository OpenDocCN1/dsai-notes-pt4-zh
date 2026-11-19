
# Transformer动画讲解 - 向量化

> 来源：`https://geekbang.feishu.cn/docx/RisBdf6OVoQhuuxRWG5cwq7DnxU`
>
> 👨🏻 作者：AllenTang
>
> 📃 作者介绍：分享人工智能，让所有人玩转AI
>
> 🛖 👸号：架构师带你玩转AI
>
> ⏰ 收录时间：2024-05-23 11:28:42
>
> 🔗 原文链接：`https://mp.weixin.qq.com/s/NeHjcQ3YPc9sDr02zmnC6w`

![](img/630223a8e1bc83029befda17c1cd7850.png)

关注作者👸号，获取更多AI知识

**动画地址：查看原文，可看动画**

从端到端的角度来看，数据在`Transformer`中的流转可以概括为四个阶段：`Embedding`（嵌入）、`Attention`（注意力机制）、`MLPs`（多层感知机）和`Unembedding`（从模型表示到最终输出）。

![](img/bbd836e2c66a5072c89aca7005ddae10.png)

`Embedding -> Attention -> MLPs -> Unembedding`

下面对第一个阶段`Embedding`（向量化）进行详细介绍：

“`Embedding`”在字面上的翻译是“嵌入”，但在机器学习和自然语言处理的上下文中，我们更倾向于将其理解为一种“向量化”或“向量表示”的技术，这有助于更准确地描述其在这些领域中的应用和作用。

![](img/10193a3fb41a8fa005605d303d039d43.png)

## Embedding（向量化）

### Embedding（向量化）的目的

+ **数值化表示：**`Embedding`的目的主要是指将离散的高维数据（如文字、图片、音频、视频）映射到低维度的连续向量空间。这个过程会生成由实数构成的向量，用于捕捉原始数据的潜在关系和结构。

+ **捕捉语义信息：**`Embedding`向量通常能够捕获单词之间的语义关系。在训练过程中，模型会学习将具有相似含义或上下文的单词映射到相近的向量空间位置，使得向量之间的距离或夹角能够反映单词之间的语义相似度。

+ **提取复杂特征：**`Embedding`层通过提供的连续向量表示，能够更全面地捕捉数据的复杂特征，使模型能够更好地学习并应用这些特征。

![](img/edecb294810ba9a4ecd7eefa68533985.png)

神经网络算法 - 一文搞懂`Embedding`（嵌入）

### Embedding（向量化）的工作流程

+ `Tokenization`：

+ **（1）对于文本数据：**在自然语言处理（NLP）中，`Tokenization` 是一个关键步骤，它将输入的文本内容（如句子、段落或整个文档）拆分成更小的片段或元素，这些片段通常被称为词元（`tokens`）。

+ **（2）对于非文本数据（如音频、图像或视频）：**`Tokenization` 的概念也可以类比应用。例如，在音频处理中，音频信号可以被分割成帧（`frames`）作为音频词元；在图像处理中，图像可以被分割成图像块（`patches`）作为图像词元；在视频处理中，视频可以被分割成视频块（`patches`）作为视频词元。

+ **词元`Tokens`：**

+ **（1）文本`Tokens`：**在 NLP 中，`tokens` 是许多任务的基础，如词性标注（POS tagging）、命名实体识别（NER）、句法分析（parsing）和文本分类等。对于文本来说，`Tokens`通常是单词。

![](img/f00017bd500f9e984c710e936ee8ecc0.png)

文本`Tokens`

+ **（2）图像或者声音`Tokens`：**在图像和声音处理中，`Tokens` 的表示方式与文本有所不同，但它们的核心思想仍然是将输入数据分解为可处理的基本单元。图像或者声音，`Tokens`表示为图像块或者声音块。

![](img/61b8fc3a8a2fcd21b3f5f72a096a2b11.png)

图像或者声音`Tokens`

+ **（3）视频`Tokens`：**为了将视觉数据转换成适合生成模型处理的格式，研究者提出了视觉块嵌入编码（`visual patches`）的概念。这些视觉块是图像或视频的小部分，类似于文本中的词元。`OpenAI`的`Sora`将视觉数据转换为视觉块（Turning visual data into patches）。

![](img/2ce746917ec35524a18d6cb37f7afbec.png)

将视觉数据转换为视觉块

+ **`Embedding`向量：**

+ **（1）向量语义表示：**`Embedding`向量是一个固定大小的实数向量，每个`Token`对应到一个`Embedding`向量，用于表示`Token`在语义空间中的位置。而这些向量都是通过无监督学习算法在大量数据上训练得到的。

向量的语义表示

+ **（2）向量语义相似度：**在高维空间中，含义相似的词汇倾向于在彼此相近的向量上。这是因为算法在训练过程中学会了将具有相似上下文的单词映射到相近的向量空间中。

![](img/c07b76a43193e4d35b502031e15b8446.png)

向量语义相似度

大模型开发 - 一文搞懂`Embedding`工作原理

### 参考资料

+ `https://www.3blue1brown.com/lessons/gpt`
