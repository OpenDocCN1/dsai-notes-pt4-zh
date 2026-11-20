
# OpenAI o1（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/MR6jdJhajoJjuFxr3ggckEE7nKh](https://kq4b3vgg5b.feishu.cn/docx/MR6jdJhajoJjuFxr3ggckEE7nKh)

## 模型介绍

OpenAI o1 是一系列大型语言模型，旨在通过链式思维提升复杂问题的解决能力。它的发布标志着 OpenAI 在推理领域的新突破，特别适合需要多步骤逻辑的任务。

o1 被设计为 GPT-4o 的补充而非替代，专注于复杂推理任务。GPT-4o 更适合快速响应和通用任务，而 o1 在数学和科学基准测试中表现更优，例如在 GPQA（物理、生物、化学博士级问题）上超越人类专家水平。然而，o1 的响应时间较长，API 定价较高，开发者需权衡性能与成本。

o1 模型的核心在于其链式思维推理能力，这是一种通过分解问题为中间步骤来模拟人类推理过程的方法。这种方法显著提升了模型在数学、编码和科学任务上的表现。例如，在美国数学邀请赛（AIME）基准测试中，o1 pro 模式得分达 86%，远超 GPT-4o 的 13%。

全 o1 模型支持多模态输入，包括文本和图像分析。用户可上传图像（如鸟舍照片或数据中心草图），模型可提供详细的构建指导或技术反馈。这一功能扩展了其在视觉数据处理中的应用潜力。目前的模型变体有：

+ **o1**：全功能模型，适合需要广泛知识和深度推理的任务，支持图像分析。

+ **o1-mini**：轻量版，优化速度和效率，成本降低 80%，特别适合编程和 STEM 相关任务。

+ **o1 pro 模式**：增强版，提供更高精度和可靠性，适用于专业场景。

| 模型 | 上下文窗口 | 最大完成令牌 | 图像分析 | 适用场景 | 成本 |

| --- | --- | --- | --- | --- | --- |

| o1 | 128,000 | 32,768 | 支持 | 广泛推理，科学研究 | 较高 |

| o1-mini | 128,000 | 65,536 | 不支持 | 编程、STEM 任务，快速响应 | 较低（80% 折扣） |

| o1 pro 模式 | 128,000 | 32,768 | 支持 | 高精度专业场景 | 最高 |

> **关键引用：**

+ [OpenAI o1 explained: Everything you need to know](https://www.techtarget.com/whatis/feature/OpenAI-o1-explained-Everything-you-need-to-know)

+ [OpenAI o1 Guide: How It Works, Use Cases, API & More](https://www.datacamp.com/blog/open-ai-o1)

+ OpenAI o1 - Wikipedia

+ [OpenAI Platform](https://platform.openai.com/docs/models)

+ [Chain-of-Thought Prompting Elicits Reasoning in Large Language Models](https://www.techtarget.com/searchenterpriseai/definition/chain-of-thought-prompting)

+ [OpenAI O1 API Tutorial: How to Connect to OpenAI's API](https://www.datacamp.com/tutorial/openai-o1-api)

+ [What is the token context window size of the GPT-4 o1-preview model?](https://community.openai.com/t/what-is-the-token-context-window-size-of-the-gpt-4-o1-preview-model/954321)

+ [OpenAI launches full o1 model with image uploads and analysis, debuts ChatGPT Pro](https://venturebeat.com/ai/openai-launches-full-o1-model-with-34-reduced-error-rate-debuts-chatgpt-pro/)

## 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### ☑️ 实际评测

+ OpenAI o1大模型深度详解（视频）

### ☑️ 部署调用

⭐️OpenAI o1模型快速入门丨模型部署调用丨小游戏开发实战（课件）

+ OpenAI o1推理大模型快速入门实战（视频）

---

## ❤️ 赋范社区交流群

海量硬核独家技术**干货内容**+无门槛**技术交流**~

![](img/e9624a2604820c19798d70741e098226.png)

上图**扫码**👆即刻入群！

📍 社群技术交**流氛围浓厚，**不定期开设硬**核干货&前沿技术公开课噢**~
---
