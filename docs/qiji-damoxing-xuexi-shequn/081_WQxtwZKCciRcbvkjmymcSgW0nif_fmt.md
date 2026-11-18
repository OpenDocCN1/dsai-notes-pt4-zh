
# 奇绩大模型日报（10月 26-27日）

> 来源：[https://miracleplus.feishu.cn/docx/QD4adhf2woCPMVxHPQMcoRVzntg](https://miracleplus.feishu.cn/docx/QD4adhf2woCPMVxHPQMcoRVzntg)

## 🔉潜空间活动报名

> 本期活动将在**11月9日** **10:00**开始，我们邀请到的嘉宾是**鱼哲**，Lepton AI 创始成员，曾在阿里云担任高性能 AI 平台产品负责人，专注于 AI 在多个行业的落地及应用。Lepton AI 致力于建立高效可用的AI 基础设施，让团队更关注于应用构建及落地。在本次分享中鱼哲将带来关于AI产品相关的思考，分享主题《Beyond Infra, what matters?—— 不同AI产品形态对团队的挑战》。

> 除嘉宾分享外，每期设置了【匹配合伙人 Cofounder Matching】环节。你可以和 GenAI 时代最有活力的创业者和研究者线下面对面交流，将有机会找到志同道合、有共同创业梦想的小伙伴。

> 报名通道已开启，欢迎扫描下方二维码报名。

![](img/0539bd22868f751fe52b360a7e2519c9.png)

## 资讯

### 微软开源OmniParser

`https://mp.weixin.qq.com/s/yEC32W-dobHF7pycDSHcPg`

近期，AI 操控计算机领域迎来一波热潮。微软推出的 OmniParser，展示了其卓越的屏幕解析能力，结合视觉和语言模型（VLM），可以将UI截图解析为结构化信息，识别交互元素，并生成精准操作。OmniParser的创新之处在于其无需依赖 HTML 结构或 Android 视图图层，便可在PC和移动平台上完成多样化的界面解析任务。

OmniParser的核心功能包括：

1.  **交互区域检测**：利用数据集标注点击区域和可操作元素，OmniParser准确识别屏幕上可交互的按钮和图标。

2.  **功能语义描述**：微调的描述模型能够为检测到的 UI 元素生成上下文相关的功能描述，使模型理解界面含义。

OmniParser还可以作为 VLM 的插件，显著增强模型的任务完成能力。在 GPT-4V 和 OmniParser结合的实验中，模型在 WindowsAgentArena 基准上达到最佳水平。此外，OmniParser还提升了在 ScreenSpot 和 Mind2Web 基准上的解析和操作表现。

在操作示例中，OmniParser帮助用户在旅游网站上筛选素食餐厅，并自动添加至行程，这种模拟任务展示了该工具从解析到操作的完整能力流程。当前，OmniParser已经支持与其他 VLM 模型（如 Phi-3.5 和 Llama-3.2）的结合，有望进一步推动AI操控界面的普及应用。

OmniParser的发布标志着屏幕解析与 AI 操作的新发展，为创建多平台、多任务的计算机控制智能体奠定了基础。

![](img/122cbfc4cafd27570be6e5348ccda90d.png)

### 斯坦福开源学术研究神器STORM新功能

`https://mp.weixin.qq.com/s/-NY8Xw8ihIFgUwy4LFLMgA`

斯坦福大学今年推出的开源工具 STORM 利用大语言模型（LLM）自动生成类维基百科的文章，支持长篇内容创作。用户仅需输入主题，STORM 即可在三分钟内生成长篇文章或研究报告并支持 PDF 下载。STORM 借助 LLM 进行信息检索、构建大纲，并通过专家模拟对话生成深入、准确的内容，尤其适合需要广泛引用的内容创作。其代码开源后，GitHub Star 已超12k。

近期，团队推出了升级版 Co-STORM，引入协作对话机制，实现更高效的学术研究支持。Co-STORM 包含以下角色：

+   **LLM专家**：基于外部知识来源生成答案，并提出后续问题。

+   **主持人**：生成引导性问题，利用未使用的信息引导深层对话。

+   **人类用户**：观察或引导对话，深入了解主题。

Co-STORM 在生成过程中通过动态思维导图（3.2）跟踪和参与讨论（3.3），提示专家基于对话历史生成上下文响应（3.4），主持人则提出新问题以引导对话（3.5）。最终，思维导图支持生成完整引用报告。

评估显示，Co-STORM 优于传统 RAG Chatbot 和 STORM + QA 基线，在深度和新颖性上尤其出色。多智能体角色设计有效提升了对话一致性和参与度。主持人通过引入“已知未知”信息引导用户探索“未知未知”领域，使其获取更广泛、更深入的信息，提升了报告质量和问答轮次质量。

![](img/94a9571bb4f8abb8b0d018baa5662847.png)

### AIDE：递归自我改进的机器学习代码生成 Agent

`https://mp.weixin.qq.com/s/uJ4bwe3HO2yHrqoml1iO1g`

OpenAI 近期发布了新的基准测试 MLE-bench，专注于机器学习自动化（MLE）中的自主执行任务。测试集涵盖 75 个 Kaggle 问题，提供一个平台来评估 LLM 在端到端机器学习中的表现。基准测试显示，GPT-4o 和 AIDE 框架的结合在奖牌数量上超越了其他开源框架。而在引入 o1-preview 模型后，性能提升翻倍，使其在 16.9% 的任务中达到 Kaggle 铜牌水平，多轮尝试后这一比例提升至 34.1%。

AIDE 是一种专注于代码优化的机器学习代码生成 Agent，使用递归自我改进（recursive self-improvement）策略，通过「解空间树搜索」进行优化，包括解决方案生成、评估与筛选三个关键组件。其 AI Function 范式将任务分解为具体指令，逐步优化，使得大模型在受限问题中表现出色。此外，AIDE 在 MLE-bench 中 16.9% 的任务上获得奖牌，甚至在 Kaggle 数据科学比赛中击败了 50% 的人类参赛者。

在 MLE-bench 中，AIDE 展现了出色的适配能力。UCL 名誉教授 Edward Grefenstette 和谷歌 DeepMind 团队认为，AIDE 的框架对 OpenAI 的智能体开发产生了深远影响。AIDE 的设计理念是构建更具自我改进潜力的 AI 工具，以递归优化方法解决复杂问题。然而，当前的技术仍然面临多步任务的局限性。为进一步推动 AIDE 的发展，团队计划发布新的产品 AI Function Builder，使得用户通过 API 接口即可使用 AI 功能，大幅简化应用流程。

WecoAI 团队由伦敦大学学院（UCL）成员组成，正通过 AIDE 推进递归自我改进的 AI 工程，致力于在科学研究领域实现更具自我学习能力的 AI 科学家。这一方向标志着 AI 正逐步向更复杂的科研协作迈进，未来在 AI 驱动科学发现方面的潜力无疑值得期待。

![](img/d251afefdc61ea73b011f992a4341318.png)

### 大模型生成RPG游戏，情节角色全自定义

`https://mp.weixin.qq.com/s/7xBtIo2x_TbTMNzFxtDXiQ`

谷歌与北卡罗来纳大学教堂山分校的研究团队开发了一款生成角色扮演游戏“Unbounded”，灵感来源于《模拟人生》。用户通过自定义角色人生，操控角色的行动、互动、状态更新（如饥饿度和能量）。角色可以实时自由探索环境，每秒更新一次，呈现高度互动性。这项研究引入了“生成无限游戏”概念，即通过生成式模型实现游戏的无限扩展，而非传统有限情境。Unbounded具备角色个性化、环境生成、开放式互动和实时更新等关键属性。

他们开发了基于大型语言模型（LLM）的游戏引擎，保障游戏机制一致性、叙事连贯性，并能根据角色上下文实时生成响应。核心技术包括使用协作性强的LLM自动生成数据，并基于Gemma-2B模型微调，强化状态更新、环境一致性、故事连贯性和指令遵循。

在视觉生成上，研究团队引入了动态区域图像提示适配器（Regional IP-Adapter），通过动态掩码调制生成一致的角色和环境图像，采用区块丢弃技术减少角色和环境间干扰，提升图像的角色一致性和环境连贯性。主要研究成员包括北卡罗来纳大学教堂山分校的Jialu Li（本科毕业于上海交大）。团队导师Nataniel Ruiz对生成游戏的未来充满期待，认为生成游戏技术可能会完全革新角色扮演类游戏。

![](img/a3855c6904c7b13cf5c23608933c4d30.png)

### OpenAI今年不计划发布名为“Orion”的AI模型

`https://techcrunch.com/2024/10/25/openai-says-it-wont-release-a-model-called-orion-this-year/`

OpenAI 近日声明，今年不会发布代号为“Orion”的AI模型，以澄清近期关于其产品路线图的报道。此前，《The Verge》曾报道称，Orion将于12月推出，微软等合作伙伴将率先获得该模型的预览权限，甚至可能于11月进行测试。然而，OpenAI 表示该报道并不准确，但并未提供进一步细节。

Orion据称是OpenAI现有旗舰模型GPT-4o的升级版本，部分训练数据来源于OpenAI的“推理”模型o1。该公司计划继续开发新GPT模型，同时拓展类似o1的推理模型，以满足不同的应用场景。不过，OpenAI的声明也留有一定的灵活空间，暗示可能会在年内推出其他新技术，但未必是Orion。

![](img/a4d943fba4920fe2d7f5ea5a1bd5f3c2.png)

## 推特

### 大语言模型玩你画我猜

贡献人：

`https://x.com/paul_cal/status/1850262678712856764`

大型语言模型 (LLM) 玩你画我猜！

![](img/7f7dda71dc1391a9d628b460252a9895.png)

[ssstwitter.com_1730019483407.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BkVjbg5Hto6052xlAbEcwGm3nBg)

### Anthropic AI分享研究更新合集：Crosscoders、字典学习特征等

贡献人：

`https://x.com/AnthropicAI/status/1849906129637085582`

过去几个月，我们的可解释性团队发布了一些研究更新：

Crosscoders（发布链接：`https://transformer-circuits.pub/2024/crosscoders/index.html`）是一种新方法，可在模型的不同层或不同模型之间找到共享特征。识别跨层持久的特征有助于简化我们对模型的理解。

此外，Crosscoders 还可以用于“差异化”模型：对比预训练模型和微调模型，观察它们的基本差异。

字典学习特征可以用于检测危险行为，并在某些情况下优于线性探测器，但可能会发现虚假关联，使分类器在对抗性攻击下更脆弱。

`https://transformer-circuits.pub/2024/features-as-classifiers/index.html`

增加与特定主题相关的稀疏自编码器数据量，也能提高该主题特征的分辨率。

`https://transformer-circuits.pub/2024/september-update/index.html#oversampling`

我们复现了 Gould 等人的“继任头”研究，发现独立成分分析可以将这些头分解为继任、归纳和类别成分。

![](img/8ae49dfcff1f7a35a240053df3bf543e.png)

更多在原帖中～

### 开源版Notebook LM？Bhutani分享NotebookLlama

贡献人：

`https://x.com/bhutanisanyam1/status/1850178626291122503`

NotebookLlama 是一个开源的 NotebookLM 版本 🙏

以下是使用 Llama 构建“PDF 到播客”流程的完整教程：

• 使用 1B 预处理 PDF

• 使用 70B 将其转换为播客文本

• 使用 8B 增加戏剧效果

• 使用 Parler 和 Suno 模型进行语音合成

项目链接：[NotebookLlama](https://github.com/meta-llama/llama-recipes/tree/main/recipes/quickstart/NotebookLlama)

![](img/7ffac5fbc78cb326551e38851fe43f98.png)

### Based Agent：在 Base 上创建具有完整链上功能的 AI 代理，耗时不到 3 分钟

贡献人：

`https://x.com/MurrLincoln/status/1850226148594082120`

Introducing Based Agent：在 Base 上创建具有完整链上功能的 AI 代理，耗时不到 3 分钟。

链上自主代理的时代已经到来！

基于 Coinbase 开发者 SDK、OpenAI 和 Replit 构建。

哦，你还可以把它变成一个 X（推特）机器人 😉

![](img/273ba01d5967352995bc896b3a23846e.png)

[ssstwitter.com_1729997568073.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Qci9bX4T6oj25Ax3RCScMTvfnbc)

### 谷歌正在准备“Project Jarvis”——一款可以接管电脑、帮助完成日常网络任务的 AI 程序

`https://x.com/theinformation/status/1850254366239776980`

独家消息：谷歌正在准备“Project Jarvis”——一款可以接管电脑、帮助完成日常网络任务的 AI 程序。

Google 正在开发一种人工智能，可以接管用户的网页浏览器来完成任务，比如收集资料、购买商品或预订航班。据三位直接了解该产品的消息人士透露，这款产品代号为 Project Jarvis，与 Anthropic 本周宣布的一款产品类似。

![](img/ee8848e6fda965e7e2f8c8d771a00d65.png)

## 信号

### Evaluating feature steering: A case study in mitigating social biases

贡献人：

研究节点：模型体系 -- 评估体系

原文链接：[https://www.anthropic.com/research/evaluating-feature-steering](https://www.anthropic.com/research/evaluating-feature-steering)

我们希望透明地分享我们的初步（混合）研究结果，能够帮助更好地理解特征引导在生成更安全的模型输出中可能发挥的作用。我们在文章的结尾列出了详细的局限性、经验教训和未来可能的方向。我们将许多额外的实验和技术细节放在附录中，并在正文中也提及这些内容，以便对感兴趣的读者提供参考。

![](img/8dc94aa299293915cb432062a83a11ed.png)

### Open Materials 2024 (OMat24) Inorganic Materials Dataset and Models

贡献人：

研究节点：应用体系 -- AI4S

原文链接：`https://arxiv.org/abs/2410.12771`

我们发布了2024年开放材料（OMat24）大规模开放数据集的Meta FAIR版本，以及一套配套的预训练模型。OMat24包含超过1.1亿个聚焦于结构和成分多样性的密度泛函理论（DFT）计算。

![](img/f41d209ab856ddb9cf01b2c7bc067460.png)

### Reinforcement Learning under Latent Dynamics: Toward Statistical and Algorithmic Modularity

贡献人：

研究节点：模型体系--后训练--RL

原文链接：`https://arxiv.org/abs/2410.17904`

这篇文章探讨了在潜在（"latent"）动态下的强化学习问题，从统计和算法的角度进行了分析。在现实世界的应用中，强化学习代理通常在复杂、高维的观测环境中操作，但潜在的动态相对简单。然而，在小潜在空间等限制性设置之外，对于潜在动态下的强化学习的基本统计要求和算法原则了解甚少。

![](img/575ff134c93ec3497b1440a705b2ff34.png)

### Stick-breaking Attention

贡献人：

研究节点：模型体系--架构体系--注意力机制

原文链接：`https://arxiv.org/pdf/2410.17980`



这篇文章提出了一种新的注意力机制，称为“stick-breaking attention”，它基于stick-breaking过程，用于替代传统的基于`softmax`操作的自注意力机制。在传统的自注意力中，需要位置嵌入（如`RoPE`）或位置偏置来考虑令牌顺序，但现有方法仍然面临长度泛化挑战。新提出的机制通过为每个令牌确定一个断裂点βi,j来表示剩余部分分配给当前令牌的比例，这个过程自然地融入了最近偏置，这在语法解析的语言动机中是有根据的。文章研究了用`stick-breaking attention`替代传统`softmax-based attention`机制的影响，并讨论了数值稳定的`stick-breaking attention`的实现，以及如何适应`Flash Attention`来容纳这种机制。当用作当前`softmax`+`RoPE`注意力系统的直接替代品时，`stick-breaking attention`在长度泛化和下游任务上的表现与当前方法相当。`stick-breaking`在长度泛化方面也表现良好，允许一个用211上下文窗口训练的模型在214上表现良好，并且困惑度有所提高。

![](img/2ddeef1883b5a381c4522fd3ad14301d.png)

### Insights from the Inverse: Reconstructing LLM Training Goals Through Inverse RL

贡献人：

研究节点：模型体系-可解释性

原文链接：`https://arxiv.org/pdf/2410.12491`

这篇文章介绍了一种新的方法来解释大型语言模型（`LLMs`），通过应用逆强化学习（`IRL`）来恢复它们的隐含奖励函数。文章在不同大小的针对毒性对齐的`LLMs`上进行实验，提取的奖励模型在预测人类偏好方面达到了高达80.40%的准确率。分析揭示了奖励函数的非唯一性、模型大小与可解释性之间的关系，以及`RLHF`过程中的潜在陷阱。文章展示了`IRL`衍生的奖励模型可以用来微调新的`LLMs`，在毒性基准测试中取得了相当或更好的性能。这项工作为理解和改进`LLM`对齐提供了新的视角，对这些强大系统的负责任开发和部署具有重要意义。

![](img/6918dc43ef90951bad8edfbc9f61d255.png)

### SMART: Self-learning Meta-strategy Agent for Reasoning Tasks

贡献人：

研究节点：模型体系-推理体系

原文链接：`https://arxiv.org/pdf/2410.16128`

这篇文章介绍了一个名为`SMART`（Self-learning Meta-strategy Agent for Reasoning Tasks）的新框架，它使语言模型（`LMs`）能够自主学习和选择各种推理任务最有效的策略。文章将策略选择过程建模为马尔可夫决策过程，并利用强化学习驱动的连续自我改进，使模型能够找到解决给定任务的合适策略。与传统的自我改进方法不同，`SMART`依赖于多次推理传递或外部反馈，`SMART`允许`LM`内部化其自身推理过程的结果，并相应地调整其策略，目标是在第一次尝试中就找到正确的解决方案。在各种推理数据集和不同模型架构上的实验表明，`SMART`显著提高了模型在没有外部指导的情况下选择最优策略的能力（在`GSM8K`数据集上提高了15个百分点）。通过在单次推理传递中实现更高的准确率，`SMART`不仅提高了性能，还降低了基于改进策略的计算成本，为`LMs`中更高效、更智能的推理铺平了道路。

![](img/da1a0b8726e98912b3159b16a586c7ec.png)

### On Designing Effective RL Reward at Training Time for LLM Reasoning

贡献人：

研究节点：模型体系--后训练--RL

原文链接：`https://arxiv.org/pdf/2410.15115`

这篇文章探讨了奖励模型在提高大型语言模型（`LLMs`）推理能力方面的重要性。研究表明，训练有素的奖励模型可以通过搜索显著提高模型在推理时的性能。然而，奖励模型在强化学习（`RL`）训练期间的潜力尚未被充分探索。目前尚不清楚这些奖励模型是否能够在使用稀疏成功奖励的`RL`训练中提供额外的训练信号，以增强`LLMs`的推理能力。

![](img/4e482be2b0c1d2a4e3503adc82f7fb08.png)

# 产品

## Retail Connect Pietra

贡献人：

`https://www.pietrastudio.com/`

Retail Connect 是一个由 Pietra 开发的平台，可以帮助品牌生成批发潜在客户并与全球零售商建立联系。它提供全球零售商的联系方式、个性化销售提案以及多渠道沟通方式，简化品牌与零售商之间的连接，推动销售增长。

![](img/5ff0aff23ab01cec656e33ca7e2ea40b.png)

## TableSprint

贡献人：

`https://www.tablesprint.com/`

TableSprint 是一个快速应用开发平台，利用表格数据提供多种功能，如强大的表单构建、应用发布、数据连接和权限控制等，可以简化 HR、销售和项目管理等领域的应用构建。它支持 Excel 导入、条形码扫描和多种集成，适合小型企业和制造商使用。

![](img/6ce23d616c6fd49fa669fc716c62c1da.png)

# HuggingFace&Github

## Skyvern

贡献人：

`https://github.com/Skyvern-AI/skyvern`

Skyvern 是一款结合 `LLM` 和计算机视觉的工具，可以自动化浏览器工作流，提供简单的 `API` 端点以全自动化手动流程，替代脆弱的自动化方案。其主要功能包括在未见过的网站上运行，通过视觉元素映射交互动作，抵抗网站布局变化，适用于多个网站的单一工作流，以及利用 `LLM` 处理复杂情况。技术栈涵盖 `Python`、`TypeScript`、`Playwright` 和多种 `LLM` 服务。

![](img/9bc4244c66bb99493f5479cf71366ced.png)

# 投融资

## Waymo获56亿美元C轮融资，加速无人驾驶出租车扩张

`https://mp.weixin.qq.com/s/9kyXRMh5qLEvf6YxLBFRPw`

Alphabet旗下的自动驾驶子公司Waymo宣布完成56亿美元的C轮超额认购融资，这是其迄今为止规模最大的一轮融资。本轮融资由Alphabet领投，参与的投资机构包括Andreessen Horowitz、Fidelity、Perry Creek、Silver Lake、Tiger Global和T. Rowe Price，但具体出资比例未披露。这是Waymo自2020年完成22.5亿美元B轮融资（后增至32亿美元）以来的首次融资。

本轮融资旨在推动Waymo的无人驾驶出租车业务扩展至更多城市，并进一步提升其自动驾驶技术能力。目前，Waymo已经将主要业务聚焦于自动驾驶出租车服务Waymo One，正在旧金山、洛杉矶、菲尼克斯等城市提供服务，并计划扩展至奥斯汀和亚特兰大。此外，Waymo在菲尼克斯天港国际机场等区域提供接驳服务，逐步将Waymo Driver系统推向更复杂的道路环境，如布法罗和华盛顿特区等城市。

投资机构对Waymo的技术和市场潜力充满信心。Tiger Global创始人Chase Coleman指出，Waymo的产品被认为是自动驾驶领域最安全、最成熟的选择。Silver Lake联合首席执行官Egon Durban进一步表示，Waymo Driver通过尖端研究和大量实践验证了AI在交通领域的潜力，建立了广泛的市场信任。

伴随着融资的注入，Waymo发布了第六代Waymo Driver，以提升成本效率并优化功能。同时，Waymo与Uber合作，进一步巩固市场扩展战略，以提升其在城市交通中的影响力。本轮融资标志着Waymo迈向关键发展阶段，其目标不仅是扩大市场覆盖，更是利用无人驾驶技术重塑城市交通格局。

![](img/8486f0789b5a54f0d183104291ed8636.png)

## YC 创业公司 Pharos 获得 Felicis 领投的 500 万美元种子轮融资

`https://techcrunch.com/2024/10/25/yc-startup-pharos-lands-a-5m-seed-led-by-felicis-to-bring-ai-to-quality-reporting/`

Pharos，这家参加了 2024 年 Y Combinator 夏季孵化器的创业公司，成功获得了由 Felicis 领投的 500 万美元种子轮融资，投资方还包括 General Catalyst、Moxxie（前期投资者）和 Y Combinator。Pharos 专注于利用 AI 来简化医院质量报告的过程，帮助医院高效地将患者的电子病历数据报告给临床注册表，如美国医疗保险与医疗补助服务中心（CMS）和美国外科学院等机构。虽然这种报告并非强制性，但有助于医院识别质量问题，改进患者护理。

Pharos 的创始团队，包括 Felix Brann、Matthew Jones 和医学博士 Alex Clarke，他们具有丰富的行业经验。Pharos 的 AI 技术能够自动从电子病历中提取非结构化数据，生成所需的报告，显著节省医院的时间和人力成本。

公司官网：`https://pharos.health/`

![](img/12e49127e91c2820ed232d8fedbbdb50.png)

# 学习

## 代码大模型(一)--业界现状

贡献人：

`https://mp.weixin.qq.com/s/OllCcuxugOqf0aLkc9K-Mg`

### 评测指标

+   `HumanEval`：由`OpenAI`提出，包含164个`Python`编程问题，专门设计以避免与模型训练数据重合，评测模型生成代码的准确性，使用 `pass@k` 指标（生成多次，计算平均通过率）。

+   `MBPP`：Google推出的`MBPP`包含多种数学和列表处理问题，每个问题有3个测试样例，`OpenCompass`中常用于代码模型的准确性测试。

### FIM训练（Fill-in-the-Middle）

+   传统的left-to-right训练限制了代码模型的补全能力，而`FIM`训练将数据切为三部分（前缀、后缀和中间部分），要求模型在已知上下文的情况下生成中间代码，提高代码补全能力。

+   通过使用特殊标记（如 `<PRE>`、`<SUF>`、`<MID>`），`FIM`数据在训练中以0.5的比例混入，提升模型在不影响生成能力的同时增强补全能力。

### 热门代码模型

+   `Code Llama`：基于`Llama 2`开发，使用了大量代码数据，并通过 `FIM` 和长上下文微调来优化。数据涵盖`Python`专用版本与通用代码生成版本，在`HumanEval`等基准测试上表现优异。

+   `StarCoder 2`：BigCode开源的`StarCoder 2`和`Stack v2`基于更大数据集（`v1`的四倍），涵盖多语言代码与相关文档，并采用专门的处理流程去重、过滤恶意内容，专注代码生成。

+   `DeepSeek-Coder-V2`：拥有`16B`和`236B`的模型规模，在千亿模型中表现突出，利用代码和数学数据的组合训练，长文本窗口与`reward model`实现高效代码生成。

+   `Qwen2.5-Coder`：包含两个规模（`1.5B`和`7B`），使用多种类型的数据，包括`Pull Requests`和`Notebook`等，采用了`FIM`和基于`YARN`的长窗口训练，并通过多语言agent合作生成丰富的多语言指令数据集。

### 关键技术总结

+   `FIM`训练极大提升了模型的代码补全能力；

+   代码数据和数学数据的双重训练对生成效果至关重要；

+   数据清洗、去重和去除敏感信息是提升代码生成效果的基础；

+   高质量代码模型的构建依赖于广泛的数据收集、精细的清洗以及严谨的实验验证。

![](img/5e60345148acf999e4892d2993aee8cc.png)

## 长上下文背景下的强化学习对齐策略探索

`https://zhuanlan.zhihu.com/p/2993874959`

当前，长文本模型（`Long-context Model`, `LCM`）在理解和生成过程中易出现“幻觉”和“指令不遵循”现象（Misalignment现象）。例如，在回答美剧《老友记》中人物职业的问题时，模型可能错误地称Rachel是护士（Hallucination），或忽视问题并生成无关内容。这些问题主要源于模型的检索头（`Retrieval Head`）无法准确定位关键信息或未能处理已定位信息。

### 长文本对齐策略

研究表明，传统的监督微调（`SFT`）在长文本对齐中效果有限。近期，`LOGO`策略（Long cOntext aliGnment via efficient preference Optimization）提出了一种基于强化学习的对齐方法，着重解决训练效率和偏好数据构建难题。该方法通过无参考对齐优化和偏好优化（`SimPO`）提升生成准确性，在构建负样本时扩大了负样本范围，使模型更易于学习。`LOGO`的优化目标不仅区分优选与非优选响应，还加入正则项以减少生成能力退化现象。

### 关键技术

1.  偏好数据构建：`LOGO`通过实体抽取对长文本划分片段并得分，将关键片段与干扰片段组合生成正负偏好数据。利用不同片段组合生成不同优劣程度的响应，减少了长文本数据标注成本。

1.  位置编码合成：通过调整位置编码标记，实现“无限长”序列感知；`LOGO`采用混合位置编码覆盖长文本范围，以减少缺失信息带来的误差。

1.  高效训练：采用`LoRA`（`Low-Rank Adaptation`）微调策略降低显存需求，`LOGO`在`A800` GPU上仅用16小时便显著提升了`Llama-3-8B-Instruct`模型在长文本任务上的表现。

### 实验效果

`LOGO`在`LongBench`数据集上达到了接近`GPT-4`的表现，并在长文本任务上显著提升`Retrieval Score`，证明了其对关键信息的更强定位和理解能力。相比传统对齐算法，`LOGO`不仅避免了幻觉现象，还激活了检索头功能，展现出更佳的长文本理解效果。

![](img/2613ead23266c34951b94052d2ad27f9.png)

## Embedding模型训练技巧的简单回顾

`https://zhuanlan.zhihu.com/p/2982612970`

嵌入（`Embedding`）是将文本、图像、视频、时间序列或语音等数据转换为特定向量表示，并应用于检索、召回、分类、聚类和相似度比较等任务的技术。在近年来，以检索增强生成（`RAG`）任务为代表的领域快速发展，进一步推动了嵌入的重要性。早期的嵌入方法包括`Word2Vec`、`GloVe`及`BERT`的`CLS`向量。然而，`BERT`的原生向量质量较差，因而发展了如`BERTFlow`、`BERT-whitening`及`Sentence Transformers`等改进方法。

近年来，对比学习（`Contrastive Learning`）在嵌入模型中崭露头角，特别是`SimCSE`等对比学习模型在文本嵌入中得到了广泛应用。中文嵌入领域中也涌现了`BGE`、`Piccolo`、`m3e`等代表性工作。在图像嵌入中，`VAE`、`ViT`、`SimCLR`、`BYOL`等模型在图像对比学习中表现良好，而`MAE`也被用于嵌入任务。

多模态嵌入则集中在`CLIP`、`BLIP`和`SimVLM`等模型上，致力于从大规模图像-文本对中学习共享空间的表示。训练流程上，许多模型采取两阶段方法：初期对比预训练及多任务微调阶段。困难负样本挖掘（Hard Negative Mining）是提升模型性能的重要手段，例如利用`BM25`或近似最近邻（`ANN`）方法生成困难负例。

在模型架构上，许多嵌入模型基于`BERT`结构，并在训练策略中结合多任务损失以优化不同下游任务。诸如`Nomic Embed`、`Matryoshka`多维度学习等新兴技术致力于在推理时生成更灵活的嵌入表示。

评估指标包括分类任务的准确率、聚类任务的`V-Measure`，以及查询任务的`MAP`和`NDCG@10`等。在现有评测中，`MTEB`（英文）、`CMTEB`（中文）等集合了多种嵌入评测基准。嵌入模型尽管不如生成模型那样受关注，仍然在诸多应用场景中展现了其独特价值。

![](img/7529393cb5dd9cd13e13cd15e68459f1.png)

## 图文详解 Continuous Batch：不写 CUDA Kernel 也能成倍优化推理效率

`https://zhuanlan.zhihu.com/p/876908831`

### 一、调度策略及特点

1.  `Static Batch`（静态批处理）：将所有请求按批次分组并顺序处理。即使个别请求提前完成，也需要等待批次内其他请求完成后才能释放资源。这种方式导致`GPU`资源浪费和请求延迟增加，但实现相对简单，只需按顺序分配批次。

1.  `Continuous Batch`（动态批处理）：按迭代进行调度，每完成一个批次的`prefill`或`decode`阶段后，立即开始下一批次，提高了`KV Cache`（键值缓存）利用率。这种策略通过在推理阶段分阶段优化，最大化批次规模，有效利用`GPU`的大规模并行处理优势。

### 二、Continuous Batching的应用原理

该策略最早应用于`Orca`推理框架，但在`vLLM`上得到更广泛应用，并成为推理标准。在`vLLM`中，`LLMEngine`每次前向推理前会让调度器检查`KV Cache`，根据当前资源占用率动态调度是否释放内存空间，从而优化显存利用。

### 三、优化效果与实现

`Continuous Batching`使`vLLM`在推理场景下的吞吐量显著提升（高达23倍），并减少了`p50`延迟。在实际应用中，调度器优先处理`prefill`阶段请求，确保`GPU`资源及时释放，从而提高整体推理效率。

![](img/19b7c7a4bf8ad86d05e9c7582ae661e2.png)

## 解构DPO：从RLHF推导到多偏好对齐的简化之道

`https://zhuanlan.zhihu.com/p/2112236366`



# 大模型日报

## 技术原理与实验分析

OpenAI推出的o1模型引发了LLM推理能力提升的关注，尤其是利用强化学习（RL）在思维链（chain of thought）中增强了中间步骤的优化，提供了更高质量的答案。近年来，大模型训练从最初的预训练（Pre-training）向后训练（Post-training）演变，其中对齐技术经历了从RLHF（人类反馈强化学习）到DPO（Direct Preference Optimization）的迭代。RLHF的结构清晰但实现复杂，而DPO则通过简化流程来优化对齐，达到更高效率。

DPO的核心是直接优化模型策略，无需训练奖励模型，采用偏好数据优化模型，避免了RLHF对资源的高要求和复杂的采样过程。RLHF的三阶段流程包含无监督预训练、监督微调、偏好对齐，其中的奖励模型和PPO微调是关键，但也带来复杂性。奖励模型通过偏好数据集训练，其损失函数基于Bradley-Terry模型，目标是让好回答的得分高于差回答。相较而言，DPO直接利用偏好数据优化策略，通过构建偏好排序概率分布，避免了显式奖励计算。

DPO架构在推导时引入KL散度，通过对策略分布和奖励加权目标策略的差异最小化来优化。使用Plackett-Luce模型扩展偏好排序，DPO优化的简单性不仅提升了效率，还减少了不确定性，特别适合资源受限场景。总体而言，DPO不需要RLHF中的在线采样迭代，更接近监督微调（SFT），在效率和资源消耗上具有显著优势。

![](img/6eab03226a98788d1d2b039856242916.png)

### 【实战】`vllm`的相关实验和结论（token速率视角）

https://zhuanlan.zhihu.com/p/2559488711

实验在4张A100（80G）显卡上运行，模型为微调后的qwen2-72b-instruct，采用`fastchat`+`vllm`部署。

1. Beam Search对token生成速率的影响：

   + 开启beam search vs. 关闭：关闭beam search时，生成速率更高，约比开启高出5-10 tokens/s。

   + `best_of`参数的影响：`best_of`值越大（如10），token生成速率越低，`best_of=5`时速率明显较高。

   + early stopping的影响：开启或关闭对生成速率无显著影响，主要用于避免复读输出。

1. 解释：Beam Search时间复杂度随`best_of`增大而增加，导致速率降低。Greedy Search（即beam search=False）则因复杂度低，生成速率更快。

1. `Fastchat`和`vllm`速率统计差异：

   + `Fastchat`客户端的统计token速率与`vllm`差异明显，尤其开启beam search时，`vllm`侧速率是客户端的`best_of`倍。这是因为`vllm`每轮生成的tokens数量按`best_of`倍返回，但客户端仅使用其中一个。

1. `tensor-parallel-size`参数无效问题：

   + 开启张量并行未显著提升速率。原因是`num_gpus`参数已自动配置`tensor-parallel-size`，无需手动设置。

1. 生成速率波动现象：

   + 在连续请求下，速率会出现骤降和回升。该现象是由于固定5秒统计周期，与请求和生成token的时间分布不一致所致。

![](img/fa5f80afb21e4f272cad3ce826c489e5.png)

# 招聘公告

---

![](img/6f50cb717921d69c7c5645e51c63cd7b.png)

**奇绩创坛**由**陆奇博士**于2019年创立，使命是在源头最大化驱动创新。奇绩研究组将研究模型理论、到投研战略分析、市场策划等不同背景的同学聚在一起，搭建GenAI时代独特的跨界人才社群。

## 🚀 F24 实习岗位

### [行研-学术科研人才交流组实习生](https://miracleplus.feishu.cn/docx/Yn6rdf2iOoY2Rcx8yVNcUBYRnch#part-QkMcdeJmuob9uvxvV3qcLSzHn8g)

### 行研-GenAI研究岗位-战略分析（偏产品）

### Web / AI 全栈开发工程师实习生

## 火热开放中🚀

### 行研-GenAI研究岗位-前沿学术探索

### 行研-GenAI研究岗位-战略分析（偏咨询）

### 创新活动产品运营 （大模型系列活动产品运营）

> **点击**相关岗位可查看对应**岗位需求【JD】；**相关岗位投递请**扫描二维码**

![](img/707631c70072b4283744c8cb91de6fd3.png)

> 欢迎**推荐/自荐**

> 期待在**奇绩创坛**遇见更好的你

---

# 声明

本文档仅供学习交流使用，版权归原作者所有，若涉侵权，请联系Jack Jin 15101136166

（欢迎在下方反馈您宝贵的意见！整理不易，觉得有用给AI日报小组点个赞👍吧！）

ChatGPT；Angela；Zijia Liu；Alex；Zeqi Tan；Tiezhen（HuggingFace）；Naixu；Lizhi；Peter；Suoman；Jack

封面生成——右脑科技

如果你有大模型相关的创业想法，欢迎来找我们聊聊～🛰️：15101136166

> 快邀请你的小伙伴们一起来读大模型日报吧！

> **【日报知识库】**这里是近期日报内容的汇总,欢迎大家查阅

> [大模型日报知识库(分享版)](https://miracleplus.feishu.cn/wiki/KapVwXfkriOBiskB0B2cUHxjnTh)

>

> 欢迎大家多多帮我们分享,转发~~

> 如果大家有任何的建议或者吐槽,欢迎随时找我聊天~

![](img/7b97110611522423d0f4793d77b02651.png)
