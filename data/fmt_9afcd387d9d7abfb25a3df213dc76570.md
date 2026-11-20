
# 【论文导读】大语言模型综述（五）：预训练和微调技术

> 来源：https://agijuejin.feishu.cn/docx/H3K7dZhAboWaSwxHK4zccyn6nec

## 👀快看👀

> **作者：**[**Sa神带你学AI**](https://space.bilibili.com/3546681717033402)
>
> **原文链接：**【论文导读】大语言模型综述（五）：预训练和微调技术

## ⚡️摘要⚡️

> 本期内容围绕大语言模型的预训练和微调展开讲解，内容涵盖常用预训练数据集及特点、预训练技术、指定条件数据集、微调方法、训练技巧与实践，分析模型性能与数据量和规模的关系、拉玛模型相关情况、训练成本、评估 benchmark 及有害输出控制技术等。

## 🤖分集🤖

### 【论文导读】大语言模型综述（五）：预训练和微调技术

介绍大语言模型预训练和微调，包括常用数据集、预训练技术、指定条件数据集、微调方法、训练实践，分析模型性能关系、拉玛模型、训练成本、评估指标和有害输出控制。

![](img/2936187907c2b006bce618cd3180a126.png)

[【论文导读】大语言模型综述（五）：预训练和微调技术.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/JYuebi2pdoRlitxZiTFcQHv5nmh)

## ✏️课代表✏️

- 常用的预训练数据集有书籍、Common Crawl、C4、Reddit、Wikipedia、BigQuery 等，它们各有特点和格式。
- 预训练技术包含数据收集、预处理、去重、隐私减除、次元化等步骤，模型架构选择很重要，如 Transformer 的 Decoder 结构。
- 数据集而非模型架构是提升模型性能的关键因素，如 OpenAI 的 GPT-3.5 和 DeepMind 的 AlphaCode 的数据集有独特特点。
- 用于微调模型的指定条件数据集有 Stanford Alpaca 等，微调方法包括人工创建数据集等。
- “小模型 + 大数据”可能比“大模型 + 小数据”更优，去重能显著提升模型性能。
- 拉玛模型的训练数据集包括 C4 等，其架构使用了 Rotary Position Embedding 和 Switch Transformer 等技术。
- 展示了拉玛模型的训练成本，列举 20 个评估 benchmark 用于测试模型表现，还讨论了有害输出控制技术。
