
# 01🈷️27日

> 来源：[https://v11enp9ok1h.feishu.cn/docx/RZq1dH7XuoArSaxeWCmcCNG4nic](https://v11enp9ok1h.feishu.cn/docx/RZq1dH7XuoArSaxeWCmcCNG4nic)

> 🔔AI情报助手午报「01月27日」

> ✨✨✨✨✨✨✨✨

## 1️⃣ [Qwen2.5-1M: 支持100万Tokens上下文的开源Qwen模型](https://mp.weixin.qq.com/s/BG6zd24ap6-RCAWx-92LUA)

🧠

Qwen团队发布了支持100万Tokens上下文的Qwen2.5-1M模型，包括Qwen2.5-7B-Instruct-1M和Qwen2.5-14B-Instruct-1M，扩展了开源Qwen模型的上下文长度。

推理框架基于vLLM并集成稀疏注意力方法，提升了处理1M标记输入的速度3倍到7倍。

技术报告分享了训练和推理框架设计思路及消融实验结果，展示了Qwen2.5-1M在长上下文任务中的性能优势。

长上下文训练采用逐步扩展长度的方法，从4K扩展到256K，并通过Dual Chunk Attention(DCA)技术将上下文扩展到1M Tokens。

推理框架通过优化算子效率和动态分块流水线并行，提高了1M Tokens序列的预填充速度3.2倍到6.7倍。

**关键词：** Qwen模型，长上下文处理，稀疏注意力，性能提升，开源框架

**🔗 文中引用的链接**

- [模型链接](https://www.modelscope.cn/collections/Qwen25-1M-d6cf9fd33f0a40)
- [技术报告](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen2.5-1M/Qwen2_5_1M_Technical_Report.pdf)
- [体验链接](https://modelscope.cn/studios/Qwen/Qwen2.5-1M-Demo)

## 2️⃣ 一文搞懂DeepSeek - 开源模型R1

🧬

DeepSeek-R1是一款开源的大型语言模型，表现出色于数学、编程和推理任务。

该模型采用纯强化学习训练方法，开源且低成本，技术创新显著。

DeepSeek系列模型包括R1、V3、VL等，各具特色，覆盖多个AI领域。

DeepSeek-R1与OpenAI o1相比，优势在于开源特性、强化学习训练和低成本。

技术创新包括GRPO算法优化策略网络，多层次奖励机制和“思考-回答”双阶段训练模板。

**关键词：** DeepSeek-R1，开源模型，强化学习，推理能力，技术创新

**🔗 文中引用的链接**

- [原文链接](https://mp.weixin.qq.com/s/ECzHVHeREHm3ZT1lcS_kEg)

## 3️⃣ [DeepSeek创始人专访：中国的AI不可能永远跟随，需要有人站到技术的前沿](https://mp.weixin.qq.com/s/rL-FURsOJ6qemNDHMvbbxQ)

🧬

DeepSeek的V3版本开源模型在训练成本和效果上超越了Llama 3.1 405B模型，达到开源SOTA水平。

DeepSeek专注于研究和技术，未全面商业化，选择开源路线，以普惠和人人可用为目标。

梁文锋强调原创式创新的重要性，认为中国AI需要从跟随者转变为贡献者。

DeepSeek致力于探索通用人工智能（AGI），从语言大模型出发，未来可能涉及视觉等多模态。

公司文化鼓励自由探索和创新，不设限于层级和部门，重视人才的热爱和基础能力。

**关键词：** AI，开源模型，AGI，技术创新，普惠

**🔗 文中引用的链接**

- [DeepSeek专访原文](https://mp.weixin.qq.com/s/rL-FURsOJ6qemNDHMvbbxQ)

## 4️⃣ LeCun：Deepseek爆火不能说明中国AI强，只能说开源模型很强

🌏

DeepSeek-R1 在AIME2024数学基准测试中超越OpenAI的o1，展现专家级水平。

DeepSeek-R1以MIT许可开源，查询成本低于OpenAI，引发全球AI社区关注。

美国对AI领域霸主地位的焦虑，Meta员工对低成本高性能的DeepSeek V3感到担忧。

LeCun指出，正确的解读是开源模型正在超越专有模型，DeepSeek得益于开源研究和项目。

DeepSeek的成功代表AI研究开源模式的崛起，推动AI技术更健康、更普惠的发展方向。

**关键词：** DeepSeek，开源模型，AI焦虑，LeCun，技术创新

**🔗 文中引用的链接**

- [MLNLP社区](https://mp.weixin.qq.com/s/bLiV7dOiMsSIUo6LRLbPKw)
- [LeCun推文](https://x.com/ylecun/status/1883002229977915535)

## 5️⃣ 大模型数学推理数据合成相关方法

🧠

本文介绍了几种提升大语言模型数学推理能力的数据合成方法，包括MetaMath、MuggleMath、WizardMath等，旨在解决高质量数据稀缺的问题。

MetaMath通过重写技术增加数学问题多样性，显著提升模型准确率。

MuggleMath评估不同数据增强策略的效果，并探讨增强数据量与模型性能的关系。

WizardMath通过进化指令合成方法增强大模型的数学能力，包括下行演化和上行演化。

Key-Point-Driven Data Synthesis (KPDDS)框架利用关键点驱动的方法合成问题-答案对，确保生成新问题的质量控制和可扩展性。

MAmmoTH2和JiuZhang3.0分别从网络语料库中收集指令数据和训练小型LLM合成高质量数学问题，以提升大模型的数学推理能力。

**关键词：** 大模型，数学推理，数据合成，MetaMath，MuggleMath

**🔗 文中引用的链接**

- [RUC AI Box](http://aibox.ruc.edu.cn)

## 6️⃣ Efficient AIGC

🧬

SiTo是一种新的加速扩散模型推理速度的方法，通过基于相似性的令牌剪枝技术，显著降低计算资源消耗而不牺牲生成质量。🌈

该方法的核心在于选择基础令牌和剪枝令牌，通过最大化相似性和均匀空间分布来减少恢复误差。🔥

SiTo在COCO30K和ImageNet数据集上表现出色，实现了1.9倍的加速和2.7倍的内存压缩，同时降低了FID值。🍭

该技术硬件友好，低计算开销，无需额外训练步骤，具有通用性和即插即用性。📌

SiTo还通过消融实验验证了其方法的有效性，并展示了在不同模块中的应用效果。🎯

**关键词：** AIGC，扩散模型，令牌剪枝，图像生成，硬件加速

**🔗 文中引用的链接**

- [SiTo代码](https://github.com/EvelynZhang-epiclab/SiTo/tree/main)
