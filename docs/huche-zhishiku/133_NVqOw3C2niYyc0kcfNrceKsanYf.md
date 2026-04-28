# Roo Code - Office Hours：OpenRouter 团队规模8人

> 来源：[https://huchegames.feishu.cn/docx/XO6MdkZSIoLYIBxQnuOcMQdinfh](https://huchegames.feishu.cn/docx/XO6MdkZSIoLYIBxQnuOcMQdinfh)

**R**oo Code - Office Hours - 2025年4月9日 | 特邀嘉宾 Tovan (来自 OpenRouter)

在本期办公时间中，我们邀请了 OpenRouter 的 Tovan 进行了一场有趣的问答环节。Tovan 回答了社区的问题，并分享了关于 AI 集成、开发者体验以及 AI 工具对软件开发影响的宝贵见解

**开场与嘉宾介绍**

+   主持人（Hanss）欢迎大家，并介绍来自 OpenRouter 的特邀嘉宾 Tovan。

**OpenRouter 介绍**

+   **核心价值：** Tovan 解释 OpenRouter 是一个 LLM 的统一接口，提供更好的价格、更高的在线时间和无需订阅的服务。

+   **为**何选择 OpenRouter 而非直连服务商（如 Anthropic, OpenAI, Google）：

+   **简化设置：** 避免了直接对接各家 API 所需的繁琐设置（如 Google 的计费账户、Anthropic 的速率限制等级协商）。OpenRouter 抽象了这些复杂性。

+   **解决速率限制：** 用户通常难以获得理想的速率限制，需要预存资金、等待、申请。OpenRouter 凭借与上游供应商的良好关系，能代表用户协商，通常让用户获得更高的限制。

+   **避免供应商锁定：** 行业模型迭代迅速（例：GPT-4.1 本周最佳，下周可能是 Gemini 2.5 Pro）。使用 OpenRouter 只需修改一行代码即可切换模型，无需重新设置 API 和担心预存资金无法跨平台使用（如 OpenAI 资金不能用于 Google）。

+   **统一接口：** 一个地方管理账单、速率限制，轻松使用多种 AI。

**关于 OpenRouter 的“S**tealth Models”（隐形模型）

+   **背景：** 通常开发者在模型正式发布前很难接触到 API 进行测试（除了 LMSys/Arena 等有限途径）。

+   OpenRouter 的目标与机遇： 一直希望获得供应商信任，在其平台上试用模型，以便为模型创建者和社区提供更全面的反馈（超越简单聊天室的功能测试）。

+   **合作契机：** OpenAI 主动接触 OpenRouter，提议在其平台上发布 Quasar Alpha 和 Optimus Alpha（GPT-4.1 的早期快照）供社区通过 API 使用。

+   **挑战与执行：**

+   时间紧迫（发布前约两周）。

+   需要保密供应商身份，进行了技术处理（模糊错误信息、修改 ID 等），在约 24 小时内完成。

+   尽管一些技术细节（如 tokenization、tool calling 形状）仍可能被专家识别，但已尽力隐藏。

+   **社区反响：** 非常积极，用户获得了约两周免费的高质量模型 API 访问。

+   **命名：** "Quasar Alpha" 和 "Optimus Alpha" 由 OpenRouter 团队命名。"Quasar" 的灵感部分来自与 GPT-4.5 的头脑风暴；"Optimus" 则是为了区分而选择的另一个名字。

+   **模型对应关系：** Quasar Alpha 是 GPT-4.1 的一个较早快照，Optimus Alpha 是一个较晚的快照。

**未来计划与 Q&A**

+   未来的 Stealth Models： OpenRouter 明确希望继续这种模式，认为这对收集模型反馈非常有价值，并愿意与任何感兴趣的供应商合作。

+   Gemini 2.5 Pro Experimental 速率限制问题：

+   **原因：** 模型性能优越（堪比 Sonnet 3.7，编程/推理能力强），需求远超 Google 预期和提供的容量。

+   **Google 限制：** 每日 25 次请求，每日 100 万 tokens。

+   **OpenRouter 限制：** 凭借合作关系获得更高限制（约 1000 万 tokens/分钟，无日请求上限），但用户需求仍是其容量的 2-5 倍。

+   OpenRouter 额外限制： 为控制流量，增加了 1 请求/分钟 和 80 请求/天 的限制（此限制适用于 AI Studio 供应商端点；Vertex 端点基本无容量）。

+   **$10 充值与免费模型额度：** OpenRouter 此前调整了免费额度政策（为打击滥用）：无余额账户 50 请求/天，有 ≥$10 余额账户 1000 请求/天（跨所有免费模型）。这与 Gemini 2.5 的发布是独立事件。

+   **当前状况：** 即使充值 $10，访问 Gemini 2.5 Pro Experimental 仍可能因底层容量不足而失败（大约 50% 成功率），且失败的请求（429 错误）也会计入每日 80 次的限额。因此，实际成功请求数可能在 40-80 次之间。

+   **解决方案：** 这是 OpenRouter 的高优先级问题，正在积极讨论解决方案（可能限制零余额用户访问、请求 Google 增加容量等），但预计该模型的免费供应不会持续太久。用户也可尝试使用自己的 AI Studio API Key 获取额外免费额度。

+   Gemini 2.5 缓存问题 (Hanss 提及)： Gemini 2.5 的缓存机制似乎需要较大的块才能生效，导致 Hanss 的使用成本增加。

+   **模型输出重复/循环问题：** 提问者遇到 Deep Coder 或 Optimus Alpha 无休止重复输出。Tovan 解释这在小型模型（如 Deep Coder）中更常见，是 token 预测在长输出下可能崩溃的现象。可以通过调整 repetition_penalty 和 frequency_penalty参数（如果提供商支持）来缓解。大型模型（如 Optimus Alpha/GPT-4.1）不太可能出现这种情况，但理论上所有模型都可能发生。

+   OpenRouter 定价模型与大客户折扣：

+   **标准费用：** 每次购买 Credits 收取 5% + $0.35 的固定费用。其中大部分（超过一半）是 Stripe 的处理费。OpenRouter 的实际提成低于 2%。

+   **未来可能调整：** 考虑根据购买金额调整费率（例如，买 $20 是 6%，买 $1000 是 3%），但这依赖于与 Stripe 协商到更好的费率。

+   **大额购买折扣：** 对于大额（通常最低 $10,000）预购 Credits 的企业客户，可以通过 Stripe 平台之外的方式（如 ACH/Wire）进行交易，避免 Stripe 费用，从而获得更低的费率。需要直接联系 OpenRouter (support@openrouter.ai)。

+   OpenRouter 团队规模： 目前 8 人（4 名工程师，CEO, COO, 财务负责人 Tovan），团队正在快速增长。

+   OpenRouter 未来路线图 (Roadmap)：

+   **Gemini 缓存支持：** 即将推出，将采用类似 Anthropic 的 cache_control 断点方式，OpenRouter 会在后端处理 Google 独立的缓存创建 API 调用，缓存有效期约为 5 分钟。

+   **多模态：** 模型页面已增加输入/输出模态信息（文本、图像）。正在开发支持原生图像输出的模型（如 GPT-4o, Gemini 2.0/2.5）。

+   **PDF/文件上传：** 即将推出。一方面支持将文件直接传递给原生支持此功能的上游 API（Anthropic, Google, OpenAI）；另一方面，将为 OpenRouter 上的 所有 模型提供 PDF 解析支持，用户可选择基础的开源 PDF-to-JSON 库或 Mistral 的新 OCR 模型。

+   **Quick Sets：** 允许用户通过可配置的开关集合来定制 API 体验（如按吞吐量/价格排序、过滤量化级别），简化常用设置。

+   **API 健壮性：** 持续改进，确保统一接口能更好地处理不同模型的细微差异（如 OpenAI 新的 responses API 格式、不同的 Tool Calling 响应形状），减少边缘情况。

+   OpenRouter SDK (可能)： 正在初步考虑开发 SDK，以克服当前 OpenAI 兼容格式的一些限制。会征求社区意见。

+   **标准化 /models 端点：** 目标是创建一个包含更丰富、标准化元数据（如上下文长度、量化、定价、视觉能力、Tool Calling 支持等）的模型信息端点。这将极大简化 OpenRouter 及供应商添加新模型、更新信息的操作流程，减少沟通成本和延迟。

+   大型上下文窗口 (如 Llama 4 Scout 10M)：

+   **挑战：** 提供超大上下文窗口计算成本极高，GPU 资源紧张，推理服务利润微薄。

+   **可行性：** 供应商难以在保证服务质量（低延迟、高并发）和合理定价的前提下提供此类服务。目前更可能在有特定需求、不担心成本的企业内部部署（on-prem）。

+   **加密货币支付：** 支持通过 Coinbase Commerce 支付，收取 5% 费用（部分给 Coinbase）。正在研究使用 Coinbase 的新工具或其他方法以降低或消除手续费。

+   与 Roo Code 等应用共享数据：

+   **模型使用情况：** 考虑在 OpenRouter 网站上为每个应用（如 Roo Code）创建公开页面，显示最常用的模型排名（类似全局 LLM 排名）。

+   **隐私：** OpenRouter 默认不记录 Prompt/Completion 内容，用户需主动选择开启日志记录（并获得 1% 折扣）。任何数据共享都会以隐私为先，进行匿名化处理。目前存储的数据未用于训练模型。

+   优先队列/Premium Tokens： 目前没有计划。OpenRouter 的目标是通过增加供应商和优化路由来确保基础服务的容量和性能，而不是创建付费优先通道。

+   Tool Calling 效果 (Adam 提问)： 使用原生 Tool Calling 参数 vs. 在 Prompt 中描述工具？Tovan 认为目前没有明确数据对比，直觉上对于现代模型，在 Prompt 中清晰描述结构化输出可能效果更好，但这正在快速变化（如 GPT-4.1 改进了原生 Tool Calling）。

+   与 Requesty 等竞品对比 (Michael 提问)： Requesty 正在做 Prompt 拦截/重写。OpenRouter 对此持谨慎态度，认为可能不符合所有用户期望，且效果难以保证，更倾向于用户控制和透明度。

+   与 Hugging Face 的合作 (Michael 提问)： 现有的模型接口是早期合作遗留的，Hugging Face 已转向与其他供应商合作提供推理服务。OpenRouter 可能会停止提供该旧接口（因成本问题）。但双方仍保持良好关系，HF 对 OpenRouter 获取开源模型信息很重要。

+   **托**管 MCPs (Multi-modal Cognitive Processors) (Adam 提问)： OpenRouter 内部讨论过，目前认为没有必要自己托管 MCP 服务器，社区已有许多优秀项目在做。MCPs 可以通过 Tool Calling 在 OpenRouter 上使用。

+   记忆 (Memory) 功能 (Adam 提问)： Tovan 认为跨模型、可移植的长期记忆非常有价值，符合 OpenRouter 避免供应商锁定的理念。这绝对是 OpenRouter 感兴趣的方向，但实现方式、时间、定价都待定。

+   **多模型同时调用/反馈端点：** 目前 OpenAI 兼容的 API 格式不支持一次请求返回多个不同模型的结果（除了已弃用的 n 参数）。未来若支持如 OpenAI responses API 的数组格式输出，理论上可行。更可能的方向是创建一个“反馈 API”，用户提交 Prompt，获得多个模型的回应，通过投票选择最佳回应，以此收集数据（用户可能因提供反馈而获得免费额度）。

Roo Code 内部讨论与更新

+   近期 UI 改进 (InElement 贡献)： 在编辑面板底部增加了 Auto Approve 等操作的小图标，方便用户操作。

+   **社区贡献的重要性：** Roo Code 的发展非常依赖社区贡献，鼓励大家参与（如修复 bug、改进 UI、提出 PR）。

+   **Roo Code 职位机会：** 正在寻找新的团队成员。方式是鼓励潜在成员先通过贡献 PR 来展示能力和对项目的热情。工作需要高度的适应性和跨领域能力。

+   Boomerang 任务模式 (Hanss 演示与解释)：

+   **目的：** 解决长任务中上下文窗口被填满、性能下降的问题。

+   **机制：** 父任务 (Parent Task) 扮演“协调者”，将复杂任务分解为子任务 (Subtasks)，并将子任务分派给专门的“模式”(Modes) 去执行。子任务完成后，只将关键结果（而非全部上下文）汇报给父任务。父任务根据结果继续规划和分派下一个子任务。这样可以保持父任务的上下文干净、聚焦。

+   **演示：** Hanss 尝试用 Boomerang 模式（父任务用 GPT-4.1，子任务用 Sonnet 3.7）创建一个 Next.js 应用，实现 bouncing logo 效果。演示中遇到一些问题（如端口冲突、未按预期工作），展示了该模式仍在开发和调试中。

+   **未来：** 计划发布 Boomerang Tasks 1.5 或 2.0，包含重大改进，预计在 1-2 周内（非官方承诺）。需要社区帮助修复边缘 Bug。

+   **社区模式/扩展的复杂性：** 承认社区开发的各种高级模式（Roo Commander, Rooflow, Spark Taskmaster 等）功能强大但上手门槛高。Roo Code 计划将类似的高级编排功能集成到核心产品中，但需要严格的内部测试 (Eval)、文档编写和持续维护资源。目前因人力限制尚未正式推出。

+   **推荐探索社区项目：** 对于想尝试最前沿、实验性编排功能的用户，推荐加入相关社区项目的 Discord（如 Jez 的 Rue Commander 服务器）。

+   Rooflow 项目需要贡献者。

+   **代码索引/RAG 支持：** 是的，有活跃的 PR 正在进行中，旨在通过代码索引减少 Token 消耗，补充现有的基于 Tree-sitter 的文件摘要功能（后者受限于 Tree-sitter 支持的语言）。

+   **代码库结构与模块化：** 目前 Roo Code 代码库相对“单体 (monolithic)”，存在一些耦合。社区（如 JetBrains 插件团队、CTE）和内部都在进行一些模块化的工作和尝试（如改进路径引用、考虑 monorepo），但这需要时间和基础架构的稳定。

+   本地 LLM 推荐 (Adam)： 对于需要反复修改代码的场景（非一次性生成），推荐使用能运行的最大版本的 Quinn Coder 2.5。Mac 用户可尝试 MLX 版本，其他平台可开启 KV 缓存加速。本地模型目前在代码生成质量上仍不如顶尖的托管模型。

+   使用 GitHub Copilot 理解 Roo Code 代码库： Adam 推荐使用 Copilot Chat (免费版即可) 来询问关于 Roo Code 代码库的问题（如某功能如何实现、某库在哪里使用），可以快速定位代码并获得解释，有助于新开发者上手。

+   **Roo Code 资金来源：** 由风险投资 (VC) 支持。目前没有向社区用户收费的计划，未来的商业化重点是面向企业的支持服务。

+   **Roo Code 与其他 Klein For**k (如 Kylo Code) 的关系： 承认基于同一开源基础，但各有侧重和发展方向。Roo Code 的特色在于更深入地拥抱和依赖社区贡献。认为市场足够大，不同项目可以共存并服务不同用户或企业。

原视频：Roo Code - Office Hours - 2025年4月9日