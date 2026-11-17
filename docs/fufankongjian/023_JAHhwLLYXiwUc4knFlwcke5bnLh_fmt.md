
# ChatGLM-3（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/KBvtdW0gWosUyjxc55Wc6HPznSe](https://kq4b3vgg5b.feishu.cn/docx/KBvtdW0gWosUyjxc55Wc6HPznSe)

## 模型介绍

ChatGLM-3 是一个由清华大学 KEG 实验室和 智谱 AI 开发的开源大型语言模型，专注于对话生成，特别优化了中文和英文任务。它于 2023 年 10 月发布，参数规模为 62 亿，适合构建聊天机器人和内容生成工具。

ChatGLM-3-6B-Base 在多个基准测试中[表现优异，以下是部分关键指标](https://github.com/THUDM/ChatGLM3/blob/main/README_en.md)（数据来源：GitHub README）：

| 基准测试 | 得分 | 备注 |
| --- | --- | --- |
| GSM8K | 72.3 | 数学推理，0-shot CoT 测试 |
| MATH | 25.7 | 数学问题，0-shot CoT 测试 |
| BBH | 66.1 | 逻辑推理，3-shot 测试 |
| MMLU | 61.4 | 多任务语言理解，0-shot 测试 |
| C-Eval | 69.0 | 中文理解，0-shot 测试 |
| CMMLU | 67.5 | 中文多任务，0-shot 测试 |
| MBPP | 52.4 | Python 代码生成，Pass@1 |
| AGIEval | 53.7 | 综合评估，0-shot 测试 |

> 相比前代 ChatGLM2-6B，ChatGLM-3-6B 在 MMLU 上提升 23%，[GSM8K](https://arxiv.org/html/2406.12793v1) 上提升 571%，BBH 上提升 60%，显示出显著进步（arXiv 论文）。其在中文任务上的表现（如 C-Eval 69.0）尤其突出，超越了许多同规模开源模型。

ChatGLM-3-6B 是开源对话模型中的重要进展，其在中文任务上的优异表现和多功能支持使其成为开发者构建智能应用的有力工具。建议开发者结合其开源生态和社区资源，进一步探索其潜力。

> **关键引用**：

+ ChatGLM3 GitHub 存储库详细介绍  
+ ChatGLM3-6B Hugging Face 模型页面  
+ [ChatGLM 系列大型语言模型从 GLM-130B 到 GLM-4 所有工具的 arXiv 论文](https://arxiv.org/html/2406.12793v1)

## 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### 实际评测

+ ChatGLM3重磅上线丨模型性能实测丨功能迭代（视频）  
+ ⭐️ChatGLM3全新消息队列构建方法及模型性能评测（课件）  
+ ChatGLM3推理性能评测（视频）

### 部署调用

+ ⭐️ChatGLM3-6B零基础部署与使用指南（课件）  
+ ChatGLM3安装部署（上）（视频）  
+ ChatGLM3安装部署（下）（视频）

### 进阶之路

#### 1、Agent实战

+ ⭐️[ChatGLM3 Function calling功能介绍与Agent实战（课件）](https://kq4b3vgg5b.feishu.cn/wiki/QOB7wYzlOiMzzxkzJAkcFhmbnme?from=from_copylink)  
+ ChatGLM3工具调用实战（视频）  
+ ChatGLM3 Agent开发实战（视频）

---

## ❤️ 赋范社区交流群

海量硬核独家技术**干货内容**+无门槛**技术交流**~

![](img/921a6c2aeb1b96210ec2549dbc8a0b09.png)

上图**扫码**👆即刻入群！

📍 社群技术交**流氛围浓厚，**不定期开设硬**核干货&前沿技术公开课噢**~

---
