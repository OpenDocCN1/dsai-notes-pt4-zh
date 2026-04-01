# LLM（十三）：Prompt Engineering 面面观

> 来源：[https://agijuejin.feishu.cn/docx/WL68d64Vro8j40xMoFjciC2dnTZ](https://agijuejin.feishu.cn/docx/WL68d64Vro8j40xMoFjciC2dnTZ)

> 作者：紫气东来

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/632369186)hihu.com/p/632369...

## 一、概述

提示工程（Prompt Engineering），也称为 In-Context Prompting，是指在不更新模型权重的情况下如何与 LLM 交互以引导其行为以获得所需结果的方法。 在提示工程中，任务的描述会被嵌入到输入中。例如，不是隐含地给予模型一定的参数，而是以问题的形式直接输入。 提示工程的典型工作方式是将一个或多个任务转换为基于提示的数据集，并通过所谓的“基于提示的学习（prompt-based learning）”来训练 语言模型 。

提示工程不仅仅是关于设计和研发提示词。它包含了与大语言模型交互和研发的各种技能和技术。提示工程在实现和大语言模型交互、对接，以及理解大语言模型能力方面都起着重要作用。用户可以通过提示工程来提高大语言模型的安全性，也可以赋能大语言模型，比如借助专业领域知识和外部工具来增强大语言模型能力。

提示词可以包含以下任意要素：

+   **指令 **：想要模型执行的特定任务或指令。

+   **上下文 **：包含外部信息或额外的上下文信息，引导语言模型更好地响应。

+   **输入数据 **：用户输入的内容或问题。

+   **输出指示 **：指定输出的类型或格式。

以下是设计提示的通用技巧:

+   **从简单开始： **在设计提示时，需要记住这是一个迭代的过程，需要大量的实验来获得最佳结果。可以从简单的提示开始，不断添加更多的元素和上下文，以获得更好的结果。

+   **指令 **：可以使用命令来指示模型执行各种简单任务，例如“写入”、“分类”、“总结”、“翻译”、“排序”等，从而为各种简单任务设计有效的提示。

+   **具体性 **：对希望模型执行的指令和任务，提示越具体和详细，结果就越好。实际上，在提示中提供示例非常有效，可以以特定格式获得所需的输出。

+   **避免不精确 **：这里的类比非常类似于有效的沟通——越直接，信息传递就越有效。

+   **做还是不做 **：设计提示时的另一个常见技巧是避免说不要做什么，而是说要做什么。

## 二、提示技术

时至今日，改进提示显然有助于在不同任务上获得更好的结果。这就是提示工程背后的整个理念。在本节中，我们将介绍更高级的提示工程技术，使我们能够完成更复杂和有趣的任务，所有测试案例均通过 text-davinci-003 得到。

### 2.1 Zero-shot 与 Few-shot

Zero-shot 与 Few-shot 是最基础的提示技术。经过大量数据训练并调整指令的LLM能够执行 Zero-shot 任务，即直接向模型输入文本以获取回答。

如，Zero-shot 输入：

```
Text: I&#x27;ll bet the video game is a lot more fun than the film.
Sentiment:
```

输出：

```
Positive - The speaker expresses that they think the video game is more enjoyable than the film.
```

Few-shot learning 在目标任务上提供了一组高质量的演示，每个演示都包含输入和期望的输出。 当模型首先看到好的例子时，它可以更好地理解人类的意图和需要什么样的答案的标准。 因此，少样本学习通常比零样本学习有更好的性能。 然而，它是以更多的 token 消耗为代价的，并且当输入和输出文本很长时可能会达到上下文长度限制。

如，Few-shot 输入：

```
Text: (lawrence bounces) all over the stage, dancing, running, sweating, mopping his face and generally displaying the wacky talent that brought him fame in the first place.
Sentiment: positive
Text: despite all evidence to the contrary, this clunker has somehow managed to pose as an actual feature movie, the kind that charges full admission and gets hyped on tv and purports to amuse small children and ostensible adults.
Sentiment: negative
Text: for the first time in years, de niro digs deep emotionally, perhaps because he&#x27;s been stirred by the powerful work of his co-stars.
Sentiment: positive
Text: I&#x27;ll bet the video game is a lot more fun than the film.
Sentiment: 
```

输出：

```
negative
```

### 2.2 思维链（Chain-of-Thought, CoT）提示

CoT 提示 生成一系列短句来逐步描述推理逻辑，称为推理链或基本原理，最终得出最终答案。 CoT 的好处对于复杂的推理任务更为，同时使用大型模型（例如，参数超过 50B）时效果更加明显。

![](img/858000a242912ed3419f853b614075dd.png)

如，Few-shot CoT 输入：

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

输出：

```
Marty has 100 cm of ribbon that needs to be cut into 4 equal parts.
Each of the four cuts will be 100/4 = &lt;&lt;100/4=25&gt;&gt;25 cm long.
Marty must then divide each of the four cuts into 5 equal parts. 
Each of the five final cuts will be 25/5 = &lt;&lt;25/5=5&gt;&gt;5 cm long.
So the answer is 5 cm.
```

如，Zero-shot CoT 输入：

```
Question: Marty has 100 centimeters of ribbon that he must cut into 4 equal parts. Each of the cut parts must be divided into 5 equal parts. How long will each final cut be?
Answer: Let&#x27;s think step by step.
```

输出：

```
Step 1: Marty has to cut 100 centimeters of ribbon into 4 equal parts. So, each of the parts will be 25 centimeters long.
Step 2: Marty has to divide each of the 4 parts into 5 equal parts. So, each of the final cut parts will be 5 centimeters long.
```

### 2.3 指令提示（Instruction Prompting）

Instructed LM（例如 InstructGPT）使用高质量数据微调预训练模型，使 LM 更好地理解用户意图并遵循指令。 RLHF 是一种常用的方法。 instruction style fine-tuning 的好处是改进了模型，使其更符合人类的意图，并大大降低了沟通成本。

在与指令模型交互时，我们应该详细描述任务要求，尽量具体和准确，避免说“不做某事”，而是具体说明要做什么。如针对特定人群的输入：

```
Describe what is quantum physics to a 6-year-old.
```

输出：

```
Quantum physics is like a super special game. It&#x27;s all about understanding the very tiny things that make up our world. It&#x27;s like trying to find out how puzzle pieces fit together to make a big picture.
```

In-context instruction learning 将小样本学习与指令提示相结合。 它在提示中包含多个跨不同任务的演示示例，每个演示都由指令、任务输入和输出组成。 请注意，他们的实验仅针对分类任务，指令提示包含所有标签选项。

如，In-context instruction learning 输入：

```
Definition: Determine the speaker of the dialogue, &quot;agent&quot; or &quot;customer&quot;.
Input: I have successfully booked your tickets.
Ouput: agent
Definition: Determine which category the question asks for, &quot;Quantity&quot; or &quot;Location&quot;.
Input: What&#x27;s the oldest building in US?
Ouput: Location
Definition: Classify the sentiment of the given movie review, &quot;positive&quot; or &quot;negative&quot;.
Input: i&#x27;ll bet the video game is a lot more fun than the film.
Output:
```

输出：

```
Positive
```

### 2.4 自我一致性（Self-Consistency）采样

自我一致性旨在“替换链式思维提示中使用的天真贪婪解码方法”。其想法是通过少样本CoT采样多个不同的推理路径，并使用生成结果选择最一致的答案。这有助于提高CoT提示在涉及算术和常识推理的任务中的性能。

尝试进行以下数学推理问题：

```
When I was 6 my sister was half my age. Now I’m 70 how old is my sister?
```

结果如下：

```
Your sister is now 35 years old.
```

结果错误，下面使用自我一致性来尝试该问题，输入如下：

```
Q: There are 15 trees in the grove. Grove workers will plant trees in the grove today. After they are done,there will be 21 trees. How many trees did the grove workers plant today?
A: We start with 15 trees. Later we have 21 trees. The difference must be the number of trees they planted.So, they must have planted 21 - 15 = 6 trees. The answer is 6.
Q: If there are 3 cars in the parking lot and 2 more cars arrive, how many cars are in the parking lot?
A: There are 3 cars in the parking lot already. 2 more arrive. Now there are 3 + 2 = 5 cars. The answer is 5.
Q: Leah had 32 chocolates and her sister had 42. If they ate 35, how many pieces do they have left in total?
A: Leah had 32 chocolates and Leah’s sister had 42. That means there were originally 32 + 42 = 74chocolates. 35 have been eaten. So in total they still have 74 - 35 = 39 chocolates. The answer is 39.
Q: Jason had 20 lollipops. He gave Denny some lollipops. Now Jason has 12 lollipops. How many lollipopsdid Jason give to Denny?
A: Jason had 20 lollipops. Since he only has 12 now, he must have given the rest to Denny. The number oflollipops he has given to Denny must have been 20 - 12 = 8 lollipops. The answer is 8.
Q: Shawn has five toys. For Christmas, he got two toys each from his mom and dad. How many toys doeshe have now?
A: He has 5 toys. He got 2 from mom, so after that he has 5 + 2 = 7 toys. Then he got 2 more from dad, soin total he has 7 + 2 = 9 toys. The answer is 9.
Q: There were nine computers in the server room. Five more computers were installed each day, frommonday to thursday. How many computers are now in the server room?
A: There are 4 days from monday to thursday. 5 computers were added each day. That means in total 4 * 5 =20 computers were added. There were 9 computers in the beginning, so now there are 9 + 20 = 29 computers.The answer is 29.
Q: Michael had 58 golf balls. On tuesday, he lost 23 golf balls. On wednesday, he lost 2 more. How manygolf balls did he have at the end of wednesday?
A: Michael initially had 58 balls. He lost 23 on Tuesday, so after that he has 58 - 23 = 35 balls. OnWednesday he lost 2 more so now he has 35 - 2 = 33 balls. The answer is 33.
Q: Olivia has $23. She bought five bagels for $3 each. How much money does she have left?
A: She bought 5 bagels for $3 each. This means she spent 5
Q: When I was 6 my sister was half my age. Now I’m 70 how old is my sister?
A:
```

输出1：

```
When you were 6, your sister was 6/2 = 3 years old. Now you are 70 years old. That means your sister is 70- 3 = 67 years old. The answer is 67.
```

输出2:

```
 When you were 6, your sister was 6 / 2 = 3 years old. Now that you are 70 years old, your sister mustbe 70 / 2 = 35 years old. The answer is 35.
```

输出3:

```
 When I was 6, my sister was 3 (half my age). 70 years have passed since then. That means my sister is now 3 + 70 = 73 years old. The answer is 73.
```

### 2.5 自动提示工程师（Automat**ic P**rompt Engineer, APE ）

APE 是一种搜索模型生成的候选指令池，然后根据所选得分函数过滤候选集以最终选择得分最高的最佳候选指令的方法。其过程可概括为3个阶段：

1.  提示 LLM 根据输入输出对形式的一小组演示生成候选指令。如： {{Given desired input-output pairs}}\n\nThe instruction is ；

1.  对于给定数据集  \mathcal{D}_{train}{\{(x,y)\}} 目标是找到一个指令  \rho 使得  \rho^*=\arg \max _\rho \mathbb{E}_{(x, y) \in \mathcal{D}_{\text {train }}}[f(\rho, x, y)] ，其中  f(\cdot) 表示打分函数；

1.  使用迭代蒙特卡洛搜索方法通过提示（如： Generate a variation of the following instruction while keeping the semantic meaning.\n\nInput: ...\n\nOutput:... ）来提出语义相似的变体来改进最佳候选者。

为了构造自动户 CoT 提示， Shum et al. (2023) 建议进行剪枝选择，包括以下3步：

1.  增强(Augment)：使用 Few-shot 或 Zero-shot CoT 提示生成给定问题的多个伪思维链；

1.  修剪(Prune)：根据生成的答案是否与基本事实相匹配来修剪伪链。

1.  选择(Select)：应用减少方差的策略梯度策略来学习所选示例的概率分布，同时将示例的概率分布视为策略，将验证集的准确性视为奖励。

Zhang et al. (2023) 认为采用聚类技术对问题进行抽样，然后生成链。 他们观察到 LLM 倾向于犯某些类型的错误。 一种类型的错误在嵌入空间中可能相似，因此被组合在一起。 通过仅从频繁错误的集群中抽取一个或几个样本，我们可以防止对一种错误类型的过多错误演示，并收集一组不同的示例。

1.  问题聚类(Question clustering)：Embed 问题使用 k-means 的方法进行聚类。

1.  示例选择(Demonstration selection)：从每个集群中选择一组有代表性的问题； 即来自一个集群的一个示例。 每个簇中的样本按到簇质心的距离排序，最接近质心的样本首先被选择。

1.  论据生成(Rationale generation)：使用 Zero-shot CoT 为选定的问题生成推理链，并构建 Few-shot 提示以运行推理。

## 三、更多资料

### 3.1 实用工具

+   OpenAI Cookbook has many in-depth examples for how to utilize LLM efficiently.

+   [LangChain ](https://link.zhihu.com/?target=https%3A//langchain.readthedocs.io/en/latest/), a library for combining language models with other components to build applications.

+   Prompt Engineering Guide repo contains a pretty comprehensive collection of education materials on prompt engineering.

+   learnprompting.org

+   [PromptPerfect](https://link.zhihu.com/?target=https%3A//promptperfect.jina.ai/)

+   [Semantic Kernel](https://link.zhihu.com/?target=https%3A//github.com/microsoft/semantic-kernel)

### 3.2 数据集

+   [Anthropic's Red Team dataset(](https://link.zhihu.com/?target=https%3A//github.com/anthropics/hh-rlhf/tree/master/red-team-attempts)opens in a new tab) ， (论文)(opens in a new tab)

+   [Awesome ChatGPT Prompts](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/fka/awesome-chatgpt-prompts)(opens in a new tab)

+   DiffusionDB(opens in a new tab)

+   [Midjourney Prompts](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/succinctly/midjourney-prompts)(opens in a new tab)

+   [P3 - Public Pool of Prompts](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/bigscience/P3)(opens in a new tab)

+   PartiPrompts(opens in a new tab)

+   [Real Toxicity Prompts](https://link.zhihu.com/?target=https%3A//allenai.org/data/real-toxicity-prompts)(opens in a new tab)

+   [Stable Diffusion Dataset](https://link.zhihu.com/?target=https%3A//huggingface.co/datasets/Gustavosta/Stable-Diffusion-Prompts)(opens in a new tab)

+   WritingPrompts(opens in a new tab)

### 3.3 相关论文

**综述**

+   [N](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.14725)ature Language Reasoning, A Survey(opens in a new tab) (March 2023)

+   [A](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.07842)ugmented Language Models: a Survey(opens in a new tab) (Feb 2023)

+   [A](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2301.00234) Survey for In-context Learning(opens in a new tab) (Dec 2022)

+   [Towards Reasoning in Large Langua](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2212.10403)ge Models: A Survey(opens in a new tab) (Dec 2022)

+   [R](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2212.09597)easoning with Language Model Prompting: A Survey(opens in a new tab) (Dec 2022)

+   [E](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2206.07682)mergent Abilities of Large Language Models(opens in a new tab) (Jun 2022)

+   [A Taxonomy of Prompt Modifiers for Text-](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2204.13988)To-Image Generation(opens in a new tab) (Apr 2022)

+   [Pre](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2107.13586)-train, Prompt, and Predict: A Systematic Survey of Prompting Methods in Natural Language Processing(opens in a new tab) (Jul 2021)

**方法**

+   [Self-Refine: Iterative Refinement](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.17651v1) with Self-Feedback(opens in a new tab) (Mar 2023)

+   [kNN Prompting: Beyond-Context Learning with Cali](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.13824)bration-Free Nearest Neighbor Inference(opens in a new tab) (Mar 2023)

+   [Vi](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.13283)sual-Language Prompt Tuning with Knowledge-guided Context Optimization(opens in a new tab) (Mar 2023)

+   [Fairness-guided Few-shot Prompting for La](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.13217)rge Language Models(opens in a new tab) (Mar 2023)

+   [Context-faithful Prompting for La](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.11315)rge Language Models(opens in a new tab) (Mar 2023)

+   [Is](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.10475) Prompt All You Need? No. A Comprehensive and Broader View of Instruction Learning(opens in a new tab) (Mar 2023)

+   [UP](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.08518)RISE: Universal Prompt Retrieval for Improving Zero-Shot Evaluation(opens in a new tab) (Mar 2023)

+   [Model-tuning Via Prompts Makes NLP Models A](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.07320)dversarially Robust(opens in a new tab) (Mar 2023)

+   [St](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.03922)ructure Pretraining and Prompt Tuning for Knowledge Graph Transfer(opens in a new tab) (March 2023)

+   [Co](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.03628)TEVer: Chain of Thought Prompting Annotation Toolkit for Explanation Verification(opens in a new tab) (March 2023)

+   [Larger language models do in-context l](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.03846)earning differently(opens in a new tab) (March 2023)

+   [OpenICL: An Open-Source Framework for ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.02913)In-context Learning(opens in a new tab) (March 2023)

+   [Dynamic Prompting: A Unified Framewor](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.02909)k for Prompt Tuning(opens in a new tab) (March 2023)

+   [Mu](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.02861)ltitask Prompt Tuning Enables Parameter-Efficient Transfer Learning(opens in a new tab) (March 2023)

+   [Ef](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.02577)fectiveness of Data Augmentation for Prefix Tuning with Limited Data(opens in a new tab) (March 2023)

+   [Mixture of Soft Prompts for Controlla](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.01580)ble Data Generation(opens in a new tab) (March 2023)

+   [Prompt, Generate, then Cache: Cascade of Foundatio](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.02151)n Models makes Strong Few-shot Learners(opens in a new tab) (March 2023)

+   [How Robust is GPT-3.5 to Predecessors? A Comprehensiv](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.00293)e Study on Language Understanding Tasks(opens in a new tab) (March 2023)

+   [Ca](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2302.10198.pdf)n ChatGPT Understand Too? A Comparative Study on ChatGPT and Fine-tuned BERT(opens in a new tab) (Feb 2023)

+   [Ev](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.14838)oPrompting: Language Models for Code-Level Neural Architecture Search(opens in a new tab) (Feb 2023)

+   [I](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.14691)n-Context Instruction Learning(opens in a new tab) (Feb 2023)

+   [Chain of Hindsight Aligns Language M](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.02676)odels with Feedback(opens in a new tab) (Feb 2023)

+   [La](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.14045)nguage Is Not All You Need: Aligning Perception with Language Models(opens in a new tab) (Feb 2023)

+   [Au](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.12822)tomatic Prompt Augmentation and Selection with Chain-of-Thought from Labeled Data(opens in a new tab) (Feb 2023)

+   [Active Prompting with Chain-of-Thought for La](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.12246)rge Language Models(opens in a new tab) (Feb 2023)

+   [More than you've asked for: A Comprehensive Analysis of Novel Prompt Inject](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.12173)ion Threats to Application-Integrated Large Language Models(opens in a new tab) (Feb 2023)

+   [A ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.11382)Prompt Pattern Catalog to Enhance Prompt Engineering with ChatGPT(opens in a new tab) (Feb 2023)

+   [Guiding Large Language Models via Directional](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.11520) Stimulus Prompting(opens in a new tab) (Feb 2023)

+   [H](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.11521)ow Does In-Context Learning Help Prompt Tuning?(opens in a new tab) (Feb 2023)

+   [Sc](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.09236)alable Prompt Generation for Semi-supervised Learning with Language Models(opens in a new tab) (Feb 2023)

+   [Bounding the Capabilities of Large Language Models in Open ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.09185)Text Generation with Prompt Constraints(opens in a new tab) (Feb 2023)

+   [À-](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.07994)la-carte Prompt Tuning (APT): Combining Distinct Data Via Composable Prompting(opens in a new tab) (Feb 2023)

+   [Gr](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.08043)aphPrompt: Unifying Pre-Training and Downstream Tasks for Graph Neural Networks(opens in a new tab) (Feb 2023)

+   [The Capacity for Moral Self-Correction in La](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.07459)rge Language Models(opens in a new tab) (Feb 2023)

+   [SwitchPrompt: Learning Domain-Specific Gated Soft Prompts for](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.06868) Classification in Low-Resource Domains(opens in a new tab) (Feb 2023)

+   [E](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.05619)valuating the Robustness of Discrete Prompts(opens in a new tab) (Feb 2023)

+   [C](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.05698)ompositional Exemplars for In-context Learning(opens in a new tab) (Feb 2023)

+   [Hard Prompts Made Easy: Gradient-Based Discrete Optim](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.03668)ization for Prompt Tuning and Discovery(opens in a new tab) (Feb 2023)

+   [Multimodal Chain-of-Thought Reasoning](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.00923) in Language Models(opens in a new tab) (Feb 2023)

+   [La](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.00093)rge Language Models Can Be Easily Distracted by Irrelevant Context(opens in a new tab) (Feb 2023)

+   [Synthetic Prompting: Generating Chain-of-Thought D](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.00618)emonstrations for Large Language Models(opens in a new tab) (Feb 2023)

+   [Progressive Prompts: Continual Learning ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2301.12314)for Language Models(opens in a new tab) (Jan 2023)

+   [B](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2301.08721)atch Prompting: Efficient Inference with LLM APIs(opens in a new tab) (Jan 2023)

+   [Demonstrate-Search-Predict: Composing retrieval and lang](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2212.14024)uage models for knowledge-intensive NLP(opens in a new tab) (Dec 2022)

+   [On Second Thought, Let's Not Think Step by Step! B](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2212.08061)ias and Toxicity in Zero-Shot Reasoning(opens in a new tab) (Dec 2022)

+   [C](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2212.08073)onstitutional AI: Harmlessness from AI Feedback(opens in a new tab) (Dec 2022)

+   [Successive Prompting for Decomposin](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2212.04092)g Complex Questions(opens in a new tab) (Dec 2022)

+   [Large Language Models are reasoners wit](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2212.09561v1)h Self-Verification(opens in a new tab) (Dec 2022)

+   [Di](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2212.09251)scovering Language Model Behaviors with Model-Written Evaluations(opens in a new tab) (Dec 2022)

+   [St](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2212.06713)ructured Prompting: Scaling In-Context Learning to 1,000 Examples(opens in a new tab) (Dec 2022)

+   [P](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2211.10435)AL: Program-aided Language Models(opens in a new tab) (Nov 2022)

+   [Large Language Models Are Human-Lev](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2211.01910)el Prompt Engineers(opens in a new tab) (Nov 2022)

+   [Ignore Previous Prompt: Attack Techniques ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2211.09527)For Language Models(opens in a new tab) (Nov 2022)

+   [Ma](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2210.07321)chine Generated Text: A Comprehensive Survey of Threat Models and Detection Methods(opens in a new tab) (Nov 2022)

+   [Teaching Algorithmic Reasoning via ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2211.09066)In-context Learning(opens in a new tab) (Nov 2022)

+   [Enh](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2211.11875)ancing Self-Consistency and Performance of Pre-Trained Language Models through Natural Language Inference(opens in a new tab) (Nov 2022)

+   [Ask Me Anything: A simple strategy for prompt](https://link.zhihu.com/?target=https%3A//paperswithcode.com/paper/ask-me-anything-a-simple-strategy-for)ing language models(opens in a new tab) (Oct 2022)

+   [R](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2210.01296)ecitation-Augmented Language Models(opens in a new tab) (Oct 2022)

+   [ReAct: Synergizing Reasoning and Acting](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2210.03629) in Language Models(opens in a new tab) (Oct 2022)

+   [Prompting GPT-3 To Be Reliable(](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2210.09150)opens in a new tab) (Oct 2022)

+   [Decomposed Prompting: A Modular Approach for So](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2210.02406)lving Complex Tasks(opens in a new tab) (Oct 2022)

+   [La](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2210.01240v3)nguage Models Are Greedy Reasoners: A Systematic Formal Analysis of Chain-of-Thought(opens in a new tab) (Oct 2022)

+   [Evaluating the Susceptibility of Pre-Trained Language Mode](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2209.02128)ls via Handcrafted Adversarial Examples(opens in a new tab) (Sep 2022)

+   [Dy](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2209.14610)namic Prompt Learning via Policy Gradient for Semi-structured Mathematical Reasoning(opens in a new tab) (Sep 2022)

+   [Promptagator: Few-shot Dense Retrie](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2209.11755)val From 8 Examples(opens in a new tab) (Sep 2022)

+   [Atlas: Few-shot Learning with Retrieval Augmen](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2208.03299)ted Language Models(opens in a new tab) (Nov 2022)

+   [DocPrompting: Generating Code by ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2207.05987)Retrieving the Docs(opens in a new tab) (July 2022)

+   [On the Advance of Making Language Mode](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2206.02336)ls Better Reasoners(opens in a new tab) (June 2022)

+   [L](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2205.11916)arge Language Models are Zero-Shot Reasoners(opens in a new tab) (May 2022)

+   [Ma](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2205.11822)ieutic Prompting: Logically Consistent Reasoning with Recursive Explanations(opens in a new tab) (May 2022)

+   [MRKL](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2205.00445) Systems: A modular, neuro-symbolic architecture that combines large language models, external knowledge sources and discrete reasoning(opens in a new tab) (May 2022)

+   [PPT: Pre-trained Prompt Tuning fo](https://link.zhihu.com/?target=https%3A//aclanthology.org/2022.acl-long.576/)r Few-shot Learning(opens in a new tab) (Mqy 2022)

+   [Toxicity Detection with Generative Pro](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2205.12390)mpt-based Inference(opens in a new tab) (May 2022)

+   [L](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2205.01543)earning to Transfer Prompts for Text Generation(opens in a new tab) (May 2022)

+   [Th](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2205.03401)e Unreliability of Explanations in Few-shot Prompting for Textual Reasoning(opens in a new tab) (May 2022)

+   [A Taxonomy of Prompt Modifiers for Text-](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2204.13988)To-Image Generation(opens in a new tab) (Apr 2022)

+   [Pr](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2203.06566)omptChainer: Chaining Large Language Model Prompts through Visual Programming(opens in a new tab) (Mar 2022)

+   [Se](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2203.11171)lf-Consistency Improves Chain of Thought Reasoning in Language Models(opens in a new tab) (March 2022)

+   [Training language models to follow instructions](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2203.02155) with human feedback(opens in a new tab)

+   [Re](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2202.12837)thinking the Role of Demonstrations: What Makes In-Context Learning Work?(opens in a new tab) (Feb 2022)

+   [Ch](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2201.11903)ain of Thought Prompting Elicits Reasoning in Large Language Models(opens in a new tab) (Jan 2022)

+   [Sh](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2112.00114)ow Your Work: Scratchpads for Intermediate Computation with Language Models(opens in a new tab) (Nov 2021)

+   [AI Chains: Transparent and Controllable Human-AI Interaction b](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2110.01691)y Chaining Large Language Model Prompts(opens in a new tab) (Oct 2021)

+   [Generated Knowledge Prompting for Co](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2110.08387)mmonsense Reasoning(opens in a new tab) (Oct 2021)

+   [Multitask Prompted Training Enables Zero-Shot ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2110.08207)Task Generalization(opens in a new tab) (Oct 2021)

+   [R](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2109.07830)eframing Instructional Prompts to GPTk's Language(opens in a new tab) (Sep 2021)

+   [De](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2109.06977)sign Guidelines for Prompt Engineering Text-to-Image Generative Models(opens in a new tab) (Sep 2021)

+   [Making Pre-trained Language Models Bette](https://link.zhihu.com/?target=https%3A//aclanthology.org/2021.acl-long.295)r Few-shot Learners(opens in a new tab) (Aug 2021)

+   [Fantastically Ordered Prompts and Where to Find Them: Overc](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2104.08786)oming Few-Shot Prompt Order Sensitivity(opens in a new tab) (April 2021)

+   [B](https://link.zhihu.com/?target=https%3A//aclanthology.org/2021.eacl-main.316)ERTese: Learning to Speak to BERT(opens in a new tab) (April 2021)

+   [The Power of Scale for Parameter-Effi](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2104.08691)cient Prompt Tuning(opens in a new tab) (April 2021)

+   [Pr](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2102.07350)ompt Programming for Large Language Models: Beyond the Few-Shot Paradigm(opens in a new tab) (Feb 2021)

+   [Ca](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2102.09690)librate Before Use: Improving Few-Shot Performance of Language Models(opens in a new tab) (Feb 2021)

+   [Prefix-Tuning: Optimizing Continuous Pro](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2101.00190)mpts for Generation(opens in a new tab) (Jan 2021)

+   [Learning to Generate Task-Specific Adapters fr](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2101.00420)om Task Description(opens in a new tab) (Jan 2021)

+   [Making Pre-trained Language Models Bette](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2012.15723)r Few-shot Learners(opens in a new tab) (Dec 2020)

+   [L](https://link.zhihu.com/?target=https%3A//aclanthology.org/2020.emnlp-main.105/)earning from Task Descriptions(opens in a new tab) (Nov 2020)

+   [AutoPrompt: Eliciting Knowledge from Language Mode](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2010.15980)ls with Automatically Generated Prompts(opens in a new tab) (Oct 2020)

+   [L](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2005.14165)anguage Models are Few-Shot Learners(opens in a new tab) (May 2020)

+   [H](https://link.zhihu.com/?target=https%3A//direct.mit.edu/tacl/article/doi/10.1162/tacl_a_00324/96460/How-Can-We-Know-What-Language-Models-Know)ow Can We Know What Language Models Know?(opens in a new tab) (July 2020)

+   [S](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2001.08361)caling Laws for Neural Language Models(opens in a new tab) (Jan 2020)

**应用**

+   [PaLM 2 Technical Report(](https://link.zhihu.com/?target=https%3A//ai.google/static/documents/palm2techreport.pdf)opens in a new tab) (May 2023)

+   [B](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.17564)loombergGPT: A Large Language Model for Finance(opens in a new tab) (March 2023)

+   [Med](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.17408)ical Intervention Duration Estimation Using Language-enhanced Transformer Encoder with Medical Prompts(opens in a new tab) (March 2023)

+   [Soft-prompt tuning to predict lung cancer using pri](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.15846)mary care free-text Dutch medical notes(opens in a new tab) (March 2023)

+   [Ta](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.16434)skMatrix.AI: Completing Tasks by Connecting Foundation Models with Millions of APIs(opens in a new tab) (March 2023)

+   [Larger Probes Tell a Different Story: Extending Psycholing](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.16445)uistic Datasets Via In-Context Learning(opens in a new tab) (March 2023)

+   [Linguistically Informed ChatGPT Prompts to Enhance Japanese-Chine](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.15587)se Machine Translation: A Case Study on Attributive Clauses(opens in a new tab) (March 2023)

+   [Kn](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.14375)owledge-augmented Frame Semantic Parsing with Hybrid Prompt-tuning(opens in a new tab) (March 2023)

+   [De](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.15413)biasing Scores and Prompts of 2D Diffusion for Robust Text-to-3D Generation(opens in a new tab) (March 2023)

+   [Zero-shot Model Diagnosis(](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.15441%23)opens in a new tab) (March 2023)

+   [Prompting Large Language Models to Generate Code-Mixed Texts:](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.13592) The Case of South East Asian Languages(opens in a new tab) (March 2023)

+   [SPe](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.13035)C: A Soft Prompt-Based Calibration on Mitigating Performance Variability in Clinical Notes Summarization(opens in a new tab) (March 2023)

+   [L](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.11455)arge Language Models and Simple, Stupid Bugs(opens in a new tab) (March 2023)

+   [Can](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.09325) Generative Pre-trained Transformers (GPT) Pass Assessments in Higher Education Programming Courses?(opens in a new tab) (Mar 2023)

+   [SelfCheckGPT: Zero-Resource Black-Box Hallucination Detecti](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.08896)on for Generative Large Language Models(opens in a new tab) (Mar 2023)

+   [ICL-D3IE: In-Context Learning with Diverse Demonstrations Updat](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.05063)ing for Document Information Extraction(opens in a new tab) (March 2023)

+   [MathPrompter: Mathematical Reasoning using La](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.05398)rge Language Models(opens in a new tab) (March 2023)

+   [Pr](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.05400)ompt-Based Learning for Thread Structure Prediction in Cybersecurity Forums(opens in a new tab) (March 2023)

+   [Cho](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.03199)ice Over Control: How Users Write with Large Language Models using Diegetic and Non-Diegetic Prompting(opens in a new tab) (March 2023)

+   [Prompting Large Language Models with Answer Heuristics for Kn](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.01903)owledge-based Visual Question Answering(opens in a new tab) (March 2023)

+   [So](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.00815)ft Prompt Guided Joint Learning for Cross-Domain Sentiment Analysis(opens in a new tab) (March 2023)

+   [SpeechPrompt v2: Prompt Tuning for Speech C](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2303.00733)lassification Tasks(opens in a new tab) (March 2023)

+   [Go](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.14233)al Driven Discovery of Distributional Differences via Language Descriptions(opens in a new tab) (Feb 2023)

+   [Navigating the Grey Area: Expressions of Overconfid](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.13439)ence and Uncertainty in Language Models(opens in a new tab) (Feb 2023)

+   [T](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.14169)abGenie: A Toolkit for Table-to-Text Generation(opens in a new tab) (Feb 2023)

+   [SGL-PT: A Strong Graph Learner with ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.12449)Graph Prompt Tuning(opens in a new tab) (Feb 2023)

+   [Few-Shot Table-to-Text Generation with P](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.12468)rompt-based Adapter(opens in a new tab) (Feb 2023)

+   [Language Models Are Few-shot Learners for Pr](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.12692)ognostic Prediction(opens in a new tab) (Feb 2023)

+   [ST](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.12784)A: Self-controlled Text Augmentation for Improving Text Classifications(opens in a new tab) (Feb 2023)

+   [Che](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.12813)ck Your Facts and Try Again: Improving Large Language Models with External Knowledge and Automated Feedback(opens in a new tab) (Feb 2023)

+   [How](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.10916) Generative AI models such as ChatGPT can be (Mis)Used in SPC Practice, Education, and Research? An Exploratory Study(opens in a new tab) (Feb 2023)

+   [Gr](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.08961)imm in Wonderland: Prompt Engineering with Midjourney to Illustrate Fairytales(opens in a new tab) (Feb 2023)

+   [La](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.08068)belPrompt: Effective Prompt-based Learning for Relation Classification(opens in a new tab) (Feb 2023)

+   [Language Model Crossover: Variation through](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.09236) Few-Shot Prompting(opens in a new tab) (Feb 2023)

+   [Pr](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.08102)ompt Tuning of Deep Neural Networks for Speaker-adaptive Visual Speech Recognition(opens in a new tab) (Feb 2023)

+   [The Capacity for Moral Self-Correction in La](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.07459)rge Language Models(opens in a new tab) (Feb 2023)

+   [Prompting for Multimodal Hateful ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.04156)Meme Classification(opens in a new tab) (Feb 2023)

+   [PL](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.03269)ACES: Prompting Language Models for Social Conversation Synthesis(opens in a new tab) (Feb 2023)

+   [Co](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.01441)mmonsense-Aware Prompting for Controllable Empathetic Dialogue Generation(opens in a new tab) (Feb 2023)

+   [Crawling the Internal Knowledge-Base](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2301.12810) of Language Models(opens in a new tab) (Jan 2023)

+   [Le](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2212.02199)gal Prompt Engineering for Multilingual Legal Judgement Prediction(opens in a new tab) (Dec 2022)

+   [Investigating Prompt Engineering ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2211.15462)in Diffusion Models(opens in a new tab) (Nov 2022)

+   [Learn to Explain: Multimodal Reasoning via Though](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2209.09513v2)t Chains for Science Question Answering(opens in a new tab) (Sep 2022)

+   [Conversing with Copilot: Exploring Prompt Engineering for Solv](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2210.15157)ing CS1 Problems Using Natural Language(opens in a new tab) (Oct 2022)

+   [Pi](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2210.14699)loting Copilot and Codex: Hot Temperature, Cold Prompts, or Black Magic?(opens in a new tab) (Oct 2022)

+   [Plot Writing From Scratch Pre-Trai](https://link.zhihu.com/?target=https%3A//aclanthology.org/2022.inlg-main.5)ned Language Models(opens in a new tab) (July 2022)

+   [Survey of Hallucination in Natural ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2202.03629)Language Generation(opens in a new tab) (Feb 2022)

**论文汇编**

+   [Chain-of-Thought Papers](https://link.zhihu.com/?target=https%3A//github.com/Timothyxxx/Chain-of-ThoughtsPapers)(opens in a new tab)

+   [Papers with Code](https://link.zhihu.com/?target=https%3A//paperswithcode.com/task/prompt-engineering)(opens in a new tab)

+   [Prompt Papers](https://link.zhihu.com/?target=https%3A//github.com/thunlp/PromptPapers%23papers)

## 参考资料

[1] Prompt Engineering

[2] Prompt Engineering Guide

[3] [https:// platform.ope](https://link.zhihu.com/?target=https%3A//platform.openai.com/playground)nai.com/pla yground

[4] [[2](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2201.11903)201.11903] Chain-of-Thought Prompting Elicits Reasoning in Large Language Models (arxiv.org)

[5] [[2211.01910] Large Language Models Are ](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2211.01910)Human-Level Prompt Engineers (arxiv.org)

[6] [Automatic Prompt Augmentation and Selection](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2302.12822) with Chain-of-Thought from Labeled Data

> 杏花疏影里，吹笛到天明。 —— 陈与义 《临江仙·夜登小阁忆洛中旧游》