
An increasing number of applications are making use of LLM-generated data for training or evalua[tion](https://www.promptingguide.ai/applications/synthetic_rag)s, including distillation, dataset augmentati[on, ](https://argilla.io/blog/mantisnlp-rlhf-part-4/)AI-assisted evaluation and labeling, self-critique, and more. This post demonstrates how to construct such a synthetic dataset (in a R[AG c](https://www.anthropic.com/news/claudes-constitution)ontext), and this post from Argilla gives an overview of RLAIF, which is often a popular alternative to RLHF, given the challenges associated with gathering pairwise human preference data. AI-assisted feedback is also a central component of the “Constitutional AI” alignment method pioneered by Anthropic (see their blog for an overview).

越来越多的应用程序正在利用 LLM 生成的数据进行训练或评估，包括蒸馏、数据集扩充、人工智能辅助评估和标记、自我批评等。这篇文章演示了如何构建这样一个综合数据集（在 RAG 环境中），Argilla 的这篇文章概述了 RLAIF，考虑到收集成对人类偏好数据相关的挑战，RLAIF 通常是 RLHF 的流行替代品。人工智能辅助反馈也是 Anthropic 首创的“宪法人工智能”对齐方法的核心组成部分（有关概述，请参阅他们的博客）。

## Chapter 32: Representation Engineering 第32章：表示工程

Representation Engineering is a new and promising technique for fine-grained steering of language model outputs via “control vectors”. Somewhat similar to LoRA adapters, it has the effect of adding low-rank biases to the weights of a network which can elicit particular response styles (e.g. “humorous”, “verbose”, “creative”, “honest”), yet is much more computationally efficient and can be implemented without any training required. Instead, the method simply looks at differences in activations for pairs of inputs which vary along the axis of interest (e.g. honesty), which can be generated synthetically, and then performs dimensionality reduction.

表示工程是一种新的、有前途的技术，用于通过“控制向量”对语言模型输出进行细粒度控制。与 LoRA 适配器有些相似，它具有向网络权重添加低秩偏差的效果，这可以引发特定的响应风格（例如“幽默”、“冗长”、“创造性”、“诚实”），但计算效率更高，无需任何培训即可实施。相反，该方法只是查看沿兴趣轴（例如诚实度）变化的输入对的激活差异，这些输入对可以综合生成，然后执行降维。

See this short [blog post](https://www.safe.ai/blog/representation-engineering-a-new-way-of-understanding-models) from Center for AI Safety (who pioneered the[ met](https://vgel.me/posts/representation-engineering/)hod) for a brief overview, and this post from Th[eia Vogel for a](https://www.youtube.com/watch?v=PkA4DskA-6M) technical deep-dive with code examples. Theia also walks through the method in this podcast episode.

请参阅人工智能安全中心（该方法的先驱）的这篇简短博客文章以获取简要概述，以及 Theia Vogel 的这篇文章以通过代码示例进行技术深入探讨。 Theia 也在本播客节目中介绍了该方法。

## Chapter 33: Mechanistic Interpretability 第33章：机械可解释性

Mechanistic Interpretability (MI) is the dominant paradigm for understanding the inner workings of LLMs by identifying sparse representations of “features” or “circuits” encoded in model weig[hts. Beyon](https://www.neelnanda.io/)d enabling potential modification or explanation of LLM outputs, MI is often viewed as an important step towards potentially “aligning” increasingly powerful systems. Most of the references here will come from Neel Nanda, a leading researcher in the field who’s created a large number of useful educational resources about MI across a range of formats:

机械可解释性 (MI) 是通过识别模型权重中编码的“特征”或“电路”的稀疏表示来理解 LLMs 内部工作原理的主导范例。除了能够对 LLM 输出进行潜在的修改或解释之外，MI 通常被视为潜在地“调整”日益强大的系统的重要一步。这里的大部分参考资料都来自 Neel Nanda，他是该领域的一位领先研究人员，他创建了大量关于 MI 的有用的教育资源，涵盖多种格式：

+   [“](https://www.neelnanda.io/mechanistic-interpretability/glossary)[A Comprehensiv](https://www.neelnanda.io/mechanistic-interpretability/glossary)e Mechanistic Interpretability Explainer & Glossary”

“全面的机制解释器和术语表”

+   [“A](https://www.neelnanda.io/mechanistic-interpretability/favourite-papers)n Extremely Opinionated Annotated List of My Favourite Mechanistic Interpretability Papers”

“我最喜欢的机械可解释性论文的极端固执己见的注释列表”

+   [“Mechanistic Interpretabili](https://www.lesswrong.com/posts/jLAvJt8wuSFySN975/mechanistic-interpretability-quickstart-guide)ty Quickstart Guide” (Neel Nanda on LessWrong)

“机械可解释性快速入门指南”（LessWrong 上的 Neel Nanda）

+   [“How useful is mechanisti](https://www.lesswrong.com/posts/tEPHGZAb63dfq2v8n/how-useful-is-mechanistic-interpretability)c interpretability?” (Neel and others, discussion on LessWrong)

“机械解释有多大用处？” （Neel 等人，LessWrong 上的讨论）

+   [“200 Concrete Problems ](https://docs.google.com/spreadsheets/d/1oOdrQ80jDK-aGn-EVdDt3dg65GhmzrvBWzJ6MUZB8n4/edit#gid=0)In Interpretability” (Annotated spreadsheet of open problems from Neel)

“200 个可解释性的具体问题”（Neel 的开放问题带注释的电子表格）

Addit[ionally, the articles “Toy Models of Supe](https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html)rposition” and “Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet” from Anthropic are on the longer side, but feature a number of great visualizations and demonstrations of these concepts.

此外，Anthropic 的文章“Toy Models of Superposition”和“Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet”篇幅较长，但对这些概念进行了许多精彩的可视化和演示。

## Chapter 34: Linear Representation Hypotheses 第34章：线性表示假设

An emerging[ theme fr](https://www.beren.io/2023-04-04-DL-models-are-secretly-linear/)[om s](https://www.youtube.com/watch?v=ko1xVcyDt8w)e[veral lines of interpr](https://arxiv.org/pdf/2310.02207)etability research has been the observation that internal representations of features in Transformers are often “linear” in high-dimensional space (a la Word2Vec). On one hand this may appear initially surprising, but it’s also essentially an implicit assumption for techniques like similarity-based retrieval, merging, and the key-value similarity scores used by attention. See this blog post by Beren Millidge, this talk from Kiho Park, and perhaps at least skim the paper “Language Models Represent Space and Time” for its figures.

多个可解释性研究中的一个新兴主题是观察到 Transformer 中特征的内部表示在高维空间中通常是“线性的”（类似于 Word2Vec）。一方面，这可能最初看起来令人惊讶，但它本质上也是基于相似性的检索、合并和注意力所使用的键值相似性分数等技术的隐含假设。请参阅 Beren Millidge 的博客文章和 Kiho Park 的演讲，也许至少浏览一下论文“语言模型代表空间和时间”中的数据。

# Section VI: Performance Optimizations for Efficient Inference 第六节：高效推理的性能优化

**Goal:** Survey architecture choices and lower-level techniques for improving resource utilization (time, compute, memory).

目标：调查用于提高资源利用率（时间、计算、内存）的架构选择和底层技术。

Here w[e’ll look](https://developer.nvidia.com/blog/mastering-llm-techniques-inference-optimization/) at a handful of techniques for improving the speed and efficiency of inference from pre-trained Transformer language models, most of which are fairly widely used in practice. It’s worth first reading this short Nvidia blog post for a crash course in several of the topics we’ll look at (and a number of others).

在这里，我们将介绍一些用于提高预训练 Transformer 语言模型的推理速度和效率的技术，其中大多数在实践中得到了相当广泛的应用。值得首先阅读这篇简短的 Nvidia 博客文章，了解我们将要讨论的几个主题（以及其他一些主题）的速成课程。

## Chapter 35: Parameter Quantization 第35章：参数量化

With the rapi[d increas](https://timdettmers.com/2022/08/17/llm-int8-and-emergent-features/)e in p[aram](https://www.tensorops.ai/post/what-are-quantized-llms)eter counts for leadin[g LL](https://www.maartengrootendorst.com/blog/quantization/)Ms and difficulties (both in cost and availability) in acquiring GPUs to run models on, there’s been a growing interest in quantizing LLM weights to use fewer bits each, which can often yield comparable output quality with a 50-75% (or more) reduction in required memory. Typically this shouldn’t be done naively; Tim Dettmers, one of the pioneers of several modern quantization methods (`LLM.int8()`, QLoRA, bitsandbytes) has a great blog post for understanding quantization principles, and the need for mixed-precision quantization as it relates to emergent features in large-model training. Other popular methods and formats are GGUF (for `llama.cpp`), AWQ, H🐧, and GPTQ; see this post from TensorOps for an overview, and this post from Maarten Grootendorst for a discussion of their tradeoffs.
