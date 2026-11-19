
# 01🈷️25日

来源：[https://v11enp9ok1h.feishu.cn/docx/IJD7dPqPLogQ9zxbiJwcW7UHnpb](https://v11enp9ok1h.feishu.cn/docx/IJD7dPqPLogQ9zxbiJwcW7UHnpb)

> 🔔AI情报助手午报「01月25日」

> ✨✨✨✨✨✨✨✨

## 1️⃣ 微软等开源`AIOpsLab`，可构建自主云AI Agent

微软联合加州大学伯克利分校、伊利诺伊大学开源`AIOpsLab`，专注于云自动化运维的AI Agents。

`AIOpsLab`模拟复杂云操作任务，实现故障自动化检测、定位和解决，具备高度可观测性。

支持人类、数字和AI Agents协作，模块化设计易于扩展新应用程序和工作负载。

架构包含协调器、服务、工作负载生成器、故障生成器和可观测性五个关键部分。

可观测性包括`Jaeger`追踪、`Filebeat`和`Logstash`日志处理、`Prometheus`系统监控等，提升全面监控能力。

关键词：`AIOpsLab`，云自动化运维，故障检测，模块化设计，可观测性

### 🔗 文中引用的链接

- [开源地址](https://github.com/microsoft/AIOpsLab/?tab=readme-ov-file)

## 2️⃣ 社区供稿 | “迷你版”书生·万象大模型开源，5% 参数实现九成性能，“小而美”易迁移

上海人工智能实验室与清华大学联合推出`Mini-InternVL`多模态大模型，包含`1B`、`2B`和`4B`参数版本。

`Mini-InternVL-4B`以5%参数量实现约九成`InternVL2-76B`性能，显著降低计算成本。

提出迁移学习框架，实现模型知识“一键”域迁移，增强模型在特定场景的视觉理解与推理能力。

书生·万象系列模型在Hugging Face平台下载量超400万，GitHub星标数超6800。

`Mini-InternVL`模型结构由视觉编码器、`MLP`连接层和大语言模型组成，支持处理最多40个图像切片。

关键词：多模态大模型，开源，性能优化，知识迁移，人工智能

### 🔗 文中引用的链接

- [代码仓库地址](https://github.com/OpenGVLab/InternVL)
- [迁移模型权重](https://hf.co/collections/OpenGVLab/internvl-adaptation-67542cc9558812823a0281cc)
- [论文链接](https://link.springer.com/article/10.1007/s44267-024-00067-6)
- [专刊网址](https://link.springer.com/journal/44267)

## 3️⃣ `Grok 3.0` 即将发布！将成为世界上最强大的 AI

`Grok 3.0` 预训练完成，计算能力是`Grok 2`的10倍，被认为是“世界上最强大的AI”。🚀

采用 Colossus 超算集群，配备10万块 NVIDIA H100 GPU，旨在推理能力、多模态处理、运算速度上实现突破。💡

Elon Musk透露，`Grok 3`正在进行`int4`推理测试，显示出对部署效率的考虑。🔍

`Grok 3`展现了对物理规律的理解和跨维度推理能力，特别是在处理四维空间任务时的能力。🌐

`Grok 3`的对话风格显示出个性和幽默感，使其更像是一个有情感的对话者。🎭

关键词：`AI`，`Grok 3.0`，NVIDIA H100，多模态处理，跨维度推理

### 🔗 文中引用的链接

- [Grok 3模型选择器开发](https://www.testingcatalog.com/xai-developing-new-model-selector-for-grok-hinting-at-grok-3-launch/)
- [SOTA模型比较](https://tereza-tizkova.medium.com/which-llm-is-really-the-best-3920b5af2ae6)

## 4️⃣ 魔搭AIGC 2月赛超前开题：联动麦橘超然，诠释AI梦一丹一世界

魔搭社区`LoRA`创意挑战赛第四期开启，主题为“AI梦：一丹一世界”，旨在通过AI技术展现梦想世界。

麦橘超然将带领参与者探索AI的无限可能，让梦想触手可及。

赛事奖励升级，包括万元现金奖和天猫超市卡，鼓励更多参与者加入。

联合麦乐园、WaytoAGI举办线上共学活动，降低参赛门槛，与专家一同学习进步。

赛题时间为1.24-2.28，详细内容可通过提供的链接查看。

关键词：AI梦，一丹一世界，麦橘超然，`LoRA`创意挑战赛，共学活动

### 🔗 文中引用的链接

- [魔搭AIGC 2月赛详情](https://mp.weixin.qq.com/s/HrZKBTXoJOWqzOFZV7hr0w)

## 5️⃣ `MustDrop`：多阶段去除冗余视觉`token`，提升多模态大模型推理效率

国防科技大学等机构提出`MustDrop`模型，通过多阶段去除冗余视觉`token`，显著提升多模态大模型推理效率。

`MustDrop`针对视觉编码、`Prefill`和`Decode`阶段定制策略，基于`LLaVA`模型降低约88.5%的`FLOPs`。

视觉编码阶段采用局部空间融合模块`LSM`去除空间冗余，保留全局重要的视觉`tokens`。

`Prefill`阶段引入文本引导的双注意力过滤机制，精确删除无效视觉`tokens`。

`Decode`阶段采用输出注意的`cache`策略，仅保留重要视觉`tokens`，降低计算成本。

关键词：多模态大模型，视觉`token`去除，推理效率，`LLaVA`，`MustDrop`

### 🔗 文中引用的链接

- [论文链接](https://arxiv.org/pdf/2411.10803)
- [模型链接](https://www.modelscope.cn/models/TingLiu20/MustDrop)

## 6️⃣ ICLR 2025 | `Ross`: 多模态大模型的 `MAE` 时刻？

`Ross`提出了一种创新的多模态大模型，通过重建视觉信号来提升模型的细粒度理解能力，并减少幻觉现象。

该模型利用视觉监督，直接从输入图像中学习，增强了模型对视觉特征的理解。

`Ross`在不同的视觉`backbone`和`LLM`上展现出显著的性能提升，尤其是在细粒度理解任务上。

引入视觉监督后，模型的`attention values`提高，更关注正确的区域，特征包含更多细粒度信息。

`Ross`的成功标志着多模态大模型的`MAE`时刻，未来研究将聚焦于设计更好的`visual pre-text task`。

关键词：多模态大模型，视觉监督，细粒度理解，重建视觉信号，`MAE`时刻

### 🔗 文中引用的链接

- [arXiv Paper](https://arxiv.org/pdf/2410.09575)
- [Project Page](https://haochen-wang409.github.io/ross/)
- [GitHub Code](https://github.com/haochen-wang409/ross)
- Huggingface Checkpoint
