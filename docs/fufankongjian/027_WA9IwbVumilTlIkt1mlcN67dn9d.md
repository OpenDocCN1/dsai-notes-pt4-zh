# Baichuan2（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/WKj6dbF69osKFyxDsnHcS7r2nXb](https://kq4b3vgg5b.feishu.cn/docx/WKj6dbF69osKFyxDsnHcS7r2nXb)

# 模型介绍

Baichuan2 是一个由百川智能开发的开源大型语言模型系列，于 2023 年 9 月发布。模型包括 70 亿和 130 亿参数的变体，训练于 2.6 万亿 token 的数据集上，特别优化了中文和英文任务。它完全开源，支持学术研究和商业应用（需获取商业许可）。

+   **Baichuan2-7B**：使用 RoPE 位置嵌入，隐藏层大小 4096，FFN 大小 11008，32 个头，32 层，序列长度 4096，最大学习率 2e-4。

+   **Baichuan2-13B**：使用 ALiBi 位置嵌入，隐藏层大小 5120，FFN 大小 13696，40 个头，40 层，序列长度 4096，最大学习率 1.5e-4。

Baichuan2 在多个基准测试中表现出色，以下是关键指标：

| 模型 | MMLU 得分 | CMMLU 得分 | C-Eval 得分 | GSM8K 得分 | HumanEval 得分 | JEC-QA 得分 | Toxigen 得分 | BHED 平均得分 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Baichuan2-7B-Base | 54.16 | 57.07 | 54.00 | 24.49 | 18.29 | 44.46 | 11.72 | 95.45 |
| Baichuan2-13B-Base | 59.17 | 61.97 | 58.10 | 52.77 | 17.07 | 47.40 | 11.48 | 97.50 |

> 这些模型在数学、代码生成、医学、法律和多语言任务上表现优于前代，并与 LLaMA 2 等同规模开源模型相当或更优。

Baichuan2 是开源大型语言模型中的重要进展，其在中文和英文任务上的高性能使其适合聊天机器人、内容生成和知识密集型任务。2.6 万亿 token 的训练数据和优化的 Transformer 架构确保了其鲁棒性，开发者可利用其开源特性进行定制部署，受益于强大的社区支持和资源。

> **关键引用**：

+   [B](https://arxiv.org/abs/2309.10305)aichuan 2: Open Large-scale Language Models arXiv 论文

+   Baichuan2 GitHub 存储库

+   Baichuan2-7B-Base Hugging Face 模型页面

+   [Baichuan2-13B-Ba](https://huggingface.co/baichuan-inc/Baichuan2-13B-Base)se Hugging Face 模型页面

+   MindFormers gitee 存储库

+   MindSpore 模型库 Baichuan2-7B

# 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### **☑️ **部署调用

⭐️BaiChuan 2 大模型生态介绍及本地私有化部署方案（课件）

+   Baichuan2体系介绍与私有化部署（视频）

---

# ❤️ 赋范社区交流群

海量硬核独家技术**干货内容**+无门槛**技术交流**~

![](img/eb444762908203bbdc1b699c1a864a0f.png)

上图**扫码**👆即刻入群！

📍 社群技术交**流氛围浓厚，**不定期开设硬**核干货&前沿技术公开课噢**~

---