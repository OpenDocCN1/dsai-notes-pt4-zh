# 进阶：a16z推荐进阶经典

> 来源：[https://waytoagi.feishu.cn/docx/VqVKdhqu9ogFxqxsgzkcgLiin3g](https://waytoagi.feishu.cn/docx/VqVKdhqu9ogFxqxsgzkcgLiin3g)

> 作者：[Derrick Harris](https://a16z.com/author/derrick-harris/)， [Matt Bornstein](https://a16z.com/author/matt-bornstein/) 和 Guido Appenzeller

> 原文地址：https://a16z.com/2023/05/25/ai-canon/

> 译者：通往 AGI 之路

> 前半部分：[目录：a16z 推荐入门经典](https://ywh1bkansf.feishu.cn/wiki/F8OMwrI3TisTPokQAJHcMG2knBh)

# 技术深度探讨：了解 transformers 和大模型

有无数的资源（有些内容更好些）试图解释大语言模型（LLMs）的工作原理。以下是我们的一些最爱，面向广泛的读者/观众。

### 讲解资源

+   The illustrated transformer：Jay Alammar 对 transformer 架构的更多技术概述。

+   The annotated transformer：如果你想在源代码级别理解 transformer 模型，这是一篇深度文章。需要一些 PyTorch 的知识。

+   [**Let’s build GPT: from scratch， **](https://www.youtube.com/watch?v=kCc8FmEb1nY)in code， spelled out：从零开始，通过代码，详细解释：对于工程师们，Karpathy 做了一个如何构建 GPT 模型的视频演示。

+   **T**he illustrated Stable Diffusion：对潜在扩散模型的介绍，这是最常见的用于图像生成的 AI 模型。

+   [**RLHF: Reinforcement Learning**](https://huyenchip.com/2023/05/02/rlhf.html) from Human Feedback：Chip Huyen 解释了 RLHF（基于人类反馈的强化学习 ），它可以使 LLMs 的行为更可预测、更符合人类的友好方式。这是像 ChatGPT 这样的系统中最重要但最不好理解的方面之一。

+   [**Reinforcement learning**](https://www.youtube.com/watch?v=hhiLw5Q_UFg) from human feedback：计算机科学家和 OpenAI 联合创始人 John Shulman 在这个精彩的演讲中更深入地探讨了 LLMs（大语言模型）与 RLHF（基于人类反馈的强化学习 ）的当前状态、进展和限制。

### 课程

+   [**Stanford CS25**](https://www.youtube.com/watch?v=P127jhj-8-Y)：Transformer 技术联盟，关于 Transformer 技术的在线研讨会。

+   [**Stanford CS324**](https://stanford-cs324.github.io/winter2022/)：由 Percy Liang， Tatsu Hashimoto 和 Chris Re 主讲的《大型语言模型》课程，涵盖了大型语言模型的各种技术和非技术方面。

### **参考和评论**

+   Predictive learning， NIPS[ 2016：在](https://youtu.be/Ount2Y4qxQo?t=1160)这次早期的演讲中，Yann LeCun 强烈主张无监督学习是大规模 AI 模型架构的关键元素。跳到 19:20 查看他著名的蛋糕类比，这仍然是现代 AI 最好的心智模型之一。

+   AI for full-self d[riving](https://youtu.be/hx7BXih7zx8?t=515) at Tesla:：另一个经典的 Karpathy 演讲，这次他介绍了特斯拉的数据收集引擎。从 8:35 开始，他进行了一次伟大的 AI 演讲，解释了为什么长尾问题（在这种情况下是停车标志检测）如此困难。

+   The scaling hypothesis：大型语言模型最令人惊讶的方面之一：规模化（增加更多的数据和计算）会继续提高准确性。GPT-3 是第一个清楚展示这一点的模型，Gwern 的文章很好地解释了其背后的直觉。

+   Chinchilla's wild implications：名义上是对重要的 Chinchilla 论文的解释，这篇文章触及了 LLM 规模化的大问题的核心：我们是否正在耗尽数据？这篇文章在上面文章的基础上，给出了对规模化规律的新鲜视角。

+   A survey of large language models：对当前 LLM 的全面分析，包括发展时间线、规模、训练策略、训练数据、硬件等。

+   [**S**](https://arxiv.org/abs/2303.12712)parks of artificial general intelligence: Early experiments with GPT-4：微软研究部对当前最先进的 LLM（GPT-4）相对于人类智能能力的早期分析。

+   [**The AI revolution: How Auto-GPT unleashe**](https://pub.towardsai.net/the-ai-revolution-how-auto-gpt-unleashes-a-new-era-of-automation-and-creativity-2008aa2ca6ae)s a new era of automation and creativity：介绍 Auto-GPT 和 AI Agents。这项技术还很早期，但重要的是要理解它——它使用互联网访问和自我生成的子任务来解决特定的、复杂的问题或目标。

+   The Waluigi Effect：名义上是对“Waluigi 效应”的解释（即，为什么 LLM 行为中会出现“另我”）【注：在回应不同的提示或问题时，它可能会表现出不同的“个性”或“角色”】的解释，但其主要的有趣之处在于它对 LLM 提示理论的深入研究。

---

# 使用 LLM 进行构建的实用指南

新的应用栈正在以 LLM 为核心形成。虽然目前还没有很多关于此主题的正规教育课程，但我们找到了一些最有用的资源。

### **参考**

+   [**B**](https://dagster.io/blog/chatgpt-langchain)uild a GitHub support bot with GPT3， LangChain， and Python：这是关于现代 LLM 应用栈的最早的公开解释之一。这里的一些建议可能已经过时，但在很多方面，它开启了新一代 AI 应用的广泛接受和实践。

+   [**Building LLM applica**](https://huyenchip.com/2023/04/11/llm-engineering.html)tions for production：Chip Huyen 讨论了构建 LLM 应用的许多关键挑战，如何解决这些挑战，以及哪种类型的用例最有意义。

+   Prompt Engin[eering Guide：](https://github.com/brexhq/prompt-engineering)对于任何编写 LLM 提示的人——包括应用开发者——这是最全面的指南，对一些流行模型提供了具体示例。如果想要更轻松、更富有对话性的处理，可以尝试阅读 Brex 的提示工程指南。

+   [**Prompt injection: What’s the wo**](https://simonwillison.net/2023/Apr/14/worst-that-can-happen/)**r**st that can happen? 可能会发生什么最糟糕的事情？提示注入是 LLM 应用潜在的严重安全漏洞，目前还没有完美的解决方案。Simon Willison 在这篇文章中对这个问题给出了最终的描述。Simon 关于 AI 的几乎所有内容都是非常棒的。

+   [**OpenAI cookbook**](https://github.com/openai/openai-cookbook/tree/main)：对于开发者来说，这是使用 OpenAI API 的指南和代码示例的最权威收集。它会不断更新新的代码示例。

+   Pinecone learning center：许多 LLM 应用都是基于向量搜索范式。尽管 Pinecone 的学习中心是其品牌所提供的内容，但它提供了如何在这种模式中构建的最有用的指导。

+   [**LangChain docs**](https://python.langchain.com/en/latest/index.html)：作为 LLM 应用的默认协调层，LangChain 将堆栈中的所有其他部分连接在一起。因此，他们的文档对于理解整个技术栈以及各部分如何协同工作提供了实用的参考。

### 课程

+   [**LLM Bootcamp**](https://fullstackdeeplearning.com/llm-bootcamp/)：这是一个实践课程，由 Charles Frye、Sergey Karayev 和 Josh Tobin 主导，专注于构建基于 LLM 的应用。

+   Hugging Face Transformers：这是一个指南，教你如何使用 Hugging Face transformers 库中的开源 LLM。

### **LLM**** 基准**

+   [**Chatbot Arena**](https://lmsys.org/blog/2023-05-03-arena/)：这是一个由 UC Berkeley 的团队领导的，采用 Elo 评分系统对热门 LLM 进行排名的平台。用户也可以通过进行模型间的直接比较参与其中。

+   Open LLM Leaderboard：是一个由 Hugging Face 提供的排行榜，比较开源 LLM 在一系列标准基准和任务中的表现。

---

# 市场分析

我们都为生成式人工智能所能创造的东西感到惊叹，但仍然有很多问题需要回答。哪些产品和公司将生存和繁荣？艺术家会发生什么变化？公司应该如何利用它？它将如何影响就业和整个社会？以下是一些试图回答这些问题的尝试。

### **a16z 思考**

+   [**Who owns the gen**](https://a16z.com/2023/01/19/who-owns-the-generative-ai-platform/)erative AI platform?：我们对生成式人工智能基础设施、模型和应用层面的价值积累和可能积累的「旗舰评估」。

+   翻译：[生成式AI平台，谁主沉浮？](https://waytoagi.feishu.cn/wiki/O1vfw5M9tiJKICky6EpcbFX2nBg)

+   [**Navigating the hig**](https://a16z.com/2023/04/27/navigating-the-high-cost-of-ai-compute/)h cost of AI compute：详细分析为什么生成式人工智能模型需要如此多的计算资源，以及如何在需求旺盛的市场中获取这些资源（即以合适的成本获得合适数量的正确 GPU）的思考方式。

+   翻译：[惊人算力成本背后，AI混战下如何选择基础设施？](https://waytoagi.feishu.cn/wiki/C87pwwOvqiDNz5kQ3WncnGq5nBc)

+   [**Art isn’t dead， it’s ju**](https://a16z.com/2022/11/16/creativity-as-an-app/)st machine-generated：该文章探讨了 AI 模型如何比软件开发等领域更迅速地改变创造领域，这些领域通常被认为是对自动化的最后抵抗。

+   翻译：[艺术并未消亡，它只是由机器生成](https://waytoagi.feishu.cn/wiki/J3EFwHoJxihlRKkq9RKcgpjynth)

+   [**The generative AI**](https://a16z.com/2022/11/17/the-generative-ai-revolution-in-games/) revolution in games：我们的游戏团队对于[如何通过轻松创建高度详细的](https://a16z.com/2023/03/17/the-generative-ai-revolution/)图形，来改变游戏设计师、工作室以及整个市场进行了分析。我们游戏团队的这篇后续文章特别关注了人工智能生成内容与用户生成内容的对比。

+   翻译：[游戏中的生成式 AI 革命](https://waytoagi.feishu.cn/wiki/GxU1wZLfni9MqPkO9pvcDtGWnMf)

+   [**For B2B generative AI**](https://a16z.com/2023/03/30/b2b-generative-ai-synthai/) apps, is less more?：对 LLM 在 B2B 企业应用领域如何发展的预测，重点是总结信息最终比产生文本更有价值。

+   翻译：[对于 B2B 生成式 AI 应用来说，少即是多吗？](https://waytoagi.feishu.cn/wiki/BES8wp0rUi0FkCkMcTQcRWHanBe)

+   [**F**](https://a16z.com/2023/04/19/financial-services-will-embrace-generative-ai-faster-than-you-think/)inancial services will embrace generative AI faster than you think：这篇文章论述了金融服务行业如何利用生成 AI 进行个性化客户体验、更好成本效益的运营、更好的合规性、改进的风险管理，以及动态预测和报告。

+   翻译：[金融服务业将比你想象得更快地接纳生成式 AI](https://waytoagi.feishu.cn/wiki/SNonwgSn1iYLqMkZWOEc8n9Xn5b)

+   [**Generative AI: The ne**](https://a16z.com/2023/02/07/everyday-ai-consumer/)xt consumer platform：这篇文章探讨了在从医疗到电子商务等各个领域，生成 AI 影响消费市场的机会。

+   翻译：[生成式AI平台，谁主沉浮？](https://waytoagi.feishu.cn/wiki/O1vfw5M9tiJKICky6EpcbFX2nBg)

+   [**To make a real difference in healt**](https://time.com/6274752/ai-health-care/)h care， AI will need to learn like we do：AI 有潜力彻底改变预防和治疗疾病的方式。但是，要实现从药物研发到医疗交付方式的真正转变，我们应该投资于创建一个“专业”AI 的生态系统，使其能够像我们最好的医生和药物研发人员今天所做的那样学习。

+   翻译：[为了在医疗保健中产生真正的改变，AI 需要像我们一样学习](https://waytoagi.feishu.cn/wiki/SPDZwWuGDiofsTkPFOPcjQv6nRc)

+   [**The new industrial **](https://a16z.com/2023/05/17/the-new-industrial-revolution-bio-x-ai/)revolution: Bio x AI：人类历史上的下一次工业革命将是人工智能驱动的生物学。

+   翻译：[新工业革命：生物技术×人工智能](https://waytoagi.feishu.cn/wiki/DJB6wiKMOi6Jqqk7IDJc1mCznqc)

### **其他观点**

+   [**On the opportunities and risks **](https://arxiv.org/abs/2108.07258)of foundation models：斯坦福大学对基础模型的概述论文。这篇论文内容丰富，观点鲜明，为“基础模型”这一术语的形成起到了关键作用。

+   State of AI Report：这是一个年度回顾，涵盖了 AI 领域的所有事物，包括技术突破、产业发展、政策/法规、经济影响、安全性以及对未来的预测。

+   [**GP**](https://arxiv.org/abs/2303.10130)Ts is GPTs: An early look at the labour market impact potential of large language models：对大型语言模型（LLMs）在劳动力市场潜在影响的早期研究：OpenAI、OpenResearch 以及宾夕法尼亚大学的研究人员在这篇论文中预测，“在 LLMs 的引入后，大约 80%的美国劳动力可能至少有 10%的工作任务会受到影响，同时约 19%的工作者可能会有至少 50%的任务受到影响。”

+   [**Deep medicine: How artificial inte**](https://www.amazon.com/Deep-Medicine-Eric-Topol-audiobook/dp/B07PJ21V5N/ref=sr_1_1?hvadid=580688888836&hvdev=c&hvlocphy=9031955&hvnetw=g&hvqmt=e&hvrand=13698160037271563598&hvtargid=kwd-646099228782&hydadcr=15524_13517408&keywords=eric+topol+deep+medicine&qid=1684965845&sr=8-1)lligence can make healthcare human again[：Eric T](https://a16z.com/2019/06/13/ai-doctor-deep-medicine-topol/)opol 医生揭示了人工智能如何有可能将医生从耗费大量时间的任务中解放出来，从而不会干扰到人与人之间的连接。这使得医生和病人之间的关系得到恢复。（a16z 播客）

---

# 里程碑式的研究成果

我们今天所见的许多令人惊奇的 AI 产品，都是由大公司和顶级大学的专家进行的令人惊奇的研究成果。最近，我们也看到了个人和开源社区对流行项目进行的卓越工作，例如，通过创建自动化代理或将大模型移植到算力更弱的硬件上运行。

以下是这些论文和项目的集合，供真正想深入研究生成性 AI 的人参考。（对于研究论文和项目，我们还包括了相关的博客文章或网站的链接（如果有的话），这些内容往往以更高的水平做出了解释。我们也包括了原始出版年份，以便您可以追踪基础研究的发展。)

### **大型语言模型**

**新模型**

+   A[tten](https://ai.googleblog.com/2017/08/transformer-novel-neural-network.html)tion is all you need （2017）：这是由 Google Brain 部门发布的，引发了所有转变的原始 Transformer 工作和研究论文。（博客文章）

+   [**BERT: pre-training of deep bidirectional**](https://arxiv.org/abs/1810.04805) tr[ansf](https://ai.googleblog.com/2018/11/open-sourcing-bert-state-of-art-pre.html)ormers for language understanding（2018 年）：这是首批公开可用的 LLM 之一，至今仍有许多变体在使用。（博客文章）

+   [**I**](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf)mproving language understanding by generative pre-train[ing ](https://openai.com/research/language-unsupervised)（2018）：这是 OpenAI 发布的首篇论文，涵盖了 GPT 架构，它已成为 LLM 发展的主要路径。（博客文章）

+   [**Language models a**](https://arxiv.org/abs/2005.14165)re few-shot learners (   2020)   ：这是 OpenAI 的论文，描述了 GPT-3 和现代 LLM 的仅解码器架构。（Decoder-only architecture）

+   [**T**](https://arxiv.org/abs/2203.02155)raining language models to follow instructions with human feedback （2022）：这是 OpenAI 的[论文，解](https://openai.com/research/instruction-following)释了 InstructGPT，它利用了人在循环训练模型，从而更好地遵循提示中的指令。这是使 LLM 能够为消费者（例如，通过 ChatGPT）使用的关键突破之一。（博客文章）

+   [**LaMDA: language models for**](https://arxiv.org/abs/2201.08239) dialog applicatio[ns（2](https://blog.google/technology/ai/lamda/)022 年）：这是 Google 专门设计的模型，用于人类和聊天机器人在各种主题上的自由对话。（博客文章）

+   [**PaLM: Scaling language mo**](https://arxiv.org/abs/2204.02311)deling with pathways（2022 年[）：Go](https://ai.googleblog.com/2022/04/pathways-language-model-palm-scaling-to.html)ogle 的[ PaLM 利用了一种新](https://arxiv.org/abs/2305.10403)系统，可以在数千个芯片上训练 LLM，并且随着模型规模的增大，在某些任务上展示出了超预期的改进。（博客文章）。另请参阅 PaLM-2 技术报告。

+   [**OPT：Open Pre-trained Transfo**](https://arxiv.org/abs/2205.01068)rmer language models （2022）：OPT 是表现最优秀的全开源 LLM 之一。这个拥[有 17](https://ai.facebook.com/blog/democratizing-access-to-large-scale-language-models-with-opt-175b/)50 亿参数的模型的发布附带了代码，并在公开可用的数据集上进行了训练。（博客文章）

+   [**Training compute-optimal l**](https://arxiv.org/abs/2203.15556)arge language models（2022）：Chinchilla 论文。[它提出大](https://www.deepmind.com/blog/an-empirical-analysis-of-compute-optimal-large-language-model-training)多数模型受到数据限制，而不是计算限制，并改变了对 LLM 规模的共识。（博客文章）

+   GPT-4 technical re[port](https://openai.com/research/gpt-4)（2[023 年）：来自 ](https://cdn.openai.com/papers/gpt-4-system-card.pdf)OpenAI 的最新和最伟大的论文，最为人所知的是它揭示的信息之少！（博客文章）。GPT-4 系统卡片揭示了 OpenAI 如何处理幻觉、隐私、安全性和其他问题。。

+   [**L**](https://arxiv.org/abs/2302.13971)LaMA: Open and eff[icie](https://ai.facebook.com/blog/large-language-model-llama-meta-ai/)nt foundation language models （2023）：来自 Meta 的模型（几乎）开始了一个开源 LLM 革命。与许多最好的闭源模型竞争，但只对研究人员开放了有限制的许可。（博客文章）

+   [**A**](https://crfm.stanford.edu/2023/03/13/alpaca.html)lpaca: A strong， replicable instruction-following model（2023 年）：来自斯坦福大学的这种模型展示了指令调整的力量，特别是在较小的开源模型中，相比于纯粹的规模。

模型改进（例如微调、检索、注意力）

+   [**Deep reinforcement learning fr**](https://proceedings.neurips.cc/paper_files/paper/2017/file/d5e2c0adad503c91f91df240d0cd4e49-Paper.pdf)om human preferences （2017）：关于游戏和机器人环境中强化学习的研究，结果证明这是 LLM 的绝佳工具。

+   [**R**](https://arxiv.org/abs/2005.11401)etrieval-a[ugme](https://ai.facebook.com/blog/retrieval-augmented-generation-streamlining-the-creation-of-intelligent-natural-language-processing-models/)nted generation for knowledge-intensive NLP tasks （2020）：由 Facebook 开发，RAG 是通过信息检索提高 LLM 准确性的两个主要研究路径之一。（博客文章）

+   [**I**](https://arxiv.org/abs/2112.04426)mproving language models by [retr](https://www.deepmind.com/blog/improving-language-models-by-retrieving-from-trillions-of-tokens)ieving from trillions of tokens （2021 年）：RETRO，即“检索增强型 TRansfOrmers”，这是另一种由 DeepMind 提出的通过访问训练数据中未包含的信息来提高 LLM 准确性的方法。（博客文章）

+   [**LoRA：Low-rank adaptation of l**](https://arxiv.org/abs/2106.09685)arge language models （2021）：这项来自 Microsoft 的研究为在新数据上训练 LLM 提供了一种比微调更有效的替代方案。它现在已经成为社区微调的标准，特别是对于图像模型。

+   Constitutional AI （2022）：Anthropic 团队介绍了来自 AI 反馈的强化学习（RLAIF）的概念。主要的想法是我们可以在其他 AI 的监督下开发出一个无害的 AI 助手。

+   [**FlashAttention: Fast and memory-eff**](https://arxiv.org/abs/2205.14135)i[cien](https://ai.stanford.edu/blog/longer-sequences-next-leap-ai/)t exact attention with IO-awareness（2022）：这项来自斯坦福的研究为最先进的模型打开了理解更长文本序列（和高分辨率图像）而无需高昂的训练时间和成本的大门。（博客文章）

+   [**H**](https://arxiv.org/abs/2212.14052)u[ngry](https://hazyresearch.stanford.edu/blog/2023-01-20-h3) hungry hippos: Towards language modeling with state space models （2022）：同样来自斯坦福，这篇论文描述了语言建模中注意力的主要替代方案之一。这是一条通向更好的扩展和训练效率的有前途的路径。（博客文章）

### **图像****生成模型**

+   [**L**](https://arxiv.org/abs/2103.00020)earning transferable visual [mode](https://openai.com/research/clip)ls from natural language supervision （2021）：这篇论文介绍了一种基础模型 CLIP ，将文本描述与图像联系起来。这是计算机视觉中首次有效的大规模使用基础模型。（博客文章）

+   Zero-shot[ tex](https://openai.com/research/dall-e)t-to-image generation （2021）：这篇论文介绍了 DALL-E，这是一种将上述的 CLIP 和 GPT-3 结合起来，根据文本提示自动生成图像的模型。它的后继者，DALL-E 2，在 2022 年引发了基于图像的生成式 AI 热潮。（博客文章）

+   [**H**](https://arxiv.org/abs/2112.10752)igh-resolution image synthesis with latent diffusion models （2021）：描述稳定扩散的论文（在发布和爆炸性开源增长之后）。

+   [**Photorealistic text-to-image diffusion**](https://arxiv.org/abs/2205.11487) models with deep language [un](https://imagen.research.google/)derstanding（2022 年）：Imagen 是 Google 进入 AI 图像生成领域的尝试。尽管在宣布后的一年多时间里，该模型截止到本文发布日期仍未公开发布。（网站）

+   [**DreamBooth：Fine tuning text-to-image diffus**](https://arxiv.org/abs/2208.12242)ion models for subject-driven generation （2022[）：](https://dreambooth.github.io/)DreamBooth 是 Google 开发的一种系统，用于训练模型识别用户提交的主题，并将其应用到提示的上下文中（例如 [用户] 在艾菲尔铁塔下微笑）。（网站）

+   [**A**](https://arxiv.org/abs/2302.05543)dding conditional control to text-to-image diffusion models （2023）：这篇来自斯坦福的论文介绍了 ControlNet，这现在是一种非常流行的工具，用于对使用潜在扩散模型的图像生成进行细粒度控制。

### Agents（智能体代理）

+   [**A path to autonomous **](https://openreview.net/pdf?id=BZ5a1r-kVsf)machine intelligence （2022）：Meta AI 领导者和纽约大学教授 Yann LeCun 提出的关于如何构建真正理解周围世界的自主智能代理的建议。

+   [**R**](https://arxiv.org/abs/2210.03629)eAct：Synergizing rea[soni](https://ai.googleblog.com/2022/11/react-synergizing-reasoning-and-acting.html)ng and acting in language models （2022）：普林斯顿大学和 Google 的一个项目，用来测试和提高 LLM（大型语言模型）的推理和规划能力。（博客文章）

+   [**G**](https://arxiv.org/abs/2304.03442)enerative agents: Interactive simulacra of human behavior （2023）：斯坦福大学和 Google 的研究人员使用 LLM 驱动代理，在类似于“The Sims”（模拟人生）这样的环境中，其互动是自发的，而不是由编程驱动的。

+   [**R**](https://arxiv.org/abs/2303.11366)eflexion: an autonomous agent with dynamic memory and self-reflection （2023）：来自东北大学和 MIT 的研究人员的工作，他们通过从错误和过去的经验中学习，教导 LLM 更可靠地解决问题。

+   [**T**](https://arxiv.org/abs/2302.04761)oolformer：Language models can teach themselves to use tools （2023）：这个来自 Meta 的项目训练 LLM 使用外部工具（在这种情况下，API 指向搜索引擎和计算器等东西），以提高准确性，而不增加模型大小。

+   [**Auto-GPT: An autonom**](https://github.com/Significant-Gravitas/Auto-GPT)ous GPT-4 experiment : 一个开源实验项目，通过给 GPT-4 提供一组工具（互联网访问、文件存储等）并选择使用哪些工具来解决特定任务，以扩大 GPT-4 的能力。

+   [**BabyAGI**](https://github.com/yoheinakajima/babyagi)：这个 Python 脚本使用 GPT-4 和向量数据库（用来存储上下文），以便计划并执行一系列解决更广泛目标的任务。

### **其他数据模态**

**代码生成**

+   [**Evaluating large language mo**](https://arxiv.org/abs/2107.03374)dels trained on code （2021）：这[是 Op](https://openai.com/blog/openai-codex)enAI 关于 Codex 的研究论文，Codex 是 GitHub Copilot 产品背后的代码生成模型。（博客文章）

+   [**Competition-level code gener**](https://www.science.org/stoken/author-tokens/ST-905/full)ation with AlphaCode（2021 年）：这项[来自 D](https://www.deepmind.com/blog/competitive-programming-with-alphacode)eepMind 的研究展示了一种模型，能够比人类程序员编写更好的代码。（博客文章）

+   [**CodeGen: An open large language model fo**](https://arxiv.org/abs/2203.13474)r code with multi-tur[n pr](https://blog.salesforceairesearch.com/codegen/)ogram synthesis（2022 年）：CodeGen 来自 Salesforce 的 AI 研究部门，目前支持 Replit Ghostwriter 的代码生成产品。（博客文章）

**视频生成**

+   [**M**](https://arxiv.org/abs/2209.14792)ake-A-Video: Text-to-video generation witho[ut t](https://makeavideo.studio/)ext-video data （2022）：来自 Meta 的一个模型，可以根据文本提示创建短视频，也可以给静态照片输入添加动作，或者创建现有视频的变体。（博客文章）

+   [**I**](https://arxiv.org/abs/2210.02303)m[ag](https://imagen.research.google/video/)en Video: High definition video generation with diffusion models（2022 年）：顾名思义：谷歌基于图像的 Imagen 模型的一个版本，专门用于根据文本提示生成短视频。（网站）

**人类生物学和医学数据**

+   [**Strategies for pre-training g**](https://arxiv.org/pdf/1905.12265.pdf)r[aph ](https://snap.stanford.edu/gnn-pretrain/)neural networks （2020）：这篇出版物为有效的预训练方法奠定了基础，这些方法对于药物发现的各种应用都很有用，比如分子性质预测和蛋白质功能预测。（博客文章）

+   [**Improved protein structure predic**](https://www.nature.com/articles/s41586-019-1923-7)tion using potentials from deep learning（2020 年）[：Dee](https://www.deepmind.com/blog/alphafold-a-solution-to-a-50-year-old-grand-challenge-in-biology)pM[ind](https://www.blopig.com/blog/2021/07/alphafold-2-is-here-whats-behind-the-structure-prediction-miracle/) 的以蛋白质为中心的 Transformer 模型 AlphaFold，使得能够从序列预测蛋白质结构——这是一个真正的突破，已经对理解生物过程和开发新的疾病治疗方法产生了深远影响。（博客文章）（解释器）

+   [**Large language models encod**](https://arxiv.org/abs/2212.13138)e clinical knowledge（2022）：Med-PaLM 是一个能够正确回答美国医疗[执照考试风格问题的](https://www.medrxiv.org/content/10.1101/2022.12.19.22283643v2) [LLM。该团队](https://arxiv.org/abs/2303.13375)已经公布了 Med-[Pa](https://www.youtube.com/watch?v=saWEFDRuNJc)LM2 的表现结果，其得分与“专家”考试者相当。其他团队已经用 ChatGPT 和 GPT-4 进行了类似的实验。（视频）

**音频生成**

+   [**Jukebox: A genera**](https://arxiv.org/abs/2005.00341)tive model for music（2020 年）：OpenAI 使用 tra[nsfo](https://openai.com/research/jukebox)rmer 进行音乐生成的尝试，能够在最小的训练下生成音乐、声音和歌词。（博客文章）

+   [**A**](https://arxiv.org/pdf/2209.03143.pdf)u[dioL](https://ai.googleblog.com/2022/10/audiolm-language-modeling-approach-to.html)M: a language modeling approach to audio generation （2022）：AudioLM 是 Google 的一个项目，用于生成多种类型的音频，包括语音和乐器演奏。（博客文章）

+   MusicLM: Genera[ting](https://google-research.github.io/seanet/musiclm/examples/) nusic from text （2023）：当前基于 AI 的音乐生成的最新技术，展示出比以前尝试更高的质量和连贯性。（博客文章）

**多维图像生成**

+   [**N**](https://arxiv.org/abs/2003.08934)eRF：Representing scenes as neur[al](https://www.matthewtancik.com/nerf) radiance fields for view synthesis （2020）：来自以加州大学伯克利分校为主的团队的研究，使用 5D 坐标“合成复杂场景的新视图”。（网站）

+   [**DreamFusion: Text-to-3**](https://arxiv.org/pdf/2209.14988.pdf)D[ u](https://dreamfusion3d.github.io/)sing 2D diffusion （2022）：来自 Google 和加州大学伯克利分校的研究人员的工作，基于 NeRF 从 2D 输入生成 3D 图像。（网站）

特别感谢[ Jack Soslow](https://a16z.com/author/jack-soslow/)、[Jay Rughani](https://a16z.com/author/jay-rughani/)、[Marco Mascorro](https://a16z.com/author/marco-mascorro/)、[Martin Casado](https://a16z.com/author/martin-casado/)、R[ajko Radovano](https://a16z.com/author/vijay-pande/)vic 和 Vijay Pande 对本文的贡献，也感谢整个 a16z 团队总是能对 AI 的最新进展进行积极的讨论。还要感谢 Sonal Chokshi 和加密货币团队在公司建立了一系列经典内容。

---