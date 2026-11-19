
# 大模型开发工程师 认证&学习指南

> 来源：[https://datawhaler.feishu.cn/docx/VPgKdiyeyoGPcuxWTcccuzosnUe](https://datawhaler.feishu.cn/docx/VPgKdiyeyoGPcuxWTcccuzosnUe)

## 《大模型开发工程师》认证说明

**欢迎参与 Datawhale 大模型开发工程师 计划**

**联合浪潮信息**

**面向在校学生、社会在职人士**

**可获官方颁发的大模型开发工程师证书**

1. **认证地址**：大模型开发工程师考试入口（电脑端打开）https://www.datawhale.cn/cert/llm-dev/quiz/intro

2. **评分标准**

+ 满分 100 分，成功获得 80 分及以上，可获得 Datawhale x 浪潮信息 联合颁发的 大模型开发工程师 证书

+ 可反复考试认证，通过学习本文档来提升分数

![](img/0a0e696d07acef77b6316ae71d26f94e.png)

> 此外，本文档也是**动手学大模型应用全栈开发**的Task1，是 Datawhale 2024 年 AI 夏令营 第四期联合浪潮信息一同开展的学习活动——
>
> + 适合想 入门并实践 大模型应用全栈开发 的学习者参与
> + 适合想成为 AI 大模型全栈工程师 的学习者参与

> **后续学习内容提要**：通过学习大模型部署、大模型检索增强生成（Retrieval Augmented Generation, RAG）实战、大模型微调实战，掌握大模型应用全栈开发

> 有进一步学习需求的小伙伴可按需继续往下学习~

## 📚 大模型应用开发必知必会

### 背景知识

#### 一、什么是大模型

为了对人类语言的内在规律进行建模，研究者们提出使用语言模型（language model）来准确预测词序列中 下一个词 或者 缺失的词 的概率。

如下图所示，截止到目前，语言模型已经演化了四代，分别是：

+ 统计语言模型（Statistical Language Model, SLM）：使用马尔可夫假设（Markov Assumption）来建模语言序列的 n 元（n-gram）语言模型

+ 神经语言模型（Neural Language Model, NLM）：基于神经网络构建语言模型，如循环神经网络（Recurrent Neural Networks, RNN），并学习上下文相关的词表示（即分布式词向量），也被称为词嵌入（Word Embedding）。代表性工作：word2vec

+ 预训练语言模型（Pre-trained Language Model, PLM）：使用大量的无标注数据预训练双向 LSTM（Bidirectional LSTM, biLSTM）或者Transformer，然后在下游任务上进行微调（Fine-Tuning）。代表性工作：ELMo、BERT、GPT-1/2

+ 大语言模型（Large Language Model, LLM）：基于“扩展法则”（Scaling Law），即通过增加模型参数或训练数据，可以提升下游任务的性能，同时具有小模型不具有的“涌现能力”（Emergent Abilities）。代表性工作：GPT-3、ChatGPT、Claude、Llama

![](img/673479fba17a3d2e68db982ff855d34e.png)

#### 二、大模型是怎么构建的

通常来说，大模型的构建过程可以分为预训练（Pretraining）、有监督微调（Supervised Fine-tuning, SFT）、基于人类反馈的强化学习对齐（Reinforcement Learning from Human Feedback, RLHF）三个阶段。接下来，我们依次介绍一下这三个阶段：

##### 预训练

预训练指使用海量的数据进行模型参数的初始学习，旨在为模型参数寻找到一个优质的“起点”。这一概念最初在计算机视觉领域萌芽，通过在ImageNet（一个大型图像数据集）上的训练，为模型参数奠定了坚实的基础。随后，这一理念被自然语言处理（NLP）领域采纳，word2vec开创先河，利用未标注文本构建词嵌入模型；ELMo、BERT及GPT-1则进一步推动了“预训练-微调”范式的普及。

起初，预训练技术专注于解决特定类别的下游任务，例如文本分类、序列标注、序列到序列生成等传统NLP任务。OpenAI在GPT-2的研究中，提出了一种创新思路——通过大规模文本数据预训练，打造能够应对广泛任务的通用解决方案，并在GPT-3中将这一理念扩展至前所未有的超大规模。

在BERT等早期预训练模型中，模型架构和训练任务呈现出多样化特征。然而，随着GPT系列模型的兴起，“解码器架构+预测下一个词”的策略证明了其卓越效能，成为了当前主流的大模型技术路线。

在预训练过程中，首要任务是搜集和清洗海量的文本数据，确保剔除潜在的有害内容。鉴于模型的知识库几乎完全源自训练数据，数据的质量与多样性对模型性能至关重要。因此，获取高质、多元的数据集，并对其进行严谨的预处理，是打造高性能语言模型的关键步骤。

当前，多数开源模型的预训练均基于数T的token。例如，Llama-1、Llama-2、Llama-3的预训练规模分别为1T、2T和15T。除了对数据量的苛刻要求，预训练阶段对计算资源的需求也极为庞大。以Llama-1的65B参数模型为例，其在2,048块A100 80GB GPU集群上进行了接近三周的训练。此外，预训练过程中还涉及诸多细节，诸如数据配比、学习率调度、模型行为监测等，这些往往缺乏公开的最佳实践指导，需要研发团队具备深厚的训练经验与故障排查能力，以规避训练过程中的回溯与重复迭代，节约计算资源，提高训练效率。

总体而言，预训练不仅是一项技术挑战，更是一场对数据质量、算力投入与研发智慧的综合考验。

##### 有监督微调

经过大规模预训练后，模型已经具备较强的模型能力，能够编码丰富的世界知识，但是由于预训练任务形式所限，这些模型更擅长于文本补全，并不适合直接解决具体的任务。

尽管引入了诸如上下文学习（In-Context Learning, ICL）等提示学习策略以增强模型的适应性，但模型本身在下游任务解决上的能力仍受限。为了克服这一局限，预训练后的大型语言模型往往需经历微调过程，以提升其在特定任务中的表现。

目前来说，比较广泛使用的微调技术是“有监督微调”（也叫做指令微调，Instruction Tuning），该方法利用成对的任务输入与预期输出数据，训练模型学会以问答的形式解答问题，从而解锁其任务解决潜能。经过指令微调后，大语言模型能够展现出较强的指令遵循能力，可以通过零样本学习的方式解决多种下游任务。

然而，值得注意的是，指令微调并非无中生有地传授新知，而是更多地扮演着催化剂的角色，激活模型内在的潜在能力，而非单纯地灌输信息。

相较于预训练所需的海量数据，指令微调所需数据量显著减少，从几十万到上百万条不等的数据，均可有效激发模型的通用任务解决能力，甚至有研究表明，少量高质量的指令数据（数千至数万条）亦能实现令人满意的微调效果。这不仅降低了对计算资源的依赖，也提升了微调的灵活性与效率。

##### 基于人类反馈的强化学习对齐

除了提升任务的解决能力外，大语言模型与人类期望、需求及价值观的对齐（Alignment）至关重要，这对于大模型的应用具有重要的意义。

OpenAI在 2022 年初发布了 InstructGPT 论文，详尽阐述了如何实现语言模型与人类对齐。论文提出了基于人类反馈的强化学习对齐（Reinforcement Learning from Human Feedback, RLHF），通过指令微调后的强化学习，提升模型的人类对齐度。RLHF的核心在于构建一个反映人类价值观的奖励模型（Reward Model）。这一模型的训练依赖于专家对模型多种输出的偏好排序，通过偏好数据训练出的奖励模型能够有效评判模型输出的质量。

目前还有很多工作试图去除复杂的强化学习算法，或其他使用 SFT 方式来达到与 RLHF 相似的效果，从而简化模型的对齐过程。例如，直接偏好优化（Direct Preference Optimization, DPO），它通过与有监督微调相似的复杂度实现模型与人类对齐，是一种典型的不需要强化学习的对齐算法。相比RLHF，DPO不再需要在训练过程中针对大模型进行采样，同时超参数的选择更加容易。

最后，我们以开源大模型Llama-2-Chat为例，简要介绍一下其训练过程。

![](img/098db6a2fb8b20fdb709a823d64c09d1.png)

整个过程起始于利用公开数据进行预训练，获得Llama-2。在此之后，通过有监督微调创建了Llama-2-Chat的初始版本。随后，使用基于人类反馈的强化学习（RLHF）方法来迭代地改进模型，具体包括拒绝采样（Rejection Sampling）和近端策略优化（Proximal Policy Optimization, PPO）。在RLHF阶段，人类偏好数据也在并行迭代，以保持奖励模型的更新。

#### 三、开源大模型和闭源大模型

根据上面的学习，我们不难发现，构建大模型不仅需要海量的数据，更依赖于强大的计算能力，以确保模型能够快速迭代和优化，从而达到预期的性能水平。鉴于此，全球范围内能够独立承担起如此庞大计算成本的机构屈指可数。这些机构可以分为以下两大阵营：

+ 一是选择将模型开源的组织，他们秉持着促进学术交流和技术创新的理念，让全球的研究者和开发者都能受益于这些模型。通过开放模型的代码和数据集，他们加速了整个AI社区的发展，促进了创新和技术的民主化。这一阵营的代表有Meta AI、浪潮信息等。

+ 另一类则是保持模型闭源的公司，它们通常将模型作为核心竞争力，用于提供专有服务或产品，以维持商业优势。闭源模型通常伴随着专有技术和服务，企业可以通过API等方式提供给客户使用，而不直接公开模型的细节或代码。这种模式有助于保障公司的商业利益，同时也为用户提供了稳定和安全的服务。这一阵营的代表有OpenAI、百度等。

无论是开源还是闭源，这些大模型都在推动人工智能领域向前发展，对于推动大语言模型技术的渐进式发展起到了至关重要的作用。

接下来，我们以浪潮信息为例，简要介绍下浪潮信息源大模型开源体系。

#### 四、源大模型开源体系

截止到目前，浪潮信息已经发布了三个大模型：源1.0 ，源2.0 和 源2.0-M32，其中 源1.0 开放了模型API、高质量中文数据集和代码，源2.0 和 源2.0-M32 采用全面开源策略，全系列模型参数和代码均可免费下载使用。

2021年9月，源1.0大模型发布，它采用76层的Transformer Decoder结构，使用5T数据训练，拥有2457亿参数量，超越OpenAI研发的GPT-3，成为全球最大规模的AI巨量模型，表现出了出色的中文理解与创作能力。

+ [项目链接](https://github.com/Shawn-IEITSystems/Yuan-1.0)

+ [官方报道](https://mp.weixin.qq.com/s/6CH0I4eOLzj3YDZyIxdeEQ)

+ [论文链接](https://arxiv.org/abs/2110.04725)

2023年11月，源2.0大模型发布，它使用10T数据训练，包括1026亿、518亿、21亿 三款参数规模，在数理逻辑、代码生成等方面表现出色。

在算法方面，与传统Attention对输入的所有文字一视同仁不同，源2.0提出了局部注意力过滤增强机制（Localized Filtering-based Attention, LFA），它假设自然语言相邻词之间有更强的语义关联，因此针对局部依赖进行了建模，最后使得模型精度提高3.53%。

+ [项目链接](https://github.com/IEIT-Yuan/Yuan-2.0)

+ [官方报道](https://mp.weixin.qq.com/s/rjnsUS83TT7aEN3r2i0IPQ)

+ [论文链接](https://arxiv.org/abs/2311.15786)

2024年5月，源2.0-M32发布，它是一个混合专家（Mixture of Experts, MoE）大模型，使用2000B Tokens训练，包含400亿参数，37亿激活参数

源2.0-M32 包含32个专家，基于LFA+Attention Router的MoE模型结构。

源2.0-M32 在数理逻辑、代码生成、知识等方面精度对标Llama3-70B，推理算力降至1/19。

+ [项目链接](https://github.com/IEIT-Yuan/Yuan2.0-M32)

+ [官方报道](https://mp.weixin.qq.com/s/WEVyYq9BkTTlO6EAfiCf6w)

+ [论文链接](https://arxiv.org/abs/2405.17976)

![](img/bf24249e425ef4bb21e71e6bb7cc6b5b.png)

本次学习，我们将以 源2.0-2B 模型为例，带领大家一起体验源大模型，欢迎大家来给项目点点 star 哦！

#### 五、大模型时代挖掘模型能力的开发范式

进入大模型时代，人工智能领域的边界正以前所未有的速度扩展，而如何充分挖掘大模型的内在潜能，成为了应用开发者面前的一道关键课题。

在这一背景下，不同的应用场景催生了多样化的应用开发策略，这些策略不仅展现了大模型应用开发的丰富可能性，也预示着未来AI技术在各行业落地的广阔前景。

##### 有手就行的Prompt工程

Prompt工程（Prompt Engineering）是指通过精心构造提示（Prompt），直接调教大模型，解决实际问题。

为了更充分地挖掘大模型的潜能，出现了以下两种技术：

+ 上下文学习（In-Context Learning, ICL）：将任务说明及示例融入提示文本之中，利用模型自身的归纳能力，无需额外训练即可完成新任务的学习。

+ 思维链提示（Chain-of-Thought, CoT）：引入连贯的逻辑推理链条至提示信息内，显著增强了模型处理复杂问题时的解析深度与广度。

##### Embedding辅助给LLM外接大脑

尽管大模型具有非常出色的能力，然而在实际应用场景中，仍然会出现大模型无法满足我们需求的情况，主要有以下几方面原因：

+ 知识局限性：大模型的知识来源于训练数据，而这些数据主要来自于互联网上已经公开的资源，对于一些实时性的或者非公开的，由于大模型没有获取到相关数据，这部分知识也就无法被掌握。

+ 数据安全性：为了使得大模型能够具备相应的知识，就需要将数据纳入到训练集进行训练。然而，对于企业来说，数据的安全性至关重要，任何形式的数据泄露都可能对企业构成致命的威胁。

+ 大模型幻觉：由于大模型是基于概率统计进行构建的，其输出本质上是一系列数值运算。因此，有时会出现模型“一本正经地胡说八道”的情况，尤其是在大模型不具备的知识或不擅长的场景中。

因此，将知识提前转成Embedding向量，存入知识库，然后通过检索将知识作为背景信息，这样就相当于给LLM外接大脑，使大模型能够运用这些外部知识，生成准确且符合上下文的答案，同时能够减少模型幻觉的出现。

##### 参数高效微调

在实际应用场景中，大模型还会经常出现以下问题：

+ 大模型在当前任务上能力不佳，如果提升其能力？

+ 另外，怎么使大模型学习其本身不具备的能力呢？

这些问题的答案是模型微调。

模型微调也被称为指令微调（Instruction Tuning）或者有监督微调（Supervised Fine-tuning, SFT），首先需要构建指令训练数据，然后通过有监督的方式对大模型的参数进行微调。经过模型微调后，大模型能够更好地遵循和执行人类指令，进而完成下游任务。

然而，由于大模型的参数量巨大， 进行全量参数微调需要消耗非常多的算力。为了解决这一问题，研究者提出了参数高效微调（Parameter-efficient Fine-tuning），也称为轻量化微调 （Lightweight Fine-tuning），这些方法通过训练极少的模型参数，同时保证微调后的模型表现可以与全量微调相媲美。

#### 六、大模型应用开发 必知必会

通常，一个完整的大模型应用包含一个客户端和一个服务端。

客户端接收到用户请求后，将请求输入到服务端，服务端经过计算得到输出后，返回给客户端回复用户的请求。

##### 客户端

在大模型应用中，客户端需要接受用户请求，并且能将回复返回给用户。

目前，客户端通常使用 [Gradio](https://www.gradio.app/) 和 [Streamlit](https://streamlit.io/) 进行开发。

**Gradio 基本概念**

![](img/07f575263e5bc5a722a0390693c6fab6.png)

[Gradio](https://www.gradio.app/) 有输入输出组件、控制组件、布局组件几个基础模块，其中

+ **输入输出组件**用于展示内容和获取内容，如：`Textbox`文本、`Image`图像



## Gradio 与 Streamlit 组件介绍

### 布局组件

`布局组件`用于更好地规划组件的布局，如：`Column`（把组件放成一列）、`Row`（把组件放成一行）

推荐使用`gradio.Blocks()`做更多丰富交互的界面，`gradio.Interface()`只支持单个函数交互

### 控制组件

`控制组件`用于直接调用函数，无法作为输入输出使用，如：`Button`（按钮）、`ClearButton`（清除按钮）

> Gradio的设计哲学是将输入和输出组件与布局组件分开。输入组件（如`Textbox`、`Slider`等）用于接收用户输入，输出组件（如`Label`、`Image`等）用于显示函数的输出结果。而布局组件（如`Tabs`、`Columns`、`Row`等）则用于组织和排列这些输入和输出组件，以创建结构化的用户界面。

[如果想了解更多组件详情，可查看官方文档](https://www.gradio.app/guides/quickstart)；

[如果想设计更复杂的界面风格，还可以查看学习官方文档：主题](https://www.gradio.app/guides/theming-guide)；

### Streamlit 基础概念

[`Streamlit`](https://streamlit.io/) 官方的视频演示如下，我们可以直接输入markdown格式的文本，网页即可渲染好。

![](img/4d4702058657efa1ba846946596ac847.png)

[`hero-video.mp4`](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YVZAbhSeLoOHXxxxMF4cRdYinAf)

+   [`Streamlit`](https://streamlit.io/) 中没有`gradio`的输入和输出概念，也没有布局组件的概念。

+   [`Gradio`](https://www.gradio.app/)和[`Streamlit`](https://streamlit.io/)中的【文件组件】对比如下图：

![](img/729cf7c633cf2e1f4c79f34d306bb421.png)

![](img/9efc0b41d85031b44f4841d18fa58fc9.png)

+   [`Streamlit`](https://docs.streamlit.io/develop/api-reference)每个组件都是独立的，需要用什么直接查看官方文档即可，大致有如下几种组件：

### 页面元素

+   文本

+   数据表格

+   图标绘制（柱状图，散点图等等）

+   输入（文本框，按钮，下拉框，滑块，复选框，文件上传，等等）

+   多媒体（图片，音频，视频）

+   布局和容器

+   Chat（聊天对话控件）

+   状态（进度条，加载中，等等元素）

+   第三方组件（提供了更加丰富的组件）

### 应用逻辑

+   导航和页面（可以切换页面）

+   执行流程

+   缓存和状态

+   连接和加密（可连接数据库，也可以对内容进行加密处理）

+   自定义组件

+   公共组件（用户信息存储，帮助，以及输出`html`）

+   `Config`（使用配置文件，来定义一些内容）

### 工具

+   应用测试

+   命令行

## 服务端

在大模型应用中，服务端需要与大模型进行交互，大模型接受到用户请求后，经过复杂的计算，得到模型输出。

目前，服务端主要有以下两种方式：

### 直接调用大模型API

将请求直接发送给相应的服务商，如`openai`，讯飞星火等，等待`API`返回大模型回复

✔️ 优点：

1.  **便捷性**： 不需要关心模型的维护和更新，服务商通常会负责这些工作。

1.  **资源效率**： 避免了本地硬件投资和维护成本，按需付费，灵活调整成本支出。

1.  **稳定性与安全性**： 专业团队管理，可能提供更好的系统稳定性和数据安全性措施。

1.  **扩展性**： `API`服务易于集成到现有的应用和服务中，支持高并发请求。

✖️ 缺点：

1.  **网络延迟**： 需要稳定的网络连接，可能会受到网络延迟的影响。

1.  **数据隐私**： 数据需要传输到服务商的服务器，可能涉及数据安全和隐私问题。

1.  **成本控制**： 高频次或大量数据的调用可能会导致较高的费用。

1.  **依赖性**： 受制于服务商的政策变化，如价格调整、服务条款变更等。

### 大模型本地部署

在本地`GPU`或者`CPU`上，下载模型文件，并基于推理框架进行部署大模型

✔️ 优点：

1.  **数据主权**： 数据完全在本地处理，对于敏感数据处理更为安全。

1.  **性能可控**： 可以根据需求优化配置，减少网络延迟，提高响应速度。

1.  **成本固定**： 初始投入后，长期运行成本相对固定，避免了按使用量付费的不确定性。

1.  **定制化**： 更容易针对特定需求进行模型微调或扩展。

✖️ 缺点：

1.  **硬件投资**： 需要强大的计算资源，如高性能`GPU`，初期投资成本较高。

1.  **运维复杂**： 需要自行管理模型的更新、维护和故障排查。

1.  **技术门槛**： 对于非专业团队而言，模型的部署和优化可能较为复杂。

1.  **资源利用率**： 在低负载情况下，本地硬件资源可能无法充分利用。

综上，选择哪种方式取决于具体的应用场景、数据敏感性、预算以及对延迟和性能的需求。

## 🔥 小白零基础 10 分钟 速通实践baseline

> 我们整个学习活动，将带你学习以下内容：

+   大模型部署【搭建你的智能编程助手】

+   大模型RAG实战【搭建你的AI科研助手】

+   大模型微调实战【搭建你的AI简历助手】

> 当前环节可让你在10分钟内，从一个完全小白的选手，搭建你的第一个大模型应用（智能编程助手）！

>

> 千里之行，始于足下，从这里，开启你的 AI 学习之旅吧！

> —— Datawhale贡献者团队

### Step0：开通阿里云PAI-DSW试用

> 链接：[https://free.aliyun.com/?productCode=learn](https://free.aliyun.com/?productCode=learn)

> **开通`PAI-DSW`试用，可获得`5000算力时`！有效期3个月！**

> 如果已经开通试用或试用已过期，可以跳过此步骤，使用魔搭免费GPU额度或寻找其他的算力方案

+   注意多模态数据合成方向无法通过魔搭免费GPU执行

+   开通免费试用

![](img/1f353dc0fe86821cbc17b540d4cc02a1.png)

![](img/fc48d3b5ac0acab9ede19dfeb75855f5.png)

![](img/1f8fdf7e16301e070beaa40d4231c747.png)

+   在[魔搭社区](https://www.modelscope.cn/my/mynotebook/authorization)进行授权

> 链接：https://www.modelscope.cn/my/mynotebook/authorization

> 如果这一步授权失败，可跳过此步骤，继续往下进行

![](img/f8ae7aeb334a487a006699eddbf23f60.png)

![](img/52ea53e146487202d52e24e853cd9034.png)

![](img/1145cd9bb52046fde36bf606d8336558.png)

![](img/27fa019b00bbfe3a69bfbbdd8fec9a30.png)

+   新用户需要先注册 & 绑定阿里云账号

![](img/ebd4e0d62bf67cef3cccc7cec3cfc1fe.png)

![](img/a707fa250152a41c44b8978589ba2c60.png)

![](img/c68cfc9c949d18593a4d289ed20acb20.png)

![](img/57d815522a865829e940999f8224900c.png)

![](img/7dfab231b0d9e8f49c1c37f49a30c783.png)

![](img/63a8e449e92ceb8b9ec5c5670881c00d.png)

+   新用户还需要完成阿里云账号的实名认证，直接完成即可

### Step1：在魔搭社区创建PAI实例！（点击即可跳转）

> 链接：https://www.modelscope.cn/my/mynotebook/authorization

> **创建实例，启动！**

> **如果在上一步授权失败，可到阿里云控制台创建 & 打开实例，具体步骤详见下方子步骤**

![](img/29006256f2342e7ab229aee974341648.png)

![](img/7d36dc01e1e9dd164adc7ae82c5165fb.png)

![](img/344904f2471c96cd843ff4a8778cd564.png)

![](img/fcb87173de576d318f574961df331f03.png)

![](img/c0c472c2882e34d298841861a44e9b46.png)

![](img/d1c991a550f97ecbf411afaeabc72df4.png)

![](img/72272dc868760153c4900004eb255ec5.png)

+   **如果在魔搭无法授权 或 点击【打开】无法打开，可到阿里云控制台创建 & 打开实例**

![](img/d6c534ffdc6c336437c1b2649a480eef.png)

![](img/6748b1dcc48c9795be4934b52b3d07f3.png)

+   创建实例

![](img/963276e6b48f01b0e4618a06ae00758e.png)

![](img/628f927b6620eb146befbf092250b684.png)

![](img/5a8661e4c4b97a85fd73140eeae043fc.png)

![](img/1b2f5fb57aadda39c6ba28e912f53eaf.png)

+   打开实例

![](img/32fcc15b0ea0b7757245f5921d0b7c42.png)

![](img/2ba0d5a117c6c8bbda12e9fc47544c03.png)

+   如果之前试用的额度已经过期，可使用魔搭的免费`Notebook`实例

![](img/b37d248c40ea5718094c44f564ffe4e4.png)

![](img/bac69dc58084dd8b799be652dded62b2.png)

### Step2：Demo搭建！

#### 第一步：文件下载

> 点击终端，然后输入如下命令，回车运行！

```
git lfs install
git clone https://www.modelscope.cn/datasets/Datawhale/AICamp_yuan_baseline.git
```

![](img/b6de73487a58505cccd310f025bd32d1.png)

![](img/88ed841bbe170eb94e6101bdeedd36a2.png)

#### 第二步：环境安装

> 点击终端，然后输入如下命令，回车运行！

```
pip install streamlit==1.24.0
```

![](img/7263f953f4f7082ee5f46f6dd5f85d00.png)

> 等待依赖安装成功！

![](img/4f5bd0a285622e0955d77030ecad1988.png)

#### 第三步：启动Demo

> 点击终端，然后输入如下命令，回车运行！

```
streamlit run AICamp_yuan_baseline/Task\ 1：零基础玩转源大模型/web_demo_2b.py --server.address 127.0.0.1 --server.port 6006
```

### Step3：对话体验！

> 点击链接，跳转到浏览器新页面！

> **注意⚠️：这里一定要保存效果截图去打卡哦！！一定要保存效果截图去打卡哦！！一定要保存效果截图去打卡哦！**

![](img/824880bfdef3dd3c2d1cf78e70422c31.png)

> 显示Demo名称，后台正在运行中！

![](img/670beae00ff85a9315c4da20d8f85b83.png)

> 等待后台完成模型下载和加载！

![](img/b215d57fdb845655e1d510352ca0ebbd.png)

> 弹出对话框，现在开始对话吧！

> 下面，让你的编程助手简单写个代码吧！输入后回车即可运行！

> 参考`Prompt`：

> 给我一个`python`打印`helloword`的代码

> 改成`c++`的

![](img/d91aaec28250556b391160c615de9b99.png)

可以看到，`Yuan2-2B-Mars` 模型不仅拥有出色的代码生成能力，而且能紧密跟随用户的指令，与上下文完美融合。（保存效果截图去打卡）

### 关闭PAI实例

<https://datawhaler.feishu.cn/sync/RMHtdxBnqshtNsbJHcqcMma2nLg>

### 🎉恭喜速通成功！👍🏻 快来给自己点赞！

学习活动已结束

> **学习活动期间，**大家完成打卡后就可以在表格里填写组队信息，积极主动寻找合适的队友

> 【腾讯文档】夏令营第四期大模型应用开发-找搭子辅助表- Task01打卡链接

### 📅 后续课程内容

| 动手学大模型应用全栈开发 |  |  |  |

| --- | --- | --- | --- |

| **Task** | **Task 描述** | **相关知识点** | **学习安排** |

| **Task 1** |  | 1.  大模型原理简介 | [开营直播(点击回放)](https://www.bilibili.com/video/BV13E4m1d7WH) |

| **Task 2** |  | 1.  热点idea &amp; 应用案例方向 | [头脑风暴会腾讯会议](https://meeting.tencent.com/v2/cloud-record/share?id=ce920da4-3dc0-40e9-a227-d2ef71b12e5f&amp;from=3&amp;is-single=false&amp;record_type=2)（密码：YM3N） |

| **Task 3** |  | 1.  基于源的RAG技术原理与实践 | RAG原理与实践直播(点击回放) |

| **Task 4** |  | 1.  源大模型微调技术原理与实践 | [微调原理与实践直播(点击回放)](https://www.bilibili.com/video/BV1ZTWLeoEyz) |

| **Task 5** |  | 1.  优化路演答辩PPT | [技术交流会腾讯会议(点击回放)](https://meeting.tencent.com/v2/cloud-record/share?id=1ec442be-25b9-42df-a1d2-bc7e533c57df&amp;from=3&amp;record_type=2) |

| **Task 6** |  | - | Demo Day1直播 |

|  |  |  |  |

> **打怪难度：⭐️**

1.  跑通 `baseline`，创建大模型应用！

1.  学习相关知识点，进行**《大模型开发工程师》认证**

1.  写笔记，填写**找搭子辅助表**

> **打怪难度：**简单

1.  参加头脑风暴会，进行组队

1.  写笔记，完成 `Task2` 打卡

> **打怪难度：⭐️⭐️**

1.  学习RAG技术与实战、群内交流分享

1.  组队进行应用开发

1.  写笔记，完成 `Task3` 打卡

> **打怪难度：⭐️⭐️**

1.  学习微调技术与实战、群内交流分享

1.  组队进行应用开发

1.  写笔记，完成 `Task4` 打卡

> **打怪难度：⭐️⭐️**

1.  组队进行应用开发

1.  参加技术交流会

1.  完成应用开发，**填写作品提交表**

> **打怪难度：⭐️⭐️⭐️**

1.  专家评审

1.  根据建议优化作品、报名Demo Day

### 祝大家开发顺利，学习愉快~ 多在群里提问和交流哟！

+   作品提交链接：**（即将开放提交，敬请期待~）**

+   作品提交规则&案例可见：[提交作品规则 & Task1案例：智能编程助手](https://datawhaler.feishu.cn/wiki/BxMQwyxBgiW7H4kmeHgcMeK1nzd)

+   提问前建议先查看`[ QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/Gf5DwJNXeiLOjDkMlPhcnwjTnMf)`是否早已收录你的问题~ （QA文档可任意编辑，助教和教程贡献者会不定时查看）

+   更多学习活动问题可见：`[动手学大模型应用全栈开发-学习者手册](https://datawhaler.feishu.cn/wiki/LSpmwpplNigJKwkvRYQcKxbTnkb)`

> 你还可以参考下面的学习方案，提升自己的学习效率——

+   定位自己的学习预期、设计自己的学习计划：每天花多少时间，做什么，大概能拿到什么结果

+   学习内容有很多，明确目标、按需学习自己所需要学习的内容即可

### 动手学大模型应用全栈开发-参考学习方案



# 学习方案与个人计划

| 学习方案类型 | 所需背景与能力 | 预期学习目标&收获 | 学习内容 & 参考时长 |
| --- | --- | --- | --- |
| 初级：60分，提见识 | 无 | + 跑通Task1内容，熟悉开发流程，构建自己的第一个大模型应用 | 1. [跑通Task1代码(30分钟)](https://datawhaler.feishu.cn/wiki/XJA9w5be6iiSDLk58LvcKhZvngh#WTw9dKhcIoeTv4xhNfecyx38nbc) |
| 进阶：80分，会应用 | + Python基础 | + 学习大模型应用开发通识，考取《大模型应用开发工程师》证书 | 1. [跑通Task1代码(30分钟)](https://datawhaler.feishu.cn/wiki/XJA9w5be6iiSDLk58LvcKhZvngh#WTw9dKhcIoeTv4xhNfecyx38nbc) |
| 高级：90+分，能创新 | + Python开发能力 | + 学习大模型应用开发通识，考取《大模型应用开发工程师》证书 | 1. [跑通Task1代码(30分钟)](https://datawhaler.feishu.cn/wiki/XJA9w5be6iiSDLk58LvcKhZvngh#WTw9dKhcIoeTv4xhNfecyx38nbc) |

+ 加入RAG，微调等技术，增强应用的使用价值
+ 做好相关提交材料的优化和制作

## 个人学习计划（复制到自己的文档中自行填写即可～）

| 序号 | 日期 | 今日目标 | 今日学习资料 & 学习计划 |
| --- | --- | --- | --- |
| Day1 | 8月9日 | 跑通Baseline并学习大模型开发基础 | [动手学大模型应用全栈开发](https://datawhaler.feishu.cn/wiki/XJA9w5be6iiSDLk58LvcKhZvngh) |
| Day2 | 8月10日 | 继续学习大模型开发基础并完成认证 | [动手学大模型应用全栈开发](https://datawhaler.feishu.cn/wiki/XJA9w5be6iiSDLk58LvcKhZvngh) |
| Day3 | 8月12日 | 完成组队 | [提交作品规则 & Task1案例：智能编程助手](https://datawhaler.feishu.cn/wiki/BxMQwyxBgiW7H4kmeHgcMeK1nzd) |
| Day4 | 8月13日 | 线上头脑风暴 | [Task2：头脑风暴会](https://datawhaler.feishu.cn/wiki/EPSuwBxEIi6btekScZRc8RIqn3H)[Task2：线上Hackathon必知必会](https://datawhaler.feishu.cn/wiki/JBbqwASUwip0hSkJYdMcTTdUnNp) |
| Day5 | 8月14日 | RAG技术原理与实践 | [Task3：源大模型RAG实战](https://datawhaler.feishu.cn/wiki/NNKJwO0q7iC3uakHjkhcwLhbn9d) |
| Day6 | 8月15日 | 案例分析 | [Task3案例：AI科研助手](https://datawhaler.feishu.cn/wiki/VqMbwzvPsilBWLkWX8zcIVXKn5g) |
| Day7 | 8月18日 | 模型微调技术原理与实践 | [Task4：源大模型微调实战-知识点文档](https://datawhaler.feishu.cn/wiki/PLCHwQ8pai12rEkPzDqcufWKnDd) |
| ··· | ··· | ··· | ··· |

## 来精读一下`baseline`

### 完整`baseline`代码

```
# 导入所需的库
from transformers import AutoTokenizer, AutoModelForCausalLM
import torch
import streamlit as st

# 创建一个标题和一个副标题
st.title("💬 Yuan2.0 智能编程助手")

# 源大模型下载
from modelscope import snapshot_download
model_dir = snapshot_download('IEITYuan/Yuan2-2B-Mars-hf', cache_dir='./')
# model_dir = snapshot_download('IEITYuan/Yuan2-2B-July-hf', cache_dir='./')

# 定义模型路径
path = './IEITYuan/Yuan2-2B-Mars-hf'
# path = './IEITYuan/Yuan2-2B-July-hf'

# 定义模型数据类型
torch_dtype = torch.bfloat16 # A10
# torch_dtype = torch.float16 # P100

# 定义一个函数，用于获取模型和tokenizer
@st.cache_resource
def get_model():
    print("Creat tokenizer...")
    tokenizer = AutoTokenizer.from_pretrained(path, add_eos_token=False, add_bos_token=False, eos_token='<eod>')
    tokenizer.add_tokens(['<sep>', '<pad>', '<mask>', '<predict>', '<FIM_SUFFIX>', '<FIM_PREFIX>', '<FIM_MIDDLE>', '<commit_before>', '<commit_msg>', '<commit_after>', '<jupyter_start>', '<jupyter_text>', '<jupyter_code>', '<jupyter_output>', '<empty_output>'], special_tokens=True)

    print("Creat model...")
    model = AutoModelForCausalLM.from_pretrained(path, torch_dtype=torch_dtype, trust_remote_code=True).cuda()

    print("Done.")
    return tokenizer, model

# 加载model和tokenizer
tokenizer, model = get_model()

# 初次运行时，session_state中没有"messages"，需要创建一个空列表
if "messages" not in st.session_state:
    st.session_state["messages"] = []

# 每次对话时，都需要遍历session_state中的所有消息，并显示在聊天界面上
for msg in st.session_state.messages:
    st.chat_message(msg["role"]).write(msg["content"])

# 如果用户在聊天输入框中输入了内容，则执行以下操作
if prompt := st.chat_input():
    # 将用户的输入添加到session_state中的messages列表中
    st.session_state.messages.append({"role": "user", "content": prompt})

    # 在聊天界面上显示用户的输入
    st.chat_message("user").write(prompt)

    # 调用模型
    prompt = "<n>".join(msg["content"] for msg in st.session_state.messages) + "<sep>" # 拼接对话历史
    inputs = tokenizer(prompt, return_tensors="pt")["input_ids"].cuda()
    outputs = model.generate(inputs, do_sample=False, max_length=1024) # 设置解码方式和最大生成长度
    output = tokenizer.decode(outputs[0])
    response = output.split("<sep>")[-1].replace("<eod>", '')

    # 将模型的输出添加到session_state中的messages列表中
    st.session_state.messages.append({"role": "assistant", "content": response})

    # 在聊天界面上显示模型的输出
    st.chat_message("assistant").write(response)
```

### `baseline`方案设计

#### 2.1 概要设计

`baseline`基于源大模型的编程能力来解决用户的问题，主要包含一个`Streamlit`开发的客户端，以及一个部署好浪潮源大模型的服务端。客户端接收到用户请求后，首先进行交互历史拼接，然后输入到服务端的浪潮源大模型，得到模型输出结果后，返回给客户端，用于回复用户的问题。

#### 2.2 详细设计

1. **导入库：**

导入所需要的依赖，包括`transformers`，`torch`和`streamlit`。其中`torch`魔搭本身已经安装，`transformers`和`streamlit`在第二步也安装完毕。

1. **模型下载：**

`Yuan2-2B-Mars`支持通过多个平台进行下载，包括魔搭、HuggingFace、OpenXlab、百度网盘、WiseModel等。因为我们的机器就在魔搭，所以这里我们直接选择通过魔搭进行下载。模型在魔搭平台的地址为`IEITYuan/Yuan2-2B-Mars-hf`。

模型下载使用的是`modelscope`中的`snapshot_download`函数，第一个参数为模型名称`IEITYuan/Yuan2-2B-Mars-hf`，第二个参数`cache_dir`为模型保存路径，这里`.`表示当前路径。

模型大小约为4.1G，由于是从魔搭直接进行下载，速度会非常快。下载完成后，会在当前目录增加一个名为`IEITYuan`的文件夹，其中`Yuan2-2B-Mars-hf`里面保存着我们下载好的源大模型。

![](img/0616f823eb55ce3f6bfaa4ac52ef512e.png)

1. **模型加载：**

使用`transformers`中的`from_pretrained`函数来加载下载好的模型和`tokenizer`，并通过`.cuda()`将模型放置在GPU上。另外，这里额外使用了`streamlit`提供的一个装饰器`@st.cache_resource`，它可以用于缓存加载好的模型和`tokenizer`。

1. **读取用户输入：**

使用`streamlit`提供的`chat_input()`来获取用户输入，同时将其保存到对话历史里，并通过`st.chat_message("user").write(prompt)`在聊天界面上进行显示。

1. **对话历史拼接：**

对于`Yuan2-2B-Mars`模型来说，输入需要在末尾添加`<sep>`，模型输出到`<eod>`结束。如果输入是多轮对话历史，需要使用`<n>`进行拼接，并且在末尾添加`<sep>`。

1. **模型调用：**

输入的`prompt`需要先经`tokenizer`切分成`token`，并转成对应的`id`，并通过`.cuda()`将输入也放置在GPU上。然后调用`model.generate()`生成输出的`id`，并通过`tokenizer.decode()`将`id`转成对应的字符串。最后从字符串中提取模型生成的内容（即`<sep>`之后的字符串），并删除末尾的`<eod>`，得到最终的回复内容。

1. **显示模型输出：**

得到回复内容后，将其保存到对话历史里，并通过`st.chat_message("assistant").write(response)`在聊天界面上进行显示。

## 尝试替换其他Yuan大模型！

> 8月10日更新：

> 浪潮信息源大模型上新：`Yuan2-2B-July-hf`，但由于模型结构修改，暂不支持`LoRA`~

### 部分Yuan大模型版本信息表

| 模型规格 | 最低GPU显 | 最低GPU显存 | 不同版本的`snapshot_download`地址 | 魔搭上的模型地址 |
| --- | --- | --- | --- | --- |
| **2B** | 6GB | 80GB | `IEITYuan/Yuan2-2B-Mars-hf` | [`IEITYuan/Yuan2-2B-Mars-hf`](https://modelscope.cn/models/IEITYuan/Yuan2-2B-Mars-hf) |
|  |  |  | `IEITYuan/Yuan2-2B-July-hf` | [`IEITYuan/Yuan2-2B-July-hf`](https://modelscope.cn/models/IEITYuan/Yuan2-2B-July-hf) |
|  |  |  | `IEITYuan/Yuan2-2B-Janus-hf` | [`IEITYuan/Yuan2-2B-Janus-hf`](https://modelscope.cn/models/IEITYuan/Yuan2-2B-Janus-hf) |
|  |  |  | `IEITYuan/Yuan2-2B-Februa-hf-GGUF` | [`IEITYuan/Yuan2-2B-Februa-hf-GGUF`](https://www.modelscope.cn/models/IEITYuan/Yuan2-2B-Februa-hf/files) |
| **51B** | 4*80GB | 32*80GB | `IEITYuan/Yuan2.0-51B-hf` | [`IEITYuan/Yuan2.0-51B-hf`](https://modelscope.cn/models/IEITYuan/Yuan2.0-51B-hf) |
| **102B** | 8*80GB | 96*80GB | `IEITYuan/Yuan2.0-102B-hf` | [`IEITYuan/Yuan2.0-102B-hf`](https://modelscope.cn/models/IEITYuan/Yuan2.0-102B-hf) |

# 关于大模型应用开发的后续学习

## 大模型基础 [`so-large-lm`](https://github.com/datawhalechina/so-large-lm)

> Datawhale开源教程，掌握大模型理论知识！

## 开源大模型食用指南 [`self-llm`](https://github.com/datawhalechina/self-llm)

> Datawhale开源教程，速通大模型部署&微调！

## 大模型白盒子构建指南 [`tiny-universe`](https://github.com/datawhalechina/tiny-universe)

> Datawhale开源教程，围绕大模型全链路的“手搓”大模型指南！

## 动手学大模型应用开发 [`llm-universe`](https://github.com/datawhalechina/llm-universe)

> Datawhale开源教程，一个面向小白开发者的大模型应用开发教程！

> 大家在本次学习过程中，可以一边将上面推荐的材料作为字典，遇到问题时通过这些材料、大模型、搜索引擎，去翻阅查看，做到理论和实践的结合，真正理解这些理论知识的价值、以及究竟要如何在真实的问题场景中使用。



# 更多QA详见[QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/Gf5DwJNXeiLOjDkMlPhcnwjTnMf)！

> [QA & 其他彩蛋](https://datawhaler.feishu.cn/wiki/Gf5DwJNXeiLOjDkMlPhcnwjTnMf) 任何想法都可以在学习群里交流或在进行填写哟~

> 本教程主要由写作贡献，
> 融合了多位助教的内测体验和宝贵建议。
> 如果你觉得对你有价值和启发，请在下面点赞和评论哟~

## 另外别忘了写笔记打卡呀！！~

## 及时思考、沉淀和产出能让你的学习事半功倍！

> **注：本文采用 署名—非商业性使用—相同方式共享 4.0 协议国际版许可协议进行许可。**
