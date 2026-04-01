# 02🈷️13日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/XBIMd4Dy5o0qPsx7zsucP9mjn1e](https://v11enp9ok1h.feishu.cn/docx/XBIMd4Dy5o0qPsx7zsucP9mjn1e)

> 🔔AI情报助手晚报「02月13日」

> ✨✨✨✨✨✨✨✨

> 1️⃣

> 🤖

> 爆肝整理：超100款DeepSeek应用清单【收藏】

>

+   DeepSeek模型已集成至超100款应用，覆盖金融、教育、医疗等多个领域。

+   DeepSeek官方整理了54款应用，包括应用程序、AI Agent框架等，提供了多样化的集成选项。

+   国内企业如百度、昆仑万维等纷纷宣布软件接入DeepSeek，强化了DeepSeek在AI领域的应用生态。

+   DeepSeek的应用展示了大模型的广泛应用和适配性，不同应用根据功能选择了不同的集成方式。

+   DeepSeek的集成带来了用户体验的升级，如智能助手、办公AI工具等，提升了服务的精准性和透明度。

> 关键词：DeepSeek，AI应用生态，大模型，集成应用，用户体验

>

> 🔗 文中引用的链接

+   DeepSeek GitHub主页

> 2️⃣

> 🧠

> [AI大牛卡帕西盛赞DeepSeek！对着](https://mp.weixin.qq.com/s/iSO6ZfGF-CVBaR3QvmIq2Q)论文夸了半小时，称其思维能力“难以置信”

>

+   安德烈·卡帕西（Andrej Karpathy）在YouTube上发布3.5小时的免费课程，详细讨论了DeepSeek-R1背后的强化学习技术。📚

+   卡帕西强调，强化学习是大模型训练中的关键环节，DeepSeek-R1的研究首次公开讨论了强化学习在大语言模型中的应用。🧩

+   DeepSeek-R1在强化学习过程中展现出了推理能力，被卡帕西称为“最令人难以置信的成效”。🤖

+   强化学习有望彻底改变AI的思考形态，解锁人类未设想过的思考方式，例如用新语言思考。🌏

+   卡帕西预言多模态AI、agents和测试时训练将成为AI研究的前沿方向。🔮

> 关键词：DeepSeek，强化学习，大模型，推理能力，AI趋势

>

> 🔗 文中引用的链接

+   [完整视频](https://www.youtube.com/watch?v=7xTGNNLPyMI&t)

> 3️⃣

> 🧬

> [ICLR 2025 | 比MoE快6倍，成本暴降8](https://mp.weixin.qq.com/s/3UBk1C8cbQHgctOaEBxv0w)3%！字节发布超稀疏架构UltraMem

>

+   字节跳动豆包大模型团队发布了UltraMem，一种新的稀疏模型架构，有效解决了MoE在推理时高额的访存问题，推理速度提升2-6倍，成本降低最多83%。🌏

+   UltraMem通过优化模型结构、改进value检索方式和隐式扩展稀疏参数，实现了高效的访存和优质的模型效果。📈

+   实验结果显示，在大规模训练下，UltraMem能实现业界领先的推理速度和模型性能，开辟了数十亿规模value或expert的新路径。🏄

+   UltraMem的设计考虑了访存和模型效果的平衡，尤其在延迟要求高的推理场景中显示出明显的速度优势。🚀

+   这项研究为开发更高效和可扩展的语言模型提供了新方向，团队将继续探索UltraMem的技术演进。🧚🏻‍♀️

> 关键词：UltraMem，MoE，PKM，稀疏模型，推理速度

>

> 🔗 文中引用的链接

+   [论文链接](https://arxiv.org/abs/2411.12364)

> 4️⃣

> 🧠

> [Cell子刊《Patterns》最新综述：大语言模型At](https://mp.weixin.qq.com/s/88LG1RSoPIdfo8Lc3tuWNg)tention Heads的可解释性研究

>

+   上海创新算法研究院联合清华大学、中国电信研究院发布综述论文，探讨大语言模型（LLMs）中注意力头的功能与工作机制。

+   提出四阶段认知框架，将人脑推理过程分为知识召回、上下文识别、潜在推理和表达准备，为大模型可解释性研究提供新视角。

+   综述首次将认知神经科学原理融入大模型研究，清晰定义不同注意力头的具体功能，增强模型内部机制的理解。

+   系统化总结探索注意力头机制的实验方法，分为Modeling-Free和Modeling-Required两大类，为后续研究提供设计思路。

+   综述总结了多种评估基准和数据集，分为Mechanism Exploration Evaluation和Common Evaluation两大类，为验证注意力头功能提供参考。

> 关键词：大语言模型，注意力头，可解释性，认知框架，评估基准

>

> 🔗 文中引用的链接

+   [文章地址](https://www.cell.com/patterns/fulltext/S2666-3899(25)00024-8)

+   [Github项目地址](https://github.com/IAAR-Shanghai/Awesome-Attention-Heads)

> 5️⃣

> 🧠

> [推理满血R1，仅需24GB！](https://mp.weixin.qq.com/s/bjxyJ7WoxAFdLocfBhl7lQ)

>

+   KTransformers开源框架支持24GB推理DeepSeek-R1/V3，实现高效显存利用。

+   在4090显卡下，prompt预处理速度可达285token/s，生成速度14 token/s，优化MoE架构存储需求。

+   采用CPU和GPU混合推理技术，专家选择策略，以及Intel AMX优化，提升性能并减少内存开销。

+   通过高效内存管理，减少NUMA节点间数据传输，加快预填充和解码过程。

+   项目地址提供了详细的使用教程和文档。

> 关键词：KTransformers，DeepSeek-R1/V3，混合推理，专家选择策略，Intel AMX优化

>

> 🔗 文中引用的链接

+   [项目地址](https://github.com/kvcache-ai/ktransformers/blob/main/doc/en/DeepseekR1_V3_tutorial.md)

> 6️⃣

> 🤖

> [今](https://mp.weixin.qq.com/s/sKSRDVE5ES6qF0CIWaa8cw)日开源（2025-02-12）：DeepScaleR-Preview分布式强化学习微调大模型，1.5B参数性能超越OpenAI

>

+   DeepScaleR-1.5B-Preview是一个通过分布式强化学习微调的语言模型，基于DeepSeek-R1-Distilled-Qwen-1.5B开发，性能超越OpenAI的O1-Preview。

+   Direct3D是一种原生3D生成模型，能够从自然输入图像生成3D模型，无需多视图扩散模型或SDS优化。

+   WorldSense是首个用于评估多模态视频理解的基准，涵盖视觉、音频和文本输入。

+   VideoMMMU是一个大规模、多模态、多学科的视频基准，旨在评估从教育视频中获取知识的能力。

+   Open-LLM-VTuber是一个支持实时语音对话和视觉感知的AI伴侣，具备生动的Live2D虚拟形象。

> 关键词：分布式强化学习，3D生成模型，多模态视频理解，知识获取评估，AI伴侣

>

> 🔗 文中引用的链接

+   DeepScaleR-Preview

+   [Direct3D](https://sota.jiqizhixin.com/project/direct3d)

+   [WorldSense](https://sota.jiqizhixin.com/project/worldsense)

+   [VideoMMMU](https://sota.jiqizhixin.com/project/videommmu)

+   [Open-LLM-VTuber](https://sota.jiqizhixin.com/project/open-llm-vtuber)