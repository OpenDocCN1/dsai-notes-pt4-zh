# GPT-OSS（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/RVMSdwGOyoVRUaxeD2zcB4ICn4d](https://kq4b3vgg5b.feishu.cn/docx/RVMSdwGOyoVRUaxeD2zcB4ICn4d)

# 模型介绍

OpenAI 于 2025 年 8 月 5 日发布了两个开源权重（open-weight）语言模型：gpt-oss-120b 和 gpt-oss-20b。这是 OpenAI 自 2019 年 GPT-2 以来首次公开模型权重，标志着公司回归开源 AI 根源的重要一步。

这两个模型是为开发者设计的，允许在本地或私有基础设施上运行，适合需要隐私保护或低延迟的应用。它们性能强劲，支持长上下文和工具使用，是开源 AI 社区的重要补充。

以下是它们的关键技术参数：

| ### **参数** | ### **gpt-oss-120b** | ### **gpt-oss-20b** |
| --- | --- | --- |
| **总参数量** | 117B | 21B |
| **活跃参数量** | 5.1B per token | 3.6B per token |
| **支持上下文长度** | 128k | 128k |
| **硬件要求** | 单个 80 GB GPU | 16 GB 内存（可在消费级设备上运行） |
| **性能基准** | 接近 OpenAI o4-mini | 接近 OpenAI o3-mini |
| **许可证** | Apache 2.0 | Apache 2.0 |

两个模型均基于 Transformer 架构，使用混合专家（Mixture-of-Experts, MoE）技术来减少活跃参数量，从而提高推理效率。MoE 允许模型在处理输入时仅激活部分参数，这在推理阶段显著降低了计算需求。

模型基于主要是英文的文本数据集训练，重点关注 STEM（科学、技术、工程、数学）、编程和通用知识领域。这种训练数据选择使其在技术相关任务中表现优异。使用 o200k_harmony 分词器（OpenAI 的 o4-mini 和 GPT-4o 的分词器超集），支持广泛的文本类型。分词器本身也已开源，方便开发者进一步定制。

两个模型均采用 Apache 2.0 许可证，允许广泛的使用、修改和分发，包括商业用途。这种许可选择特别吸引希望将模型集成到产品中的开发者，无需担心版权限制或专利风险。

GPT-OSS 模型设计用于以下场景：

+   **推理任务**：支持强指令遵循和工具使用，如网络搜索、Python 代码执行。模型提供完整的链式思维（chain-of-thought, CoT）输出，便于调试和信任。

+   **代理工作流**：适用于复杂的多步骤任务，需要与外部工具和数据源交互，如构建 AI 驱动的 IDE 或虚拟助手。

+   **定制化**：开发者可以根据特定需求进行微调和优化，支持开源工具和基础设施，不通过 OpenAI API 提供细调服务。

+   **本地部署**：适合需要数据隐私或低延迟的应用，如敏感数据处理或离线 AI 助手。

> **下载与访问**：模型可以在 Hugging Face 和 GitHub 上免费下载，方便开发者快速获取。

> **相关链接包括：**

> https://github.com/openai/gpt-osshugging

> https://huggingface.co/openai/gpt-oss-20bhugging

> https://huggingface.co/openai/gpt-oss-120b

> https://openai.com/index/introducing-gpt-oss/

# 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### ☑️** **实际评测

⭐️ GPT-OSS 模型介绍，OpenAI为何开源？

⭐️ 开源新SOTA！OpenAI最新开源GPT-OSS模型【使用指南】

+   [O](https://www.bilibili.com/video/BV16D4ZzmErg)penAI重磅开源！GPT-OSS模型介绍、评测与本地部署指南|开源新SOTA，最低16G显存运行o4级大模型！

### ☑️ 高效微调 / Agent性能微调

⭐️ [GPT-OSS高效微调实战（上）丨高效微调入门](https://kq4b3vgg5b.feishu.cn/docx/F1Mtdi4qtoM5AIxVdROc9Ai4noh?from=from_copylink)、环境准备、微调数据集专题...（课件）

⭐️ [高效微调提升模型Agent性能基本](https://kq4b3vgg5b.feishu.cn/docx/DYH5d4G1HoLnCKxhveBcuQQwnNd?from=from_copylink)原理丨GPT-OSS高效微调实战（课件）

⭐️ Unsloth快速上手指南丨中文微调链实战过程（课件）

⭐️ GPT-OSS Agent性能微调（课件）

+   [【6小时实战教程】快速提升Agent性能，GPT-OSS模型Agent性能高效微调实战](https://www.bilibili.com/video/BV159arzgEqY)！Agent性能微调数据集构造实战，多工具调用、多步调用、混合调用工具微调实践！

---

# ❤️ 赋范社区交流群

海量硬核独家技术**干货内容**+无门槛**技术交流**~

![](img/eb86310a5e574283a27c27a196dbe343.png)

上图**扫码**👆即刻入群！

📍 社群技术交**流氛围浓厚，**不定期开设硬**核干货&前沿技术公开课噢**~

---