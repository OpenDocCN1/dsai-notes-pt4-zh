# 奇绩大模型日报（9月 12日）

> 来源：[https://miracleplus.feishu.cn/docx/FITpd1lY2ofj6WxHSI9cwzBunGg](https://miracleplus.feishu.cn/docx/FITpd1lY2ofj6WxHSI9cwzBunGg)

# 资讯

### Reader-LM：用于清理和转换HTML为Markdown的小型语言模型

https://jina.ai/news/reader-lm-small-language-models-for-cleaning-and-converting-html-to-markdown/?nocache=1

Jina AI 推出了两款小型语言模型（SLM），Reader-LM-0.5B 和 Reader-LM-1.5B，专门用于从嘈杂的HTML中提取并生成干净的Markdown。这些模型有效地替代了传统的Readability和Turndown库，通过直接从HTML生成Markdown，解决了噪声过滤和标记转换的问题。与大模型相比，这些SLM更具成本效益，同时支持长达256K的上下文长度。

+   **任务**：模型的主要任务是选择性复制HTML内容到Markdown，跳过不必要的HTML标记（如导航栏、侧边栏等），并转换为相应的Markdown语法。

+   **性能**：Reader-LM在多个维度的性能表现优异，包括内容提取、Markdown语法使用和文档结构保留，甚至在某些方面超越了更大的语言模型。

+   **多语言支持**：模型支持多种语言，适用于从新闻文章到电子商务页面等多种类型的网页内容转换。

+   **应用场景**：这些模型已经集成到Google Colab和云平台（如Azure和AWS），用户可以通过这些平台轻松体验和使用Reader-LM进行HTML到Markdown的转换。

1.  **长上下文支持**：可以处理长达256K tokens的上下文，适合处理包含大量噪声的现代HTML。

1.  **高效性**：相比于传统方法，Reader-LM在准确性和效率上都有所提升，尤其在结构保留和格式转换方面表现出色。

1.  **小模型大用处**：尽管参数量较小，Reader-LM仍能高效完成复杂的HTML到Markdown转换任务，且大大减少了推理时间和计算资源消耗。

![](img/6b745b8970d19fb577f0d0cb38073c21.png)

### Arcee-SuperNova：70B参数旗舰模型，OpenAI替代方案

https://blog.arcee.ai/meet-arcee-supernova-our-flagship-70b-model-alternative-to-openai/

Arcee-SuperNova是Arcee AI推出的旗舰70B模型，作为OpenAI迁移计划的一部分，专注于指令跟随能力和与人类偏好的高度对齐。通过使用开源工具和创新的后训练技术，Arcee-SuperNova在性能和人类偏好得分上表现出色，并且为企业提供了灵活、安全的部署选择。

**核心内容：**

+   **模型结构**：Arcee-SuperNova由Llama-3.1-405B-Instruct的蒸馏版本构建，使用自研的DistillKit工具，保证模型在缩减参数量的同时，保留了指令跟随能力。

+   **多模型融合**：最终模型结合了三种训练方法，包括指令优化模型和人类偏好优化模型，确保了性能和人类偏好的双重提升。

+   **性能表现**：模型在多个基准测试中表现优异，特别是在数学查询、推理和知识检索方面，优于其他70B模型。其人类偏好得分接近405B-Instruct模型，大幅提升了用户体验。

**部署和客户集成：**

+   **数据隐私**：SuperNova的一个显著优势是完全在客户的私有环境中托管，保证数据隐私。客户拥有模型权重，且所有更新都需客户同意。

+   **灵活性和定制化**：支持通过人类反馈强化学习（RLHF）对模型进行持续训练，使其更符合客户的业务需求。企业还可以根据特定任务对模型进行定制训练，确保其在关键任务上的卓越表现。

![](img/9538a2c19fb650726c170c08ff5914be.png)

### 机房锂电池火灾致阿里字节服务瘫痪，超 30 小时灭火仍未结束

https://mp.weixin.qq.com/s/pYCVlNB4QnZBy1t250rvLw

2024年9月10日，阿里云位于新加坡可用区C的数据中心发生火灾，起因是锂电池爆炸，火灾持续超过30小时，导致主要科技公司服务中断。受影响的关键云产品包括Redis、MongoDB、RDS MySQL、OSS、OTS和MaxCompute。阿里云表示，部分服务已恢复，但仍有业务因机房断电未恢复。Lazada、TikTok Shop等平台因系统故障影响订单同步，用户反馈服务中断。消防员持续进行灭火，火灾引发的安全隐患增加了救援难度。

![](img/6ba2d349fbb942e00ebba5c0dad180c8.png)

### 字节Loopy、CyberHost研究成果

https://mp.weixin.qq.com/s/sf_fmjDUOaQXD7BbovaY5A

字节跳动近期推出的视频生成模型Loopy引发广泛关注。Loopy基于Diffusion视频生成框架，能够通过输入一张图片和音频生成高保真的肖像视频，展现出呼吸、叹气等自然动作和情绪细节。Loopy引入了inter/intra-clip temporal layers模块，用以捕捉跨时间片段及单片段内的时序信息，并通过Temporal Segment Module强化对长时运动的捕捉。音频驱动生成时，Loopy使用了A2L模块，将音频、表情和运动参数转化为motion latents，实现细腻自然的人像表情生成。

此外，字节跳动还推出了半身人像生成模型CyberHost，其通过Codebook Attention模块强化了对人脸和手部的结构先验学习，解决了纯音频驱动下手部动作生成的难题。通过Body Movement Map和Hand Clarity Score等机制，CyberHost进一步提升了人体和手部动作生成的质量。

![](img/2506209dd0c09c023f56e111a9ef15e4.png)

# 推特

### Shawn Wang分享MistralAI 峰会现场笔记：前沿AI就在你手中

https://x.com/swyx/status/1833926630861070359

Shawn Wang 在他的博客中记录了他参加 MistralAI 峰会的现场笔记，该峰会由 Jensen Huang 和 Arthur Mensch 以及团队成员出席。他提到这是 Mistral 平台愿景的首次阐述，超越了简单的 Mistral 模型托管服务。

Shawn Wang 还指出，他曾见过类似于 Mistral 模型优先级的图表，但这次的图表是公开的。他介绍了 Guillaume Lample 对当前 Mistral 模型套件的看法，特别提到 Large 2 模型在代码任务上的表现优于 L3 405b 模型。尽管如此，由于不是开源的，选择使用它的理由并不明确。

Shawn Wang 还提到 Pixtral 12B 模型的亮点，该模型支持任意大小的图片和任意长度的任务序列，并在多种基准测试中表现优于封闭模型，例如 Phi 3、Qwen VL、Claude Haiku 和 LLaVA。

![](img/736bf443c1b67efcc490b7884e82b520.png)

### 同理心语音接口2：全新的语音到语音基础模型

https://x.com/hume_ai/status/1833906262351974483

介绍同理心语音接口2（EVI 2），我们全新的语音到语音基础模型。EVI 2将语言和语音合并为一个专门为情感智能训练的模型。

你可以立即试用并开始构建。

![](img/157a4b3943f23cc720a675ccd1de3cd7.png)

[ssstwitter.com_1726116815236.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PIpyb99zVoitUtx169occg20nIu)

### Fish Speech 1.4：开源文本转语音模型

https://x.com/rohanpaul_ai/status/1834030861605384241

全新开源的文本转语音模型：Fish Speech 1.4 非常出色！

基于 70 万小时的多语言语音数据训练，支持 8 种语言

+   即时语音克隆 🗣️

+   超低延迟 ⚡

+   紧凑模型（~1GB 权重） 🏋️‍♂️

![](img/b4c630f2309357a2576b2e75f6ee0046.png)

[ssstwitter.com_1726130959425.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/RlWBbILc4oOHqnxQvizcKzh8nhg)

### Midjourney正在开发一个3D系统，允许用户进入 Midjourney 图片中

https://x.com/MartinNebelong/status/1833961448734699989

Midjourney 办公时间：他们正在开发一个3D系统，允许用户进入 Midjourney 图片中。不是多边形，也不是 NeRF，也不是高斯技术，而是一种类似 NeRF 的新格式。该项目由在 NeRF 技术推广中扮演重要角色的团队负责人带领。抱歉多边形，但大多数 Midjourney 图片已经比今天的大多数游戏更好看了。

同意关于多边形的看法……希望几年后它们会成为过去。

如果领导 Midjourney 新3D格式项目的人不是 Alex Evans，我会非常失望 ;-)

看看这个小视频，想象一下，Dreams 图形引擎的很大一部分是由于 Alex Evans 的工作。

![](img/be40b90f0434f17e8d7695aef7687e0c.png)

[ssstwitter.com_1726124176125.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/TvhWbYFb4o87Wrx7CGkcoBwTnsd)

# 论文

### **智能体工作流记忆**

贡献人：

链接：http://arxiv.org/abs/2409.07429v1

尽管基于语言模型的智能体在解决诸如网页导航等现实世界任务方面具有潜力，但当前的方法仍然在具有复杂动作轨迹的长期任务中遇到困难。相比之下，人类可以通过从过去经验中学习可重用的任务工作流程并将其用于指导未来动作，灵活地解决复杂任务。为了构建能够从这一过程中获益的智能体，我们引入了智能体工作流记忆（AWM），一种用于诱导常用重复例程（即工作流程）并有选择地为智能体提供工作流程以指导后续生成的方法。AWM灵活地适用于离线和在线场景，智能体可以事先从训练示例中诱导工作流程，也可以即时从测试查询中诱导。我们在两个主要的网页导航基准上进行了实验--Mind2Web和WebArena--涵盖了来自旅行、购物、社交媒体等200多个领域的1000多个任务。AWM显著提高了基准结果，在Mind2Web和WebArena上相对成功率分别提高了24.6%和51.1%，同时减少了成功解决WebArena任务所需的步骤。此外，在跨任务、网站和领域评估中，在线AWM可以稳健地泛化，超越基准，当训练-测试任务分布差距扩大时，优势从8.9到14.0绝对点。

![](img/e3cf261acafb817da1051ba6b4242cbe.png)

![](img/3ddcfcf90b1f1531c9e6e1779b6e8851.png)

### **T**hink Together and Work Better: 结合人类和LLM的心理声音结果，实现有效的文本评估

贡献人：

链接：http://arxiv.org/abs/2409.07355v1

本研究介绍了 \textbf{InteractEval}，一个整合人类专业知识与大语言模型 (LLMs) 的框架，利用“思考大声”（TA）方法为基于检查表的文本评估生成属性。通过将人类的灵活性和推理与LLM的一致性结合起来，InteractEval在四个不同维度，包括连贯性、流畅性、一致性和相关性方面，优于传统非LLM和LLM基线。实验还调查了TA[方法的有效性，显示其促进了人类和LLM的发散思维，](https://github.com/BBeeChu/InteractEval.git%7D%7D%E3%80%82)[导](https://github.com/BBeeChu/InteractEval.git%7D%7D%E3%80%82)致生成更广泛相关属性，提高了文本评估性能。比较分析表明，人类擅长识别与内部质量相关的属性（连贯性和流畅性），但LLMs在与外部对齐相关的属性（一致性和相关性）方面表现更好。因此，充分利用人类和LLMs一起能产生最佳评估结果。换句话说，本研究强调了在自动化基于检查表的文本评估框架中有效结合人类和LLMs的必要性。代码位于 \textbf{\url{https://github.com/BBeeChu/InteractEval.git}}。

![](img/eb44e541e356bf8daefdd7aaa320462c.png)

![](img/531a2c3c6ff1d0e44b9c72935d829505.png)

### 门控插槽注意力：高效线性时间序列建模

贡献人：

链接：http://arxiv.org/abs/2409.07146v1

本文介绍了门控槽关注（GSA），它通过结合受门控线性注意力（GLA）启发的门控机制来增强具有有界内存控制（ABC）的注意力。 GSA由两层GLA组成，通过softmax连接，利用上下文感知的内存读取和自适应遗忘来提高内存容量，同时保持紧凑的循环状态大小。这种设计通过GLA的硬件高效训练算法和减小的状态大小极大地增强了训练和推理效率。在“微调预训练Transformer为RNN”（T2R）设置中保留softmax操作尤为有益，减少了从头开始进行广泛训练的需要。广泛的实验验证了GSA在需要上下文召回和T2R设置中的优越性能。

![](img/ccce5a65a4c8f192cb1a8dc3f1dd1943.png)

![](img/6dd88c0db838dbc9303cefbd20ca3ab2.png)

### **合成持续预训练**

贡献人：

链接：http://arxiv.org/abs/2409.07431v1

摘要：在大规模、非结构化的互联网文本上进行预训练，使语言模型能够获得大量世界知识。然而，这种知识获取是数据效率低下的——为了学习一个给定的事实，模型必须在数百到数千种不同的表示形式上进行训练。当将预训练模型适应到小语料库的特定领域文档时，这就构成了挑战。为了弥合这一差距，我们提出了一种合成持续预训练的方法：利用小的特定领域语料库来合成一个更适合学习的大语料库，然后在合成语料库上继续进行预训练。我们用EntiGraph实现了这一提议，这是一种合成数据增强算法，从源文档中提取显著实体，然后通过连接抽样的实体生成多样化的文本。使用EntiGraph进行合成持续预训练，可以使语言模型能够回答与源文档相关的问题并遵循通用指令，而无需访问这些文档。如果在推理时可访问源文档，我们展示通过我们的方法获得的知识与检索增强生成相结合。为了更好理解这些结果，我们建立了一个简单的EntiGraph数学模型，并展示了合成数据增强如何“重新排列”知识以实现更高效的学习。

![](img/eedfddccb78eeb8f5273ccdbd23954b7.png)

![](img/690f7e5d3cf329678b684b652fd7425d.png)

### **表征微调**

贡献人：

链接：http://arxiv.org/abs/2409.06927v1

激活工程在在线控制大语言模型（LLM）中越来越受欢迎。本文扩展了用代表兴趣行为方向的向量进行主动导向的思想，直接将这些向量微调到模型中，避免了在线控制的需要。作者首先识别了与诚实相关的激活向量，然后通过在生成过程中向残差流激活加上这些向量的倍数来使模型输出变得更或更不诚实。接着，作者展示了通过将这些向量直接微调到模型中，同时使用基于余弦相似性和标准token损失的双损失函数（“表示微调”），可以实现类似的效果。最后，作者比较了生成的响应模型对来自不同模型的诚实探测提示的反应，结果显示通过余弦相似性加token损失微调向量到模型中，效果比在线导向更强，并且比使用标准损失泛化得更好，显示了这种方法作为一种安全措施的潜在实用性。

![](img/8753ed7aecdf660e715f76f633d45cd8.png)

![](img/1809dbf2a59b4e1e83b095c0b8ffd2b5.png)

# 产品

### Serra

贡献人：

https://cloud.serra.io/

Serra 是一个基于 AI 的招聘搜索引擎，允许招聘人员通过自然语言输入搜索条件，快速找到合适的人才。它从 LinkedIn、Indeed 和 GitHub 等平台整合约 10 亿候选人数据，自动生成关键词并展示最佳匹配的候选人，可以帮助提高招聘效率。

![](img/59fbebae2cb208bbf958af700e4c98e4.png)

### Replit Agent

贡献人：

https://docs.replit.com/

Replit Agent 是一个 AI 驱动的工具，可以帮助用户通过自然语言提示轻松构建软件项目。它支持编写代码、设置环境和管理部署，可以提升开发者的创造力，使软件开发对所有技能水平的用户更加友好。

![](img/65020d9d9fdd9a2e93b4807a491205d6.png)

# HuggingFace&Github

### Draw an Audio

贡献人：

https://yannqi.github.io/Draw-an-Audio/

Draw an Audio 是一种创新的视频到音频合成方法，通过结合文本、视频和信号指令，利用潜在扩散模型生成高质量的同步音频，克服了传统方法的可控性和灵活性不足的问题，展现出更强的实用性和创意潜力。

![](img/785820601791a0ec27b0faf508e78ca2.png)

[Draw_An_Audio_SUV_in_dust_by_SORA.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OkYZbSwFdo5Ty7xeqeZcQSAynNh)

### Llama-3.1-8B-Omni

贡献人：

https://huggingface.co/ICTNLP/Llama-3.1-8B-Omni

LLaMA-3.1-8B-Omni 是一个高质量的语音语言模型，支持低延迟（226 毫秒）且能够同时生成文本和语音响应，实现无缝的语音交互。它基于 Llama-3.1-8B-Instruct 构建，训练速度快，适用于语音助手等应用场景。

![](img/ac442c35494015435d743c856802757e.png)

# 投融资

### Trieve完成350万美元融资，推动AI搜索与生成式问答技术发展

https://trieve.ai/trieve-fundraise-announcement/

2024年9月11日，Trieve宣布完成超额认购的350万美元融资，由Root Ventures领投，其他投资者包括Y Combinator、Soma Capital、Kulveer Taggar、Transpose Platform，以及一批知名的战略天使投资人，如JJ Fliegelman、Richard Aberman、Rajiv Ayyangar、Jenny Fleiss和Rohan Das。Trieve由Myself和Denzell Ford创立，旨在为AI应用开发提供定制化基础设施。

本轮融资将用于增强销售策略和提升客户成功率，Trieve的客户已涵盖电商、ERP系统、社交媒体平台等多个行业，目前包括Mintlify、BillTrack50和AmLaw100律所的客户，Trieve已支持超过16,000个搜索栏。

领投方Root Ventures的合伙人Lee Edwards指出，Trieve正在通过AI驱动的搜索和生成式问答（RAG）技术，改善应用内的搜索体验，推动产品搜索功能的提升。

![](img/9fe44048dab7ff36fc181403896b34a9.png)

# 学习

### Transformer，SSM，Linear Attention的联系与理解

贡献人：

https://zhuanlan.zhihu.com/p/705837508?utm_psn=1790475218659184640

Transformer 模型通过自注意力机制解决了长距离依赖问题，但计算成本较高。SSM 模型将序列建模转化为状态空间问题，通过线性代数优化计算效率。Linear Attention 机制进一步降低了计算复杂度，实现了更高效的序列处理。三者皆致力于提升序列建模的时间空间效率，各自通过不同的技术细节实现了对长距离依赖关系的捕捉与优化。

![](img/da15dbd76679e51bf3ea4472f05f12ba.png)

### LLM 推理框架之上：10 中常见 LLM 推理系统总结

https://mp.weixin.qq.com/s/vkHIVjFHGsg2FeBoBxJFJA

文章介绍了 LLM 在推理阶段面临的挑战，如模型规模庞大导致的高计算成本和延迟问题。接着，文章详细剖析了 10 种主流的 LLM 推理解决方案，包括 NVIDIA Triton、Hugging Face Inference API、OpenAI API 等，分别讲解了它们的架构设计、优化策略以及在实际应用中的表现。文章指出，这些系统通过硬件加速、模型优化和框架集成等技术手段，显著提升了推理效率。其中，NVIDIA Triton 支持多种硬件和框架，实现了灵活的模型部署；Hugging Face Inference API 提供了用户友好的接口，方便开发者快速集成模型；OpenAI API 则提供了强大的 GPT-3 模型推理服务。此外，文章还探讨了如 DeepSpeed Inference 采用了高效的推理优化技术，特别是针对 Transformer 模型的优化。最后，文章预测了 LLM 推理系统未来发展的趋势，强调了定制化和自动化优化的重要性，以及对边缘计算和云端集成的支持。

![](img/4ee291aa660bacc863a1f593e4d493d0.png)

### NVidia GPU指令集架构-整数运算

https://zhuanlan.zhihu.com/p/700921948?utm_psn=1801032039450959872

NVIDIA GPU 的整数运算指令集架构支持多种精度的整数计算，包括 8bit、16bit、32bit 和 64bit。GPU 通过统一的 32bit 加法器执行整数加法，对于小于等于 32bit 的计算，使用IADD3指令，而对于 64bit 加法，则需要两条指令配合 Predication 寄存器处理进位。在表示有符号整数时，采用补码形式，以便于计算机内部的处理。整数的位移操作包括逻辑位移和算术位移，通过SHF指令实现，并根据操作数的类型区分逻辑和算术位移。NVIDIA GPU 没有直接提供整数除法指令，通常会转换为浮点数运算，然后再进行整数结果的修正。整数与浮点数之间的转换也得到了指令集的支持，如I2F指令及其变种。此外，为了提高计算效率，NVIDIA GPU 还提供了 Tensor Core 整数指令，用于矩阵乘法等高性能计算任务。

![](img/27bc11ac2f2247ede7b3a5c789ad5287.png)

### 开源框架 ModelScope-Agent 加速多智能体应用构建

https://zhuanlan.zhihu.com/p/711759503?utm_psn=1801706406128250880

ModelScope-Agent 是一个开源框架，专为加速多智能体应用的构建而设计。它采用模块化架构，支持不同类型的智能体，如机器人、虚拟助手和自动驾驶车辆，并且能够跨平台运行。该框架的核心在于其高度集成化的数据处理和机器学习模型，这些模型不仅支持智能体的决策过程，还能够进行行为学习。技术细节上，ModelScope-Agent 提供了丰富的 API 接口，允许开发者自定义和扩展功能，同时保持代码的整洁和可维护性。此外，框架还遵循了前后端分离的原则，使得前端界面和后端逻辑能够独立开发和部署，从而提升了开发效率。

![](img/528d9f076e0857a8baa6c944797fda6f.png)

### llm 论文淘金

https://zhuanlan.zhihu.com/p/719626313?utm_psn=1817471415827439617

建议重点关注以下方面：数据清洗方法、预训练数据配比、超参数调优、退火技巧、SFT任务和数据量、DPO/PPO训练技巧、合成数据方法等。

评估工作被认为比训练更重要，尤其是现阶段预训练和优化方法逐渐定型，评估可以通过结果倒推训练技巧的合理性。当前评估方法存在瓶颈，如刷Benchmark结果不准确，GPT4评估不稳定，人工评估虽好但速度慢、成本高。

论文筛选建议关注arxiv和企业发布的研究，尤其是具有数学证明的网络结构和训练方法研究。相较高校，企业的实操经验更有价值。未来趋势上，多模态技术值得提前布局，而可解释性研究虽有趣但实际意义有限。

### Triton 源码走读 -- pass in ttir

https://zhuanlan.zhihu.com/p/718936671?utm_psn=1817541425258176512

OpenAI Triton 是一个为深度学习模型优化的编程框架，提供了高效的 GPU 编程能力。与 CUDA 类似，Triton 允许开发者使用 Python 编写代码，并将其编译为低级 GPU 代码，优化模型的执行效率。Triton 编译流程从 Triton 语言到最终的二进制文件共经历五个阶段：Triton IR、Triton GPU IR、LLVM IR、PTX 和 cubin。在此过程中，编译器对代码进行一系列的转换和优化。

1.  **编译过程中的 Pass 管理**：Triton 的优化流程中包含了多个 Pass，如 Inliner、Canonicalizer、CSE、DCE 和 LICM 等。Inliner Pass 将函数调用内联至主函数，方便后续优化；Canonicalizer 则优化中间表示（IR）中的算子，提升分析效率；CSE 和 DCE 分别用于消除冗余表达式和死代码。

1.  **Triton 特有的操作**：Triton 中的特定操作如 tt.make_tensor_ptr 和 tt.advance 用于处理 tensor 指针的生成和偏移。重写这些操作能够优化内存访问模式，例如通过结合 load 和 store 操作提升性能。

1.  **Pass 实现细节**：每个 Pass 的实现依赖于 MLIR 框架，如 ReorderBroadcast Pass 会将广播操作移至 elementwise 操作之后，减少计算量。此外，Combine Pass 能够合并多余的操作，如合并嵌套的 addptr 操作。

1.  **架构优化与硬件抽象**：Triton 的 Pass 和转换过程中混合了硬件特定的信息，如针对 NVIDIA GPU 的优化。在生成中间表示的过程中，会根据底层硬件特性进行特定转换，如内存布局和线程分配的优化。

# 招聘公告

---

![](img/025390ee0f975feaa52f0d0bbec03892.png)

**奇绩创坛****由陆奇博士**于2019年创立，使命是在源头最大化驱动创新。奇绩研究组将研究模型理论、到投研战略分析、市场策划等不同背景的同学聚在一起，搭建GenAI时代独特的跨界人才社群。

## 🚀F**24实习岗位**

##### [行研-学术科研人才交流组实习生](https://miracleplus.feishu.cn/docx/Yn6rdf2iOoY2Rcx8yVNcUBYRnch#part-QkMcdeJmuob9uvxvV3qcLSzHn8g)

##### 行研-GenAI研究岗位-战略分析（偏产品）

##### Web / AI 全栈开发工程师实习生

## **火热开放中🚀**

##### 行研-GenAI研究岗位-前沿学术探索

##### 行研-GenAI研究岗位-战略分析（偏咨询）

##### 创新活动产品运营 （大模型系列活动产品运营）

> **点击**相关岗位可查看对应**岗位需求【JD】；**相关岗位投递请**添加微信💻【****l_ivresse29】**

> ‼️欢迎**推荐/自荐**，期待在**奇绩创坛**遇见更好的你‼️

---

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

##

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～微信：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报****知识库****】**这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/305a168bb7347a903258ee97e7cc5c48.png)