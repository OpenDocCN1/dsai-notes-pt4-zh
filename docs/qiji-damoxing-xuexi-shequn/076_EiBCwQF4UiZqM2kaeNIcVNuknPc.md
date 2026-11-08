# 奇绩大模型日报（10月 21日）

> 来源：[https://miracleplus.feishu.cn/docx/UMQ6dn8Puop0QVxT6s9cHnOBn54](https://miracleplus.feishu.cn/docx/UMQ6dn8Puop0QVxT6s9cHnOBn54)

# 🔉潜空间活动报名

> 本期活动将在**11月9日** **10:00**开始，我们邀请到的嘉宾是**鱼哲**，Lepton AI 创始成员，曾在阿里云担任高性能 AI 平台产品负责人，专注于 AI 在多个行业的落地及应用。Lepton AI 致力于建立高效可用的AI 基础设施，让团队更关注于应用构建及落地。在本次分享中鱼哲将带来关于AI产品相关的思考，分享主题《Beyond Infra, what matters?》。

> 除嘉宾分享外，每期设置了【匹配合伙人 Cofounder Matching】环节。你可以和 GenAI 时代最有活力的创业者和研究者线下面对面交流，将有机会找到志同道合、有共同创业梦想的小伙伴。

> 报名通道已开启，欢迎扫描下方二维码报名。

![](img/050cb8a3e46288e91b6f46afeed7b100.png)

#

# 资讯

### 清华开源混合精度推理系统MixQ

https://mp.weixin.qq.com/s/Y1eB2h86KYpU7SsxxoK4HQ

1.  **量化权重与激活**：MixQ同时量化权重和激活，使用INT8/INT4张量核心进行推理加速，并通过FP16张量核心处理少量激活中的离群值，既保持精度又提升吞吐量。其混合精度量化策略使推理精度几乎无损，精度下降不到0.1%。

1.  **等价变换优化计算图**：MixQ基于离群点的局部性，优化了混合精度推理的计算图，避免了重复检查离群点的开销。通过量化系数中的amax值判断矩阵中的离群点，从而降低计算开销。

1.  高效数据结构与CUTLASS优化：MixQ设计了新型数据结构，通过将离群点“拼接”成矩阵，减少访存开销。同时，使用CUTLASS 3.x编写高性能算子，将低精度计算结果反量化并与高精度结果相加，进一步提升性能。

![](img/25bbcdeefa5feb5be1e96b2fb0737d95.png)

### 智源发布原生多模态世界模型Emu3 实现图像、文本、视频大一统

https://mp.weixin.qq.com/s/b4emisljL0bXvBfSyDcAbw

**视觉tokenizer**：Emu3通过一个强大的视觉tokenizer将图像和视频转化为离散的token，能够与文本token统一处理。这个视觉tokenizer基于MoVQGAN架构，在时间和空间维度上实现了显著的压缩，同时通过三维卷积层增强了视频的处理能力。

**网络架构**：Emu3保留了Llama-2的大语言模型架构，但扩展了嵌入层以容纳视觉token，并采用了如RMSNorm、GQA注意力机制、SwiGLU激活函数等技术。同时，模型通过改进的嵌入层处理多模态输入，使用QwenTokenizer进行多语言文本编码，消除了部分传统偏置设计。

**多模态数据处理**：Emu3对图像和视频进行了密集的标注和过滤处理，创建了一个大规模的多模态数据集。其预训练分为两个阶段，分别处理图像和视频数据，通过极长的上下文长度（最高达131,072个token）实现了复杂视频数据的建模。

**直接偏好优化(DPO)**：在视觉生成任务中，Emu3使用DPO技术，使模型生成的内容更符合人类偏好，这种方法无缝应用于自回归的视觉生成，确保生成结果更自然和贴近人类需求。

![](img/f44743c7d950ad4ea1553d56a74ce256.png)

### Bengio团队新论文！强化学习新策略：不要做我可能不会做的事情

https://mp.weixin.qq.com/s/oKz3QbqKPZCgVbcTkBKbEQ

在强化学习中，当智能体的奖励机制与设计者的意图不一致时，可能会导致不理想的行为。为防止这种情况，通常采用KL正则化技术，通过限制智能体的行为来减少不符合设计者意图的行为。然而，这种方法并不能完全避免意料之外的行为，特别是在基于贝叶斯预测模型进行模仿学习的情形下。

KL正则化通过衡量提议策略与基础策略的相似性来限制智能体的行为，但它有局限性。研究表明，当基础策略是可信策略的贝叶斯模仿时，KL约束的效果会随着训练数据的增加而逐渐减弱，这意味着智能体在新环境下仍然可能表现出异常行为。核心问题在于，贝叶斯模仿在处理不确定性时，无法有效区分一些看似合理但实际上不应采取的行动。

为解决此问题，研究者提出了新的理论方案，建议将指导原则从“不要做我不会做的事”改为“不要做我可能不会做的事”，以增强智能体在新环境中的谨慎性。此外，研究人员提出了一种基于贝叶斯预测和模仿的替代方案，允许智能体在不确定时寻求帮助，并通过实验证明这种方法可以提高智能体的可靠性。

实验结果表明，即使奖励机制不完美，智能体仍能找到简单有效的策略来最大化奖励，这表明我们需要更慎重地设计奖励系统。KL正则化在限制智能体行为方面的作用有限，尤其是在复杂的对话任务中，较小的KL预算虽然能使智能体的行为更复杂，但仍无法完全避免简单化和不理想的行为。这提醒我们在微调语言模型时，应关注整体的KL散度控制。

![](img/9bb55f4784459fc27b76a3b61bb8b7c1.png)

### 美国AI博士为何高产？欧洲博士的质疑与反思

https://mp.weixin.qq.com/s/HFMmkBOtN2qu5IpsVFAcIg

在机器学习社区中，一位欧洲博士的质疑引发了广泛讨论。他正在攻读AI/机器学习方向的博士学位，学业进展看似顺利，但当他看到美国博士生们人均10篇AI顶会论文、5篇一作时感到震惊，提出了疑问：这些博士生是如何如此高效地产出如此多的高质量研究成果？

这位博士分析了差异背后的几个主要原因：

1.  **高强度工作文化**：美国顶尖学术机构的博士生通常一周工作超过60小时，全年无休，形成了高强度的“内卷”文化。激烈的竞争环境推动博士生不断加大工作强度，以保持领先。

1.  **资源优势**：美国顶尖实验室拥有大量的GPU等计算资源，极大地加速了研究进度。资源的巨大差异使得顶尖博士生能够快速完成高质量的研究。

1.  **顶尖人才聚集**：美国的顶尖项目吸引了来自全球的优秀学生，他们在高压环境中协同工作，进一步推动了学术产出。

1.  **机构与名人效应**：知名大学和实验室的资源与声望让这些博士生的论文更容易受到学术界的关注，即便质量未必最优，顶尖机构的标识也为其提供了隐形优势。

1.  **幸存者偏差**：讨论中也有人指出，这类现象可能是一种幸存者偏差。美国顶尖博士的高产表现并不代表整体情况，许多博士生面临同样的科研压力却未能取得如此亮眼的成绩。

![](img/c712e73fdbf35fbf24a8e50808d78b15.png)

# 推特

### Arxiver开源：包含 138,830 篇 arXiv 论文的多Markdown格式

贡献人：

https://x.com/alicanacar04/status/1847979788801052719

我们正在开源 Arxiver：包含 138,830 篇 arXiv 论文的多Markdown格式！📚

你可以通过 @huggingface 的数据集库使用 Arxiver 进行语义搜索、语言建模、问答、摘要等任务。

![](img/26fcd00b4ff1860ad260d4ab3e1d6cfb.png)

### Sully谈自动代理：一次性代理任务是最没有想象力的用例之一

贡献人：

https://x.com/SullyOmarr/status/1848126562169999517

Sully：我已经构建智能代理将近1.5年了，可以自信地说99%的“AI浏览”演示是无用的。

现实是，消费者不会有数百万个AI代理24/7为他们工作，讨价还价、购物来省下20美元。最终只会有一个AI购物应用。

计算资源不是免费的，服务器是要花钱的。

目前，代理的真正价值在于自动化那些无聊但重复的工作，这些工作对公司来说花费巨大。

例如：

• 阅读网站、复制产品名称等并将它们存储在表格中（这样做1万次）。

• 阅读1000份PDF文件以查找特定药物或专利的提及。

• 研究1000家公司。

• 客户支持。

随着它们的进步，代理将以“一对多”的方式解决更多复杂的任务（如代理群体发现新技术、编程等）。

一次性代理任务是最没有想象力的用例之一。

我更愿意让代理自动化一个2分钟的任务1万次，而不是自动化一次15分钟的任务。

---

Logan Kilpatrick：火热观点🔥：我认为消费者对让AI代理为他们完成购物/浏览任务的兴趣被大大夸大了。

人们通常希望代理能够处理那些无聊的部分，但很多此类任务往往风险较高…

![](img/5a96629b49bf9f41c3f742d2badc812c.png)

![](img/0b7e61271a1febd52f9123a684fae7bf.png)

### Tom Yeh分享手写Transfermer之Karpathy llm.c与矩阵乘法相连接

贡献人：

https://x.com/ProfTomYeh/status/1847673742106775668

我制作了这个动画来手动✍️将 @karpathy 的 llm.c 与矩阵乘法相连接。

![](img/ae2eb8f68128fbba3c7f7d1f4a926430.png)

[ssstwitter.com_1729495888257.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SV8hbMel7okXvAxuxVCccudBnjg)

### IBM 和 Ollama 宣布合作推出 Granite 3.0 模型

贡献人：

https://x.com/ollama/status/1848223852465213703

今天，IBM 和 Ollama 宣布合作推出 Granite 3.0 模型！😍

两种架构：

**Dense:**

• 2B：ollama run granite3-dense

• 8B：ollama run granite3-dense:8b

Mixture of Experts:

• 1B：ollama run granite3-moe

• 3B：ollama run granite3-moe:3b

博客文章：

https://ollama.com/blog/ibm-granite

![](img/f96cee538da4b2168cb7cbba0f19e196.png)

# 信号

### SANA: Efficient High-Resolution Image Synthesis with Linear Diffusion Transformers

贡献人：

原文链接：https://arxiv.org/abs/2410.10629

推文链接：[h](https://x.com/JulienBlanchon/status/1846318852637949964)ttps://x.com/JulienBlanchon/status/1846318852637949964

信号节点：应用体系-多模态模型

这篇文章介绍了Sana，一个高效的文本到图像生成框架，能够生成高达4096×4096分辨率的高质量图像。Sana的关键设计包括：(1)  深度压缩自编码器，可以将图像压缩32倍；(2) 线性DiT（Diffusion  Transformer），使用线性注意力替代传统注意力机制，提高高分辨率下的效率而不牺牲质量；(3)  仅解码器的文本编码器，用现代小型解码器模型替换T5，并通过上下文学习增强图像-文本对齐；(4)  高效训练和采样技术，采用Flow-DPM-Solver减少采样步骤，并加速收敛。结果表明，Sana-0.6B在性能上与大型扩散模型（如Flux-12B）相当，但体积小20倍，速度快100多倍。此外，Sana-0.6B可以在16GB笔记本GPU上运行，生成1024×1024分辨率图像的时间不到1秒。代码和模型将公开发布。

![](img/46d2e4f2868e38b838f5d453b52d9c32.png)

### Fine-Tuning Discrete Diffusion Models via Reward Optimization with Applications to DNA and Protein Design

贡献人：

原文链接：https://arxiv.org/html/2410.13643v1

推文链接：[https://x.com/svlevine/status](https://x.com/svlevine/status/1847837069324013977)/1847837069324013977

信号节点：应用体系- AI4Science

这篇文章介绍了一种新的算法DRAKES，它通过在离散扩散模型中结合强化学习（RL）来优化特定任务目标。该算法允许直接通过扩散模型生成的整个轨迹反向传播奖励，并使用Gumbel-Softmax技巧使原本不可微的轨迹可微。文章的理论分析表明，这种方法可以生成既类似于自然序列（即在预训练模型下有很高概率）又能得到高奖励的序列。文章还展示了该算法在生成优化增强子活性的DNA序列和优化蛋白质稳定性的蛋白质序列方面的有效性，这些是基因疗法和基于蛋白质的治疗中的重要任务。

![](img/2cba063f54aba0b3da694de5bf7551bf.png)

### Fluid: Scaling Autoregressive Text-to-image Generative Models with Continuous Tokens

贡献人：

原文链接：https://arxiv.org/abs/2410.13863

推文链接：

信号节点：模型体系- 跨模态架构

这篇文章探讨了在文本到图像生成任务中，自回归模型的扩展问题，特别是在视觉领域。研究关注了两个关键因素：模型是使用离散令牌还是连续令牌，以及令牌是按随机顺序还是固定光栅顺序生成。实验结果表明，使用连续令牌的模型在视觉质量上显著优于使用离散令牌的模型。此外，随机顺序模型在GenEval评分上明显优于光栅顺序模型。基于这些发现，研究者训练了一个名为Fluid的随机顺序自回归模型，该模型在MS-COCO  30K上的零样本FID达到了6.16的新最佳状态，并在GenEval基准测试中获得了0.69的总体得分。

![](img/7f58c034d8eb1ecc5c2ef78a630ec41d.png)

### Movie Gen: A Cast of Media Foundation Models

贡献人：

原文链接：https://arxiv.org/abs/2410.13720

推文链接：Meta

信号节点：应用体系-多模态模型

这篇文章介绍了一个名为Movie  Gen的新型基础模型系列，该模型能够生成高质量的1080p高清视频，具有不同的宽高比和同步音频。此外，还展示了基于精确指令的视频编辑和根据用户图像生成个性化视频的附加功能。这些模型在文本到视频合成、视频个性化、视频编辑、视频到音频生成和文本到音频生成等多个任务上都取得了新的突破。文章还介绍了在架构、潜在空间、训练目标、数据管理、评估协议、并行化技术和推理优化等方面的多项技术创新和简化，这些创新使得在预训练数据、模型规模和训练计算方面取得了规模化的优势。

![](img/31bb8dce7066432b3428507a5a1dc01b.png)

### MixEval-X: Any-to-Any Evaluations from Real-World Data Mixtures

贡献人：

原文链接：[https://huggingface.](https://huggingface.co/papers/2410.13754)co/papers/2410.13754

推文链接：https://arxiv.org/abs/2410.13754

信号节点：模型体系-评估体系

这篇文章介绍了MixEval-X，这是一个全新的多模态基准测试，旨在优化和标准化跨不同输入和输出模态的评估。文章指出了当前评估中存在的两个主要问题：不一致的标准和显著的查询、评分及泛化偏差。MixEval-X通过多模态基准混合和适应-校正流程来重建现实任务分布，确保评估能够有效泛化到现实用例。广泛的元评估表明，该方法能有效地将基准样本与现实任务分布对齐，并且其模型排名与众包现实评估高度相关，同时效率更高。

![](img/d94eac9bb6e621d0e19df84565273a35.png)

# 产品

### AI Desk

贡献人：

https://aidesk.pro/

AI Desk 是一款由人工智能驱动的客户服务助手，可以在自动化支持流程并提升销售效率。它可以提供全天候的客户服务，通过训练 AI 使用业务数据，将会使成本降低多达 90%。AI Desk 能够识别潜在客户，自动回复消息，并支持多语言聊天，确保没有查询被遗漏。

![](img/b080982f63a1fee50e8b87693a35df36.png)

### BrowserCopilot AI

贡献人：

https://www.browsercopilot.ai/

BrowserCopilot AI 是一款 AI 浏览器助手,可以在任何网页上提供帮助,包括阅读、写作、解释、总结等功能。它能够理解用户在浏览器中的工作环境并提供相应的支持。据介绍,使用这款软件可以让用户每天工作提高 2-3 小时的效率。

![](img/c4c9ca52a6fab74a1de5beb7f1bc8df1.png)

# HuggingFace&Github

### LazyLLM

贡献人：

https://github.com/LazyAGI/LazyLLM?tab=readme-ov-file

LazyLLM是一款低代码构建**多Agent**大模型应用的开发工具，协助开发者用极低的成本构建复杂的AI应用，并可以持续的迭代优化效果。LazyLLM提供了便捷的搭建应用的workflow，并且为应用开发过程中的各个环节提供了大量的标准流程和工具。

基于LazyLLM的AI应用构建流程是原型搭建 -> 数据回流 -> 迭代优化，可以先基于LazyLLM快速跑通应用的原型，再结合场景任务数据进行bad-case分析，然后对应用中的关键环节进行算法迭代和模型微调，进而逐步提升整个应用的效果

![](img/3f115bec30623b292ab14b300ac23ba0.png)

![](img/ef00f2ff3c4843ef75d36421a78bd178.png)

# 投融资

### 2024年第三季度生成式AI初创公司投资超过39亿美元

https://techcrunch.com/2024/10/20/investments-in-generative-ai-startups-topped-3-9b-in-q3-2024/

尽管生成式AI的投资回报率存在争议，但许多投资者仍在积极下注。根据PitchBook的数据显示，2024年第三季度，风险投资公司（VC）在206笔交易中向生成式AI初创公司投资了39亿美元（不包括OpenAI的66亿美元融资）。其中，美国公司获得了29亿美元，涉及127笔交易。大额融资的公司包括：Magic（3.2亿美元），Glean（2.6亿美元），Hebbia（1.3亿美元），中国的Moonshot AI（3亿美元），以及日本的Sakana AI（2.14亿美元）。

生成式AI涵盖了从文本和图像生成器到编码助手和网络安全自动化工具的广泛技术领域。然而，技术可靠性和基于未授权数据训练模型的合法性引发了一些专家的质疑。但风险投资家认为，生成式AI将深入大规模、盈利丰厚的行业，并将在长尾市场获得稳固增长。

尽管生成式AI的计算需求巨大，可能带来能源消耗问题，但市场热情依旧高涨。包括微软、亚马逊、谷歌和甲骨文在内的大型数据中心运营商，正在投资核能来应对其非可再生能源消耗的增加，尽管这些投资的成效可能需要数年才能显现。

未来，生成式AI初创公司的投资热潮仍未显示出减缓迹象。

### Perplexity计划以80亿美元估值筹集5亿美元资金

https://techcrunch.com/2024/10/20/perplexity-is-reportedly-looking-to-fundraise-at-an-8b-valuation/

据《华尔街日报》报道，AI搜索引擎公司Perplexity正在进行融资谈判，计划以80亿美元的估值筹集约5亿美元。如果此轮融资顺利完成，Perplexity的估值将比今年夏天从软银获得融资时的30亿美元翻倍。目前，Perplexity每天处理约1500万次搜索请求，年收入约为5000万美元。

Perplexity通过AI技术为用户提供类似聊天机器人的搜索体验。然而，该公司因未经授权抓取网页内容而受到部分新闻出版商的指控，《纽约时报》甚至向其发出停止侵权函。对此，CEO Aravind Srinivas表示，他希望与出版商合作，无意与任何人对立。

此次融资谈判紧随OpenAI宣布完成66亿美元融资、估值达到1570亿美元之后。随着OpenAI推出更接近搜索引擎功能的产品SearchGPT，AI搜索领域竞争加剧。

目前，Perplexity的发言人拒绝对该报道发表评论。

![](img/828fa85ce9e594b706c4d4c10de8707c.png)

# 学习

### 基于GPU的树状结构构建

贡献人：

https://zhuanlan.zhihu.com/p/669019977

本文探讨了如何使用包围体层次结构（BVH）来进行高效的碰撞检测，重点介绍了如何优化这些结构以适应大规模并行处理器（如GPU）。在展示了减少BVH遍历过程中的分歧（divergence）对性能的重要性后，文章转向讨论如何并行构建BVH树。

文章介绍了线性BVH（LBVH）方法，该方法通过使用Z-Order曲线（莫顿码）对对象进行排序，以保持空间邻近性，从而简化树结构的构建。在动态场景中，由于对象的移动，每个时间步都需要重新构建BVH树，这对并行化提出了挑战。

技术上，文中强调了"占用率"（occupancy）的优化，即在任意时刻GPU中活跃线程的比例。传统的自上而下BVH构建方法在树的高层级时占用率低，导致性能下降。相对而言，Tero Karras提出的全并行BVH生成算法通过消除节点之间的依赖关系，使得每个节点的覆盖范围能够直接计算，避免了父子节点间的顺序依赖问题，从而显著提升了性能，使得构建时间相比自上而下方法缩短了50倍。

该算法依赖于高效的位运算，包括莫顿码的生成和二分查找来划分对象范围。尽管每个节点的计算量增加，但由于其完全并行的特性，GPU利用率大幅提高，即便在小规模数据集下也表现出色。文章最后指出，并行编程与传统的串行编程有根本性区别，需要重新思考算法设计，以最大化并行计算的潜力。

![](img/fa5ee3c9640e63cedb007ed7339beb17.png)

### LLM开源模型：Baichuan-Omni/Pixtral 12B/Qwen2.5-Series

https://zhuanlan.zhihu.com/p/894987175

**Baichuan-Omni** 是第一个开源的 7B 多模态模型，能够处理图像、视频、音频和文本模态，提供强大的多模态交互能力。模型的预训练数据包括图像字幕、视频字幕、音频-文本对以及跨模态数据。其架构中，视觉编码器采用 Siglip-384px，视频处理则通过采样和卷积优化视频帧序列，音频编码器使用 Whisper-large-v3，并引入卷积门控 MLP 进行音频信息的保留。模型在多模态对齐时，通过大规模跨模态数据集进行微调，最终在多模态理解任务中表现出色。

**Pixtral 12B** 构建在 Mistral Nemo 12B 的基础上，使用了 RoPE-2D 相对位置编码以支持变分辨率的图像处理，显著提升了多模态任务的性能。模型视觉编码器是从头训练的 Pixtral-ViT，支持不同纵横比的图像处理，尤其在细粒度推理和多模态推理任务中表现突出。通过动态分辨率策略和块对角掩码，Pixtral 12B 实现了在延迟受限环境中的高效推理。

**Qwen2.5-Coder** 是一个专注于代码生成的模型，涵盖了 92 种编程语言。其预训练数据集通过分层过滤技术确保了数据质量的提升，并通过扩展上下文长度的 RoPE 外推策略，提升了长代码片段的处理能力。Qwen2.5-Coder 结合了多语言协作框架，生成了大规模高质量指令数据集，并通过监督微调和拒绝采样策略优化了模型的指令跟随和代码生成能力。

Arctic-SnowCoder 作为一个专注于高质量数据代码预训练的模型，通过预训练和高质量数据过滤策略，确保了代码生成任务中的卓越表现。模型的核心在于其基于 BERT 的代码质量注释器，通过 ROC-AUC 分数评估数据质量，确保了代码数据的高可执行性和可靠性。

![](img/baa7631b5ed2d11a2ec6a1e0fbcae79e.png)

### 浅析字节事件的Checkpoint安全问题

https://zhuanlan.zhihu.com/p/2148291162

**漏洞来源**：PyTorch 采用了 Python 原生的 pickle 模块进行模型序列化，导致反序列化时可以执行任意代码。这并非 Huggingface 独有的问题，而是 Python pickle 模块的长期问题。

**漏洞原理**：攻击者可以通过模型的序列化文件植入恶意代码。反序列化时，由于 pickle 模块的特性，恶意代码在与执行程序同权限下运行。

**关键技术分析**：

+   **Pickle 注入**：通过修改序列化数据，恶意代码会在反序列化时执行。例如攻击者可以通过重载 Python 对象的 reduce 方法注入系统调用。

+   **Monkey Patching**：动态替换函数或模块，例如在运行时劫持 PyTorch 的 all_reduce 方法，导致梯度操作被篡改。

**解决方案**：

+   **安全序列化**：使用 Huggingface 提供的 safetensors 或 Google 的 Protobuf 作为替代，可以有效防止代码注入。

+   **文件校验**：通过检查 ckpt 文件的哈希值检测文件篡改。

+   **重载 find_class**：在 pickle 反序列化时通过重载 Unpickler.find_class 限制可反序列化的类型。

+   Monkey Patching 检测：利用 Python 的 sys.settrace() 跟踪代码调用，检测可能的运行时修改。

![](img/0b711accaae2dc0d78b33f6f4837e55c.png)

### LLM在Reranker任务上的最佳实践？A simple experiment report（with code）

https://zhuanlan.zhihu.com/p/987727357

在BERT时代，Reranker任务通常通过encoder-only的BERT，将query与文档拼接输入，并通过CLS向量及MLP得出评分。然而，随着大语言模型（LLM）的发展，研究者开始探索使用decoder-only的LLM作为Reranker任务的基座。

本实验报告探索了两种使用LLM做Reranker的方法并与BERT类模型对比：

1.  **生成方法**：将Reranker任务建模为生成任务，让模型预测query与doc的相关性（是/否）。该方法充分利用LLM预训练和监督微调（SFT）的能力，采用交叉熵loss进行训练。

1.  **判别方法**：类似于BERT，使用LLM的last_hidden_state通过MLP层得到评分，将任务视为二分类问题，使用BCEWithLogitsLoss训练。

实验旨在回答以下问题：

+   LLM是否在Reranker任务上优于BERT？

+   生成方法与判别方法的效果对比？

+   是否可以将LLM的信号蒸馏到BERT中？

**实验结果**：

+   在小规模数据下，LLM（尤其是生成方法）表现优于BERT，生成方法的MAP比BERT高0.62，比判别方法高0.30。

+   难负例较少时，LLM仍优于BERT，生成方法的MAP比BERT高1.61。

+   在数据量充足和难负例丰富的情况下，LLM和BERT的差距缩小，但生成方法依然略优。

**蒸馏结果**： 将生成方法的LLM蒸馏到BERT中，在数据较少的情况下，蒸馏后的BERT模型表现接近LLM，MAP提升1.45。

**总结**：

+   LLM在数据少或难负例少的情况下优于BERT。

+   生成方法略优于判别方法。

+   蒸馏技术可在兼顾效果和效率的前提下，将LLM的信号传递给BERT。

**最佳实践**：

+   数据充足时，BERT是较好的选择，具有高效推理性能。

+   数据较少或需多语言支持时，优先选择LLM生成方法，并可通过蒸馏优化推理效率。

实验代码可在[RAG-Retrieval仓库](https://github.com/NLPJCL/RAG-Retrieval)中获取。

![](img/43a96afda7bc7e82b46df23a5145c80d.png)

### RecSys 2024参会见闻

https://zhuanlan.zhihu.com/p/1002795145

**大模型无法完全替代推荐模型**：DeepMind分析了大模型的三大限制：

+   缺乏领域内急剧变化的物品知识

+   不具备特定领域用户的协同信息

+   无法达到工业级推荐系统的实时响应需求

**推荐系统的多样性提升**：YouTube和Spotify都利用大模型进行多样性推荐。大模型通过生成式检索（将物品转为向量、压缩为ID，再生成推荐）来探索用户的新兴趣，并通过生成宽泛查询帮助发现新内容。

**生成式检索技术**：此技术已应用在YouTube等平台，利用大模型生成推荐物品的ID，通过向量化物品数据，提升推荐精准度。

**多模态推荐的潜力**：Gemini等多模态大模型的出现，预示推荐系统未来可能不仅限于信息筛选，还能提供更智能的决策支持，如根据用户的体型或肤色推荐合适的服装。

**推荐解释的改进**：Indeed展示了大模型生成的个性化推荐解释能够提升用户参与度，尤其是在求职领域。相比于传统模板生成的推荐，大模型的语言生成能力带来更具说服力的解释。

**LLMs在推荐解释评估中的作用**：清华大学研究表明，传统文本相似度指标无法有效评估推荐解释，而大模型能够提供更一致的评价，与用户反馈相吻合。

**重复购买和跨域推荐**：大模型能通过行为模式迁移将知识从一个领域迁移到另一个领域，提升用户对新物品的尝试意愿。此外，生成系统还可以处理重复购买场景，探索提高不同物品曝光度以鼓励新尝试。

### 盘点SCARA结构与竞争格局

https://zhuanlan.zhihu.com/p/2077276588

**EPSON产品线**：

+   **LS系列**：性能和性价比兼具，是市场主力。其设计优化了伺服电机与谐波减速机的直连方式，提升了小臂的惯量表现，适用于3C等精密制造领域。

+   **G系列**：高端产品线，采用中空走线设计，适用于洁净环境和电子制造领域，支持更高负载且通过了UL认证。

+   **T系列**：低成本阉割版，为应对中国市场竞争，集成控制柜进本体，创新点在于紧凑设计，但性能有所下降，主要针对简单需求场景。

+   **GX系列**：超高端系列，对标Staubli，支持更高负载及复杂环境应用。

+   **RS系列**：专为大客户定制，采用倒装设计，支持360度旋转。

**YAMAHA的技术创新**：

+   **无同步带设计**：YG系列避免了同步带维护，提升了精度和免维护性。

+   **谐波减速机应用**：提高了A4轴的旋转惯量承受能力。

+   **自动标定系统**：无需电池的设计避免了丢零点问题，自动标定技术大幅提升了用户体验。

**国产SCARA格局**：

+   **汇川、埃斯顿、新时达**位列国产前三，汇川通过收购莱恩和自研驱动器形成供应链优势，正在对标EPSON的SCARA产品，逐步蚕食市场份额。

+   **50kg SCARA**：国产厂商逐渐以YAMAHA为标准，使用RV减速机设计大负载SCARA，进入动力电池等领域。

**避坑指南**：

+   文章强调在SCARA的IP等级和洁净等级理解上的误区，IP等级侧重防尘防水，洁净等级则是防止内部污染外界。误将两者混淆可能导致选型失误。

![](img/e358aa45199b880842cb5f7b103b2f27.png)

### 茶思读书汇 | 通晓半导体的24本必读好书

https://mp.weixin.qq.com/s/sut32HIsQNzEC5lJTWVrSQ

1. **模拟电路**

+   《Design of Analog CMOS Integrated Circuits》（模拟CMOS集成电路设计） - Behzad Razavi

+   《Analysis and Design of Analog Integrated Circuits, Fifth Edition》（模拟集成电路的分析与设计，第五版） - Paul R. Gray, Paul J. Hurst, Stephen H. Lewis, Robert G. Meyer

+   《CMOS Analog Circuit Design, Third Edition》（CMOS 模拟集成电路设计，第三版） - Phillip E. Allen, Douglas R. Holberg

2. **数字电路**

+   **无具体书籍推荐**

3. **硬件描述语言**

+   《Verilog编程艺术》 - 魏家明

+   《SystemVerilog for Design, Second Edition》（SystemVerilog硬件设计及建模，第二版） - Stuart Sutherland, Simon Davidmann, Peter Flake

4. 高级综合技术（High-Level Synthesis）

+   《An Introduction to High-Level Synthesis》 - Philippe Coussy, Daniel D. Gajski, Michael Meredith, Andres Takach

5. **数字验证**

+   《Writing Testbenches using SystemVerilog》 - Janick Bergeron

6. **逻辑综合**

+   《高级ASIC芯片综合》 - Himanshu Bhatnagar

7. **静态时序分析（STA）**

+   《Static Timing Analysis for Nanometer Designs》 - Jayaram Bhasker, Rakesh Chadha

8. 物理实现（Floorplan, CTS, APR）

+   《数字集成电路物理设计》 - 陈春章, 艾霞, 王国雄

9. **半导体制造工艺**

+   《半导体制造技术》（Semiconductor Manufacturing Technology） - Michael Quirk, Julian Serda

+   《Introduction to Microelectronic Fabrication, Second Edition》（微电子制造导论，第二版） - Richard C. Jaeger

10. **半导体器件物理**

+   《Integrated Microelectronic Devices: Physics and Modeling》（集成微电子器件） - J. del Alamo

11. **可测试性设计（DFT）**

+   《Introduction to Advanced System-on-Chip Test Design and Optimization》 - Erik Larsson

12. **可制造性设计（DFM）**

+   《Design for Manufacturability Handbook, Second Edition》 - James Bralla

13. **半导体封装与测试**

+   《Semiconductor Packaging: Materials Interaction and Reliability》 - Andrea Chen, Randy Hsiao-Yu Lo

14. **半导体材料科学**

+   《Semiconductor Materials》 - Lev I. Berger

+   《The Materials Science of Semiconductors》 - Angus Rockett

![](img/f04d33ea9f9e12ba6937c4d4133d714d.png)

# 招聘公告

---

![](img/8bb868bd25d19c30f0402288f34a68d3.png)

**奇绩创坛****由陆奇博士**于2019年创立，使命是在源头最大化驱动创新。奇绩研究组将研究模型理论、到投研战略分析、市场策划等不同背景的同学聚在一起，搭建GenAI时代独特的跨界人才社群。

## 🚀F**24实习岗位**

##### [行研-学术科研人才交流组实习生](https://miracleplus.feishu.cn/docx/Yn6rdf2iOoY2Rcx8yVNcUBYRnch#part-QkMcdeJmuob9uvxvV3qcLSzHn8g)

##### 行研-GenAI研究岗位-战略分析（偏产品）

##### Web / AI 全栈开发工程师实习生

## **火热开放中🚀**

##### 行研-GenAI研究岗位-前沿学术探索

##### 行研-GenAI研究岗位-战略分析（偏咨询）

##### 创新活动产品运营 （大模型系列活动产品运营）

> **点击**相关岗位可查看对应**岗位需求【JD】；**相关岗位投递请**扫描二维码**

![](img/1b7ca25ce3b6681e5c1e2621b07e288f.png)

> 欢迎**推荐/自荐**

> 期待在**奇绩创坛**遇见更好的你

---

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

##

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报****知识库****】**这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/c1e909ac1422e718dfe59e0c8f1de762.png)