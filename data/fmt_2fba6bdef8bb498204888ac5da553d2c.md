
# DeepSeek Janus（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/UbpOdGJlpoNzbtxhwQmcvdNznSh](https://kq4b3vgg5b.feishu.cn/docx/UbpOdGJlpoNzbtxhwQmcvdNznSh)

## 模型介绍

DeepSeek Janus 是由 DeepSeek AI 推出的开源多模态大语言模型（MLLM），它通过一个创新的自回归框架，统一了图像理解和生成任务，旨在解决传统多模态模型在任务冲突上的局限性。

Janus 的核心是一个自回归 `Transformer`，基于 `DeepSeek-LLM-1.3B`（13 亿参数）。其独特之处在于将视觉编码分为两条独立路径，同时保持单一 `Transformer` 处理所有任务：

| 模型 | 基础模型 | 参数规模 | 上下文长度 | 图像分辨率 |
| --- | --- | --- | --- | --- |
| DeepSeek Janus | `DeepSeek-LLM-1.3B` | 13 亿 | 4096 | 384x384 |

> 这种解耦设计缓解了视觉编码器在理解和生成任务间的冲突，提升了模型的灵活性和性能。

Janus 在多模态理解（如图像问答）和文本到图像生成（如根据提示生成图像）方面表现出色。研究表明，其性能超越了之前的统一模型，并在特定任务上与专用模型相当。它适用于教育工具、内容生成和视觉分析等场景，尤其适合需要轻量级多模态能力的开发人员。

Janus 在多模态理解和生成任务中表现出色，以下是关键基准测试结果：

| 任务类型 | 基准测试 | Janus 表现 | 比较模型 |
| --- | --- | --- | --- |
| 多模态理解 | `POPE`, `MME`, `GQA` | 优于 `LLaVA-13B` | 接近 `GPT-4 Vision` |
| 文本到图像生成 | `GenEval` | 超越 `SD 1.5` | 未达 `Flux` 水平 |

> 尽管图像生成质量不及顶级专用模型（如 `Flux` 或 `DALL-E 3`），Janus 在轻量级模型中的表现令人印象深刻，尤其是在资源受限环境下的多功能性。

#### 关键引用：

+   DeepSeek Janus GitHub 仓库

+   [Janus 技术报告](https://github.com/deepseek-ai/Janus/blob/main/docs/janus_tech_report.pdf)

+   Hugging Face Janus 模型页面

## 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### ☑️ 实际评测

+   Janus Pro入门介绍丨文生图+图像识别（视频）

### ☑️ 部署调用

⭐️DeepSeek Janus本地部署与调用实战（课件）

⭐️Janus Pro本地调用指南(课件)

+   Janus Pro本地部署与调用实战！（视频）

---

## ❤️ 赋范社区交流群

海量硬核独家技术干货内容+无门槛技术交流~

![](`img/aec141075949a9aa452afda8af19b051.png`)

上图扫码👆即刻入群！

📍 社群技术交流氛围浓厚，不定期开设硬核干货&前沿技术公开课噢~

---
