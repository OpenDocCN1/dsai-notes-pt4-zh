# 02🈷️20日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Jakkd4LrMoLDgWxAHQScw1Ehnle](https://v11enp9ok1h.feishu.cn/docx/Jakkd4LrMoLDgWxAHQScw1Ehnle)

> 🔔AI情报助手午报「02月20日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🧠🌐

> [从ReFT, Kimi K1.5到DeepSeek R1，聊聊](https://mp.weixin.qq.com/s/ADY6lp36DLwHrfSAAQjrsw)Reasoning Model的精巧实现

>

+   Reasoning Model领域近期火热，Kimi和DeepSeek推出的K1.5和R1效果卓越，引起行业关注。

+   推理模型的核心提升在于Post-Training阶段通过强化学习(RL)增强模型的推理能力。

+   国内LLM开源玩家在模型开源的同时，也公布了技术细节，满足了行业对技术细节的好奇心。

+   文章介绍了三篇重要的工作：字节的ReFT、Kimi的K1.5和DeepSeek的R1，它们都采用了RL来提升模型的推理能力。

+   国产推理模型的复现展现了精巧和简洁，与OpenAI的o1模型相比，实现了有效的推理能力提升。

> 关键词：Reasoning Model，强化学习，模型开源，推理能力，国产AI

>

> 🔗 文中引用的链接

+   [字节ReFT](https://arxiv.org/pdf/2401.08967)

+   [KIMI K1.5](https://arxiv.org/pdf/2501.12599)

+   [DeepSeek-R 1](https://arxiv.org/pdf/2501.12948)

> 2️⃣

> 🧠

> 张俊林：Grok 3是否意味着大力出奇迹的大模型法则仍然成立？

>

+   Grok 3的发布引发了关于大模型法则是否仍然有效的讨论，特别是在预训练阶段的Scaling Law是否成立的问题上。

+   尽管数据量增长趋缓，预训练阶段的Scaling Law依然有效，只是性价比降低，导致研究者转向RL Scaling Law和Test Time Scaling Law。

+   Grok 3作为通用基座模型，其评测指标主要集中在数学、科学和代码数据集，暗示其通用能力可能没有大幅提升。

+   Grok 3的算力消耗是Grok 2的10倍，可能意味着数据量和模型尺寸的大幅增加，尽管当前趋势是减小模型尺寸，增大数据量。

+   Grok 3的逻辑推理版本在评测指标上达到或超过了o3 mini，显示出其在深度思考方面的优势。

> 关键词：大模型法则，预训练，Scaling Law，Grok 3，深度思考

>

> 🔗 文中引用的链接

+   [张俊林文章](https://mp.weixin.qq.com/s/tmM9iD0-EVL-Umkv2AnIUw)

> 3️⃣

> 🤖

> [Z](https://mp.weixin.qq.com/s/oS76JoQK76KbmhzEI-QoSQ) Tech｜2月24日直播对话00后OpenAI奖学金博士Peter Tong：揭秘多模态大模型的革命突破

>

+   多模态大模型(MLLM)在视觉理解领域取得显著进展，尤其是大语言模型(LLM)在视觉生成能力的提升。

+   Peter Tong（童晟邦），纽约大学博士生，将分享其团队在Cambrian与MetaMorph项目中的颠覆性工作，旨在提升LLM的视觉学习能力。

+   Cambrian-1项目探索视觉为核心的多模态大语言模型，提出新的视觉基准测试CV-Bench和动态空间感知连接器SVA。

+   MetaMorph项目通过200k数据和指令微调，使语言模型能够直接输出连续视觉Token，使用VPiT预测多模态Token。

+   分享会将包括QA环节，提供扫码预约和进群的机会。

> 关键词：多模态大模型，LLM，视觉理解，Cambrian-1，MetaMorph

>

> 🔗 文中引用的链接

+   [Cambrian论文链接](https://github.com/cambrian-mllm/cambrian)

+   [MetaMorph论文地址](https://arxiv.org/pdf/2412.14164v1)

> 4️⃣

> 🧬

> [生成理解统一模型解读 (一)｜Transfusio](https://mp.weixin.qq.com/s/WEqV1ilmo3r9M3qhQAWuWg)n：只用一个模型搞定图像生成和理解任务！

>

+   Transfusion是一个新型多模态模型，结合了语言建模和扩散模型的训练目标，实现了在单一模型中生成离散文本和连续图像。

+   该模型在多模态任务上表现出色，生成质量和效率均优于现有的离散化图像处理方法，并在图像生成和文本生成任务中达到了与专门模型相当的性能。

+   实验结果显示，Transfusion模型在图像生成和文本生成任务中均优于Chameleon模型，并且通过引入特定于模态的编码和解码层，性能得到进一步提升。

+   Transfusion模型的参数量级可达7B，通过扩展到7B参数和2T多模态token训练后，能够与相似尺度的扩散模型相媲美生成图像，与相似尺度的语言模型媲美生成文本。

+   在GenEval基准测试中，Transfusion优于其他流行的模型，如DALL-E 2和SDXL，同时还能生成文本，达到与Llama 1相同的性能水平。

> 关键词：多模态模型，Transfusion，图像生成，文本生成，性能比较

>

> 🔗 文中引用的链接

+   [Transfusion论文地址](http://arxiv.org/pdf/2408.11039)

> 5️⃣

> 🧬

> [仅用1.44M参数超越SOTA 4.5个点！小红](https://mp.weixin.qq.com/s/CtHf-gCvQZd2MI5lCgLlpg)书&中科大提出轻量高效的AI图像检测模型

>

+   📕联合中国科学技术大学提出SAFE模型，仅用1.44M参数量在33个测试子集上达到96.7%的准确率，超越SOTA模型4.5个百分点。

+   研究团队从成像机制差异出发，发现AI图像检测的关键线索在于上采样和卷积操作留下的“伪影特征”。

+   SAFE通过三种图像变换（痕迹保留、不变性增强、局部感知）和离散小波变换提取高频特征，有效提升检测性能。

+   实验表明SAFE具有出色的泛化能力和即插即用特性，在DiTFake测试集上平均准确率达到99.4%。

+   SAFE为AI图像检测领域开辟新方向，启发后续研究从优化训练模式入手，减少训练偏差。

> 关键词：AI图像检测，参数效率，伪影特征，泛化性能，训练优化

>

> 🔗 文中引用的链接

+   [论文链接](https://arxiv.org/abs/2408.06741)

+   [代码链接](https://github.com/Ouxiang-Li/SAFE)

> 6️⃣

> 🤖

> 从Deepseek R1和NSA算法谈谈个人的一些反思

>

+   本文探讨了DeepSeek R1算法和NSA算法在AI领域的应用和影响，以及作者对大模型推理能力的思考。🧬

+   讨论了模型的计算复杂度、优化策略、以及对大模型训练和推理的内存开销问题。🧠

+   分析了Transformer模型结构优化，并提出了模型网络结构改进的方向。🌐

+   作者反思了国内AI研究的现状，强调了基础研究的重要性和对现有模型的深入理解。📚

+   提出了未来大模型基础研究的方向，包括FP4/Log8量化训练和模型结构深度研究。🔬

> 关键词：AI算法，大模型，Transformer，基础研究，优化策略

>

> 🔗 文中引用的链接

+   [极市平台](https://mp.weixin.qq.com/s/8LYcGcucv_J-hklKrDZCWQ)

+   [《大模型时代的数学基础(1)》](http://#)

+   《谈谈大模型架构的演进之路, The Art of memory.》