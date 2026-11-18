
# Custom API Keys  自定义 API 密钥

> 来源：`https://kcn9ut4m7icc.feishu.cn/docx/D3jcdrlXaoYiicxqOBKcyE6gn5g`

Learn how to use your own API keys in Cursor for OpenAI, Anthropic, Google, and Azure LLM providers

学习如何在 Cursor 中使用自己的 API 密钥访问 OpenAI、Anthropic、Google 和 Azure 等提供商

Cursor lets you input your own API keys for various LLM providers to send as many AI messages as you want at your own cost. When a Customer API key is used, we will use that when calling the LLM providers.

光标允许您输入自己的 API 密钥，以便向各种 LLM 提供商发送您想要的任意数量的 AI 消息，费用由您自行承担。当使用客户 API 密钥时，我们将使用该密钥调用 LLM 提供商。

To use your own API key, go to `Cursor Settings > Models` and enter your API keys. Then, click on the `Verify` button. Once your key is validated, your API key will be enabled.

使用您自己的 API 密钥，请前往 `Cursor Settings > Models` 并输入您的 API 密钥。然后，点击 `验证` 按钮。一旦您的密钥验证通过，您的 API 密钥将被启用。

> Some Cursor features like `Tab Completion` require specialized models and won’t work with custom API keys. Custom API keys only work for features that use standard models from providers like OpenAI, Anthropic, and Google.

> 一些光标功能，如 `Tab 补全`，需要专用模型，并且不会与自定义 API 密钥一起工作。自定义 API 密钥仅适用于使用 OpenAI、Anthropic 和 Google 等提供商的标准模型的特性。

## OpenAI API Keys  OpenAI API 密钥

You can get your own API key from the OpenAI platform. `https://platform.openai.com/account/api-keys`

您可以从 OpenAI 平台获取自己的 API 密钥。

> OpenAI’s reasoning models (`o1`, `o1-mini`, `o3-mini`) require special configuration and are not currently supported with custom API keys.

> OpenAI 的推理模型（`o1`、`o1-mini`、`o3-mini`）需要特殊配置，并且目前不支持使用自定义 API 密钥。

![](img/29d4a4ba09ba60db9b895032561a9429.png)

## Anthropic API Keys  Anthropic API 密钥

Similar to OpenAI, you can also set your own Anthropic API key so that you will be using `claude-based` models at your own cost.

与 OpenAI 类似，您也可以设置自己的 Anthropic API 密钥，以便您可以使用基于 `Claude` 的模型并自行承担费用。

![](img/486dbfb4bacfeff4abb0d4c222ddceec.png)

## Google API Keys  谷歌 API 密钥

For Google API keys, you can set your own API key so that you will be using Google models such as `gemini-1.5-flash-500k` at your own cost.

谷歌 API 密钥，您可以设置自己的 API 密钥，这样您就可以使用谷歌模型（如 `gemini-1.5-flash-500k`）并自行承担费用。

![](img/b5d93c07d0b2db952390fdecd8b603f0.png)

## Azure Integration  Azure 集成

Finally, you can also set your own Azure API key so that you will be using Azure OpenAI models at your own cost.

最后，您还可以设置自己的 Azure API 密钥，这样您就可以使用 Azure OpenAI 模型并自行承担费用。

![](img/a0601bbd8e667c2d3e94080cf9b19e2f.png)

## FAQ  常见问题解答

### Will my API key be stored or leave my device? 我的 API 密钥会被存储还是离开我的设备？

Your API key will not be stored, but it will be sent up to our server with every request. All requests are routed through our backend where we do the final prompt building.

您的 API 密钥将不会存储，但每次请求都会发送到我们的服务器。所有请求都通过我们的后端路由，我们在那里完成最终的提示构建。

### What custom LLM providers are supported? 支持哪些自定义 LLM 提供商？

Cursor only supports API providers that are compatible with the `OpenAI API` format (like `OpenRouter`). We do not provide support for custom local LLM setups or other API formats. If you’re having issues with a custom API setup that isn’t from our supported providers, we unfortunately cannot provide technical support.

光标仅支持与 `OpenAI API` 格式兼容的 API 提供商（如 `OpenRouter`）。我们不提供对自定义本地 LLM 设置或其他 API 格式的支持。如果您在使用来自我们支持的提供商之外的 API 设置时遇到问题，很遗憾我们无法提供技术支持。
