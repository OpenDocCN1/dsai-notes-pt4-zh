
# GPT-5模型使用指南【面向开发者】

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/QaSvd1kBCoZId8xUHvBcbRtSn2f](https://kq4b3vgg5b.feishu.cn/docx/QaSvd1kBCoZId8xUHvBcbRtSn2f)

![](img/f429ea9bfa1efb789bf8b67310029da9.png)

## 一、GPT-5 模型介绍

当地时间2025年8月7日上午10点，OpenAI在 API 平台中发布了 `GPT-5`——这是OpenAI迄今为止用于编码和代理任务的最佳模型。

`GPT-5` 在关键编程基准测试中均达到 SOTA 水平，在 SWE-bench Verified 测试中得分为 74.9%，在 Aider polyglot 测试中得分为 88%。`GPT-5` 可以说是真正的编程协作者。它擅长生成高质量代码，并能处理诸如修复错误、编辑代码以及解答复杂代码库相关问题等任务。该模型具有可操控性和协作性——它可以高精度地遵循非常详细的指令，并在工具调用之前和调用之间提供对其操作的预先解释。该模型在前端编码方面也表现出色，在OpenAI内部测试中，前端 Web 开发方面 70% 的成绩都击败了 OpenAI `o3`。

OpenAI与初创公司和大型企业的早期测试人员合作，对`GPT-5`进行了真实编程任务的训练。Cursor 表示，`GPT-5` 是“他们用过的最智能的模型”，“非常智能，易于操控，甚至拥有其他模型中从未见过的个性”。Windsurf 表示，`GPT-5` 在他们的评估中处于 SOTA 阶段，“其工具调用错误率仅为其他前沿模型的一半”。Vercel 表示：“它是最好的前端 AI 模型，在审美和代码质量方面都达到了顶级水平，独树一帜。”

`GPT-5` 在长时间运行的代理任务方面也表现出色，在两个月前发布的工具调用基准测试 τ2 (-) bench telecom (96.7%) 上取得了 SOTA 结果。`GPT-5` 改进的工具智能使其能够可靠地将数十个工具调用（顺序和并行）链接在一起而不会迷失方向，从而大大提高了端到端执行复杂的实际任务的能力。它还能更精确地遵循工具指令，更好地处理工具错误，并且擅长长上下文内容检索。Manus 表示，`GPT-5`“在内部基准测试中取得了迄今为止单个模型的最佳性能”。Notion 表示：“该模型的快速响应，尤其是在低推理模式下，使 `GPT-5` 成为需要一次性解决复杂任务的理想模型。” Inditex 分享道：“`GPT-5` 的真正与众不同之处在于其推理的深度：细致入微、多层次的答案反映了对真实主题的理解。”

OpenAI在 API 中引入了新功能，使开发者能够更好地控制模型响应。`GPT-5` 支持新的 `verbosity` 参数（取值：`low`、`medium`、`high`），以帮助控制答案是简短扼要还是详尽全面。`GPT-5` 的 `reasoning_effort` 参数现在可以采用最小值，以便更快地返回答案，而无需事先进行大量推理。还添加了一个新的工具类型——自定义工具，允许 `GPT-5` 使用纯文本而非 JSON 调用工具。自定义工具支持通过开发者提供的上下文无关语法进行约束。

在 API 中发布三种规格的 `GPT-5`——`gpt-5`、`gpt-5-mini` 和 `gpt-5-nano` 以便开发者能够更灵活地权衡性能、成本和延迟。ChatGPT 中的 `GPT-5` 是一个包含推理、非推理和路由器模型的系统，而 API 平台中的 `GPT-5` 则是为 ChatGPT 提供最高性能的推理模型。值得注意的是，具有最低推理能力的 `GPT-5` 与 ChatGPT 中的非推理模型不同，并且针对开发者进行了更优化。ChatGPT 中使用的非推理模型的版本号为 `gpt-5-chat-latest`。

### 1、GPT-5 编程Coding

`GPT-5` 是OpenAI迄今为止发布的最强大的编程模型。它在编程基准测试和实际用例中的表现均优于 `o3`，并且经过微调，在 Cursor、Windsurf、GitHub Copilot 和 Codex CLI 等agentic coding产品中表现出色。`GPT-5`在许多内部评估中创下了纪录。

#### SWE-bench Verified

在基于真实世界软件工程任务的评估 SWE-bench Verified 中，`GPT-5` 的得分为 74.9%，高于 `o3` 的 69.1%。值得注意的是，`GPT-5` 以更高的效率和速度获得了高分：相对于高推理工作量的 `o3`，`GPT-5` 使用的输出标记减少了 22%，工具调用减少了 45%。

![](img/70684412dea5ac11bac75b2f52984090.png)

> 在 SWE-bench Verified 中，模型会被赋予一个代码库和问题描述，并且必须生成一个补丁来解决问题。横坐标表示推理工作量。此次评分忽略了23 个问题（共500个问题），这些问题的解决方案在我们的基础设施上无法可靠地通过。`GPT-5` 收到了一个简短的提示，强调彻底验证解决方案；同样的提示并没有在 `o3` 上获得比较好的结果。

#### Aider polyglot

在代码编辑评估 Aider polyglot 上，`GPT-5` 创下了 88% 的新纪录，与 `o3` 相比错误率降低了三分之一。

![](img/4c2a9f782ab0a382efd94c67ae12e25d.png)

> 在 Aider polygot (diff) 中，模型会收到来自 Exercism 的编码练习，并必须将其答案写成代码 `diff`。推理模型的运行需要付出巨大的努力程度。

此外，`GPT-5` 非常擅长深入挖掘代码库，以解答各个部分如何工作或互操作的问题。在像 OpenAI 强化学习堆栈这样复杂的代码库中，`GPT-5` 可以帮助工程师推理和解答有关代码的问题，从而加速工程师的日常工作。

### 2、GPT-5 前端工程能力

在为 Web 应用编写前端代码时，`GPT-5` 更注重美观、更雄心勃勃且更准确。以下是 `GPT-5` 通过单个提示即可完成的一些有趣且精选的示例：

#### （1）网站生成

![](img/0a627b74a7dedb521b21b7e6ede06978.png)

> Prompt: Please generate a beautiful, realistic landing page for a service that provides the ultimate coffee enthusiast a $200/month subscription that provides equipment rental and coaching for coffee roasting and creating the ultimate espresso. The target audience is a bay area middle-aged person who might work in tech and is educated, has disposable income, and is passionate about the art and science of coffee. Optimize for conversion for a 6 month signup.

#### （2）应用生成

![](img/138d6303c309d49a0119dd0883c1c088.png)

> Prompt: Create a single-page app in a single HTML file with following requirements:

+ Name: Audio Step Sequencer
+ Stack: `WebAudio API`.
+ Goal: 16-step drum grid.
+ Features: Tempo, swing, patterns save/load, export WAV (render simple buffer).
+ The UI should be futuristic and make the play fun!

#### （3）游戏页面生成

![](img/86a6a6eee3f482343fc7e032b291b20d.png)

> Prompt: Make a 2d space game, in which I can fly a ship, avoid and blow up asteroids, and dogfight with other computer-controlled AI. Be creative with the design of the ships. Ensure the gameplay works and is fun. Output code in a single next.js `page.tsx` file, which can be pasted directly into a next.js app created by `create-next-app`, alongside any context or instructions needed to run it.

### 3、编程协作

`GPT-5` 的协作能力更佳，尤其是在 Cursor、Windsurf、GitHub Copilot 和 Codex CLI 等代理编码产品中。`GPT-5` 在工作时，可以在工具调用之间输出计划、更新和摘要。与OpenAI过去的模型相比，`GPT-5` 在完成雄心勃勃的任务时更加积极主动，不会因为用户同意而暂停，也不会因为高复杂性而犹豫不决。

以下是 `GPT-5` 在处理复杂任务（在本例中为餐厅创建网站）时的示例：

![](img/e0386d60b5099ad0efa86ca06d06b321.png)

> 当用户请求为其餐厅创建网站后，`GPT-5` 会分享一个快速计划，搭建应用框架，安装依赖项，创建网站内容，运行构建检查编译错误，总结工作内容，并提出可能的后续步骤。此视频已加速约 3 倍，以节省您的等待时间；创建网站的全部时间约为三分钟。

### 4、Agent能力

除了代理编程之外，`GPT-5` 在代理任务方面也普遍表现更佳。`GPT-5` 在指令遵循（Scale MultiChallenge 上 69.6%，由 `o3-mini` 评分）和工具调用（τ (2) -bench telecom 上 96.7%）的基准测试中创下了新纪录。工具智能的提升使 `GPT-5` 能够更可靠地将操作串联起来，从而完成现实世界的任务。

#### （1）指令遵循能力Instruction following

`GPT-5` 比其任何前辈都更可靠地遵循指令，在 COLLIE、Scale MultiChallenge 和OpenAI的内部指令评估中得分很高。

![](img/0e41ba10f2ddc9dfe85cfaeb3bf2f25c.png)

> 在 COLLIE 中，模型必须编写满足各种约束的文本。在 Scale MultiChallenge 中，模型在多轮对话中接受挑战，需要正确运用先前消息中的四种信息。此分数来自使用 `o3-mini` 作为评分器，它比 `GPT-4o` 更准确。在内部的 OpenAI API 指令中，在评估之后，模型必须遵循来自真实开发者反馈的复杂指令。推理模型的运行需要付出很大的推理努力。

#### （2）工具调用Tool calling

OpenAI声称努力以开发者关注的方式改进工具调用。`GPT-5` 更擅长遵循工具指令，更擅长处理工具错误，并且更擅长主动地按顺序或并行进行多次工具调用。根据指示，`GPT-5` 还可以在工具调用之前和调用之间输出前导消息，以便在较长的代理任务期间向用户更新进度。

两个月前，Sierra.ai 发布了 τ (2) -bench telecom，这是一项颇具挑战性的工具使用基准测试，该基准测试强调了语言模型在与用户可更改的环境状态交互时性能会显著下降。在他们的论文中，没有一个模型的得分超过 49%。而 `GPT-5` 的得分高达 97%。

![](img/f2e12461f57331e2b17ac15fd3ef4245.png)

> 在τ2-bench 中，模型必须使用工具来完成客户服务任务，其中可能存在能够进行沟通并根据世界状态采取行动的用户。推理模型的运行需要付出很大的推理努力。

#### （3）长上下文性能

`GPT-5` 在长上下文性能方面也表现出显著的提升。在 OpenAI-MRCR（一种衡量长上下文信息检索的指标）上，`GPT-5` 的表现优于 `o3` 和 `GPT-4.1`，且在输入长度较长的情况下，差距显著扩大。

![](img/f0f82b661e6f61c62b2cc95fff8ea1d3.png)

> 在 OpenAI-MRCR（多轮共指消解）中，多个相同的“针状”用户请求被插入到类似请求和响应的长长“干草堆”中，并要求模型重现对第 i 个针状的响应。平均匹配率衡量模型响应与正确答案之间的平均字符串匹配率。最大输入标记数为 256k 的点代表 128k 到 256k 个输入标记的平均值，依此类推。此处，256k 代表 `256 * 1,024 = 262,114` 个标记。推理模型以较高的推理工作量运行。

此外，OpenAI还开源了 BrowseComp Long Context，这是一个用于评估长上下文问答的新基准。在此基准测试中，模型会收到一个用户查询和一长串相关搜索结果，并且必须根据搜索结果回答问题。我们将 BrowseComp Long Context 设计得切合实际、难度适中，并能提供可靠、正确的基本事实答案。对于 128K 到 256K 个 token 的输入，`GPT-5` 的正确答案率高达 89%。

在 API 中，所有 `GPT-5` 模型最多可以接受 272,000 个输入token，最多可以发出 128,000 个推理和输出token，总上下文长度为 400,000 个token。

### 5、事实性

`GPT-5` 比OpenAI之前的模型更值得信赖。在 LongFact 和 FactScore 基准测试中，`GPT-5` 的事实错误比 `o3` 少了约 80%。这使得它更适合那些正确性至关重要的代理用例——尤其是在代码、数据和决策方面。

![](img/cf68aba7ca75d5e069d596178f5daa3f.png)

> 分数越高，成绩越差。LongFact 和 FactScore 包含开放式事实探究问题。使用基于大模型 (LLM) 的评分系统，并结合浏览功能，根据这些基准的提示对答案进行事实核查，并衡量事实错误陈述的比例。推理模型的推理工作量较大。搜索功能未启用。

总体而言，`GPT-5` 经过训练后，能够更好地了解自身的局限性，并更好地应对突发状况。此外，OpenAI还训练 `GPT-5` 在健康问题上更加准确。

![](img/cc0fb3f3c95b8d1262d8347eaa2b18d5.png)

## 二、GPT-5 新功能

### 1、最小推理努力

开发者可以通过 API 中的 `reasoning_effort` 参数控制 `GPT-5` 的思考时间。除了先前的值（`low`、`medium`（默认值）和 `high`）之外，`GPT-5` 还支持 `minimal`，这将最大限度地减少 `GPT-5` 的推理时间，以便快速返回答案。

较高的 `reasoning_effort` 值可最大化质量，较低的值可最大化速度。并非所有任务都能从额外的推理中获益，因此具体使用哪种，得看实际应用。

例如，`low` 以上推理对相对简单的长上下文检索帮助不大，但对视觉推理基准 CharXiv Reasoning 却增加了不少个百分点。

![](img/1cc6cf8d4103acb595b4e836df0f8be2.png)

> `GPT-5` 的推理能力在不同任务上产生了不同的效果。在 CharXiv 推理中，`GPT-5` 被赋予了 Python 工具的使用权限。

### 2、模型回答长度

为了帮助控制 `GPT-5` 答案的默认长度，OpenAI引入了一个新的 API 参数 `verbosity`，其取值包括 `low`、`medium`（默认值）和 `high`。如果明确的指示与详细程度参数冲突，则以明确的指示为准。例如，如果你要求 `GPT-5`“写一篇 5 段文章”，则无论详细程度如何，模型的响应都应始终为 5 段（但是，段落本身可能会更长或更短）。

+ 示例

Prompt：“天空为什么是蓝色的？

+ `Verbosity=low`

这是瑞利散射造成的。空气分子比可见光的波长小得多，因此它们散射的短波长（蓝光/紫光）远多于长波长（红光）。散射的蓝光从四面八方射入人眼，使天空呈现蓝色。我们之所以看不到那么多紫光，是因为太阳辐射的紫光较少，我们的眼睛对紫光的敏感度较低，而且一部分紫光被高层大气吸收了。日出日落时，空气中较长的路径会带走更多的蓝光，使天空呈现微红色。

+ `Verbosity=medium`

这是因为瑞利散射。阳光包含所有颜色，但空气分子对短波长（蓝/紫）光的散射比对长波长（红）光的散射强得多。散射强度大约为波长的1/4，因此蓝光会穿过天空，进入你的眼睛。

我们看不到紫色的天空是因为：

+ 太阳发出的紫光比蓝光略少。

+ 高层大气吸收了一些紫色/紫外线。

+ 人眼对紫色不太敏感。

在日出和日落时，阳光穿过更多的大气层，散射出沿途的大部分蓝色光，只留下较长的红色/橙色波长占据主导地位。

+ `Verbosity=high`

天空之所以呈现蓝色，是因为阳光被地球大气中的微小分子散射。这个过程称为瑞利散射。

要点：

+ 阳光由多种颜色（波长）组成。波长较短的光，例如紫色和蓝色，在空气分子中散射的强度远高于波长较长的光，例如红色。波长越短，散射强度就越大。

+ 当阳光穿过大气层时，波长较短的光（尤其是蓝光）会向四面八方散射，因此无论你将目光从太阳移开，你都会看到散射的蓝光——“蓝天”。

为什么不是紫色？

+ 太阳发出的紫光比蓝光少。

+ 我们的眼睛对紫色不太敏感。

+ 一些紫光/紫外线被较高处的臭氧吸收。

综合起来，我们感知到的散射光主要呈现蓝色。

为什么日落是红色/橙色的？

+ 在日出和日落时，阳光穿过大气层的路径要长得多。

+ 大部分蓝光在到达您的眼睛之前就从直射路径中散射出去，剩下的直射阳光则富含红色和橙色。

其他影响：



## 一、大气光学与天空颜色

+ 霾、污染、烟雾或灰尘含有较大的颗粒，它们散射光的选择性较低（称为米氏散射），使天空看起来更苍白或发白。

+ 由于散射几何的原因，蓝天被部分偏振，这就是偏光太阳镜可以使部分天空变暗的原因。

+ 在具有不同大气层和粒子大小的星球上（例如火星），由于类似的散射原因，天空可能呈现奶油糖色或微红色。

如果地球没有大气层，就没有东西散射阳光，即使在白天，天空也会呈现黑色，而太阳只是一个明亮的圆盘。

## 二、GPT-5 工具与功能

### 3、工具调用前的前导消息

如果需要，GPT-5 将在工具调用之前和调用之间输出用户可见的前导消息。与隐藏的推理消息不同，这些可见消息允许 GPT-5 向用户传达计划和进度，帮助最终用户理解其工具调用背后的方法和意图。

### 4、自定义工具

OpenAI推出了一种新的工具类型——自定义工具，它允许 GPT-5 使用纯文本（而非 JSON）来调用工具。为了限制 GPT-5 遵循自定义工具格式，开发者可以提供正则表达式，甚至是更完整指定的上下文无关语法。

以前，OpenAI为开发者自定义工具提供的接口要求使用 JSON 进行调用，JSON 是 Web API 和开发者普遍使用的通用格式。然而，输出有效的 JSON 需要模型完美转义所有引号、反斜杠、换行符和其他控制字符。尽管我们的模型训练有素，能够输出 JSON，但在处理数百行代码或 5 页报告等长输入时，出错的几率会逐渐增加。使用自定义工具时，GPT-5 可以将工具输入写入纯文本，而无需转义所有需要转义的字符。

在 SWE-bench Verified 上使用自定义工具而不是 JSON 工具，GPT-5 的得分大致相同。

## 三、GPT-5的安全性

GPT-5 在安全性方面取得了突破，是一个更加稳健、可靠且实用的模型。与OpenAI之前的模型相比，GPT-5 出现幻觉的可能性显著降低，能够更诚实地向用户传达其行为和能力，并在不超出安全界限的情况下尽可能提供最有用的答案。

## 四、GPT-5的使用方法和价格

GPT-5 目前在 API 平台上提供三种规格：`gpt-5`、`gpt-5-mini` 和 `gpt-5-nano`。它可在 Responses API 和 Chat Completions API 上使用，并且是 Codex CLI 中的默认规格。GPT-5 的售价为每 100 万个输入token 1.25 美元，每 100 万个输出token 10 美元；`GPT-5 mini` 的售价为每 100 万个输入token 0.25 美元，每 100 万个输出token 2 美元；`GPT-5 nano` 的售价为每 100 万个输入token 0.05 美元，每 100 万个输出token 0.40 美元。

这些模型支持 `reasoning_effort` 和 `verbosity` API 参数以及自定义工具。它们还支持并行工具调用、内置工具（网页搜索、文件搜索、图像生成等）、核心 API 功能（流式传输、结构化输出等），以及快速缓存和 Batch API 等节省成本的功能。

ChatGPT 中使用的 GPT-5 非推理版本在 API 中以 `gpt-5-chat-latest` 形式提供，价格为 1.25 美元/100 万个输入token和 10 美元/100 万个输出token。

![](img/b9d8bfa1fe592ea6222620af78dbffd4.png)

GPT-5 还将在 Microsoft 平台上推出，包括 Microsoft 365 Copilot、Copilot、GitHub Copilot 和 Azure AI Foundry。

## 五、GPT-5基准测试

### 1、智力

![](img/b9af0fc435829b398ce752bf7007f468.png)

### 2、多模态

![](img/8c5d9ec9e4d6ed0cd030b37527e9459b.png)

### 3、编程Coding

![](img/c217bb2209c1b5fd4f04188f92ccaf6c.png)

### 4、指令跟随能力

![](img/a76f0599ec3d19e5a2a02deb6ae9b8c6.png)

### 5、工具调用能力

![](img/0ad8378cb1641ca94dc5a791c039d369.png)

### 6、长上下文能力

![](img/efb55b9918bc1d4bc67b2664c506d528.png)

### 7、模型幻觉

![](img/94d8068e6c79d6bf61429368544d1ab1.png)

## 六、GPT-5 提示指南

在本指南中，将介绍一些提示技巧，以最大程度地提高模型输出的质量，这些技巧源于OpenAI训练和将模型应用于实际任务的经验。此外，也会讨论一些概念，例如提升代理任务性能、确保指令遵循性、利用新的 API 功能以及优化前端和软件工程任务的编码，并深入探讨 AI 代码编辑器 Cursor 与 GPT-5 的快速调优工作。

OpenAI表示，通过应用这些最佳实践获得[了还不错的结果](http://platform.openai.com/chat/edit?optimize=true)。希望本指南以及这里构建的提示优化器工具能够成为你使用 GPT-5 的启动板。但是，请记住，提示并非万能的——希望你多尝试，并在此提供的基础上不断迭代，以找到适合你问题的最佳解决方案。

### 1、代理工作流可预测性

#### 1.1 控制代理积极性

在训[练 GPT-5 时始终以开](https://platform.openai.com/docs/api-reference/responses)发者为中心：专注于改进工具调用、指令遵循和长上下文理解，使其成为代理应用的最佳基础模型。如果你正在采用 GPT-5 进行代理和工具调用流程，建议升级到 Responses API ，该 API 可在工具调用之间持久化推理，从而实现更高效、更智能的输出。

代理工具涵盖的范围可以很广——有些系统将绝大多数决策委托给底层模型，而另一些系统则通过繁琐的程序化逻辑分支严格控制模型。GPT-5 经过训练，可以在此范围内运行，从在模糊情况下做出高层决策，到处理重点明确、定义明确的任务。在本节中，将介绍如何最好地校准 GPT-5 的代理积极性：换句话说，如何在主动性和等待明确指导之间取得平衡。

+ **鼓励不那么热切**

默认情况下，GPT-5 在代理环境中尝试收集上下文以确保其能够生成正确答案时，是全面且全面的。为了缩小 GPT-5 代理行为的范围（包括限制间接工具调用操作并最大限度地减少获得最终答案的延迟），请尝试以下方法：

+ 切换到较低的 `reasoning_effort` 。这会降低探索深度，但会提高效率并减少延迟。许多工作流程在中等甚至较低的 `reasoning_effort` 下也能完成，并取得一致的结果。

+ 在提示中定义清晰的标准，说明你希望模型如何探索问题空间。这可以减少模型探索和推理过多想法的需要：

```
<context_gathering>
Goal: Get enough context fast. Parallelize discovery and stop as soon as you can act.

Method:
- Start broad, then fan out to focused subqueries.
- In parallel, launch varied queries; read top hits per query. Deduplicate paths and cache; don’t repeat queries.
- Avoid over searching for context. If needed, run targeted searches in one parallel batch.

Early stop criteria:
- You can name exact content to change.
- Top hits converge (~70%) on one area/path.

Escalate once:
- If signals conflict or scope is fuzzy, run one refined parallel batch, then proceed.

Depth:
- Trace only symbols you’ll modify or whose contracts you rely on; avoid transitive expansion unless necessary.

Loop:
- Batch search → minimal plan → complete task.
- Search again only if validation fails or new unknowns appear. Prefer acting over more searching.
</context_gathering>
```

如果你想最大限度地规范，甚至可以设置固定的工具调用预算，如下所示。预算可以根据你所需的搜索深度自然变化。

```
<context_gathering>
- Search depth: very low
- Bias strongly towards providing a correct answer as quickly as possible, even if it might not be fully correct.
- Usually, this means an absolute maximum of 2 tool calls.
- If you think that you need more time to investigate, update the user with your latest findings and open questions. You can proceed if the user confirms.
</context_gathering>
```

在限制核心上下文收集行为时，明确地为模型提供一个“出口”，使其更容易满足较短的上下文收集步骤会很有帮助。这通常以子句的形式出现，允许模型在不确定的情况下继续运行，例如上例中的 “even if it might not be fully correct” 。

+ **激发更多热情**

如果你希望鼓励模型自主性，增加工具调用的持久性，并减少澄清问题或交还给用户的次数，建议增加 `reasoning_effort` ，并使用如下提示来鼓励坚持和彻底完成任务：

```
<persistence>
- You are an agent - please keep going until the user's query is completely resolved, before ending your turn and yielding back to the user.
- Only terminate your turn when you are sure that the problem is solved.
- Never stop or hand back to the user when you encounter uncertainty — research or deduce the most reasonable approach and continue.
- Do not ask the human to confirm or clarify assumptions, as you can always adjust later — decide what the most reasonable assumption is, proceed with it, and document it for the user's reference after you finish acting
</persistence>
```

一般来说，清晰地说明代理任务的停止条件、概述安全操作和不安全操作，并定义何时（如果有的话）模型可以交还给用户，会很有帮助。例如，在一组购物工具中，结账和支付工具应明确设置较低的不确定性阈值，以便用户进行澄清，而搜索工具则应设置极高的阈值；同样，在编码设置中，删除文件工具的阈值应比 `grep` 搜索工具低得多。

#### 1.2 工具前言

在用户监控的代理轨迹上，间歇性地更新模型对其工具调用的操作及其原因，可以提供更佳的交互用户体验——推出时间越长，这些更新带来的改进就越显著。为此，GPT-5 经过训练，可以通过“工具前言”消息提供清晰的前期计划和持续的进度更新。

你可以控制提示中工具前言的频率、样式和内容——从每个工具调用的详细解释到简要的前期计划，以及介于两者之间的所有内容。这是一个高质量前言提示的示例：

```
<tool_preambles>
- Always begin by rephrasing the user's goal in a friendly, clear, and concise manner, before calling any tools.
- Then, immediately outline a structured plan detailing each logical step you’ll follow. - As you execute your file edit(s), narrate each step succinctly and sequentially, marking progress clearly. 
- Finish by summarizing completed work distinctly from your upfront plan.
</tool_preambles>
```

下面是响应此类提示时可能发出的工具前言示例 - 此类前言可以大大提高用户跟随代理工作的能力，因为代理的工作变得越来越复杂：

```
{
  "output": [
    {
      "id": "rs_6888f6d0606c819aa8205ecee386963f0e683233d39188e7",
      "type": "reasoning",
      "summary": [
        {
          "type": "summary_text",
          "text": "**Determining weather response**

I need to answer the user's question about the weather in San Francisco. ...."
        },
    },
    {
      "id": "msg_6888f6d83acc819a978b51e772f0a5f40e683233d39188e7",
      "type": "message",
      "status": "completed",
      "content": [
        {
          "type": "output_text",
          "text": "I’m going to check a live weather service to get the current conditions in San Francisco, providing the temperature in both Fahrenheit and Celsius so it matches your preference."
        }
      ],
      "role": "assistant"
    },
    {
      "id": "fc_6888f6d86e28819aaaa1ba69cca766b70e683233d39188e7",
      "type": "function_call",
      "status": "completed",
      "arguments": "{\"location\":\"San Francisco, CA\",\"unit\":\"f\"}",
      "call_id": "call_XOnF4B9DvB8EJVB3JvWnGg83",
      "name": "get_weather"
    },
  ]
}
```

#### 1.3 推理努力

OpenAI提供了一个 `reasoning_effort` 参数来控制模型思考的难度以及它调用工具的意愿；默认值为 `medium` ，但你应该根据任务的难度进行调整。对于复杂的多步骤任务，建议使用更高级的推理以确保获得最佳输出。此外，当不同的、可分离的任务被分解到多个代理回合中，每个任务一个回合时，可以观察到最佳性能。使用 Responses API 重用推理上下文，强烈建议在使用 GPT-5 时使用 Responses API，以在你的应用程序中解锁改进的代理流程、降低成本和更高效的token使用。



## 1.4 使用 Responses API 再利用推理上下文

相比使用聊天完成 API，使用 Responses API 进行评估时，统计结果显著提升。例如，只需切换到 Responses API 并添加 `previous_response_id` 参数，将之前的推理项传递到后续请求中，Taubench-Retail 得分就从 73.9% 提升至 78.2%。这使得模型能够参考之前的推理轨迹，节省 CoT token，并消除每次工具调用后从头构建计划的需要，从而同时降低延迟和性能。此功能适用于所有 Responses API 用户，包括 ZDR 组织。

强烈建议在使用 GPT-5 时使用 Responses API，以解锁应用程序中改进的代理流程、降低成本和更高效的 token 使用。

相比使用聊天完成 API，使用 Responses API 进行评估时，统计结果显著提升。例如，只需切换到 Responses API 并添加 `previous_response_id` 参数，将之前的推理项传递到后续请求中，Tau-Bench Retail 得分就从 73.9% 提升到了 78.2%。这使得模型能够参考之前的推理轨迹，节省 CoT token，并消除每次工具调用后从头构建计划的需要，从而同时降低延迟和性能。此功能适用于所有 Responses API 用户，包括 ZDR 组织。

## 2 从规划到执行，最大化编程性能

GPT-5 在编程能力方面领先于所有前沿模型：它可以在大型代码库中工作，修复错误、处理大量差异，并实现多文件重构或大型新功能。它还擅长从头开始构建新应用，涵盖前端和后端实现。在本节中，我们将讨论在生产用例中为编码代理客户提升编程性能的即时优化。

### 2.1 前端应用程序开发

GPT-5 经过训练，拥有出色的基础审美品味和严谨的实现能力。OpenAI 表示，相信它能够使用各种类型的 Web 开发框架和软件包；但是，对于新应用，还是建议使用以下框架和软件包，以充分利用该模型的前端功能：

+ 框架：Next.js（TypeScript）、React、HTML

+ UI：Tailwind CSS、shadcn/ui、Radix Themes

+ 图标：Material Symbols、Heroicons、Lucide

+ 动画：Motion

+ 字体：San Serif、Inter、Geist、Mona Sans、IBM Plex Sans、Manrope

#### 从零到一的应用生成

GPT-5 擅长一次性构建应用程序。在该模型的早期实验中，用户发现，类似下面的提示——要求模型根据自行构建的卓越标准进行迭代执行——能够利用 GPT-5 的周密计划和自我反思能力来提升输出质量。

```
<self_reflection>
- First, spend time is not hitting the top marks across all categories in the rubric, you need to start again.
</self_reflection>
```

#### 匹配代码库设计标准

在现有应用中实施增量更改和重构时，模型编写的代码应遵循现有的样式和设计标准，并尽可能整齐地“融入”代码库。无需特殊提示，GPT-5 已经可以从代码库中搜索参考上下文（例如，读取 `package.json` 以查看已安装的软件包），但这种行为可以通过提示说明进一步增强，这些提示说明总结了代码库的关键方面，例如工程原则、目录结构和最佳实践（包括显式和隐式）。下面的提示片段演示了组织 GPT-5 代码编辑规则的一种方法：你可以根据自己的编程设计喜好随意更改规则的实际内容！

```
<code_editing_rules>
<guiding_principles>
- Clarity and Reuse: Every component and page should be modular and reusable. Avoid duplication by factoring repeated UI patterns into components.
- Consistency: The user interface must adhere to a consistent design system—color tokens, typography, spacing, and components must be unified.
- Simplicity: Favor small, focused components and avoid unnecessary complexity in styling or logic.
- Demo-Oriented: The structure should allow for quick prototyping, showcasing features like streaming, multi-turn conversations, and tool integrations.
- Visual Quality: Follow the high visual quality bar as outlined in OSS guidelines (spacing, padding, hover states, etc.)
</guiding_principles>

<frontend_stack_defaults>
- Framework: Next.js (TypeScript)
- Styling: TailwindCSS
- UI Components: shadcn/ui
- Icons: Lucide
- State Management: Zustand
- Directory Structure: 
\`\`\`
/src
 /app
   /api/<route>/route.ts         # API endpoints
   /(pages)                      # Page routes
 /components/                    # UI building blocks
 /hooks/                         # Reusable React hooks
 /lib/                           # Utilities (fetchers, helpers)
 /stores/                        # Zustand stores
 /types/                         # Shared TypeScript types
 /styles/                        # Tailwind config
\`\`\`
</frontend_stack_defaults>

<ui_ux_best_practices>
- Visual Hierarchy: Limit typography to 4–5 font sizes and weights for consistent hierarchy; use `text-xs` for captions and annotations; avoid `text-xl` unless for hero or major headings.
- Color Usage: Use 1 neutral base (e.g., `zinc`) and up to 2 accent colors. 
- Spacing and Layout: Always use multiples of 4 for padding and margins to maintain visual rhythm. Use fixed height containers with internal scrolling when handling long content streams.
- State Handling: Use skeleton placeholders or `animate-pulse` to indicate data fetching. Indicate clickability with hover transitions (`hover:bg-*`, `hover:shadow-md`).
- Accessibility: Use semantic HTML and ARIA roles where appropriate. Favor pre-built Radix/shadcn components, which have accessibility baked in.
</ui_ux_best_practices>

</code_editing_rules>
```

### 2.2 生产中的协作编码：Cursor的GPT-5提示调整

下文将展示 Cursor 如何调整其提示，以最大限度地发挥模型的功能。欲了解更多信息，请参阅 Cursor 团队发布的一篇博客文章，详细介绍了 GPT-5 与 Cursor 的首日集成： https://cursor.com/blog/gpt-5

#### 系统提示与参数调整

Cursor 的系统提示注重可靠的工具调用，在冗长和自主行为之间取得平衡，同时允许用户配置自定义指令。Cursor 的系统提示的目标是让 Agent 在执行长期任务时能够相对自主地运行，同时仍然忠实地遵循用户提供的指令。

Cursor 团队最初发现，该模型的输出内容冗长，通常包含状态更新和任务后总结，虽然技术层面上相关，但却扰乱了用户的自然阅读流程；同时，工具调用中输出的代码质量很高，但有时由于过于简洁而难以阅读，单字母变量名尤为突出。为了寻求更好的平衡，他们将详细程度 API 参数设置为“低”，以保持文本输出简洁，然后修改了提示，强烈建议仅在编码工具中使用详细输出。

```
Write code for clarity first. Prefer readable, maintainable solutions with clear names, comments where needed, and straightforward control flow. Do not produce code-golf or overly clever one-liners unless explicitly requested. Use high verbosity for writing code and code tools.
```

参数和提示的双重使用产生了一种平衡的格式，结合了高效、简洁的状态更新和最终工作摘要以及更易读的代码差异。

Cursor 还发现，模型偶尔会先向用户寻求澄清或后续步骤，然后再采取行动，这在执行较长的任务时造成了不必要的摩擦。为了解决这个问题，他们发现，不仅要包含可用的工具和周围环境，还要包含更多关于产品行为的细节，这有助于模型以最少的干扰和更高的自主性执行较长的任务。突出显示 Cursor 功能的具体细节（例如撤消/拒绝代码和用户偏好设置），通过明确说明 GPT-5 在其环境中的行为方式，有助于减少歧义。对于较长的任务，他们发现这种提示可以提高性能：

```
Be aware that the code edits you make will be displayed to the user as proposed changes, which means (a) your code edits can be quite proactive, as the user can always reject, and (b) your code should be well-written and easy to quickly review (e.g., appropriate variable names instead of single letters). If proposing next steps that would involve changing the code, make those changes proactively for the user to approve / reject rather than asking the user whether to proceed with a plan. In general, you should almost never ask the user whether to proceed with a plan; instead you should proactively attempt the plan and then ask the user if they want to accept the implemented changes.
```

Cursor 发现，他们的提示中一些在早期模型中有效的部分需要进行调整，才能最大限度地发挥 GPT-5 的作用。以下是一个例子：

```
<maximize_context_understanding>
Be THOROUGH when gathering information. Make sure you have the FULL picture before replying. Use additional tool calls or clarifying questions as needed.
...
</maximize_context_understanding>
```

虽然这种方法对需要鼓励才能彻底分析上下文的旧模型很有效，但他们发现，这种方法对 GPT-5 适得其反，因为 GPT-5 本身就具有内省和主动收集上下文的能力。在较小的任务中，这种提示常常导致模型过度使用工具，反复调用搜索功能，而实际上内部知识本身就足够了。

为了解决这个问题，他们改进了提示符，删除了“maximize_”前缀，并围绕全面性弱化了语言。调整后的指令实施后，Cursor 团队发现 GPT-5 能够更好地决定何时依赖内部知识，何时使用外部工具。它保持了高度的自主性，无需使用不必要的工具，从而实现了更高效、更相关的行为。在 Cursor 的测试中，使用结构化 XML 规范（例如 `<[instruction]_spec>`）提高了提示符的指令遵循性，并使其能够在提示符的其他位置清晰地引用先前的类别和章节。

```
<context_understanding>
...
If you&#x27;ve performed an edit that may partially fulfill the USER&#x27;s query, but you&#x27;re not confident, gather more information or use more tools before ending your turn.
Bias towards not asking the user for help if you can find the answer yourself.
</context_understanding>
```

虽然系统提示提供了强大的默认基础（https://docs.cursor.com/en/context/rules），但用户提示仍然是提高可操作性的高效杠杆。GPT-5 对直接明确的指令反应良好，Cursor 团队始终认为结构化、范围明确的提示能够产生最可靠的结果。这包括冗长程度控制、主观代码风格偏好以及对边缘情况的敏感度等方面。Cursor 发现，允许用户配置自定义 Cursor 规则与 GPT-5 改进的可操作性息息相关，从而为用户提供更加个性化的体验。

## 3 优化智力和指令遵循能力

作为迄今为止最具可操作性的模型，GPT-5 对有关详细程度、语气和工具调用行为的提示指令具有极强的接受能力。

### 3.1 模型冗长性

除了能够像之前的推理模型一样控制推理强度 (`reasoning_effort`) 之外，在 GPT-5 中引入了一个名为“verbosity”的新 API 参数，它影响模型最终答案的长度，而不是其思考的长度。但在本指南中，想强调的是，虽然 API 的 `verbosity` 参数是本次发布的默认参数，但 GPT-5 经过训练，能够在特定情境下响应提示中自然语言的详细程度覆盖，在这些情境下，你可能希望模型偏离全局默认值。上文 Cursor 的示例是全局设置低详细程度，然后仅为编码工具指定高详细程度，这就是此类情境的一个典型示例。

### 3.2 指令遵循

与 GPT-4.1 一样，GPT-5 能够精准地执行提示指令，这使得它能够灵活地融入各种类型的工作流程。然而，由于 GPT-5 遵循指令的谨慎行为，包含矛盾或模糊指令的不良提示对 GPT-5 的损害可能大于其他模型，因为它会消耗推理 token 来寻找协调矛盾的方法，而不是随机选择一条指令。

下面，给出一个对抗性的例子，该例子经常会损害 GPT-5 的推理轨迹——虽然乍一看它可能看起来内部一致，但仔细检查就会发现关于预约安排的指令存在冲突：



# GPT-5 提示工程与指令优化指南

## 指令层次冲突与解决

+ Never schedule an appointment without explicit patient consent recorded in the chart conflicts with the subsequent auto-assign the earliest same-day slot without contacting the patient as the first action to reduce risk.

+ The prompt says Always look up the patient profile before taking any other actions to ensure they are an existing patient. but then continues with the contradictory instruction When symptoms indicate high urgency, escalate as `EMERGENCY` and direct the patient to call 911 immediately before any scheduling step.

```
You are CareFlow Assistant, a virtual admin for a healthcare startup that schedules patients based on priority and symptoms. Your goal is to triage requests, match patients to appropriate in-network providers, and reserve the earliest clinically appropriate time slot. Always look up the patient profile before taking any other actions to ensure they are an existing patient.

- Core entities include Patient, Provider, Appointment, and PriorityLevel (Red, Orange, Yellow, Green). Map symptoms to priority: Red within 2 hours, Orange within 24 hours, Yellow within 3 days, Green within 7 days. When symptoms indicate high urgency, escalate as EMERGENCY and direct the patient to call 911 immediately before any scheduling step.
+Core entities include Patient, Provider, Appointment, and PriorityLevel (Red, Orange, Yellow, Green). Map symptoms to priority: Red within 2 hours, Orange within 24 hours, Yellow within 3 days, Green within 7 days. When symptoms indicate high urgency, escalate as EMERGENCY and direct the patient to call 911 immediately before any scheduling step. 
*Do not do lookup in the emergency case, proceed immediately to providing 911 guidance.*

- Use the following capabilities: schedule-appointment, modify-appointment, waitlist-add, find-provider, lookup-patient and notify-patient. Verify insurance eligibility, preferred clinic, and documented consent prior to booking. Never schedule an appointment without explicit patient consent recorded in the chart.

- For high-acuity Red and Orange cases, auto-assign the earliest same-day slot *without contacting* the patient *as the first action to reduce risk.* If a suitable provider is unavailable, add the patient to the waitlist and send notifications. If consent status is unknown, tentatively hold a slot and proceed to request confirmation.

- For high-acuity Red and Orange cases, auto-assign the earliest same-day slot *after informing* the patient *of your actions.* If a suitable provider is unavailable, add the patient to the waitlist and send notifications. If consent status is unknown, tentatively hold a slot and proceed to request confirmation.
```

通过解决指令层次结构冲突，GPT-5 能够实现更高效、更高性能的推理。通过以下方式解决了这些矛盾：

+ 将自动分配更改为在联系患者后进行，在告知患者你的操作后自动分配当天最早的时间段。与仅经同意安排的时间保持一致。

+ 添加紧急情况下不要查找，立即提供 911 指导。让模型知道在紧急情况下可以不查找。

## 提示构建的迭代过程

[构建提示的过程是一个迭代的过程](https://cookbook.openai.com/examples/gpt-5/platform.openai.com/chat/edit?optimize=true)，许多提示都是动态文档，由不同的利益相关者不断更新——但这更需要彻底审查其中是否存在措辞不当的说明。已经看到多位早期用户在进行此类审查后，发现了其核心提示库中存在歧义和矛盾之处：移除这些内容后，GPT-5 的性能得到了显著提升。建议你使用提示优化工具测试你的提示词，以帮助识别此类问题。

## 3.3 最低限度的推理

在 GPT-5 中，首次引入了最小推理工作量：这是最快的方案，同时仍然保留了推理模型范式的优势。OpenAI认为这对于对延迟敏感的用户以及 GPT-4.1 的现有用户来说，是最佳的升级。

为了获得最佳结果，OpenAI建议使用与 GPT-4.1 类似的提示模式。最低限度的推理性能可能会因提示而比更高的推理水平发生更大的变化，因此需要强调的关键点包括：

+ 提示模型在最终答案的开头给出一个简短的解释来总结其思维过程，例如通过项目符号列表，可以提高需要更高智能的任务的性能。

+ 请求全面且描述性的工具调用前言，不断向用户更新任务进度，从而提高代理工作流程的性能。

+ 尽可能地消除工具指令的歧义并插入如上所述的代理持久性提醒，对于最大限度地提高长期运行中的代理能力并防止过早终止的最低限度推理尤其重要。

+ 同样，提示式规划也更为重要，因为模型进行内部规划所需的推理标记较少。下方，你可以找到放置在代理任务开头的示例规划提示片段：第二段特别确保代理在将任务交还给用户之前完全完成任务及其所有子任务。

```
Remember, you are an agent - please keep going until the user&#x27;s query is completely resolved, before ending your turn and yielding back to the user. Decompose the user&#x27;s query into all required sub-request, and confirm that each is completed. Do not stop after completing only part of the request. Only terminate your turn when you are sure that the problem is solved. You must be prepared to answer multiple queries and only finish the call once the user has confirmed they&#x27;re done.

You must plan extensively in accordance with the workflow steps before making subsequent function calls, and reflect extensively on the outcomes each function call made, ensuring the user&#x27;s query, and related sub-requests are completely resolved.
```

## 3.4 Markdown格式

默认情况下，API 中的 GPT-5 不会将其最终答案格式化为 Markdown 格式，以便最大限度地兼容那些应用程序可能不支持 Markdown 渲染的开发者。不过，类似以下的提示在很大程度上可以成功生成分层的 Markdown 最终答案。

```
- Use Markdown **only where semantically correct** (e.g., `inline code`, ```code fences```, lists, tables).

- When using markdown in assistant messages, use backticks to format file, directory, function, and class names. Use \( and \) for inline math, \[ and \] for block math.

```

有时，在长时间的对话过程中，系统提示中指定的 Markdown 指令的遵循度可能会降低。如果你遇到这种情况，每 3-5 条用户消息附加一条 Markdown 指令，就能始终如一地遵循。

## 3.5 元提示

最后，总结一下元要点：早期测试人员发现，将 GPT-5 用作元提示器非常成功。目前，已有多名用户将快速修订部署到生产环境中，这些修订只需通过询问 GPT-5 即可生成：在不成功的提示中添加哪些元素可以引发期望的行为，或删除哪些元素可以防止出现不良行为。

以下是一个比较好用的 `metaprompt` 模板示例：

```

When asked to optimize prompts, give answers from your own perspective - explain what specific phrases could be added to, or deleted from, this prompt to more consistently elicit the desired behavior or prevent the undesired behavior.

Here&#x27;s a prompt: [PROMPT]

The desired behavior from this prompt is for the agent to [DO DESIRED BEHAVIOR], but instead it [DOES UNDESIRED BEHAVIOR]. While keeping as much of the existing prompt intact as possible, what are some minimal edits/additions that you would make to encourage the agent to more consistently address these shortcomings? 

```

## 4、附录

### SWE-Bench验证开发人员说明

```

In this environment, you can run `bash -lc &lt;apply_patch_command&gt;` to execute a diff/patch against a file, where &lt;apply_patch_command&gt; is a specially formatted apply patch command representing the diff you wish to execute. A valid &lt;apply_patch_command&gt; looks like:

apply_patch &lt;&lt; &#x27;PATCH&#x27;

*** Begin Patch

[YOUR_PATCH]

*** End Patch

PATCH

Where [YOUR_PATCH] is the actual content of your patch.

Always verify your changes extremely thoroughly. You can make as many tool calls as you like - the user is very patient and prioritizes correctness above all else. Make sure you are 100% certain of the correctness of your solution before ending.

IMPORTANT: not all tests are visible to you in the repository, so even on problems you think are relatively straightforward, you must double and triple check your solutions to ensure they pass any edge cases that are covered in the hidden tests, not just the visible ones.

```

### 代理编码工具定义

```

## Set 1: 4 functions, no terminal

type apply_patch = (_: {

patch: string, // default: null

}) =&gt; any;

type read_file = (_: {

path: string, // default: null

line_start?: number, // default: 1

line_end?: number, // default: 20

}) =&gt; any;

type list_files = (_: {

path?: string, // default: &quot;&quot;

depth?: number, // default: 1

}) =&gt; any;

type find_matches = (_: {

query: string, // default: null

path?: string, // default: &quot;&quot;

max_results?: number, // default: 50

}) =&gt; any;

## Set 2: 2 functions, terminal-native

type run = (_: {

command: string[], // default: null

session_id?: string | null, // default: null

working_dir?: string | null, // default: null

ms_timeout?: number | null, // default: null

environment?: object | null, // default: null

run_as_user?: string | null, // default: null

}) =&gt; any;

type send_input = (_: {

session_id: string, // default: null

text: string, // default: null

wait_ms?: number, // default: 100

}) =&gt; any;

```

[正如](https://github.com/openai/openai-cookbook/tree/main/examples/gpt-5/apply_patch.py) GPT-4.1 提示指南中所述，以下是最新的 `apply_patch` 实现：强烈建议使用 `apply_patch` 进行文件编辑，以匹配训练分布。在绝大多数情况下，最新实现应该与 GPT-4.1 实现一致。

### Taubench-Retail最低限度推理指令




As a retail agent, you can help users cancel or modify pending orders, return or exchange delivered orders, modify their default user address, or provide information about their own profile, orders, and related products.

Remember, you are an agent - please keep going until the user’s query is completely resolved, before ending your turn and yielding back to the user. Only terminate your turn when you are sure that the problem is solved.

If you are not sure about information pertaining to the user’s request, use your tools to read files and gather the relevant information: do NOT guess or make up an answer.

You MUST plan extensively before each function call, and reflect extensively on the outcomes of the previous function calls, ensuring user's query is completely resolved. DO NOT do this entire process by making function calls only, as this can impair your ability to solve the problem and think insightfully. In addition, ensure function calls have the correct arguments.

# Workflow steps

- At the beginning of the conversation, you have to authenticate the user identity by locating their user id via email, or via name + zip code. This has to be done even when the user already provides the user id.

- Once the user has been authenticated, you can provide the user with information about order, product, profile information, e.g. help the user look up order id.

- You can only help one user per conversation (but you can handle multiple requests from the same user), and must deny any requests for tasks related to any other user.

- Before taking consequential actions that update the database (cancel, modify, return, exchange), you have to list the action detail and obtain explicit user confirmation (yes) to proceed.

- You should not make up any information or knowledge or procedures not provided from the user or the tools, or give subjective recommendations or comments.

- You should at most make one tool call at a time, and if you take a tool call, you should not respond to the user at the same time. If you respond to the user, you should not make a tool call.

- You should transfer the user to a human agent if and only if the request cannot be handled within the scope of your actions.

## Domain basics

- All times in the database are EST and 24 hour based. For example `02:30:00` means 2:30 AM EST.

- Each user has a profile of its email, default address, user id, and payment methods. Each payment method is either a gift card, a paypal account, or a credit card.

- Our retail store has 50 types of products. For each type of product, there are variant items of different options. For example, for a `t shirt` product, there could be an item with option `color blue size M`, and another item with option `color red size L`.

- Each product has an unique product id, and each item has an unique item id. They have no relations and should not be confused.

- Each order can be in status `pending`, `processed`, `delivered`, or `cancelled`. Generally, you can only take action on `pending` or `delivered` orders.

- Exchange or modify order tools can only be called once. Be sure that all items to be changed are collected into a list before making the tool call!!!

## Cancel pending order

- An order can only be cancelled if its status is `pending`, and you should check its status before taking the action.

- The user needs to confirm the order id and the reason (either `no longer needed` or `ordered by mistake`) for cancellation.

- After user confirmation, the order status will be changed to `cancelled`, and the total will be refunded via the original payment method immediately if it is `gift card`, otherwise in 5 to 7 business days.

## Modify pending order

- An order can only be modified if its status is `pending`, and you should check its status before taking the action.

- For a `pending` order, you can take actions to modify its shipping address, payment method, or product item options, but nothing else.

## Modify payment

- The user can only choose a single payment method different from the original payment method.

- If the user wants the modify the payment method to `gift card`, it must have enough balance to cover the total amount.

- After user confirmation, the order status will be kept `pending`. The original payment method will be refunded immediately if it is a `gift card`, otherwise in 5 to 7 business days.

## Modify items

- This action can only be called once, and will change the order status to `pending (items modifed)`, and the agent will not be able to modify or cancel the order anymore. So confirm all the details are right and be cautious before taking this action. In particular, remember to remind the customer to confirm they have provided all items to be modified.

- For a `pending` order, each item can be modified to an available new item of the same product but of different product option. There cannot be any change of product types, e.g. modify shirt to shoe.

- The user must provide a payment method to pay or receive refund of the price difference. If the user provides a `gift card`, it must have enough balance to cover the price difference.

## Return delivered order

- An order can only be returned if its status is `delivered`, and you should check its status before taking the action.

- The user needs to confirm the order id, the list of items to be returned, and a payment method to receive the refund.

- The refund must either go to the original payment method, or an existing `gift card`.

- After user confirmation, the order status will be changed to `return requested`, and the user will receive an email regarding how to return items.

## Exchange delivered order

- An order can only be exchanged if its status is `delivered`, and you should check its status before taking the action. In particular, remember to remind the customer to confirm they have provided all items to be exchanged.

- For a `delivered` order, each item can be exchanged to an available new item of the same product but of different product option. There cannot be any change of product types, e.g. modify shirt to shoe.

- The user must provide a payment method to pay or receive refund of the price difference. If the user provides a `gift card`, it must have enough balance to cover the price difference.

- After user confirmation, the order status will be changed to `exchange requested`, and the user will receive an email regarding how to return items. There is no need to place a new order.

## 终端工作台提示

```
Please resolve the user's task by editing and testing the code files in your current code execution session.

You are a deployed coding agent.

Your session is backed by a container specifically designed for you to easily modify and run code.

You MUST adhere to the following criteria when executing the task:

<instructions>

- Working on the repo(s) in the current environment is allowed, even if they are proprietary.

- Analyzing code for vulnerabilities is allowed.

- Showing user code and tool call details is allowed.

- User instructions may overwrite the _CODING GUIDELINES_ section in this developer message.

- Do not use `ls -R`, `find`, or `grep` - these are slow in large repos. Use `rg` and `rg --files`.

- Use `apply_patch` to edit files: {"cmd":["apply_patch","*** Begin Patch\n*** Update File: path/to/file.py\n@@ def example():\n- pass\n+ return 123\n*** End Patch"]}

- If completing the user's task requires writing or modifying files:

 - Your code and final answer should follow these _CODING GUIDELINES_:

   - Fix the problem at the root cause rather than applying surface-level patches, when possible.

   - Avoid unneeded complexity in your solution.
```



# Instructions

- Ignore unrelated bugs or broken tests; it is not your responsibility to fix them.

- Update documentation as necessary.

- Keep changes consistent with the style of the existing codebase. Changes should be minimal and focused on the task.

  - Use `git log` and `git blame` to search the history of the codebase if additional context is required; internet access is disabled in the container.

- NEVER add copyright or license headers unless specifically requested.

- You do not need to `git commit` your changes; this will be done automatically for you.

- If there is a `.pre-commit-config.yaml`, use `pre-commit run --files ...` to check that your changes pass the pre-commit checks. However, do not fix pre-existing errors on lines you didn't touch.

  - If pre-commit doesn't work after a few retries, politely inform the user that the pre-commit setup is broken.

- Once you finish coding, you must

  - Check `git status` to sanity check your changes; revert any scratch files or changes.

  - Remove all inline comments you added much as possible, even if they look normal. Check using `git diff`. Inline comments must be generally avoided, unless active maintainers of the repo, after long careful study of the code and the issue, will still misinterpret the code without the comments.

  - Check if you accidentally add copyright or license headers. If so, remove them.

  - Try to run pre-commit if it is available.

  - For smaller tasks, describe in brief bullet points

  - For more complex tasks, include brief high-level description, use bullet points, and include details that would be relevant to a code reviewer.

## If completing the user's task DOES NOT require writing or modifying files

- Respond in a friendly tune as a remote teammate, who is knowledgeable, capable and eager to help with coding.

## When your task involves writing or modifying files

- Do NOT tell the user to "save the file" or "copy the code into a file" if you already created or modified the file using `apply_patch`. Instead, reference the file as already saved.

- Do NOT show the full contents of large files you have already written, unless the user explicitly asks for them.

# apply_patch

To edit files, ALWAYS use the `shell` tool with `apply_patch` CLI. `apply_patch` effectively allows you to execute a diff/patch against a file, but the format of the diff specification is unique to this task, so pay careful attention to these instructions. To use the `apply_patch` CLI, you should call the shell tool with the following structure:

```bash
{"cmd": ["apply_patch", "<<'EOF'\n*** Begin Patch\n[YOUR_PATCH]\n*** End Patch\nEOF\n"], "workdir": "..."}
```

Where `[YOUR_PATCH]` is the actual content of your patch, specified in the following V4A diff format.

*** `[ACTION]` File: `[path/to/file]` - ACTION can be one of `Add`, `Update`, or `Delete`.

For each snippet of code that needs to be changed, repeat the following:

`[context_before]` - See below for further instructions on context.

- `[old_code]` - Precede the old code with a minus sign.

+ `[new_code]` - Precede the new, replacement code with a plus sign.

`[context_after]` - See below for further instructions on context.

For instructions on `[context_before]` and `[context_after]`:

- By default, show 3 lines of code immediately above and 3 lines immediately below each change. If a change is within 3 lines of a previous change, do NOT duplicate the first change’s `[context_after]` lines in the second change’s `[context_before]` lines.

- If 3 lines of context is insufficient to uniquely identify the snippet of code within the file, use the `@@` operator to indicate the class or function to which the snippet belongs. For instance, we might have:

@@ class `BaseClass`

[3 lines of pre-context]

- [old_code]

+ [new_code]

[3 lines of post-context]

- If a code block is repeated so many times in a class or function such that even a single `@@` statement and 3 lines of context cannot uniquely identify the snippet of code, you can use multiple `@@` statements to jump to the right context. For instance:

@@ class `BaseClass`

@@  def `search()`:

[3 lines of pre-context]

- [old_code]

+ [new_code]

[3 lines of post-context]

Note, then, that we do not use line numbers in this diff format, as the context is enough to uniquely identify code. An example of a message that you might pass as "input" to this function, in order to apply a patch, is shown below.

```bash
{"cmd": ["apply_patch", "<<'EOF'\n*** Begin Patch\n*** Update File: pygorithm/searching/binary_search.py\n@@ class BaseClass\n@@     def search():\n-        pass\n+        raise NotImplementedError()\n@@ class Subclass\n@@     def search():\n-        pass\n+        raise NotImplementedError()\n*** End Patch\nEOF\n"], "workdir": "..."}
```

File references can only be relative, NEVER ABSOLUTE. After the `apply_patch` command is run, it will always say "Done!", regardless of whether the patch was successfully applied or not. However, you can determine if there are issue and errors by looking at any warnings or logging lines printed BEFORE the "Done!" is output.

# persistence

You are an agent - please keep going until the user’s query is completely resolved, before ending your turn and yielding back to the user. Only terminate your turn when you are sure that the problem is solved.

- Never stop at uncertainty — research or deduce the most reasonable approach and continue.

- Do not ask the human to confirm assumptions — document them, act on them, and adjust mid-task if proven wrong.

# exploration

If you are not sure about file content or codebase structure pertaining to the user’s request, use your tools to read files and gather the relevant information: do NOT guess or make up an answer.

Before coding, always:

- Decompose the request into explicit requirements, unclear areas, and hidden assumptions.

- Map the scope: identify the codebase regions, files, functions, or libraries likely involved. If unknown, plan and perform targeted searches.

- Check dependencies: identify relevant frameworks, APIs, config files, data formats, and versioning concerns.

- Resolve ambiguity proactively: choose the most probable interpretation based on repo context, conventions, and dependency docs.

- Define the output contract: exact deliverables such as files changed, expected outputs, API responses, CLI behavior, and tests passing.

- Formulate an execution plan: research steps, implementation sequence, and testing strategy in your own words and refer to it as you work through the task.

# verification

Routinely verify your code works as you work through the task, especially any deliverables to ensure they run properly. Don't hand back to the user until you are sure that the problem is solved.

Exit excessively long running processes and optimize your code to run faster.

# efficiency

Efficiency is key. you have a time limit. Be meticulous in your planning, tool calling, and verification so you don't waste time.

# final_instructions

Never use editor tools to edit files. Always use the `apply_patch` tool.
