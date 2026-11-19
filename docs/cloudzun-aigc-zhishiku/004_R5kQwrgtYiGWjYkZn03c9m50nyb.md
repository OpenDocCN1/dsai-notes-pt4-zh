
# 04-DeepSeek API 的获取和使用

> 来源：[https://zsyhjtnsa5.feishu.cn/docx/L8IGdW26AoC5iBxjHPQcLXNGnag](https://zsyhjtnsa5.feishu.cn/docx/L8IGdW26AoC5iBxjHPQcLXNGnag)

## 语言模型 API 与 DeepSeek

### 大语言模型（LLM）与 API 的关系：一座便捷的桥梁

我们知道，大语言模型（LLM）是经过海量数据训练、拥有强大自然语言理解和生成能力的复杂人工智能系统。它们能够执行写文章、回答问题、翻译、写代码等多种任务。然而，这些强大的模型通常具备以下特点：

+ **巨大的计算资源需求：** 训练和运行（推理）LLM 需要高性能的计算集群（尤其是 GPU）和大量的内存，这对于个人开发者或许多企业来说是一笔巨大的开销。

+ **部署与维护的复杂性：** 搭建稳定、高效的 LLM 服务环境，并持续进行维护、更新和扩展，需要专业的技术知识。

**那么**，如何让开发者和应用程序能够轻松地利用 LLM 的能力，而无需直接承担上述的复杂性和成本呢？答案就是 `API`（Application Programming Interface，应用程序编程接口）。

`API` 在这里扮演的角色，可以理解为：

1. **一个标准化的“插座”：** `API` 定义了一套规则和协议，允许你的应用程序（客户端）与托管在云端的 LLM 服务（服务端）进行通信。就像你不需要了解发电厂的运作细节，只需要将电器插头插入符合标准的插座就能用电一样。

1. **能力的“传送带”：** 你通过 `API` 将你的请求（比如一个问题或一段指令文本）发送给 LLM 服务。LLM 服务处理这个请求，并将模型生成的结果（比如答案或生成的文本）通过 `API` 返回给你的应用程序。

1. **复杂性的“封装器”：** `API` 将底层复杂的模型推理、硬件管理、负载均衡等细节隐藏起来。你只需要关注如何构造请求和处理响应，大大降低了使用 LLM 的门槛。

**总结来说，`LLM API` 就是将大语言模型的强大能力封装成一项易于访问的网络服务。开发者通过调用这个 `API`，就能像调用一个普通的函数或服务一样，将 LLM 集成到自己的网站、软件或工作流中，而无需关心模型本身的部署和运维。**

### LLM API 工作原理、调用方式与通用适用场景

#### 工作原理 （基本流程）

1. **请求 （`Request`）：** 你的应用程序通过互联网向指定的 `API` 端点（一个 `URL` 地址）发送一个 `HTTP` 请求。这个请求通常包含：

   + **身份验证信息：** 如 `API Key`，证明你有权使用该服务。

   + **指定模型：** 告诉服务你想使用哪个具体的 `LLM`（例如 `DeepSeek` 的某个版本）。

   + **输入内容：** 你想让模型处理的文本，通常称为“提示”（`Prompt`）或“消息”（`Messages`，用于对话场景）。

   + **（可选） 配置参数：** 如 `temperature` （控制创造性），`max_tokens` （限制输出长度），`stream` （是否流式输出） 等，用于调整模型的行为。

1. **处理 （`Processing`）：** `API` 服务接收到请求后，验证身份，将输入内容和参数传递给对应的 `LLM` 进行推理计算。

1. **响应 （`Response`）：** `LLM` 生成结果后，`API` 服务将其打包成标准格式（通常是 `JSON`），通过 `HTTP` 响应返回给你的应用程序。响应内容通常包括：

   + **生成文本：** 模型的主要输出。

   + **使用量信息：** 如消耗的 `Token` 数量，用于计费。

   + **（可选） 其他元数据或错误信息。**

#### 流式输出 （`Streaming`）

对于实时性要求高的场景（如聊天机器人），`API` 通常支持流式输出。模型会边生成边将结果分块发送回来，而不是等待全部生成完毕再返回，带来更流畅的体验。

#### 调用方式

1. **直接 `HTTP` 请求：** 使用编程语言的网络库（如 `Python` 的 `requests` 库）或命令行工具（如 `curl`）直接构造和发送 `HTTP` 请求。

1. **官方/社区 `SDK`（`Software Development Kit`）：** 服务提供商（如 `DeepSeek`、`OpenAI` 等）或社区通常会提供针对特定编程语言（如 `Python`，`JavaScript`）的 `SDK`。这些 `SDK` 封装了底层的 `HTTP` 调用细节，提供了更简洁、易用的函数接口来调用 `API`。这是推荐的方式，尤其是对于初学者。

#### 通用适用场景

`LLM API` 的应用非常广泛，几乎涵盖了所有需要自然语言处理能力的领域：

+ **内容创作：** 自动撰写文章、博客、广告文案、邮件、报告等。

+ **智能客服与聊天机器人：** 提供 7x24 小时的自动问答和客户支持。

+ **文本摘要：** 快速提炼长篇文章或文档的核心内容。

+ **机器翻译：** 在多种语言之间进行高质量互译。

+ **情感分析：** 判断文本中表达的情绪（正面、负面、中性）。

+ **代码生成与解释：** 根据自然语言描述生成代码片段，或解释现有代码的功能。

+ **信息提取：** 从非结构化文本中抽取关键信息（如姓名、日期、地点）。

+ **教育辅导：** 个性化的学习问答、作文批改建议。

+ **创意工具：** 辅助写诗、写歌词、构思故事等。

## API 调用 vs 本地部署：优劣势对比与选择考量

选择使用云端的 `LLM API` 还是在自己的硬件上本地部署模型，取决于你的具体需求、资源和限制。

### 如何选择？

+ **优先考虑云 `API` 的情况：**

  + 快速原型开发和验证想法。

  + 项目初期，使用量不确定。

  + 需要使用最大、最先进的模型。

  + 缺乏自建和维护高性能计算环境的资源或意愿。

  + 对数据隐私要求不高，或可通过协议保障。

+ **优先考虑本地部署的情况：**

  + 处理高度敏感的数据，要求数据绝对不出内网。

  + 应用场景需要极低延迟或离线运行。

  + 有持续、可预测的大量调用需求（长期成本可能更低）。

  + 需要对模型进行深度定制或微调。

  + 拥有相应的硬件资源和技术运维能力。

**在实际应用中，混合使用（部分场景用 `API`，部分场景本地部署）也是常见的策略。**

## DeepSeek 模型家族介绍：特点、优势与版本概览

**`DeepSeek` 是由 `DeepSeek AI` 公司开发的一系列大语言模型。** 它们在业界以其强大的代码生成和数学推理能力而闻名，同时在通用语言任务上也表现出色。

### 主要特点与优势：

+ **卓越的代码能力：** `DeepSeek Coder` 系列模型在多种编程语言的代码补全、生成、解释和修复方面表现突出，是许多开发者的得力助手。

+ **强大的通用语言与推理能力：** `DeepSeek LLM` 系列模型不仅在标准语言理解和生成基准上取得优异成绩，还在数学、逻辑推理等需要深度思考的任务上展现实力。

+ **开放与闭源结合：** `DeepSeek` 提供了可通过 `API` 访问的商业模型，同时也开源了部分模型（如一些 `Coder` 和 `LLM` 版本），供研究和本地部署使用，给予用户更多选择。

+ **多种模型规模：** 提供了从数十亿（`Billion`）参数到数百亿参数的不同规模模型，满足从资源受限环境到追求极致性能的不同需求。

+ **具有竞争力的性能与成本：** `DeepSeek API` 通常提供有吸引力的定价策略，包括一定的免费额度，使其在性能和成本效益上具有竞争力。

### 版本概览 （示例，具体型号可能随时间更新）:

### 关键说明

1. **参数与硬件需求关系**：

   + 每个参数约占用 4 字节内存，例如 `70B` 模型需要约 `28GB` 显存（`70B×4B`），但实际运行需更高显存以支持推理优化。

   + 满血版（`671B`）通常需云端计算集群，本地部署仅限高端研究机构。

1. **经济型替代方案**：

   + 若预算有限，可考虑 **蒸馏版**（如 `DeepSeek-R1-Distill-Qwen-7B`），降低显存需求。

   + 国产显卡（如摩尔纪元 `48GB`、华为昇腾 `96GB`）适配 `DeepSeek`，性价比更高。

1. **性能对比**：

   + **`7B` vs. `8B`**：`8B` 生成质量提升约 20%，但计算能力仅增 14%。

   + **`32B` 及以上**：适合长文本理解、专业领域分析，但成本陡增。

## 获取 DeepSeek 云 API 访问权限

**引言：** 在了解了 `API` 的基本概念后，这一部分我们将进入实践环节：如何真正获得调用 `DeepSeek` 云 `API` 的“钥匙”—`API Key`。我们将介绍几个提供 `DeepSeek API` 服务的主流平台，并指导您完成账号注册、认证，以及最终获取 `API Key` 的步骤。同时，我们还会重点讨论各平台的计费方式和 `API Key` 的安全管理，这对于控制成本和保障账户安全至关重要。

### API 提供平台概览

从 `DeepSeek` 官方开放平台、硅基流动（`SiliconFlow`） 和 火山引擎（`Volcano Engine`） 在 `DeepSeek V3`/`R1` `API` 服务 上的差异来看，三者在服务模式、功能支持、部署方式等方面存在明显区别：

1. **服务模式**

1. **功能支持**

1. **部署方式**

1. **免费与付费**

**小结**

+ **`DeepSeek` 官方**：适合需要最新功能的开发者，但稳定性较差。

+ **硅基流动**：适合企业 **私有化部署**，提供 **专属算力** 和 **高级 `API` 功能**（如 `Function Calling`）。

+ **火山引擎**：适合 **高并发、低延迟** 需求的企业，支持 **云上优化部署** 和 **`API` 高性能调用**。

如需更详细的技术参数，可参考各平台官方文档或评测报告。

### 各平台账号注册、认证流程简介

无论选择哪个平台，获取 `API Key` 的前提通常是拥有该平台的有效账号，并可能需要完成实名认证。基本流程大同小异：

1. **访问平台官网：** 前往您选择的平台官方网站（如 `platform.deepseek.com`，硅基流动官网，`volcengine.com`）。

1. **注册账号：** 点击“注册”按钮，通常需要使用手机号或邮箱进行注册。

1. **验证身份：** 按照提示完成短信或邮件验证码的验证。

1. **登录账号：** 使用注册的账号信息登录平台。

1. **实名认证 （通常需要）:** 根据国家相关法律法规和平台要求，使用 `API` 服务前往往需要进行实名认证。这通常涉及提供姓名、身份证号码等信息，有时可能需要人脸识别或绑定已实名认证的支付方式。请按照各平台的具体指引完成此步骤。未完成实名认证可能无法创建或使用 `API Key`。

### 如何在平台申请 DeepSeek API Key

获取 `API Key` 的具体操作界面因平台而异，但核心逻辑相似。以下是通用步骤指南：

#### 在 `DeepSeek` 官方开放平台 （`platform.deepseek.com`）

1. 登录您的 `DeepSeek` 账号。

1. 导航至用户控制台或仪表板 （`Dashboard`）。

   ![](img/f58696e6bbccdb880daed9ba17b44159.png)

1. 在左侧菜单或顶部导航栏中寻找“`API 密钥`” （`API Keys`） 或类似的选项。

   ![](img/a1a7562264d7fed77f195dcc4b25df5b.png)

1. 点击“创建新密钥” （`Create New Key`） 按钮。

1. **（可选）** 为您的密钥命名，以便区分用途（例如 `my-test-app-key`）。

   ![](img/dfa95b9385857818b26056215233cda8.png)

1. **系统会生成一个 `API Key`**（通常是一长串字符）。请立即复制并妥善保存此密钥！ 出于安全考虑，该密钥可能只显示一次，关闭窗口后将无法再次查看完整密钥。

   ![](img/960eb2b6af3b5d7a52c3b65e206c8ee4.png)

1. 您可以在密钥管理界面看到已创建的密钥列表、创建时间等信息，并可以随时停用或删除不再需要的密钥。

#### 在 硅基流动 （`SiliconFlow` `https://siliconflow.cn/zh-cn/`）

1. 登录您的硅基流动账号。

1. 导航至其模型市场、`API` 服务或开发者中心等相关区域。

1. 找到 `DeepSeek` 模型相关的服务页面。

1. 寻找“`API 管理`”、“凭证管理”或“获取 `API Key`”等入口。

1. 按照平台的指引创建新的 `API Key`。可能需要指定密钥用途或关联到特定服务。

1. 同样，立即复制并安全地保存生成的 `API Key`。

1. 熟悉平台提供的密钥管理功能（查看、禁用、删除）。

#### 在 火山引擎 （`VolcEngine` `https://www.volcengine.com/`）

1. 登录您的火山引擎控制台。

1. 在服务列表中，找到“人工智能服务” -> “大模型服务平台（`Maas`）” 或类似路径，进入相关服务。

1. 可能需要先“开通服务”或“订阅” `DeepSeek` 相关的模型产品。

1. 寻找 `API` 访问凭证的管理入口。这可能在模型服务的设置中，或者需要通过火山引擎统一的“访问控制（`IAM`）”服务来创建。

1. 创建访问密钥 （`Access Key`）。火山引擎通常会**生成一对密钥：`Access Key ID` （`AK`） 和 `Secret Access Key` （`SK`）**。`AK` 是公开的标识，`SK` 是私密的密钥，两者都需要妥善保管，尤其是 `SK`，绝不能泄露！

1. 立即复制并安全地保存 `AK` 和 `SK`。`SK` 通常也只显示一次。

1. 了解如何在火山引擎的 `IAM` 中管理（查看、禁用、轮换、删除）您的访问密钥。

**重要提示：** 各平台的界面和术语可能随时更新，请以实际操作界面和官方文档为准。如果在获取过程中遇到困难，请查阅对应平台的帮助文档或联系客服支持。

### API 定价与计费模型详解（免费额度、付费标准）

**核心原则：按量付费 （`Pay-as-you-go`），基于 `Token` 计算。**

#### Token 是什么？

+ `Token` 是模型处理文本的基本单位。对于英文，1 个 `token` 通常对应几个字母或一个单词；对于中文，1 个 `token` 通常对应 1 个或多个汉字。

+ 模型处理的 输入文本 （`Prompt`/`Messages`） 和 输出文本 （`Completion`/`Response`） 都会被计算 `Token` 数。

#### 计费模型：

+ 平台通常会为不同的 `DeepSeek` 模型设定不同的价格。

+ 价格通常以“元/千 `Token`”或“元/百万 `Token`”为单位。

+ 输入 `Token` 和输出 `Token` 的价格可能不同！ 通常输出 `Token` 的价格会略高于输入 `Token`。

#### 计费公式 （简化）:

总费用 = (输入 `Token` 数 * 输入单价) + (输出 `Token` 数 * 输出单价)

#### 免费额度：

+ **许多平台（尤其是官方平台）为了吸引用户，会提供 一次性或每月 的免费 `API` 调用额度（例如，免费的 `Token` 数量或等值的金额）。**

+ **这是开始体验和开发的好机会！** 但请注意免费额度的有效期和限制。超出免费额度的部分将开始计费。

#### 如何查看具体价格？

+ **必须访问各平台的官方定价页面！** 价格信息可能会调整。

+ **`DeepSeek` 官方：** `https://api-docs.deepseek.com/quick_start/token_usage` （或类似路径）

下表所列模型价格以“百万 `tokens`”为单位。`Token` 是模型用来表示自然语言文本的的最小单位，可以是一个词、一个数字或一个标点符号等。我们将根据模型输入和输出的总 `token` 数进行计量计费。

#### 模型 & 价格细节

1. `deepseek-chat` 模型对应 **`DeepSeek-V3`**；`deepseek-reasoner` 模型对应 `DeepSeek-R1`。

1. **思维链**为 [`deepseek-reasoner`](https://api-docs.deepseek.com/zh-cn/guides/reasoning_model) 模型在给出正式回答之前的思考过程，其原理详见推理模型。

1. 如未指定 `max_tokens`，默认最大输出长度为 `4K`。请调整 `max_tokens` 以支持更长的输出。

1. 关于上下文缓存的细节，请参考 [`DeepSeek` 硬盘缓存](https://api-docs.deepseek.com/zh-cn/news/news0802)。

1. `deepseek-reasoner` 的输出 `token` 数包含了思维链和最终答案的所有 `token`，其计价相同。

1. **`DeepSeek API` 现实行错峰优惠定价，每日优惠时段为北京时间 `00:30-08:30`，其余时间按照标准价格计费。请求的计价时间为该请求完成的时间。**

+ **硅基流动：** 请在其官网查找“定价”、“计费”或模型服务相关页面。

+ **火山引擎：** 请在其官网查找“定价”中心，并定位到“大模型服务（`Maas`）”或相关产品的价格详情页。

+ 仔细阅读不同模型的输入/输出定价、免费额度规则和计费周期（通常是按月结算）。

当前时间 （2025 年 4 月 3 日）: 请务必在上述官方渠道核实最新的定价信息，本课件不保证价格的实时准确性。

+ **使用量监控：** 各平台都提供控制台，让您可以实时或定期查看 `API` 调用次数、`Token` 消耗量以及产生的费用，便于成本管理和预测。

## API 密钥的安全管理最佳实践



`API Key` 就如同您账户的密码，甚至可能直接关联到您的支付方式，一旦泄露，可能导致账户被盗用、产生非预期的费用，甚至数据泄露。务必高度重视其安全！

+   **视为机密：** 绝对不要将 `API Key` 分享给不信任的人或在公共场合（如论坛、社交媒体）展示。

+   **不要硬编码在代码中：**

+   **绝对禁止**将 `API Key` 直接写在源代码文件里，尤其是那些会被提交到代码仓库（如 `GitHub`）或部署到客户端的代码中。

+   **推荐做法：** 使用 `Environment Variables` 或安全的 配置文件 来存储 `API Key`。应用程序在运行时从这些安全的位置读取密钥。

+   对于需要分发的软件，应要求用户自行配置他们的 `API Key`。

+   **不在不安全的渠道传输：** 避免通过邮件、即时消息等不加密或不安全的渠道发送 `API Key`。

+   权限最小化原则 （如平台支持）: 如果平台允许，为不同的应用或环境（开发、测试、生产）创建独立的 `API Key`，并只授予所需的最小权限（例如，限制可调用的模型、设置调用频率限制、绑定 `IP 地址` 等）。

+   **定期轮换：** 养成定期更换 `API Key` 的习惯（例如每隔几个月更换一次），即使没有发生泄露，也能降低潜在风险。

+   **监控与审计：** 定期检查 `API` 调用日志和账单，留意是否有异常活动。如果发现可疑调用，立即禁用相关 `API Key` 并进行调查。

+   **及时撤销：** 对于不再使用的应用或怀疑已泄露的 `API Key`，应立即在平台控制台中将其禁用或删除。

+   **使用官方 `SDK`:** 官方 `SDK` 通常会指导更安全的认证方式，而不是让您手动处理原始密钥。

遵守这些安全实践，能够最大限度地保护您的账户和资源安全。

# 使用 DeepSeek 云 API 进行开发

**引言：** 理论学习之后，是时候卷起袖子开始编码了！在本部分，您将学习如何构造发送给 `DeepSeek API` 的请求、理解关键参数的含义、使用 Python 编写代码来实际调用 `API`（包括标准模式和流式模式），并学会如何解析返回的结果以及处理可能出现的错误。最后，我们会通过一个简单的小例子，将所学知识应用起来。

## API 调用基础：HTTP 请求结构与认证

从本质上讲，调用 `DeepSeek API` 就是向其服务器发送一个精心构造的 `HTTP` 请求。了解其基本结构有助于理解 `API` 的工作方式。

+   **HTTP 请求方法：** `DeepSeek` 的聊天补全 `API` 通常使用 `POST` 方法，因为我们需要向服务器发送包含输入信息的数据体（`Request Body`）。

+   **`URL`（`Endpoint`）:** 这是您请求的目标地址。对于 `DeepSeek` 官方平台的聊天补全功能，通常的端点是：

`https://api.deepseek.com/v1/chat/completions`

+   `https://api.deepseek.com`： 这是基础 `URL` （`Base URL`）。

+   `/v1`： 表示 `API` 的版本号。

+   `/chat/completions`： 指向具体的聊天补全功能。

+   `HTTP Headers`（请求头）: `Headers` 包含了关于请求的元数据，其中最重要的两个是：

1.  **`Authorization`**（认证）: 用于验证您的身份。`DeepSeek API` 使用 `Bearer Token` 认证方式。您需要将第二部分获取的 `API Key` 放在这里。

+   格式： `Authorization: Bearer YOUR_DEEPSEEK_API_KEY`

+   **务必将 `YOUR_DEEPSEEK_API_KEY` 替换为您自己的密钥。

1.  **`Content-Type`**（内容类型）: 告知服务器您发送的数据体是什么格式。对于 `DeepSeek API`，通常是 `JSON` 格式。

+   格式： `Content-Type: application/json`

+   `HTTP Body`（请求体）: 这是 `POST` 请求的核心部分，包含了您希望模型处理的信息和相关的配置参数，以 `JSON` 格式组织。例如：

```
{
  "model": "deepseek-chat",
  "messages": [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "Hello!"}
  ],
  "temperature": 0.7,
  "max_tokens": 100,
  "stream": false
}
```

我们将在下一节详细解释这些参数。

**总结：** 调用 `API` 就是使用 `POST` 方法，向正确的 `URL` 发送一个包含 `Authorization` 和 `Content-Type` 的 `Headers`，以及一个包含模型、输入和参数的 `JSON Body`。

## 核心 API 参数详解

在请求体 （`Body`） 中，您可以指定多个参数来控制模型的行为：

+   **`model`**（必需）: 指定您想要使用的模型名称。

+   示例： `deepseek-chat` （官方推荐的通用聊天模型，会指向当前最佳版本） 或 `deepseek-coder` （代码模型）。也可以指定更具体的版本，如 `deepseek-llm-67b-chat` （如果平台支持直接调用）。

+   **`messages`**（必需）: 包含对话历史或指令的列表。这是一个对象数组，每个对象代表对话中的一条消息，包含两个关键字段：

+   `role`： 消息发送者的角色。常见的有：

+   `system`： 用于设定助手的行为或背景信息（可选，但推荐）。

+   `user`： 代表最终用户的输入。

+   `assistant`： 代表模型之前的回复（用于多轮对话）。

+   `content`： 消息的具体文本内容。

+   示例：

```
"messages": [
  {"role": "system", "content": "你是一个乐于助人的AI助手。"},
  {"role": "user", "content": "你好，请介绍一下你自己。"}
]
```

+   **`temperature`**（可选， 默认值通常为 0.7 或 1.0）: 控制输出的随机性。

+   值越高 （例如 1.5），输出越随机、越有创意，但可能不连贯。

+   值越低 （例如 0.2），输出越确定性、越集中，更适合需要精确回答的场景。

+   取值范围通常在 0 到 2 之间。

+   **`max_tokens`**（`可选`）: 限制模型生成的最大 `Token` 数量。这有助于控制输出长度和 `API` 成本。注意，这个限制是针对 生成内容 的，不包括输入 `messages` 的 `Token` 数。

+   **`stream`**（可选， 默认值通常为 `false`）: 控制是否使用流式输出。

+   `false`： 服务器会在模型完全生成所有内容后，一次性返回整个响应。

+   `true`： 服务器会边生成边将内容分块（`chunk`）通过 `Server-Sent Events` （`SSE`） 协议发送回来，可以实现打字机效果，提升实时交互体验。

+   **其他可选参数 （简介）:**

+   `top_p`： 一种替代 `temperature` 的采样策略，控制选择 `Token` 的范围。

+   `frequency_penalty`： 惩罚重复出现的 `Token`，降低重复度。

+   `presence_penalty`： 惩罚新出现的 `Token`，鼓励使用已出现过的 `Token`。

+   `stop`： 指定一个或多个字符串，当模型生成这些字符串时停止输出。

根据您的需求组合这些参数，可以精细地控制模型的输出行为。

## 编程实践

### 使用 Python 和 `openai` 库调用 `DeepSeek API`

现在我们将动手实践，学习如何使用 Python 和推荐的 `openai` 库来调用 `DeepSeek API`。这个库的接口设计与 `OpenAI API` 兼容，使得调用 `DeepSeek API` 变得非常方便。我们将分步骤展示基本调用、流式调用和构建一个简单的交互式应用。

**前提条件：**

+   **安装 `openai` 库**： 如果您还没有安装，请执行 `pip install openai`。

+   **获取 `API Key`:** 您需要拥有在第二部分获取的 `DeepSeek API Key`。

+   **安全设置 `API Key`:** **强烈建议**将您的 `API Key` 设置为环境变量 `DEEPSEEK_API_KEY`，而不是直接写在代码里。

+   Linux/macOS: `export DEEPSEEK_API_KEY='sk-xxxxxxxxxxxxxxxxxxxx'`

+   Windows (CMD): `set DEEPSEEK_API_KEY=sk-xxxxxxxxxxxxxxxxxxxx`

+   Windows (PowerShell): `$env:DEEPSEEK_API_KEY='sk-xxxxxxxxxxxxxxxxxxxx'`

或者，您可以将其存储在安全的配置文件中，并在代码中读取。切勿将 `API Key` 提交到公共代码仓库 （如 `GitHub`）！

**步骤 1: 基本的非流式调用**

这是最简单的调用方式，发送请求后等待服务器完成全部生成，然后一次性返回结果。

```
import os
from openai import OpenAI

# --- 安全地获取 API Key ---
# 推荐从环境变量读取
api_key = os.getenv("DEEPSEEK_API_KEY")
if not api_key:
    raise ValueError("请设置环境变量 DEEPSEEK_API_KEY")

# --- 初始化 OpenAI 客户端，指向 DeepSeek API ---
client = OpenAI(
    api_key=api_key,
    base_url="https://api.deepseek.com/v1" # 使用 DeepSeek 的 Base URL
    # 注意: v1 不可省略, https 也不可省略
)

print("--- 步骤 1: 基本非流式调用 ---")
try:
    # --- 发起聊天补全请求 ---
    response = client.chat.completions.create(
        model="deepseek-chat",  # 使用 DeepSeek 的模型
        messages=[
            {"role": "system", "content": "你是一个乐于助人的AI助手"},
            {"role": "user", "content": "你好！请解释一下什么是API？"},
        ],
        stream=False, # 设置为 False 或省略 (默认为 False)
        max_tokens=500, # 可选：限制最大输出长度
        temperature=0.7   # 可选：控制随机性
    )

    # --- 打印模型回复 ---
    assistant_message = response.choices[0].message.content
    print("模型回复:\n", assistant_message)

    # --- (可选) 打印 Token 使用情况 ---
    # 注意：只有在非流式模式下，才能直接从response对象获取完整的usage信息
    usage = response.usage
    if usage:
        print("\nToken 使用情况:")
        print(f"  提示 Token 数: {usage.prompt_tokens}")
        print(f"  完成 Token 数: {usage.completion_tokens}")
        print(f"  总 Token 数: {usage.total_tokens}")

except Exception as e:
    print(f"API 调用出错: {e}")
```

**代码解释：**

1.  导入 `os` 和 `OpenAI`。

1.  安全地从环境变量获取 `API Key`。

1.  创建 `OpenAI` 客户端实例，关键在于：

+   `api_key`： 传入您的 `DeepSeek Key`。

+   `base_url`： **必须**设置为 `DeepSeek` 的 `API` 地址 `https://api.deepseek.com/v1`。

1.  调用 `client.chat.completions.create()` 方法发起请求。

1.  `model`： 指定 `deepseek-chat`。

1.  `messages`： 提供对话上下文。

1.  `stream=False`： 明确指定非流式。

1.  `response.choices[0].message.content`： 从返回的 `response` 对象中提取模型生成的内容。

1.  `response.usage`： 在非流式模式下，可以获取 `Token` 使用统计。

1.  使用 `try...except` 块捕获潜在的 `API` 调用错误。

**步骤 2: 启用流式会话**

流式调用可以更快地看到模型输出的第一个字，带来类似打字机的效果，提升交互体验。



# 步骤 2: 流式调用

```python
import os
from openai import OpenAI

# --- (复用之前的 API Key 获取和 client 初始化代码) ---
api_key = os.getenv("DEEPSEEK_API_KEY")
if not api_key:
    raise ValueError("请设置环境变量 DEEPSEEK_API_KEY")
client = OpenAI(api_key=api_key, base_url="https://api.deepseek.com/v1")


print("\n\n--- 步骤 2: 流式调用 ---")
try:
    # --- 发起流式聊天补全请求 ---
    stream_response = client.chat.completions.create(
        model="deepseek-chat",
        messages=[
            {"role": "system", "content": "你是一位知识渊博的历史学家。"},
            {"role": "user", "content": "简要介绍一下丝绸之路。"}
        ],
        stream=True, # 设置为 True 以启用流式输出
        max_tokens=500
    )

    print("模型回复 (流式):")
    full_response_content = ""
    # --- 迭代处理返回的流数据块 ---
    for chunk in stream_response:
        # 检查 chunk 是否有效且包含内容
        if chunk.choices and chunk.choices[0].delta and chunk.choices[0].delta.content:
            content_part = chunk.choices[0].delta.content
            print(content_part, end="", flush=True) # 实时打印内容块
            full_response_content += content_part # 拼接完整内容

        # 检查流是否结束 (可选：可以通过 finish_reason 判断)
        if chunk.choices and chunk.choices[0].finish_reason:
            finish_reason = chunk.choices[0].finish_reason
            print(f"\n[流结束原因: {finish_reason}]")
            # 注意：流式模式下，通过标准迭代器获取精确的 token usage 可能比较困难
            # 有些实现可能在最后一个 chunk 包含 usage，但这并非标准保证
            # 如果需要精确 usage，建议使用非流式调用或查看平台文档是否有特殊说明

    print("\n--- 流式输出结束 ---")
    # print("\n拼接后的完整回复:\n", full_response_content) # 可选

except Exception as e:
    print(f"\nAPI 调用出错: {e}")
```

## 代码解释

1. 关键在于设置 `stream=True`。
1. 调用 `create` 方法会返回一个生成器（`stream_response`）。
1. 使用 `for` 循环迭代这个生成器。每次迭代会收到一个数据块（`chunk`）。
1. `chunk.choices[0].delta.content` 包含了当前数据块中的文本片段（增量内容）。
1. 需要检查 `chunk.choices` 和 `delta.content` 是否存在且不为 `None`，因为流的开始和结束可能会有不含内容的 `chunk`。
1. 使用 `print(..., end="", flush=True)` 实现实时打印，`flush=True` 确保内容立即显示。
1. 可以根据 `chunk.choices[0].finish_reason` 判断流结束的原因（如 `stop` 或 `length`）。
1. 在流式模式下，直接获取准确的 `usage` 信息比较复杂，通常建议使用非流式调用来获取。

# 步骤 3: 实现简单的交互式应答

这个示例演示了如何维护对话历史，实现多轮对话。

```python
import os
from openai import OpenAI

# --- (复用之前的 API Key 获取和 client 初始化代码) ---
api_key = os.getenv("DEEPSEEK_API_KEY")
if not api_key:
    raise ValueError("请设置环境变量 DEEPSEEK_API_KEY")
client = OpenAI(api_key=api_key, base_url="https://api.deepseek.com/v1")


print("\n\n--- 步骤 3: 交互式对话 (输入 'quit' 退出) ---")

# --- 初始化对话历史 ---
# 可以包含一个 system message 来设定助手的角色
history = [
    {"role": "system", "content": "你是一个简洁而友好的聊天机器人。"},
    # 可以预设一个开场白，让机器人先说
    # {"role": "assistant", "content": "你好！有什么可以帮你的吗？"}
]

while True:
    # --- 获取用户输入 ---
    user_input = input("你: ")
    if user_input.lower() == 'quit':
        print("机器人: 再见！")
        break
    if not user_input:
        continue

    # --- 将用户输入添加到历史记录 ---
    history.append({"role": "user", "content": user_input})

    try:
        # --- 发起请求，传入完整的历史记录 ---
        # 建议在交互式应用中使用流式输出以获得更好的体验
        stream_response = client.chat.completions.create(
            model="deepseek-chat",
            messages=history, # 传入包含所有历史的 messages 列表
            temperature=0.7,
            stream=True
        )

        print("机器人: ", end="")
        assistant_response_content = ""
        # --- 处理流式输出 ---
        for chunk in stream_response:
            if chunk.choices and chunk.choices[0].delta and chunk.choices[0].delta.content:
                content_part = chunk.choices[0].delta.content
                print(content_part, end="", flush=True)
                assistant_response_content += content_part
            if chunk.choices and chunk.choices[0].finish_reason:
                 # 流结束后换行，为下次输入做准备
                 print() # 换行
                 break # 结束当前轮次的流处理

        # --- 将机器人的完整回复添加到历史记录 ---
        if assistant_response_content: # 确保有回复内容再添加
             history.append({"role": "assistant", "content": assistant_response_content})

        # --- (可选) 限制历史记录长度，防止 Token 过多 ---
        # 例如，只保留最近 N 轮对话，同时保留 system message
        MAX_HISTORY_TURNS = 10 # 保留最近10轮对话（10用户+10助手）
        if len(history) > 1 + MAX_HISTORY_TURNS * 2: # 1 是 system message
             # 保留 system message 和最新的 N*2 条消息
             history = [history[0]] + history[-(MAX_HISTORY_TURNS * 2):]

    except Exception as e:
        print(f"\n[API 调用出错: {e}]")
        # 出错时，可以选择移除刚才添加的用户输入，避免影响下一轮
        if history and history[-1]["role"] == "user":
            history.pop()
        print("机器人: 抱歉，我暂时遇到了一些问题，请稍后再试。")
```

## 代码解释

1. 使用一个列表 `history` 来存储对话记录。通常以 `system` 消息开始。
1. 在循环中：
   - 获取用户输入。
   - 将用户的消息（`role: user`）添加到 `history`。
   - 调用 API 时，将 `history` 作为 `messages` 参数传入。这让模型能够理解之前的对话内容。
   - 接收并处理模型的回复（推荐使用流式）。
   - 将模型的完整回复（`role: assistant`）添加回 `history`。
1. **注意：** 随着对话进行，`history` 会越来越长，导致每次 API 请求消耗的输入 `Token` 增多，成本上升。实际应用中需要策略来限制历史记录的长度（如示例中的 `MAX_HISTORY_TURNS`）。
1. 增加了错误处理，防止因单次 `API` 失败导致整个程序崩溃。

# 解析 API 响应与错误处理（基于 openai 库）

## 解析成功响应

### 非流式（`stream=False`）

- 模型回复：`response.choices[0].message.content`
- 结束原因：`response.choices[0].finish_reason`（`stop`、`length` 等）
- `Token` 用量：`response.usage`（包含 `prompt_tokens`、`completion_tokens`、`total_tokens`）

### 流式（`stream=True`）

- 通过迭代 `for chunk in stream_response:` 获取数据块。
- 增量内容：`chunk.choices[0].delta.content`（需要检查非 `None`）。
- 结束原因：在某个 `chunk` 中 `chunk.choices[0].finish_reason` 会变为非 `None`。
- `Token` 用量：通常不易在流式迭代中直接获取完整 `usage`。

## 错误处理

`openai` 库会根据 HTTP 状态码和响应内容抛出特定的异常，方便捕获：

- `openai.AuthenticationError`：`API Key` 无效或权限问题（对应 `401`/`403`）。
- `openai.RateLimitError`：超出速率限制（对应 `429`）。
- `openai.BadRequestError`：请求参数错误（对应 `400`）。
- `openai.APIConnectionError`：网络连接问题。
- `openai.APITimeoutError`：请求超时。
- `openai.APIError` / `openai.APIStatusError`：其他 API 层面错误（如 `5xx` 服务器错误）。

**通用捕获：** 可以使用 `except openai.APIError as e:` 或更通用的 `except Exception as e:` 来捕获所有潜在错误，并通过 `e` 对象获取错误信息。

## 推荐的错误处理结构

```python
from openai import OpenAI, APIError, RateLimitError, AuthenticationError # 导入特定错误类型

# ... client 初始化 ...

try:
    # ... 调用 client.chat.completions.create(...) ...
    pass
except AuthenticationError as e:
    print(f"认证错误: API Key 可能无效或过期。 {e}")
except RateLimitError as e:
    print(f"速率限制错误: 请求过于频繁，请稍后重试。 {e}")
    # 可以考虑在这里加入等待和重试逻辑
except APIError as e: # 捕获其他 API 相关错误 (4xx, 5xx)
    print(f"API 错误: {e.status_code} - {e.message}")
except Exception as e: # 捕获其他意外错误 (如网络问题)
    print(f"发生意外错误: {e}")
```

# 构建一个基于 Gradio 的网页聊天机器人

在掌握了如何通过 `Python` 调用 `DeepSeek API`（包括基本调用、流式处理和错误处理）之后，我们可以将这些知识整合起来，构建一个更直观、更用户友好的应用程序。在本节中，我们将使用流行的 `Python` 库 `Gradio` 来快速创建一个基于 Web 页面的聊天机器人界面，让用户可以直接在浏览器中与我们的 `DeepSeek` 模型进行交互。

## 为什么选择 `Gradio`？

`Gradio` 使得为机器学习模型创建 `Web UI` 变得极其简单。我们只需提供一个处理输入的 `Python` 函数，`Gradio` 就能自动生成交互式的界面组件，如文本框、按钮和聊天记录显示区域。这非常适合快速搭建原型和演示应用。

## 前提条件

- 您需要安装 `gradio` 库：`pip install gradio`
- 确保您已经按照前面的步骤，能够通过 `openai` 库成功调用 `DeepSeek API`，特别是您的 `DEEPSEEK_API_KEY` 已正确设置为环境变量。

## 核心代码实现

下面是构建这个网页聊天机器人的完整 `Python` 代码。建议将其保存为一个 `.py` 文件（例如 `deepseek_gradio_app.py`）。



# DeepSeek Gradio 应用完整代码

```python
# deepseek_gradio_app.py
# (将上面提供的完整代码粘贴到这里)
import os
import gradio as gr
from openai import OpenAI, APIError, RateLimitError, AuthenticationError
import traceback

# --- 安全地获取 API Key ---
api_key = os.getenv("DEEPSEEK_API_KEY")

# --- 初始化 OpenAI 客户端 ---
client = None
if api_key:
    try:
        client = OpenAI(api_key=api_key, base_url="https://api.deepseek.com/v1")
        print("DeepSeek API 客户端初始化成功。")
    except Exception as e:
        print(f"初始化 OpenAI 客户端时出错: {e}")
        client = None
else:
    print("警告：环境变量 DEEPSEEK_API_KEY 未设置。")
    client = None

# --- 定义 Gradio 的核心处理函数 ---
def predict(message, history):
    if not client:
        yield "**错误**：DeepSeek API 客户端未初始化。请检查 API Key 设置。"
        return

    openai_messages = [{"role": "system", "content": "你是一个乐于助人、知识渊博的AI助手。请用清晰、简洁的语言回答问题。"}]
    openai_messages.extend(history)
    # Gradio ChatInterface 通常会将最新用户消息加入 history, 我们信任它
    # openai_messages.append({"role": "user", "content": message}) # 可能重复，注释掉

    MAX_HISTORY_MESSAGES = 15
    if len(openai_messages) > MAX_HISTORY_MESSAGES:
        openai_messages = [openai_messages[0]] + openai_messages[-(MAX_HISTORY_MESSAGES - 1):]

    print(f"\n--- 向 API 发送的消息 (共 {len(openai_messages)} 条) ---")
    print("----------------------------------------")

    try:
        stream_response = client.chat.completions.create(
            model="deepseek-chat",
            messages=openai_messages,
            stream=True,
            temperature=0.7,
            max_tokens=1024
        )

        partial_message = ""
        print("模型回复 (流式): ", end="")
        for chunk in stream_response:
            if chunk.choices and chunk.choices[0].delta and chunk.choices[0].delta.content:
                content_part = chunk.choices[0].delta.content
                partial_message += content_part
                print(content_part, end="", flush=True)
                yield partial_message # 流式返回给 Gradio

        print("\n--- 流式输出结束 ---")

    except AuthenticationError as e:
        print(f"\n认证错误: {e}")
        yield f"**错误**：认证失败。检查 API Key。"
    except RateLimitError as e:
        print(f"\n速率限制错误: {e}")
        yield f"**错误**：请求过于频繁，请稍后再试。"
    except APIError as e:
        print(f"\nAPI 错误: Status Code={e.status_code}, Response={e.response}")
        yield f"**错误**：API 返回错误 (代码: {e.status_code})。详情: {e.message}"
    except Exception as e:
        print(f"\n发生意外错误: {type(e).__name__} - {e}")
        traceback.print_exc()
        yield f"**发生意外错误**: {type(e).__name__}。请检查服务器日志。"


# --- 创建 Gradio 聊天界面组件 ---
chatbot_component = gr.Chatbot(
    label="DeepSeek 对话窗口",
    height=650,
    type='messages', # 重要：使用 OpenAI 格式
    show_copy_button=True,
    bubble_full_width=False
)

# --- 使用 gr.ChatInterface 创建完整的聊天应用 UI ---
chatbot_interface = gr.ChatInterface(
    fn=predict,
    title="🚀 DeepSeek 智能聊天助手",
    description="你好！我是基于 DeepSeek 大模型的聊天机器人。输入你的问题，开始对话吧！",
    chatbot=chatbot_component,
    examples=[
        ["你好，介绍一下你自己。"],
        ["用 Python 写一个快速排序算法。"],
        ["解释一下什么是“量子纠缠”？"],
        ["给我讲个笑话吧。"]
    ],
    cache_examples=False,
    textbox=gr.Textbox(placeholder="🤔 在这里输入您的问题...", container=False, scale=7)
    # 移除或注释掉 retry_btn, undo_btn, clear_btn, submit_btn 如果它们导致 TypeError
    # retry_btn="🔄 重试",
    # undo_btn="↩️ 撤销",
    # clear_btn="🗑️ 清除对话",
    # submit_btn="发送"
)

# --- 启动 Gradio 应用 ---
if __name__ == "__main__":
    if not client:
        print("\n" + "="*40)
        print("‼️ 严重警告：未能初始化 DeepSeek API 客户端。")
        print("   聊天功能将无法工作！")
        print("   请确保正确设置并导出 DEEPSEEK_API_KEY 环境变量。")
        print("="*40 + "\n")
    else:
        print("\n" + "="*40)
        print("✅ DeepSeek API 客户端已准备就绪。启动 Gradio 应用...")
        print("="*40 + "\n")

    chatbot_interface.launch(server_name="127.0.0.1") # 允许局域网访问     
```

## 代码关键点解析

1. `import gradio as gr`：导入 Gradio 库。

1. `predict(message, history)` 函数：这是 Gradio 应用的核心逻辑。

   + 它接收用户当前输入 `message` 和由 Gradio 自动管理的 `history`（我们配置了 `type='messages'`，所以 `history` 直接是 OpenAI 的消息格式列表）。
   + 构建 `messages`：我们添加了一个系统提示（system role），然后将 `history` 扩展进去，最后（隐式地，由 Gradio 处理）用户的最新 `message` 也会包含在内（体现在 `history` 中）。这保证了模型能理解对话的上下文。
   + 历史截断：添加了简单的逻辑来限制 `messages` 列表的最大长度，防止 API 调用因 Token 过多而失败或费用过高。
   + 调用 API（流式）：使用 `client.chat.completions.create` 并设置 `stream=True`。这与我们在 3.3 节学到的流式调用一致。
   + `yield partial_message`：这是实现 Gradio 流式输出的关键。函数不再是一次性 `return` 完整结果，而是通过 `yield` 逐步返回部分生成的文本。Gradio 会捕捉这些 `yield` 的值并动态更新聊天界面，给用户带来实时的打字机效果。
   + 错误处理：函数内包含了对常见 API 错误的捕获（认证、速率限制、API 返回错误）以及通用异常处理，并将错误信息通过 `yield` 返回给 Gradio 界面显示。

1. `gr.Chatbot(...)`：创建了一个聊天气泡样式的显示组件，通过 `type='messages'` 指定了其内部处理历史记录的格式。

1. `gr.ChatInterface(...)`：这是 Gradio 提供的一个高级便利组件，它封装了创建完整聊天界面所需的各种元素：

   + 自动使用我们定义的 `predict` 函数作为后端逻辑（`fn=predict`）。
   + 自动管理聊天历史的显示（使用我们传入的 `chatbot_component`）。
   + 自动提供文本输入框（`textbox`）。
   + 自动添加发送、重试、撤销、清除等常用按钮。
   + 可以通过 `title`、`description`、`examples` 等参数定制界面外观和行为。

1. `chatbot_interface.launch(...)`：这行代码启动一个本地的 Web 服务器来运行我们的 Gradio 应用。`server_name="0.0.0.0"` 参数使得在同一局域网内的其他设备也可以通过运行脚本的机器的 IP 地址来访问这个聊天机器人（如果防火墙允许的话）。

## 如何运行

1. **保存代码：** 将上述完整代码保存为 Python 文件，例如 `deepseek_gradio_app.py`。

1. **设置环境变量：** 再次强调，在运行脚本的终端（或环境中）必须先设置好您的 DeepSeek `API Key` 环境变量。例如：

   + Linux/macOS: `export DEEPSEEK_API_KEY='sk-xxxxxxxxxxxxxxxxxxxx'`
   + Windows CMD: `set DEEPSEEK_API_KEY=sk-xxxxxxxxxxxxxxxxxxxx`
   + Windows PowerShell: `$env:DEEPSEEK_API_KEY='sk-xxxxxxxxxxxxxxxxxxxx'`

1. **运行脚本：** 在设置好环境变量的终端中，执行 `python deepseek_gradio_app.py`。

1. **访问应用：** 终端会输出类似 `Running on local URL: http://127.0.0.1:7860` 和 `Running on public URL: ...`（如果使用了 `share=True`，或类似 `Running on http://0.0.0.0:7860` 如果使用了 `server_name="0.0.0.0"`）的信息。

1. **打开浏览器：** 在您的浏览器中访问提供的本地 URL（通常是 `http://127.0.0.1:7860` 或 `http://<你的IP地址>:7860`）。

1. **开始聊天：** 您现在应该能看到一个功能完善的聊天界面，可以开始与 DeepSeek 模型进行对话了！

![](img/3635fbde20c7f149b0425798b3e8ef04.png)

## 安全提示

+ 这个示例假设您在本地运行，并通过环境变量管理 API Key。
+ 如果您打算将此类应用部署到公共服务器或与他人分享，绝对不能将 `API Key` 硬编码在代码中。需要采用更安全的密钥管理策略，例如使用服务器端环境变量、Secrets Manager 服务或专门的配置文件（并确保该文件不被公开访问）。

> 如果您希望从互联网上临时访问您在本地运行的 Gradio 应用（例如，为了方便演示或与他人短暂分享），最关键的修改是在 `launch()` 方法中添加 `share=True` 参数。
>
> 找到代码末尾的启动部分，修改 `launch()` 方法如下：

```python
# --- 启动 Gradio 应用 ---
if __name__ == "__main__":
    # ... (之前的检查 API Key 的代码保持不变) ...

    # *** 关键修改：添加 share=True ***
    chatbot_interface.launch(
        server_name="0.0.0.0", # 保持或移除皆可，share=True 主要依赖Gradio的隧道
        share=True             # <--- 添加此参数
    )
    # 如果只想本机和公开访问，可以省略 server_name
    # chatbot_interface.launch(share=True)
```

### 说明

1. `share=True`：当您添加这个参数后，Gradio 会尝试通过其内置的隧道服务（通常利用类似 frp 或 ngrok 的技术）创建一个临时的、公开可访问的 URL。
1. 输出：运行脚本后，除了本地 URL（`http://127.0.0.1:7860` 或 `http://0.0.0.0:7860`），终端还会输出一个公网 URL（Public URL），通常看起来像 `https://<随机字符>.gradio.live` 或类似的域名。
1. 访问：任何人只要知道这个公网 URL，就可以在浏览器中访问您的 Gradio 应用，无论他们在世界的哪个地方。

### ⚠️ 重要警告和注意事项

1. 安全性：这个生成的公网 URL 是完全公开的！任何知道该链接的人都可以访问您的应用。这意味着他们可以与您的聊天机器人交互，并消耗您的 DeepSeek `API Key` 的额度。请极其谨慎地分享这个链接，并且只分享给您信任的人。
1. 临时性：这个公网链接通常是临时的（Gradio 官方说明一般是 72 小时有效），并且在您停止本地运行的 `Python` 脚本后就会失效。它不适用于永久托管应用。
1. 性能：通过隧道访问可能会比本地访问稍慢一些。
1. `API Key` 暴露风险（间接）：虽然代码本身不暴露 `Key`，但任何能访问应用的人都在使用您的 `Key`。如果应用本身有任何（不太可能的）漏洞被利用，或者被恶意大量使用，都可能带来风险或产生高额费用。绝对不要在需要公开部署的应用中直接硬编码或以不安全方式加载 `API Key`。对于需要长期公开访问的应用，应考虑部署到云平台并实施更严格的安全措施（如用户认证、更安全的密钥管理等）。

## 总结



这个 Gradio 应用演示了如何将我们之前学习的 DeepSeek API 调用知识（特别是使用 `openai` 库、处理 `messages` 历史、实现流式输出和错误处理）集成到一个交互式的 Web 应用中。这为您将来构建更复杂的基于 LLM 的应用打下了基础。

# 在本地运行 DeepSeek 模型

## 引言

在前面几个部分，我们重点学习了如何通过云 API 来利用 DeepSeek 大模型的能力。云 API 提供了极大的便利性和强大的性能，但并非适用于所有场景。在这一部分，我们将深入探讨另一种选择：在您自己的计算机上本地部署和运行 DeepSeek 模型。我们将讨论本地部署的利弊，分析不同规模模型对硬件的具体要求，了解常见的本地模型格式与获取途径，并重点实操如何使用用户友好的工具 LM Studio 来完成本地部署和测试。最后，我们也会简单介绍其他流行的本地部署工具。

## 本地部署的优势与挑战

正如我们在第一部分简单对比过的，选择本地部署而非云 API 调用，主要基于以下考量：

### 优势

1.  **数据隐私与安全：** 这是本地部署最核心的优势。所有数据处理都在您的本地机器上完成，数据完全不离开您的设备或网络，对于处理敏感信息（如个人隐私、商业机密）的场景至关重要。

1.  **无网络依赖：** 一旦模型和运行环境配置完成，您可以在没有互联网连接的情况下运行模型，适合离线环境或网络不稳定的场景。

1.  **无持续 API 调用费用：** 除了初期的硬件投入和电力消耗，本地运行模型没有按 Token 计费的持续性成本。对于高频、大量的调用需求，长期来看可能更经济。

1.  **潜在的低延迟：** 在配置得当的本地硬件上，请求无需经过网络传输和远程服务器排队，理论上可以获得更低的响应延迟。

1.  **高度定制化与控制：** 您可以更自由地选择模型版本、配置运行参数，甚至对模型进行微调（Fine-tuning，需要额外技术和资源）以适应特定任务。

### 挑战

1.  **高昂的硬件门槛：** 大语言模型，尤其是参数量较大的模型，对硬件资源（特别是 GPU 显存 VRAM）有极高的要求。购置或租赁满足要求的高性能硬件是一笔巨大的初始投资。

1.  **部署与维护的复杂性：** 搭建运行环境、下载配置模型、解决依赖问题、优化性能以及后续的模型更新都需要一定的技术知识和时间投入。

1.  **模型规模受限：** 受限于本地硬件能力，您可能无法运行最大、最先进的模型（如云端提供的数百亿甚至千亿参数模型），导致在处理复杂任务时效果不如云 API。

1.  **性能优化难度：** 要在本地达到理想的推理速度，往往需要对硬件驱动、运行库、模型加载方式等进行细致的优化配置。

1.  **维护成本：** 除了硬件，还需要考虑电力消耗、散热以及可能的硬件故障维护。

### 总结

本地部署为特定需求（如数据隐私、离线运行、成本控制）提供了解决方案，但同时也带来了显著的硬件和技术挑战。

## DeepSeek 不同模型版本对硬件（CPU， RAM， VRAM）的需求分析

要在本地顺利运行 DeepSeek 模型，选择与您硬件相匹配的模型版本至关重要。模型的参数量（通常以 B，即十亿为单位）是决定硬件需求的核心因素。以下是基于您提供的信息和社区实践整理的 DeepSeek 模型（以常见可下载版本为例）大致硬件需求：

### 关键硬件因素说明

1.  VRAM （GPU 显存）: 最关键的瓶颈。

+   模型的主要计算发生在 GPU 上，模型参数（权重）需要加载到显存中。

+   **量化（`Quantization`）**：为了在有限的显存中运行大模型，通常使用量化模型（我们将在 4.3 节详述，如 `GGUF` 格式）。量化会降低模型精度（通常影响不大）来大幅减少显存占用。上表中的 VRAM 需求是基于常见的 4 位量化（如 `Q4_K_M`）估算的。未量化的原始模型（`FP16`/`BF16`）需要约 2 倍或更多的显存！

+   显存 = 模型大小 + 计算缓存 （KV Cache）。除了模型本身，推理过程中还需要额外显存存储中间状态（KV Cache），其大小与处理的上下文长度有关。因此，实际所需显存通常比模型量化后的大小要多一些。

1.  **RAM （系统内存）:**

+   如果显存不足以完全容纳模型，一些工具（如 `LM Studio`）支持将部分模型层卸载（`Offload`）到系统内存中。这会显著降低推理速度，但能让您运行更大的模型。此时 RAM 的需求会大大增加。

+   即使模型完全在 GPU 上运行，操作系统和运行工具本身也需要消耗一定的 RAM。

1.  **CPU:**

+   在模型加载、处理输入/输出（Tokenization/Detokenization）时需要 CPU。

+   如果完全使用 CPU 进行推理（没有 GPU 或显存不足），CPU 的性能（核心数、频率、指令集支持如 `AVX2`）将直接决定推理速度，通常会非常慢。

1.  **存储 （硬盘/SSD）:**

+   需要足够的空间下载和存储模型文件。`GGUF` 模型文件大小从几 GB 到几十 GB 不等。

+   推荐使用 `SSD`（固态硬盘），可以显著加快模型的加载速度。

**经济型替代方案提示:**

+   **蒸馏版模型：** 寻找官方或社区提供的蒸馏（Distilled）版本，它们在保持较好性能的同时，参数量和硬件需求更低。

+   **国产加速卡：** 如果适用，可以关注如华为昇腾、摩尔线程等提供的 AI 计算卡，可能提供不同的性价比选项。

**性能权衡：**

+   一般来说，参数量越大的模型，在处理复杂任务、理解长上下文、生成内容质量等方面表现越好。

+   但更大的模型也意味着更高的硬件成本和更慢的推理速度（在同等硬件下）。选择哪个版本需要在您的任务需求、预算和硬件条件之间找到平衡点。

## 本地模型格式（如 `GGUF`）与下载来源（如 Hugging Face）

要在本地运行 DeepSeek（或其他许多开源 LLM），您需要获取适合本地推理的模型文件。这涉及到两个关键概念：模型格式和下载来源。

+   模型格式：`GGUF`（`GPT-Generated Unified Format`）

+   当前主流：`GGUF` 是目前在 `llama.cpp` 生态系统（包括 `LM Studio`、`Ollama` 等许多本地运行工具）中最广泛使用的模型文件格式。它取代了之前的 `GGML` 格式。

+   优点：

+   自包含：一个 `.gguf` 文件通常包含了运行模型所需的所有信息（模型结构、权重、分词器配置等），方便分发和使用。

+   支持量化：`GGUF` 原生支持多种量化级别。模型发布者通常会提供同一基础模型的多个不同量化版本的 `GGUF` 文件，让用户根据自己的硬件选择。

+   跨平台：可以在 CPU 和多种 GPU（NVIDIA， AMD， Apple Metal）上运行。

+   量化级别（`Quantization Levels`）：您在下载 `GGUF` 文件时会看到类似 `Q4_K_M`、`Q5_K_M`、`Q8_0`、`F16` 等标识。这代表了不同的量化程度：

+   数字越小（如 `Q2`、`Q3`、`Q4`）：量化程度越高，文件越小，VRAM 占用越低，速度可能越快，但模型精度损失相对越大。`Q4_K_M` 是一个非常流行的平衡点，提供了良好的压缩率和可接受的性能损失。

+   数字越大（如 `Q5`、`Q6`、`Q8_0`）：量化程度越低，文件越大，VRAM 占用越高，模型精度损失越小，更接近原始模型。`Q8_0` 通常性能损失很小。

+   **`F16` / `FP16`：** 16 位浮点数，基本无损（相对于原始 `FP32`），但文件最大，VRAM 需求最高。

+   **`K_M`、`K_S` 等：** 指示了特定的量化实现细节（K-quants），通常 `_K_M`（Medium）比 `_K_S`（Small）质量稍好。

+   **如何选择量化：** 根据您的 VRAM 大小选择能容纳下的最大量化级别。例如，8GB VRAM 用户通常选择 7B 模型的 `Q4_K_M` 或 `Q5_K_S` 版本。

+   下载来源：Hugging Face Hub

+   **模型中心：** Hugging Face（`huggingface.co`）是目前全球最大、最活跃的 AI 模型、数据集和代码的托管平台。它是下载开源 LLM（包括 DeepSeek 的 `GGUF` 版本）的首选来源。

+   如何寻找 DeepSeek `GGUF` 模型：

1.  访问 Hugging Face 网站（`huggingface.co`）。（亦可访问国内镜像站：`https://hf-mirror.com/`）

1.  在顶部的搜索框中输入关键词，例如 `deepseek gguf`、`deepseek coder gguf` 或 `deepseek llm gguf`。

1.  浏览搜索结果。您会看到由 DeepSeek 官方或社区贡献者（如知名的 `TheBloke` 曾是 `GGUF` 模型的主要转换和提供者，但请注意验证上传者信誉）上传的模型仓库（`Repositories`）。

1.  进入模型仓库页面，通常会有一个 `Files and versions`（文件和版本）标签页。

1.  在该标签页下，您会看到一系列以 `.gguf` 结尾的文件列表，文件名通常会清晰地标明模型版本、参数量和量化级别（例如 `deepseek-llm-7b-chat.Q4_K_M.gguf`）。

1.  选择适合您硬件的量化版本进行下载。注意文件大小，下载可能需要一些时间。

+   **验证来源：** 尽量从 DeepSeek 官方账号或社区内声誉良好的贡献者处下载模型，以确保模型的完整性和安全性。查看模型的 `README` 文件和社区讨论通常有助于判断。

## 使用 LM Studio 部署和测试 DeepSeek 模型

`LM Studio` 是一个非常受欢迎的桌面应用程序，它提供了一个图形化界面，让用户可以轻松地发现、下载、管理并在本地运行兼容 `llama.cpp`（即支持 `GGUF` 格式）的大语言模型。它非常适合初学者进行本地部署体验。

### 步骤 1: 软件安装与配置

1.  **下载 `LM Studio`：** 访问 `LM Studio` 官方网站 `https://lmstudio.ai/`。网站会自动检测您的操作系统（Windows，macOS，Linux），并提供相应的下载链接。

1.  **安装：** 下载完成后，运行安装程序。安装过程通常是标准的图形化安装流程，按照提示操作即可。

1.  **首次启动与配置：**

+   首次启动 `LM Studio` 时，它可能会进行一些初始化设置，例如检查硬件兼容性（特别是 GPU 检测）。

+   界面通常分为几个主要区域（通过左侧图标切换）：搜索模型、聊天界面、本地服务器、模型管理等。熟悉一下界面布局。

+   在设置（通常是右下角的齿轮图标或类似位置）中，确认 `Use LM Studio's Hugging Face Proxy` 选项已经选中，这个选项能加速模型的下载速度。

![](img/1f57f8ba34ab0767932d2e090f6f916b.png)

+   可选，您可以在设置中检查 GPU 加速是否已启用（如果您的机器有兼容的 NVIDIA/AMD/Apple Silicon GPU）。`LM Studio` 通常会自动配置，但可以手动调整如 VRAM 使用上限、CPU 线程数、是否允许内存映射等高级选项。对于初学者，保持默认设置通常即可。

### 步骤 2: 模型下载与加载

1.  **搜索模型：** 点击左侧导航栏的放大镜图标（`Search`）进入模型搜索界面。

1.  **输入关键词：** 在顶部的搜索框中输入 `deepseek` 或更具体的 `deepseek llm gguf` / `deepseek coder gguf`。

1.  **浏览结果：** `LM Studio` 会从 Hugging Face Hub 拉取匹配的模型列表。您会看到不同的 DeepSeek 模型仓库。点击一个仓库（例如 `DeepSeek-R1-Distill-Qwen-7B-GGUF` 或社区转换的版本）。

![](img/850152de81a1b986f77f0b38b6e18e3c.png)

1.  **选择 `GGUF` 文件：** 在右侧或下方区域，会列出该仓库中可用的 `GGUF` 文件。仔细查看文件名，找到包含您想要的模型（如 `llm` 或 `coder`）、参数量（如 `7b`）和适合您硬件的量化级别（如 `Q4_K_M`）的文件。旁边通常会显示文件大小和大致的 VRAM 需求估计。

![](img/22aae7abedd5f03a42c0646730f76340.png)

1.  **下载模型：** 找到合适的文件后，点击其旁边的 `Download` 按钮。下载进度会显示在界面下方。根据模型大小和您的网速，下载可能需要几分钟到几十分钟甚至更长时间。

1.  **加载模型：** 下载完成后，切换到聊天图标（`Chat`）界面。

1.  **选择模型：** 在聊天界面顶部，点击 `Select a model to load` 的下拉菜单，您应该能看到刚刚下载的 DeepSeek `GGUF` 文件。选择它。

![](img/df5c52f42ac351182c18e879985d03b2.png)

![](img/d2eae2d360495cd1a4616a3d2082173b.png)

1.  **等待加载：** `LM Studio` 会开始将模型加载到内存/显存中。您可以在界面顶部或右侧看到加载进度条以及实时的 RAM 和 VRAM 使用情况。加载大型模型可能需要一些时间。

+   **注意：** 如果您的 VRAM 不足以完全容纳所选模型，`LM Studio` 默认会尝试将超出的部分卸载（`Offload`）到系统 RAM 中。这会使模型运行，但速度会显著变慢。您可以在右侧的配置面板中调整卸载的层数（`GPU Offload`）。如果 VRAM 充足，请确保 `Offload` 设置为 `0` 或 `Max`/全部加载到 GPU。

1.  **加载完成：** 模型加载成功后，聊天界面会准备就绪，您可以开始与模型进行交互式对话了！直接在下方的输入框输入文字，按回车或点击发送按钮即可。

![](img/516fac2ff11991e0b69a117c87b71659.png)

### 步骤 3: 本地 API 服务启动与测试

`LM Studio` 不仅提供聊天界面，还能启动一个本地的 HTTP 服务器，该服务器兼容 OpenAI API 格式。这意味着您可以使用在第三部分学习的 API 调用代码（基于 `openai` 库）来与本地运行的模型进行交互，就像调用云 API 一样！

1.  **切换到服务器界面：** 点击左侧导航栏的终端图标（`<>`）。

1.  **选择已加载模型：** 在顶部下拉菜单中，确保选中您当前已经在 `Chat` 界面加载好的那个 DeepSeek 模型。如果您尚未加载模型，需要先去 `Chat` 界面加载。

1.  **配置服务器（可选）：** 您可以调整服务器监听的端口号（默认为 `1234`）、上下文长度限制（`n_ctx`）等参数。对于基本测试，保持默认即可。可以考虑启用 `在网络中提供服务` 选项，允许网络中其他设备连接 API 接口。

![](img/9b9f577806fc6f1e2dbfe22d31c31d0b.png)

1.  **启动服务器：** 点击 `Start Server` 按钮。

![](img/8fcd43e05bc6401247e82c172897b453.png)

1.  **获取 Base URL：** 服务器启动后，界面会显示监听地址和端口，关键信息是 `Base URL`，通常是 `http://localhost:1234/v1`。记下这个地址。

1.  **测试 API 调用：** 现在，您可以使用 Python 代码来调用这个本地 API。复用我们在 3.3 节的 `openai` 库代码，只需做两处关键修改：

+   `base_url`：将其修改为 `LM Studio` 提供的本地服务器地址。

+   `api_key`：`LM Studio` 的本地服务器默认不需要 API Key 验证，您可以将其设置为 `None`、空字符串 `""` 或任何虚拟值（如 `"lm-studio"`）。

1.  示例 Python 代码（修改自 3.3 节）:



```
import os
from openai import OpenAI

# --- 初始化 OpenAI 客户端，指向 LM Studio 本地服务器 ---
client = OpenAI(
    base_url="http://localhost:1234/v1", # 修改 Base URL
    api_key="lm-studio"                  # 设置虚拟 API Key 或 None
)

print("--- 调用本地 LM Studio API ---")
try:
    completion = client.chat.completions.create(
        model="loaded-model", # 模型名称通常可以任意写，因为服务器只加载了一个
        # 或者从 LM Studio 界面确认是否需要特定名称
        messages=[
            {"role": "system", "content": "你正在通过 LM Studio 本地运行。"},
            {"role": "user", "content": "请用 Python 写一个简单的 'Hello World' 程序。"}
        ],
        temperature=0.7,
        stream=False # 可以设为 True 测试流式输出
    )

    # 打印回复
    print("模型回复:\n", completion.choices[0].message.content)

    # (可选) 打印 Token 使用情况 (本地服务器可能不提供精确的 usage)
    # usage = completion.usage
    # if usage:
    #     print("\nToken 使用情况 (可能不准确):")
    #     print(usage)

except Exception as e:
    print(f"调用本地 API 出错: {e}")
    print("请确保 LM Studio 服务器已启动，并且模型已加载。")

# 停止 LM Studio 服务器: 回到 LM Studio 界面，点击 "Stop Server" 按钮。
```

1. Cherry Studio 中的模型配置如下图所示

![](img/90e64e5f82c30c801c1ed4c5f712bd08.png)

**查看日志：** 在 `LM Studio` 的服务器界面，您可以看到实时的请求日志，有助于调试 API 调用。

![](img/3befb199012ff86bf9efa45c4e926bba.png)

通过这种方式，您可以在本地环境中测试和开发需要调用 LLM API 的应用程序，而无需实际消耗云 API 的额度，并且保证了数据完全在本地处理。

## **（可选） 其他本地部署工具简介**

除了 `LM Studio`，还有一些其他流行的工具可以帮助您在本地运行大语言模型：

### Ollama (`ollama.ai`)

**特点：** 非常轻量级、易于使用的命令行工具。安装后，通过简单的命令如 `ollama run deepseek-llm:7b` 即可下载并运行模型。它在后台运行一个服务，并自动提供一个兼容 `OpenAI` 的 API 端点 （默认 `http://localhost:11434`）。

**优点：** 命令行友好，启动快速，资源占用相对较低，非常适合开发者集成到脚本或应用中。社区活跃，支持模型库不断更新。

**缺点：** 相对于 `LM Studio`，图形化管理功能较少，配置选项不如 `LM Studio` 直观。

### llama.cpp (`github.com/ggerganov/llama.cpp`)

**特点：** 这是驱动 GGUF 格式模型运行的核心 C/C++ 引擎项目。`LM Studio` 和 `Ollama` 等工具底层都使用了它或其变种。

**优点：** 性能优异，高度可配置，可以直接通过命令行编译和运行，适合需要深度定制和优化性能的开发者。可以直接控制量化、编译选项等。

**缺点：** 使用门槛相对较高，需要手动编译（虽然通常有预编译版本），命令行参数较多，不如封装好的工具易用。

### Text Generation Web UI (Oobabooga)

**特点：** 一个功能非常丰富的 Gradio Web UI，支持多种模型格式（包括 GGUF、Transformers 等），提供大量参数调整选项、扩展插件、角色扮演模式等。

**优点：** 功能强大，可玩性高，适合喜欢尝试各种模型和参数的用户。

**缺点：** 安装配置相对 `LM Studio` 或 `Ollama` 可能稍复杂，资源占用可能较高。

### vLLM / Text Generation Inference (`TGI`)

**特点：** 这些是更专注于高性能推理和吞吐量的服务框架，常用于生产环境部署或需要同时服务多个用户的场景。它们通常支持 PagedAttention 等高级优化技术。

**优点：** 推理速度快，吞吐量高，适合大规模应用。

**缺点：** 安装和配置通常比上述工具更复杂，对硬件（尤其是高端 GPU）要求更高。

### 选择哪个工具？

+ **新手入门/图形界面偏好：** `LM Studio` 是最佳选择。

+ **命令行/开发者集成/快速启动：** `Ollama` 非常方便。

+ **深度控制/性能极致追求：** 直接使用 `llama.cpp`。

+ 功能丰富/Web UI/参数实验： `Text Generation Web UI`。

+ **生产部署/高吞吐量：** `vLLM` 或 `TGI`。

**总结：**本部分详细介绍了在本地运行 DeepSeek 模型的各个方面，从基本概念到具体工具的实操。希望您现在对本地部署有了更全面的认识，并能根据自己的需求和条件，尝试在本地体验大模型的魅力。

# 总结回顾与展望

### 引言

恭喜您完成了本次关于获取与使用 DeepSeek API 及本地部署的课程学习！通过前面的四个部分，您应该已经对如何利用强大的 DeepSeek 大语言模型有了全面的了解，无论是在云端还是在本地。本部分将对整个学习路径进行总结回顾，为您提供进一步学习的资源和方向，并对课程中涉及的核心概念进行梳理和解释，以巩固您的理解。

## 学习路径回顾与总结

在本课程中，我们共同探索了以下关键领域：

1. **理解基础 （第一部分）:** 我们首先厘清了什么是大语言模型 API，它是如何作为连接开发者与 LLM 能力的桥梁，以及它的基本工作原理、调用方式和广泛应用场景。我们还对比了云 API 调用与本地部署的优劣势，并对 DeepSeek 模型家族的特点和版本进行了介绍。

1. **获取云访问权限 （第二部分）:** 我们实践了如何从 DeepSeek 官方平台、硅基流动、火山引擎等主要渠道注册账号、完成认证，并成功申请到调用 API 所必需的 `API Key`。同时，我们也重点学习了各平台的计费模型（特别是基于 Token 的计费方式）和保障 `API Key` 安全的最佳实践。

1. 实践云 API 调用 （第三部分）: 这是动手编码的核心环节。我们学习了构造 HTTP 请求的基本要素，理解了 `model`， `messages`， `temperature`， `stream` 等核心 API 参数的意义。我们使用 Python 的 `openai` 库，通过清晰的代码示例演示了如何进行标准的同步调用和实时的流式调用，并学会了解析响应、处理常见错误。最后，我们还构建了一个基于 `Gradio` 的交互式网页聊天应用，综合运用了所学知识。

1. **探索本地部署 （第四部分）:** 我们转向了在本地运行 DeepSeek 模型的方案。我们分析了本地部署的优势（如数据隐私）与挑战（如硬件需求），详细了解了不同参数规模（1.3B 到 70B+）的 DeepSeek 模型对 CPU、RAM、特别是 VRAM 的具体要求。我们学习了主流的本地模型格式 GGUF 及其量化级别的含义，知道了可以从 Hugging Face Hub 下载这些模型。最后，我们重点实操了如何使用用户友好的 `LM Studio` 工具来下载、加载 DeepSeek 模型，并在本地进行聊天测试以及启动一个兼容 `OpenAI` 格式的本地 API 服务器。我们也简要介绍了 `Ollama`、`llama.cpp` 等其他本地部署工具。

通过这一系列的学习和实践，您不仅掌握了理论知识，更具备了在实际项目中接入和使用 DeepSeek 大模型的基本能力。

## 进阶资源与社区推荐

大语言模型的世界广阔且发展迅速，本次课程只是一个起点。如果您希望继续深入探索，以下资源和社区可能会对您有帮助：

+ **官方文档：**

+ **DeepSeek 开放平台：** `https://platform.deepseek.com/api-docs` （获取最新的模型信息、定价、API 文档和官方示例）

+ **LM Studio:** `https://lmstudio.ai/docs` （查阅 `LM Studio` 的详细使用指南、配置说明和故障排除）

+ **Ollama:** `https://ollama.ai/library` 和 `https://github.com/ollama/ollama` （查看支持的模型库、命令行用法和项目文档）

+ **Gradio:** `https://www.gradio.app/docs/` （学习 `Gradio` 的各种组件和高级用法）

+ OpenAI Python Library: `https://github.com/openai/openai-python` （虽然我们用它调用 DeepSeek，但其文档对理解 API 结构和错误处理有帮助）

+ **模型与数据集中心：**

+ Hugging Face Hub: `https://huggingface.co/models` （探索和下载各种开源 LLM（包括 GGUF 格式）、数据集和相关工具的核心平台）

+ **社区与讨论：**

+ Reddit （`r/LocalLLaMA`）: 一个非常活跃的关于本地运行大语言模型的社区，可以找到大量关于硬件配置、模型评测、工具使用的讨论和经验分享。

+ 相关的 Discord 服务器： 许多开源项目（如 `LM Studio`， `Ollama`）或 AI 社区都有自己的 Discord 服务器，是获取帮助和交流的实时渠道。

+ **开发者论坛和博客：** 关注 AI 领域的知名博客、技术论坛，了解最新的技术进展和实践经验。

+ **进阶主题：**

+ 提示工程 （`Prompt Engineering`）: 学习如何设计更有效的提示（`Prompt`）来引导模型产生期望的输出。

+ 模型微调 （`Fine-tuning`）: 学习如何使用自己的数据对预训练模型进行微调，使其更适应特定任务或领域。

+ **模型评估：** 了解评估 LLM 性能的常用指标和方法。

+ **框架与库：** 探索如 `LangChain`， `LlamaIndex` 等用于构建复杂 LLM 应用的框架。

## 核心概念名词解释

以下是本次课程中涉及的一些核心概念的简要解释：

1. `API` （应用程序编程接口 / Application Programming Interface）: 一套预定义的规则和协议，允许不同的软件应用程序相互通信和交换数据。在本次课程中，特指让我们可以调用远程或本地 LLM 服务的接口。

1. `API Key` （API 密钥）: 一串唯一的代码，用作访问 API 服务的身份凭证，如同密码一样需要妥善保管。

1. `Authorization Header` （认证头）: HTTP 请求头的一部分，用于携带 `API Key` 或其他认证信息，以证明请求方有权访问服务。常用的是 `Bearer Token` 方式。

1. `Base URL` （基础 URL）: API 服务的主地址，具体的 API 功能通常在其后的路径（`Endpoint`）中指定。例如 `https://api.deepseek.com/v1`。

1. `Body` （请求体）: HTTP POST 请求中包含的主要数据部分，通常为 JSON 格式，用于向服务器传递参数和输入信息（如 `model`， `messages`）。

1. `Cloud API` （云 API）: 由服务提供商（如 DeepSeek）在云端服务器上托管和运行的 API 服务，用户通过互联网调用。

1. `content`（消息内容）: `messages` 列表里每个字典中的一个键，表示该条消息的具体文本内容。

1. `Endpoint` （API 端点）: `Base URL` 后面的具体路径，指向 API 提供的特定功能。例如 `/chat/completions`。

1. `Environment Variable` （环境变量）: 存储在操作系统级别的配置变量，是安全存储 `API Key` 等敏感信息的推荐方式，避免硬编码在代码中。

1. `GGUF` （GPT-Generated Unified Format）: 目前主流的、用于本地运行 LLM 的模型文件格式，通常支持量化，并且自包含模型所需信息。

1. `Gradio`：一个 Python 库，可以快速为机器学习模型创建交互式的 Web 用户界面 （UI）。

1. `GPU` （图形处理器 / Graphics Processing Unit）: 专门设计用于并行计算的硬件，对于加速 LLM 的训练和推理至关重要，其显存 （VRAM） 大小是本地运行 LLM 的关键瓶颈。

1. `Header` （请求头）: HTTP 请求的一部分，包含关于请求的元数据，如 `Content-Type` 和 `Authorization`。

1. `HTTP Request` （HTTP 请求）: 客户端（如我们的 Python 代码）向服务器（如 DeepSeek API）发送的网络消息，用于请求数据或执行操作。常用方法有 `GET`， `POST` 等。

1. `Hugging Face Hub`: 一个领先的在线平台，用于托管和分享 AI 模型、数据集和代码，是下载 GGUF 模型的主要来源。

1. `Inference` （推理）: 指使用已经训练好的大语言模型来处理新的输入（Prompt）并生成输出（Response）的过程。

1. `JSON` （JavaScript Object Notation）: 一种轻量级的数据交换格式，常用于 Web API 的请求体和响应体。

1. `LLM` （大语言模型 / Large Language Model）: 经过海量文本数据训练、参数量巨大、能够理解和生成自然语言的深度学习模型。

1. `LM Studio`：一个用户友好的桌面应用程序，简化了在本地下载、管理和运行 GGUF 格式 LLM 的过程，并能提供兼容 OpenAI 的本地 API 服务。

1. `Local Deployment` （本地部署）: 将大语言模型下载并配置在用户自己的计算机硬件上运行的过程。

1. `llama.cpp`：一个用 C/C++ 编写的高性能 LLM 推理引擎，是许多本地运行工具（如 `LM Studio`， `Ollama`）的核心基础，直接支持 GGUF 格式。

1. `max_tokens`（参数）: API 调用中的一个参数，用于限制模型单次生成内容的最大 Token 数量。

1. `messages`（参数）: 调用聊天补全 API 时传递的主要参数，是一个包含对话历史或指令的列表，每个元素是一个包含 `role` 和 `content` 的字典。

1. `model`（参数）: API 调用中用于指定要使用的具体模型名称的参数，例如 "deepseek-chat"。

1. `Ollama`：一个轻量级的命令行工具，用于在本地快速下载、运行 LLM 并提供本地 API 服务。

1. `Parameter` （参数）: 在调用 API 时，可以在请求体中设置的各种选项，用于控制模型的行为（如 `temperature`， `max_tokens`）或指定输入（如 `model`， `messages`）。

1. `Prompt` （提示）: 输入给大语言模型的文本指令或问题，用于引导模型生成期望的输出。

1. `Quantization` （量化）: 一种模型压缩技术，通过降低模型权重数值的精度（如从 16 位浮点数降到 4 位整数）来减小模型文件大小和 VRAM 占用，使得在资源有限的设备上运行大模型成为可能，但可能会牺牲少量模型性能。

1. `RAM` （内存 / Random Access Memory）: 计算机的系统内存。在本地运行 LLM 时，如果 VRAM 不足，部分模型可以卸载到 RAM 中运行（速度变慢），同时运行工具本身也需要 RAM。

1. `role`（消息角色）: `messages` 列表里每个字典中的一个键，指示该条消息的发送者身份，常见值有 `system`， `user`， `assistant`。

1. `SDK` （软件开发工具包 / Software Development Kit）: 由服务提供商或社区提供的库或工具集，封装了底层的 API 调用细节，让开发者能用更简洁的编程语言接口来使用服务（如 `openai` Python 库）。

1. `stream`（参数）: API 调用中的一个布尔型参数，设置为 true 时启用流式输出，服务器会边生成边返回结果，实现打字机效果。



## System Prompt（系统提示）

`messages` 列表中 `role` 为 `system` 的消息，通常放在最前面，用于设定 AI 助手的角色、行为准则或背景信息。

## Token（令牌）

大语言模型处理文本的基本单位。输入和输出的文本都会被转换成 `Token` 序列进行处理和计费。通常一个 `Token` 对应几个字母、一个单词或一个或几个汉字。

## VRAM（显存 / Video RAM）

`GPU` 上的专用高速内存，是本地运行大语言模型最关键的硬件资源，直接决定了能流畅运行多大的模型。

## 结语

希望本次课程为您打开了通往大语言模型应用世界的大门。无论是利用云端 `API` 的便捷，还是探索本地部署的深度，都蕴藏着巨大的潜力和乐趣。持续学习，不断实践，相信您一定能利用 `DeepSeek` 这样的强大工具创造出更多有价值的应用！感谢您的参与！
