# 奇绩大模型日报（10月 23日）

> 来源：[https://miracleplus.feishu.cn/docx/GG3Vd0V8KoNSBExO4uXcmGQnnBh](https://miracleplus.feishu.cn/docx/GG3Vd0V8KoNSBExO4uXcmGQnnBh)

# 🔉潜空间活动报名

> 本期活动将在**11月9日** **10:00**开始，我们邀请到的嘉宾是**鱼哲**，Le**pton AI 创始成员，曾在阿里云担任高性能 AI** 平台产品负责人，专注于 AI 在多个行业的落地及应用。Lepton AI 致力于建立高效可用的AI 基础设施，让团队更关注于应用构建及落地。在本次分享中鱼哲将带来关于AI产品相关的思考，分享主题《Beyond Infra, what matters?—— 不同AI产品形态对团队的挑战》。

> 除嘉宾分享外，每期设置了【匹配合伙人 Cofounder Matching】环节。你可以和 GenAI 时代最有活力的创业者和研究者线下面对面交流，将有机会找到志同道合、有共同创业梦想的小伙伴。

> 报名通道已开启，欢迎扫描下方二维码报名。

![](img/acdf1a8b9b4690b528c72a032b5013dc.png)

#

# 资讯

### OpenAI 任命首位首席经济学家

https://techcrunch.com/2024/10/22/openai-hires-its-first-chief-economist/

OpenAI 近日聘请了其首位首席经济学家——Aaron Chatterji。Chatterji 曾在拜登政府时期担任美国商务部的首席经济学家，并在奥巴马总统的经济顾问委员会中担任高级经济学家。目前，他还是杜克大学商学院和公共政策的教授。在 OpenAI，他将研究人工智能（AI）对经济的影响，带领团队探讨 AI 如何影响经济增长和就业前景等问题。

Chatterji 在协助拜登政府实施2022年《芯片法案》（CHIPS Act）方面发挥了重要作用，该法案为美国计算机芯片的发展提供了约2800亿美元的资金支持。Chatterji 对该项目的深入了解以及他在政界的广泛人脉，可能会为 OpenAI 未来在芯片设计领域的探索提供帮助。

此外，OpenAI 当日还任命了另一位重要人物——Scott Schools，曾担任美国司法部副助理检察长和Uber合规部门负责人。Schools 将担任 OpenAI 的首席合规官，负责与公司董事会及各团队合作，确保 OpenAI 遵守法律法规并维持伦理标准。

![](img/0773e81881b93bc727eafbc5da975346.png)

### Anthropic 推出新款 Claude 3.5 Sonnet AI 模型，可控制PC操作

https://techcrunch.com/2024/10/22/anthropics-new-ai-can-control-your-pc/

Anthropic 最近发布了升级版的 Claude 3.5 Sonnet 模型，其核心新功能是能够通过“Computer Use” API 与桌面应用程序交互。该模型可以模拟用户的键盘输入、鼠标点击和光标移动，实际上模拟了人在电脑前的操作。此功能现已开放测试，开发者可以通过 Anthropic API、Amazon Bedrock 和 Google Cloud 的 Vertex AI 进行尝试。

**技术细节与创新：**

1.  **屏幕交互**：Claude 3.5 Sonnet 能够通过屏幕截图分析用户界面，计算鼠标光标移动的像素数来执行点击等操作。它还支持使用软件工具完成复杂任务，如处理数据、填写表单等。

1.  **性能提升**：新版本带来多项性能优化，包括改进的指令执行能力和工具使用精度，尤其是在编码任务上的表现优于 OpenAI 的 GPT-4o。尽管模型并未专门针对这些任务进行训练，但它具备自我纠正和重试的能力，能处理需要多步骤的复杂任务。

1.  **局限性**：虽然 Claude 3.5 Sonnet 能够执行多种桌面级命令，但在实际操作中仍存在一定的缺陷，例如对滚动和缩放操作的处理不佳，并且容易遗漏一些短暂的操作提示。此外，操作速度较慢且易出错，Anthropic 建议开发者从低风险任务入手。

**安全与风险：** Anthropic 意识到这一功能可能带来的安全风险。尽管模型没有在训练期间访问过用户的屏幕截图和网络内容，Anthropic 采取了措施来防止滥用，包括为模型配置高风险操作分类器，避免其与社交媒体或政府网站互动。此外，所有通过 Computer Use API 生成的截图将至少保留 30 天，供开发者和安全团队参考。公司表示，在必要时可以限制模型的访问权限，以防止垃圾邮件、欺诈和错误信息传播。

**市场应用与未来方向：** 早期版本的 Claude 3.5 Sonnet 已被 Replit 用于自动化应用程序验证，Canva 也在探索该模型对设计和编辑过程的支持能力。此外，Anthropic 还计划推出更经济的 Claude 3.5 Haiku 版本，旨在以较低成本提供更高效的工具使用和定制服务。

![](img/8a6a1b14be64a6d38951173c0d01f726.png)

### **Stability 宣布其最新的 Stable Dif**fusion 模型可生成更“多样化”图像

https://stability.ai/stable-image

在经历了一系列技术问题和许可变更的争议之后，AI 初创公司 Stability AI 宣布了其最新的图像生成模型家族——Stable Diffusion 3.5 系列。公司声称，该系列比之前的技术更具定制性和多功能性，且性能更强。

Stable Diffusion 3.5 系列包含三个模型：

+   Stable Diffusion 3.5 Large：拥有 80 亿参数，是该系列最强大的模型，能够生成分辨率高达 1 百万像素的图像。

+   Stable Diffusion 3.5 Large Turbo：这是 3.5 Large 的蒸馏版本，虽然生成速度更快，但图像质量有所降低。

+   Stable Diffusion 3.5 Medium：优化于手机和笔记本等边缘设备，支持生成分辨率为 0.25 至 2 百万像素的图像。

目前，Stable Diffusion 3.5 Large 和 3.5 Large Turbo 已上线，而 3.5 Medium 将于 10 月 29 日发布。

Stability 表示，3.5 系列模型可以生成更“多样化”的输出，即无需复杂提示就能生成具有不同肤色和特征的人物图像。首席技术官 Hanno Basse 透露，在训练过程中，使用多个版本的提示对每张图像进行标注，并优先处理简短提示，从而确保对给定文本描述的图像概念有更广泛和多样的分布。

尽管其他公司在过去曾尝试在图像生成器中引入类似的“多样化”功能，但结果往往导致社交媒体上的负面反响。比如，谷歌的 Gemini 聊天机器人早期版本在生成历史人物时，常常出现不符合历史背景的角色，导致该功能被暂停了近六个月。

Stability 也承认，虽然新版本在多样性上有所提升，但与早期模型一样，3.5 系列在提示响应方面可能仍会存在问题，特别是那些缺乏特异性的提示会导致输出的美学水平有所波动。

此外，Stability 保留了其先前模型的许可模式：模型可以免费用于“非商业”用途，收入少于 100 万美元的企业可以自由使用，而超过此金额的公司则需要获得企业许可。

Stability 还计划在未来几天发布 ControlNet 模型，允许用户进行微调和自定义，同时它还提供了多种第三方平台的 API 接口支持。

尽管 Stable Diffusion 3.5 系列在版权方面面临挑战，但公司允许数据所有者请求从训练数据集中删除其内容，并强调采取了合理措施防止模型被滥用于误导性内容。

![](img/909e52dd5a2f8ff76c29f0ca8d699604.png)

### Mochi -10B 视频生成开源模型

https://www.genmo.ai/blog

Mochi 1是一款最新的开源视频生成模型，展现了高质量的运动表现和极强的文本提示响应能力，极大缩小了开源与封闭视频生成系统的差距。该模型基于Apache 2.0许可开放，初期版本支持480p分辨率的视频生成，未来将推出720p高清版本。Mochi 1在运动质量和提示响应度上都达到了业界最前沿水平，生成的视频帧率为30fps，长度可达5.4秒，具有高时间一致性和逼真的物理模拟效果，如流体、毛发等动态表现。模型采用了新颖的非对称扩散Transformer架构（AsymmDiT），通过多模态自注意力机制联合处理文本与视频编码，视频分辨率压缩至128倍，并应用RoPE三维位置嵌入。

Mochi 1基于10亿参数的扩散模型，是目前最大的视频生成开源模型，支持长时段视频上下文推理，具备SwiGLU前馈层和查询-键归一化等技术优化。与传统多语言模型联合的提示编码不同，Mochi 1仅依赖单一的T5-XXL模型处理文本。虽然当前版本在极端运动场景中偶有失真，但其未来版本将进一步提高生成分辨率并改善可控性。同时，Mochi 1支持开放研究和商业应用，为娱乐、教育、机器人等领域提供创新的解决方案。

![](img/9aab4ed12ce2e46d8cf4fd095fbef003.png)

# 推特

### Runway推出Act-One：不需要动作捕捉或绑定，通过一个驱动视频和角色图像生成富有表现力的角色表演

贡献人：

https://x.com/runwayml/status/1848785907723473001

介绍 Act-One。在 Gen-3 Alpha 中，通过一个驱动视频和角色图像生成富有表现力的角色表演。不需要动作捕捉或绑定。

Act-One 让你能够忠实地捕捉演员表演的精髓，并将其应用到生成过程中。传统的面部动画流程复杂，而 Act-One 只需一个简单的驱动视频，甚至可以用手机拍摄。

无需动作捕捉或角色绑定，Act-One 能够将单一输入视频的表演应用到不同的角色设计和风格中。

该模型可以在多种摄像机角度和焦距下生成电影感和逼真的效果，让角色的情感表现更加丰富，带来全新创作可能。

通过 Act-One，视线、微表情、节奏和表达都能在最终输出中忠实呈现。

Act-One 将逐步开放使用，并很快向所有人提供。

![](img/b246770e4e96c3b5b36af5a0e2dab1d6.png)

[ssstwitter.com_1729622125604.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UUkmbmMbSovkjtxsVNEc3BClnCh)

### Perplexity推出Pro Search推理模式：激发你的好奇心，提出多层次的问题

贡献人：

https://x.com/perplexity_ai/status/1848800407071690755

Pro Search 现在更强大了，推出了推理模式！

激发你的好奇心，提出多层次的问题。Perplexity 会根据你的问题进行适应。

亲自试试看吧！（示例查询在下方👇）

![](img/c7c8ca0806b8e39e8f10705bcb885e6b.png)

[ssstwitter.com_1729628918423.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HOvpbar68omyOkxIjqWcm21bnJd)

### Ideogram Canvas：无限的创意画板，用于组织、生成、编辑和组合图像

贡献人：

https://x.com/ideogram_ai/status/1848757699606983143

今天，我们推出了 Ideogram Canvas，一个无限的创意画板，用于组织、生成、编辑和组合图像。

将你的头像或品牌视觉导入 Ideogram Canvas，使用业内领先的 Magic Fill 和 Extend 功能，将它们与创意、AI 生成的内容融合在一起。

![](img/5d1e25e79d95b98d5889800477803a70.png)

[ssstwitter.com_1729620861521.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/N9lKb4SyHotavqxI0WUckWcAnVd)

### Dreamcut AI：AI驱动的视频编辑与屏幕录制，在任何设备上轻松编辑视频

贡献人：

https://x.com/MengTo/status/1848669694800367901

介绍 http://dreamcut.ai

这是我用 Claude AI 从零开始构建的视频编辑器。耗时 3 个月，写了超过 5 万行代码。我完全跳过了设计阶段，直接开始编写代码。

目前处于早期测试阶段。你可以注册一个免费账户，试用 AI 工具。

---

AI驱动的视频编辑与屏幕录制

DreamCut 是一个全新的 AI 视频编辑器和屏幕录制工具，直接在浏览器中运行。你可以在任何设备上轻松编辑视频，借助云端强大的 AI 功能提升效率。通过 AI 语音配音、智能聊天助手、即时笔记和惊艳的图像/视频生成功能，为你的内容增添魅力，令观众惊叹。

![](img/5663bcc717ca9522eb3b7ec410bdb6c4.png)

[ssstwitter.com_1729671251002.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LXElbWRy1obvKTxuzHLchNFzn16)

### Replit主席分享：Claude 计算机使用功能接入 Replit 的 Agent效果惊人

贡献人：

https://x.com/pirroh/status/1848752337080488177

我已经不记得上一次我对一个新的 AI 功能如此激动是什么时候了。

我们将 Claude 的计算机使用功能接入了 @Replit 的 Agent，作为人工反馈的替代品。结果……它就这么直接起作用了！我觉得不久之后，我们的 Agent 就会实现完全自主。

![](img/7b01fbb8dd440e8a6d5ad80547c19154.png)

[ssstwitter.com_1729671123010.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/XnnXb0HWkoLOVMx8rg2cC7ALn3b)

# 信号

## Adaptive Data Optimization: Dynamic Sample Selection with Scaling Laws

贡献人：

原文链接：https://arxiv.org/abs/2410.11820v1

推文链接：https://x.com/SadhikaMalladi/status/1848696629765018103

研究节点：模型体系-数据体系

目前并没有一个标准指南来指导如何在有限的计算预算下分配不同数据源。大多数现有方法要么依赖于对较小模型的广泛实验，要么通过需要Proxy模型的动态数据调整，这两者都显著增加了工作流程的复杂性和计算开销。本篇论文引入了一种名为Adaptive Data Optimization（ADO）的算法，该算法在模型训练过程中在线优化数据分布。ADO使用每个领域的Scaling Law来估计 该领域在训练过程中的学习潜力，并相应调整数据组合，使其更具可扩展性并更易于集成。

![](img/5ed0976d27a13fdf68e206efa0bc64df.png)

## Fact, Fetch, and Reason: A Unified Evaluation of Retrieval-Augmented Generation

贡献人：

原文链接：https://huggingface.co/papers/2409.12941

推文链接：https://x.com/SatyaScribbles/status/1848721681709498480

研究节点：开发体系-RAG；模型体系-评估体系

大型语言模型（LLMs）在各种认知任务中表现出显著的性能提升。一个新兴的应用是利用 LLMs RAG 的能力。这些系统要求LLMs理解用户查询，检索相关信息，并生成连贯且准确的回应。鉴于此类系统在现实世界中的应用日益增加，全面的评估变得至关重要。为此，作者提出了FRAMES（Factuality, Retrieval, And reasoning MEasurement Set），这是一个高质量的评估数据集，旨在测试LLMs提供事实性回应的能力、评估其检索能力以及生成最终答案所需的推理能力。虽然之前的研究提供了评估这些能力的单独数据集和基准，FRAMES则提供了一个统一的框架，可以更清晰地展示LLMs在端到端RAG场景中的表现。

![](img/e202dfdb58a7dc9f7a99300fdea4111a.png)

## A Watermark for Black-Box Language Models

贡献者：

原文链接：https://arxiv.org/abs/2410.02099

推文链接：https://x.com/johnwieting2/status/1848795087477592213

研究节点：模型体系-评估体系

Watermarking技术最近作为一种有效策略，用于检测大型语言模型（LLMs）的输出。大多数现有方案需要对模型的下一个词的概率分布进行白盒访问，而这一分布通常无法为LLM API的下游用户所获取。在这项工作中，作者提出了一种 principled watermarking 方案，该方案只需要能够从LLM中采样片段，并且可以使用多个secret keys进行串联或嵌套。

![](img/395aca79a26957eab0e5e7eb7f2707fc.png)

## Navigating the Digital World as Humans Do: Universal Visual Grounding for GUI Agents

贡献人：

原文链接：https://arxiv.org/abs/2410.05243

推文链接：https://x.com/ysu_nlp/status/1848766660183818448

研究节点：模型体系-多模态

多模态大型语言模型（MLLMs）正在改变图形用户界面（GUI）agents 的能力，推动其从受控模拟向跨多个平台的复杂现实世界应用过渡。然而，这些代理的有效性依赖于其稳健的基础能力。目前的 GUI agents 主要使用基于文本的表示形式，例如HTML或辅助功能树，尽管这些表示形式有其用处，但往往会引入噪声、不完整性以及增加计算开销。在本文中，作者主张GUI agents 采用类似人类的方式，通过完全视觉化感知环境并直接在GUI上进行像素级操作。关键在于视觉基础模型，这些模型能够准确地将GUI元素的各种指代表达映射到不同平台上GUI的坐标。作者展示了一种简单的方案，包括基于网页的合成数据和对LLaVA架构的轻微调整，这对训练这样的视觉基础模型非常有效。我们收集了迄今为止最大的GUI视觉基础数据集，包含1000万个GUI元素及其指代表达，覆盖了130多万张截图，并使用该数据集训练了UGround，一种强大的通用GUI代理视觉基础模型。

![](img/f54f5aac2358dad2b367f7027037821b.png)

# 产品

### Averi

贡献人：

https://www.averi.ai/

Averi 是一款 AI 驱动的营销管理平台，帮助企业快速制定营销策略、创建内容并组建团队。它结合了基于公司数据的智能营销工具和经过审核的营销专家，提供高效的协作环境，用户可以免费使用基础计划，提升营销效率。

![](img/5ed2852517215bf8777804ba52036294.png)

### Capgo

贡献人：

https://capgo.ai/

Capgo 是一个用于构建和管理微服务架构的工具，可以在简化服务的部署、监控和管理，提供服务发现、负载均衡、监控日志记录及弹性容错等功能，以提高系统的可扩展性和可靠性。

![](img/2d92dba929589b132bfe1e8d9d6226c0.png)

# HuggingFace&Github

### AutorizePro

贡献人：

https://github.com/sule01u/AutorizePro

AutorizePro 是一款专注于越权检测的 Burp 插件。它基于 Autorize 插件进行二次开发，通过增加 AI 分析模块和优化检测逻辑，大幅降低了误报率，提升了越权漏洞检出效率。

![](img/1115d38ac7f1ea82f0d13c2ef2ef4747.png)

### Meta Lingua

贡献人：

https://github.com/facebookresearch/lingua

Meta Lingua 是一个轻量级、高效和易于修改的 PyTorch LLM 训练库，可以为 LLM 研究提供一个简单易用的框架。

![](img/3092d2d095aef7737b2991e79e00cf91.png)

# 投融资

### CrewAI 利用第三方模型实现业务任务自动化

贡献人：

https://techcrunch.com/2024/10/22/crewai-uses-third-party-models-to-automate-business-tasks/

CrewAI 是 João Moura 创立的一家公司，旨在通过自动化来简化重复的后台任务，如报告总结和员工入职流程。与传统的机器人流程自动化（RPA）不同，CrewAI 通过集成 OpenAI 和 Anthropic 等第三方模型，实现更灵活、智能的工作流自动化。用户可以通过 CrewAI 平台构建自动化流程，并从仪表盘上监控它们的执行情况。

CrewAI 的亮点在于其灵活的 AI 代理体系，允许用户根据不同的业务需求选择最佳的 AI 模型，并且能够与超过1000个应用程序进行集成，确保数据隐私的同时提升业务效率。Moura 强调，CrewAI 提供的技术比 RPA 更具弹性，尽管 AI 也可能会出现“幻觉”或偏差问题，但整体上比传统的基于固定规则的 RPA 系统更稳定。

在融资方面，CrewAI 已经通过种子轮和A轮融资共筹集了1800万美元，投资方包括 Boldstart Ventures、Craft Ventures、Earl Grey Capital、Insight Partners 等知名投资机构。此外，Coursera 联合创始人及 AI 企业家 Andrew Ng 和 HubSpot 的联合创始人兼CTO Dharmesh Shah 也参与了投资。目前，CrewAI 的估值已达1亿美元，客户数量在公司成立的第一年内已达到150个。

CrewAI 计划将这笔资金用于扩大其核心自动化产品，并扩充目前16人的团队。随着业务增长，公司有望在明年夏天实现正向现金流。

公司官网：https://www.crewai.com/

![](img/0126ec7d9b794af85674708249a241ac.png)

### Highlight AI 助手独立融资1000万美元

https://techcrunch.com/2024/10/22/desktop-ai-assistant-app-highlight-spins-out-of-medal-with-10m-in-funding/

今年早些时候，因游戏视频剪辑功能闻名的初创公司Medal推出了跨平台AI助手应用Highlight。现Medal将Highlight拆分为一个独立实体，并成功完成了一轮融资，募集资金1000万美元。

本轮融资由General Catalyst领投，Valor、SV Angel和Conviction Embed参与投资。同时，Medal在7月份筹集的1300万美元中，也向Highlight注资了300万美元。

Medal的联合创始人Pim de Witte是Highlight与Medal之间的唯一联系，Highlight现有独立的团队，包括来自ClickUp的前设计副总裁Haris Butt、Medal联合创始人Josh Lipson以及Medal的首位增长负责人Mark Bond。

Highlight是一款跨平台的桌面应用，用户可以上传屏幕、语音笔记或文档作为上下文，并通过大语言模型（LLM）进行提问。该应用还支持会议音频转录，方便用户在会后查询相关内容。Highlight的功能包括总结、重写、突出显示和解释上下文等常见的AI助手操作。

公司计划未来通过按处理字数收费的模式进行商业化，目前Highlight仍免费使用。

公司官网：https://highlightai.com/

![](img/60466e2fdcaff9d5d555accdad32aa63.png)

### Interface.ai 融资3000万美元助力银行处理客户请求

https://techcrunch.com/2024/10/22/interface-ai-raises-30m-to-help-banks-field-customer-requests/

金融机构客户自动化平台Interface.ai近日宣布完成了一轮3000万美元的融资，由Avataar Venture Partners领投。其中2000万美元为股权融资，另外1000万美元为债务融资。这是Interface首次引入外部资本，此前公司完全依靠自筹资金运营。

Interface.ai的CEO兼联合创始人Srinivas Njay表示，该平台已被北美超过100家金融机构信赖，每天处理数百万次客户互动，并且已实现数千万美元的年经常性收入。Njay的创业灵感来自他父亲在印度经营的一家信用社。此前，Njay曾在微软担任Bing广告团队的产品经理，并在EA负责移动游戏的产品管理。

Interface.ai 的核心产品是一套基于语音和文本的AI代理，专为处理银行客户服务请求设计。这些AI代理经过专门训练，能够完成诸如修改抵押贷款付款和开设新账户等任务。银行可以通过调整内部数据进一步定制这些代理，并根据之前的聊天内容进行产品推荐。

此次融资的3000万美元将用于扩展Interface在北美和印度的120人团队，并加速市场推广计划。

公司官网：https://interface.ai/

![](img/34fbfd08d978315a726a72c3c87c09ca.png)

### Socket 融资4000万美元加强软件供应链安全

https://techcrunch.com/2024/10/22/socket-lands-a-fresh-40m-to-scan-software-for-security-flaws/

初创公司Socket专注于检测开源代码中的安全漏洞，最近完成了一轮4000万美元的融资，以应对日益严重的软件供应链安全问题。这轮融资由Elad Gil和Andreessen Horowitz领投，参与的投资者还包括雅虎联合创始人Jerry Yang、OpenAI董事长Bret Taylor、Twilio联合创始人Jeff Lawson和Shopify的联合创始人兼CEO Tobias Lütke。

Socket的创始人兼CEO Feross Aboukhadijeh指出，传统的安全工具无法有效解决现代软件开发中的依赖性问题，尤其是开源软件组件的依赖关系，这些依赖关系极易带来安全隐患。Socket的解决方案是通过扫描工具检测开源组件中的恶意活动，例如后门程序和模糊代码，并在更新或添加依赖项时及时向开发者发出警报。

Socket目前拥有超过100个客户，保护7500多个组织的300,000个代码库，支持全球超过100万名开发者。其客户包括Anthropic、Harvey、Figma、Vercel和美国四大银行之一。此外，公司每周检测超过100起零日软件供应链攻击，远超同类竞争对手。

Socket还整合了OpenAI和Anthropic的生成式AI API，能够生成漏洞摘要，并检查开源代码的许可证合规性。Aboukhadijeh称，本轮融资是“抢先进行的”，因为公司尚未花完去年8月A轮融资的资金。这次新融资将使Socket继续扩大规模，特别是在AI生成代码引入更多安全漏洞的情况下，Socket的技术填补了市场空缺。

此次融资使Socket的总融资额达到6500万美元，公司计划在年底前将员工人数从32人增加到50人，主要加强工程、产品设计和销售团队的力量。

公司官网：https://socket.dev/

![](img/b479fb444ddcf4866b92b29a8dd03d2a.png)

# 学习

### 强化学习课程

贡献人：

https://web.mit.edu/dimitrib/www/RLbook.html

亚利桑那州立大学（ASU）教授强化学习课程时使用的主要教材，基于老师在2019-2024年间开发的课堂笔记编写。该书不仅是一本独立的教材，还可与老师在本网站提供的视频讲座和幻灯片结合使用。

**第1章**：精确与近似动态规划。内容包括：AlphaZero离线训练与在线对弈、确定性动态规划、随机动态规划的精确与近似解法、无限期问题概述、无限期线性二次问题、重构与简化的示例、强化学习与决策/控制。

**第2章**：价值空间中的近似——回滚算法。内容包括：确定性有限期问题、确定性问题的价值空间近似、离散优化的回滚算法、多步前瞻的价值空间近似、受约束形式的回滚算法、小阶段成本与长时段——连续时间回滚、随机回滚与蒙特卡洛树搜索、无限空间问题的回滚、多智能体回滚、贝叶斯优化与序贯估计的回滚、自适应控制中的POMDP回滚、极大极小控制的回滚。

**第3章**：学习价值与策略。内容包括：参数化近似架构、神经网络、近似动态规划中的成本函数训练、近似动态规划中的策略训练、策略梯度及相关方法、聚合方法。

https://web.mit.edu/dimitrib/www/RLCOURSECOMPLETE%202ndEDITION.pdf

![](img/920d92941d8b802ce4f5f4a09a7191a4.png)

### The State of vLLM 2024

https://zhuanlan.zhihu.com/p/2597683936

+   模型支持，支持几乎所有llm和vlm模型且效率非常高，包括LLama系列模型/Mixtral系列模型/LLava多模态/状态空间模型/reward模型等

+   硬件支持，支持主流的xPU，如nvidia GPU/amd GPU/Intel GPU/Google TPU等

+   模型性能优化

+   深层次的 cuda kernel优化，如计算时异步数据传输/优化内存访问（精度精度-高效数据类型-减少数据传输量）/增加并行度等优化allreduce通信。利用cutlass库优化GEMM，实现kernel fusion等功能，提供专门的flashatention kernel等

+   cuda图优化

+   支持主流的量化技术，FP8/INT8/GPTQ/AWQ等，并集成marlin kernel等

+   新特性

+   支持sarathiserver论文提出的chunked prefill

+   speculative decodin(draft-verification)/dynamic speculative deccoding（根据系统负载和推测精度动态调整长度）

+   基于hash的自动前缀缓存，缓存system prompt及多轮对话，减少请求数据量，这与sglang中采用的radix树维护kv缓存不同，但都提高了缓存重用

+   支持多个lora adapter及热加载

+   支持结构化输出等

+   基于prometheus/grafana的实时监控，包括gpu内存使用/kvcache重用率/访问延迟吞吐等指标

+   分布式系统架构

+   支持tensor并行/流水线并行等

+   多GPU/多节点支持

+   cpu offloading

+   动态批处理/流式输出

+   v0.6.0中最小化GPU负载功能，

+   包括APIserver与推理引擎分离到不同进程，通过ZMQ消息队列通信

+   多步调度

+   异步输出

![](img/ed28947cfebd99e574a15b3b6421a185.png)

### 为什么加速LLM推断有KV Cache而没有Q Cache？

https://www.zhihu.com/question/653658936/answer/12031294363

**Q的动态性**：推理阶段的查询向量Q是根据新输入的token实时生成的。每个新token都会带来新的Q，因此不能直接缓存Q。而K和V来自已处理的历史token，它们在推断过程中保持不变，因此可以缓存。

**计算模式的区别**：在每个推断步骤中，新的查询Q需要与先前缓存的K进行匹配计算，即Q和K的点积。由于K和V可以在模型首次处理时缓存下来，后续的推理阶段只需要每次用新的Q与缓存的K、V进行矩阵运算。而Q的动态特性使得缓存它没有意义，因为每次生成的新Q都不同。

**计算复杂度与效率**：在LLM的推断阶段，使用KV Cache可以显著减少计算量。每次只需进行新Q和缓存的K、V矩阵的计算，而不必重新计算整个序列的键值对，这极大提高了推理效率。如果缓存Q，则每次输入新token时仍需要重新计算Q与K的点积，无法实现计算加速。

**内存与带宽限制**：KV Cache的引入也有助于减少推理时的内存和带宽使用，因为只需缓存历史的K和V，而不需要保留动态变化的Q。

![](img/00742124e376406176d7ccb8afbbdd28.png)

### 试试Mirage零门槛轻松编写高性能PyTorch算子

https://zhuanlan.zhihu.com/p/1274248985

Mirage是一个新兴的项目，致力于通过SuperOptimization技术自动生成高效的GPU内核，无需用户编写CUDA或Triton代码。相比手动设计内核，Mirage在多个基准测试中显示出1.2至2.5倍的性能提升。通过几行Python代码，Mirage能自动搜索最优的GPU内核，实现如FlashAttention等复杂操作。此外，Mirage使用形式化验证确保内核的正确性，减少手动编写CUDA内核时可能出现的错误。

Mirage的核心优势在于：

1.  **生产力提升**：无需深度了解GPU编程，只需描述PyTorch操作。

1.  **性能优化**：通过自动搜索优化方案，比手动编写更高效。

1.  **正确性保证**：形式化验证提高了内核的可靠性。

Mirage不仅适用于现有模型，还能支持带有复杂变体的LLM，如QK-Norm和Multi-Head Latent Attention，通过融合不同的操作进一步提高性能。Mirage为未来的AI开发提供了无需学习复杂GPU编程的可能性，大幅降低开发门槛。

![](img/2abc5890500ba0bf22335515a48f459d.png)

### sglang 学习（一）：radix-tree

https://zhuanlan.zhihu.com/p/2511078239

Radix-tree的实现细节主要围绕两个核心目标：快速查找和最大匹配，以优化token的KV缓存和检索。以下是技术细节总结：

Prefix Cache的需求：

Prefix cache的目的是存储prompt_token_ids对应的KV值，并在相似的输入出现时直接从缓存中取出已有的KV值，减少重复计算。其关键需求是：

+   前向最大匹配：找到prompt_token_ids中前n个已缓存的KV值，并将这些值从KV池中取出，节省重复计算。

核心问题与解决方法：

1.  如何高效记录KV值：使用radix-tree来实现类似字典树的结构存储已计算过的token的KV值。

1.  如何分散存储KV值：采用分散存储策略，类似vLLM的方案，不再赘述。

Radix-tree的实现：

Radix-tree通过分层存储token的KV值，并在新token插入时进行节点分裂（split），确保后续的prompt输入时能够快速找到前向最大匹配的token。

+   首次插入：将token_id和对应的KV值插入radix-tree，如插入"hello, what your first name"，形成树状结构，子节点存储的token可通过key快速查找。

+   多次插入与split操作：如插入第二个输入"hello, what your second name"，节点进行split，使得共享前缀部分快速匹配，未匹配的部分进入新的分支。split确保最大匹配的token能被快速找到。

具体代码解析：

+   插入逻辑：通过递归查找子节点，并根据匹配长度（prefix_len）进行分裂和插入新节点。

+   LRU剔除策略：当显存不足时，采用最少使用（LRU）策略剔除叶子节点的KV值。将叶子节点构建最小堆，并根据访问时间剔除最早的节点，确保活跃token优先保留。

查找最大匹配：

通过递归查找radix-tree中的前向最大匹配token，将匹配的KV值提取出来，并继续查找未匹配的部分。举例来说，对于两个prompt：[1,2,3,4,5,6,7,8]和[1,2,3,4,9,10,11,12]，radix-tree会识别前四个token匹配，并取出对应的KV值，剩余部分继续计算。

Radix-tree设计的目的是高效记录和查找缓存的token KV值，其核心在于split操作确保最大匹配的实现，结合LRU策略来管理内存使用，优化了token的KV缓存。

![](img/182e3b5d8e50ad0880c59db760d08de4.png)

### 一种解决SMAC任务的新方法：让大语言模型写决策树代码

https://zhuanlan.zhihu.com/p/2355110476

作者首先以SMAC（StarCraft Multi-Agent Challenge）作为实验平台，在一周内开发并完成了初步的实验。相关工作发布在论文《LLM-SMAC》中，且代码已开源，供大家引用。

通过LLM生成决策树代码具有以下优势：

1.  **可解释性**：生成的策略是白盒，具备可解释性，而不像神经网络模型那样是黑盒。

1.  **少量环境交互**：相比传统MARL（多智能体强化学习），LLM策略所需的环境交互次数极少。

1.  **策略迁移性强**：基于知识生成的策略不会依赖漏洞，而是依靠正当手段获胜，且同类任务间的迁移性极佳。

在实验中，作者通过LLM生成了一些初步策略，并使用这些策略控制单位在SMAC任务中进行作战。实验结果显示，通过几次迭代调整，生成的决策树策略能够达到接近100%的胜率。相比MARL训练的策略，作者发现LLM生成的决策更为纯粹，甚至在地图之间无需任何代码修改就能成功迁移。

未来，作者计划将对局结果作为奖励函数，进一步微调LLM参数，甚至预训练一个专门用于星际争霸的大模型。通过结合已有的决策树代码和强化学习，作者相信可以开发出一个高质量的决策大模型。

![](img/979a2b26a4538df39c885332bc0946ab.png)

### Ai编译器开发日记-前言

https://zhuanlan.zhihu.com/p/994832931

本系列记录了开发AI编译器的过程。编译器系统复杂，涉及大量与硬件密切相关的细节，个人难以实现工业级别的AI编译器。因此，在开发中尽量简化实现方案，避免过度关注细节优化，同时确保功能齐全。项目的目标是分享开发过程中的问题与解决方案，帮助自己记录学习体会，并欢迎大家讨论更好的解决思路。

该项目的前端使用Python，理论上可接入多种神经网络框架（如Torch、ONNX、TensorFlow等），当前只专注于对接Torch。编译器基于“先编译后优化”的思路，重点在于使用MLIR框架进行图优化、动态shape和符号表达、模型量化、混合精度、算子融合及代码生成、内存优化等。目前暂不涉及LLVM后端的指令集、寄存器优化等。

1.  **前端与框架集成**：前端使用Python，理论上可以对接多种神经网络框架，目前专注于Torch。后续可能支持DSL。

1.  **MLIR 图优化**：重点使用MLIR进行图优化，包括符号推导、动态shape支持、算子分解与融合等技术。

1.  **内存优化**：通过算子融合、内存复用分析等手段优化内存使用，提升性能。

1.  **量化与混合精度**：引入模型量化和混合精度以提高推理速度，特别是对大模型的优化，如Flash Attention与KV缓存。

1.  **代码生成**：通过Linalg dialect生成算子代码，并结合auto tilling与内存分块优化执行效率。

# 招聘公告

---

![](img/2ef06d76ce9568c4adcbde127a5b09fe.png)

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

![](img/5a2810221b7280870c357b1caef7b45e.png)

> 欢迎**推荐/自荐**

> 期待在**奇绩创坛**遇见更好的你

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

![](img/ba355330d00da42d6875b9c79d346837.png)