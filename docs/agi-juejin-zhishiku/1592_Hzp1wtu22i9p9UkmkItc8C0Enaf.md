
# Agent 应用于提示工程

> 来源：[https://agijuejin.feishu.cn/docx/Qlw4dZ4j6o7PHkxxQxsc7AbUn7c](https://agijuejin.feishu.cn/docx/Qlw4dZ4j6o7PHkxxQxsc7AbUn7c)

> 作者：半吊子全栈工匠 | 喔家ArchiSelf

> 原文：https://mp.weixin.qq.com/s/VDefk6v7q8wE0y0_bckDQw

如果Agent模仿了人类在现实世界中的操作方式，那么，能否应用于提示工程即Prompt Engingeering 呢？

## 从LLM到Prompt Engineering

大型语言模型(LLM)是一种基于Transformer的模型，已经在一个巨大的语料库或文本数据集上进行了训练，包括了互联网上的大多数网页。在训练期间，需要花费大量的时间(和/或图形处理器)、能量和水(用于冷却) ，梯度下降法被用来优化模型的参数，以便它能够很好地预测训练数据。

![](img/445c3548d530b4b3804bce33231ccf89.png)

本质上，LLM 学习根据前面的词序预测最可能的下一个词。这可以用来执行推理即查找模型生成某些文本的可能性，或者用来生成文本，像 ChatGPT 这样的 LLM 使用这些文本与人进行对话。一旦 LLM 完成了训练，意味着它的参数被保存，不会向训练数据或重新训练的数据中添加输入。值得注意的是，LLM 仍然会从训练数据中学到偏见，而 ChatGPT 背后的 OpenAI 公司不得不增加安全措施，使用来自人类反馈的强化学习(RLHF)以防止该模型产生有问题的内容。此外，由于 LLM 默认情况下只是根据他们所看到的内容生成最有可能的下一个单词，而没有进行任何事实核查或推理，因此它们容易产生幻觉，或者编造事实，以及推理错误。

自从 ChatGPT 公开发布以来，LLM 风靡全球。这些模型的新兴智能以及它们在我们生活中的各个方面的应用，使它们成为一种非常受欢迎的工具，每家公司都想从中分一杯羹。除了聊天机器人以及编码和写作助手，LLM 还被用来创建与包括互联网在内的模拟环境交互的Agent。

如果尝试过 ChatGPT，有时会发现它的回答很糟糕，但是如果重新措辞这个问题，可能会得到更好的结果。这就是提示工程的艺术，通过修改输入使 LLM 以您希望的方式作出响应。提示语的背后只是尝试通过提出一些事实来“唤醒LLM的记忆”，或者告诉它一个新的领域，然后从一个训练好的 LLM 那里得到适当的响应。这就是所谓的上下文学习，一般主要有两种类型: zero-shot和few-shot。zero-shot为 LLM 提供了一个提示，可以在问题/命令之前包含一些背景信息，以帮助 LLM 找到一个好的响应。few-shot给 LLM 提供了一些示例提示和可取的响应，然后提出一个新的提示，LLM 将以示例的格式响应这些提示。

提示工程可能是自然语言处理(NLP)的未来趋势之一。这个领域正在从定制模型转向定制提示，因为 LLM 比任何人在没有大量时间和精力的情况下自己制作的语言模型要好得多。当 LLM 与正确的提示工程技术配对时，通常情况下，它可以完成专用模型所能完成的任何工作。

![](img/b4a8efffd3204de244f14ffa672f6f90.png)

## 从CoT到Agent

思维链推理（CoT）是一种流行的提示工程技术，旨在对抗推理错误。它包括给 LLM 提供一个或多个例子，说明如何通过语言推理解决问题，然后给它一个不同的问题，以这种方式解决。这可以帮助克服推理的错误，但它仍然受到幻觉的影响以及幻觉的“事实”可以通过推理传播，导致模型得出错误的结论。

只提出一个问题的标准提示语被比作思维链(CoT)提示的输入 ，需要多步推理才能解决问题。由于幻觉的问题，使用 CoT 提示的 LLM 有更令人信服的响应，但它仍然有可能是错误的。如果 LLM 要成为一个有用的工具，如果不能左右它胡编乱造，我们就永远不能相信它们，最好还是自己做研究。而Agent似乎可以解决这个问题，允许 LLM 采取行动，比如搜索维基百科，以便从中找到事实和理由。

## Agent 应用于提示工程示例——ReAct

与思维链推理一样，ReAct 也是一种提示工程方法，它使用少量学习来教模型如何解决问题。CoT 被认为是模仿人类如何思考问题，ReAct 也包括了这个推理元素，但它更进一步，允许Agent操作文本，让它与环境互动。人类使用语言推理来帮助我们制定策略并记住事情，但也可以采取行动来获得更多的信息并实现目标。这就是 ReAct 的基础。ReAct 提示包括行动的例子、通过行动获得的观察结果，以及人类在过程中各个步骤中转录的思想(推理策略)。LLM 学习模仿这种交叉思考和行动的方法，使其成为其环境中的Agent。

一定要记住，观察结果不是由 LLM 生成的，而是由环境生成的，环境是一个单独的模块，LLM 只能通过特定的文本操作与之交互。因此，为了实现 ReAct，需要:

1. 一种环境，它采取一个文本操作, 从一组可以根据环境的内部状态改变的潜在操作中返回一个文本观察。

1. 一个输出解析器框架，一旦Agent编写了一个有效的操作，它就停止生成文本，在环境中执行该操作，并返回观察结果, 一般是将其追加到目前生成的文本中，并用该结果提示 LLM。

1. 人工产生的示例，混合了思想，行动和观察，在环境中可以使用few-shot，例子的数量和细节取决于目标和开发者的设计，例如：

![](img/cd2f443e9d16166a8434b96ee266242f.png)

这里，思想、动作和观察都被清楚地标记如上，而且动作使用了一种特殊的格式，查询放在括号中，这样Agent就可以学习以这种方式编写任务，然后输出解析器就可以轻松地提取查询。

Yao 等人(2023)使用了 PalM-540B，用这个 LLM 在两个知识密集型推理任务和两个决策任务上测试了 基于ReAct 的提示工程效果。

### 知识密集型推理任务

在这个任务领域中使用的两个域是 HotPotQA 和 FEVER，前者是使用 Wikipedia 段落进行多跳问答，后者是事实验证。Agent可以使用以下操作与Wikipedia API 进行交互:

+ Search: 根据名称或大多数相似结果列表查找页面。

+ 查找: 在页中查找字符串。

+ 以答案结束任务。

在 HotPotQA 和 FEVER 这两个评估领域中， ReAct 与如下提示工程 技术进行了比较，结果是通过 FEVER 的准确性和 HotPotQA 中 EM 来评估的。

![](img/d5a9adae9d7561edda8298eaddbfdca8.png)

其中：

+ 标准Prompt: 没有思想，行动，或观察。

+ CoT: 没有及时的行动或观察。

+ CoT-SC (自我一致性) : 从 LLM 中抽取一定数量的回答，并选择大多数作为回答。

+ 动作: 没有思维的prompt。

+ ReAct ->CoT-SC: 开始时是ReAct，然后转换为 CoT-SC。

+ CoT-SC-> ReAct: 开始时是 CoT-SC，然后切换到 ReAct。

ReAct 在 HotPotQA 中表现不佳，但在 FEVER 中表现优于 CoT。ReAct 比 CoT 更不容易产生幻觉，但是有更高的推理错误率。尽管 ReAct 确实有这个缺点，但是 ReAct-> CoT-SC 和 CoT-SC-> ReAct 方法是其中最成功的。

### 决策任务

测试ReAct 的两个决策任务是 ALFWorld 和 WebShop。

ALFWorld 是一个基于文本的游戏，具有真实的环境。它具有用于在模拟世界中移动和交互的文本操作，例如“打开抽屉1”给代理人的一个目标可能是在房子里找到一个特定的物体，因此常识推理有助于知道这样一个物体通常在哪里被发现。衡量成功的标准是达到目标的试验的百分比。

WebShop 是一个模拟在线购物网站，数据来自亚马逊。这是一个具有挑战性的领域，因为它有大量的行动导航网站和搜索产品。目标是找到一个符合用户规范的项目。衡量成功的标准是所选项目与用户心目中的隐藏项目有多接近。

从论文中的评估结果来看，ReAct 的表现明显优于基线。

ReAct 虽然由于其推理错误而并不完善，但仍然是一个强有力的提示工程方法，它克服了思维链推理的事实幻觉问题，并且允许 LLM 成为一个可以与其环境相互作用的Agent。此外，它具有可解释性，因为Agent在行动时输出其思维过程。

![](img/06f32b35c54d771fc71d41ce2b02edfe.png)

## 小结

作为Agent 应用于提示工程的一种具体实现，ReAct 迈出通往通用人工智能和具象语言模型的一步。如果一个机器人有一种基于熟悉的特征对外部环境进行建模并使用该模型创建提示的方法，那么它至少可以尝试在各种领域独立行动，而不需要人工制作的示例。它还需要某种记忆，或者从它的经历中学习的能力，具有常识性知识的机器人，只要能够解决诸如推理错误和幻觉之类的问题，就可能对我们有很大的帮助。

BTW，拥有了大模型就可以拥有好的产品么？ 产品经理的贡献往往仍然是不可或缺的，以智能语音产品为例， 你可能需要这样的一本案头手册（笔者的最新译作），帮助自己更好的完成产品设计。

【参考资料】

+ Yao etc, “ReAct: Synergizing Reasoning and Acting in Language Models”,ICLR,2023

+ [如何构建基于大模型的App](http://mp.weixin.qq.com/s?__biz=MzAwOTcyNzA0OQ==&mid=2658978464&idx=1&sn=8b9c617bced2b5ce012fb1f4aa87c086&chksm=80d35786b7a4de901400ffe4db5061c08df2912b4b49466e91a9f52813f22f3137e41802abc9&scene=21#wechat_redirect)

+ Qcon2023: 大模型时代的技术人成长（简）

+ 论文学习笔记：增强学习应用于OS调度

+ 《深入浅出Embedding》随笔

+ [LLM的工程实践思考](http://mp.weixin.qq.com/s?__biz=MzAwOTcyNzA0OQ==&mid=2658978144&idx=1&sn=e8bff055687f7e7ad17baca58dc26c23&chksm=80d35446b7a4dd507b02832851e2308a276edc328f3e79bca0f3ae931ab27db69aeccd8d7c8d&scene=21#wechat_redirect)

+ [解读大模型的微调](http://mp.weixin.qq.com/s?__biz=MzAwOTcyNzA0OQ==&mid=2658978125&idx=1&sn=b342e9c10734c1f7cd124ebc88aa75a8&chksm=80d3546bb7a4dd7d9c74dabd6046d1ccea01592760a8add3d71b2f75fa68712093a4834e6d88&scene=21#wechat_redirect)

+ [解读ChatGPT中的RLHF](http://mp.weixin.qq.com/s?__biz=MzAwOTcyNzA0OQ==&mid=2658977979&idx=1&sn=4d66d662595d9aba355e0564df545370&chksm=80d3559db7a4dc8b96a5f3ac4d01b53a2601d5e1dcb51789ee3d9974ec9ae5f028c3e469b5a5&scene=21#wechat_redirect)

+ 解读大模型（LLM）的token

+ 解读提示工程（Prompt Engineering）

+ [解读Toolformer](http://mp.weixin.qq.com/s?__biz=MzAwOTcyNzA0OQ==&mid=2658977935&idx=1&sn=9dd162f270f7c17a673291c0f136cf8c&chksm=80d355a9b7a4dcbf3ab824ac6de5c27d94a11455a681ebeb98a211e9b58ff867ddddf33d1c8d&scene=21#wechat_redirect)

+ [解读TaskMatrix.AI](http://mp.weixin.qq.com/s?__biz=MzAwOTcyNzA0OQ==&mid=2658977922&idx=1&sn=2f67513b540485d0177ded24c0807bcc&chksm=80d355a4b7a4dcb2495ab97febd0ac88cd0780e0d8e4b3bbc369250304289e289f71b0a76f0c&scene=21#wechat_redirect)

+ [解读LangChain](http://mp.weixin.qq.com/s?__biz=MzAwOTcyNzA0OQ==&mid=2658978479&idx=1&sn=47d6d226a3ac6cc08980e7562ae09fce&chksm=80d35789b7a4de9f3ba9a3eeb21c6c812b0f7fe43cee47fc38368ed31a1346114a67daa3cd30&scene=21#wechat_redirect)

+ [浅析多模态机器学习](http://mp.weixin.qq.com/s?__biz=MzAwOTcyNzA0OQ==&mid=2658978122&idx=1&sn=baf54b7a1a0758e19bf6a3622a43d8fb&chksm=80d3546cb7a4dd7ab9b857ea75bcc3e361cb68489ff5c43ff4d8ab65b2da838b46621b2936b1&scene=21#wechat_redirect)

+ [Agent 与对象的辨析](http://mp.weixin.qq.com/s?__biz=MzAwOTcyNzA0OQ==&mid=2658977521&idx=1&sn=378ae6e0f2c53d31993515f62e295327&chksm=80d35bd7b7a4d2c174a99a8dc2910c193cc846d145e4351e768131f0a977410d3c9d0e668baa&scene=21#wechat_redirect)

+ [深度学习架构的对比分析](http://mp.weixin.qq.com/s?__biz=MzAwOTcyNzA0OQ==&mid=2658978063&idx=1&sn=f934c531bacfe1bf6c95ccbdababe439&chksm=80d35429b7a4dd3f8df5f8c439bd98420c13d4a115c609a60370a6e09ad7c667c4a8c95418c1&scene=21#wechat_redirect)

+ [老码农眼中的大模型（LLM）](http://mp.weixin.qq.com/s?__biz=MzAwOTcyNzA0OQ==&mid=2658977916&idx=1&sn=d38f03af2b3916251e9a8577d995e065&chksm=80d3555ab7a4dc4c0c919ce0a678adc9f3018d81994dc072e698cfa481665ec8faf788d606be&scene=21#wechat_redirect)

+ [系统学习大模型的20篇论文](http://mp.weixin.qq.com/s?__biz=MzAwOTcyNzA0OQ==&mid=2658978109&idx=1&sn=4d15334a5cccc300a18686b1dd2a2098&chksm=80d3541bb7a4dd0df321e516ad40c8e849dde084328b61259f4b17f5d24280686305a87fa8b5&scene=21#wechat_redirect)
