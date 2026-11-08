# 奇绩大模型日报（10月 22日）

> 来源：[https://miracleplus.feishu.cn/docx/PKxXddMdfoSzhWxnn3McVbp1n1e](https://miracleplus.feishu.cn/docx/PKxXddMdfoSzhWxnn3McVbp1n1e)

# 🔉潜空间活动报名

> 本期活动将在**11月9日** **10:00**开始，我们邀请到的嘉宾是**鱼哲**，Le**pton AI 创始成员，曾在阿里云担任高性能 AI** 平台产品负责人，专注于 AI 在多个行业的落地及应用。Lepton AI 致力于建立高效可用的AI 基础设施，让团队更关注于应用构建及落地。在本次分享中鱼哲将带来关于AI产品相关的思考，分享主题《Beyond Infra, what matters?—— 不同AI产品形态对团队的挑战》。

> 除嘉宾分享外，每期设置了【匹配合伙人 Cofounder Matching】环节。你可以和 GenAI 时代最有活力的创业者和研究者线下面对面交流，将有机会找到志同道合、有共同创业梦想的小伙伴。

> 报名通道已开启，欢迎扫描下方二维码报名。

![](img/d94a0231b1dc57aeb5b1d2c9b2d35085.png)

> 本期活动将在**11月9日** **10:00**开始，我们邀请到的嘉宾是**鱼哲**，Lepton AI 创始成员，曾在阿里云担任高性能 AI 平台产品负责人，专注于 AI 在多个行业的落地及应用。Lepton AI 致力于建立高效可用的AI 基础设施，让团队更关注于应用构建及落地。在本次分享中鱼哲将带来关于AI产品相关的思考，分享主题《Beyond Infra, what matters?》。

> 除嘉宾分享外，每期设置了【匹配合伙人 Cofounder Matching】环节。你可以和 GenAI 时代最有活力的创业者和研究者线下面对面交流，将有机会找到志同道合、有共同创业梦想的小伙伴。

> 报名通道已开启，欢迎扫描下方二维码报名。

![](img/3c7aca6f62b63e757ef99ba50fe20810.png)

# 资讯

### RoboDual：行业首款通用具身操作的双系统协同框架诞生

https://mp.weixin.qq.com/s/LYmjmrw2ay2scSYPg1GDvA

智元机器人与上海人工智能实验室合作推出了首款通用具身操作双系统协同框架——RoboDual，旨在融合通才（Generalist）的广泛适应性和专才（Specialist）的高效精准性，以提升机器人的操作灵活性和准确性。传统具身智能系统中，负责任务规划的“大脑”泛化能力强但实操效率低，而负责运动控制的“小脑”精度高但适应性不足，两者通常难以协同工作。RoboDual通过在机器人边缘侧部署该框架，利用“小脑”的低延迟、高精度特点弥补“大脑”的不足，实现高效推理和任务执行。

RoboDual框架采用异步控制架构，即在Generalist生成一次动作时，Specialist能生成多步连续动作，从而在保证鲁棒性和精度的前提下，显著降低了控制延迟。在实验中，RoboDual在NVIDIA RTX 5000 Ada GPU上实现了15Hz的控制频率，在仿真和真机环境中的泛化和适应能力超越了传统模仿学习方法。相比OpenVLA，RoboDual在高效任务适配的同时，推理延迟更低，能够以仅5%的数据量实现对新场景和任务的快速适应。

RoboDual框架中，Generalist部分基于Prismatic-7B VLM的OpenVLA架构，处理视频帧和指令，自回归生成动作潜变量，并交给Specialist模型。Specialist使用Diffusion Transformer (DiT)架构，结合Generalist的动作参考及传感器输入进行多步连续动作预测，确保动态场景中的精确控制。实验表明，RoboDual在多个泛化场景下表现优越，提供了更广泛的通用机器人操作解决方案，推动了机器人技术的广泛应用与发展。

![](img/63e395102acfdea3ba200129f0ea49b7.png)

### OpenAI满血版o1剧透：数学代码能力再破天花板，已开启测试评估

https://mp.weixin.q1q.com/s/tauednAxd_VNjsPTr9SloQ

OpenAI在官网商业化频道中对即将发布的满血版o1模型进行了提前预告。视频显示，满血版o1与GPT-4o和o1-preview（预览版）同场对比，在数学和编码能力上明显领先。据透露，OpenAI计划同步发布GPT和o1两个系列，满血版o1正在基准测试中，未来几个月还将为o1系列增加网页浏览、文件和图像上传等功能，并支持ChatGPT自动选择合适模型。

然而，网友发现一个有趣现象：在PhD级别的科学任务中，满血版o1竟不敌o1-preview。对此，OpenAI首席产品官Kevin Weil解释，这或许是由于两者的构建方式存在细微差别。目前，o1推理模型仍处于改进阶段，未来的测试结果将揭示更详细的性能差异。

![](img/8d4e65043715dc832db4795fdc80b4c0.png)

### 2¹³⁶²⁷⁹⁸⁴¹−1，GPU发现人类已知最大的素数，比第二大多1600万位数字

https://mp.weixin.qq.com/s/QZRE0JU7lSO9rndvNcdetg

近日，人类已知的最大素数被通过GPU发现！这个素数是2¹³⁶²⁷⁹⁸⁴¹−1，总共有41024320位十进制数字。该发现由英伟达前员工卢克通过开源项目GIMPS实现，初步在英伟达A100上发现，并经H100验证。2¹³⁶²⁷⁹⁸⁴¹−1比之前的记录大了1600多万位，是第52个已知的梅森素数，也是GIMPS项目发现的第18个。梅森素数以2P−1的形式存在，由法国数学家梅森命名，因其高效的素性检验算法备受关注。GIMPS项目自1996年成立，作为世界上寿命最长的分布式项目之一，专注于寻找最大梅森素数。卢克的贡献也证明了GPU不仅适用于AI，还能推动基础数学研究。他的“云超级计算机”使用数千台GPU服务器，横跨17个国家，最终发现了这一新素数。虽然梅森素数的实际用途有限，但其探索代表了人类在数学研究领域的追求与荣耀。

![](img/342e9ac6a8560035f1fe2ea04498e889.png)

### 微软一口气发10个商业智能体！内测提高9.4%销售收入，六成500强企业在用Copilot

https://mp.weixin.qq.com/s/NNynk22K0gwmphWauzQKvg

微软CEO纳德拉宣布了AI领域的最新进展，一口气推出了10个商业智能体（Agent），覆盖销售、服务、财务、供应链等业务场景。这些Agent能够自动筛选潜在客户、监控供应链表现、分析客户意图，帮助企业优化业务流程。例如，销售资格Agent可以锁定最有价值的商机，供应商通讯Agent实时处理供应链延迟问题，客户知识管理Agent协助客服快速响应。

这些Agent的功能从简单的提示响应到完全自主决策不等，微软表示，目前已有60%的财富500强企业正在使用Microsoft 365 Copilot来提高业务效率。例如，Lumen Technologies每年预计节省5000万美元，Honeywell的生产力提升相当于增加了187名全职员工，Finastra将项目时间从七个月缩短到七周。

微软还计划下月启动Copilot Studio的公测，允许用户创建自主Agent，连接多个数据源并执行复杂任务。例如，Pets at Home通过案例整理Agent节省了数百万美元，麦肯锡的客户入职Agent使流程缩短90%，汤森路透的法律尽职调查Agent也大大加速了工作流程。

在安全和数据治理方面，微软为Agent提供了全面的保护，确保数据源安全和使用权限管理，Agent严格遵循安全与隐私原则。这些Agent不仅能为企业创造直接的业务价值，还将重新定义业务流程的自动化和智能化。

![](img/72ca7d93893a0613bddc7b2f0d8d9d28.png)

### 大模型是否有推理能力？DeepMind数月前的论文让AI社区热议

https://mp.weixin.qq.com/s/NdRBGFT6systLwn7p2ER7Q

DeepMind 在2023年2月发布了一篇论文《Grandmaster-Level Chess Without Search》，展示了一个参数量为2.7亿的Transformer模型无需复杂的搜索算法，即可达到特级大师级别的国际象棋水平。该模型通过监督学习从Stockfish 16生成的大量棋局数据中学习棋盘动作-值的预测，模型的Elo评分达到了2895，优于AlphaZero的策略网络和GPT-3.5-turbo-instruct。

该论文的结果引发了广泛讨论。支持者认为，这表明Transformer不仅是“统计模式识别器”，还具备一定的推理和规划能力。然而，批评者指出该模型的评估方法有局限性，尤其是在“blitz”（闪电战）这种快速棋赛环境下，这类比赛依赖直觉而非深度推理，因此不足以证明模型具备真正的推理能力。此外，模型对人类玩家的表现不及其与机器人对战时的得分，这也引发了关于模型泛化能力的争议。

论文中，DeepMind通过监督学习将复杂的国际象棋策略蒸馏到前馈Transformer中。为了构建训练数据，研究者从lichess.org下载了1000万场游戏，并使用Stockfish 16为这些游戏的棋盘状态估值。模型使用FEN字符串编码棋局状态，并通过标准的Transformer架构进行训练。结果表明，随着数据量和模型规模的增加，模型在各种国际象棋谜题和实际比赛中的表现显著提升，表明规模和数据对于模型的推理能力至关重要。

![](img/a5e1844ed37c4c1f8871cd4ff8da6994.png)

### xAI推出API，探索生成式AI市场

https://techcrunch.com/2024/10/21/xai-elon-musks-ai-startup-launches-an-api/

2024年10月，埃隆·马斯克的AI初创公司xAI正式推出其首个API，名为“grok-beta”。该API提供xAI的生成式AI模型，按使用量收费，每百万输入Token收费5美元，每百万输出Token收费15美元。虽然目前API的功能相对基础，但支持与外部工具的连接，未来可能会加入视觉模型，具备分析文本和图像的能力。

xAI于2023年成立，最初推出的Grok模型被集成到X（前身为Twitter）中，供X Premium+用户使用。Grok以其“叛逆性格”著称，敢于回答其他AI系统拒绝的问题，并且在图像生成和新闻摘要等功能上表现活跃。不过，Grok也因其错误频发的摘要和无防护的图像生成功能引发争议。

xAI目前正加速追赶OpenAI等竞争对手，并已获得包括Andreessen Horowitz、红杉资本和富达投资在内的多家机构的60亿美元投资。马斯克的计划包括使用X平台上的数据以及其旗下公司（如特斯拉、SpaceX等）的数据训练AI模型，但这一举措遭到特斯拉股东的反对，认为这会分散资源。

xAI的下一代Grok模型正在田纳西州的孟菲斯数据中心训练，该中心因未经许可使用涡轮机被指责加剧雾霾。公司计划于2025年升级该数据中心，但需得到当地能源监管部门的批准。

![](img/5345e0eb6b12b9688c2e466a5ce2c959.png)

# 推特

### Willison分享：过去七天内使用Claude Artifacts构建的14个互动式迷你应用

贡献人：

https://x.com/simonw/status/1848372319548240371

我在过去七天内使用Claude Artifacts构建了14个互动式迷你应用——以下是一个描述它们的帖子

---

我这周使用Claude Artifacts构建的一切

我是Claude Artifacts功能的超级粉丝。这个功能允许你通过提示Claude创建一个使用HTML、CSS和JavaScript的互动单页应用（SPA），然后可以直接在Claude界面中查看结果，进一步与Claude进行迭代，如果满意的话，还可以复制生成的代码。

我在查看Claude的活动导出数据时（我构建了一个claude-to-sqlite工具，用来将其转换为SQLite，以便在Datasette中进行探索），我决定看看我在过去一周内使用了多少次Artifacts。结果比我预想的还要多！

能够快速创建一个完整的互动应用——有时是作为一个示例原型，但通常是直接解决问题的工具——这真是一个非常有用的功能。

以下是我在过去七天内使用Claude Artifacts做的大部分事情。我几乎为每个项目提供了提示或完整的对话记录。

![](img/c26d90a8a6bac33dc64b534e2ba887d0.png)

### 个性化解释器：只需上传一个PDF，即可收到包含视觉效果和文档中图像的、用你自己声音生成的解释

贡献人：

https://x.com/MisbahSy/status/1848377457574863145

NotebookLM很棒，但如果我想使用自己的声音并添加来自PDF的视觉效果呢？

🚀 介绍个性化解释器——只需上传一个PDF，即可收到包含视觉效果和文档中图像的、用你自己声音生成的解释。

这个视频完全由AI生成。🔊

![](img/e0f9f30e8de9aa9f8d382219022e999c.png)

[ssstwitter.com_1729552812467.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/ZnITbVs3hoyofVxwAbocXZ34nCc)

### IBM和CrewAI宣布合作，与watsonx.ai集成，旨在通过多个AI代理构建和简化工作流程

贡献人：

https://x.com/joaomdmoura/status/1848394061910323325

IBM和CrewAI从今天起合作，帮助美国及全球的企业大规模采用AI代理。

---

宣布新的CrewAI与watsonx.ai集成，旨在通过多个AI代理构建和简化工作流程。

我们非常高兴地分享这一令人兴奋的新合作，将IBM的watsonx.ai™和CrewAI结合在一起。通过结合IBM的AI专业知识与CrewAI独特的协作式代理解决方案，这一集成将彻底改变组织管理复杂工作流程的方式。

![](img/83e142fcfe7f14c981c97be8faafdd2e.png)

### AnthropicAI和OpenAI收入来源对比：前者更像基础设施提供者，而后者更像面向消费者的公司

贡献人：

https://x.com/auchenberg/status/1848427656598970387

根据它们的收入来源，@AnthropicAI 作为基础设施提供者出现，而 @OpenAI 则更像一家面向消费者的公司。

很期待看看几年后这两家公司会发展到什么程度。

![](img/a3654841909dbb727fa75e01fe2ecaf2.png)

### 谷歌分享CT Foundation：医学影像嵌入工具，可用于快速训练模型

贡献人：

https://x.com/GoogleAI/status/1848437918513434780

宣布CT Foundation，这是一个新的医学影像嵌入工具，它接受计算机断层扫描（CT）体积作为输入，并返回一个小型且信息丰富的数值嵌入，用于快速训练模型。了解更多并亲自试用 → https://goo.gle/4dYkClf

![](img/61a1309af156f4ac7a3ed78715c300e9.png)

[ssstwitter.com_1729586877922.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MiCZbEo63oxOCvxBMkkcZPwRnGd)

# 信号

### Heavy-Tailed Diffusion Models

贡献人：

原文链接：https://arxiv.org/abs/2410.14171

推文链接：[https://x.com/xuyilun2/status](https://x.com/xuyilun2/status/1848490806321230212)/1848490806321230212

研究节点：模型体系- 架构体系

这篇文章提出了一种新的扩散模型框架，用于估计具有重尾分布的数据生成。传统的扩散和流匹配模型使用标准高斯先验，无法捕捉重尾行为。文章通过使用多变量学生t分布重新构建扩散框架来解决这个问题，并开发了专门的扰动核，以及基于条件学生t分布的去噪后验。受γ-散度在重尾分布中的启发，文章推导出了重尾去噪器的训练目标。这个框架通过单一的标量超参数控制尾部生成，使其易于调整以适应多种现实世界分布。文章还介绍了t-EDM和t-Flow，这是现有扩散和流模型的扩展，采用学生t先验。这种方法与标准的高斯扩散模型兼容，并且只需要最小的代码更改。实证结果表明，t-EDM和t-Flow在高分辨率天气数据集上的重尾估计方面优于标准扩散模型，其中生成罕见和极端事件至关重要。

![](img/ee8d7e2637c4c8dbb2d3375583479b3f.png)

### A Comparative Study on Reasoning Patterns of OpenAI's o1 Model

贡献人：

原文链接：https://arxiv.org/abs/2410.13639

推文链接：[https://x.com/siweiwu7/status](https://x.com/siweiwu7/status/1848013413060051303)/1848013413060051303

研究节点：模型体系- 推理体系

这篇文章探讨了如何使大型语言模型（LLMs）能够处理更广泛的复杂任务，例如编程和数学问题。文章指出，仅仅增加模型参数数量会导致性能提升递减和计算成本增加。最近，OpenAI的o1模型展示==了测试时计算方法（Test-time  Compute  methods）也能显著提高LLMs的推理能力，但这些方法背后的机制尚未被充分探索。研究者通过在三个领域的一般推理基准测试（数学、编程、常识推理）上比较o1模型与其他测试时计算方法（BoN、Step-wise  BoN、Agent  Workflow和Self-Refine），使用OpenAI的GPT-4o作为基础模型。实验结果表明：o1模型在大多数数据集上表现最佳；搜索多样化响应的方法（如BoN）受到奖励模型能力和搜索空间的限制；将问题分解为多个子问题的方法（如Agent  Workflow）由于领域特定的系统提示而表现更好；此外，文章总结了o1的六种推理模式，并在几个推理基准测试上提供了详细分析。

![](img/ceab10b3c59dcdab6c7a2e86b7ad061f.png)

### Decomposing The Dark Matter of Sparse Autoencoders

贡献人：

原文链接：https://arxiv.org/abs/2410.14670

推文链接：[h](https://x.com/JoshAEngels/status/1848339201864974674)ttps://x.com/JoshAEngels/status/1848339201864974674

研究节点：模型体系- 架构体系

这篇文章研究了稀疏自编码器（SAEs）在分解语言模型激活时的表现，特别是它们未能完全解释模型性能的“暗物质”部分，即未解释的方差。研究发现，SAE的“暗物质”中约一半的错误向量和超过90%的范数可以通过初始激活向量线性预测。此外，研究发现SAE错误范数的缩放行为在每个令牌层面上是可预测的：更大的SAEs主要难以重建与更小的SAEs相同的上下文。文章提出了可能解释这些观察结果的激活模型，包括假设一种新型的“引入错误”，并验证了非线性SAE错误可能与线性可预测部分有根本的不同。最后，文章考察了两种在固定稀疏度下减少非线性SAE错误的方法：推理时梯度追踪和从早期层SAE输出的线性变换。

![](img/465f4ee540dc7e72cd5b498d45d0555c.png)

### GRANITE 3.0 LANGUAGE MODELS

贡献人：

原文链接：[https://github.com/ibm-granite/granite](https://github.com/ibm-granite/granite-3.0-language-models/blob/main/paper.pdf)-3.0-language-models/blob/main/paper.pdf

推文链接：[https://x.com/omarsar0/status](https://x.com/omarsar0/status/1848404138641527105)/1848404138641527105

研究节点：模型体系-预训练-后训练

Granite  3.0是一系列新型的轻量级、开放的基础模型，参数规模从4亿到80亿不等，支持多语言、编码和函数调用，专为企业级应用设计，包括本地和设备上部署。这些模型在多项任务中展现出与其规模相称的最先进性能。报告详细披露了预训练和后训练的技术细节，旨在促进研究社区开发更多开放基础模型。Granite  3.0的所有模型版本均在Apache 2.0许可下发布，支持研究和商业用途，并已与多种量化、微调和部署工具集成。

![](img/58c3624cd396e058519877756f4d62ef.png)

# 产品

### Trag

贡献人：

https://usetrag.com/

Trag 是一个 AI 代码审查助手，能够将工程师的知识转化为可执行的规则，快速审查代码并确保其符合团队标准。用户通过自然语言描述规则，Trag 自动理解并应用于代码库，简化了代码审查流程，提高了效率。

![](img/d35d513ff7e5bf2ee676a589b3504f86.png)

### Talkstack

贡献人：

https://dashboard.talkstack.ai/

TalkStack 是一个基于人工智能的语音和文本助手，可以执行自动化销售和客户支持工作。它支持多种语言，能够创建无代码的自定义工作流程，快速响应并提供个性化服务，适合初创企业和大型公司，提高工作效率。

![](img/8c7dd89376078cee1af6de4557305837.png)

# HuggingFace&Github

### Hallo2

贡献人：

https://fudan-generative-vision.github.io/hallo2/#/

Hallo2 是一种先进的潜在扩散生成模型，能够生成长达1小时的 4K 分辨率人像视频动画。它通过引入可调的文本标签增强可控性，并采用新颖的技术解决外观漂移和时间伪影问题，显著提升了生成内容的质量和多样性。

![](img/0881b98a24854423376202a8d2811b24.png)

### Emu3-Gen

贡献人：

https://huggingface.co/BAAI/Emu3-Gen

Emu3 是一款新型多模态模型，通过下一个标记预测进行训练，能够生成高质量的图像和视频，并具备强大的视觉-语言理解能力。它无需依赖 CLIP 或预训练的语言模型，展现出不错的生成与感知性能。

![](img/630917201c7e675e7daaf041bb13b371.png)

# 投融资

### One Zero融资1亿美元，致力于AI驱动的金融科技创新

贡献人：

https://techcrunch.com/2024/10/21/one-zero-the-ai-fintech-started-by-the-founder-of-mobileeye-is-raising-100m-say-sources/

由Mobileye创始人Amnon Shashua创立的金融科技公司One Zero正在筹集至少1亿美元的资金。这家AI驱动的金融科技公司旨在通过人工智能技术，提供零售银行服务，尤其是为普通用户提供类似高净值客户的私人银行服务。One Zero目前已经筹集了约2.42亿美元，2023年估值为3.2亿美元。此次融资后，估值预计将显著提升。

One Zero已在以色列获得银行执照，并建立了一个零售银行，其零售业务目前拥有约11万名客户。公司计划将这些零售业务的数据和经验用于训练其AI模型，以便向全球其他银行提供技术授权。此外，公司推出的核心产品是名为“Ella”的智能聊天机器人，旨在超越传统银行服务中的聊天机器人，提供更复杂和个性化的金融建议服务。

One Zero的主要投资者包括腾讯、OurCrowd和SBI Ventures。此次融资将用于进一步开发其AI技术，并扩展全球业务。

公司官网：https://www.onezerobank.com/en/

![](img/6bfac5395997cedbae6d8eef8334529e.png)

### Neysa完成3000万美元A轮融资，旨在与全球AI超大规模计算服务商竞争

https://techcrunch.com/2024/10/21/indias-neysa-bags-30m-to-compete-with-global-ai-hyperscalers/

印度初创公司Neysa近日宣布完成了3000万美元的A轮融资，旨在扩展其AI基础设施业务，进军全球AI市场。此次融资由NTTVC、Z47（前称Matrix Partners India）和Nexus Venture Partners共同领投。此前，Neysa在今年早些时候完成了2000万美元的种子轮融资。

Neysa由资深技术企业家Sharad Sanghi于2023年创立，提供基于AI的基础设施和平台服务。公司推出的旗舰平台Velocis于7月上线，提供按需计算基础设施。此次新融资将用于加强Neysa的基础设施建设、提升研发能力，并拓展市场。公司还计划推出开发者平台和推理即服务（Inference-as-a-Service），并致力于优化AI工作负载管理。

Neysa目前拥有约12位付费客户，并在印度运营多个大型概念验证项目。尽管目前客户主要集中在印度，Neysa计划在下一轮融资后进入全球市场。此外，公司计划通过债务融资以满足日益增长的GPU和基础设施需求。

公司官网：https://www.neysa.ai/

![](img/557b068b630295af80e2bd306b9c0dfe.png)

### DataCrunch完成1300万美元种子轮融资

https://techcrunch.com/2024/10/21/datacrunch-wants-to-be-europes-first-ai-cloud-hyperscaler-powered-by-renewable-energy/

DataCrunch，这家总部位于芬兰的初创公司，计划成为欧洲首批“AI计算”超大规模计算服务商之一，并以可再生能源为核心竞争优势。该公司提供GPU即服务（GPU-as-a-service），为AI处理任务提供算力支持。2024年10月，公司宣布完成1300万美元的种子轮融资，其中760万美元为股权融资，主要投资者包括byFounders、J12 Ventures和Aiven联合创始人Oskari Saarenmaa。其余540万美元来自LokalTapiola和Nordea的债务融资。

这种债务融资的模式使DataCrunch能够利用物理资产（如Nvidia的GPU）作为抵押，降低投资风险。这一轮融资使DataCrunch的总融资额达到了1800万美元，资金将用于扩大其基础设施，支持Nvidia最新的服务器和集群，并继续拓展其客户基础，其中包括索尼和OpenAI的研究人员。

DataCrunch的另一大卖点是其使用绿色能源的数据中心，位于芬兰和冰岛，后者长期依赖100%可再生能源。公司未来计划建设自有数据中心，并有可能在2025年进一步扩大融资以支持这一计划。

![](img/9613b7942e8429afefaf92d6f3c206b0.png)

# 学习

### RLOO论文笔记 & rloo_trainer部分源码阅读

https://zhuanlan.zhihu.com/p/1073444997

本文从RLOO论文出发，讨论了强化学习在大语言模型（LLM）中的应用，特别是针对人类反馈优化（RLHF）的方法。RLOO的核心思想是将完整生成视为单一action，而不是逐个token进行建模，这减少了训练复杂性，并提升了效率。相比于PPO，RLOO基于REINFORCE算法，能够使用多个online samples，实验显示在所有数据集和模型上均优于PPO、DPO和RAFT。

RLOO的主要技术贡献包括：通过完整生成而非部分序列的建模，简化了RL阶段的复杂性；多样本策略有效减少了方差，且在面对噪声和KL惩罚时表现出更好的鲁棒性。PPO通过部分序列建模引入了较大的方差和复杂性，而REINFORCE则避免了这些问题。代码实现细节进一步展示了RLOO如何通过Leave-One-Out的方式进行无偏的方差减少，使得训练更高效。

实验证明，RLOO在奖励优化和采样效率方面显著优于PPO和RAFT，并且避免了PPO中对低方差估计器和剪裁机制的过度依赖。

![](img/ab7f1bbfc1001903d954443e2735e893.png)

### ISSCC 2024智能芯片研究趋势分析

https://zhuanlan.zhihu.com/p/712712200

1.  面[向生成式AI](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=NVIDIA&zhida_source=entity)的智能芯片：该主题涉及生成式AI大模型推理和训练的智能芯片[，可以算是](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=%E5%A4%A7%E6%A8%A1%E5%9E%8B%E8%AE%A1%E7%AE%97&zhida_source=entity)竞争最激烈的赛道之一。除了学术论文以外，来自NVIDIA的大会主题报告“Computing in the Era of Generative AI”，以及大模型计算论坛，都反映出这一主题的火热程度。

1.  [存算一体](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=%E5%AD%98%E7%AE%97%E4%B8%80%E4%BD%93&zhida_source=entity)[从电路](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=%E5%AD%98%E5%82%A8%E5%A2%99&zhida_source=entity)到系统的创新：存算一体被认为是大模型时[代突破](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=%E5%AE%8F%E5%8D%95%E5%85%83&zhida_source=entity)[“存储](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=%E6%B7%B7%E5%90%88%E5%9F%9F&zhida_source=entity)墙”瓶颈的关键技术。存算一体技术在ISSCC上已流行多年，大多聚焦在模拟域高能效宏单元设计。如今能看到大家在高算力、高精度场景下的新思考：电路层面有数字域、混合域等宏单元电路创新，也有更多工作认可我们的浮点存算一体技术；系统层面有很多研究在探索如何讲存算一体宏单元应用于大规模系统。值得注意的是，联发科与台积电合作开发了一款3nm数字存算一体视觉增强引擎。

1.  [嵌入式视觉](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=%E5%B5%8C%E5%85%A5%E5%BC%8F%E8%A7%86%E8%A7%89&zhida_source=entity)DSA芯片：随着AI技术的发展，[人们开始](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=%E5%85%B7%E8%BA%AB%E6%99%BA%E8%83%BD&zhida_source=entity)畅想能够处理复杂任务的智能体。“具身智能”这一名词学术界与业界兴起。视觉是机器人与环境交互的主要模态，因此我们看到今年涌现出面向神经辐射场（NeRF）、即时定位与地图构建（SLAM）等视觉任务的DSA芯片。

1.  优化求解器DSA芯片：优化[问题求解](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=%E9%97%AE%E9%A2%98%E6%B1%82%E8%A7%A3&zhida_source=entity)[器是不同](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=%E6%99%BA%E8%83%BD%E8%AE%A1%E7%AE%97&zhida_source=entity)[于数据](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=%E4%BC%8A%E8%BE%9B%E6%9C%BA&zhida_source=entity)驱[动-AI模型的另](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=%E5%B8%83%E5%B0%94%E5%8F%AF%E6%BB%A1%E8%B6%B3%E6%80%A7%E9%97%AE%E9%A2%98&zhida_source=entity)一[种风格的智](https://zhida.zhihu.com/search?content_id=246511038&content_type=Article&match_order=1&q=%E5%81%8F%E5%BE%AE%E5%88%86%E6%96%B9%E7%A8%8B&zhida_source=entity)能计算。其数学可解释性在我私心看来是很优雅的。今年ISSCC上可以看到针对伊辛机、布尔可满足性问题和偏微分方程等优化问题求解器的DSA芯片。

![](img/c7de70b93d1c57e9338a7ffcb9ec3ba4.png)

### 迈向复现 OpenAI o1 的一小步：Steiner 开源模型阶段性进展报告

https://zhuanlan.zhihu.com/p/2340089725

本文分享了季逸超（Yichao 'Peak' Ji）为复现 OpenAI o1 所做的工作，Steiner 模型是这一过程中的实验产物。Steiner 模型采用了强化学习，通过推理路径的自回归生成、多路径尝试、回溯验证等技术，实现了隐含搜索树的线性遍历。不同于传统的选择题评估方法，Steiner 旨在应对开放式推理任务，并通过公开模型来获得真实用户的反馈。

技术上，Steiner 的推理过程不依赖 Chain of Thought (CoT) 提示词或 agent 框架，能够产生高质量的零样本推理结果，但与 OpenAI o1 的推理时扩展能力仍存在差距。模型训练分为三个阶段：持续预训练 (CPT)、监督微调 (SFT)、以及基于步骤奖励的强化学习 (RL)。尽管实验表明 Steiner 目前在某些基准测试中仍未达到 o1 的水平，且额外的推理步骤并未带来预期的性能提升，Steiner 仍然展现了推理路径生成和探索能力的潜力。

本文探讨了推理数据生成的挑战及启发式奖励机制，分析了 Steiner 在推理步数增加情况下性能下降的可能原因，并提出了下一步工作重点，如模型的输出质量、奖励设计和推理深度平衡等。

![](img/e0fe203ad185558b8ec08359deced546.png)

### 把Contrastive Loss的Batch Size冲到100M

https://zhuanlan.zhihu.com/p/1681887214

本文介绍了一种名为 Inf-CL 的对比损失（Contrastive Loss）实现方案，通过分块计算策略，使得对比损失的显存消耗从二次增长转变为线性增长，成功将 batch size 扩展到 12M，并显著降低了显存占用和时间开销。这一方法突破了“显存瓶颈”的传统认知，为对比学习相关领域带来了 batch size 的大规模扩展可能。

技术细节：

1.  **显存优化策略**：

+   传统对比损失中，相似度矩阵的存储复杂度为 O(b2)O(b^2)O(b2)，导致随着 batch size 增加显存需求迅速上升。

+   Inf-CL 引入了**分块计算策略**，避免了完整相似度矩阵的实例化，通过逐块计算局部 LSE（Log-Sum-Exp）值，将显存复杂度降至 O(b)O(b)O(b)，大幅减少显存消耗。

1.  **分块与累加策略**：

+   Inf-CL 将相似度矩阵按行和列分块，逐块计算每列的 LSE 值，并在行方向上进行串行累加，避免了将整个相似度矩阵存储于显存中。

+   该策略同样适用于反向传播，在梯度计算中也采用分块累积的方式，进一步降低显存需求。

1.  **多层级分块加速**：

+   Inf-CL 引入了跨 GPU 和单 GPU 的分块策略，通过并行计算和异步通信来优化跨 GPU 的列数据同步，提升了计算效率。

+   单 GPU 分块中采用更细粒度的分块，将计算任务分配给 CUDA 核心，减少高带宽内存（HBM）与片上内存（SRAM）之间的 I/O 操作，进一步提升性能。

1.  **数值稳定性与溢出防护**：

+   为防止溢出，Inf-CL 使用行最大值作为调整因子，确保数值计算的稳定性，并通过融合操作减少不必要的数据存取，大大优化了算子性能。

实验结果：

+   **显存消耗**：在 1024k 的 batch size 下，Inf-CL 显存需求比 CLIP 和 OpenCLIP 减少了 281 倍，支持在单台 A800 上实现 12M batch size 的训练。

+   **训练速度**：尽管 Inf-CL 增加了计算复杂度，但通过分块策略并行化处理，整体速度仅有极少的额外开销，并实现了 batch size 扩展下的线性时间增长。

+   **精度**：实验结果表明，Inf-CL 与传统对比损失方法在精度上几乎无误差，保持了高效的训练效果。

![](img/94974eadb0c530fd0453515599ae0479.png)

### Janus: 解耦视觉编码，引领多模态理解与生成统一的新范式

https://zhuanlan.zhihu.com/p/2360185063

Janus 是一种基于自回归的多模态理解与生成统一模型，核心技术为解耦视觉编码器。Janus 使用独立的视觉编码器分别处理多模态理解和生成任务，以避免任务冲突，提升模型性能。通过这一设计，Janus 解决了此前统一模型在处理不同任务时存在的性能瓶颈问题，尤其在高层语义理解与细粒度视觉信息传递方面，实现了突破。

1.  **解耦视觉编码器**：

+   **多模态理解**使用 SigLIP-Large 进行视觉编码，注重高层语义提取。

+   **视觉生成**使用 LlamaGen 的 VQ Tokenizer，保留细粒度的图像细节。

+   这两个编码器的输出通过独立适配器输入到统一的 Transformer，模型整体遵循自回归的 Next-Token-Prediction 训练方式。

1.  **训练流程**：

+   训练分为三个阶段：图像描述、联合预训练、指令跟随训练。Janus 使用 1:1 的多模态理解与视觉生成数据比率。

+   推理过程中，使用了常见的优化技术如 KV Cache 和 classifier-free guidance，确保高效推理和生成。

1.  **实验结果**：

+   Janus 在多模态理解任务中，超越了同规模的统一模型，并在 POPE、MMBench 等基准测试中取得了优异成绩。

+   视觉生成任务中，Janus 在 MSCOCO-30K 和 GenEval 等数据集上表现出色，甚至超越了部分专用生成模型如 SDXL。

+   消融实验表明，解耦编码器显著提升了多模态理解性能，且在视觉生成方面表现也不逊色于专用生成模型。

1.  **模型扩展性**：

+   Janus 设计灵活，可替换更强的视觉编码器或 tokenizer，如 EVA-CLIP 或 MoVQGan，支持更多模态如视频、3D 数据等。

![](img/8832bbb0fb83a91cd3e8e76cc0959ea7.png)

### 如何用一个统一的视角，分析RLHF下的各种算法？

https://mp.weixin.qq.com/s/2txfqHpyiW-ipKuQSWAsLA

在探讨DPO局限性的过程中，作者从RLHF（基于人类反馈的强化学习）的最初优化目标出发，分析了该目标下的隐藏前提。这些前提决定了RLHF与客观世界真值之间的误差，并解释了不同RLHF实现算法（如DPO和PPO）之间的差异。作者通过总结RLHF中奖励函数、数据采集与分布等关键因素，阐明了现有算法在理论设计或实际操作中如何违背这些前提，并提出了改进思路。

首先，RLHF的优化目标基于一个固定的奖励函数，假设它具备完美的泛化能力。然而，现实中，训练数据往往不完美，无法准确反映人类的复杂偏好。这导致了优化过程中误差的产生，尤其在奖励函数的评估性能上存在偏差。此外，KL散度用于衡量模型与真值分布之间的相似度，但其优化往往受到数据分布和奖励函数泛化能力的限制。

在算法分析部分，作者重点比较了DPO和PPO。DPO通过偏好数据的离线采样进行优化，属于off-policy方法。它依赖于观测数据来拟合最优分布，但由于训练数据往往偏离目标分布，容易导致模型无法准确学习真值分布。为此，作者提出了通过拒绝采样（rejection sampling）来提高数据采样效率的改进方法，从而减少训练偏差。

相比之下，PPO是一种on-policy方法，能够通过自我验证逐步逼近真值分布，因此比DPO更符合RLHF的原始优化目标。然而，PPO依赖于固定的奖励函数进行探索，当奖励函数性能不佳时，策略模型可能获得错误的反馈信号，从而影响最终优化效果。

作者还进一步探讨了online + on-policy训练的可能性，即在训练过程中同时优化策略模型和奖励模型。这种方法通过不断更新奖励函数来减少误差，但面临标注资源有限的问题，因此需要选择不确定性高的数据点进行重新标注和评估。

文章最后强调，RLHF算法在优化过程中需要有效控制数据分布、奖励函数的泛化能力，并通过结合on-policy和online方法来逐步缩小与最优目标的差距。

![](img/04f7ac5aa6cce1a697c551c7a57f30cf.png)

### 企业级 RAG 全链路优化关键技术

https://mp.weixin.qq.com/s/qoCBaKxyXGnk6p74sXgXNA

企业级RAG关键链路 RAG链路分为离线和在线两部分，离线主要处理文档解析、向量化和混合检索，在线则处理查询理解、Rerank及大模型生成。RAG的核心挑战包括效果、性能和成本，尤其是性能瓶颈和GPU资源的高成本问题。

企业级RAG效果优化 文档解析与文本切片：阿里云优化了文档解析服务，支持多种格式的文档，包括结构化和非结构化数据。文本切片方法则包含层次切片和语义切片，进一步提升了解析和向量化效果。 混合检索与NL2SQL：阿里云开发了统一向量化模型，能同时生成稠密和稀疏向量，实现更高效的混合检索。此外，RAG还支持通过自然语言转换为SQL语句，进行精确查询。 大模型Rerank和微调：RAG采用基于Qwen模型的Rerank技术，显著提升检索结果的质量，并通过大模型微调降低幻觉率，优化生成效果。

企业级RAG性能优化 通过引入GPU加速和大模型量化，RAG实现了大规模推理的性能提升，14B参数的Qwen模型可在1-3秒内生成200 token的答案。在成本方面，阿里云使用了LoRA技术，通过单卡或多卡微调，将微调成本降低40倍。

企业级RAG应用实践 阿里云的AI搜索开放平台支持多种微服务，如文档解析、向量化和LLM推理。用户可以通过SDK和API调用这些服务，开发多模态搜索、RAG问答等场景。阿里云还推出了OpenSearch LLM版，帮助用户快速搭建RAG服务，支持表格问答和推荐功能。

![](img/ce62720ef55fb2a306b9a2f974f71f8b.png)

# 招聘公告

---

![](img/ce81a00bef5bc11edc515cf1b03a41e0.png)

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

![](img/779f9579f4148c06d4bd83a98e70850e.png)

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

![](img/e96fbae234856c5dbc4f1021a4c4b48f.png)