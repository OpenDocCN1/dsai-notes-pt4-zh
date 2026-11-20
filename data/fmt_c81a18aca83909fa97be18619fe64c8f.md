
# DeepSeek R1（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Mon8dE9V2ogcejxp3IbcWo4gnQr](https://kq4b3vgg5b.feishu.cn/docx/Mon8dE9V2ogcejxp3IbcWo4gnQr)

## 模型介绍

DeepSeek R1 是一款由 DeepSeek AI 开发的开源大型语言模型（LLM），是 DeepSeek AI 在其 DeepSeek-V3 基础上的进一步发展，专注于推理任务。旨在提升数学、编码和一般推理能力，特别适合需要复杂推理的应用场景。

它采用 Mixture of Experts (MoE) 架构，总参数为 6710 亿，激活参数为 370 亿，每次仅激活部分参数以提高计算效率。以下是架构相关细节的表格：

| 模型 | 架构细节 | 上下文长度 | 总参数数 | 激活参数数 |
| --- | --- | --- | --- | --- |
| DeepSeek-R1 | MoE | 128,000 | 6710 亿 | 370 亿 |

> 这种设计使模型能够在保持高性能的同时，降低计算资源需求，特别适合需要实时推理的应用程序。

模型的训练结合了强化学习和监督微调，先通过 RL 开发推理能力，再通过 SFT 改善输出可读性和连贯性。这种方法解决了早期版本（如 DeepSeek R1-Zero）中存在的可读性和语言混合问题。

除了主模型，DeepSeek 还提供了基于 Qwen 和 Llama 的蒸馏版本，尺寸从 15 亿到 700 亿参数不等，适合不同计算需求的开发人员，这为资源有限的团队提供了更多选择。DeepSeek R1 在多个基准测试中表现出色，例如 MATH-500 达到 97.3% 的通过率，CodeForces 评分为 2029，接近 OpenAI o1 的水平。蒸馏版本如 Qwen-32B 和 Llama-70B 也表现出色，适合资源有限的场景。

> 关键引用：

+ DeepSeek R1 GitHub 仓库详细介绍  
+ DeepSeek API 文档和定价信息  
+ Medium 文章：DeepSeek R1 架构与部署指南  
+ DataCamp 博客：DeepSeek R1 特性与 o1 比较  

## 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### ☑️ 实际评测

⭐️[DeepSeek-R1-0528](https://kq4b3vgg5b.feishu.cn/wiki/PipBwsglqiA3mjknHSNcmT20nLd?from=from_copylink)模型评测流程与完整测评报告（课件）

⭐️DeepSeek-R1-0528模型编程问题测试（课件）

+ [比肩Claude 4，拥有强悍MCP性能！最强开源大模型DeepSeek R1 0528性能评测与深度解读（视频）](https://www.bilibili.com/video/BV1x1jrzDEu4)
+ DeepSeek-R1-0528重大升级！新功能介绍与性能评测（视频）
+ DeepSeek R1全面解析！小尺寸推理模型企业可本地部署（视频）
+ DeepSeek R1开源情况介绍+模型组基本情况介绍（视频）
+ [零基础揭秘DeepSeek R1训练流程！多阶段GRPO强化学习训练技术揭秘！（视频）](https://www.bilibili.com/video/BV1ESAMepE4K)

### ☑️ 部署调用

🔥大模型配置硬件参考自查表！！（点我点我）

+ [万元服务器运行满血DeepSeek！全网最全低成本部署方案+硬件采购避坑指南！（视频）](https://www.bilibili.com/video/BV1K7ACewEfM)

⭐️DeepSeek R1 API接入指南（课件）

+ [DeepSeek R1 API调用实战｜自动编程、自动机器学习项目入门实战（视频）](https://www.bilibili.com/video/BV1UyfGYqE28)

⭐️DeepSeek R1本地部署与调用方法（课件）

+ [DeepSeek R1&蒸馏模型本地部署指南丨DeepSeek R1离线调用（视频）](https://www.bilibili.com/video/BV1UofEYXEtv)

⭐️[DeepSeek R1 + Open-Webui多功能企业级部署实战（课件）](https://kq4b3vgg5b.feishu.cn/wiki/Kw00w4KfCin7sRkAJjtcOYmWnYs?from=from_copylink)

⭐️[独家 KTransformers 技术实战！(课件)](https://kq4b3vgg5b.feishu.cn/wiki/HOrmwchRGi7WQBk5eBCcZFTsn3I?from=from_copylink)

+ [独家 KTransformers 技术实战！单卡4090运行DeepSeek满血版（视频）](https://www.bilibili.com/video/BV1kyAke9EBA)

⭐️[Unsloth R1动态量化部署方案（课件）](https://kq4b3vgg5b.feishu.cn/wiki/UC0Yw4WzTix4fAkboPccaop1nvg?from=from_copylink)

+ 最低0显存，独家Unsloth动态量化部署满血DeepSeek（视频）

⭐️Ktransformers+Unsloth联合部署方案实践（课件）

+ [60G内存+14G显存！！KTransformers部署Unsloth动态量化DeepSeek R1满血模型（视频）](https://www.bilibili.com/video/BV1AL9mY2E5a)

### ☑️ 进阶之路

**1、模型蒸馏**

⭐️DeepSeek R1模型蒸馏入门实战（课件）

⭐️DeepSeek R1蒸馏模型部署与调用（课件）

⭐️DeepSeek-R1蒸馏1.5B Qwen模型调用流程（课件）

+ [模型蒸馏快速入门丨从零训练DeepSeek R1 Distill模型（视频）](https://www.bilibili.com/video/BV1X1FoeBEgW)

**2、高效微调**

⭐️DeepSeek R1高效微调入门（课件）

⭐️DeepSeek R1 Distill高效微调入门实战（课件）

+ [训练定制化DeepSeek R1！零基础DeepSeek R1高效微调实战（视频）](https://www.bilibili.com/video/BV1YjN8eGE1H)

3、论文精读 - GRPO算法实战

⭐️DeepSeek R1 GRPO算法实战（课件）

+ [DeepSeek R1论文逐段精读与训练流程代码复现｜GRPO原理介绍与代码复现（视频）](https://www.bilibili.com/video/BV1n9NfezErB)

**4、本地知识库问答**

⭐️DeepSeek R1本地知识库问答快速实战（课件）

+ DeepSeek R1搭建本地知识库问答系统（视频）

---

## ❤️ 赋范社区交流群

海量硬核独家技术干货内容+无门槛技术交流~

![](img/8f28c79847af4833eaf23032b3f17967.png)

上图扫码👆即刻入群！

📍 社群技术交流氛围浓厚，不定期开设硬核干货&前沿技术公开课噢~
---
