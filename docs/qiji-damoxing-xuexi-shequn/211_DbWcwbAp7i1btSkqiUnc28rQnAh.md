# 奇绩大模型日报（5月 9日）

> 来源：[https://miracleplus.feishu.cn/docx/Ow4ddjzGtoN9kWxMZoKca0YlnRh](https://miracleplus.feishu.cn/docx/Ow4ddjzGtoN9kWxMZoKca0YlnRh)

# 推特

### OpenAI发布模型规范——塑造期望模型行为的方法

贡献人：

https://x.com/OpenAI/status/1788254844765217227

OpenAI：

为了加深公众对人工智能模型应如何行为的讨论，我们正在分享我们的模型规范——我们塑造期望模型行为的方法。

![](img/8869ec2c05fa9f78097964ef83af9249.png)

#### -- Shawn Wang评价：非常周到，基本上可以视为我们这个时代的三大法则

https://x.com/swyx/status/1788383796225573017

swyx在ICLR会议上提到了OpenAI API中的新功能：“聊天”与“无闲聊”的切换以及更多即将推出的更新。他认为Joanne Jang等人设计的新模型规范非常周到，基本上可以视为我们这个时代的三大法则，特别是目标、规则和默认设置的分割。但他也提出了一个问题：为什么没有人在讨论这里宣布的API的低调变更呢？新的角色变更包括将“平台”、“系统”改为“开发者”、“工具”，新设置包括“互动模式”的聊天风格与无闲聊，以及新增的多方控制字段，允许指定“接收者”和“结束回合”，这主要适用于代理间的对话。此外，还引入了多模态输入/输出功能。

![](img/975d86c05c35116405b00664bcdf99be.png)

#### -- Yi Ding评价模型规范：GPT-5在推理能力将是重大进步，但对开发者而言，GPT更加不透明和不可预测

https://x.com/yi_ding/status/1788281765637038294

来自 @OpenAI 模型规范的几点总结：

1.  GPT-5和未来的模型在决策制定和指令执行方面将有显著改善。这里的条件数量庞大，有些甚至近乎矛盾，令人印象深刻。

1.  多级指令，用户将无法访问大多数顶级指令。系统消息现在变为开发者消息，顶层有一个称为平台消息的层，只能由OpenAI更改。这一变更是出于安全/安保原因，但如果处理不当，可能会导致过度拒绝，就像我们在Llama 2中看到的那样。

1.  基于“设置”的自定义输出。"交互式"和"最大令牌数"现在将直接影响模型的输出，这意味着将不再有一个标准的“GPT-5”响应。之前，最大令牌数只是进行截断，但现在它会影响简洁性。

1.  YAML、JSON和XML在模型中被明确指出，并与提示的其他部分不同对待。这样做是为了避免提示注入，但这也可能意味着用这些类型的输入构建的提示在未来模型中的表现会更好。

1.  有迹象表明，一些规则可以被覆盖。测试哪些规则被模型更强烈地持有，哪些规则被更弱地持有将是有趣的。

好消息：我认为这表明GPT-5在推理能力上将是一个重大的进步。这也表明OpenAI继续认真对待安全，尽管到目前为止，他们所实施的每一层过滤和安全措施都被绕过了。新的提示层次结构是否能彻底解决这个问题将是有趣的。

坏消息：对于应用程序开发者来说，这可能表明GPT输出将变得更加不可预测和不透明。许多规则层次无疑会导致之前调整好的程序和提示停止工作的特殊情况。

丑陋的一面：即使现在有了4个级别的提示，也没有专门的上下文或RAG消息。

![](img/531ef3b9f594dc6ba9b0928365d7ad2e.png)

![](img/56bc252f4598f3f6b4d7079252e3b502.png)

### Phi-3 WebGPU：浏览器中本地运行，设备上推理！

贡献人：

https://x.com/xenovacom/status/1788177160227660079

推出Phi-3 WebGPU，这是一个私密且强大的AI聊天机器人，它在您的浏览器中本地运行，由🤗 Transformers.js和onnxruntime-web驱动！

🔒 设备上推理：不将数据发送到服务器

⚡️ WebGPU加速（> 20 t/s）

📥 模型下载一次并缓存

![](img/80d4324c4be70b8100ed42b96e3c247c.png)

[ssstwitter.com_1715227275261.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MY0Gbw6pqoKodIx7PkEcPMTJnBc)

### Niels Rogge分享：在Idefics2的基础上为文档的应用情景微调

贡献人：

https://x.com/NielsRogge/status/1788160339088912876

Idefics2 是同等规模中最强大的开源视觉语言模型之一。为了庆祝它的发布，我创建了一个演示笔记本，展示了如何为文档AI用例（例如，收据图像 -> JSON）微调它。

https://github.com/NielsRogge/Transformers-Tutorials/blob/master/Idefics2/Fine_tune_Idefics2_for_JSON_extraction_use_cases_(PyTorch_Lightning).ipynb

![](img/df515a197c704b4b21266a5be0cb2845.png)

### DeepLearning AI分享新课程：Jerry Liu讲述《构建具有代理性的RAG与LlamaIndex》

贡献人：

https://x.com/AndrewYNg/status/1788246239517282795

我很高兴启动我们的短期课程系列，这些课程聚焦于代理，首先是由 @jerryjliu0，@llama_index 的CEO教授的《构建具有代理性的RAG与LlamaIndex》。

这涵盖了RAG（检索增强生成）中的一个重要转变，在这一转变中，我们不再由开发者编写显式程序来检索信息以供给LLM（大型语言模型）上下文，而是构建一个具有获取信息工具的RAG代理。这使得代理可以决定获取哪些信息，并利用多步骤推理回答更复杂的问题。

具体来说，你将学习到：

+   路由：你的代理将使用决策来将请求路由到多个工具。

+   工具使用：你将创建一个界面，让代理选择使用哪个工具（函数调用）以及生成正确的参数。

+   使用工具的多步骤推理：你将使用LLM执行多个推理步骤，同时在整个过程中保持记忆。

你还将学习如何逐步检查你的代理正在做什么，以便调试和迭代改进。

构建代理的时代令人兴奋。在这里报名并开始学习吧！https://deeplearning.ai/short-courses/building-agentic-rag-with-llamaindex

![](img/83466f47ee784313feb0009b8616bd3c.png)

[ssstwitter.com_1715230229981.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/CDcIbBBxroYBFtxMC0Oc9jzHnpc)

### Yuzhe Qin：我为什么敢让BunnyVisionPro机器人触摸我的脸

贡献人：

https://x.com/QinYuzhe/status/1787970486594641933

许多人对模特（也就是我）的勇敢印象深刻，因为我允许机器人触摸我的脸。然而，这并不像看起来那么危险。以下是常见的机器人操控系统如何增强安全性的要点：

1.  硬件级保护：大多数机器人会监控电机电流。如果机器人发生重大撞击，电流将超过预设限制，触发自动关闭以防止任何危险行为。

1.  控制级保护：装备有扭矩传感器的机器人可以监控和调整每个关节的扭矩。它们使用阻抗控制来适应不同的任务，这涉及到为最佳性能微调众多参数。这更像是精确性与顺应性之间的权衡，且是特定于任务的。例如，在护肤任务中，优先保证安全比精确性更重要。

1.  算法级保护：运动生成和规划算法将期望的末端执行器姿态转换为特定的机器人运动命令，如关节级命令。在此过程中，我们可以对环境中的对象进行建模，以避免潜在的碰撞。碰撞检测需要大量计算资源，而且经常需要工程解决方案来加速这一过程，以实现实时执行。

多亏了这些技术，与机器人互动比你想象的要安全。

![](img/17ed01914486b9e43544e44223eae920.png)

[ssstwitter.com_1715230318847.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SNwgbzbpGoTEhixv4HVcDyInnQH)

### Keshav分享 Ilya 30u30：如果你真正学会了这些，你就掌握了当今90%重要的知识

贡献人：

https://x.com/keshavchan/status/1787861946173186062

今天我了解到，Ilya Sutskever 给 John Carmack 提供了一个包含大约30篇研究论文的阅读清单，并说，‘如果你真正学会了这些，你就掌握了当今90%重要的知识。’

![](img/997f66d38bf3028c7d581e21d5bd662b.png)

### AI Device Template：由AI驱动的语音助手，利用各种AI模型和服务为用户查询提供智能响应

贡献人：

https://x.com/Dev__Digest/status/1787860999950712846

这个项目是一个由AI驱动的语音助手，利用各种AI模型和服务为用户查询提供智能响应。它支持语音输入、转录、文本到语音、图像处理以及具有条件渲染的UI组件的函数调用。这个项目的灵感来源于最近的AI设备趋势，如Humane AI Pin和Rabbit R1。

今天，我将开源我最新的项目，深入研究人工智能设备。这个项目使用了多种技术，包括GroqInc的Llama3（即将推出耳语预览），LangChainAI和Langsmith，OpenAI的文字转语音、视觉和耳语技术，Vercel的人工智能开发套件，Next.js，Serperapi的搜索功能，Upstash的速率限制以及FAL的Llava。相关视频和代码仓库信息请查看下方链接。

![](img/424bec45efa36463456e3d5a40409f78.png)

[ssstwitter.com_1715230496260.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SUsWbiFmconH8fxmUoDcFZJ1nTb)

# 资讯

### 所有生命分子一夜皆可AI预测！AlphaFold 3改变人类对生命的理解，全球科学家都能免费使用

贡献人：

https://mp.weixin.qq.com/s/27ZHwSgo5Cp3_ch0tktO-w

https://www.tmtpost.com/7077710.html

**AlphaFold 3**再登Nature！这次重磅升级，不再仅限于蛋**白质结构预测**—**—可以以前所**未有的精度**预测所有生命分子的**结构和相互作用。"只有了解它们如何在数百万种组合中相互作用，我们才能开始真正理解生命的过程。"这次的最大创新之一，是用上了AI绘画上常见的去噪扩散模型，直接生成每个原子的3D坐标。现在，AlphaFold 3对普通感冒病毒Spike蛋白（蓝色）的结构预测，灰色部分为预测结果。这能让人类更进一步了解冠状病毒。

![](img/bf924b004e09f20a1495a2237cee5229.png)

#### -- Jim Fan评价：大突破，得益于现在的通用人工智能配方

https://x.com/DrJimFan/status/1788233450123936020

AlphaFold-3发布了，这是人工智能在生物学领域最伟大突破的最新迭代。新特性是AlphaFold-3使用扩散过程来“渲染”分子结构。它从一个模糊的原子云开始，然后通过去噪逐渐实现分子的物质化。

我们生活在一个时代，Llama和Sora的学习成果可以为生命科学的信息提供和加速。我发现这种通用性的层次令人难以置信。同样的变换器+扩散主干不仅可以生成精美的像素，只要你相应地将数据转换为浮点数序列，它还可以想象蛋白质。

我们还没有达到一个单一的通用人工智能模型，但我们已经成功构建了一套通用的人工智能配方，这些配方在不同领域间转移训练、数据和神经架构。按理说这不应该行得通，但感谢上帝，它确实行得通！

![](img/d52161a1c247dcba96efe4f776c58b4e.png)

### 阿里云发布通义千问2.5：全面赶超GPT-4 Turbo

贡献人：

https://news.mydrivers.com/1/978/978802.htm

**阿里云正式发布了通义千问2.5，模型性能全面赶超GPT-**4 Turbo，声称地表最强中文大模型。据了解，通义千问2.5最新开源的1**1**00亿参数模型在多个基准测评中均取得了最佳成绩，成功超越了Meta的Llama-3-70B模型，成为开源领域的新标杆。相比通义千问2.1版本，通义千问2.5在上述四项能力上分别提升了9%、16%、19%和10%，其中中文能力更是持续领先业界。在权威基准OpenCompass上，通义千问2.5的得分追平了GPT-4 Turbo，这是该基准首次有国产大模型取得如此出色的成绩。这一成绩不仅证明了通义千问2.5在中文语境下的卓越性能，也展示了阿里云在人工智能领域的创新能力。

![](img/cae0a294ef476c4cd3c3814e53bd9e3e.png)

### 再见机器人：Altera的游戏AI代理得到了埃里克·施密特的支持

贡献人：

https://techcrunch.com/2024/05/08/bye-bye-bots-alteras-game-playing-ai-agents-get-backing-from-eric-schmidt/

自主的，AI-based的玩家正在接近你的游戏体验，而一家名为Altera的初创公司正在加入这场战斗，以建立这种新的人工智能代理卫队。 该公司周三宣布，在一轮超额认购的种子轮融资中筹集了900万美元，该轮融资由First Spark Ventures(埃里克•施密特的深度科技基金)和Patron (Riot Games校友共同创立的种子期基金)共同领投。

![](img/abae3afc9f28e633b9fbfbace89021a2.png)

### 微软将在原地投33亿美元建AI基础设施

贡献人：

https://www.cls.cn/detail/1670891

微软周三宣布，将在威斯康星州投资数十亿美元用于建设人工智能(AI)基础设施，这家美国科技巨头正在为日益增长的AI计算需求做好准备，这也是该公司在全球范围内进行的一系列大规模投资中的最新一站。

![](img/169d2ef4e1ff4fe9f4525e8beed864c6.png)

### “驯服”不受控的大模型，要搞定哪些事？专访达观数据副总裁王文广

贡献人：

https://mp.weixin.qq.com/s/4p-1j5J4bHaanchJVmk9qQ

GPT 爆火一年多后，无论在国内、外，“几乎所有领域都需要用大模型重构”的论调已深入人心。中国 200 多家厂商掀起的“百模大战”、层出不穷的千亿和万亿大参数模型、性能效果与应用方向的飞速迭代，无一不在表明大模型被各行各业拥抱的热潮力度。但在更多行业对大模型跃跃欲试之际，也有许多现实的落地问题浮现出来，可控性问题就是其中之一。最近，达观数据副总裁王文广进行了演讲分享，他将从大模型相关技术和幻觉问题为切入点，探讨如何利用知识图谱、RAG 和大模型融合的技术路线提高大模型的可解释性、可操作性和可控性。

![](img/3058580482ec962b3493e150e71005a7.png)

# 论文

### 只缓存一次：语言模型的解码器-解码器架构

贡献人：

链接：http://arxiv.org/abs/2405.05254v1

我们引入了一种名为YOCO的解码器-解码器体系结构，适用于大型语言模型，仅对键值对进行一次缓存。它由两个组件组成，即交叉解码器叠加在自解码器上。自解码器通过交叉注意力有效编码全局键值（KV）缓存，交叉解码器通过交叉注意力复用这些缓存。尽管YOCO只缓存一次，但整体模型表现为解码器-只Transformer。这种设计显著减少了GPU内存需求，同时保留了全局注意力能力。此外，计算流程支持预填充以提前退出，而不更改最终输出，从而显着加快了预填充阶段。实验结果表明，与不同规模和训练token数量的Transformer相比，YOCO在各种设置下达到了有利的性能。我们还将YOCO扩展到了100万上下文长度，并实现了接近完美的needle检索准确性。性能分析结果显示，YOCO在各种上下文长度和模型大小上显著提高了推断内存、预填充延迟和吞吐量。代码可在https://aka.ms/YOCO找到。

![](img/64330cb64e0f9e491339098394bf85d4.png)

### 多模态数据高效3D场景理解在自动驾驶中的应用

贡献人：

链接：http://arxiv.org/abs/2405.05258v1

高效利用数据对于推进自动驾驶中的3D场景理解至关重要，在这种情况下，过于依赖人工标注的LiDAR点云挑战了完全监督的方法。为解决这一问题，我们的研究扩展到了LiDAR语义分割的半监督学习，利用驾驶场景的内在空间先验和多传感器补充来增强未标记数据集的有效性。我们介绍了LaserMix++，一个进化框架，整合了来自不同LiDAR扫描的激光束操作，并结合了LiDAR-相机对应关系，进一步辅助数据有效学习。我们的框架旨在通过整合多模态来增强3D场景一致性正则化。LaserMix++的多功能性使其适用于LiDAR表示的各种应用，将其确立为一种通用适用的解决方案。我们的框架经过理论分析和广泛实验验证。结果表明，LaserMix++显著优于完全监督的替代方案，实现了与五倍较少注释相比可比的准确性，并显著改善了仅监督的基线。这一重大进步突显了半监督方法在减少LiDAR为基础的3D场景理解系统中对大量标记数据依赖的潜力。

![](img/bea0db2f52e5c27ccd47f72585e966da.png)

### ACEGEN: 用于药物发现的生成化学智能体的强化学习

贡献人：

链接：http://arxiv.org/abs/2405.04657v1

近年来，强化学习（RL）已成为药物设计中一种有价值的工具，为提议和优化具有所需性质的分子提供潜力。然而，由于高级RL算法的复杂性和对专门代码的重要依赖，如何在功能、灵活性和可靠性之间取得平衡仍具有挑战性。在这项工作中，我们介绍了ACEGEN，一个专为生成式药物设计量身定制的全面且简化的工具包，使用了TorchRL，一个现代决策库，提供高效且经过充分测试的可重复使用组件。ACEGEN为分子设计提供了一个强大、灵活和高效的平台。我们通过在各种算法上进行基准测试和进行多个药物发现案例研究来验证其有效性。ACEGEN可在https://github.com/acellera/acegen-open 获取。

![](img/d979eb8beeaa0bfb60f7b4c9cd7e2e7d.png)

### 探索训练动态解读“逆转诅咒”理论

贡献人：

链接：http://arxiv.org/abs/2405.04669v1

自回归的大语言模型(LLMs)在解决许多复杂推理任务时表现出色，但在一些简单逻辑推理任务中则表现不佳，如逆向搜索：在训练''A是B''后，在推理过程中LLM无法直接推断''B是A''，这被称为''逆转魔咒''。本文通过两种自回归模型的(随机)梯度下降训练动态，理论分析了逆转魔咒：(1) 一个可以视为一层Transformer简化的双线性模型；(2) 使用Tian等人(2023a)框架的一层Transformer。我们的分析揭示了逆转魔咒发生的核心原因：两种自回归模型的(有效)权重显示出不对称性，即在训练过程中从标记$A$到标记$B$的权重增加未必会导致从$B$到$A$的权重增加。此外，我们的分析还可以自然地应用于其他逻辑推理任务，如思维链(COT)。我们展示了COT的必要性，即一个训练了''$A \to B$''和''$B \to C$''的模型无法在没有COT的情况下直接推断''$A \to C$''，对于一层Transformer通过训练动态提供了一个新的观点，与之前关注表达性的工作有所不同。最后，我们还进行了实验证实了我们理论在不同设置下多层Transformer的有效性。

![](img/6dacfa4f0f7b9b05209d304a42888c8e.png)

### ChuXin **1.6B技术报告**

贡献人：

链接：http://arxiv.org/abs/2405.04828v1

摘要：在本报告中，我们介绍了ChuXin，一个完全开源的拥有16亿参数的语言模型。与大多数仅开源模型权重和架构的作品不同，我们提供了训练模型所需的一切内容，包括训练数据、训练过程和评估代码。我们的目标是为开放研究社区赋予力量，促进透明度，并在语言建模领域开启一波新的创新浪潮。此外，通过轻量级持续预训练，我们将上下文长度扩展到100万个token，并展示了出色的针对堆中之针的检索性能。这两个模型的权重可在Hugging Face上下载和使用。

![](img/cd7ace985d6d7c7b21845e0382689351.png)

# 产品

### Abstra Workflows

贡献人：

https://www.abstra.io/

Abstra Workflows 是一款基于 Python 和 AI 的工作流引擎，帮助公司设计和管理定制的业务流程，无需额外开支。它可以将自动化任务与人工评审相结合，提供实时运营洞察和完整的历史跟踪，解决了企业在使用无代码工具、管理本地脚本、赋予非技术人员自主权、审计流程变更等方面的痛点。

![](img/d9e3ce2196a872f464067f25fa6dcd9e.png)

### **Ringly.io**

贡献人：

https://www.ringly.io/

Ringly.io 是一家 AI 电话代理服务公司，旨在帮助电商企业自动化客户服务并降低成本。它通过将人工客户服务替换为基于 AI 的即时响应电话系统，让企业能节省 90% 的客户服务开支，同时为客户提供更快捷、更准确的服务。该公司使用了ElevenLabs的先进语音技术，使 AI 代理的声音更加自然流畅，提升了客户体验。

![](img/0d9be9c06cc9b514f065c72fe6e37029.png)

# HuggingFace&Github

### Kan-GPT

贡献人：

https://github.com/AdityaNG/kan-gpt

Kan-GPT 是一种结合了 GPT 模型和 Kolmogorov-Arnold 网络 (KANs) 的语言生成模型，通过使用 KANs 作为基础架构来提高 GPT 的性能。它提供了安装、使用、训练等功能，并且开源了代码供开发者参考和进一步研究。

![](img/0c2065817d5ab0e4fd6e7bd42f4cd8ae.png)

### Agents-Flex

贡献人：

https://github.com/agents-flex/agents-flex

Agents-Flex 是一个基于 Java 的 LLM 应用程序框架，提供了丰富的功能，如 LLM 访问、内存管理、资源加载与解析、智能代理和函数调用等，帮助开发者快速构建基于 LLM 的智能应用。它类似于LangChain，简化了 LLM 相关开发工作，为开发者提供了一站式的 LLM 应用开发解决方案。

![](img/165012c5e5067e9f3c648a3fc69efe50.png)

# 投融资

### DatologyAI完成4,600万美元A轮融资

贡献人：

https://www.datologyai.com/post/datologyai-raises-46m-series-a

DatologyAI宣布完成了由Felicis Ventures的Viv Faga和Astasia Myers领投的4,600万美元A轮融资，现有种子投资者Radical Ventures和Amplify Partners以及新投资者Elad Gil、M12和Amazon Alexa Fund也参与了此轮融资。该公司专注于构建全球最佳的自动数据筛选平台，帮助客户在PB级规模下更快地训练模型，提高性能，并使较小的模型实现与更大模型相竞争的性能。这笔资金将使公司能够大幅扩展团队规模，增加计算能力，推动数据筛选的可能性前沿，不仅限于语言模型，而是跨越多种模态。

公司官网：https://www.datologyai.com/

![](img/c1e08e2bfcac440ad32f6a0ba221413e.png)

### XTEND获得4,000万美元B轮融资，推出机器人和无人机操作系统

贡献人：

https://www.calcalistech.com/ctechnews/article/b1lfrkfz0

以色列初创公司XTEND开发了一款AI驱动的机器人和无人机操作系统，旨在为它们提供人工智能能力，并帮助它们与人类合作。该公司宣布已经完成4,000万美元的B轮融资，由Chartered Group领投，以及现有和新的战略投资者Clal-Tech等的进一步参与。CEO Aviv Shapira表示，此轮融资是在10月7日加沙战争爆发前发起的，自那时以来，公司已将全部注意力转向为以色列国防军开发系统。XTEND的人工监督、AI驱动的无人机和机器人操作系统使操作员能够在任何环境中执行高度复杂和动态的任务，无需太多培训。

公司官网：https://www.xtend.me/

![](img/ad30d5b09c81723e7a3ca3427926fc7f.png)

### Atlan以1.05亿美元投资了其数据控制平面，正值LLMs增强数据重要性

贡献人：

https://techcrunch.com/2024/05/08/atlan-scores-105m-for-data-control-plane-just-as-llms-increase-importance-of-data/

Atlan是一家数据治理初创公司，数据一直是他们工作的核心，甚至在公司成立之前。事实上，联合创始人Prukalpa Sankar和Varun Banka开始是在建立印度的国家数据平台SocialCops。因此，他们开发了一款用于控制和协作数据的工具，整合了各种数据源，并在大型组织内带来了数据混乱的控制。

该公司成立于2020年，但数据问题自那时以来变得愈发重要，如今，随着公司试图整理数据以利用生成式人工智能，这一问题变得更加重要。也许这就是为什么该公司在周三宣布了一笔1.05亿美元的投资，估值高达7.5亿美元的原因。该初创公司似乎正处于正确的时间和地点，解决着正确的问题——而投资者则希望利用这一机会。

Atlan的目标是解决每个组织数据生态系统的内在复杂性，并帮助人们了解，即使有各种各样的工具和数据存储库，也可以掌握大局。

公司官网：https://atlan.com/

![](img/351144525a59d4fc462f0c242a62ecc7.png)

### Espresso AI以1100万美元种子轮融资正式亮相，应对云成本危机

贡献人：

https://venturebeat.com/ai/espresso-ai-emerges-from-stealth-with-11m-to-tackle-the-cloud-cost-crisis/

Espresso AI是一家位于硅谷的人工智能初创公司，已经完成了超过1100万美元的种子轮融资，旨在将AI的力量带入当前企业计算领域最大的挑战之一：控制不断增长的云成本。该轮融资由Daniel Gross和Nat Friedman领投的种子轮和Matt Turck在FirstMark领投的Pre-Seed轮共同完成，还得到了业内领先公司的参与。Espresso AI开发了一种使用先进的语言模型和机器学习来自动优化代码，将云计算成本降低多达80%的技术。其初期产品专注于简化Snowflake这一热门云数据仓储平台的SQL查询。该公司的首席执行官Ben Lerner表示，这一技术在数据仓储领域的潜在节省达数十亿美元，为客户带来巨大的潜在价值。

公司官网：https://espresso.ai/

![](img/e78afc2c8480f8230e49b298d74f07c9.png)

### Altera游戏AI代理获得Eric Schmidt支持的900万美元种子轮融资

贡献人：

https://techcrunch.com/2024/05/08/bye-bye-bots-alteras-game-playing-ai-agents-get-backing-from-eric-schmidt/

Altera是一家新兴初创公司，致力于构建自主的、基于人工智能的游戏代理。该公司近日宣布完成900万美元的种子轮融资，由Eric Schmidt的深科技基金First Spark Ventures和由Riot Games前员工共同创立的种子阶段基金Patron共同领投。这笔资金将用于扩大团队规模，加速产品开发和增长。Altera的首席执行官Robert Yang表示，该公司旨在打造具有社交情感智能的AI代理，可以与玩家互动并自主决策。Altera的首个产品是一个可以与玩家一起玩Minecraft的AI代理，未来计划将该技术拓展到更多视频游戏和数字体验领域。

公司官网：https://altera.al/

![](img/1cce8dd8736449eb09f7d62e05501eba.png)

### CheckFirst获得150万美元Pre-Seed轮融资

贡献人：

https://techcrunch.com/2024/05/08/checkfirst-raises-1-5m-pre-seed-applying-ai-to-remote-inspections-and-audits/

CheckFirst是一家AI驱动的工作流工具初创公司，专注于远程检查和审核领域。创始人Ben Lambert意识到，在葡萄牙生活后，妻子的房地产检查业务需要远程运营，于是创立了CheckFirst。该公司解决了远程检查的挑战，还允许企业根据地理位置和资格安排检查员的日程，从而减少出差、降低环境足迹，并提高员工满意度。CheckFirst最近完成了150万美元的Pre-Seed轮融资，由里斯本的早期风投公司Olisipo Way和Hiero VC领投。其他参投方包括Notion Capital以及来自Source Point、Busuu、Swogo和FaceIT等公司的天使投资者。

公司官网：https://www.checkfirst.ai/

![](img/5ec092bef58fc86772f68477055ef5f9.png)

### 以色列初创公司Panax获得1000万美元A轮融资

贡献人：

https://techcrunch.com/2024/05/08/israeli-startup-panax-raises-a-10-million-series-a-for-its-ai-driven-cash-flow-management-platform/

Panax是一家两年历史的以色列初创公司，专注于为中大型传统行业公司提供现金流管理平台。该公司最近完成了由Team8领投、TLV Partners参投的1000万美元A轮融资。Panax的目标客户包括制造业、物流和房地产等传统行业的中大型企业，这些企业通常需要优化复杂的资金管理需求，例如跨地域、跨货币的操作。Panax利用人工智能技术帮助客户优化现金流，提供一站式的财务管理仪表盘，整合推荐和预测功能，帮助客户更好地执行决策。公司计划利用这轮融资扩大市场营销，并建立更强大的人工智能和数据团队。

公司官网：https://www.thepanax.com/

![](img/726b19ee7f788d7d04ef567fccd6fad8.png)

### 「超星未来」完成数亿元Pre-B轮融资

贡献人：

https://mp.weixin.qq.com/s/Q7KIEdvd3xWKufTr7puArA

中关村U30年度优胜者梁爽创业项目「超星未来」完成数亿元Pre-B轮融资。投资方包括中安资本、梁溪科创、龙鼎投资、天智投资、陕汽智能汽车基金和讯飞创投。资金将用于开发新一代大模型推理芯片、扩大现有营收业务规模，并进一步拓展产业合作。超星未来成立于2019年，是一家边缘侧人工智能芯片提供商，致力于成为边缘侧AGI计算的引领者。CEO梁爽是清华大学、帝国理工联合培养的博士，曾任职于百度IDL、深鉴科技，全程参与国内最早AI芯片“听涛”开发落地。公司创始团队具有较为鲜明的清华基因，产品已推广到智能驾驶、智慧电力、智慧矿山等重要客户中。超星未来团队计划将芯片与大模型结合，寻找AI PC、具身智能等场景，进一步拓展大模型推理计算的可能。

公司官网：https://www.novauto.com.cn/#/home

![](img/9eec6e198672804f509bd4f86df20cd0.png)

### CR Index - 华兴一级市场温度指数报告（第六期）2024年H1篇｜华兴报告

贡献人：

https://mp.weixin.qq.com/s/W9VrHT-vrFJQtu9ZwkmgFw

2024年上半年一级市场整体情况积极，新材料赛道持续受关注，AI应用与大模型、机器人、低空经济等领域热度上升。投资人对A股市场持谨慎乐观态度，预测A股将横盘震荡，A股上市仍是主要IPO退出渠道。投资策略上，机构去年出手谨慎，预计今年将分化，整体偏向谨慎乐观。投资人对出海投资表现出浓厚兴趣，尤其关注东南亚和中东市场，同时关注二级市场，对2024年海外中资股和A股市场持稳中向好的预期。

![](img/2b628bc75ff1faa08843cc286e357ce3.png)

# 学习

### 决策智能与强化学习（2）：World Models

贡献人：

https://zhuanlan.zhihu.com/p/696752967

文章深入探讨了决策智能与强化学习领域的前沿技术——世界模型（World Models）。首先回顾了世界模型的起源和核心思想，随后详细介绍了Dreamer系列、TD-MPC系列和MuZero系列等代表性工作，这些研究在架构设计和训练范式方面取得了显著进展。文章还讨论了基于Transformer的世界模型，以及如何通过不同的模型架构提升性能。最后，文章展望了世界模型在提高样本效率、泛化能力和鲁棒性方面的未来研究方向，以及其在大规模视频-语言数据预训练中的应用潜力。

![](img/41562108c6ca5c9a79112719d079dd3c.png)

### Refuel LLM-2

贡献人：

https://www.refuel.ai/blog-posts/announcing-refuel-llm-2

RefuelLLM-2是为数据标签化、清洗和丰富专门构建的大型语言模型的新版本，比所有现有顶尖模型都要优秀，包括在约30个数据标签任务中超越了GPT-4-Turbo和Claude-3-Opus。该模型基于Mixtral-8x7B基模型，训练了一个覆盖多种任务（如分类、阅读理解、结构化属性提取和实体解析）的2750多个数据集。RefuelLLM-2小型版也展示了优越性，被开源并可通过Hugging Face获取。Refuel采用自动标签库Autolabel对这些模型进行了测试，显示出在非公开数据集上的出色泛化能力，特别是在处理长文本输入时的强大表现。

![](img/f06b7387e2f4b407029457e5cef3b819.png)

### Unsloth x Qwen2，提速47.32%，节省39.13%显存，最少仅需8.43GB显存

贡献人：

https://mp.weixin.qq.com/s/x2N3p1qgJy_RyRsO2PHS_A

Unsloth是一个训练框架，通过优化梯度计算、链式矩阵乘法等手段，大幅提升训练速度，降低显存占用。最新版本支持Qwen2模型，可在一张1080Ti上训练Qwen1.5-7B，提速47.32%，显存占用减少39.13%。通过Firefly项目和Unsloth对Qwen1.5-7B的SFT和DPO两阶段训练，在🤗Open LLM Leaderboard上取得优异表现。未来，开源社区有望进一步完善Unsloth，推动大模型训练成本和速度的进一步提升。

![](img/24fad67d9aef6871d079653fec98e653.png)

### 使用GGML和LLAMA-CPP对LLAMA模型进行量化

贡献人：

https://towardsdatascience.com/quantize-llama-models-with-ggml-and-llama-cpp-3612dfbcc172

GGML和LLAMA-CPP是两个工具，可用于对LLAMA模型进行量化，从而减小模型尺寸、提高推理速度。GGML提供了一种简单的方法，可以将PyTorch模型转换为TFLite格式，并在TFLite模型上运行Post Training Quantization（PTQ）流程。LLAMA-CPP是一个用C++实现的LLAMA推理引擎，支持INT8量化推理，并提供用于加载和运行LLAMA模型的API。通过GGML和LLAMA-CPP的结合使用，用户可以方便地将LLAMA模型转换为INT8量化版本，并在嵌入式设备上进行高效推理。

![](img/e11bcad30ed13e15dcf17bb7fefddd7e.png)

### 飞腾、鲲鹏、海光、龙芯、兆芯、申威国产CPU处理器

贡献人：

https://zhuanlan.zhihu.com/p/609496571?utm_psn=1771518998435684352

国产CPU处理器领域中，飞腾、鲲鹏、海光、龙芯、兆芯、申威等厂商正推动“中国芯”的发展。飞腾基于ARM架构，提供高性能服务器和桌面CPU，支持PK生态；鲲鹏为华为旗下，以ARM V8架构实现端边云算力同构；海光与AMD合作，开发X86架构CPU，性能领先；龙芯完全自主研发，基于MIPS架构；兆芯是X86架构下的合资企业，产品性能较好；申威基于Alpha架构，专注于超级计算。各厂商正通过技术创新和生态建设，提升产品性能，增强信息安全，逐步减少对外部技术的依赖。

![](img/95a754e2d651a7b255e33a5250673f43.png)

### **优化微调语言模型的GPU利用率**

贡献人：

https://jillanisofttech.medium.com/optimizing-gpu-utilization-for-fine-tuned-language-models-a-comprehensive-guide-d681743a4a00

该文章详细介绍了如何优化大型语言模型（LLMs）的GPU利用率，特别是通过适配器模块和低秩适配器（LoRA）技术，以减少计算成本并提高模型性能。LoRA技术通过对模型权重进行小规模且易于管理的矩阵修改来实现这一目标，有效减少了计算开销，同时保持了模型性能。通过在单一基础模型中集成多个适配器，可以在共享GPU资源上实现高效的多租户部署，智能地将请求路由到适当的适配器，从而优化资源使用，减少成本，并保持灵活性和可扩展性。此外，文章还强调了动态资源分配和适配器的监控调整在维持运营效率和高客户满意度中的重要性。

![](img/f7bcdc5ae48eb03f093d5236b504ec0a.png)

### 单节点8xA800跑起来HuggingFace DeepSeek V2踩坑

贡献人：

https://zhuanlan.zhihu.com/p/696792195?utm_psn=1771933567855411200

该文章是关于在单节点8xA800上运行HuggingFace DeepSeek V2时遇到的一些问题和解决方法的总结。作者首先介绍了在运行DeepSeek V2时遇到的两个报错情况，以及对应的解决方法。其次，作者分享了在使用transformers库进行模型推理时遇到的程序卡死问题，并给出了优化代码的解决方案。文章还提到了在单节点A800推理时需要限制输出长度以避免内存不足的问题，并给出了相应的调试建议和实测结果。最后，作者提到了如果想在单节点8卡A100/A800上加速推理并且输出更长长度的文本，可以使用vllm的实现。文章突出了技术细节和解决问题的方法，对于在实际应用中遇到类似问题的读者具有一定的参考价值。

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

![](img/d8e7c1036ac717062a12b8b347042b96.png)