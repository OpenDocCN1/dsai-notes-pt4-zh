# Claude系列（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/JY8RdvnP2ozIKixlBoXcXPJZncg](https://kq4b3vgg5b.feishu.cn/docx/JY8RdvnP2ozIKixlBoXcXPJZncg)

# 模型介绍

Claude 系列是由 Anthropic 开发的 AI 模型，旨在提供高性能、安全的对话和任务处理能力。

Claude 3 系列于 2024 年 3 月发布，包括 Haiku、Sonnet 和 Opus 三个模型，分别针对速度、性能和成本进行优化。Claude 3.5 Sonnet 则于 2024 年 6 月发布，是对 Claude 3 系列的重大升级，特别是在推理和编码任务上表现更强。

+   **Claude 3系列**

+   **Claude 3 Haiku：**作为系列中最快的模型，适合需要即时响应的任务，如实时客户聊天和数据提取。

+   Claude 3 Sonnet：提供性能和速度的平衡，适合内容创建、编码辅助和复杂数据分析。

+   **Claude 3 Opus：**性能最高，适合研究、战略规划和高复杂度任务。

+   Claude 3.5 Sonnet

+   **性能提升**：在研究生级推理（GPQA）上表现优异，MMLU 得分 82.4%，HumanEval 编码任务准确率 74.5%。

+   **速度**：比 Claude 3 Opus 快 2 倍，适合复杂任务如多步骤工作流。

+   **多模态能力**：支持图像理解，在多模态数学和光学字符识别（OCR）上排名靠前。

+   **技术规格**：上下文窗口 200,000 令牌，定价为每百万输入令牌 3 美元，输出令牌 15 美元。

+   **可用性**：通过 API、Amazon Bedrock 和 Google Cloud’s Vertex AI 访问，适合开发者构建自定义 AI 解决方案。

| 模型 | 上下文窗口 | 定价（每百万令牌） | MMLU 得分 | GPQA 得分 | GSM8K 准确率 | 主要用例 |
| --- | --- | --- | --- | --- | --- | --- |
| Claude 3 Haiku | 200,000 | 低 | ~70.0% | ~42.0% | 78.0% | 实时聊天，数据提取 |
| Claude 3 Sonnet | 200,000 | 中 | ~76.0% | ~48.0% | 85.0% | 内容创建，复杂数据分析 |
| Claude 3 Opus | 200,000 | 高 | ~79.0% | 51.0% | 89.0% | 研究，战略规划 |
| Claude 3.5 Sonnet | 200,000 | 输入 3 美元，输出 15 美元 | 82.4% | 54.6% | - | 复杂任务，多模态理解 |

> Claude 3 系列和 Claude 3.5 Sonnet 为开发者提供了强大的工具，覆盖从快速低成本到高性能复杂任务的广泛需求。

> **关键引用：**

+   Meet Claude Anthropic

+   [Introducing Claude ](https://www.anthropic.com/news/claude-3-5-sonnet)3.5 Sonnet Anthropic

+   [Introducing the next generation](https://www.anthropic.com/news/claude-3-family) of Claude Anthropic

+   [Claude 3 Models Co](https://claudeaihub.com/claude-3-models-compared/)mpared Claude AI Hub

+   [Comparison Analysis Claude ](https://www.vellum.ai/blog/claude-3-5-sonnet-vs-gpt4o)3.5 Sonnet vs GPT-4o

+   [Claude 2/3/3.5 Benchmarks & ](https://beginswithai.com/claude-benchmarks-performance/)Reviews Begins w/ AI

# 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### ☑️** **实际评测

+   Claude 4正式发布！实测最强编程大模型（视频）

+   Claude 3.5大模型综合介绍与性能详解（视频）

+   Claude 3模型功能介绍与性能评测（视频）

### **☑️ **部署调用

⭐️Claude3.5 API调用方法介绍（课件）

⭐️Claude 3.5账号注册与API-KEY获取流程（课件）

+   Claude 3.5账号注册与API-KEY获取（视频）

⭐️Claude 3账号注册与API-KEY获取流程（课件）

+   Claude API-KEY获取与本地调用指南（视频）

### **☑️ **进阶之路

**1、入门实战教程**

+   [C](https://www.bilibili.com/video/BV1QM4m1m7GV)laude 3.5快速入门实战教程丨文本对话、多模态对话、Function calling功能（视频）

---

# ❤️ 赋范社区交流群

海量硬核独家技术**干货内容**+无门槛**技术交流**~

![](img/a8ace5f4ade4b7c4c7635fb89fcbe4e4.png)

上图**扫码**👆即刻入群！

📍 社群技术交**流氛围浓厚，**不定期开设硬**核干货&前沿技术公开课噢**~

---