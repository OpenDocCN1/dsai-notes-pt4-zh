# 奇绩大模型日报（7月 16日）

> 来源：[https://miracleplus.feishu.cn/docx/QmNcdpI8uoRzkexJDNacBQmJnYd](https://miracleplus.feishu.cn/docx/QmNcdpI8uoRzkexJDNacBQmJnYd)

# 资讯

### 大模型集体失智！9.11和9.9哪个大，几乎全翻车了

https://mp.weixin.qq.com/s/s1GGFgADmjxdeLHaQfSxRw

一觉醒来，一众响当当的大模型开始认为**“9.11>9.9”了？发现**这个问题的**是Riley Good**side，有史以来第一个全职提示词工程师。简单介绍下，他目前是硅谷独角兽Scale AI的高级提示工程师，也是大模型提示应用方面的专家。最近他在使用GPT-4o时偶然发现，当提问：9.11 and 9.9——which is bigger？GPT-4o竟毫不犹豫回答前者更大。面对这一常识性“错误”，他不死心地又去问了其他大模型，结果几乎全军覆没。

![](img/7ed24b293125786985d426c853eaea67.png)

### 太酷了！iPhone、iPad、MacBook老旧设备组成异构集群，能跑Llama 3

https://mp.weixin.qq.com/s/coMIiu8a2oAJXRx48tKgDQ

将 iPhone、iPad、Macbook 进行组合，就能组装成「异构集群推理方案」， 然后顺畅的运行 Llama3 模型。值得一提的是，这个异构集群可以是 Windows 系统，也可以是Linux、iOS 系统，并且对 Android 的支持很快到来。根据项目作者 @evilsocket 的介绍，这个异构集群包括 iPhone 15 Pro Max、iPad Pro、MacBook Pro (M1 Max)、NVIDIA GeForce 3080、2x NVIDIA Titan X Pascal。所有代码都已经上传到 GitHub。

![](img/cb77dd77142feeda693ca7a6a5235751.png)

### 公理训练让LLM学会因果推理：6700万参数模型比肩万亿参数级GPT-4

https://mp.weixin.qq.com/s/Yera281WG8RNAOrRUYjH6g

通常来说，因果推理使用的数据对应于一个系统中的变量。通过正则化、模型架构或特定的变量选择，可以归纳偏置的形式将公理或规则集成到机器学习模型中。根据可用数据种类的差异（观察数据、干预数据、反事实数据），Judea Pearl 提出的「因果阶梯」定义了因果推理的可能类型。由于公理是因果性的基石，因此我们不禁会想是否可以直接使用机器学习模型来学习公理。也就是说，如果学习公理的方式不是学习通过某个数据生成流程得到的数据，而是直接学习公理的符号演示（并由此学习因果推理），哪又会如何呢？相较于使用特定的数据分布构建的针对特定任务的因果模型，这样的模型有一个优势：其可在多种不同的下游场景中实现因果推理。随着语言模型具备了学习以自然语言表达的符号数据的能力，这个问题也就变得非常重要了。事实上，近期已有一些研究通过创建以自然语言编码因果推理问题的基准，评估了大型语言模型（LLM）是否能够执行因果推理。微软、MIT 和印度理工学院海得拉巴分校（IIT Hyderabad）的研究团队也朝这个方向迈出了重要一步：提出了一种通过公理训练（axiomatic training）学习因果推理的方法。

![](img/719da1fa005729dcbd7eeac1934b9729.png)

### 谷歌机器人专家：机器人在现实中碰过的壁，AI也会碰

https://mp.weixin.qq.com/s/rW0qzhCBTRypNG8hPShAhA

有人说，机器人领域进展缓慢，甚至和机器学习的其他子领域相比显得毫无进展。谷歌 DeepMind 机器人科学家，SayCan、RT-1、RT-2 等具身智能项目参与者 Alex Irpan 同意这一说法。但他认为，这是因为机器人学是一个和现实紧密连接的领域，现实的复杂性决定了他们不免碰壁。他还指出，这些问题不是机器人技术所独有的。同样的问题也适用于大语言模型（LLM）等技术。这些模型在面对现实世界时，会遇到与机器人学类似的复杂性问题。最近，他写了一篇题为「The Tragedies of Reality Are Coming for You（现实的悲剧正在向你袭来）」的博客来阐述这一观点。

![](img/19026d14ab755dc3ce25aa92ea2b5b78.png)

###

# 推特

### Pietro Schirano分享：Claude Engineer 2.0，配备代理

https://x.com/skirano/status/1812943785237639218?s=46&t=GRStLXDcUNuun8J5Noyw4Q

引入 Claude Engineer 2.0，配备代理！ 🚀

这是迄今为止最大的更新，新增了代码编辑器和代码执行代理，以及动态编辑功能。

在编辑文件（尤其是大文件）时，Engineer 会指挥一个编码代理，代理会分批提供更改。

批次是根据文件复杂度智能选择的。

代码执行代理将运行代码并检查问题。

它甚至可以启动（如实时服务器）和结束进程。

功能非常强大！🔥

![](img/969d2f4d80dc805529662448f6bbc4c4.png)

[ssstwitter.com_1721134796934.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/UycwbKWJmo94Q7x1elyc7oOPnmf)

### Yuchen Jin分享：通过训练一个 2.7B GPT-2模型来感受扩展规律

https://x.com/yuchenj_uw/status/1812893615372575180?s=46&t=GRStLXDcUNuun8J5Noyw4Q

在训练了最大的 GPT-2 模型 (1.5B) 之后，我决定"更深入"并通过训练一个 2.7B 模型来感受扩展规律，使用 @karpathy 的 llm.c 📈

扩展模型很简单，主要是堆叠更多的 Transformer 层（使模型更深）。在仅训练了 100B tokens 后，在 HellaSwag 上达到了 63.5% 的准确率，超过了 GPT-3 (2.7B) 在他们的论文中训练 300B tokens（3 倍多的 tokens）后达到的 62.8% 准确率。

训练详情：

+   模型配置：

深度 = 60，通道数 = 1920，头数 = 30（如 github 上 ngc92 所建议）。

+   训练设置：

这是在 4 个 8xH100 节点上训练的，节点间通过 400 Gb/s InfiniBand 互连，训练时间为 34 小时。训练速度为 830k tokens/秒，模型 FLOPs 利用率 (MFU) 为 44.3%。

+   学习率：

我将 GPT-3 论文中的学习率提高了三倍，一如既往 :)

+   训练数据：

FineWeb-Edu

这个 2.7B 模型架构与原始 GPT-3 论文略有不同，但参数规模相似。在引用的推文中，1.5B 模型在训练相同数量的 tokens 后仅在 HellaSwag 上达到了 60.1% 的准确率。

根据 Chinchilla 扩展规律，每个参数 20 个训练 tokens 是实现计算最佳化的经验法则。

+   1.5B 模型在 1.5e9 * 20 = 30B tokens 时应该达到计算最佳化

+   2.7B 模型在 2.7e9 * 20 = 54B tokens 时应该达到计算最佳化

我觉得我们可以做得更深入，也许是时候训练 GPT-3 6.7B 模型了？😉

![](img/9cf7e122258cb13511c5b19420fc42a4.png)

### Auto GPT下一代：让构建、运行和共享 AI 代理比以往更容易，同时提高它们的可靠性

https://x.com/siggravitas/status/1812803289739633018?s=46&t=GRStLXDcUNuun8J5Noyw4Q

引入下一代 AutoGPT

[我们一直在努力开发一些重要的东西，现在是时候与大家分享了。 我们即将推出（预](https://github.com/Significant-Gravitas/AutoGPT/tree/master/rnd/autogpt_server)[览版）下一代 AutoGPT，我们对此非常兴奋。

现在，让我向你们详细介绍一](https://github.com/Significant-Gravitas/AutoGPT/tree/master/rnd/autogpt_builder)下我们正在发布的内容： 它是开源的，目前处于预览版阶段（非常早期，但从第一天起就开源！）。 你现在可以从我们的 GitHub 仓库访问它。 重点是让构建、运行和共享 AI 代理比以往更容易，同时提高它们的可靠性。 该项目有两个主要组成部分：

AutoGPT Server（后端）：https://github.com/Significant-Gravitas/AutoGPT/tree/master/rnd/autogpt_server AutoGPT Builder（前端）：https://github.com/Significant-Gravitas/AutoGPT/tree/master/rnd/autogpt_builder

![](img/ca01ac555647e51fa06c2b034fa505bd.png)

### UMI on Legs：一种在机器人狗上扩展操作技能的方法

https://x.com/haqhuy/status/1812873189430886767?s=46&t=GRStLXDcUNuun8J5Noyw4Q

自从初中以来我一直在训练狗。现在是时候训练机器人狗了 😛

介绍一下，UMI on Legs，这是一种在机器人狗上扩展操作技能的方法🐶 它可以抛掷、推动重物，并使你的~现有~视觉运动策略变得可移动！

![](img/16b8ed03a56cc696869a4099b674055e.png)

[ssstwitter.com_1721135117594.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/H1FJbMqrUoGtCKxCuD4c1957nme)

# 论文

### **Qwen2技术报告**

贡献人：

链接：http://arxiv.org/abs/2407.10671v1

本报告介绍了Qwen2系列，这是我们大型语言模型和大型多模态模型的最新成员。我们发布了一套全面的基础和指导调整的语言模型，包括参数范围从0.5亿到72亿，具有密集模型和专家混合模型。Qwen2超越了大多数先前的开放权重模型，包括其前身Qwen1.5，并在语言理解、生成、多语言能力、编码、数学和推理等多样化基准测试中表现出竞争力。

旗舰模型Qwen2-72B展示了出色的性能：在MMLU上为84.2，在GPQA上为37.9，在HumanEval上为64.6，在GSM8K上为89.5，在BBH上为82.4作为基础语言模型。经过指导调整的变体Qwen2-72B-Instruct，在MT-Bench上达到了9.1，在Arena-Hard上为48.1，在LiveCodeBench上为35.7。此外，Qwen2展示了强大的多语言能力，在大约30种语言中熟练，涵盖英语、中文、西班牙语、法语、德语、阿拉伯语、俄语、韩语、日语、泰语、越南语等，突显了其多功能性和全球覆盖范围。

为促进社区创新和可访问性，我们已在Hugging Face和ModelScope上公开提供了Qwen2模型权重，以及在GitHub上的示例代码等补充材料。这些平台还包括量化、微调和部署资源，促进各种应用和研究工作。

![](img/b96faae2d5f295b9da499e3028d21ebd.png)

### Qwen2-Audio 技术报告

贡献人：

链接：http://arxiv.org/abs/2407.10759v1

我们介绍了Qwen-Audio的最新进展，一个名为Qwen2-Audio的大型音频语言模型，能够接受各种音频信号输入，并进行音频分析或直接对话处理。通过利用自然语言提示简化了预训练过程，拓展了数据量，提升了Qwen2-Audio的指令跟随能力，并实现了语音聊天和音频分析两种交互模式。通过评估结果显示，Qwen2-Audio在音频中心的指令跟随能力测试中表现优异。希望通过开源Qwen2-Audio推动多模态语言社区的发展。

![](img/42e481cffa2cdb28e6600bf24a6dff73.png)

### 竞技学习：通过模拟聊天机器人竞技场为LLM后期培训构建数据飞轮

贡献人：

链接：http://arxiv.org/abs/2407.10627v1

评估大语言模型（LLMs）的有效性存在巨大挑战。在线Chatbot Arena中进行人工注释对抗是一种高效的评估技术，但受限于人工注释所需的成本和时间。本文介绍了一种创新的离线策略——Arena Learning，旨在使用基于AI的注释模拟这些对抗，评估对抗结果，从而通过监督微调和强化学习持续改进目标模型。Arena Learning包括两个关键元素。第一，通过WizardArena确保精准评估并保持离线模拟与在线比赛之间的一致性。第二，根据对抗结果和精炼模型不断改进训练数据。我们应用Arena Learning训练目标模型WizardLM-$\beta$，在各种指标上展现出显著的性能提升。该完全自动化的训练和评估流程奠定了后续对各种LLMs的持续进步的基础。值得注意的是，Arena Learning在WizardLM-2的成功中扮演了关键角色，本文既探讨了其效力，也为与WizardLM-2及其衍生品相关的未来讨论奠定了基础。

![](img/405d99b040414590bbc290736967f30e.png)

### **从自然反馈中学习**

贡献人：

链接：http://arxiv.org/abs/2407.10944v1

人类反馈数据是发展语言模型的关键组成部分。然而，收集这些反馈是昂贵的，最终不可扩展。我们提出了一种可扩展的方法，用于提取用户在与聊天模型交互时自然包含的反馈，并利用它进行模型训练。我们的动机进一步来源于先前的研究表明，使用自然生成的反馈（而不是自动生成的）也具有定量优势，如减少幻觉和偏见。我们手动注释了对话数据，验证了标准语料库中自然发生的反馈存在，并发现高达30%的聊天包含明确的反馈。我们将方法应用于超过100万个对话，获得数十万个反馈样本。训练提取的反馈表现出明显的性能提升，显示了我们方法在增强模型与人类偏好之间的对齐性方面的功效。

![](img/978bef2917f4e364eda095a2bca8cd1e.png)

### Transformer中的加权分组查询注意力

贡献人：

链接：http://arxiv.org/abs/2407.10855v1

注意力机制是Transformer语言模型的基础模块。最近的方法表明，扩展模型可以实现人类水平的性能。然而，随着对扩展的需求增加和硬件内存的限制，这些模型的推理成本仍然很高。为了减少推理时间，在Shazeer等人（2019年）和Ainslie等人（2023年）分别提出了多查询注意力（MQA）和分组查询注意力（GQA）。本文提出了分组查询注意力的一种变体，称为加权分组查询注意力（WGQA）。我们为T5解码器注意力块中的每个关键和值头引入了新的可学习参数，使模型能够在微调期间采用加权平均值。我们的模型在GQA上取得了平均0.53％的改进，并且性能在推理期间没有额外开销的情况下收敛到传统的多头注意力（MHA）。我们评估了这些参数的引入及随后的微调如何在训练期间通知模型有关分组机制，从而增强性能。此外，我们通过比较T5-small和T5-base架构之间的结果，展示了我们分析中的扩展规律。

![](img/8615a7ce4f96acddadaed4fe54c5ff70.png)

### **基础****自动评估者**：驯服大语言模型以实现更好的自动评估

贡献人：

链接：http://arxiv.org/abs/2407.10817v1

随着大型语言模型（LLMs）的发展，由于人力评估的高成本，可靠评估它们的输出变得更具挑战性。为了在更好的LLM评分模型上取得进展，我们介绍了FLAMe，一个基础大型评分模型系列。FLAMe经过训练，在我们的100多项质量评估任务中显著提高了对各种保留任务的泛化能力，优于像GPT-4和Claude-3等专有数据训练的LLMs。我们展示了FLAMe还可作为进一步下游微调的强大起点，以奖励建模评估为案例研究。总的来说，我们的FLAMe变体在8项12个评定者评估基准测试中表现优异。

![](img/00b680ca4bcb4737cd14e1ecf10abf2b.png)

### **NoviCode：****基于编程新手的指令****生成程序**

贡献人：

链接：http://arxiv.org/abs/2407.10626v1

当前的文本到代码模型展示出了生成可执行代码的能力。然而，当前的研究集中在技术指令和面向程序员的语言，有个问题是这些模型能否有效地将非技术用户提供的自然语言描述翻译，并表达复杂目标，转化为包含API访问和控制结构（如循环、条件和序列）的可执行程序中。为解锁从简单的非技术描述生成完整程序的挑战，我们提出了NoviCode，一个新颖的自然语言编程任务，输入为API和新手非程序员提供的自然语言描述，输出为可执行程序。为评估模型在此任务上的有效性，我们提供了一个新的基准测试，并根据程序的功能执行而不是形式来评估生成的代码。我们的实验表明，NoviCode在代码合成领域确实是一个具有挑战性的任务，从非技术指令生成复杂代码超越了当前文本到代码范式。其次，我们展示了一种新的方法，通过将NL话语与代码的分层结构对齐，与端到端文本到代码对应物相比大大提高了LLM在此任务上的性能。

![](img/32e51ca1aa0baebf3486c0521a004a4a.png)

### DOCBENCH: 一个用于评估基于LLM的文档阅读系统的基准测试

贡献人：

链接：http://arxiv.org/abs/2407.10701v1

最近，大语言模型（LLM）开发者对基于LLM的文档阅读系统表现出越来越大的兴趣，这些系统使用户能够上传自己的文档并提出与文档内容相关的问题，超越了简单的阅读理解任务。因此，这些系统已经被精心设计来解决文件解析、元数据提取、多模态信息理解和长篇内容阅读等挑战。然而，目前没有现有基准可以评估它们在这种场景中的表现，即提供原始文件和问题作为输入，期望输出相应的回应。在本文中，我们介绍了DocBench，这是一个旨在评估基于LLM的文档阅读系统的新基准。我们的基准包括精心设计的过程，包括招募人类标注者和生成合成问题。它包括229个真实文档和1,102个问题，涵盖五个不同领域和四种主要类型的问题。我们评估了通过Web界面或API可访问的专有LLM系统，以及采用开源LLM的解析-阅读管道。我们的评估揭示了现有基于LLM的文档阅读系统与人类表现之间的明显差距，突显了开发高效系统的挑战。总而言之，DocBench旨在为评估基于LLM的文档阅读系统在不同真实场景下建立一个标准化基准，从而引导未来在这一研究领域的进展。

![](img/00ceb00dbf7c19c1e03c0e57e89e91f8.png)

# 产品

### BlitzToksAI

贡献人：

https://www.blitztoksai.com/

BlitzToksAI 是一款 AI 驱动的视频生成 SaaS 平台，帮助用户快速创造高质量的社交媒体内容，从而提高曝光度和参与度。它使用 AI 技术自动生成视频，无需复杂的编辑工具。该平台采用按需购买的信用点系统，没有订阅费用。用户可以自定义视频元素，增加创意和独特性。

![](img/951ec5054c9b7fe55251c819a5a635e0.png)

### Shadow

贡献人：

https://www.shadow.do/

Shadow 是一款无需机器人的 AI 会议助手，能够自动运行于各类视频/语音会议中，主动协助完成任务管理、自动记录会议洞见，并拥有 Copilot 模式，帮助用户提升会议效率和生产力。

![](img/8f59f3d8959b40a585afbd5d7f453882.png)

# HuggingFace&Github

### llm-graph-builder

贡献人：

https://github.com/neo4j-labs/llm-graph-builder

该应用程序旨在将非结构化数据（如 PDF、文档、文本、YouTube 视频和网页等）转换为存储在 Neo4j 中的知识图谱。它利用大型语言模型（如 OpenAI 和 Gemini）的强大功能从文本中提取节点、关系及其属性，并使用 Langchain 框架创建结构化知识图谱。

### baml

贡献人：

https://github.com/BoundaryML/baml

BAML 是一种领域特定语言（DSL），用于编写和测试大语言模型（LLM）函数。它可以帮助开发人员更快地编写和测试复杂的 LLM 函数，并提供类型验证、流式输出、实时提示预览等功能。BAML 还提供了一个观察平台 Boundary Studio，用于可视化和跟踪 LLM 函数的执行情况。

![](img/cda642c7394ddda0bcc0379b8f4a2730.png)

# 投融资

### 后摩智能完成数亿元战略融资，携手中国移动产业链发展基金

贡献人：

https://new.qq.com/rain/a/20240715A07SPT00

后摩智能创立于2020年底，由吴强博士和多位业内顶级芯片及市场专家组建，专注于大算力智能计算芯片的设计、研发及应用。后摩智能基于自主研发的存算一体技术及先进的存储工艺，面向无人车等边缘端及云端推理和训练领域，打造出能效比数十倍高的大算力芯片，致力于成为国内第一家单芯片算力达到1000TOPS的公司，让每个场景都能用上AI计算。近日，后摩智能完成数亿元人民币的战略融资，由中国移动旗下北京中移数字新经济产业基金、上海中移数字转型产业基金共同对公司进行投资。

公司官网：https://www.houmoai.com/

![](img/be58f84bddd4c1a76e25df4ff45abe25.png)

### Adaptive构建自动化工具加速建筑支付

贡献人：

https://techcrunch.com/2024/07/15/adaptive-builds-automation-tools-to-speed-up-construction-payments/

Adaptive是一家旨在通过自动化工具加速建筑支付流程的初创公司，最近完成了由Emergence Capital领投的1900万美元A轮融资。这使得Adaptive的总融资额达到2640万美元。参与此次融资的还有Andreessen Horowitz、Definition、Exponent等。Adaptive计划利用这笔资金将员工数量从29人增加到45人，并专注于为分包商客户定制产品。公司目前服务于280多家建筑公司，致力于简化财务管理工作流程，通过嵌入式金融服务提升小型和中型企业的财务效率。

公司官网：https://www.adaptive.build/

![](img/0a07b7c51dcb4421b04e2aaa91d1185e.png)

### Presti利用生成式AI提升家具行业产品摄影

贡献人：

https://techcrunch.com/2024/07/15/presti-uses-generative-ai-to-improve-product-photography-in-the-furniture-industry/

法国初创公司Presti利用生成式AI将单张产品图片转换为逼真照片，显著降低家具公司产品摄影的时间和成本。Presti近日完成了由Partech领投的350万美元种子轮融资，其他投资者包括Thibaud Elzière、Maxime Brousse等。Presti目前与法国大型家具零售商Maisons du Monde达成战略合作，通过其平台，用户可以为产品添加配件并生成真实的影像效果。Presti的技术基础是Stable Diffusion XL模型，专为家具行业的产品图像进行了优化。

公司官网：https://www.presti.ai/

![](img/f0bd410546078ebb80d4840e92285849.png)

# 学习

### 持续的故事：将在人工智能芯片上花费多少钱？

贡献人：

https://www.nextplatform.com/2024/07/15/ongoing-saga-how-much-money-will-be-spent-on-ai-chips/

文章提到了 AMD 首席执行官苏姿丰对数据中心 AI 加速器市场潜力的评估，预测市场规模将从 2023 年的 450 亿美元增长到 2027 年的 4000 亿美元。富国银行和 Gartner 提供了数据中心 GPU 销售额的增长预测，预计到 2027 年将达到 953 亿美元。IDC 的分析显示，AI 芯片内容的收入从 2022 年的 421 亿美元增长到 2023 年的 691 亿美元，并预计到 2027 年将达到 1933 亿美元。同时，AI 服务器的收入在 2023 年增长了 3.2 倍，占市场的 23%，预计到 2027 年将推动 491 亿美元的销售额。

![](img/0eafb0e418ec1e867aa46b2257b7ba3d.png)

### AI fabric is a bus or a network？

贡献人：

https://zhuanlan.zhihu.com/p/708602042?utm_psn=1796556145168625664

AI Fabric 是一种集成计算、存储和网络功能的新型架构，旨在优化人工智能计算任务的执行效率。它通过采用内存语义（Load/Store/Atomic）和异步 DMA 操作，实现了高效的数据交换机制，提供了较低的延迟和更高的吞吐量。AI Fabric 在处理稀疏和低精度计算时，能够更好地发挥其优势，适应 AI 计算的特点。此外，AI Fabric 支持多路径和负载均衡，通过随机选路和带宽均衡技术，提高了系统的可靠性和资源利用率。然而，AI Fabric 也面临着一些挑战，包括系统可维护性、测试性能以及与现有网络协议栈的兼容性问题。为了克服这些挑战，需要在计算和网络领域的专家之间进行更多的合作和技术交流。

![](img/70032b728664174fc9d2718b6b26d0ab.png)

### FPGA前沿 Intel NPU 设计实现 与英伟达GPU性能对比测试

贡献人：

https://zhuanlan.zhihu.com/p/708760328?utm_psn=1796556280275558400

本文介绍了 Intel 在 FPGA 上实现 NPU 的设计和与英伟达 GPU 的性能对比测试。NPU 采用高度并行化的结构，包含矩阵向量乘法单元（MVU）、外部 VRF（eVRF）块、多功能单元（mfu）和加载器（LD）等，以支持低延迟、低批量的 AI 推理。MVU 结构采用 5 个宏操作（mOPs）的 VLIW 指令，通过流水线技术优化数据路径。代码实现中，通过模拟和实际测试验证了 NPU 的性能，显示出在特定 AI 任务中，FPGA 结合 NPU 能够实现与 GPU 相当或更优的性能。

![](img/37bd95a5a1bbc6f8a010688e1ca654e4.png)

### 昇腾大模型|热点算法1——Direct Preference Optimization（DPO）直接偏好优化

贡献人：

https://zhuanlan.zhihu.com/p/708652317?utm_psn=1796556670362595328

昇腾大模型中的 Direct Preference Optimization（DPO）算法，是一种创新的机器学习优化方法。与传统的损失函数优化不同，DPO 直接针对预测偏好进行优化，通过定义偏好函数来量化模型输出与理想偏好之间的差异，并利用梯度下降法进行参数调整。这种方法使得模型能够更加贴近实际应用场景的需求，提高了模型的准确性和实用性。在昇腾大模型的实验中，DPO 展现了优异的性能，能够在较短的训练周期内达到更高的优化效果，并在多个场景下证明了其优越的泛化能力。此外，DPO 算法还有助于提升模型的解释性，使得模型的决策过程更加透明。未来，DPO 算法有望在更广泛的机器学习领域中得到应用，推动人工智能技术的发展。

![](img/7be8c000c65e8bfc8402a69327c4cbf4.png)

### GPGPU 和 CUDA 统一内存研究现状综述

贡献人：

https://zhuanlan.zhihu.com/p/708786413?utm_psn=1796548081971499011

GPGPU 技术利用 GPU 进行通用计算，CUDA 作为 NVIDIA 的并行计算平台，为 GPGPU 提供了支持。统一内存是 GPGPU 的关键技术，它消除了主机与设备之间内存管理的差异，提高了数据处理效率。在技术细节上，统一内存通过将 CPU 和 GPU 的内存抽象为一个统一的地址空间，实现了自动内存管理，减少了程序员的工作量。性能优化方面，通过缓存机制、数据预取和内存访问模式的优化，进一步提升了计算效率。然而，统一内存也面临着内存一致性、性能开销等挑战，未来研究将致力于解决这些问题，提出更高效的内存管理策略。

![](img/d6a306458623e174e6816d9f711d3a7d.png)

### 我做了一个 AI 搜索引擎

贡献人：

https://zhuanlan.zhihu.com/p/705463402?utm_psn=1796556778294620160

文章介绍了一个名为ThinkAny的AI搜索引擎。它使用RAG（Retrieval-Augmented Generation）技术快速检索和聚合高质量内容，并通过AI智能回答用户问题。该引擎支持多语言，包括英语、中文、韩语、日语等，目标是提供更快更准确的搜索体验。创始人受启发于贾扬清的开源项目"Lepton Search"和其他AI搜索引擎源码，决定创建这款产品。

![](img/f8f39308f5e336c4867b5220f0b5c488.png)

### 2024年中国AI基础数据服务研究报告

贡献人：

https://mp.weixin.qq.com/s/GwrlGSrbcHNZGHDbiX2KiQ

2024 年中国 AI 基础数据服务研究报告指出，AI 产业发展趋向于多模态和大模型小型化的方向，数据服务市场规模预计将实现 30.4% 的复合增长率。数据、算法和算力是 AI 技术发展的三大支柱，其中数据服务厂商通过提供标准和定制数据集，以及配套的工具服务，支持 AI 算法的研发与优化。大模型在需求数据量和多模态数据处理方面提出了更高要求，同时评测工具和服务的发展对于模型的商业化应用至关重要。智能驾驶领域的数据服务则需要处理复杂的多传感器数据，以提高自动化驾驶的准确性和鲁棒性。市场结构分析显示，自建团队和品牌数据服务商市场份额上升，而中小服务商份额下降。未来发展的关键在于强化自动化平台能力，深入行业理解，以及对技术与数据的前瞻性布局。

![](img/4c07d721fd5a562f149655d6ca9d902a.png)

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

![](img/0055c13f1ce70ebd4135b6b59180c17b.png)