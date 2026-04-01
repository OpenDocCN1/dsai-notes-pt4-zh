# 奇绩大模型日报（6月 14日）

> 来源：[https://miracleplus.feishu.cn/docx/TAdKdXNo4o9AdNxiIPBcLGz3n5g](https://miracleplus.feishu.cn/docx/TAdKdXNo4o9AdNxiIPBcLGz3n5g)

# 推特

### 震惊！经典男人牵着女人回头梗图后续居然是这样的！LeCun：真是令人惊叹

https://x.com/karpathy/status/1801305852735115357?s=46&t=GRStLXDcUNuun8J5Noyw4Q

哇。来自@LumaLabsAI的新模型将图像扩展为视频，真是令人惊叹。我直觉上理解到这很快就会成为可能，但看到它并思考未来的迭代，仍然让人感到不同凡响。

这里还有一些例子，比如那个在着火房子前的女孩

[h](https://x.com/CharaspowerAI/status/1801196982104457393)ttps://x.com/CharaspowerAI/status/1801196982104457393

![](img/42df6313ca2f0f52b7f29ed8300bd9eb.png)

[ssstwitter.com_1718291858528.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/LPDnbD6s0ooqMRxaEjCcRnSHnwd)

### “构建你自己的数据库代理”：构建一个将自然语言问题转化为SQL查询的AI助手

https://x.com/andrewyng/status/1801277928740970619?s=46&t=GRStLXDcUNuun8J5Noyw4Q

新短课程：与Microsoft @Azure的@adriangs86合作创建的“构建你自己的数据库代理”。您将学习构建一个将自然语言问题转化为SQL查询的AI助手。使用自然语言查询使您组织中的每个人，从业务领导到开发人员，都可以直接使用简单的英语访问数据洞察。

您将使用Azure OpenAI服务和LangChain来实现检索增强生成和函数调用，并利用Assistants API。您将获得构建一个可以处理CSV文件和SQL数据库的代理的实际操作经验。

请在此注册！[h](https://deeplearning.ai/short-courses/building-your-own-database-agent/)ttps://deeplearning.ai/short-courses/building-your-own-database-agent/

![](img/01f00506ace7094945c50c72374fd8cb.png)

[ssstwitter.com_1718372923762.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JHj7bCfuhopxUBxSZ7Gc52PSnpd)

### Mollick：如何仅使用一个提示词来构建教学模拟

https://x.com/emollick/status/1801292063289295049?s=46&t=GRStLXDcUNuun8J5Noyw4Q

我们为哈佛撰写了一份指南，介绍如何仅使用一个提示词来构建教学模拟。教师可以将这些模拟作为练习或作业进行实验。

我们已经构建教学模拟十年了，令人惊叹的是，一个提示词能带你走多远！

![](img/dbe1fce3fd545eae46f783966f6b771e.png)

### 《提示报告》：到底什么样的提示最好？纯文本、提示工程、答案工程、代理、安全、对齐

https://x.com/sophiamyang/status/1801340351447203917?s=46&t=GRStLXDcUNuun8J5Noyw4Q

一篇总结提示技术的优秀论文——《提示报告》。

+   58种纯文本提示技术，包括零样本、多样本、思维生成、集成、自我批评和分解技术。在他们的基准测试中，多样本连锁思维（Few-shot CoT）表现最佳。

+   提示工程：元提示、自动提示、自动提示工程师、无梯度指令提示搜索、文本梯度优化提示、RLPrompt、基于对话的策略梯度离散提示优化。

+   答案工程：答案形状、答案空间、答案提取器。

+   代理：工具使用代理、基于代码的代理、基于观察的代理、RAG（检索增强生成）。

+   安全：提示攻击（提示注入、越狱）、风险（数据隐私、代码生成问题、客户服务）、强化措施（基于提示的防御、护栏、检测器）。

+   对齐：模糊性、偏见、校准、提示敏感性。

🔗https://arxiv.org/abs/2406.06608

![](img/3ab6ac4aa95f3976b331f08a23b2a0f8.png)

### Samba 3.8B：简单的Mamba+滑动窗口注意力架构

https://x.com/liliang_ren/status/1801027052147216457?s=46&t=GRStLXDcUNuun8J5Noyw4Q

介绍Samba 3.8B，这是一种简单的Mamba+滑动窗口注意力架构，在主要基准测试（如MMLU、GSM8K和HumanEval）上大幅超越Phi3-mini。😮 并且它具有无限的上下文长度和线性复杂度。🤯

论文链接：https://arxiv.org/abs/2406.07522

![](img/5b949a5c4a275059be0055145b329995.png)

### 违反直觉的发现：即使在确定性任务中，温度为1的GPT-4也比温度为0时“更聪明”

https://x.com/eugeneyan/status/1801081017144647829?s=46&t=GRStLXDcUNuun8J5Noyw4Q

Kyle Corbitt：大家部署LLMs时都应该知道的一个疯狂事实——即使在确定性任务中，温度为1的GPT-4也比温度为0时“更聪明”。

老实说，在我尝试之前我自己都不相信这一点，但在我们的评估中这很明显。感谢@eugeneyan的提示！

---

Eugene Yan ：分享一个违反直觉的发现

> 开放思维的实践者用评估进行尝试

> 用GPT-4确认了这个发现，但用未微调的Llama3-8b却没有确认（下一条推文）

> 更多的实践者进行确认/否认

> 社区从讨论中受益

温度参数控制模型输出的“创造性”。它的范围从0.0到1.0，较高的值会导致更具多样性和不可预测的响应，而较低的值会产生更集中的确定性输出。（令人困惑的是，OpenAI的API允许高达2.0的温度值，但这不是常态。）

我的经验法则是从0.8的温度开始，并在必要时降低它。我们想要的是仍然能带来良好结果的最高温度。

另一种启发法是，对于分析或多选任务，使用较低的温度（接近0）；而对于创造性或开放性任务，使用较高的温度（接近1）。不过，我发现温度太低会降低模型的“智能性”（因此我更喜欢从0.8开始并仅在必要时降低）。

![](img/de080c69fdfd561afdbf9e7be8c797de.png)

![](img/437c8c346b78e2f222fe9f0c91d70329.png)

# 资讯

### 亚马逊承诺以AWS信用额度投资2.3亿美元，用于AI初创企业

https://cn.investing.com/news/stock-market-news/article-2397215

亚马逊承诺以亚马逊云服务(AWS)的信用额度投资2.3亿美元，为处于早期阶段的人工智能初创企业提供指导和培训，以促进对人工智能和机器学习技术的使用。新承诺的一部分资金将用于资助第二批AWS生成式人工智能加速器，这一计划将为前80家使用生成式人工智能解决复杂挑战的早期初创企业提供专业知识，及每家高达100万美元的额度。

![](img/91b8e4c34f593ca933f20bc57f22f342.png)

### 刚刚，HuggingFace 宣布千万美元收购了一家数据集服务公司！CEO 透露今年 AI 初创公司寻求被收购数量增多

https://mp.weixin.qq.com/s/LmhVxBQKSdtXDFkJCkTsew

Hugging Face 周四宣布，已同意以 1000 万美元收购名为 Argilla 的公司，这是 Huggingface 迄今为止第四笔收购。CEO Clément Delangue 透露，每周都会听到大约 10 家 AI 初创公司表示有意被收购，尤其是今年，这个数字增长很多，也可能是市场进一步整合的迹象。

![](img/44433de08b6bdddc29fafeaab3b0a2e6.png)

### 领英(LinkedIn)宣布将推出系列AI求职顾问新功能

https://www.chinaz.com/ainews/9592.shtml

作为求职界的龙头公司之一，领英(Linkedin)周四宣布将推出一系列由人工智能驱动的新服务，从申请工作到提供相关的准备材料，以及搜索整个领英以帮助用户更快地找到心仪职位。该功能仅面向领英的高级订阅者，个人求职者月费约30美元，企业订阅者月费约60美元。

![](img/e374538f32282cf651872e0e656028cf.png)

### 阶跃星辰推出移动端AI智能问答助手跃问APP

https://apps.apple.com/cn/app/%E8%B7%83%E9%97%AE-ai%E6%99%BA%E8%83%BD%E9%97%AE%E7%AD%94%E5%8A%A9%E6%89%8B/id6502382318

由微软前全球副总裁姜大听所创办的人工智能初创公司阶跃星辰日前宣布上线其AI智能问答助手跃问的移动端APP，用户可在苹果App Store和各大安卓应用商店进行下载使用。

![](img/4000a5d115b4381083d3e58338f5208d.png)

# 论文

### 开放VLA：一个开源的视觉-语言-动作模型

贡献人：

链接：http://arxiv.org/abs/2406.09246v1

摘要：通过在互联网规模的视觉语言数据和多样化机器人演示中预训练的大型策略，有潜力改变我们如何教导机器人新技能：我们可以微调这些视觉语言动作（VLA）模型，而不是从零开始训练新的行为，从而获得强大、可泛化的视觉运动控制策略。我们引入了OpenVLA，一个开源VLA模型，参数量为7B，训练集包含多样化的97万真实世界机器人演示。OpenVLA基于Llama 2语言模型，结合了来自DINOv2和SigLIP的预训练特征。具有音效的数据多样性和新模型组件，OpenVLA表现出强大的通用操作能力，在29个任务和多个机器人实体中，绝对任务成功率比RT-2-X（55B）高出16.5％，且参数减少7倍。我们展示了OpenVLA可以有效进行微调以适应新环境，并在多对象和强语言基础能力涉及的多任务环境中表现出强大的泛化结果，胜过了20.4％的从头开始模仿学习方法。我们还探讨了计算效率；作为额外的贡献，我们展示了OpenVLA可以通过现代低秩适应方法在消费级GPU上进行微调，并通过量化有效地进行服务而不影响下游成功率。最后，我们发布了模型检查点、微调笔记本以及我们的PyTorch代码库，支持在Open X-Embodiment数据集上规模化训练VLA。

![](img/ef2b3c40795586090f742007cb29fef8.png)

### LRM-Zero: 使用合成数据训练大型重构模型

贡献人：

链接：http://arxiv.org/abs/2406.09371v1

我们提出了LRM-Zero，一个完全在合成的3D数据上训练的大型重建模型（LRM），实现了高质量的稀疏视图3D重建。LRM-Zero的核心是我们的程序化3D数据集Zeroverse，它自动从简单的基本形状中合成，具有随机的纹理和增强（例如，高度场，布尔差异和线框）。与以前的3D数据集（例如Objaverse）不同，那些通常是由人类捕获或制作的，以近似真实的3D数据，Zeroverse完全忽略了现实的全局语义，但在复杂的几何和纹理细节上富有，这些细节在局部上类似甚至比真实对象更复杂。我们展示了我们的LRM-Zero，使用完全合成的Zeroverse进行训练，可以在重建真实世界对象时获得高视觉质量，与在Objaverse上训练的模型相媲美。我们还分析了Zeroverse的几个关键设计选择，这些选择有助于LRM-Zero的能力和训练稳定性。我们的工作表明，作为3D视觉中的核心任务之一的3D重建，可能可以在不考虑真实世界对象的语义的情况下解决。Zeroverse的程序合成代码和交互式可视化可在以下网址找到：https://desaixie.github.io/lrm-zero/。

![](img/9376738d5aeb9bb5994156eef86a9c8c.png)

### **探索大规模全模态预训练的极限**

贡献人：

链接：http://arxiv.org/abs/2406.09412v1

我们提出构建全模态智能，能够理解任何形式并学习通用表示。我们提出一种名为Multimodal Context（MiCo）的可扩展预训练范式，可以在预训练过程中扩大模态数量、数据量和模型参数。通过MiCo，预训练模型在多模态学习中展现出显著涌现能力，评估了10种不同模态的单模态感知基准任务，25项跨模态理解任务以及18项多模态大型语言模型基准任务。我们的模型在性能上建立了37项新的最先进记录。我们希望我们的研究能为全模态智能的发展做出贡献。源代码和模型位于https://github.com/invictus717/MiCo。

![](img/64f0144f5cf4ae1fd6f084245698b2f3.png)

### CS-Bench: 朝着计算机科学领域精通的大语言模型综合基准测试

贡献人：

链接：http://arxiv.org/abs/2406.08587v1

计算机科学（CS）作为人类智能复杂性的见证，深刻推动了人工智能和现代社会的发展。然而，当前大语言模型（LLMs）社区过于专注于分析特定基础技能的基准（如数学和代码生成），忽视了对计算机科学领域的全面评估。为了弥补这一差距，我们介绍了CS-Bench，这是第一个专门用于评估LLMs在计算机科学中表现的双语（中英文）基准。CS-Bench包括约5K个经过精心策划的测试样本，涵盖了计算机科学的4个关键领域的26个子领域，包括各种任务形式和知识和推理的分支。利用CS-Bench，我们对30多个主流LLMs进行了全面评估，揭示了CS性能与模型规模之间的关系。我们还定量分析了现有LLMs失败的原因，并强调了改进方向，包括知识补充和计算机科学特定的推理。进一步的跨能力实验显示，LLMs在计算机科学领域的能力与它们在数学和编码方面的能力之间存在高度相关性。此外，专门从事数学和编码的专家LLMs在几个CS子领域中也表现出色。展望未来，我们期待CS-Bench成为LLMs在CS领域应用的基石，并开拓新的评估LLMs多样推理能力的途径。CS-Bench数据和评估代码可在https://github.com/csbench/csbench获取。

![](img/b84593187182f55d49fc145a251a039e.png)

### Transformer 遇上 神经算法推理者

贡献人：

链接：http://arxiv.org/abs/2406.09308v1

摘要：Transformer以其简单而有效的架构彻底改变了机器学习。在大规模文本数据集上对Transformer进行预训练为自然语言理解（NLU）任务带来了无与伦比的泛化能力。然而，在需要精确和稳健计算的算法推理任务中，这些语言模型仍然很脆弱。为了解决这一局限性，我们提出了一种新颖的方法，将Transformer的语言理解能力与基于图神经网络（GNN）的神经算法推理器（NARs）的稳健性结合起来。NARs在图形式指定的情况下被证明有效作为算法任务的通用求解器。为了使它们的嵌入可被Transformer访问，我们提出了一种混合架构，采用两阶段训练程序，允许语言模型中的token与NAR的节点嵌入相互参照。我们在CLRS-Text上评估了我们的结果TransNAR模型，这是CLRS-30基准测试的文本版本，并展示了在算法推理方面显著优于仅有Transformer模型的结果，无论是在样本内还是在样本外。

![](img/a6db58d7fd6a9a93ac24d701019c266e.png)

# 产品

### Mapify

贡献人：

https://mapify.so/editor/new

Mapify 是一款基于 AI 的智能思维导图工具，能够快速将各种内容（包括 PDF 文档、网页、YouTube 视频等）转化为清晰的思维导图，大大提高信息整理和知识管理的效率。无论是学生、职业人士还是好奇的学习者，Mapify 都能简化他们的工作流程，提升创造力和生产力。该工具拥有多种便捷的功能，如从单一提示生成思维导图、使用内置 AI 助手完善导图、以多种格式导出等。

![](img/98435e68b66444ec3f764eb18f3f6149.png)

### Twoshpt

贡献人：

https://twoshot.app/

TwoShot 是一个新推出的 AI 音乐创作平台，让用户可以通过语音、描述甚至哼唱来创作和混音音乐，无需专业技能。TwoShot 的目标是提升音乐创作者的工作流程，而不是取代他们的创造力。此外，用户还可以在平台上创建、分享和变现自己的 AI 音乐模型。

![](img/8613ceaf8f670f5c59889969a18b31e1.png)

# HuggingFace&Github

### Warp

贡献人：

https://github.com/warpdotdev/Warp

Warp 是一款基于 Rust 语言开发的 GPU 加速终端应用程序，内置 AI 功能，希望可以提高用户和团队的生产力。Warp 正处于"公开测试"阶段，虽然还有一些需要改进的地方，但已大幅改善了终端使用体验。Warp 的 Rust UI 框架部分已开源，未来还会开放更多客户端代码，但服务器端将保持闭源。

![](img/0a8d1ff850465018cac0eb8ae3715b47.png)

### GroundingDINO

贡献人：

https://github.com/IDEA-Research/GroundingDINO

Grounding DINO 是一个由 IDEA 研究团队开发的强大开放世界目标检测模型，它具有出色的零样本学习性能（COCO 52.5 AP）和微调性能（63.0 AP），支持与 Stable Diffusion、GLIGEN 等模型协作实现灵活的图像编辑，提供丰富的教程和演示，代码和预训练模型开源，是一个功能强大、应用广泛的计算机视觉模型。

![](img/97189d6350f70cf688271ffa97aa2189.png)

# 投融资

### GPTZero完成1000万美元A轮融资，推动AI检测技术发展

贡献人：

https://techcrunch.com/2024/06/13/gptzero-profitable-ai-detection-startup-10m-series-a/

AI检测初创公司GPTZero由24岁的Edward Tian和26岁的Alex Cui创立，在短短一年半内实现盈利，并生成了数百万美元的收入。GPTZero近期完成了由Footwork合伙人Nikhil Basu Trivedi领投的1000万美元“抢占性”A轮融资。该公司用户量从100万增长到400万，年经常性收入（ARR）在过去六个月增长了500%。在此轮融资后，GPTZero估值约为5000万美元。

公司官网：https://gptzero.me/

![](img/ca8f65ed3d2b2145fe7f0ca93dda255b.png)

### **三星牵头对人工智能芯片公司Tenstor**rent的一轮投资 投前估值为20亿美元

贡献人：

https://www.theinformation.com/articles/samsung-values-chip-customer-tenstorrent-at-2-billion

中文链接：https://www.cls.cn/detail/1703750

三星牵头对Tenstorrent进行了一轮至少3亿美元的投资。Tenstorrent是一家总部位于多伦多的人工智能芯片公司，其首席执行官吉姆·凯勒(Jim Keller)曾在苹果和特斯拉工作过。据参与这轮融资的两名人士透露，Tenstorrent的投前估值为20亿美元。其中一位知情人士说，韩国的LG电子(LG Electronics)也计划作为新投资者参与其中，其他投资者还有富达管理(Fidelity Management)和现代汽车集团(Hyundai Motor Group)。

公司官网：https://tenstorrent.com/

![](img/4064278ccf43dc59ae93b14f62719d9a.png)

### Optopia区块链平台获得关键种子资金

贡献人：

https://cointelegraph.com/press-releases/powering-ai-innovation-optopias-blockchain-platform-secures-vital-seed-funding

著名风险投资公司和知名投资者支持Optopia在AI和区块链集成领域的愿景

Optopia，一个专为AI应用设计的Layer2区块链，成功获得种子资金。这一轮投资显示了市场对Optopia在全球范围内重新定义AI应用标准的信心。

主要投资者包括G·Ventures、Kucoin Ventures、JRR Capital、KKP International Limited、ZenTrading、Klein Labs和MCS Capital，他们不仅带来了资金，还提供了战略指导。此外，著名科技投资者MrBlock也参与了这轮融资，贡献了他的专业知识和对前沿技术的热情。

Optopia将利用这笔资金进一步开发其基于Op Stack的AI Layer2区块链解决方案，提供100%以太坊虚拟机（EVM）兼容性、卓越性能、降低成本和增强安全性。Optopia致力于通过AI代理简化Web3操作，降低用户进入门槛，并通过代币经济学赋能AI代理，推动Web3创新，扩大去中心化数字经济的参与度。

公司官网：https://www.optopia.ai/

![](img/14d0f4728795c15b8af9b49347686318.png)

# 学习

### LLM参数高效微调综述和详解-从Adpter、PrefixTuning到LoRA

贡献人：

https://zhuanlan.zhihu.com/p/696057719

文章揭示了大型语言模型（LLM）参数高效微调的关键技术，如 Adapter、PrefixTuning 和 LoRA。这些方法通过引入额外的少量参数或对现有参数进行低秩近似，实现在特定任务上的性能优化，同时大幅度减少了计算资源的消耗。其中，LoRA 通过低秩矩阵分解，有效地减少了可训练参数，使得微调过程更加高效和经济。这些技术的应用不仅提高了模型在各种任务上的适应性，还为大型模型的推广和应用开辟了新的可能性。

![](img/3e2c5ab3e4b15f36051f3da34e41ffe0.png)

### 几款多模态向量检索引擎：Faiss 、milvus、Proxima、vearch、Jina等

贡献人：

https://zhuanlan.zhihu.com/p/364923722?utm_psn=1784926732165357568

本文综述了多模态向量检索引擎的几款代表性工具，包括 Faiss、milvus、Proxima、vearch 和 Jina。Faiss以其高效的近邻搜索算法和丰富的索引结构，适合大规模数据处理。milvus则以开源性和高性能著称，支持多种数据类型和距离度量。Proxima作为美团点评开发的产品，能够与关系型数据库无缝集成，适合中等规模数据集。vearch轻量级且高性能，特别适合多媒体数据的检索。Jina则提供了一个云原生的解决方案，支持多模态数据并易于扩展。这些工具在处理复杂的信息检索任务时，各有特点，需要根据具体的技术细节如数据规模、实时性需求、系统复杂度等因素来选择最合适的引擎。

![](img/b91e32cd558c179d08191fc8324d9576.png)

### 引入 Lamini 内存调优：95% 的大型语言模型准确率，幻觉减少 10 倍。

贡献人：

https://www.lamini.ai/blog/lamini-memory-tuning

Lamini Memory Tuning 技术通过对数百万个 LoRA 适配器进行细致的内存调优，实现了大语言模型（LLMs）在事实准确性上的大幅提升，减少了幻觉现象。该技术不同于传统的指令微调，它能够将模型对特定事实的回忆准确率提高至接近完美，同时保持了模型在其他任务上的泛化能力。Lamini Memory Tuning 采用了一种名为 Mixture of Memory Experts（MoME）的模型架构，通过在推理时智能检索和路由最相关的专家适配器，确保了高准确性和低延迟的响应，同时避免了高昂的计算成本。这一技术已经在高精度的文本到 SQL、数据分类和产品推荐等领域的实际应用中得到了验证，显著提升了模型的实用性和市场价值。

![](img/186e5a26be86847bff7f172d6c56fe58.png)

### 小米公司 Con 2024-Agent 技术在语音助手场景中的应用

贡献人：

https://zhuanlan.zhihu.com/p/703326562?utm_psn=1784926956711636992

小米公司的 Con 2024-Agent 技术在语音助手领域的应用，通过感知、思考和行动三个环节，提升了对话系统的智能化。在感知层，技术不仅支持文本输入，还融合了多模态输入，如图像感知。思考阶段，通过知识注入和记忆召回，实现了任务规划和信息整合。行动阶段，除了文本回复外，还能调用工具完成复杂任务，如购买火车票或搜索宾馆。小米的 Agent 技术框架包括 NLU Parser、Prompt Manager 和 Executor & API Platform，通过紧密结合大模型和系统环境，以及复用现有 NLP 能力，实现了更端到端的用户体验。技术细节如 Prompt 模板的设计、历史会话的管理、输出格式的标准化，以及记忆管理的优化，都有助于提升语音助手的性能和用户满意度。未来，小米计划进一步与系统融合，加强跨设备协同，以及探索多模态方向，以实现更智能和全面的用户交互体验。

![](img/3c3cd4c7543ca051df99fe681155d462.png)

### 2024年6月具身智能机器人领域情报第二弹

贡献人：

https://zhuanlan.zhihu.com/p/703314003?utm_psn=1784927248832331776

在技术细节方面，美国投资人当前非常关注强化学习技术，特别是英伟达在最近的ComputerX演讲中提出的“Reinforcement Learning from Physical Feedback”(RLPF)概念。这种方法基于实际的物理反馈，与以往的“强化学习从人类反馈”(RLHF)方法类似，但更强调在具身智能中的应用，即通过物理世界的交互来训练和优化算法，这对于发展更高级的机器人技术至关重要。

英伟达的最新数据仿真平台也进行了重大更新，此平台允许所有企业在一个共享的环境中训练他们的机器人来完成各种任务。这种开源策略将有助于标准化具身智能的技术水平，使各公司能够在同一基础上进行创新。

交流还指出了计算机视觉在机械臂控制等具体应用中的局限性。目前，单靠大模型进行物体感知和决策制定往往无法解决复杂的操作任务，这突显了开发更为精确和特定目标的算法的必要性。

在中国，技术的应用展示如卢策吾的刮胡子演示，凸显了精准机械臂控制算法在实际操作中的重要性，这在长远来看可能会推动精细操作机器人技术的市场需求。

整体来看，投资和技术发展的趋势都显示出对实用技术和垂直领域的重视，而对大模型的炒作态度趋于谨慎，投资者和开发者更倾向于追求能够实际落地和产生市场影响的技术解决方案。

### 【招商传媒】AI 文生视频：开启内容创作新纪元

贡献人：

https://mp.weixin.qq.com/s/tcWpC3Z9ph4tzV7u1BK6Dg

AI 文生视频技术作为生成式 AI 的一大创新，通过将文本描述转换为视频内容，实现了内容创作的自动化。技术上，文生视频模型结合了扩散模型和 Transformer 架构，能够生成高质量、高分辨率的视频内容，如 OpenAI 的 Sora 模型，其参数量为 30 亿，能够一次性生成 60 秒视频，实现复杂多机位视频的生成，提升了场景画面和角色刻画的精细真实。此外，技术细节包括视频生成与编辑功能、AI 编辑工具、创意转视频、图像生成与处理、3D 内容创作、智能处理功能等，这些技术的应用不仅提高了视频内容的生产效率，还降低了创作成本，推动了影视和游戏行业的革新。例如，Pika 和 Runway 等产品支持用户根据指令生成各种风格的视频，实现角色和场景的快速变换，同时也支持图像的生成和处理，以及 3D 模型的创建和渲染。文生视频技术的进步，预示着内容创作方式的革命，为行业内的创新和商业化开辟了新的可能性。然而，技术发展的不确定性、商业化的挑战以及政策监管的风险，也是该领域面临的重要问题。

![](img/9ecaba08e86c225128c1dfb77b25e9ef.png)

### 「“人形机器人+”科研领域探索」乐聚冷晓琨

贡献人：

https://mp.weixin.qq.com/s/hA-SpyA65rhtCHs3vfPliQ

乐聚机器人创始人冷晓琨在 2024 年中国国际机器人高峰论坛上发表报告，强调人形机器人作为新兴科研领域的潜力。报告指出，特斯拉发布完人形机器人后，人形机器人研究得到了加速推进。中国政府将其视为新能源和机器人领域推动经济增长的关键点。人形机器人的产业链长且复杂，其核心技术包括高动态运控系统、模块化关节和智能操作系统。模块化关节占产品成本的 30% 以上，是性能的关键。操作系统需要具备高度智能自适应能力，以适应不同的服务机器人场景。

报告还提出了 “人形机器人 +” 生态算法，旨在通过与不同场景合作，推动人形机器人的产业化应用。在应用场景方面，乐聚机器人与华为、海尔等合作，探索人形机器人在家庭、工业和特种场景的产业化应用。此外，报告还强调了人形机器人领域的人才缺口问题，呼吁与高校合作，提前培养人才。总体而言，人形机器人被视为未来重要的技术发展方向，其成功商业化将对社会经济产生深远影响。

![](img/c3b6ff93725c8f229b2535cfee6bcdfc.png)

### [**盘点Softmax及其替代品**](https://kexue.fm/archives/10145)

贡献人：

https://kexue.fm/archives/10145

Softmax 函数是一种将神经网络输出转化为概率分布的激活函数，广泛应用于多分类问题。然而，它在处理大规模或类别不平衡数据时可能会遇到计算稳定性和梯度消失问题。为了克服这些限制，研究者提出了多种替代方案：SPIGAN 通过对抗生成网络增强稀有类别的表现；Gumbel-Softmax 利用 Gumbel 噪声进行连续 relax，使得可以在大规模多分类任务中有效地进行梯度传递；SphereSoftmax 则将数据在球面上进行分布，提高了模型对长尾分布的拟合能力。选择合适的概率分布函数对于提升模型性能至关重要，需要根据具体问题和数据特性来决定。

> 潜空间系列活动第二季启动！

> 第2季第1期将在6.15 10:00开始，嘉宾是姚顺雨。活动包括嘉宾分享、访谈Q&A、cofounder matching餐会。

>

> 姚顺雨-普林斯顿计算机系博士，ReAct、思维树ToT、SWE框架提出者；

>

> Jesse Lyu吕骋-最热🔥🔥🔥AI硬件Rabbit- R1创始人；

>

> 尤洋-新加坡国立大学校长青年教授，曾创造Imagenet、Bert等多项训练速度世界纪录；

>

> 蔡天乐-普林斯顿“小天才”、美杜莎推理优化论文作者。

>

>

> 本季活动同时开放北京线下、上海线下和线上场次，欢迎校友和外部创业者扫描右方二维码报名！

![](img/0538468c2ee4feea11e1072bf886bae0.png)

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

![](img/3d29f21c00ecbcde60b6c7832d23cc0d.png)