
# 奇绩大模型日报（5月 31日）

> 来源：[https://miracleplus.feishu.cn/docx/UoY3du0slo7Ma3xN6gscvByZnfh](https://miracleplus.feishu.cn/docx/UoY3du0slo7Ma3xN6gscvByZnfh)

## 推特

### Karpathy谈评估：从SEAL排名到训练集的泄露

https://x.com/karpathy/status/1795873666481402010

很好，一个与 @lmsysorg 在评估大语言模型方面的有力竞争者加入了对话。大语言模型的评估正在改进，但不久前它们的状态还非常黯淡，定性体验与定量排名往往不一致。这是因为建立良好的评估方法非常困难 - 在特斯拉，我可能有1/3的时间花在数据上，1/3花在评估上，1/3花在其他所有事情上。评估必须全面、有代表性、高质量，并衡量梯度信号（即不能太容易，也不能太难），在定性和定量评估一致之前，有很多细节需要考虑和处理。我推荐的一些有趣的细节可能是 Open LLM Leaderboard MMLU 的说明:https://github.com/huggingface/blog/blob/main/open-llm-leaderboard-mmlu.md... 另一个不太明显的部分是，任何开放（非私有）的测试数据集不可避免地会泄露到训练集中。这是人们强烈直觉怀疑的事情，也是最近这个 GSM1k 轰动一时的原因 https://arxiv.org/html/2405.00332 即使大语言模型开发人员尽了最大努力，防止测试集渗透到训练集中（并记住答案）也是困难的。当然，你可以尽最大努力过滤掉完全匹配的内容。你还可以用 `n-gram` 重叠或类似方法过滤掉近似匹配。但是你如何过滤掉合成数据重写，或与数据相关的在线讨论？一旦我们开始常规训练多模态模型，你如何过滤掉数据的图像/截图？你如何防止开发人员例如对测试集进行向量嵌入，并专门针对在嵌入空间中与测试集高度一致的数据进行训练？最后一个组成部分是，并非所有我们关心的大语言模型任务都可以自动评估（例如总结等），此时你希望让人类参与进来。当你这样做时，你如何控制所涉及的所有变量，例如人们对实际答案、长度、风格的关注程度，以及如何处理拒绝等。总之，良好的评估出人意料地困难，需要大量的工作，但相当重要，所以我很高兴看到更多的组织加入努力，做好这项工作。

![](img/3a807066f78678fb094a131be664ce67.png)

### Perplexity Pages：将您的研究转化为视觉吸引人的文章

https://x.com/perplexity_ai/status/1796203494401040846

我们很兴奋地推出了 Perplexity Pages，这是一种将您的研究转化为视觉吸引人的文章的简单方法。通过格式化的图像和章节，Pages 让您可以分享关于任何主题的深入知识。现在 Pro 用户可以使用，并将很快向所有用户推广！

![](img/a2b4a53935250e30335ed8824115f8a0.png)

[`ssstwitter.com_1717090452129.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OzCVbnriko2ZF5xt5nMch0ypn0g)

### Daniel Han 分享英伟达研究报告笔记：4nm 的研究推理芯片、指数尾数等

https://x.com/danielhanchen/status/1796253349932843214

我从 NVIDIA 研究报告中的笔记：

1. NVIDIA 有一个 4nm 的研究推理芯片，每瓦特可达 96 `int4` TOPs，而 Blackwell 的芯片每瓦特可达 20 TOPs。
2. B200 的 `float4` 是指数（exponent）为 2，尾数（mantissa）为 2？也许我听错了？我以为指数 + 尾数 = 3。
3. 加速不是来自摩尔定律（提升 3 倍），而是来自更小的数值表示。从 `fp32` 到 `f4` 可提升 32 倍。但最近的 `LLMs` 物理学论文表明，`int4` 的效果差 2 倍，这限制了加速的上限。1.58 bit 不会有帮助。
4. Tensor Cores / 复杂指令 `HMMA` 的速度提高了 13 倍，能耗更低。
5. NVIDIA 正在研究从 `2:4` 稀疏性到 `2:8` 稀疏性？

演讲链接: https://youtu.be/gofI47kfD28?si=41UIMkpMCyb_qWqA

`LLMs` 物理学论文: https://arxiv.org/abs/2404.05405

![](img/c59e2f8e69af190fa95684b0c8f23711.png)

### 非常好模型，使我的大脑旋转：通过 Era3D 创造 3D 肖像

https://x.com/xxlong0/status/1796229904843030836

只需一张图片，使用我们强大的 `Era3D` 模型创建您的 3D 肖像。@ylecun

`Era3D` 是一个新的高分辨率跨域扩散模型，可以生成 `512x512` 的彩色图像和法线贴图（`Wonder3D` 和 `Syncdreamer` 只支持 `256x256` 分辨率）。有了这样的 `2D` 表示，就可以创建如此详细的 `3D` 重建。非常感谢 @pengli_hkust 的努力。

重建部分无法集成到 `Huggingface` 演示中。访问我们的 `Github` 仓库以获得完整体验。

`Github` 仓库: https://github.com/pengHTYX/Era3D

网页: https://penghtyx.github.io/Era3D/

用于 `MV` 颜色和法线预测的 `Huggingface` 演示: https://huggingface.co/spaces/pengHTYX/Era3D_MV_demo

![](img/21bc2d11e18b2feca44f78bdd1357d49.png)

[`ssstwitter.com_1717138543284.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/H1h1b3FYGoVeBYx2ClycADSPnQg)

### Suno v3.5 上线，每个人都可以一次生成完整歌曲；LeCun：我超爱

https://x.com/suno_ai_/status/1796273804991156326

`v3.5` 现已向所有用户开放！🥳

现在每个人都可以：

• 制作 4 分钟的歌曲。您现在可以一次生成完整的歌曲了！
• 创建最长 2 分钟的歌曲延伸部分
• 体验改进的歌曲结构和人声流畅度

您的反馈对我们很重要，有助于我们随时间改进 `Suno`。所以请继续给我们反馈，对你喜欢的歌曲点赞 👍，对你不喜欢的歌曲点踩 👎。

❤️ `Suno` 团队

---

LeCun：我能说我超爱 `Suno` 吗？我最喜欢的一些作品：

狗狗狗狗狗狗狗狗汪汪 https://suno.com/song/1783c864-18fb-440f-bc51-15701a19e4b5

化学元素 https://suno.com/song/5f324463-08a7-490e-b9c5-f8e2d399baa9

`train_gpt2.c` 头文件（谁做的哈哈）https://suno.com/song/2a210337-62fc-49f8-8850-9af12e06e6e0

`Suno` 教程（用 `Suno` 写的！）：

https://suno.com/song/d960e84a-1b03-46a2-999e-2a896a56bd57

还有很多其他的。太棒了。还有人有最爱的作品吗？

![](img/e3e2bbc1b98fb7fc64056d54936257df.png)

## 资讯

### AI 内容创作开卷，为什么百度文库成为超强玩家？

https://mp.weixin.qq.com/s/zsJvXcrjxVU6T2kc9NK0_Q

大语言模型发展至今，对各行各业的赋能程度越来越深，一些代表性产品正在改变甚至颠覆着整个行业。简单回顾便能发现，`ChatGPT` 引领了一场文本创作革命、`Stable Diffusion` 等文生图大模型降低了绘画门槛、视频大模型 `Sora` 令影视从业者感到恐慌、音乐大模型 `Suno` 引发了一场音乐圈地震…… 我们能够感受到 AI 大模型重塑内容创作行业尤其是跨模态生成的巨大潜力。如今，AI 大模型能力的释放，不仅可以将用户需求转化为成熟产品，也能让经典产品展现出前所未有的活力。在 5 月 30 日举行的 2024 百度移动生态万象大会上，我们从百度搜索、百度文库、百度 `APP` 等产品基于文心大模型发布的各项行业领先新功能中，看到了全新的产品形态。本届万象大会还是国内首个智能体生态大会，百度希望更多用户、创作者和生态伙伴加入并共同构建低门槛、多场景、高价值的智能体生态圈，探索智能体赋能产品、场景和行业的新思路。

![](img/b82b45a231a4cc4e37d7770ba174f6fa.png)

### Suno `3.5` 版本模型已向所有人开放 可制作 4 分钟歌曲

https://www.pingwest.com/w/295428

据 `Suno` 官方消息，`Suno` 的最新 `3.5` 版本模型已经向所有人开放，这个版本的改进包括：可以制作 4 分钟的歌曲，创建最长 2 分钟的歌曲扩展，以及显著改进的歌曲结构。

![](img/bd9a447d67f4a963fe04c0378e2cf6a9.png)

### 爆火 `ChatTTS` 突破开源语音天花板，3 天斩获 9k 的 `Star` 量

https://mp.weixin.qq.com/s/O6a5LdjH-2LGR5tep3uUeg

近日，一个名为 `ChatTTS` 文本转语音项目爆火出圈，引来大家极大的关注。短短三天时间，在 `GitHub` 上已经斩获了 `9.2 k` 的 `Star` 量。作者本人也在 x 上表示，`ChatTTS` 突破了开源天花板。不过，目前开源的只是底模，没有经过 `SFT` 监督微调。

![](img/6dd8df57ee52f79d161690a75b774f1c.png)

### Claude 上线 `ToolUse` 功能，可与外部数据和工具交互

https://www.anthropic.com/news/tool-use-ga

人工智能公司 `Anthropic` 宣布 AI 助手 `Claude` 的 `Tool Use` 功能已全面上线。该功能使 `Claude` 能够自主与外部数据源、`API` 和工具进行交互，以改变企业利用人工智能实现任务自动化、个性化推荐和简化数据分析的方式。对于使用 `Anthropic Messages API`、`Amazon Bedrock` 和 `Google Vertex AI` 的开发人员来说，`Tool Use` 现已在整个 `Claude 3` 模型系列中可用。

![](img/5c95788e3cf6f706c52696a5b7868762.png)

### 解决 `Transformer` 根本缺陷，`CoPE` 论文爆火：所有大模型都能获得巨大改进

https://mp.weixin.qq.com/s/JxB6JU6MxO3709mkg7penw

LeCun 宣传的这篇论文题目为《`Contextual Position Encoding: Learning to Count What's Important`》，来自 Meta 的 FAIR。这篇论文的重要性不言而喻。短短 `24` 小时之内就成为了 AI 领域最热门的论文之一。它有望解决如今大模型（`LLM`）最让人头疼的问题。总的来说，该研究提出了一种新的用于 `Transformer` 的位置编码方法 `CoPE`（全称 `Contextual Position Encoding`），解决了标准 `Transformer` 无法解决的计数和复制任务。传统的位置编码方法通常基于 `token` 位置，而 `CoPE` 允许模型根据内容和上下文来选择性地编码位置。`CoPE` 使得模型能更好地处理需要对输入数据结构和语义内容进行精细理解的任务。文章通过多个实验展示了 `CoPE` 在处理选择性复制、计数任务以及语言和编码任务中相对于传统方法的优越性，尤其是在处理分布外数据和需要高泛化能力的任务上表现出更强的性能。`CoPE` 为大型语言模型提供了一种更为高效和灵活的位置编码方式，拓宽了模型在自然语言处理领域的应用范围。有网友表示，`CoPE` 的出现改变了在 `LLM` 中进行位置编码的游戏规则，此后，研究者能够在一个句子中精确定位特定的单词、名词或句子，这一研究非常令人兴奋。

![](img/cf029634fb26d997b4b907320cdd8994.png)

### `SOTA` 性能，多尺度学习，中山大学提出蛋白质-药物相互作用 AI 框架

https://mp.weixin.qq.com/s/wmE9wr-IMDK96IujuHgWPA

蛋白质、药物和其他生物分子之间的相互作用，在各种生物过程中发挥着至关重要的作用。了解这些相互作用对于破译生物学过程背后的分子机制和开发新的治疗策略至关重要。当前的多尺度计算方法，常常过于依赖于单一尺度，而对其他尺度的拟合不足，这可能与多尺度学习的不平多尺度衡性和固有的贪婪性有关。为了缓解优化不平衡，中山大学和上海交通大学的研究人员提出了一种基于变量期望最大化的多尺度表示学习框架 `MUSE`，它可以有效地整合多尺度信息进行学习。该策略通过相互监督和迭代优化，有效融合原子结构和分子网络尺度之间的多尺度信息。`MUSE` 不仅在分子相互作用（蛋白质-蛋白质、药物-蛋白质和药物-药物）任务方面优于当前最先进的模型，而且在原子结构尺度的蛋白质界面预测方面也优于当前最先进的模型。更重要的是，多尺度学习框架可扩展到其他尺度的计算药物发现。

![](img/1ccd99816003b03d5d1d18a9575123e3.png)

### 抗衡英伟达 `NVLink`！微软、谷歌、`Meta` 以及 `AMD` 等硅谷巨头联手成立 `UALink` 制定统一标准

https://mp.weixin.qq.com/s/nx0xzqowbeSR-VT06x7dzw

许多人认为 `Nvidia` 的 AI 系统之所以占据主导地位，只是因为 `GPU` 或 `CUDA` 软件，其实 `Nvidia` 还拥有一系列技术，可用于在多个 `GPU` 和系统上扩展工作负载，其中包括其片上和封装互连、用于服务器或 `POD` 中 `GPU` 到 `GPU` 通信的 `NVLink`、用于扩展 `POD` 之外的 `Infiniband` 以及用于连接到更广泛基础设施的以太网。如今，微软、谷歌以及 `Meta` 等硅谷科技巨头正在成立新的行业组织 `UALink`（`Ultra Accelerator Link`）小组以推动数据中心 AI 加速器芯片的组件开发，通过开放标准对 `Nvidia` 进行反击，其他成员还包括 `AMD`、英特尔、惠普企业（`HPE`）、博通以及思科。

![](img/968d12fc62c87928788e29974ef22d4b.png)

## 产品

### oh！a potato

贡献人：

https://ohapotato.app/

`oh！a potato!` 是一款 AI 驱动的零浪费餐单规划应用程序，由 Radu Lupu 和 Dalma Szabo 开发。该应用旨在帮助用户减少家庭食物浪费，主要功能包括：自动保存和提取各种来源的食谱、根据现有食材生成食谱建议、制定和共享餐单、生成购物清单等。这款应用力图解决人们在制定和执行餐单计划时容易造成食物浪费的问题，为用户提供一站式的零浪费餐单管理解决方案。

![](img/ba1e6f42396557ad2a3d4fcaaa341c30.png)

### `AI Notebook`

贡献人：

https://ainotebook.app/

`AI Notebook App` 是一款智能笔记应用，基于人工智能技术，可以无缝地组织保存用户在手机上的各种内容，包括文本、图像、音频和视频，并提供实时转录、内容摘要、问答等功能，帮助用户更有效地管理信息，增强移动学习和工作的效率。该应用还计划后续增加语音操作和离线访问等新功能，为用户打造一个智能“第二大脑”。

![](img/b7d3466bbd82bf6fd3c30fddffe245f9.png)

## HuggingFace&Github

### `V-Express`

贡献人：

https://github.com/tencent-ailab/V-Express

`V-Express` 是一种创新的肖像视频生成方法，通过渐进的 `dropout` 操作来平衡不同强度的控制信号（如音频、姿态和原始图像），解决了弱信号被强信号所掩盖的问题，并提供了多种生成策略，如无翻转、固定人脸和偏移翻转等，可根据需求调整不同输入条件的权重，生成逼真自然的说话视频，为视频编辑、虚拟主播等应用提供了帮助，同时发布了可直接使用的代码和预训练模型。

![](img/b3476cf4182352a014e33f059795ad1c.png)

### `MusePose`

贡献人：

https://github.com/TMElyralab/MusePose

`MusePose` 是一个基于姿势的虚拟人类视频生成框架，可以根据给定的姿势序列，生成目标人物在参考图像中的舞蹈视频。它采用了扩散模型技术，生成效果超越了当前大多数开源模型。

![](img/fe18648c50775c1cd68f83f12fca196a.png)

[`334049449-bb52ca3e-8a5c-405a-8575-7ab42abca248.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Q8yxbNsrjozFJbxHtmwcKnVpnmf)

### `FinRobot`

贡献人：

https://github.com/AI4Finance-Foundation/FinRobot

`FinRobot` 是一个开源的、综合性的 AI 代理平台，专门针对金融领域设计，旨在利用大型语言模型作为“大脑”搭建智能 AI 代理系统，通过多层次的架构实现金融数据分析、市场预测、交易决策等功能，并配备智能调度器优化代理的性能与集成，为金融行业提供全面、灵活的 AI 解决方案。

![](img/210aa667945ebbc6dffe79f8a5fee6d5.png)

## 投融资

### 光轮智能获数千万 `Pre-A` 轮融资，加速合成数据商业化

贡献人：
