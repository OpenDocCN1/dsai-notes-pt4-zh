
# Qwen3 你需要了解的 8 个问题

> 来源：[https://qianniucity.feishu.cn/docx/JmrMdP2R3o2imCxbyeHcNoxRnrd](https://qianniucity.feishu.cn/docx/JmrMdP2R3o2imCxbyeHcNoxRnrd)

![](img/4bb99ccd360e435b5738644c90059177.png)

[Qwen3：思深，行速.wav](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DOwfbV08wo3s7Ox19ubc9II7nSh)

## Qwen3 是什么？

Qwen3 是 Qwen 系列大型语言模型的最新版本。它代表了在模型性能、多语言支持和 Agent 能力方面的重要进步。

## Qwen3 有哪些主要的模型版本？

Qwen3 发布了多种模型，包括两个 MoE（混合专家）模型和六个 Dense（密集）模型。MoE 模型是 `Qwen3-235B-A22B` 和 `Qwen3-30B-A3B`。Dense 模型包括 `Qwen3-32B`、`Qwen3-14B`、`Qwen3-8B`、`Qwen3-4B`、`Qwen3-1.7B` 和 `Qwen3-0.6B`。所有开源模型均采用 Apache 2.0 许可。

## Qwen3 的核心亮点是什么？

Qwen3 的核心亮点包括：

+ 多种思考模式： 支持“思考模式”进行深入推理，和“非思考模式”进行快速响应，允许用户根据任务需求灵活控制模型的“思考预算”。

+ 多语言能力： 支持 119 种语言和方言，极大地扩展了模型的全球适用性。

+ 增强的 Agent 能力： 优化了 Agent 和代码能力，并加强了对 `MCP` 的支持，方便用户构建更强大的 Agent 应用。

## Qwen3 在预训练方面有哪些改进？

Qwen3 在预训练数据集方面相比 `Qwen2.5` 实现了显著扩展，使用了约 36 万亿个 token 的数据，几乎是 `Qwen2.5` 的两倍，并涵盖了 119 种语言。预训练过程分为三个阶段，逐步增加数据量、知识密度和上下文长度，以提升模型的通用能力和长文本处理能力。

## Qwen3 的后训练过程是如何提升模型能力的？

Qwen3 的后训练采用了四阶段流程：长思维链冷启动、长思维链强化学习、思维模式融合和通用强化学习。这个流程旨在 equipping 模型具备基础的推理能力，增强探索和钻研能力，无缝结合推理和快速响应能力，并进一步提升通用能力和纠正不良行为。

## 如何使用 Qwen3 模型进行开发？

用户可以通过 Hugging Face、ModelScope 和 Kaggle 等平台获取 Qwen3 模型。对于部署，推荐使用 `SGLang` 和 `vLLM` 等框架；对于本地使用，Ollama、LMStudio、MLX、`llama.cpp` 和 KTransformers 等工具是很好的选择。文章中提供了使用 Hugging Face `transformers` 库加载和使用模型的示例代码。

## 如何控制 Qwen3 的思考模式？

Qwen3 默认启用思考模式。用户可以通过设置 `enable_thinking=False` 来禁用思考模式。此外，Qwen3 还提供了一种软切换机制，允许用户在多轮对话中通过在用户输入或系统消息中添加 `/think` 和 `/no_think` 标签来动态切换思考模式。

## Qwen3 的未来发展方向是什么？

Qwen3 是迈向通用人工智能（AGI）和超级人工智能（ASI）的重要一步。未来，Qwen 团队计划从多个维度提升模型，包括优化架构和训练方法，扩展数据和模型规模，延长上下文长度，拓宽模态范围，并利用环境反馈推进强化学习，实现长周期推理。未来的迭代将更侧重于训练 Agent，旨在为用户的工作和生活带来有意义的进步。

## Qwen3 模型深度解析

![](img/5d5f81d6976bbb1738e8fb61be72e063.png)
