# 入门必读科普

> 来源：[https://ga67w23t0nn.feishu.cn/docx/B4YbdZXzIo1WKhxvAXpcKRANnkf](https://ga67w23t0nn.feishu.cn/docx/B4YbdZXzIo1WKhxvAXpcKRANnkf)

> 作者：[Derrick Harris](https://a16z.com/author/derrick-harris/)， [Matt Bornstein](https://a16z.com/author/matt-bornstein/) 和 Guido Appenzeller

> 原文地址：https://a16z.com/2023/05/25/ai-canon/

> 整理翻译：AI星球

人工智能研究的发展速度正以指数级增长。即使是AI专家也很难跟上所有新出现的研究成果,对于初学者来说,更是难以知道从何处入手。

因此,在这篇文章中,我们分享了一份我们依赖的精选资源列表,用来提高对现代人工智能的理解。我们称之为"AI正典",因为这些论文、博客文章、课程和指南在过去几年对这个领域产生了巨大影响。

我们从温和地介绍推动当前AI浪潮的transformer模型和潜在扩散模型做起。接下来,我们将深入探讨技术学习资源;构建大型语言模型(LLM)的实用指南;以及对AI市场的分析。最后,我们列出了一份里程碑式研究成果的参考资料,从2017年谷歌发表的"注意力就是你所需要的"论文开始,这篇论文引入了transformer模型,并拉开了生成式AI时代的序幕。

# 基础介绍

这些文章不需要任何专门的背景知识,可以帮助你快速了解现代AI浪潮中最重要的部分。

+   [**Software 2.0**](https://karpathy.medium.com/software-2-0-a64152b37c35)**:**(Andrej Karpathy)是最早清晰解释(在2017年!)为什么这场新的AI浪潮真的很重要的人之一。他的论点是,AI是一种强大的新方式来编程计算机。随着大语言模型的快速改进,这一论点被证明是有先见之明的,并为AI市场的未来发展提供了一个良好的心智模型。

+   [**State of GPT**](https://build.microsoft.com/en-US/sessions/db3f4859-cd30-4445-a0cd-553c3304f8e2)::也来自卡帕西,这是一个非常简单的解释,说明ChatGPT/通用GPT模型是如何工作的,如何使用它们,以及未来的研发可能会采取什么方向。

+   [**What is ChatGPT doing … a**](https://writings.stephenwolfram.com/2023/02/what-is-chatgpt-doing-and-why-does-it-work/)nd why does it work?::计算机科学家和企业家斯蒂芬·沃夫拉姆(Stephen Wolfram)从第一原理出发,对现代AI模型是如何工作的给出了一个冗长但非常易懂的解释。他沿着从早期神经网络到当今的大型语言模型和ChatGPT的时间线进行了解说。

+   Transformers, explained:这篇文章由戴尔·马克威茨(Dale Markowitz)撰写,更直接地回答了"什么是LLM,它是如何工作的?"这是一个很好地方式来轻松进入这个主题,并发展对这项技术的直觉。这篇文章是关于GPT-3的,但仍然适用于更新的模型。

+   How Stable Diffusion works:如何稳定扩散工作:这是计算机视觉领域的类似文章。克里斯·麦考密克(Chris McCormick)给出了一个通俗的解释,说明了稳定扩散是如何工作的,并发展了对文本到图像模型的直觉。要获得更为温和的介绍,可以查看r/StableDiffusion上的这个漫画。

# 基础学习:神经网络、反向传播和嵌入

这些资源提供了对机器学习和人工智能基础思想的基本理解,从深度学习基础到来自AI专家的大学水平课程。

## 文章教程

+   **深度学习-核心概念（**[**Deep learning in a nut**](https://developer.nvidia.com/blog/deep-learning-nutshell-core-concepts/)**s**hell: core concepts）:

这是来自英伟达的四部分系列文章,通过2015年实践的深度学习基础知识,是任何刚开始学习人工智能的人的良好资源。

+   **实用深度学习给程序员（****Pr**actical deep learning for coders）:

这是一门全面的免费课程,通过实际示例和代码解释深度学习基础知识。

+   **解释-Word2vec（****Wo**rd2vec explained）:对嵌入和标记的简单介绍,这是大型语言模型(以及所有语言模型)的构建块。

+   **你应该理解反向传播（****Yes** you should understand backprop）:、如果你想深入理解细节,这里有一篇更深入的关于反向传播的文章。如果你想了解更多,可以尝试斯坦福大学 CS231n 课程的 Youtube 视频。

## 课程

+   [**Stanford CS229**](https://www.youtube.com/playlist?list=PLoROMvodv4rMiGQp3WXShtMGgzqpfVfbU): 斯坦福CS229:安德鲁·恩格(Andrew Ng)的机器学习导论,涵盖机器学习的基础知识。

+   [**Stanford CS224N**](https://www.youtube.com/playlist?list=PLoROMvodv4rOSH4v6133s9LFPRHjEmbmJ): 斯坦福CS224N:克里斯·曼宁(Chris Manning)的自然语言处理与深度学习,从基础到第一代大型语言模型。

# 深入探讨技术:理解变压器和大型模型

有无数资源试图解释大型语言模型是如何工作的。以下是我们最喜欢的一些,针对不同程度的读者/观众。

## 文章教程

+   **图解变压器（****Th**e illustrated transformer）:这是杰伊·阿拉马尔(Jay Alammar)对变压器架构的较为技术性的概述。

+   注释变压器（The annotated transformer）:如果你想在源代码级别理解变压器,这是一篇深入的文章。需要一些PyTorch的知识。

+   **从头构建GPT（**[**Let’s build GPT: from scratch, **](https://www.youtube.com/watch?v=kCc8FmEb1nY)**in** code, spelled out）:用代码一步步解释:对于工程师来说,Karpathy用一段视频演练了如何构建GPT模型。

+   图解稳定扩散（The illustrated Stable Diffusion）:这是对潜在扩散模型(图像生成AI模型中最常见的类型)的介绍。

+   RLHF:人类反馈强化学习（[**RLHF: Reinforcement Learning**](https://huyenchip.com/2023/05/02/rlhf.html) from Human Feedback）:芝普·胡颤(Chip Huyen)解释了RLHF,RLHF可以使大型语言模型表现得更可预测和人性化。这是ChatGPT等系统中最重要但最不为人所理解的方面之一。

+   来自人类反馈的强化学习（[Reinforcement learning](https://www.youtube.com/watch?v=hhiLw5Q_UFg) from human feedback）:OpenAI的联合创始人约翰·舒尔曼(John Shulman)在这个精彩的演讲中,更深入地探讨了具有RLHF的大型语言模型的现状、进展和局限性。

## 课程

+   斯坦福CS25:Transformers United,一个关于Transformer的在线研讨会。

+   [**视频教程-斯坦福CS25**](https://www.youtube.com/watch?v=P127jhj-8-Y)

+   斯坦福CS324:大型语言模型,由李昂、田姓·桥本和克里斯·雷教授,涵盖了大型语言模型的广泛技术和非技术方面。

+   [**视频教程-斯坦福CS324**](https://stanford-cs324.github.io/winter2022/)

+

## 参考和评论

+   **P**redictive learning, NIPS 2016:在这个早期的演讲中,扬·勒棍(Yann LeCun)强有力地阐述了无监督学习作为AI模型架构大规模应用的关键要素。从19:20开始跳过,那里有一个著名的关于蛋糕的比喻,至今仍然是理解现代AI的最佳心智模型之一。

+   **A**I for full-self driving at Tesla:另一个经典的Karpathy演讲,这次是关于特斯拉的数据采集引擎。从8:35开始是AI史上最伟大的演讲之一,解释了为什么长尾问题(在这种情况下是检测停止标志)如此困难。

+   **Th**e scaling hypothesis: 大型语言模型最令人惊讶的方面之一是,简单地增加更多数据和计算就能不断提高准确性。GPT-3是第一个清楚展示这一点的模型,Gwern的这篇文章很好地解释了其中的直觉。

+   **Ch**inchilla’s wild implications: 虽然标称是对重要的Chinchilla论文(见下文)的解释,但这篇文章抓住了大型语言模型规模问题中最关键的问题:我们是否用尽了数据?这建立在上面的帖子之上,对规模定律提供了一种新的看法。

+   **A** survey of large language models:对现有大型语言模型的全面分析,包括发展时间线、规模、训练策略、训练数据、硬件等。

+   [**S**](https://arxiv.org/abs/2303.12712)**p**arks of artificial general intelligence: Early experiments with GPT-4：来自微软研究院、OpenResearch和宾夕法尼亚大学研究人员对GPT-4(当前最先进的大型语言模型)相对于人类智能的能力的早期分析。

+   [**The AI revolution: How Auto-GPT unleashe**](https://pub.towardsai.net/the-ai-revolution-how-auto-gpt-unleashes-a-new-era-of-automation-and-creativity-2008aa2ca6ae)**s **a new era of automation and creativity: 对Auto-GPT和AI代理的介绍。这项技术虽然非常初期,但很重要,它使用互联网访问和自我生成的子任务来解决特定的复杂问题或目标。

+   **Th**e Waluigi Effect: 虽然名义上是对"Waluigi效应"(即为什么大型语言模型行为中会出现"分身"角色)的解释,但主要是深入探讨了LLM提示理论。

+

# 构建基于LLM的实用指南

一个新的应用程序堆栈正在以LLM为核心逐渐出现。虽然在这个主题上还没有多少正式的教育资源,但我们挑选了一些我们发现最有用的资源。

参考

课程

+   [**LLM Bootcamp**](https://fullstackdeeplearning.com/llm-bootcamp/): 查尔斯·弗赖、谢尔盖·卡拉耶夫和乔希·托宾为构建基于LLM的应用程序提供实用课程。

+

+   Hugging Face Transformers: :关于在抱抱脸变形金刚库中使用开源LLM的指南。

## LLM基准测试

+   聊天机器人竞技场:由加州大学伯克利分校的一个团队领导的流行LLM的埃洛级别排名系统。用户也可以通过相互比较模型来参与其中。

+   开放LLM排行榜:来自Hugging Face的排名,将开源LLM在一系列标准基准测试和任务中进行了比较。

# 里程碑式的研究成果

我们今天所见的许多令人惊奇的 AI 产品，都是由大公司和顶级大学的专家进行的令人惊奇的研究成果。最近，我们也看到了个人和开源社区对流行项目进行的卓越工作，例如，通过创建自动化代理或将大模型移植到算力更弱的硬件上运行。

以下是这些论文和项目的集合，供真正想深入研究生成性 AI 的人参考。（对于研究论文和项目，我们还包括了相关的博客文章或网站的链接（如果有的话），这些内容往往以更高的水平做出了解释。我们也包括了原始出版年份，以便您可以追踪基础研究的发展。)

# **大型语言模型**

## **新模型**

+   A[tten](https://ai.googleblog.com/2017/08/transformer-novel-neural-network.html)tion is all you need （2017）：这是由 Google Brain 部门发布的，引发了所有转变的原始 Transformer 工作和研究论文。（博客文章）

+   [**BERT: pre-training of deep bidirectional**](https://arxiv.org/abs/1810.04805) transformers for l[angu](https://ai.googleblog.com/2018/11/open-sourcing-bert-state-of-art-pre.html)age understanding（2018 年）：这是首批公开可用的 LLM 之一，至今仍有许多变体在使用。（博客文章）

+   [**I**](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf)m[prov](https://openai.com/research/language-unsupervised)ing language understanding by generative pre-training （2018）：这是 OpenAI 发布的首篇论文，涵盖了 GPT 架构，它已成为 LLM 发展的主要路径。（博客文章）

+   [**Language models a**](https://arxiv.org/abs/2005.14165)re few-shot learners (   2020)   ：这是 OpenAI 的论文，描述了 GPT-3 和现代 LLM 的仅解码器架构。（Decoder-only architecture）

+   [**T**](https://arxiv.org/abs/2203.02155)ra[inin](https://openai.com/research/instruction-following)g language models to follow instructions with human feedback （2022）：这是 OpenAI 的论文，解释了 InstructGPT，它利用了人在循环训练模型，从而更好地遵循提示中的指令。这是使 LLM 能够为消费者（例如，通过 ChatGPT）使用的关键突破之一。（博客文章）

+   [**LaMDA: language models for**](https://arxiv.org/abs/2201.08239) dialog applications（2022 年）：这[是 Go](https://blog.google/technology/ai/lamda/)ogle 专门设计的模型，用于人类和聊天机器人在各种主题上的自由对话。（博客文章）

+   [**PaLM: Scaling language mo**](https://arxiv.org/abs/2204.02311)deling with pathways（2022 年）：Google 的 [PaLM](https://ai.googleblog.com/2022/04/pathways-language-model-palm-scaling-to.html) 利用了一种[新系统，可以在数千个芯片](https://arxiv.org/abs/2305.10403)上训练 LLM，并且随着模型规模的增大，在某些任务上展示出了超预期的改进。（博客文章）。另请参阅 PaLM-2 技术报告。

+   [**OPT：Open Pre-trained Transfo**](https://arxiv.org/abs/2205.01068)rmer language models （2022）：OPT 是[表现最优](https://ai.facebook.com/blog/democratizing-access-to-large-scale-language-models-with-opt-175b/)秀的全开源 LLM 之一。这个拥有 1750 亿参数的模型的发布附带了代码，并在公开可用的数据集上进行了训练。（博客文章）

+   [**Training compute-optimal l**](https://arxiv.org/abs/2203.15556)a[rge ](https://www.deepmind.com/blog/an-empirical-analysis-of-compute-optimal-large-language-model-training)language models（2022）：Chinchilla 论文。它提出大多数模型受到数据限制，而不是计算限制，并改变了对 LLM 规模的共识。（博客文章）

+   GPT-4 technical report（2023[ 年）：](https://openai.com/research/gpt-4)来自[ OpenAI 的最](https://cdn.openai.com/papers/gpt-4-system-card.pdf)新和最伟大的论文，最为人所知的是它揭示的信息之少！（博客文章）。GPT-4 系统卡片揭示了 OpenAI 如何处理幻觉、隐私、安全性和其他问题。。

+   [**L**](https://arxiv.org/abs/2302.13971)L[aMA:](https://ai.facebook.com/blog/large-language-model-llama-meta-ai/) Open and efficient foundation language models （2023）：来自 Meta 的模型（几乎）开始了一个开源 LLM 革命。与许多最好的闭源模型竞争，但只对研究人员开放了有限制的许可。（博客文章）

+   [**A**](https://crfm.stanford.edu/2023/03/13/alpaca.html)lpaca: A strong， replicable instruction-following model（2023 年）：来自斯坦福大学的这种模型展示了指令调整的力量，特别是在较小的开源模型中，相比于纯粹的规模。

## 模型改进（例如微调、检索、注意力）

+   [**Deep reinforcement learning fr**](https://proceedings.neurips.cc/paper_files/paper/2017/file/d5e2c0adad503c91f91df240d0cd4e49-Paper.pdf)om human preferences （2017）：关于游戏和机器人环境中强化学习的研究，结果证明这是 LLM 的绝佳工具。

+   [**R**](https://arxiv.org/abs/2005.11401)e[trie](https://ai.facebook.com/blog/retrieval-augmented-generation-streamlining-the-creation-of-intelligent-natural-language-processing-models/)val-augmented generation for knowledge-intensive NLP tasks （2020）：由 Facebook 开发，RAG 是通过信息检索提高 LLM 准确性的两个主要研究路径之一。（博客文章）

+   [**I**](https://arxiv.org/abs/2112.04426)mproving language models by retrieving from tri[llio](https://www.deepmind.com/blog/improving-language-models-by-retrieving-from-trillions-of-tokens)ns of tokens （2021 年）：RETRO，即“检索增强型 TRansfOrmers”，这是另一种由 DeepMind 提出的通过访问训练数据中未包含的信息来提高 LLM 准确性的方法。（博客文章）

+   [**LoRA：Low-rank adaptation of l**](https://arxiv.org/abs/2106.09685)arge language models （2021）：这项来自 Microsoft 的研究为在新数据上训练 LLM 提供了一种比微调更有效的替代方案。它现在已经成为社区微调的标准，特别是对于图像模型。

+   Constitutional AI （2022）：Anthropic 团队介绍了来自 AI 反馈的强化学习（RLAIF）的概念。主要的想法是我们可以在其他 AI 的监督下开发出一个无害的 AI 助手。

+   [**FlashAttention: Fast and memory-eff**](https://arxiv.org/abs/2205.14135)i[cien](https://ai.stanford.edu/blog/longer-sequences-next-leap-ai/)t exact attention with IO-awareness（2022）：这项来自斯坦福的研究为最先进的模型打开了理解更长文本序列（和高分辨率图像）而无需高昂的训练时间和成本的大门。（博客文章）

+   [**H**](https://arxiv.org/abs/2212.14052)u[ngry](https://hazyresearch.stanford.edu/blog/2023-01-20-h3) hungry hippos: Towards language modeling with state space models （2022）：同样来自斯坦福，这篇论文描述了语言建模中注意力的主要替代方案之一。这是一条通向更好的扩展和训练效率的有前途的路径。（博客文章）

## **图像生成模型**

+   [**L**](https://arxiv.org/abs/2103.00020)e[arni](https://openai.com/research/clip)ng transferable visual models from natural language supervision （2021）：这篇论文介绍了一种基础模型 CLIP ，将文本描述与图像联系起来。这是计算机视觉中首次有效的大规模使用基础模型。（博客文章）

+   Zero-shot text-to-image generation （2021）：这篇论文介绍[了 DA](https://openai.com/research/dall-e)LL-E，这是一种将上述的 CLIP 和 GPT-3 结合起来，根据文本提示自动生成图像的模型。它的后继者，DALL-E 2，在 2022 年引发了基于图像的生成式 AI 热潮。（博客文章）

+   [**H**](https://arxiv.org/abs/2112.10752)igh-resolution image synthesis with latent diffusion models （2021）：描述稳定扩散的论文（在发布和爆炸性开源增长之后）。

+   [**Photorealistic text-to-image diffusion**](https://arxiv.org/abs/2205.11487) models with deep language understa[nd](https://imagen.research.google/)ing（2022 年）：Imagen 是 Google 进入 AI 图像生成领域的尝试。尽管在宣布后的一年多时间里，该模型截止到本文发布日期仍未公开发布。（网站）

+   [**DreamBooth：Fine tuning text-to-image diffus**](https://arxiv.org/abs/2208.12242)ion models for subject-driven generation （2022[）：](https://dreambooth.github.io/)DreamBooth 是 Google 开发的一种系统，用于训练模型识别用户提交的主题，并将其应用到提示的上下文中（例如 [用户] 在艾菲尔铁塔下微笑）。（网站）

+   [**A**](https://arxiv.org/abs/2302.05543)dding conditional control to text-to-image diffusion models （2023）：这篇来自斯坦福的论文介绍了 ControlNet，这现在是一种非常流行的工具，用于对使用潜在扩散模型的图像生成进行细粒度控制。

## Agents（智能体代理）

+   [**A path to autonomous **](https://openreview.net/pdf?id=BZ5a1r-kVsf)machine intelligence （2022）：Meta AI 领导者和纽约大学教授 Yann LeCun 提出的关于如何构建真正理解周围世界的自主智能代理的建议。

+   [**R**](https://arxiv.org/abs/2210.03629)e[Act：](https://ai.googleblog.com/2022/11/react-synergizing-reasoning-and-acting.html)Synergizing reasoning and acting in language models （2022）：普林斯顿大学和 Google 的一个项目，用来测试和提高 LLM（大型语言模型）的推理和规划能力。（博客文章）

+   [**G**](https://arxiv.org/abs/2304.03442)enerative agents: Interactive simulacra of human behavior （2023）：斯坦福大学和 Google 的研究人员使用 LLM 驱动代理，在类似于“The Sims”（模拟人生）这样的环境中，其互动是自发的，而不是由编程驱动的。

+   [**R**](https://arxiv.org/abs/2303.11366)eflexion: an autonomous agent with dynamic memory and self-reflection （2023）：来自东北大学和 MIT 的研究人员的工作，他们通过从错误和过去的经验中学习，教导 LLM 更可靠地解决问题。

+   [**T**](https://arxiv.org/abs/2302.04761)oolformer：Language models can teach themselves to use tools （2023）：这个来自 Meta 的项目训练 LLM 使用外部工具（在这种情况下，API 指向搜索引擎和计算器等东西），以提高准确性，而不增加模型大小。

+   [**Auto-GPT: An autonom**](https://github.com/Significant-Gravitas/Auto-GPT)ous GPT-4 experiment : 一个开源实验项目，通过给 GPT-4 提供一组工具（互联网访问、文件存储等）并选择使用哪些工具来解决特定任务，以扩大 GPT-4 的能力。

+   [**BabyAGI**](https://github.com/yoheinakajima/babyagi)：这个 Python 脚本使用 GPT-4 和向量数据库（用来存储上下文），以便计划并执行一系列解决更广泛目标的任务。

特别感谢[ Jack Soslow](https://a16z.com/author/jack-soslow/)、[Jay Rughani](https://a16z.com/author/jay-rughani/)、[Marco Mascorro](https://a16z.com/author/marco-mascorro/)、[Martin Casado](https://a16z.com/author/martin-casado/)、R[ajko Radovano](https://a16z.com/author/vijay-pande/)vic 和 Vijay Pande 对本文的贡献，[也感谢整个 a16z 团队总是](https://a16z.com/author/sonal-chokshi/)能对 AI 的最新进展进行积极的讨论。还要感谢 Sonal Chokshi 和加密货币团队在公司建立了一系列经典内容。

![](img/87f39f547f9e8a06b4a91b357208eb90.png)