
+ 推出参数量更大、多模态应用更强、以及专门用于代码或数学建模的`API`模型；

+ 打破上下文限制，设定目标为`100万个tokens`；同时也将提高推理速度，降低推理成本；

+ 以超长上下文为基础，构建了包括向量数据库、`RAG`、`Agent`架构等全新的开发者AI框架，以适应更多样化的应用场景。

参考博客链接：`https://www.51cto.com/article/783692.html`

![](img/85f7bc73d8388bf3498a246e237a20da.png)

1. 【苹果发布`MM1`多模态大语言模型，最高为`30B`参数规模】苹果近日发布了它自家的大语言模型`MM1`，这是一个最高达到`30B`规模的多模态`LLM`。根据苹果的研究文档，图像分辨率、图像编码器的预训练数据以及模型规模对性能有显著影响。视觉和语言连接器应用的设计则相比之下影响较小。同时，预训练数据的混合比例对少样本和零样本（`zero-shot`）性能至关重要。通过预训练和`SFT`（`Supervised Fine-Tuning`，监督式微调），`MM1`模型在多个基准测试中取得了`SOTA`性能。模型展现了一些令人兴奋的特性，比如上下文内预测、多图像推理和少样本学习能力。模型的实现方案分为架构组件和数据选择，以及模型构建和预训练等几部分，最后通过对模型在多个基准测试中的性能进行评估，`MM1`展现出了有竞争力的成果。参考博客链接：`https://arxiv.org/pdf/2403.09611.pdf`

![](img/f2af28fde2505416a7bba10f27d81e52.png)

## 🥰 今日份Prompt技巧

`Use Delimiters.` 使用分隔符。（技巧来源于：`https://arxiv.org/pdf/2312.16171.pdf`）

# 3月14日 Sora计划2024年内发布

## 🤖 AIGC每日新闻

1. 【`Devin`：全球首个自主AI软件工程师】`Devin`由`Cognition`实验室开发，旨在通过先进的长期推理和规划能力，协助工程师解决复杂任务。`Devin`能在沙盒化环境中使用常见开发工具，并能实时与用户协作，接受反馈。在`SWE-bench`编码基准测试中，`Devin`的表现超越了以往任何模型，正确解决了`13.86%`的问题。`Cognition`实验室获得了`Founders Fund`领投的`2100万美元A轮融资`，致力于开发具有超越现有AI工具能力的AI团队。`Devin`目前处于早期访问阶段，有兴趣使用或加入团队的可以通过指定的联系方式了解更多信息。参考博客链接：`https://www.cognition-labs.com/blog`

![](img/5af1bb44eed2ed80a79eaec5654cfdcd.png)

[`videoplayback.webm`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/QehDbOLjbol7eJxwMf2cHPZTnG5)

1. 【`Meta`正在开发两个拥有`2.4万GPU`的超级计算集群，以支持各种AI任务】`Meta`公司正在开发两个拥有`2.4万GPU`的超级计算集群，并计划在`2024`年底使其综合计算能力接近`60万个` `H100 GPU`。这些集群将被用于支持多种人工智能任务，诸如训练大型AI模型`Llama 3`。`Meta`强调他们秉持开放式计算和开源技术理念进行开发，所以这些集群是基于`Grand Teton`、`OpenRack`和`PyTorch`等平台建构的。这些集群的设计强调了性能提升，可靠性保障及可扩展性，使其可以支持大型且复杂的AI模型。此外，`Meta`采取了一系列策略，例如设计定制化硬件设备，采用如`RDMA over converged Ethernet (RoCE)` 和`NVIDIA Quantum2 InfiniBand`网络技术等高级网络解决方案，以及优化存储方案如利用`FUSE API` 和 `Tectonic` 分布式存储等技术。这些举措旨在进一步优化软件性能，并通过与如`PyTorch`这样的框架开发者合作，使得AI集群在大幅度提升性能的同时，依然保持易于使用。参考博客链接：`https://engineering.fb.com/2024/03/12/data-center-engineering/building-metas-genai-infrastructure/`

![](img/5031fa4d169e8f47b9b9310542470787.png)

1. 【`OpenAI`揭秘`Sora`：计划在`2024`年内发布，视频生成只需几分钟】`OpenAI`在接受采访时，提供了关于其新项目`Sora`的更多细节：

+ `Sora`计划在`2024`年内发布；

+ 与历史版本相比，`Sora`能在几分钟内生成`20`秒的`720p`视频，大大提升了视频生成速度；

+ AI生成视频的最大挑战在于保持画面帧间的连贯性，而这也是`Sora`的特长之一；

+ 为了避免错误信息的传播和防止滥用，`Sora`将采用与`Dall-E`相似的内容过滤政策，例如限制生成公众人物的内容。

这些信息表明，`OpenAI`正努力在加速AI视频生成的同时，也致力于防止其产生的信息被错误使用。

参考博客链接：`https://x.com/jasonzhou1993/status/1768034822658949307?s=20`

## 👻 今日份Prompt技巧

Assign a role to the large language models.为大型语言模型分配角色（技巧来源于：`https://arxiv.org/pdf/2312.16171.pdf`）

# 3月13日 GPT-4.5 Turbo即将问世？速度快，精准度高，可扩展性强。

## 🥳 AIGC每日新闻

1. 【`Meta`发布将大语言模型融合入`MOE`的新方法`Branch-Train-MiX`】`Branch-Train-MiX` (`BTX`) 是一种提高大型语言模型（`LLMs`）在多个专业领域（如编程、数学推理和世界知识）能力的训练方法。它从一个种子模型开始，异步并行地训练专家模型，然后将这些专家的前馈参数混合到一个混合专家（`MoE`）层中，并进行`MoE`微调以学习令牌级路由。`BTX`在准确性和效率之间取得了最佳平衡，与`Branch-Train-Merge`和稀疏升级等替代方法相比，`BTX`在所有任务上都显示出更高的训练吞吐量和更平衡的性能。参考论文链接：`https://arxiv.org/pdf/2403.07816.pdf`

![](img/c6f8adbf82a898d4b88e6beb3798befa.png)

1. 【`OpenAI`即将推出`GPT-4.5 Turbo`：速度快，精准度高，可扩展性强】`OpenAI`的`GPT-4.5 Turbo`的相关信息可能已提前泄露，`Bing`和`DuckDuck Go`等搜索引擎索引了产品页面，尽管官方尚未宣布。这款模型被称为`OpenAI`迄今为止开发的速度最快、准确度最高，且扩展性最强的模型。消息显示，`GPT-4.5 Turbo`能够一次性处理的数据量高达`256,000`个`Token`，是`GPT-4 Turbo`的两倍，约相当于`200,000`个词。如果它能在处理大量信息时避免忽略重要内容，将会是一大进步。目前，谷歌正在测试一款具备强大处理能力的大模型`Gemini 1.5`，它也显示出了类似的潜力。预览信息还透露，`GPT-4.5 Turbo`将包含截至`2024`年`6`月的最新信息，意味着新模型预计将在`6`月份发布，届时很可能会与近期推出的竞争对手模型如`Anthropic`的`Claude 3`展开竞争。参考博客链接：`https://the-decoder.com/openais-gpt-4-5-turbo-leaked-on-search-engines-and-could-launch-in-june/`

1. 【`Google`首次披露模型窃取攻击，能精确提取黑盒语言模型信息】`Google`的论文《`Stealing Part of a Production Language Model`》介绍了模型窃取攻击，能够从像`OpenAI`的`ChatGPT`或`Google`的`PaLM-2`这样的黑盒生成语言模型中提取出精确，重要的信息。这种攻击通过`API`访问恢复了变换模型的嵌入投影层（直到对称性），并确认了这些模型的隐藏维度分别为`1024`和`2048`。论文还讨论了潜在的防御措施和未来工作可能扩展攻击的影响。参考论文链接：`https://arxiv.org/pdf/2403.06634.pdf`

## 😵 今日份Prompt技巧

To inquire about a specific topic or idea or any information and you want to test your understanding, you can use the following phrase: “`Teach me any [theorem/topic/rule name] and include a test at the end, and let me know if my answers are correct after I respond, without providing the answers beforehand.`” 要询问特定主题或想法并测试您的理解，您可以使用以下短语：-“教我`[定理/主题/规则名称]`并在最后进行测试，但不要给我答案，并且然后在我回答时告诉我答案是否正确”（技巧来源于：`https://arxiv.org/pdf/2312.16171.pdf`）

# 3月12日 xAI本周将开源`Grok`

## 🤠 AIGC每日新闻

1. 【`OpenAI`推出工具`"Transformer Debugger"`，帮助研究语言模型的具体行为】`OpenAI`的`Superalignment`团队成功研发并推出了一款名为`"Transformer Debugger"`（简称`TDB`）的高效工具，旨在深入研究小型语言模型的精细运作过程。这款工具巧妙地将自动化理解技术与稀疏自动编码器相融合。这款工具的强大实用性体现在，用户能在编写代码之前进行快速的初始探索，并且能实时干预正向传播过程，观察这背后对特定模型行为的影响力。例如，`"为何模型在接收到特定提示后，选择输出token A而不是token B？"` `"为何在接收到某一提示后，注意力机制会专注于token T？"`等问题，均能通过`Transformer Debugger`找到答案。`TDB`通过识别导致模型特定行为的特定组成元素（例如神经元、注意力层、自动编码器的隐层变量等），并且能展示出自动生成的解析，以阐明是何种因素最直接地引发了这些神经元或机制的激活。更进一步地，`TDB`能追踪这些神经元或者机制之间的相互关联，帮助用户理解深度学习模型的内部电路工作方式。这次公布了一系列工具包括神经元观测工具，激活服务器，以及模型等部分。神经元查看器是基于`React`的应用程序，其承载了`TDB`以及关于各个模型组件的相关信息。同时，激活服务器作为后端服务，能对主题模型进行推断分析，为`TDB`提供所需数据，同时也能从公共的`Azure`存储桶中读取和提供数据。至于模型部分，则是一个为`GPT-2`模型及其自动编码器设计的简易推理库，内部集成了抓取激活钩子的功能。参考博客链接：`https://github.com/openai/transformer-debugger`

1. 【首款大规模文本到视频提示数据集`"VidProM"`发布】`Sora`的出现为文本到视频扩散模型带来了新的时代，它在视频生成和潜在应用方面带来了重大的进步。然而，`Sora`以及其他文本到视频扩散模型，高度依赖于提示，而且没有大规模公开可用的对文本到视频提示进行研究的数据集。在这篇论文中，介绍了`VidProM`——首个包含了`167`万个真实用户的唯一文本到视频提示的大规模数据集。此外，该数据集还包括`669`万个由四个最先进的扩散模型生成的视频以及一些相关数据。我们首先展示了这个大规模数据集的策划过程，这是一个耗时且成本高的过程。然后，展示了所提出的`VidProM`与`DiffusionDB`（一个大规模的为图像生成的提示-图库数据集）的区别。基于这些提示的分析，认为有必要为特别设计的文本到视频生成提供一个新的提示数据集，并了解到真实用户在创建视频时的偏好。本次大规模和多样化的数据集也激发了许多令人兴奋的新研究领域。例如，我们建议探索文本到视频提示工程、有效的视频生成和扩散模型的视频复制检测，以制定更好、更高效、更安全的文本到视频扩散模型。我们根据`CC-BY-NC 4.0` 许可在`GitHub`和`Hugging Face`上公开提供了收集的数据集`VidProM`。参考博客链接：`https://huggingface.co/papers/2403.06098`

1. 【`xAI`本周将开源`Grok`】本周，`xAI`将公开`Grok`源代码。这表示用户可以查看、使用并协助改进`Grok`，这对于未来`Grok`的发展与完善或将产生重要影响。参考博客链接：`https://x.com/elonmusk/status/1767108624038449405?s=20`

1. 【`Midjourney`发布创新特色功能`“角色参考”`，实现跨图像间的角色一致性生成】`Midjourney`发布了角色一致性的新功能，这项功能声称能够在多个参考图像之间生成一致的角色。如何使用？你只需要在角色图像`URL`后的提示中键入`“–cref URL”`。可以通过使用`“–cw”`将参考的`“强度”`从`100`调整至`0`。默认强度`100`（`–cw 100`）会考虑脸部、头发和衣着。而强度`0`（`–cw 0`）只关注面部（适用于换装或换发型）。可以通过使用`“–cref URL1 URL2”`（类似于多图像或风格提示）将来自多个图像的信息融合。参考博客链接：`https://x.com/Analyticsindiam/status/1767435537252753690?s=20`

## 🤓 今日份Prompt技巧

Allow the model to elicit precise details and requirements from you by asking you questions until he has enough information to provide the needed output (for example, “`From now on, I would like you to ask me questions to ...`”).让模型通过向您提出问题来引出您的精确细节和要求，直到它有足够的信息来提供所需的输出 -“从现在开始，我希望您向我提问......”。（技巧来源于：`https://arxiv.org/pdf/2312.16171.pdf`）

# 3月11日 OpenAI董事会扩容，聚焦用AI普惠人类

## 😇 AIGC每日新闻

1. 【苹果正为`Siri`进行升级，悄然构建AI软硬件基础】苹果公司正为`Siri`进行重大升级，从而在AI助手领域与`ChatGPT`和`Gemini`竞争。`Siri`的共同创造者`Dag Kittlaus`暗示`2024`年将有新功能。苹果自`2017`年以来收购了`21`家AI初创公司，并发布了重要AI研究。传闻中的`“AppleGPT”`大型语言模型预计将在`2024`年`6`月的`WWDC`上与`Siri`整合。新`M3`芯片的AI性能也表明苹果在设备上高效运行AI模型方面取得了进展。尽管竞争对手急于利用`ChatGPT`的热潮，苹果却在悄然构建AI软硬件基础，旨在为数十亿设备服务，让苹果的设备在AI竞赛中仍处于领先地位。某推参考：`https://twitter.com/rowancheung/status/1766152736620597539?s=20`

1. 【`OpenAI`董事会扩容，聚焦AI普惠】`OpenAI`官方宣布其董事会新增三位成员：前比尔·盖茨基金会首席执行官苏·德斯蒙德-赫尔曼博士，索尼公司前执行副总裁兼总法律顾问妮可·塞利格曼，以及`Instacart`的首席执行官兼董事长菲吉·西莫。此外，`OpenAI`的首席执行官萨姆·奥特曼将重新加入董事会。这些成员在全球领导力、技术和复杂的监管环境方面拥有丰富的经验，这与`OpenAI`确保人工智能普遍惠及人类的使命相契合。官方博客链接：`https://openai.com/blog/openai-announces-new-members-to-board-of-directors`

1. 【意大利数据保护机构调查`Sora`，要求`OpenAI`数据合规】意大利数据保护机构宣布对`OpenAI`新推出的人工智能视频工具`Sora`展开调查。该机构关注`Sora`对欧盟尤其是意大利用户的个人数据处理可能产生的影响，并已要求`OpenAI`就此提供澄清证明。`Sora`目前仍处于测试阶段，能够根据简单的用户提示创建长达一分钟的真实视频。监管机构要求`OpenAI`在`20`天内提供关于收集和使用的数据类型、是否涉及个人数据以及是否符合欧洲数据保护规则的信息。此前，意大利曾因`ChatGPT`未遵守欧洲规定而暂时封锁该聊天机器人。参考博客链接：`https://techxplore.com/news/2024-03-italy-probe-openai-video-tool.html`

## 😍 今日份Prompt技巧

Add to your prompt the following phrase “`Ensure that your answer is unbiased and avoids relying on stereotypes.`”请确保你的回答是公正无偏见的，并且避免依赖于刻板印象。（技巧来源于：`https://arxiv.org/pdf/2312.16171.pdf`）

# 3月10日 Pika新增AI音效，`1.0`已向所有用户免费开放

## 🙂 AIGC每日新闻

1. 【`Salesforce`发布医疗AI工具，简化行政流程，减轻医生负担】`Salesforce`最近宣布了针对医疗工作者的新型AI工具，这些工具旨在简化繁琐的行政流程，减轻医生的工作压力。其中，`Einstein Copilot: Health Actions`工具通过对话式语言提示，使医生能够轻松预约、总结患者资料和发送转诊。另一个工具`Assessment Generation`则允许医疗机构数字化健康评估，无需手动输入或编码。这些功能都建立在`Salesforce`的`Einstein 1`平台之上，该平台能够整合来自保险索赔系统和电子健康记录等多种来源的医疗数据。这些工具的推出，有望帮助医生减少因数据分散在不同数据库和格式中而导致的查找信息的时间消耗。`Salesforce`表示，这些工具将在今年夏季遵守`HIPAA`规定，并计划在年底前投入使用。
