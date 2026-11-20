
# 引言

为了缓解处理长序列中的自注意力机制的计算复杂性，线性注意力利用计算技巧实现了线性复杂度，同时状态空间模型（SSMs）普遍采用一种良好的做法，即使用非数据依赖的记忆模式，即强调近处而忽略远处，来处理序列。最近的研究表明了通过将它们结合为一个整体可以获得优先级。然而，线性注意力的效率仍然仅停留在理论级别上，在因果设置中，而SSMs需要各种设计良好的约束条件才能有效地处理特定数据。因此，为了揭示混合设计真正的威力，需要解决以下两个问题：（1）线性注意力的硬件高效实现和（2）SSMs的稳定性。为了实现这一目标，我们利用平铺和层次分层的思想提出了CHELA（短长卷积与硬件高效线性注意力），它用短长卷积替换了SSMs，并以分治方式实现线性注意力。这种方法享有来自稳定SSM和线性注意力的全局抽象和数据依赖选择，同时保持真正的线性复杂度。我们在Long Range Arena基准和语言建模任务上进行了全面实验，证明了所提出方法的有效性。

![](img/8268ad05a4e0ed902654c493ba98833e.png)

# 项目推荐

### UICoder：通过自动反馈微调大语言模型以生成用户界面代码

贡献人：

链接：`http://arxiv.org/abs/2406.07739v1`

大语言模型(LLMs)在生成符合视觉设计要求且可编译的UI代码时存在困难。现有方法依赖昂贵的人工反馈或提炼专有模型来改进生成过程。本文提出使用自动化反馈(编译器和多模态模型)引导LLMs生成高质量UI代码。我们的方法从现有LLM开始，通过自动生成大规模合成数据集，利用自动化工具对数据进行严格过滤、评分和去重，生成更优质的数据集，然后通过在此数据集上微调原LLM来改进模型。我们将这种方法应用到几个开源LLMs上，并通过自动化指标和人类偏好与基准模型进行比较。评估结果显示，生成的模型胜过所有其他可下载的基线模型，并接近较大的专有模型性能。

![](img/e2807be33bba82321f9678b79971ec46.png)

# 产品

### Namify AI

贡献人：

`https://namify.tech/?`

Namify AI 是一款免费的企业名称生成器,它不仅使用 AI 提供最适合用户产品或初创公司的品牌名称建议,还可以进行商标、域名和社交媒体用户名检查,让用户自定义名称以匹配品牌风格,并支持多种语言,还免费提供品牌徽标设计,帮助用户快速找到可用且有吸引力的品牌名称,是一款专为企业主和创业者设计的全面性名称生成和检查工具。

![](img/0a727a9ac406bd83eefbdd5076edf867.png)

### Autodesigner 2.0

贡献人：

`https://uizard.io/?ref=producthunt`

Autodesigner 2.0 是 Uizard 公司推出的一款 AI 驱动的 UI 设计生成工具,将 ChatGPT 的对话交互和 Uizard 的 UI 设计功能相结合,帮助用户快速将创意转化为数字产品概念。Autodesigner 2.0 通过持续的技术迭代升级,已经成为 Uizard 平台上超过 80% 新 UI 的生成引擎。

![](img/a4b81e2231624cac9adc56de6dd07f7d.png)

# HuggingFace&Github

### L3-8B-Stheno-v3.2

贡献人：

`https://huggingface.co/Sao10K/L3-8B-Stheno-v3.2`

L3-8B-Stheno-v3.2 在训练过程中融合了多种数据集,包括短篇故事生成、指令式对话以及聊天日志数据。与上一版本相比,该模型在处理 SFW 和 NSFW 内容、故事写作、助手任务等方面都有所改进,同时在多轮对话连贯性和提示遵循度上也有所提升,尽管创造力略有下降。作者推荐使用特定的采样设置来充分发挥该模型的性能。

![](img/664d70ac4b5f970fed5ca2e23d211c86.png)

### Aidapal

贡献人：

`https://github.com/atredispartners/aidapal`

aidapal 是一个集成了大型语言模型的 IDA Pro 插件,利用人工智能技术来辅助逆向工程师分析二进制代码。它使用了微调过的 LLM 模型来生成 Hex-Rays 伪代码,帮助提高代码分析的效率和准确性。aidapal 的源代码、模型权重和训练数据都可以从 Hugging Face 上下载,开发者可以自行部署和使用。这个插件结合了机器学习和逆向工程领域的前沿技术,为二进制代码分析带来了新的解决方案。

![](img/f622d1e7ec12a469b0a28b6e56d3d1a5.png)

# 投融资

### Black Semiconductor 融资2.74亿美元以推动欧洲芯片发展

贡献人：

`https://www.cnbc.com/2024/06/12/black-semiconductor-raises-274-million-in-funding-in-europe-chip-push.html`

Black Semiconductor是一家致力于开发下一代芯片技术的初创公司，宣布筹集了2.544亿欧元（约2.74亿美元）。其中，德国经济事务部和北莱茵-威斯特法伦州政府提供了2.287亿欧元（约2.48亿美元）的公共资金，剩余的2570万欧元（约2800万美元）由保时捷创投和风险投资公司Project A领投。资金将用于在德国亚琛建立试生产设施，并计划在2031年前实现大规模生产。此外，公司计划在2026年前将员工人数从30人增加到120人。此举显示了欧洲在半导体制造领域追赶美国的雄心。

公司官网：`https://blacksemi.com/`

![](img/c6ad3502015a710e0aa2ceee992f9d35.png)

### BlinqIO 融资500万美元以利用生成式AI自动化软件测试

贡献人：

`https://siliconangle.com/2024/06/12/blinqio-raises-5m-automate-software-testing-using-generative-ai/`

BlinqIO是一家生成式AI软件测试初创公司，宣布完成了500万美元的融资，主要投资者为Flint Capital和Inovia Capital的Precede Fund I。BlinqIO开发了业内首个AI测试自动化工程师，能够自主创建和维护测试自动化代码，无需人工监督。该技术利用专有的大型语言模型，支持多达50种语言，旨在提高软件测试的效率。融资将用于进一步发展其自动化测试工具，帮助企业更频繁地推出新软件产品和服务。

公司官网：`https://blinq.io/`

![](img/3287d65e39a32010b3bf878682fcb579.png)

### Pyte 融资500万美元以推动企业私密和安全的数据利用与协作

贡献人：

`https://www.prnewswire.com/news-releases/pyte-announces-5m-in-funding-to-advance-private-and-secure-data-utilization-and-collaboration-for-every-enterprise-302170244.html`

Pyte宣布获得500万美元的新一轮融资，由Myriad Venture Partners领投，Innovation Endeavors、Liberty Mutual Strategic Ventures和Pillar VC等参投，使总融资金额超过1200万美元。Pyte利用其专有的安全多方计算（SMPC）技术，允许企业在数据加密状态下进行复杂的AI和机器学习计算。此次融资将用于加速其技术在金融服务和医疗等高度监管行业的商业化应用，帮助企业在保证数据隐私和安全的前提下实现数据协作。

公司官网：`https://www.pyte.ai/`

![](img/a92667f960be8b06d644c8fd0842880f.png)

### InScope 融资430万美元以推动自动化财务报告

贡献人：

`https://techcrunch.com/2024/06/12/sheel-mohnot-inscope-financial-reporting/`

InScope是一家自动化财务报告的金融科技初创公司，近日宣布完成了由Lightspeed Venture Partners领投的430万美元种子轮融资。InScope利用机器学习和大型语言模型为中型市场和企业提供财务报告和审计服务。公司将利用这笔资金扩展产品、扩大团队并增强AI能力，目标是在年底前将客户数量增加到50家。参与投资的还有Together AI的创始人兼CEO Vipul Ved Prakash和Casetext的创始人兼CEO Jake Heller等个人投资者。

公司官网：`https://www.inscopehq.com/`

![](img/be06090263eea5bb926c07443eb08b13.png)

# 学习

### 潞晨训推一体机，助力算力高效利用

贡献人：

`https://mp.weixin.qq.com/s/kVF2fOR8_i7EVoUrt3vBIg`

潞晨科技推出的训推一体机，采用英伟达 H20 GPU，每台配备 8 张卡，每卡 96GB 显存，支持 32B Qwen1.5 模型全参数微调和 34B Yi1.5 模型推理。该机器利用 Colossal-AI 加速系统，在 8 卡规模下，平均每卡算力提升 21%，实现快速模型微调和推理。集成了多种开源模型，如 LLaMA3、Mixtral、Qwen 等，并提供了 Colossal LLM Studio 软件，实现了低代码、可视化的模型训练和微调。此外，还提供了 Colossal Reader 和 Open-Sora 等应用，分别针对文档理解和视频生成进行了深度优化。这款一体机的推出，旨在解决企业在 AI 落地过程中的算力利用问题，降低技术门槛，促进 AI 技术的商业化应用。

![](img/79d349f2441b4bbe37f56ac4ebe23b0f.png)

### 如何理解Nvidia英伟达的Multi-GPU多卡通信框架NCCL？

贡献人：

`https://www.zhihu.com/question/63219175?utm_psn=1784234882622324736`

NCCL 库：NVIDIA NCCL 库是一个针对多 GPU 并行通信优化的库，支持深度学习框架中的 collective communications，如 `all-reduce`, `broadcast`, `reduce` 等。

性能优化：NCCL 在不同硬件环境下进行了优化，特别是在 NVIDIA 的 GPU 上，通过利用 NVLink 和 InfiniBand 技术，显著提高了通信性能。

Ring-Allreduce 算法：在单机多卡场景下，NCCL 采用 Ring-Allreduce 算法，通过数据切片和环形通信结构，实现高效的数据传输和聚合。

多机通信：对于多机多卡的环境，NCCL 使用了 Double Binary Tree 和 CollNet 技术，后者基于 SHArP 协议，可在网络交换机上进行数据聚合，减少通信延迟和带宽压力。

硬件拓扑 sensing：NCCL 能够自动探测系统的链路拓扑，根据探测结果创建最优的通信环路，从而实现最佳的通信性能。

与深度学习框架集成：NCCL 与主流深度学习框架兼容，允许开发者在这些框架中无缝集成，以便进行高效的并行计算。

实际性能测试：文章中包含了在不同的 GPU 配置（如 DGX-1 V100、DGX A100）下进行的 NCCL 性能测试，实际案例显示 NCCL 能够提供高达 130GB/s 甚至更高的通信带宽。

![](img/04839271bf7a647870b303314ed99fc1.png)

### 当"狂飙"的大模型撞上推荐系统

贡献人：

`https://mp.weixin.qq.com/s/hQ5MYFxdKA4dgXetFEc_Bg`

大型人工智能模型如 GPT-3 和 BERT，利用深度学习技术，能够高效处理自然语言数据，显著提升文本理解和生成的能力。这些模型通过大量数据训练，掌握了语言的复杂模式和结构，从而在搜索引擎和推荐系统中提供更加精准的服务。技术细节上，GPT-3 采用 1750 亿参数的转换器模型，通过无监督学习方式从互联网数据中学习语言知识。BERT 则通过双向训练策略，能够更好地理解搜索语句的上下文含义。结合推荐系统时，这些模型能够根据用户行为和偏好，提供个性化推荐，同时面临着解释性、计算成本和多样性平衡等技术挑战。未来发展将聚焦于提高模型的透明度和可解释性，同时确保推荐系统的公平性和用户隐私保护。

![](img/985d80c907203e50afbd766b0a54efb0.png)

### AI时代进击的CPU们

贡献人：

`https://zhuanlan.zhihu.com/p/702957407?utm_psn=1784491896913133569`

AI 时代下，CPU 面临着性能提升的巨大挑战，主要集中在增加核心数量、提高每个核心的性能、优化指令集以及提升内存访问速度上。多核处理器成为提升并行处理能力的关键，通过提高时钟频率和微架构优化，每个核心的计算效率得到显著提升。为了适应 AI 计算，指令集优化被视为提高处理速度的重要手段，支持更多 AI 指令，降低计算复杂度。此外，内存系统的优化也不容忽视，高速缓存和内存接口的改进对提升 CPU 性能至关重要。未来的 CPU 将更加注重能效比，通过异构计算架构，与 GPU、TPU 等处理器协同工作，以满足 AI 时代的计算需求。

![](img/eeffb119b0ee37ca82aefdf82e0f41bb.png)

### Jina CLIP v1：多模态向量模型，为文本和图像而生！

贡献人：

`https://mp.weixin.qq.com/s/9cIAP9PJRHE4-N6AKhn_SQ`

Jina CLIP v1 是一个基于 OpenAI CLIP 的多模态向量模型，专注于处理文本和图像数据。该模型通过大规模中文数据集进行训练，提升了对中文内容的理解能力。技术上，Jina CLIP v1 实现了文本和图像到共享向量空间的映射，使得跨模态的相似性搜索成为可能。此外，模型支持微调，可以针对特定应用进行优化。用户可以通过 Jina 框架轻松地调用 API 进行文本和图像编码，并进行相似性搜索。该模型适用于多模态内容理解和检索，为开发者提供了一个强大的工具，以实现更智能的搜索和内容分析应用。

![](img/d0b158c7e64907159a1234d85bcd4f52.png)

### 再谈苹果AI

贡献人：

`https://mp.weixin.qq.com/s/pZeoVk-ezf7jHtftWWm3cA`

文章详细介绍了苹果发布的Apple Intelligence技术，其核心由端侧大模型、云端大模型和ChatGPT三部分组成。苹果端侧大模型拥有约30亿参数，每秒生成30个token，初始响应延迟约为0.6毫秒；苹果云端大模型的处理能力相当于GPT-3.5。新Siri在Apple Intelligence加持下，实现了跨应用信息整合、连续对话和屏幕内容理解的能力。这些技术进步依赖于苹果系统、芯片和模型的高度集成，使其在AI体验上具有独特优势。文章还分析了苹果产品的创新周期及其对未来出货量的影响，并提出了相关的投资建议。

![](img/7572f538f9a0b2ef82a099656bf7d676.png)

### 巴黎开源AI开发者见面会

贡献人：

`https://lu.ma/docker-paris`

Docker 将在 2023 年 6 月 20 日星期四，时间为 18:00 - 21:00 GMT+2，在巴黎的 STATION F 举办一个本地和开源的 AI 开发者聚会。聚会需要主办方批准注册，并且参与者需要使用钱包验证代币所有权。活动包括开门迎宾、Koyeb、LlamaIndex、Giskard、Red Hat 和 Docker 等公司的闪电演示，以及后续的网络交流环节。参与者可以在活动中拾取由合作伙伴 Ollama 提供的 Ollama 钥匙扣。

![](img/f82dd2abe4fb96ee8d17ab10900b67b7.png)

> 潜空间系列活动第二季启动！

> 第2季第1期将在6.15 10:00开始，嘉宾是姚顺雨。活动包括嘉宾分享、访谈Q&A、cofounder matching餐会。

> 姚顺雨-普林斯顿计算机系博士，ReAct、思维树ToT、SWE框架提出者；

> Jesse Lyu吕骋-最热🔥🔥🔥AI硬件Rabbit- R1创始人；

> 尤洋-新加坡国立大学校长青年教授，曾创造Imagenet、Bert等多项训练速度世界纪录；

> 蔡天乐-普林斯顿“小天才”、美杜莎推理优化论文作者。

本季活动同时开放北京线下、上海线下和线上场次，欢迎校友和外部创业者扫描右方二维码报名！

![](img/aeb13d6bd0756bcb4754b4e2ed8686a3.png)

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报知识库】**这里是近期日报内容的汇总,欢迎大家查阅

> `https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh`

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/05a37475a0388faf13fd34157aea7bed.png)
