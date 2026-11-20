
# LoRA（README）

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/Zoc5dmEQUop2Uex02Ahc82xcnrH](https://kq4b3vgg5b.feishu.cn/docx/Zoc5dmEQUop2Uex02Ahc82xcnrH)

## 技术介绍

LoRA，全称低秩适应，LoRA是一种参数高效的微调方法，通过减少可训练参数数量，降低计算和内存需求，特别适合资源有限的场景。它能够高效微调大型语言模型，使模型适应具体开发应用的特殊需求。对于垂直领域（如医疗、金融或法律），LoRA允许模型适应特定行业的需求：

+ 医疗：理解医学术语，生成诊断报告。
+ 金融：分析财务报告，生成市场预测。
+ 法律：处理法律文件，生成合同条款。

LoRA在此场景下的益处包括：

1. **计算效率**：减少可训练参数，训练时间缩短，内存需求降低，适合标准GPU或CPU。
2. **知识保留**：通过冻结原权重，保留通用语言能力，同时学习领域知识，减少灾难性遗忘。
3. **灵活性**：可选择性应用LoRA到特定层，针对领域需求定制模型。
4. **成本效益**：降低微调成本，适合中小企业或资源有限的组织。

> 此外，LoRA支持多领域适配，可为不同客户创建定制权重，无需单独模型，增强部署灵活性。

> **关键引用：**

+ Hugging Face `PEFT` Documentation
+ [Original LoRA Paper: LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685)
+ [Efficient Fine-Tuning with LoRA: A Guide to Optimal Parameter Selection for Large Language Models](https://www.databricks.com/blog/efficient-fine-tuning-lora-guide-llms)
+ [Fine-Tuning Large Language Models with LORA: Demystifying Efficient Adaptation](https://medium.com/@kailash.thiyagarajan/fine-tuning-large-language-models-with-lora-demystifying-efficient-adaptation-25fa0a389075)

## 快速阅览

我们团队在文档不同的位置进行了详细的介绍，您可以点击进入并查看具体的技术文档。

### ☑️ LoRA微调入门

⭐️[LoRA理论介绍及基于LoRA微调一个医疗垂直领域的Qwen模型（课件）](https://kq4b3vgg5b.feishu.cn/wiki/FOuSwOGqJi97cOktI8ycEW8onAg?from=from_copylink)

+ [LoRA微调原理（视频）](https://www.bilibili.com/video/BV15z421D79k)
+ LoRA+Qwen大模型（视频）
+ 借助`PEFT`库导入和使用LoRA模型（视频）

### ☑️ Qwen1.5 高效微调

⭐️Qwen1.5模型高效微调（课件）

+ 开源大模型微调方法入门介绍（视频）
+ Llama-Factory项目介绍与安装（视频）
+ [微调数据集创建过程（视频）](https://www.bilibili.com/video/BV1oE421378v?p=3)
+ Lora微调脚本创建与运行（视频）
+ [微调权重合并与性能测试（视频）](https://www.bilibili.com/video/BV1oE421378v?p=5)

---

## ❤️ 赋范社区交流群

海量硬核独家技术**干货内容**+无门槛**技术交流**~

![](img/a1b633839693905b24493a2603154716.png)

上图**扫码**👆即刻入群！

📍 社群技术交流氛围浓厚，不定期开设**硬核干货**&**前沿技术公开课**噢~

---
