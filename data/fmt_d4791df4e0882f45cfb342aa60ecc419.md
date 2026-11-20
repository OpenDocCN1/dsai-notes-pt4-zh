
# Qwen系列（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Co1Nd4X27oZxbXxYg9ic1Mqsnqb](https://kq4b3vgg5b.feishu.cn/docx/Co1Nd4X27oZxbXxYg9ic1Mqsnqb)

## 模型介绍

Qwen（中文名：通义千问）是由阿里巴巴云 Qwen 团队开发的大型语言模型（LLM）和多模态模型系列，旨在为自然语言处理、代码生成、多模态理解等领域提供高效、开源的解决方案。自初代 Qwen 发布以来，该系列经历了多次迭代，逐步扩展到包括 Qwen2、Qwen2.5，以及专门针对特定任务的 Qwen2.5-Coder、QwQ、Qwen2.5-VL（2VL）和 QvQ 等模型。这些模型在预训练数据规模、任务性能和应用场景上均有显著提升，支持开发者在多种场景下构建智能应用。

### Qwen（初代）
包括 1.8B、7B、14B 和 72B 参数的基础模型和聊天模型（如 Qwen-72B-Chat），专注于自然语言理解与生成，覆盖中文、英文等多种语言，适用于跨语言任务，通过在超过 3 万亿 token 的多语言数据集上进行预训练，奠定了坚实的语言能力基础。

### Qwen1.5
在 2024 年初推出，参数范围扩展至 0.5B 到 110B，所有模型支持 32K 上下文，提供更强的多语言支持。

### Qwen2
2024 年 6 月发布，参数规模包括 0.5B、1.5B、7B 和 72B 的模型变体，上下文长度提升至 128K，预训练数据增至 7 万亿Tokens，显著改进编码和数学能力，并引入了更高效的训练策略和数据清洗方法，确保模型在知识广度和推理能力上的提升。

### Qwen2.5
2024 年 9 月推出，参数范围覆盖 0.5B、1.5B、3B、7B、14B、32B 和 72B，提供丰富的选择。在架构设计和后训练技术上进行了优化，预训练数据高达 18 万亿Tokens，支持 128K 上下文（部分模型达 1M），优化指令遵循和结构化数据处理。

### Qwen2.5-Coder（代码专用）
2024年11月12日推出，覆盖0.5B、1.5B、3B、7B、14B 和 32B参数的模型，支持92种编程语言，适用于代码生成、修复、推理、代码审查等应用场景，支持 128K token 的上下文，适合处理大型代码库。

### QwQ-32B（复杂推理）
2024年11月29日推出的实验性语言模型，专注于复杂推理任务，灵感来源于 OpenAI 的 o1 模型，通过鼓励模型在生成过程中“反思”和“质疑”，并通过深度探索和大量试验优化了数学和编程推理能力。

### Qwen2-VL（多模态）
2024年12月发布的图像、视频识别的多模态大模型，参数包括 3B、7B 和 72B，尤其在物体识别与光学文字识别（OCR）方面表现卓越。Qwen2VL 支持识别长度超过 20 分钟的视频内容，展现出卓越的性能与广泛的应用落地潜力。

### QvQ-72B-Preview（视觉推理）
2024 年 12 月发布的实验性视觉模型，专注于增强视觉推理能力。它基于 Qwen2.5-VL 开发，针对数学、科学和视觉任务进行了优化。

#### 关键引用
+ [Qwen GitHub 仓库](https://github.com/QwenLM)
+ Hugging Face Qwen 页面
+ [Qwen 官方博客](https://qwenlm.github.io/)

## 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### 实际评测
+ QwQ模型性能深度评测！文本&推理等八项能力深度评测！（视频）
+ Qwen2.5-Coder系列模型参数详解及本地测试报告（视频）
+ Qwen-QWQ最强开源推理大模型8小时极速测评（视频）
+ [Qwen2模型深度介绍（视频）](https://www.bilibili.com/video/BV1rS411P75Q)
⭐️Qwen1.5系列模型介绍及使用方法（课件）
+ Qwen1.5模型介绍及使用方法（视频）

### 部署调用
#### Qwen2.5-QWQ
⭐️QWQ-32B本地部署与调用指南（课件）
+ [QwQ+ollama、vLLM、llama.cpp部署方案详解，个人&企业部署方案介绍（视频）](https://www.bilibili.com/video/BV1hZRTYUEtR)

#### Qwen-QVQ
⭐️QvQ视觉推理大模型快速入门（课件）
+ Qwen-QVQ：多模态推理大模型安装部署与调用指南（视频）

#### Qwen2VL
⭐️多模态大模型Qwen2VL安装部署与调用教程（课件）
+ Qwen2VL：多模态大模型安装部署与调用指南（视频）

#### Qwen-QWQ
⭐️[Qwen-QWQ:32B]本地部署完整流程（课件）
+ Qwen-QWQ完整部署流程丨Ollama快速调用（视频）

#### Qwen2.5-Coder
⭐️Qwen2.5-Coder系列模型安装部署（课件）
+ Qwen2.5-Coder：32B本地部署全流程教程（视频）

#### Qwen2.5
⭐️Qwen2.5介绍与部署流程（课件）
+ 3种方法实现Qwen2.5系列模型本地化部署全流程教程（视频）

#### Qwen1.5
⭐️LangChain使用Ollama接入本地化部署的开源大模型（课件）
+ Ollama项目介绍&本地安装（视频）
+ Qwen1.5+Ollama本地私有化部署（视频）

#### Qwen-Chat-7B
⭐️Ubuntu22.04系统Qwen-Chat-7B部署文档（课件）
+ Qwen开源大模型的权重下载方法（视频）

### 进阶之路
#### QwQ高效微调实战
⭐️QwQ-32B高效微调实战（课件）
+ [仅需20G显存，QwQ-32B高效微调实战丨知识灌注+问答风格微调（视频）](https://www.bilibili.com/video/BV1wRQgYBEQb)

#### Qwen2.5-Coder适配Cursor实现低代码开发
⭐️[Qwen2.5-Coder系列模型适配Cursor实现低代码开发（课件）](https://kq4b3vgg5b.feishu.cn/wiki/ORB8wd6t8iQiWQkfd7McZxChnif?from=from_copylink)
+ [Qwen2.5-Coder完美适配Cursor部署使用，实现低代码开发（视频）](https://www.bilibili.com/video/BV1K4mDY5EuS)

#### Cursor调用QWQ实现辅助编程
⭐️Cursor调用推理大模型 QWQ 实现辅助编程（课件）
+ Cursor本地调用推理大模型QWQ实现代码辅助开发（视频）
+ Cursor调用推理大模型QWQ线上API实现代码辅助开发（视频）

#### 批量推理与加速
⭐️Qwen模型的批量推理与推理加速（课件）
+ Qwen大模型推理加速框架（视频）

#### Jupyter应用开发
⭐️Transformers库的使用及配置大模型开发环境（课件）
+ Qwen大模型+Jupyter应用开发（视频）

#### 接入vLLM框架
⭐️Qwen大模型接入vLLM框架（课件）
+ Qwen大模型接入vLLM框架（视频）

#### API调用开发实战
⭐️QwenAPI调用开发实践（课件）
+ Qwen+ReAct构建AI Agent（视频）
+ Qwen API工具调用（视频）
+ Qwen模型查询实时数据功能（视频）

## ❤️ 赋范社区交流群

海量硬核独家技术干货内容+无门槛技术交流~

![](img/31ef30775c1b313bfc374d698955d24f.png)

上图扫码👆即刻入群！

📍 社群技术交流氛围浓厚，不定期开设硬核干货&前沿技术公开课噢~
