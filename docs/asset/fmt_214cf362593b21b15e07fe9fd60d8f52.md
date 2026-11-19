
# 12月6日 AI情报助手早报

> 来源：[https://v11enp9ok1h.feishu.cn/docx/OiZgdOJkRoq5yQxBktwcOJYtn4d](https://v11enp9ok1h.feishu.cn/docx/OiZgdOJkRoq5yQxBktwcOJYtn4d)

> 🔔AI情报助手早报「12月06日」

## 🧠 1️⃣ 推动大模型自我进化，北理工推出「流星雨计划」

北京理工大学计算机科学与技术学院启动「流星雨计划」，旨在深入研究大模型自我进化理论与方法。

研究提出SRA-MCTS方法，通过自我进化提升代码大模型处理复杂问题的能力，无需外部监督。

SRA-MCTS方法通过自我生成推理路径并转化为代码，增强模型推理能力并提升解决复杂任务的成功率。

流星雨计划提出从无领域能力到领域专家模型的自我进化训练方案，包含导师监督学习、自我评估能力习得和自我提升训练三个阶段。

性能分析显示，应用Meteor进化方法后，LLM在准确性、完整性等方面取得性能提升。

### 关键词
大模型，自我进化，SRA-MCTS，代码生成，领域专家模型

### 参考链接
- [SRA-MCTS论文地址](https://arxiv.org/pdf/2411.11053)
- [SRA-MCTS代码开源](https://github.com/DIRECT-BIT/SRA-MCTS)
- [8B模型数据开源](https://huggingface.co/datasets/BinXD/SRA-MCTS-Llama-3.1-8B)
- [Meteor计划论文](https://arxiv.org/abs/2411.11933)
- Meteor计划Github链接

## 🌏 2️⃣ 开源开发者指南：欧盟《人工智能法案》解读

欧盟《人工智能法案》正式生效，影响开源社区的AI开发和使用。

开源AI系统和模型需遵守文档提供、模型信息披露等义务。

Hugging Face提供工具支持合规，包括模型卡、数据集卡和个人数据删除等。

法规根据AI系统风险等级设定规则，保护开放研究和支持中小企业。

开发者需标记合成内容，披露深度伪造和AI生成内容，遵守版权和隐私规则。

### 关键词
人工智能法案，开源社区，合规工具，风险等级，数据保护

### 参考链接
- [模型卡](https://hf.co/docs/hub/en/model-cards)
- [数据集卡](https://hf.co/docs/hub/en/datasets-cards)
- [Gradio](https://www.gradio.app/docs/gradio/video)
- [水印工具](https://hf.co/spaces/meg/watermark_demo)
- [支持退出机制的工具](https://techcrunch.com/2023/05/03/spawning-lays-out-its-plans-for-letting-creators-opt-out-of-generative-ai-training/)
- [个人数据删除](https://hf.co/blog/presidio-pii-detection)
- [许可](https://hf.co/docs/hub/en/repositories-licenses)

## 🤖 3️⃣ 全球首例机器人刮胡子，斯坦福校友1年拿下3轮数亿融资！红杉领投圈内热门

穹彻智能，一家专注于具身智能的初创公司，完成了数亿元人民币的Pre-A+轮融资，由红杉中国领投。

穹彻智能展示了其在衣物折叠和黄瓜削皮等任务上的突破性技术，体现了其具身大脑的通用性和鲁棒性。

公司技术路线以力为中心，通过快速迭代通用技能，实现高效低成本的解决方案。

穹彻智能的Noematrix Brain系统覆盖了智能体应用的完整生命周期，聚焦非结构化、开放场景下的机器人智能边界突破。

穹彻智能的技术实力和商业化潜力获得了国际顶级投资公司的认可，其AnySkill技能库将加速技术到市场的转化。

### 关键词
具身智能，穹彻智能，红杉中国，Noematrix Brain，AnySkill

### 参考链接
- [穹彻智能融资新闻](https://mp.weixin.qq.com/s/guPcWoRkgU2z4YRML9nafA)

## 🤖 4️⃣ 破案了！ChatGPT拒绝说出ta的名字，原因竟是……

ChatGPT拒绝提及特定名字，如David Mayer，因涉及法律诉讼和GDPR被遗忘权请求。

用户尝试多种方法让ChatGPT说出名字但均失败，包括密码、谜语、技巧等。

除了David Mayer，还有五人名字被ChatGPT屏蔽，与法律问题或诽谤有关。

猜测指向罗斯柴尔德阴谋论，但实际上是AI系统设计问题，屏蔽特定名字导致历史同名人物也被屏蔽。

OpenAI最终确认因隐私保护屏蔽名字，并已修复问题。

### 关键词
ChatGPT，法律诉讼，GDPR，隐私保护，AI系统设计

### 参考链接
- [新智元报道](https://mp.weixin.qq.com/s/uH-KNt3JDTZl2Bo7crUreg)
- David Mayer GDPR请求
- [YouTube讨论](https://www.youtube.com/watch?v=77W3xWGofNs)

## 🚀 5️⃣ 英伟达提出全新Star Attention，10倍加速LLM推理！登顶Hugging Face论文榜

英伟达推出Star Attention技术，可显著减少大模型推理计算量，加速边缘计算。

Star Attention通过两阶段推理过程，优化长上下文任务处理，提升效率和准确性。

在多个基准测试中，Star Attention实现高达11倍的加速，同时保持准确率仅小幅下降。

该技术能够无缝集成到基于Transformer的LLMs中，减少内存需求，适用于本地设备。

未来研究将探索更长序列和更大模型的应用，以增强Star Attention的可扩展性和稳健性。

### 关键词
Star Attention，LLM推理，Transformer，边缘计算，大模型

### 参考链接
- [论文地址](https://arxiv.org/abs/2411.17116)

## 🧬 6️⃣ 1-bit大模型的新突破！新一代BitNet架构启用4位激活值

BitNet a4.8新一代架构为1-bit大模型启用4位激活值，支持3-bit KV cache，效率显著提升。

采用混合量化和稀疏化策略，减轻量化误差，实现与前代BitNet b1.58相当的性能。

仅激活55%参数，支持3-bit KV cache，提升大规模LLM部署和推理效率。

实验表明，BitNet a4.8在语言模型困惑度和任务的平均准确性方面与LLaMA相当，且平均精度几乎没有损失。

低比特注意力对于高效的长序列建模至关重要，减少了KV cache的内存占用和IO，并加速了注意力计算。

### 关键词
BitNet a4.8，量化，稀疏化，LLM，效率提升

### 参考链接
- [论文地址](https://arxiv.org/pdf/2411.04965)
- [参考资料](https://venturebeat.com/ai/how-microsofts-next-gen-bitnet-architecture-is-turbocharging-llm-efficiency/)
