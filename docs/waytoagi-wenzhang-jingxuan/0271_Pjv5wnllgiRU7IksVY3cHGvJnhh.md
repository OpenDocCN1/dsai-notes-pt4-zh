# 入门经典必读

> 来源：[https://waytoagi.feishu.cn/docx/JPBMduSM6o5RkxxWm1EcrpkGnQH](https://waytoagi.feishu.cn/docx/JPBMduSM6o5RkxxWm1EcrpkGnQH)

> 作者：[Derrick Harris](https://a16z.com/author/derrick-harris/)， [Matt Bornstein](https://a16z.com/author/matt-bornstein/) 和 Guido Appenzeller

> 原文地址：https://a16z.com/2023/05/25/ai-canon/

> 译者：通往 AGI 之路

> 后半部分：[目录：a16z推荐进阶经典](https://ywh1bkansf.feishu.cn/wiki/EXX1w3QyFiBz8fknLFYce7gvnug)

人工智能的研究正在以指数级别的速度增长。对于 AI 专家来说，跟上所有新发布的内容已经非常困难，对于初学者来说更是难上加难。

因此，在这篇文章中，**我们****分**享了一份我们信赖的、用于更深入了解现代 AI 的精选资源列表。我们称之为“AI 典藏”，因为这些论文、博客文章、课程和指南在过去几年中对该领域产生了巨大影响。

我们首先对 Transformer 和 Latent Diffusion（潜在扩散）模型进行了简单的介绍，这些模型正在推动当前的 AI 浪潮。接下来，我们深入研究了技术学习资源；构建大型语言模型（LLMs）的实用指南；以及 AI 市场的分析。最后，我们列出了一份里程碑式研究成果的参考列表，从 2017 年谷歌发布的“Attention is All You Need”开始——这篇论文向世界介绍了 Transformer 模型，并开启了生成 AI 的时代。

# 轻松的入门……

这些文章不需要专门的背景知识，可以帮助你快速理解现代 AI 浪潮的最重要部分。

+   [**Software 2.0**](https://karpathy.medium.com/software-2-0-a64152b37c35)：Andrej Karpathy 是最早清楚解释（在 2017 年！）为什么新的 AI 浪潮真正重要的人之一。他的论点是，AI 是一种新的、强大的编程计算机的方式。随着大语言模型（LLMs）的快速改进，这个论点被证明是有先见之明的，并为 AI 市场的可能进展提供了一个良好的思维模型。

+   翻译：[软件2.0（Software 2.0）](https://ywh1bkansf.feishu.cn/wiki/FFJawIKRpi77JokULCvcjIPBnBv)

+   [**State of GPT**](https://build.microsoft.com/en-US/sessions/db3f4859-cd30-4445-a0cd-553c3304f8e2)：这也是 Karpathy 的文章，这是一个非常容易理解的解释，说明了 ChatGPT / GPT 模型一般如何工作，如何使用它们，以及研发可能采取的方向。

+   翻译：[GPT的现状（State of GPT）](https://ywh1bkansf.feishu.cn/wiki/IXjfwF9l9iX0IzkUg1Ec0Vcinx3)

+   [**What is ChatGPT doing … a**](https://writings.stephenwolfram.com/2023/02/what-is-chatgpt-doing-and-why-does-it-work/)nd why does it work?：计算机科学家和企业家 Stephen Wolfram 给出了一篇长而易读的解释，从一开始的原理解释了现代 AI 模型是如何工作的。他跟随从早期神经网络到今天的 LLMs 和 ChatGPT 的时间线。

+   翻译：[ChatGPT是在做什么，为什么它有效？](https://ywh1bkansf.feishu.cn/wiki/LnMOwmKIwihcRakRalcczNgrnjf)

+   Transformers， explained：这篇文章由 Dale Markowitz 撰写，是对“什么是 LLM，它是如何工作的？”这个问题的一个更短、更直接的回答。这是一种很好的方式，可以轻松地进入这个主题，并对这项技术建立直观理解。这篇文章是关于 GPT-3 的，但仍适用于新的模型。

+   翻译：[解析Transformer模型：理解GPT-3、BERT和T5背后的模型](https://ywh1bkansf.feishu.cn/wiki/WCOcwp3DYiNj2mkiGVycjJ0Znaf)

+   How Stable Diffusion[ w](https://www.reddit.com/r/StableDiffusion/comments/zs5dk5/i_made_an_infographic_to_explain_how_stable/)orks：这是一篇与上一篇文章在计算机视觉领域的对应文章。Chris McCormick 为非专业人士解释了 Stable Diffusion 是如何工作的，并从文本到图像模型的角度，帮助你对这种技术建立直观理解。如果你希望更轻松地理解这个概念，可以查看来自 r/StableDiffusion 的这个漫画。

+   翻译：[稳定扩散（Stable Diffusion）是如何运作的](https://ywh1bkansf.feishu.cn/wiki/TNIRw7qsViYNVgkPaazcuaVfndc)

---

# 基础学习：神经网络、反向传播和嵌入

这些资源为你提供了机器学习和 AI 基本概念的基础理解，从深度学习的基础知识到 AI 专家的大学水平课程。

### 讲解资源

+   [**Deep learning in a nut**](https://developer.nvidia.com/blog/deep-learning-nutshell-core-concepts/)shell: core concepts：这是 Nvidia 的四部分系列文章，介绍了 2015 年实践中的深度学习基础，对于刚开始学习 AI 的人来说是一个很好的资源。

+   翻译：[深度学习（1）核心概念](https://waytoagi.feishu.cn/wiki/UdLFwdjgPiXm5tkVi0dcfcRMn0f)

+   翻译：[深度学习（2）历史和训练](https://waytoagi.feishu.cn/wiki/QacGwCWxoizfmlkOsKFc0PHQnWb)

+   翻译：[深度学习（3）序列学习](https://waytoagi.feishu.cn/wiki/CHMXwqoEYibzHdkC3QmcXZ53niX)

+   翻译：[深度学习（4）强化学习](https://waytoagi.feishu.cn/wiki/X8WZwBW9jiynLhkGOd2cBB56nMc)

+   Practical deep learning for coders：通过实用的例子和代码，解释了 AI 基础知识的全面、免费的课程。

+   Word2vec explained：对嵌入和令牌的简单介绍，它们是 LLMs（和所有语言模型）的构建块。

+   翻译：[Word2Vec 详解](https://waytoagi.feishu.cn/wiki/PJYJwS9Uei3l8DkOsBocnyvNnud)

+   Yes you should understand backprop：如果你想理解细节，这是关于反向传播更深入的文章。如果你想了解更多，可以看看 Youtube 上的 Stanford CS231n 讲座。

+   翻译：[是的！你应该了解反向传播](https://waytoagi.feishu.cn/wiki/JMQewmoeWidXrXkZzhmcjN1Rn6z)

### 课程

+   [**Stanford CS229**](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU)：Andrew Ng 的机器学习入门课程，覆盖了机器学习的基础知识。

+   [**Stanford CS224N**](https://www.youtube.com/playlist?list=PLoROMvodv4rOSH4v6133s9LFPRHjEmbmJ)：Chris Manning 的深度学习自然语言处理（NLP）课程，通过第一代 LLM 介绍涵盖了 NLP 基础知识。

---

# 入门文章的翻译

多维表格（FGDwbll5Rat4gnsRL08chvbQnCf_tblTDPwtTvCbSs5I）

群友解读：

张海庚

https://mp.weixin.qq.com/s/cpLDPDbTjarU0_PpBK_RDQ