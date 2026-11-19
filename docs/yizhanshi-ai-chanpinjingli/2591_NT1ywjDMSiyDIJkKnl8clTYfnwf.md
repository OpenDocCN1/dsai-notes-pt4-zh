
# 8🈷️10日

来源：https://v11enp9ok1h.feishu.cn/docx/JnJIdqRBNoTjIGxWyJ5cQ1aJnxg

> 🔔AI情报助手早报「08月10日」

> ✨✨✨✨✨✨✨✨

## 1️⃣ OpenAI 重回开源，发布 `gpt-oss-120b` 和 `gpt-oss-20b`

🤖

OpenAI 于 2025 年 8 月 8 日深夜官宣重回开源，发布两款模型 `gpt-oss-120b` 和 `gpt-oss-20b`，系自 GPT-2 后首次开源。

`gpt-oss-120b` 适用于高推理场景，`gpt-oss-20b` 适用于低延迟场景，二者在多项基准测试中表现强劲。

模型采用先进预训练和后训练技术，许可证宽松，支持微调，有完整思维链等亮点。

其预训练成本低于 50 万美元，与 `Responses API` 兼容，推理投入可配置。

此次开源为开发者提供强大工具，降低部分群体准入门槛。

关键词：OpenAI，开源模型，`gpt-oss`，推理能力，后训练

🔗 文中引用的链接

- [体验地址](https://gpt-oss.com/)
- [GitHub 项目](https://github.com/openai/gpt-oss)
- Hugging Face（120B）
- Hugging Face（20B）
- [模型卡](https://cdn.openai.com/pdf/419b6906-9da6-406c-a19d-1bb078ac7637/oai_gpt-oss_model_card.pdf)
- [参考资料](https://openai.com/index/introducing-gpt-oss/)

## 2️⃣ 深夜读完 OpenAI `gpt-oss-...`

🤖

2025 年 8 月 5 日，OpenAI 开源推理模型 `gpt-oss-120b` 和 `gpt-oss-20b`，被认为位于开源第一梯队。

技术上，架构为经典 `MoE` 的“细节拉满”版，分词器有创新，采用三段式训练。

评测效果佳，在 `AIME 2025`、`SWE - Bench Verified` 等测试中表现出色。

可通过 3 条命令快速上手，`Harmony Chat` 的 `JSON` 模板可照抄附录。

该模型适合复杂工作流 `Agent` 等场景，值得相关领域实测。

关键词：OpenAI，`gpt-oss`，开源模型，技术拆解，评测

🔗 文中引用的链接

- [1] OpenAI. `gpt-oss-120b` & `gpt-oss-20b` Model Card, 2025-08-05
- [2] 知乎问答：如何看待 OpenAI 开源 `MoE` 模型 `gpt-oss-120b` & `gpt-oss-20b`？

## 3️⃣ Qwen 紧追 OpenAI 开源 4B 端侧大模型

🤖

2025 年 8 月 7 日深夜，Qwen 开源两款 4B 端侧大模型：`Qwen3-4B-Instruct-2507` 与 `Qwen3-4B-Thinking-2507`，尺寸友好，甚至可在树莓派运行。

两模型更智能精准，支持 256k 上下文，在 `AIME25` 上，`Qwen3-4B-Thinking-2507` 得分 81.3，超越 `Gemini 2.5 Pro` 和 `Claude 4 Opus`。

`Qwen3-4B-Instruct-2507` 提升通用能力等，`Qwen3-4B-Thinking-2507` 增强推理能力。

代码可在抱抱脸和魔搭社区获取，官方给出部署及 `prompt` 建议。

`Qwen` 此次更新恰逢 OpenAI 开源，且 `GPT - 5` 或于 8 月 8 日凌晨发布。

关键词：Qwen，端侧大模型，开源，`AIME25`，性能提升

🔗 文中引用的链接

- [1] https://huggingface.co/Qwen/Qwen3-4B-Instruct-2507
- [2] https://huggingface.co/Qwen/Qwen3-4B-Thinking-2507
- [1] https://modelscope.cn/models/Qwen/Qwen3-4B-Instruct-2507
- [2] https://modelscope.cn/models/Qwen/Qwen3-4B-Thinking-2507
- [1] https://x.com/Alibaba_Qwen/status/1953128028047102241
- [2] https://www.reddit.com/r/LocalLLaMA/comments/1mj8ndr/qwen_3_4b_thinking_model_released/
- [3] https://news.ycombinator.com/item?id=44813627

## 4️⃣ `GPT-OSS-20B` 和 `Qwen3 30B-A3B` 模型对比

🎯

文章对比了 `GPT-OSS-20B` 与 `Qwen3 30B-A3B`，`GPT-OSS` 虽开源但风评一般。

`GPT-OSS` 技术细节有隔层 `Sliding Widow` 等，推理分三档。

实测中，`Qwen3` 在简单通用、创作等任务上完爆 `GPT-OSS`，而 `GPT-OSS` 在数学推理上表现较好。

整体来说，OpenAI 此次开源不及预期，国内开源模型实力强劲。

作者表示会选择 `Qwen3 30B-A3B`。

关键词：`GPT-OSS-20B`，`Qwen3 30B-A3B`，模型对比，开源模型

🔗 文中引用的链接

- [HF 链接](https://huggingface.co/collections/openai/gpt-oss-68911959590a1634ba11c7a4)
- [x.com 链接 1](https://x.com/rasbt/status/1952842273848279364)
- [x.com 链接 2](https://x.com/rasbt/status/1952842273848279364)

## 5️⃣ DeepSeek 的 `GRPO` 会导致模型崩溃...

🎯

DeepSeek 提出 `GRPO` 算法提升训练效率，但 Qwen 团队研究表明，`GRPO` 在 token 级别应用重要性采样，会导致训练不稳定，使模型崩溃。

Qwen 团队提出新算法 `GSPO`，将重要性采样提升到序列级别并归一化，解决了 `GRPO` 的问题。

实验显示，`GSPO` 训练效率更高，在 `CodeForces` 任务中表现优于 `GRPO`。

针对 `MoE` 模型，`GSPO` 无需“路由技巧”即可稳定收敛，简化了训练。

`GSPO` 或成大语言模型后训练强化学习新标准。

关键词：`GRPO`，`GSPO`，大语言模型，强化学习，模型稳定性

🔗 文中引用的链接

- [论文链接](https://huggingface.co/papers/2507.18071)
- [博客链接 1](https://qwenlm.github.io/blog/gspo/)
- [博客链接 2](https://blog.netmind.ai/article/Qwen_Team_Proposes_GSPO_for_Qwen3%2C_Claims_DeepSeek's_GRPO_is_Ill-Posed)
- [参考链接 1](https://www.reddit.com/r/MachineLearning/comments/1mj3t3r/d_gspo_qwen3s_sequencelevel_rlhf_method_vs_grpo/)
- [参考链接 2](https://www.ft.com/content/ea803121-196f-4c61-ab70-93b38043836e?utm_source=chatgpt.com)
- [参考链接 3](https://zhuanlan.zhihu.com/p/22845155602)

## 6️⃣ 硬核拆解大模型，从 `DeepSeek-V3` 到 `Kimi K2`

🎯

文章硬核拆解主流大模型架构，从 `DeepSeek-V3` 到 `Kimi K2`，探讨 `LLM` 架构演进。

`DeepSeek V3` 引入多头潜在注意力和 `MoE` 提升效率，`OLMo 2` 在归一化策略有创新。

`Gemma 3` 用滑动窗口注意力降内存需求，`Mistral Small 3.1` 推理速度更快。

`Llama 4`、`Qwen3` 采用 `MoE` 架构，`SmolLM3` 无位置嵌入有独特表现。

`Kimi K2` 基于 `DeepSeek-V3` 扩展，训练用 `Muon` 优化器变体表现出色。

关键词：大模型，架构拆解，`LLM`，`MoE`，优化器

🔗 文中引用的链接

- `DeepSeek-V3 Technical Report`
- [`DeepSeekMoE: Towards Ultimate Expert Specialization in Mixture-of-Experts Language Models`](https://arxiv.org/abs/2401.06066)
- 2 `OLMo 2 Furious`
- `Scaling Vision Transformers`
- `Gemma 3 Technical Report`
- [`The Impact of Positional Encoding on Length Generalization in Transformers`](https://arxiv.org/abs/2305.19466)
