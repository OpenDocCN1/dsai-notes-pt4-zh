
# 奇绩大模型日报（7月 6-7日）

> 来源：[https://miracleplus.feishu.cn/docx/V5VwdtPwHo4ysrxD77QcCaDVnjg](https://miracleplus.feishu.cn/docx/V5VwdtPwHo4ysrxD77QcCaDVnjg)

## 推特

### WH分享文档：使用仅 `Rust` 标准库，构建带有自动微分支持的 `Tensor` 库时的反思和内容

https://x.com/nrehiew_/status/1809174488048972005

上周，我开始用 `Rust` 构建 @karpathy 的 `micrograd`。到本周末，我使用仅 `Rust` 标准库构建了一个带有自动微分支持的 `Tensor` 库。在这个过程中，我学到了很多关于 `PyTorch` 的知识，所以我在这里写了一些相关内容。我尝试剥离 `PyTorch` 的许多抽象概念，比如 `Tensor` 的实现方式、如何思考广播机制以及如何建立反向传播的直觉。希望大家无论对 `PyTorch` 是否熟悉，都能觉得有趣！这个项目真的很有趣（也很有挑战性）。希望你们能看看，像往常一样，任何反馈都非常感谢！代码可以在这里找到：https://nrehiew.github.io/blog/pytorch/

![](img/9514daea5e54e04715b0c9b72b31d872.png)

### Reibman展示用 `crewAIInc` 代理工具的效果

https://x.com/alxfazio/status/1809170736537387367

这个演示非常精彩：

@AlexReibman 来自 @AgentOpsAI，展示了他们使用 `crewAIInc` 代理工具的效果。在几分钟内，他演示了 `AgentOps` 如何帮助使你的代理更具确定性并准备好用于实际应用。

Evals 和代理的无缝集成令人耳目一新——对任何对 Agentic 工作流程感兴趣的人来说，绝对值得一看。

![](img/026fc9b6faa73c810a9589d715076b73.png)

[ssstwitter.com_1720343779641.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/G9gfbbE5eoJ8XYxTZAkcG9Jinbe)

### 小科技议程：Andreessen和Horowitz呼吁支持初创企业

https://x.com/pmarca/status/1809340015505469692

小科技议程

Marc Andreessen & Ben Horowitz

2024年7月5日

“小科技”是我们对科技初创企业的称呼，以区别于大型科技公司。

在我们的整个职业生涯中，小科技一直独立于政治之外。但是，正如老苏联笑话所说，“你可能对政治不感兴趣，但政治对你感兴趣。”

我们认为不良的政府政策现在是小科技的头号威胁。

我们认为美国的技术霸主地位，以及小科技初创企业在确保这一霸主地位中所扮演的关键角色，是一个与其他任何问题同等重要的头等政治议题。

现在是时候站出来支持小科技了。

我们公司的政治努力完全专注于捍卫小科技。我们不会参与与小科技无关的政治斗争。但是，我们会用我们所有的资源，为小科技而战——为研究、发明、创造就业、建设未来的自由而战。

我们发现有三类政治家：

+ 支持小科技的。我们支持他们。

+ 反对小科技的。我们反对他们。

+ 处于中间的——他们想支持，但有一些顾虑。我们会真诚地与他们合作。

我们支持或反对政治家，不论其党派，不论其在其他问题上的立场。

我们准备长期作战。

[1] https://a16z.com/politics-and-the-future/

![](img/3b53c070a0d6813624364c3586077749.png)

![](img/04f40f4be29d1d5aec81e9066c4c6742.png)

### Rakshit Awasthi分享：编译器和 `LLVM` 内部原理的第二部分

https://x.com/sh4dy_0011/status/1809354492040867844

这是我的博客系列中关于编译器和 `LLVM` 内部原理的第二部分，在这篇文章中，我解释了以下概念：

1. 基本块

1. 控制流图

1. 模块

1. `LLVM` 传递的一些应用

文章链接：https://sh4dy.com/2024/07/06/learning_llvm_02

源代码链接：https://github.com/0xSh4dy/learning_llvm/tree/master/part_2

![](img/ebb276380a1303aba86414a29d1d05e1.png)

### Moura分享：使用 `RLHF` 的代理人找到候选人、评分并制作我可以使用的模板

https://x.com/joaomdmoura/status/1809614136222818719

🤖🔍🧑‍💻 AI代理帮我找到候选人！

使用 `RLHF` 的代理人找到候选人、评分并制作我可以使用的模板！

展示新的 `crewAI` 训练功能

比我平时的内容要长，希望你喜欢🤞

![](img/4950666ff9f2b8b83d4a97eeae64f6f1.png)

[ssstwitter.com_1720344073833.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FsEJbRAkxoiey0xPhJScJfKEnIf)

### Andriy Burkov分享博客：AI工程师应了解的搜索知识

https://x.com/burkov/status/1809744908346949678

AI工程师应了解的搜索知识

我可能不需要讨论双/交叉编码器等话题。这些方面已经有很多优秀的内容，特别是像 Pinecone 这样针对AI/LLM/RAG群体的公司。但也许你需要快速了解一些高级的、历史性的、词汇搜索背景……好吧，我在这里帮你！你可能对这一切都很陌生。刚刚被聘为“AI工程师”，突然需要索引大量的搜索内容:）。欢迎来到这个勇敢的新世界，你可以做出很多贡献！

![](img/4a8c6a8fd3ffe31e6e3b9c41420cb827.png)

## 产品

### Elevenlabs Voice isolator

贡献人：

https://elevenlabs.io/voice-isolator

ElevenLabs 推出了一款名为 Voice Isolator 的免费语音隔离和背景噪音去除工具。它可以从任何嘈杂的音频中提取出清晰的对话，让播客、采访或电影听起来就像是在录音室录制的一样。这个工具适用于各种音频创作场景，已经获得了积极的用户反馈。ElevenLabs 团队还计划在未来推出 `API` 访问功能，让用户可以更方便地集成使用。

![](img/7003e01db456469f3852aa9427748d84.png)

### Pillser Ask

贡献人：

https://pillser.com/ask

Pillser Ask 是一款 AI 助手，帮助人们做出明智的补充剂决策。它由 Pillser 公司开发，Pillser 汇总了数千篇研究论文和补充剂数据，弥合科学研究与消费者之间的差距，提供公正、科学支持的见解。它可以回答用户关于补充剂的各种问题，并根据用户的个人信息提供个性化的建议和指导，帮助他们做出明智的健康决策。

![](img/c35226832a0590a802506193a513cbcf.png)

### FreePhotoAI

贡献人：

https://freephotoai.co/

FreePhotoAI 是一个利用人工智能技术来帮助用户创作专业级照片和设计的服务。它提供了全面的 AI 工具箱，可以轻松地进行产品摄影、艺术创作以及快速的照片编辑。该服务旨在为用户带来一个高效、便捷且富有创意的照片处理体验。无论是专业设计师还是创意新手，FreePhotoAI 都能帮助他们发挥想象力，制作出出色的视觉作品。

![](img/808770fb63075f161ffee1575b95f53e.png)

## HuggingFace&Github

### X-AnyLabeling

贡献人：

https://github.com/CVHub520/X-AnyLabeling

`X-AnyLabeling` 是一款基于 AI 推理引擎和丰富功能特性于一体的强大辅助标注工具，其专注于实际应用，致力于为图像数据工程师提供工业级的一站式解决方案，可自动快速进行各种复杂任务的标定，近期更新：`yolov8-pose` 模型，支持 `yolo-pose` 标签导入/导出功能。

![](img/0409e9a50d19d00a50688a0cc311b26d.png)

### Ax

贡献人：

https://github.com/ax-llm/ax

`Ax` 是一个基于 `TypeScript` 的库，帮助开发者构建基于大型语言模型（`LLM`）的智能代理和“代理工作流”。它受到了斯坦福 `DSP` 论文的启发，提供了多种 `LLM` 和向量数据库的支持，并包含了许多高级功能，如流式验证、多模态 `DSP`、自动提示调优等。

![](img/17a7d2484abbb103ab8f70a5dbd9ec22.png)

## 投融资

### Quantum Rise获1500万美元种子轮融资

贡献人：

https://techcrunch.com/2024/07/05/quantum-rise-grabs-15m-seed-for-its-ai-driven-consulting-2-0-startup/

芝加哥初创公司Quantum Rise获得了由Erie Street Growth Partners领投的1500万美元种子轮融资。Quantum Rise专注于为企业提供AI驱动的自动化解决方案，通过“Consulting 2.0”模式，自动化工作流程并提供定制化AI解决方案。该公司由Cognitive Match前创始人Alex Kelleher创办，旨在填补中小企业在AI自动化领域的空白。Erie Street Growth Partners的董事长兼CEO Terry Graunke表示，Quantum Rise团队独特地理解如何应用数据、自动化和AI来增强人类智能并推动增长。

公司官网：https://www.quantumrise.com/

![](img/d09af289ecf423a10d2e10e654f3c98c.png)

## 学习

### 聊聊大模型推理内存管理之 `CachedAttention`/`MLA`

贡献人：

https://zhuanlan.zhihu.com/p/707190620

本文探讨了大模型推理中的内存管理优化，特别是针对多轮对话场景的 `CachedAttention` 和 `MLA` 方法。`CachedAttention` 通过分层 `KVCache` 系统（`AttentionStore`），实现了显存内 `KVCache` 的有效管理，减少了 87% 的 `TFTT`，并提升了多轮对话的提示预填充吞吐量，降低了 70% 的端到端推理成本。该方法采用了逐层预加载、异步保存技术，以及对 `RoPE` 位置编码的解耦和截断，以优化内存使用和提高推理效率。`MLA` 方法则通过降低 `KVCache` 维度，实现了超过 90% 的 `KVCache` 减少，通过低秩分解技术和对 `RoPE` 的特殊处理，减少了内存占用，但在推理过程中增加了计算开销。两种方法都在内存管理和推理效率方面取得了显著进展，为大模型的实际应用提供了有效的解决方案。

![](img/5327fcb3e36a7076b17cd1357deeb0c6.png)

### 图解序列并行云台28将（上篇）

贡献人：

https://zhuanlan.zhihu.com/p/707204903

序列并行技术在大模型训练中扮演关键角色，使得处理长序列数据成为可能。`Megatron-SP` 通过结合张量并行和序列并行，减少了计算量和显存消耗。`Colossal-SP` 和 `MQSP` 采用了分布式 Attention 机制，实现了序列的并行处理，但可能会带来通信效率问题。`Blockwise Parallel Transformer` 通过序列分块的方法，优化了长序列的处理过程。`Ring Attention` with `Blockwise Transformers` 和 `STRIPED ATTENTION` 则通过计算和通信重叠，以及负载均衡策略，提高了处理效率。`BurstAttention` 结合了 `Ring Attention` 和 `FlashAttention`，提出了 `LAO` 方法，进一步优化了计算效率。`LSS` 通过 `AllGather` 操作获取全量的 key 和 value 数据，但可能存在通信消耗较大的问题。`LIGHTSEQ` 通过负载均衡算法改进了 `Ring Attention` 的不均衡问题，并提出了重计算优化方法。目前，序列并行的主流方案可以分为 `Ring FlashAttention`、`LSS` 和 `Ulysess`，每种方法都有其独特的优势和适用场景。随着技术的不断发展，序列并行技术将继续进步，为大模型训练提供更高效的解决方案。

![](img/b7133f3f290a9b4aa8c091c8fbeb402d.png)

### 站在AI Scale-Up域的一个岔路口

贡献人：

https://zhuanlan.zhihu.com/p/707355769

我的朋友在研发AI训练芯片，特别是高性能`Scale-Up`网络（如`Nvidia`的`NVL72`），但他遇到技术困扰，反馈常围绕进度、KPI等非技术性方面。他的问题是如何设计>16P的`Scale-Up` AI训练芯片。`NVL72`通过`Clos`结构和高密度`Cable`实现无收敛的TB级别带宽。正常情况下，`Scale-Up`网络会有分层结构，例如`Chiplet`接口、`PCB`或`Cable`互联等。面对距离感，不同层次有不同最优解。计划A和计划B是两种分层结构的例子，分别强调`Cable`和`Chiplet`的互联。朋友的问题是，基于通用集合通信策略，计划A和计划B哪个更好？

![](img/bb55a9c85b2ef71ce0ab44f2e83f7b4d.png)

### LLM分离式推理可能带来的软硬件变革的迷思

贡献人：

https://zhuanlan.zhihu.com/p/707199343?utm_psn=1792627597320003584

方佳瑞在其文章中探讨了 `LLM` 分离式推理架构可能引发的软硬件变革。在软件层面，他提出了 `KVCache Memory Pool` 组件的概念，该组件能够管理多层次内存，实现快速查找和数据迁移策略，以及智能的 `Cache Eviction` 策略。在硬件方面，他建议将 `GPU` 集群重构为 `Prefill` 和 `Decode` 两个子集群，以适应不同的计算需求，并采用 `RDMA` 网络实现高效的跨实例数据传输。此外，他还强调了 `Memory Pool` 与计算资源的解耦，以及如何通过弹性扩缩容机制优化资源利用。

![](img/98ca549275348460cde0a3d0286b186b.png)

### 图解大模型计算加速系列：vLLM源码解析3，Prefix Caching

贡献人：

https://zhuanlan.zhihu.com/p/707228704?utm_psn=1792627972915744768

`vLLM` 中的 `Prefix Caching` 机制通过在推理过程中缓存具有相同前缀（`Prefix`）的逻辑块，有效地减少了冗余计算，提高了大模型的推理速度。该机制在 `prefill` 和 `decode` 阶段分别采用不同的 `hash` 计算方法来确保缓存的有效性。在 `prefill` 阶段，通过计算逻辑块的 `hash` 值来判断是否可以共享物理块，从而实现节省显存和减少重复计算。而在 `decode` 阶段，`Prefix Caching` 机制则根据逻辑块是否满载动态调整缓存策略，采用 `copy-on-write` 机制以优化内存使用。此外，文章还详细介绍了 `CachedBlockAllocator` 的实现，包括如何管理物理块的分配、释放和复用，以及驱逐器（`Evictor`）的作用，它通过 `LRU`（Least Recently Used）策略来管理不活跃的物理块，确保在设备空间有限时仍能保持高效的缓存利用。通过这些技术细节的深入分析，文章展示了 `Prefix Caching` 在提升大模型推理效率方面的潜力和实际应用价值。

![](img/adad2ede1e4fdfec6c528f831af4d613.png)

### 无穹TechView | 漫谈大模型推理优化技术系列——静态推理

贡献人：

https://zhuanlan.zhihu.com/p/702888478?utm_psn=1792634001007190016

大模型推理优化技术中的静态推理，通过在模型部署前对计算图进行深度改造，实现算法层面的优化，包括算子融合、常量传播和精细化内存管理。这种预先优化策略能够在不影响预测准确度的前提下，降低推理计算的延迟和成本。文章特别强调了注意力和线性算子在推理过程中的关键作用，并介绍了如 `FlashAttention`、`FlashDecoding` 和 `TensorRT-LLM` 等技术的应用，它们通过优化这些核心算子，显著提升了大模型推理的效率。此外，计算图优化通过算子融合减少了数据搬运的开销，进一步提高了性能。无问芯穹推理引擎的实现案例展示了这些优化技术在实际大模型推理中的应用效果，能够在不同的输入输出长度和批处理大小下保持较高的吞吐率。

![](img/1428e2faf050e1be5d60ef36e9d42f36.png)

### 平安壹钱包：RAG等技术在金融支付类ToC应用场景的探索与落地

贡献人：

https://mp.weixin.qq.com/s/goV5UbMo0PjREp0kGH02YQ
