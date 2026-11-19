
# Models 模型

> 来源：[https://kcn9ut4m7icc.feishu.cn/docx/UIiKddrGJoMMA0xE85Zc4Bj9nWc](https://kcn9ut4m7icc.feishu.cn/docx/UIiKddrGJoMMA0xE85Zc4Bj9nWc)

Switch between AI models in Cursor using `Chat`, `Composer`, `Tab`, or `Agent` with different pricing tiers

在 Cursor 中使用 `Chat`、`Composer`、`Tab` 或 `Agent` 在不同定价层级之间切换 AI 模型

With `Composer`, `⌘ K`, and `Terminal Ctrl/⌘ K`, you can easily switch between different models of your choice.

使用 `Composer`、`⌘ K` 和终端 `Ctrl/⌘ K`，您可以轻松地在您选择的不同模型之间切换。

## Available models 可用模型

> Premium models count against your monthly request quota based on your subscription plan. Once you exceed your quota, additional requests can be purchased by enabling usage based pricing.

> 高级模型根据您的订阅计划计入每月请求配额。一旦超出配额，可以通过启用基于使用量的定价来购买额外请求。

Pro and Business plans get 500 requests/month included, and can be extended by enabling usage based pricing.

专业和企业计划每月包含 500 个请求，并可通过对基于使用量的定价进行扩展。

You can add additional models under `Cursor Settings > Models`. All models are hosted on US-based infrastructure.

您可以在 `Cursor Settings > Models` 下添加更多模型。所有模型均托管在美国基础设施上。

## Model dropdown 模型下拉菜单

![](img/b0b3a8a769ba6e36bfe96045748ed64c.png)

Underneath the AI input box, you will see a dropdown that allows you to select the model you want to use. The following models are available:

在 AI 输入框下方，您将看到一个下拉菜单，允许您选择您想要使用的模型。以下模型可供选择：

## Context windows 上下文窗口

In `Chat` and `Composer`, we use a 40,000 token context window by default. For `Cmd-K`, we limit to around 10,000 tokens to balance TTFT and quality. `Agent` starts at 60,000 tokens and supports up to 120,000 tokens. For longer conversations, we automatically summarize the context to preserve token space. Note that these threshold are changed from time to time to optimize the experience.

在聊天和作曲器中，我们默认使用 40,000 个 token 的上下文窗口。对于 `Cmd-K`，我们限制在约 10,000 个 token 以平衡 TTFT 和质量。代理从 60,000 个 token 开始，支持高达 120,000 个 token。对于较长的对话，我们自动总结上下文以保留 token 空间。请注意，这些阈值会不时更改以优化体验。
