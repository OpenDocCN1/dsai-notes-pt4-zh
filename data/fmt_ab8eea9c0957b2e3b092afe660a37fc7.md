
# 🤔 一图看懂提示词设计维度

去年 一泽Eze 在《我深入拆解了 Claude 曝光的内置提示词，不愧是官方最佳实践》一文中，详细拆解过 Claude 3.5 sonnet 的官方提示词，并对每句提示词的作用进行了分析。

如果对 `system prompt` 如何对大模型 AI 起作用不是很了解，依然推荐再次阅读。

以下是 Claude 3.7 sonnet 系统提示词的主要设计维度：

![](img/d213a423553d97701cdb7c112e7b9f1a.png)

# 🆚 一表看清与 Claude 3.5 sonnet 提示词的设计变化

相较于官方公布的 Claude 3.5 sonnet （Nov 22nd, 2024）提示词，Claude 3.7 sonnet 更加强调了对话的主动性和引导能力，并给出了更多“AI 个性”设定、伦理和安全规范，对回应方式有了多处优化。

| 特性 | Claude 3.5 提示词 | Claude 3.7 提示词 | 设计意图 |

| --- | --- | --- | --- |

| 对话主动性 | 强调“engage in conversation”，更多是被动响应。 | 明确指出 Claude 可以“lead or drive the conversation”，“suggest topics”，“take the conversation in new directions”，更像一个积极的对话伙伴。 | 让 Claude 更像一个主动的、有引导能力的对话伙伴，而不仅仅是被动响应者，提升用户体验。 |

| 个性设定 | 较为笼统地描述为“intellectually curious”。 | 更具体地描述了 Claude 的性格特征：乐于助人、智慧、善良、有深度、果断，对科学和哲学问题感兴趣。 | 塑造更鲜明、更人性化的 AI 形象，增强用户对 Claude 的信任感和亲近感。 |

| 伦理与安全规范 | 有关于敏感话题、危险活动、有害请求的处理原则。 | 在此基础上，增加了：用户福祉优先（明确避免鼓励自残行为）；儿童安全（特别强调对涉及儿童内容的谨慎）；不创作涉及真实公众人物的内容；不提供制造武器、恶意代码等信息（更加具体）。 | 进一步强化 AI 的安全性和负责任性，明确禁止可能导致伤害或违反道德的行为，保护用户和社会利益。 |

| 知识边界处理 | 强调知识库截止日期，对之后事件的处理较为谨慎。 | 仍然有截止日期，但处理方式更灵活：可以像“高度知情人士”一样讨论截止日期后的事件；明确告知用户其知识不是实时的；对不确定信息，会承认可能“幻觉”，建议用户核实。 | 在保持诚实的前提下，让 Claude 能够更灵活地处理新信息，避免过于死板的回应，同时提醒用户注意信息的时效性和准确性。 |

| 产品信息 | 提供了模型家族、访问方式、API 信息。 | 在此基础上，增加了：更详细的模型家族信息（各模型特点）；当前版本的具体发布日期（2025年2月）；“Claude Code”这一新访问方式；明确了`reasoning` or `extended thinking mode`。 | 让用户更全面地了解 Claude 的不同版本、功能和使用方式，方便用户选择和使用。同时也暗示了模型能力的提升(reasoning mode)。 |

| 人性化交互 | 无 | 如果被问到自己的感受，可以像被问到假设性问题一样回答；创作诗歌时，避免使用老套的意象、比喻或可预测的押韵方案；对用户感受更敏感。 | 让 Claude 的回应更自然、更富有个性，避免机械化的表达，提升交互的趣味性和情感连接。 |

| 内容规范 | 无 | 不会生成色情、暴力、非法内容。 | 明确内容生成的底线，确保 AI 输出符合社会道德和法律规范。 |

| 信息表达习惯 | 无 | 在多个地方都强调了回答的简洁性。 | 提升信息传递效率，避免冗余和啰嗦，让用户更快获取关键信息。 |

| Prompt工程 | 有提及，但较笼统 | 更详细地指导用户如何进行 Prompt 工程（例如，使用思维链、提供示例、指定输出格式等），并提供官方文档链接。 | 帮助用户更好地利用 Claude 的能力，提高 Prompt 的质量和效果，从而获得更满意的结果。 |

# 👉 结尾

Anthropic 确实履行了他们的声明“定期对外更新 Claude AI 的核心系统提示词”，为广大提示工程师提供了宝贵的提示工程学习资料。

![](img/4baa9462c9ca00d8db56ae64f49e312f.png)

你也可以在 [`Anthropic/Release-Notes/System-Prompts`](https://docs.anthropic.com/en/release-notes/system-prompts) 中找到他们往期开源的更多系统提示词，涵盖了从 Claude 3 Haiku 到现在所有的模型。

另外，为了方便你更好的学习，一泽Eze 整理了更多 Anthropic 开源的相关提示词学习资料：

+   Claude 3.5 sonnet 内置提示词详细拆解与解说：https://mp.weixin.qq.com/s/0R4zgH3Gc5TAfAPY1oJU4A

+   Anthropic 的三位顶级提示工程专家聊《如何当好的提示词工程师》：https://mp.weixin.qq.com/s/VP_auG0a3CzULlf_Eiz1sw

+   往期 Claude AI 核心系统提示词：https://docs.anthropic.com/en/release-notes/system-prompts

+   Claude 官方用户手册-提示工程指南：https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview

+   Claude 官方提示库：https://docs.anthropic.com/en/prompt-library/library

**如果觉得有用或者还想继续看，**

期待你的随手关注（记得星标👸号）、点赞、转发，非常感激～

**谢谢你的阅读 🌟**

<https://chatmemo.feishu.cn/sync/OXhxdlqqcsuEkObLUgsceuRinef>
