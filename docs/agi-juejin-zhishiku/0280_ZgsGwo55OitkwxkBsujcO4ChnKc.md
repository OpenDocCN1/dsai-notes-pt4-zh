
# 【`RAG`论文解读】`Self-RAG`

> 来源：[https://agijuejin.feishu.cn/docx/BqsCdQsjDosZ6XxXkwGcKqSXnle](https://agijuejin.feishu.cn/docx/BqsCdQsjDosZ6XxXkwGcKqSXnle)

## 👀快看👀

> **作者：**[**黄志国hzg0601**](https://space.bilibili.com/286387578)

> **原文链接：**【`RAG`论文解读】`Self-RAG`

---

## ⚡️摘要·作者有话说⚡️

> 本视频解读《`Self-RAG`》论文，指出传统 `RAG` 模型的不足，介绍其提出的 `SAFRAG` 框架，包括创新点、两大部分、方法流程及训练相关细节，该框架能提高语言模型生成文本的质量。

---

## 🤖分集🤖

### 【`RAG`论文解读】`Self-RAG`

先阐述背景与研究动机，指明传统模型缺陷。接着详解论文提出的 `SAFRAG` 框架，其具自适应检索生成和反思 `token` 等创新，涵盖问题定义、训练推理流程等方法，还提及训练数据集与细节，以提升语言模型生成文本的质量。

![](img/7ac5378b74eada4c9a4098c18c790eb5.png)

[【`RAG`论文解读】`Self-RAG`.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/IGNGbNaikotyaExhf4xcXEcpnLd)

---

## ✏️课代表✏️

+   传统 `RAG` 模型存在的问题：不考虑检索是否必要，以及检索内容是否与问题相关，可能导致生成无用的响应。

+   `SAFRAG` 框架的创新点：引入了自适应的检索和反思机制，通过生成反思 `token` 来提高模型的生成质量和事实性。

+   反思 `token` 的作用：用于评估检索到的文本的相关性、辨识度和有用性，以及生成内容的一致性和支持度。

+   训练和推理流程：`SAFRAG` 框架的训练分为批评模型和生成模型的训练，推理流程涉及检索决策、内容生成和反思评估。

+   论文的贡献：提出了一种新的 `RAG` 框架，通过反思机制使模型能够更好地自我评估和调整，从而提高了文本生成的质量和可靠性。

+   视频作者的看法：对论文的看法持中立态度，认为其有启发性，尤其是在引入批评和反思机制方面，但也指出有些部分缺乏说服力，对大模型的期望可能过高。
