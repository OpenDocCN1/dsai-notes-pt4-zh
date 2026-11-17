
# 关于Prompt Engineering你该了解啥？OpenAI应用研究负责人帮你梳理了

> 来源：[https://agijuejin.feishu.cn/docx/S7OUdvhhho941rxX7XfcLqPZnwh](https://agijuejin.feishu.cn/docx/S7OUdvhhho941rxX7XfcLqPZnwh)

> 机器之心｜[阅读原文](https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650874849&idx=3&sn=c4d0a9c8a5bbdb384027d50d9639fa9e&chksm=84e4e79fb3936e897e15d3e6aaff16f22e5752c640d8fbc4d52041d9d38fc1a35f94411f00f0&scene=21#wechat_redirect)

> 转载请联系原作者取得授权

> 选自 Lil’Log｜机器之心编译｜编辑：rome rome

随着 ChatGPT、GPT-4 等模型的兴起，人们对如何创建提示以获得想要的输出越来越感兴趣。研究者对特定提示的响应可能很难预测，并且会因模型的不同而不同。本文来自 OpenAI 的翁丽莲（ Lilian Weng ） 撰文介绍了关于提示的一些内容，包括基础提示、指令提示等内容。

Prompt 工程，也称为 In-Context Prompt，是指在不更新模型权重的情况下如何与 LLM 交流以引导其行为来获得所需结果的方法。这是一门经验科学，Prompt 工程方法的效果在模型之间可能会有很大差异，因此需要大量的实验和启发式方法。

本文来自 OpenAI 的 Lilian Weng (翁丽莲) 介绍了一些关于提示工程的知识。翁丽莲为 OpenAI 人工智能应用研究的负责人，2018 年加入 OpenAI，在 GPT-4 项目中主要参与预训练、强化学习 & 对齐、模型安全等方面的工作。

![](img/68198304d72c991e7d62f59d69f3984f.png)

主要内容包含以下章节，下面我们看看本文主要内容。

![](img/8e7f5147f8c351012203afd7746e0085.png)

## 基础 Prompt

Zero-shot 和 few-shot 学习是 prompt 模型的两种最基本的方法，许多关于 LLM 论文都有涉及，并常用于评估 LLM 的性能。

### Zero-Shot 学习

Zero-Shot 学习是简单地将任务文本输入模型并要求其返回结果。（所有情感分析示例均来自 SST-2）

```
Text: i&#x27;ll bet the video game is a lot more fun than the film.
Sentiment:
```

### Few-shot 学习

Few-shot 学习提供了一组关于目标任务的高质量的演示，每个演示都包含目标任务的输入和期望的输出。当模型首先看到好的例子时，它可以更好地理解人类的意图和对所需答案的标准。因此，与 zero-shot 学习相比，few-shot 学习通常会导致更好的性能。然而，这样的代价是消耗更多的 token，并且当输入和输出文本很长时，可能会达到上下文长度限制。

```
Text: (lawrence bounces) all over the stage, dancing, running, sweating, mopping his face and generally displaying the wacky talent that brought him fame in the first place.
Sentiment: positive

Text: despite all evidence to the contrary, this clunker has somehow managed to pose as an actual feature movie, the kind that charges full admission and gets hyped on tv and purports to amuse small children and ostensible adults.
Sentiment: negative

Text: for the first time in years, de niro digs deep emotionally, perhaps because he&#x27;s been stirred by the powerful work of his co-stars.
Sentiment: positive

Text: i&#x27;ll bet the video game is a lot more fun than the film.
Sentiment:
```

许多研究探讨了如何构建上下文示例以最大限度地提高性能，并观察到 prompt 格式、训练示例和示例顺序的选择可能导致截然不同的性能，从接近随机猜测到接近 SOTA。

Zhao 等人的研究调查了 few-shot 分类情况，并提出几个导致高方差的原因（他们在实验中使用 GPT-3）：（1）如果标签在示例之间的分布不平衡，会导致 Majority label 偏差；（2） Receny 偏差是指模型可能在最后重复标签的趋势；（3） Common token 偏差表明 LLM 倾向于生成常见的 token 而不是罕见的 token。为了克服这类偏差，他们提出了一种方法来校准模型输出的标签概率，使得当输入字符串为 `N/A` 时，标签的概率输出变为均匀分布。

### 样本选择小技巧

![](img/cd04b6badd10ccc17e305c758ed22ab7.png)

#### 关于样本排序的小技巧

+ 建议保持样本选择的多样性、与测试样本相关并以随机顺序排列，以避免 Majority Label bias 和 Recency bias；
+ 增加模型大小或包含更多训练样本并不能减少不同上下文样本排列的方差。同一顺序可能适用于一个模型，但对另一个模型效果不佳。当验证集有限时，考虑选择顺序，使得模型不会产生极不平衡的预测或对其预测过于自信 (Lu et al. 2022)。

## 指令 prompt

在 prompt 中使用 few-shot 示例的目的是向模型解释我们的意图，换句话说，以演示的形式向模型描述任务指令。然而，few-shot 样本在标记使用方面可能很昂贵，并且由于上下文长度有限，它会限制输入长度。那么，为什么不直接给出指令呢？

Instructed LM 模型（例如 InstructGPT、自然指令）通过高质量的元组（任务指令，输入，正确输出）来微调预训练模型，以使 LM 更好地理解用户意图并遵循指令。RLHF（来自人类反馈的强化学习）是一种常用的方法。指令跟随式微调的好处在于，它使模型更加符合人类意图，大大降低通信成本。

与指令模型交互时，应该详细描述任务要求，尽量具体和精确，避免说 “不要做某事”，而是具体说明要做什么。

```
Please label the sentiment towards the movie of the given movie review. The sentiment label should be &quot;positive&quot; or &quot;negative&quot;. 
Text: i&#x27;ll bet the video game is a lot more fun than the film. 
Sentiment:
```

向指定群体解释是另一种聪明的指示方式，例如为孩子们制作教育资料

```
Describe what is quantum physics to a 6-year-old.
```

以及安全的内容

```
... in language that is safe for work.
```

情境指令学习 [Ye et al. 2023] 将 few-shot 学习与指令 prompt 相结合。它在 prompt 中包含多个跨不同任务的演示示例，每个演示都由指令、任务输入和输出组成。请注意，他们的实验仅针对分类任务，指令 prompt 包含所有标签选项。

```
Definition: Determine the speaker of the dialogue, &quot;agent&quot; or &quot;customer&quot;.
Input: I have successfully booked your tickets.
Ouput: agent


Definition: Determine which category the question asks for, &quot;Quantity&quot; or &quot;Location&quot;.
Input: What&#x7s the oldest building in US?
Ouput: Location


Definition: Classify the sentiment of the given movie review, &quot;positive&quot; or &quot;negative&quot;.
Input: i&#x27;ll bet the video game is a lot more fun than the film.
Output:
```

## 自洽采样

自洽采样 [Wang et al. 2022a] 是对 `temperature > 0` 的多个输出进行采样，然后从这些候选中选择最好的一个。选择最佳候选的标准因任务而异。一般的解决方案是选择多数票。对于易于验证的任务，例如带有单元测试的编程问题，可以简单地运行解释器并通过单元测试验证正确性。

## 思维链 (CoT)

思维链 (CoT) prompt (Wei et al. 2022) 生成一系列短句来逐步描述推理逻辑，称为推理链，最终得出最终答案。CoT 的好处对于复杂的推理任务更为明显，同时使用大型模型（例如，参数超过 50B）。简单的任务从 CoT 的 prompt 中获益甚微。

### CoT prompt 的类型

CoT prompt 的两种主要类型：

few-shot CoT：通过一些演示来给模型 prompt，每个演示都包含人类编写（或模型生成）的高质量推理链。

（所有数学推理例子均来自 GSM8k）

```
Question: Tom and Elizabeth have a competition to climb a hill. Elizabeth takes 30 minutes to climb the hill. Tom takes four times as long as Elizabeth does to climb the hill. How many hours does it take Tom to climb up the hill?
Answer: It takes Tom 30*4 = &lt;&lt;30*4=120&gt;&gt;120 minutes to climb the hill.
It takes Tom 120/60 = &lt;&lt;120/60=2&gt;&gt;2 hours to climb the hill.
So the answer is 2.
===
Question: Jack is a soccer player. He needs to buy two pairs of socks and a pair of soccer shoes. Each pair of socks cost $9.50, and the shoes cost $92. Jack has $40. How much more money does Jack need?
Answer: The total cost of two pairs of socks is $9.50 x 2 = $&lt;&lt;9.5*2=19&gt;&gt;19.
The total cost of the socks and the shoes is $19 + $92 = $&lt;&lt;19+92=111&gt;&gt;111.
Jack need $111 - $40 = $&lt;&lt;111-40=71&gt;&gt;71 more.
So the answer is 71.
===
Question: Marty has 100 centimeters of ribbon that he must cut into 4 equal parts. Each of the cut parts must be divided into 5 equal parts. How long will each final cut be?
Answer:
```

Zero-shot CoT：使用像「让我们一步一步来思考」这样的自然语言语句来明确鼓励模型首先生成推理链，然后 prompt「因此答案是」来生成答案 (Kojima et al. 2022)。或者类似的陈述「让我们一步一步地解决这个问题，以确保我们有正确的答案」（Zhou et al. 2022）。

```
Question: Marty has 100 centimeters of ribbon that he must cut into 4 equal parts. Each of the cut parts must be divided into 5 equal parts. How long will each final cut be?
Answer: Let&#x27;s think step by step.
```

## 小技巧和扩展

+ 自洽采样可以通过抽取多个不同的答案然后进行多数表决来提高推理的准确性。(Wang et al. 2022a)；
+ 集成学习的另一种方法是改变样本顺序或使用模型生成的基本原理来代替人工编写的基本原理，以在多个样本试验中引入随机性。然后聚合模型根据多数投票的结果获得最终答案。(Wang et al. 2022b)；
+ 如果训练样例只与真实答案相关联，但没有基本推理，可以遵循 `STaR`（Self-Taught Reasoner；Zelikman et al. 2022）方法：（1）让 LLM 生成推理链，只保留那些导致正确答案的链；(2) 然后用生成的基本推理微调模型并重复该过程直到收敛。需要注意的是，较高的 `temperature` 更有可能产生错误的基本原理和正确的答案。如果训练样本没有真实答案，可以考虑使用多数票作为 “正确” 答案；
+ Prompt 具有更高推理复杂性的演示可以实现更好的性能，其中复杂性由链中推理步骤的数量来衡量。分隔推理步骤时，换行符 `\n` 比 “步骤 i”、句号或分号等要好 (Fu et al. 2023)；
+ Complexity-based 的一致性是通过仅在前 `k` 个复杂链中进行多数投票表决，选出 `k` 个中明确表现较好的复杂链 (Fu et al. 2023)；
+ [Shum et al. (2023) ] 发现在他们的实验中，仅使用复杂示例的 CoT 提示可以提高复杂问题的准确性，但在简单问题中表现不佳 (GSM8k 上的表现就是明显的证据）；
+ 将 `Q:` 更改为 `Question:` 发现很有帮助 (Fu et al. 2023)；
+ [Ye & Durrett (2022) ] 发现，对于涉及文本推理的 NLP 任务（即 QA 和 NLI），在 prompt 中包含解释的好处有用处但是不大，并且效果因模型而异。他们观察到解释更可能是非事实的而不是不一致的（即解释是否需要预测）。非事实的解释很可能导致错误的预测；
+ [`Self-Ask` (Press et al. 2022) ] 是一种反复 prompt 模型提出后续问题以迭代构建思维过程的方法。可以通过搜索引擎结果回答后续问题。同样，`IRCoT` [Interleaving Retrieval CoT；Trivedi et al. 2022] 和 `ReAct` [Reason + Act；Yao et al. 2023] 将迭代 CoT 提示与维基百科 API 查询相结合，以搜索相关实体和内容，然后将其添加回语境中。

![](img/4690c543874f33c434fcb7396634a784.png)

图 1. Self-Ask 如何与外部搜索查询一起使用（图源：Press et al. 2022）。

## 自动 Prompt 设计

Prompt 是一系列前缀 token，可增加在给定输入的情况下获得所需输出的概率。因此，可以将它们视为可训练参数，并通过梯度下降直接在嵌入空间上对其进行优化，例如 `AutoPrompt` [Shin et al., 2020, `Prefix-Tuning` (Li & Liang (2021)]，`P-tuning` [Liu et al. . 2021)] 和 `Prompt-Tuning` [Lester et al. 2021]。从 `AutoPrompt` 到 `Prompt-Tuning` 的趋势是设置逐渐简化。

`APE` [Automatic Prompt Engineer；Zhou et al. 2022] 是一种搜索模型生成的候选指令池，然后根据所选得分函数过滤候选集以最终选择得分最高的最佳候选的方法。

1. `Prompt LLM` 根据 `input-output` 对形式的演示集合生成候选指令。例如：`{{Given desired input-output pairs}}\n\nThe instruction is`；
2. 给定 `Dtrain={(x,y)}` 的数据集，想要找到一条指令 `ρ` 使得 `ρ∗=arg max ρ E (x,y)∈Dtrain [f (ρ,x,y)]`，其中 `f (.)` 是每个样本的得分函数，比如执行精度 `LM (.|ρ, x)=y` 或对数概率：`Plm (y|ρ,x)`；
3. 使用迭代蒙特卡洛搜索方法通过 prompt 提出语义相似的变体来改进最佳候选，例如 `Generate a variation of the following instruction while keeping the semantic meaning.\n\nInput: ...\n\nOutput:...`；



为了自动构建 CoT prompt [ Shum et al. (2023) ] 建议 augment-prune-select，包含三个步骤的过程：

1. Augment：使用 few-shot 或 zero-shot CoT prompt 生成给定问题的多个伪 CoT；
2. Prune：根据生成的答案是否与基本事实相匹配来修剪伪链；
3. Select：应用 variance-reduced 梯度策略来学习所选示例的概率分布，同时将示例的概率分布作为策略，将验证集的准确性作为奖励。

[Zhang et al. (2023) ] 采用聚类技术对问题进行抽样，然后生成链。他们观察到 LLM 倾向于犯某些类型的错误。一种类型的错误在嵌入空间中可能相似，因此被组合在一起。通过只从频繁错误的集群中抽取一个或几个样本，可以防止对一种错误类型的过多错误演示，并收集一组不同的例子。

1. 问题聚类：嵌入问题并运行 k-means 聚类的方法；
2. 示例选择：从每个簇中选择一组有代表性的问题；即来自一个簇的一个示例。每个簇中的样本按到簇质心的距离排序，最接近质心的样本首先被选择；
3. 生成推理链：使用 zero-shot CoT 为选定的问题生成推理链，并构建 few-shot prompt 来运行推理。

## 增强语言模型

[Mialon et al. (2023) ] 对增强语言模型进行的一项调查。涵盖了多类增强推理技能和使用外部工具能力的语言模型。推荐读者可以读一下。

## 检索

通常，我们需要在模型预训练时间截止或内部 / 私有知识库之后，完成需要最新知识的任务。在这种情况下，如果不在 prompt 中显式提供上下文，模型将不知道上下文。开放领域问答的许多方法都依赖于首先在知识库上进行检索，然后将检索到的内容作为 prompt 的一部分。这个过程的准确性取决于检索和生成步骤的质量。

[Lazaridou et al. (2022) ] 研究了如何使用谷歌搜索进行文档检索以增强 LLM。给定一个问题 `q`，从谷歌返回的 20 个 URL 中提取文本，得到一组文档。由于这些文档很长，因此每个文档被拆分为 6 个句子的段落，`{p}`。段落是根据证据段落和查询之间基于 TF-IDF 的余弦相似性进行排序的。在 prompt 中只使用最相关的段落来产生答案 `a`。

对于闭卷问答，每个演示的格式如下，以构建 few-shot prompt。发现将问题与证据交换（问题和答案之间的距离更长）在所有数据集中始终产生较低结果。

```
Evidence: ...
Question: ...
Answer: ...
```

答案的概率以三种方式计算：

![](img/4cf4711d3a624c6ced153a9012098eee.png)

根据他们在生成和分类任务的实验，在三个答案的重新排序得分中，PoE > 噪声信道推断 > RAG。在所有概率中，`pLM (a|q,pi)` 和 `pLM (q|pi,a)` 提供的信息最多。`pLM (q|pi,a)` 捕捉到 LM 在给定证据段落和答案的情况下对问题的解释有多好，并且可以可靠地用于对候选答案进行重新排序。

针对基于不同日期的问题的 SituatedQA 数据集进行的一项观察是，尽管 LM（预训练截止日期为 2020 年）可以通过 Google 搜索访问最新信息，但其在 2020 年后问题上的表现仍然比 2020 年前问题差很多。这表明上下文信息和模型内部知识之间存在一些差异或参数冲突。

有趣的是，即使只有 “内部检索” 也是有益的，即在回答问题之前生成关于某个主题的知识 [Liu et al. 2022]。首先可以使用下面的模板来抽取知识：

```
Generate some knowledge about the input. Examples:

Input: What type of water formation is formed by clouds?
Knowledge: Clouds are made of water vapor.

Input: {question}
Knowledge:
```

然后用模型生成的知识，进一步给 LM 进行 prompt 得到答案。

## 编程语言

既有 PAL（程序辅助语言模型) [ Gao et al. 2022] 和 PoT (Program of Thoughts prompting [Chen et al. 2022] 要求 LLM 生成编程语言语句来解决自然语言推理问题，从而将解决方案步骤转移到运行时，如 Python 解释器。这样的设置解耦了复杂的计算和推理。该方法依赖于具有足够好的编程技能的 LM。

![](img/b5e53773255aebf9c306b0c7af568d40.png)

图 2. 比较 CoT 和 PoT.(图片源自: Chen et al. 2022)。

## 外部 API

TALM（Tool Augmented Language Models [ Parisi et al. 2022]）是一种通过文本到文本 API 调用增强的语言模型。LM 被引导生成以任务输入文本为条件的工具调用和工具输入文本，以构建 API 调用请求。当 `result` 出现时，调用指定的工具 API，并将返回的结果附加到文本序列。最终输出是在 output token 之后生成的。

![](img/172d2a1fd6cdf42b9acba439b8606fac.png)

图 3. TALM 中调用 API 的格式 (图片源自: Parisi et al. 2022)。

TALM 采用 self-play 的方法来迭代引导工具使用示例的数据集，并用它来微调 LM。这个迭代的 self-play 模仿了一个 RL 过程，其中 LM 是策略网络，它由带有二元奖励信号的策略梯度训练。

![](img/472c1e793009c5199ec6ece9ab386b8c.png)

图 4. Self-play 迭代提升模型的性能 (图片源自: Parisi et al. 2022)。

Toolformer [Schick et al. 2023] 是一种可以通过简单的 API 使用外部工具的 LM，通过自我监督的方式构建，每个 API 只需要少量的演示。Toolformer 的工具箱包括：

+ 计算系统：帮助 LM 缺乏精确的数学技能；
+ 问答系统：帮助解决无效内容；
+ 搜索引擎：在预训练截止时间后提供最新信息；
+ 翻译系统：提高低资源语言性能；
+ 日历系统：使 LM 了解时间进程。

![](img/e3c5e7a28f8477eb9a19b47edc437d01.png)

图 5. 如何构建 Toolformer.(图片源自: Schick et al. 2023)。

Toolformer 的训练过程如下：

1. Prompt 注释下的 API 调用。要求预训练的 LM 通过带有 API 调用使用示例的 few-shot 学习来注释数据集。格式化示例：

![](img/1eed175c94548e7c17bbebf9c22053e1.png)

图 6. 数据集如何在调用 API 的时候注释 (图片源自: Schick et al. 2023)。

![](img/3ba8f5b95930b3110aac1141f31ee8d3.png)

![](img/13b7bfdac67c6c7e282999c387620097.png)

3. 在注释数据集上微调 LM。新的训练序列构造为

![](img/f75cc182f2527b1064e4a6c0ff491a87.png)

训练数据是原始数据集（例如论文中的 CCNet 子集）及其增强版。

在推理时，解码会一直运行，直到模型产生 `→` token，表明它期待接下来来自 API 调用的响应。

Toolformer 目前不支持在链中使用工具（即使用一个工具的输出作为另一个工具的输入）或以交互方式（即在人工选择后采用 API 响应）。两者都是未来扩展模型的方向。

原文链接：https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/
