
# DeepSeek-OCR（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Oe9KdQvIMovf85x7slCcnGwJn8f](https://kq4b3vgg5b.feishu.cn/docx/Oe9KdQvIMovf85x7slCcnGwJn8f)

## 模型介绍

DeepSeek-OCR 是一个开源的多模态 AI 模型，由 DeepSeek AI 于 2025 年 10 月 21 日发布，旨在通过视觉感知压缩文本输入来革新长上下文处理。该模型迅速成为 AI 社区的焦点，已在 GitHub 上获得广泛关注（截至发布后两天，支持数千开发者）。它支持 Python 和其他语言的集成，适用于构建高效的文档处理、数据提取和 LLM 增强应用。

DeepSeek-OCR 的核心目标是解决 LLM 在处理长上下文时的 token 限制和计算开销问题。尽管 LLM 在生成响应和任务自动化方面表现出色，但传统方法往往受限于高 token 消耗，导致成本增加和效率低下。DeepSeek-OCR 通过引入“视觉-文本压缩”范式，实现了显著的 token 减少（7-20 倍），同时保持高准确性。

DeepSeek-OCR 采用多模态架构，结合 OCR 和视觉编码技术，将文本转换为 2D 视觉表示。其核心是一个端到端的系统，包括编码器和解码器，支持高效的压缩和恢复。

以下是 DeepSeek-OCR 关键技术参数的总结：

| **参数** | **描述** |
| --- | --- |
| **架构类型** | 多模态 OCR 系统，结合视觉-文本压缩。 |
| **压缩率** | 7-20 倍 token 减少，10x 典型压缩比。 |
| **准确性** | 96%+ OCR 准确率，97% 解码精度。 |
| **训练数据生成** | 单个 A100 GPU 上 200,000+ 页/天。 |
| **上下文处理** | 支持长文档，如整个百科全书压缩成单图像。 |
| **集成支持** | Python SDK、Hugging Face、RESTful API。 |
| **硬件要求** | 单个 A100 GPU 即可高效运行。 |
| **开源许可** | MIT 许可，GitHub 仓库活跃。 |
| **部署选项** | 自托管、云端（AWS、Azure）、企业内部。 |
| **安全实践** | 数据加密、布局保留，确保隐私合规。 |

对于 LLM 开发者，DeepSeek-OCR 是一个变革性的工具。它消除了长上下文处理的瓶颈，通过视觉压缩减少计算资源需求，从而降低成本并提高效率。研究显示，该模型支持高级用例，如多模态文档分析和代理工作流集成。它还减少了 LLM 的幻觉，通过精确的文本恢复提高响应准确性，并支持自主代理的构建。

> **关键引用**：

+   DeepSeek OCR is here - Medium

+   [Revolutionary Context Compression Th](https://deepseek.ai/blog/deepseek-ocr-context-compression)rough Optical 2D Mapping - DeepSeek Blog

## 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### ☑️ 实际评测

+   [D](https://www.bilibili.com/video/BV1eJWUzTECU)eepSeek-OCR深度解读+上手指南！高精度表格、公式、CAD图字符&语义混合识别，多模态混合DeepSeek实战（视频）

### ☑️ 部署调用

⭐️ DeepSeek-OCR快速入门实战（课件）

+   [保姆级DeepSeek-OCR部署与调用指南！文字+图表+CAD图+手写体公式高精度](https://www.bilibili.com/video/BV1TPsBzaEdh)识别，PDF到MarkDown一键转化！最强开源OCR模型快速入门实战！（视频）

⭐️ [基于PaddleOCR-VL+DeepSeek-](https://kq4b3vgg5b.feishu.cn/wiki/MhIjw4jqZi83rrk4irecZgL1ncd?from=from_copylink)ocr搭建企业级多模态RAG系统（课件）

### ☑️ 进阶实战

+   [最强开源PaddleOCR-VL+DeepSeek-OCR从零搭建复杂PDF解析链路](https://www.bilibili.com/video/BV1GasBzVEej)，企业级多模态RAG系统开发实战！CAD图、架构图、图片、表格细粒度溯源（视频）

### ☑️ 团队自研工具

⭐️ DeepSeek-OCR-Web 项目已开源【使用文档】

🔗 Github开源地址：DeepSeek- OCR-Web

+   [【团队自研】DeepSeek-OCR-Web项目开源！零门槛](https://www.bilibili.com/video/BV17ks5z7E3U)一键部署+网页端一键OCR应用！（视频）

---

## ❤️ 赋范社区交流群

海量硬核独家技术**干货内容**+无门槛**技术交流**~

![](img/544c41a16c5f20f19163285cdd6729d4.png)

上图**扫码**👆即刻入群！

📍 社群技术交流氛围浓厚，不定期开设硬核干货&前沿技术公开课噢~
