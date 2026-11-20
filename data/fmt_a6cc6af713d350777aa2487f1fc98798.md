
# 谷歌《Gemini 2.5 模型技术白皮书》

> 来源：[https://gotoaiworld.feishu.cn/docx/FDoVdesBWo2qJqx8eeMcGZPanZd](https://gotoaiworld.feishu.cn/docx/FDoVdesBWo2qJqx8eeMcGZPanZd)

《Gemini 2.5 模型技术白皮书》介绍了Google DeepMind的Gemini 2.5模型家族，新一代多模态AI模型家族，包括Gemini 2.5 Pro和Gemini 2.5 Flash等版本。Gemini 2.5 Pro在推理、编码和多模态理解方面表现卓越，支持长达100万token的上下文处理，能分析3小时的视频内容。该系列模型具备先进的“思考”能力，支持动态分配计算资源以高回答准确性。Gemini 2.5 Flash以低延迟和低成本提供高性能推理。模型在安全性、多语言支持和事实性方面也有显著提升，广泛应用于代码生成、教育工具和创意设计等领域，展现强大的代理能力和实际应用潜力。

![](img/612d982affa109c7a20a69766c8a6113.png)

## 简介（Introduction）

介绍 Gemini 2.5 系列模型家族（包括 Gemini 2.5 Pro 和 Gemini 2.5 Flash），强调多模态、长上下文、推理能力和工具使用特性。

### Gemini 2.5 Pro

当前最强大的模型，支持 100 万 token 上下文，可处理 3 小时视频内容。

### Gemini 2.5 Flash

高效推理模型，在较低的计算和延迟要求下提供出色的推理能力。

### Gemini 2.5 Flash-Lite

谷歌推出的高效轻量级AI模型，支持100万token长上下文和工具调用，以超低延迟和低成本提供高性能推理，适用于大规模应用场景。

![](img/a4785156aff52f78d957451162428e23.png)

## 模型架构、训练与数据集（Model Architecture, Training and Dataset）

### 模型架构（Model Architecture）

基于稀疏混合专家（MoE）的 Transformer 架构，支持多模态输入（文本、图像、音频、视频）。基于动态路由输入标记到子集参数（专家）解耦总模型容量与计算和每标记服务成本。Gemini 2.5在大规模训练稳定性、信号传播和优化动态方面取得显著进展，在预训练后直接提升了性能。

### 数据集（Dataset）

预训练数据集是大规模、多样化的数据集合，涵盖多个领域和模态，包括公开可用的网络文档、代码、图像、音频和视频。

### 训练基础设施（Training Infrastructure）

基于TPUv5p架构训练。用同步数据并行训练，在多个8960芯片的Google TPUv5p加速器上进行分布式训练。主要的软件训练基础设施改进包括弹性切片和分裂相位SDC检测，提高了训练的弹性和效率。

### 后训练（Post-training）

监督微调（SFT）、奖励建模（RM）和强化学习（RL）的改进，提升模型性能。

### 思考能力（Thinking）

模型基于额外推理时间（“Thinking”）提高答案准确性，支持动态计算资源分配。

### 特定能力改进（Capability-specific Improvements）

#### 代码（Code）

代码生成和理解能力显著提升（如 LiveCodeBench 分数从 30.5% 提升至 69.0%）。

#### 事实性（Factuality）

整合谷歌搜索工具，提升多模态事实准确性。

#### 长上下文（Long Context）

优化百万级 token 上下文检索和推理。

#### 多语言（Multilinguality）

支持 400+ 语言，优化中文、印度语等表现。

#### 音频与视频（Audio/Video）

新增音频生成和视频理解能力（如 3 小时视频分析）。

#### 代理能力（Agentic Use Cases）

Gemini Deep Research 代理在复杂任务中表现优异。

#### Gemini 2.5 的开发路径（The Path to Gemini 2.5）

实验性模型版本（如 Gemini 2.0 Pro、2.0 Flash 图像生成模型）的迭代过程。

![](img/fcaee1cde3175a61975d088a315badad.png)

## 定量评估（Quantitative Evaluation）

### 方法论（Methodology）

比较Gemini 2.5模型与Gemini 1.5模型的性能，和Gemini 2.5 Pro与其他大型语言模型的性能。

### 核心能力结果（Core Capability Results）

Gemini 2.5 Pro 在代码（Aider Polyglot 82.2%）、数学（AIME 2025 88.0%）和长上下文任务（LOFT 87.0%）中领先。对比其他大模型（如 GPT-4o、Claude 4），Gemini 2.5 Pro 在多模态和事实性任务中表现最优。

### 音频与视频评估（Audio/Video Evaluation）

在 FLEURS（语音识别）和 VideoMME（视频理解）等基准中达到 SOTA。

![](img/d15016c0a0c8d80678a8aa6531db3a9a.png)

## 应用案例（Example Use Cases）

### Gemini Plays Pokémon

独立开发者使用 Gemini 2.5 Pro 通关《宝可梦蓝》，展示长时任务规划和复杂推理能力。

### 其他能力展示（What Else Can Gemini 2.5 Do?）

将剧本转为互动工具、图像生成 SVG、创建教育应用等。

### 谷歌产品集成（Gemini in Google Products）

应用于 Google Search（AI Overviews）、NotebookLM（播客生成）等产品。

![](img/e982bd636ca639aebccda520f413342d.png)

## 安全与责任（Safety, Security, and Responsibility）

### 流程（Our Process）

安全评估框架，包括自动化红队测试和外部专家评审。

### 政策与目标（Policies and Desiderata）

禁止有害内容生成（如暴力、医疗错误），追求响应帮助性和中立性。

### 安全训练（Training for Safety）

通过数据过滤、强化学习优化安全性。

### 评估结果（Results on Training Evaluations）

Gemini 2.5 比前代模型减少 8.2% 的政策违规，同时提升响应友好度（+18.4%）。

### 自动化红队测试（ART）

描述自动化红队测试（ART）的过程和结果，Gemini 2.5 Flash和Pro在保持强大安全性的同时，成为迄今为止最有帮助的模型。

### 记忆与隐私

分析Gemini 2.5模型的可发现记忆率和隐私风险，发现Gemini 2.5模型的记忆率显著低于之前的模型，并且没有发现包含个人隐私信息的输出。

### 安全性评估与前沿安全框架

描述对Gemini 2.5 Pro进行的全面安全评估，包括对CBRN（化学、生物、放射性、核信息风险）、网络安全、机器学习研发和欺骗性对齐等领域的评估。

### 外部安全测试

描述外部安全测试计划的结果，包括对Gemini 2.5 Pro（Preview 05-06）的评估，重点关注自主系统风险、网络安全风险、CBRN风险和社会风险。

![](img/6bbfcfa4c30149561abd57b68e4d5d7b.png)
