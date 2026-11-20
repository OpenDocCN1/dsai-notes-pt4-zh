
# 大模型提示工程方法

## Least-to-Most

相较于Self-C onsistency 简单多次生成，然后选择最自洽的结果来讲，Least-to-Most更为精细。它在解决复杂问题时，先引导模型把问题拆分成子问题；然后再让大模型逐一回答子问题，并把子问题的回答作为下一个问题回答的上文，直到给出最终答案。可以看出，Least-to-Most是一种循序渐进式的引导大模型解决问题的方式。

下面是一个实际的例子：

```
CUSTOMER INQUIRY:I just bought a T-shirt from your Arnold collection on March 1st. I saw that it was on discount, so bought a shirt that was originally $30, and got 40% off. I saw that you have a new discount for shirts at 50%. I&#x27;m wondering if I can return the shirt and have enough store credit to buy two of your shirts?
INSTRUCTIONS:You are a customer service agent tasked with kindly responding to customer inquiries. Returns are allowed within 30 days. Today&#x27;s date is March 29th. There is currently a 50% discount on all shirts. Shirt prices range from $18-$100 at your store. Do not make up any information about discount policies.Determine if the customer is within the 30-day return window.
```

未使用Least-To-Most方式，`text-danvnci-003`回答错误。

采用该方法，首先诱导提出子问题：

![](img/368ae64db584e14191760375ec053517.png)

然后让大模型依次回答这些问题：

![](img/320148364f960f7f2795a971b579222f.png)

可以看到，大模型在引导下，正确的回答了问题：

```
Yes, the customer can purchase two shirts at the current 50% discount rate with their store credit, as $18 of store credit will cover the cost of two shirts at $36.
```

## Self-Ask

与Least-To-Most类似，对于一个大模型没有在训练时直接见到的问题，通过诱导大模型以自我提问的问题，将问题分解为更小的后续问题，而这些问题可能在训练数据中见到过，这样就可以通过自问自答的方式，最终获得正确答案。

![](img/c0b2992f9ab5e7388469eec138521638.png)

在此基础上，更进一步，对于没有见过的子问题还可以借助外部检索，帮助大模型最终完成问题解答。

![](img/5876f139bc0dc85e98aabbcf76c42cd3.png)

## Meta-Prompting

简单讲，既然自己不知道写什么样的prompt有利于解决问题，那么让大模型帮你写提示，然后使用大模型提供的prompt，再去操纵大模型，从而获得效果改进，这就是元提示。

![](img/436559145f8c89e415faad555f9bcdde.png)

一个实际的为例：

![](img/b1d4917fcefb2e5189670b93a74a5e38.png)

完整例子见：https://chat.openai.com/share/77c59aeb-a2d6-4df8-abf6-42c8d19aba3d

## Knowledge Generation Prompting

通过知识生成来增强prompt，提升大模型的效果。其基本思路就是先基于问题让大模型给出问题相关的知识，再将知识整合到问题中，从而让大模型给予更细致有针对性的回答。它在一些特定领域的任务中有效果，比如问题本身比较笼统，这个时候就可以通过这种方法增强。

![](img/7c689900f2d60ec46548bd361b759ed9.png)

https://browse.arxiv.org/pdf/2110.08387.pdf

一个简单的示例，让大模型推荐一份健康、方便制作的早餐食谱：

1）创建一个提示模板，要求模型生成有关健康早餐选项的知识：

Prompt template:

```
Generate knowledge about healthy, easy-to-make breakfast recipes that are high in protein and low in sugar.
```

2）该模型可能会提供有关不同成分和配方的信息，例如：

```
Healthy breakfast options that are high in protein and low in sugar include Greek yogurt with berries, oatmeal with nuts and seeds, and avocado toast with eggs. These recipes are easy to make and require minimal cooking.
```

3）根据生成的知识，创建一个新的prompt，其中包含大模型提供的信息，并要求基于此提供食谱。

```
Based on the knowledge that Greek yogurt with berries, oatmeal with nuts and seeds, and avocado toast with eggs are healthy, high-protein, low-sugar breakfast options, provide a detailed recipe for making oatmeal with nuts and seeds.&quot;
```

4)如此，大模型将会提供一个相对更为细致，有针对性的回答。

## Iterative Prompting

![](img/1556217944db0708e651d4a63b8459e5.png)

迭代型提示，是指和大模型进行交互时，不要把它看作是独立的过程，而是将前一次的回答作为上下文提供给大模型，这样的方式可以有效的提高模型信息的挖掘能力，并消除一些无关的幻觉。上图所示，在上下文的帮助下，大模型顺利的回答出结果，而标准的prompt可能无法回答或者产生幻觉，回答错误。

## TOT（Tree of Thoughts）

![](img/76fba3b8a9d6af18fead4de64f6e8bf0.png)

TOT是在 "思维链 "方法的基础上的扩展，并能够探索连贯的子步骤（“思维”），作为解决问题的中间步骤。简单讲，COT是和大模型的一次交互，但复杂问题，并不能一次搞定，那么，可以诱导大模型将复杂问题拆分为多层的树结构，这样每一步选择，都可以以树的模式（广度优先搜索（BFS）和深度优先搜索（DFS））动态选择最合适的路径，它 允许 大模型通过考虑多种不同的推理路径和自我评估选择来决定下一步行动方案，并在必要时进行前瞻或回溯以做出全局选择，从而执行深思熟虑的决策。

![](img/a1c35e589da6f7b6c2b9f8f38687a3e1.png)

https://arxiv.org/pdf/2305.08291.pdf

其基本过程是 首先，系统会将一个问题分解，并生成一个潜在推理“思维”候选者的列表。然后，对这些思维进行评估，系统会衡量每个想法产生所需解决方案的可能性，最后方案进行排序。

基本流程参考这个视频：

对应的prompt参考：

```
Step1 :
Prompt: I have a problem related to [describe your problem area]. Could you brainstorm three distinct solutions? Please consider a variety of factors such as [Your perfect factors]
Step 2:
Prompt: For each of the three proposed solutions, evaluate their potential. Consider their pros and cons, initial effort needed, implementation difficulty, potential challenges, and the expected outcomes. Assign a probability of success and a confidence level to each option based on these factors
Step 3:
Prompt: For each solution, deepen the thought process. Generate potential scenarios, strategies for implementation, any necessary partnerships or resources, and how potential obstacles might be overcome. Also, consider any potential unexpected outcomes and how they might be handled.
Step 4:
Prompt: Based on the evaluations and scenarios, rank the solutions in order of promise. Provide a justification for each ranking and offer any final thoughts or considerations for each solution
```

如果将其过程实现，基本实现架构为：

![](img/37b85e311c0772338b0d1662f7c9b91f.png)

一个执行的例子：

![](img/75b5befbd93bd6762ec682b4520768ba.png)

结果表明，ToT 能显著提高语言模型在需要非繁琐规划或搜索的新任务中解决问题的能力，如：24点游戏、数独等。

github地址：

https://github.com/princeton-nlp/tree-of-thought-llm

https://github.com/jieyilong/tree-of-thought-puzzle-solver

## GOT（Graph-of-Thoughts）

由于 ToT 方法为思维过程强加了严格的树结构，所以会极大限制 prompt 的推理能力，而GOT则更进一步，将整个结构变成了DAG图，实际上，人类在进行思考时，不会像 CoT 那样仅遵循一条思维链，也不是像 ToT 那样尝试多种不同途径，而是会形成一个更加复杂的思维网。GoT的新颖之处在于它能够对这些想法进行转换，进一步完善推理过程。

![](img/987a68d0f37fae67dc74aed0a67424b3.png)

https://arxiv.org/pdf/2308.09687v2.pdf

相较于TOT，主要的变化为：

+ 聚合，即将几个想法融合成一个统一的想法；
+ 精化，对单个思想进行连续迭代，以提高其精度；
+ 生成，有利于从现有思想中产生新的思想。

这种转换强调推理路线的融合，相对于之前的CoT或ToT模型，提供了更复杂的问题支持。

![](img/c4d0246c24c4092215ea9185d3312026.png)

此外，GoT引入了一个评估维度-思维容量（the volume of a thought），可用于评估 prompt 设计策略。研究者表示，使用这一指标的目标是更好地理解 prompt 设计方案之间的差异。 对于一个给定的思维 v，v 的容量是指 LLM 思维的数量，用户可以基于此使用有向边得到 v。 直观上说，这些就是有望对 v 做出贡献的所有 LLM 思维。

作者通过研究表明，通过整合聚合等思维变换技术，GoT 能让思维容量比其它方案显著更大。另一方面，GoT 是唯一能做到低延迟 log_k N 和高容量 N 的方案。GoT 之所以能做到这一点，是因为其利用了思维聚合，使其可从图分解中任何其它中间思维得到最终思维。

![](img/30d4747faf43c935aeb374cd720c32aa.png)

在具体技术官方实现上，由于GOT的更强泛化能力，因此，其本身也能支持COT或TOT方法。

```
from examples.sorting.sorting_032 import SortingPrompter, SortingParser, utilsfrom graph_of_thoughts import controller, operations
# Problem input
to_be_sorted = &quot;[0, 2, 6, 3, 8, 7, 1, 1, 6, 7, 7, 7, 7, 9, 3, 0, 1, 7, 9, 1, 3, 5, 1, 3, 6, 4, 5, 4, 7, 3, 5, 7]&quot;
# Create the Graph of Operationsgop = operations.GraphOfOperations()gop.append_operation(operations.Generate())gop.append_operation(operations.Score(scoring_function=utils.num_errors))gop.append_operation(operations.GroundTruth(utils.test_sorting))
# Configure the Language Model (Assumes config.json is in the current directory with OpenAI API key)lm = controller.ChatGPT(&quot;config.json&quot;, model_name=&quot;chatgpt&quot;)
# Create the Controllerctrl = controller.Controller(  lm,   gop,   SortingPrompter(),   SortingParser(),  # The following dictionary is used to configure the initial thought state  {    &quot;original&quot;: to_be_sorted,    &quot;current&quot;: &quot;&quot;,    &quot;method&quot;: &quot;cot&quot;  })
# Run the Controller and generate the output graphctrl.run()ctrl.output_graph(&quot;output_cot.json&quot;)
```

github实现：https://github.com/spcl/graph-of-thoughts

## Algorithm-of-Thoughts（AoT）

![](img/5e8489e7ae56d3a9830a86a5adf29ec1.png)

前面了解到TOT和GOT的基本过程，一个很直观的感觉就是会和大模型产生多次的交互，从而导致高昂的算力成本，整体的交互逻辑也比较复杂。那么，怎么样尽可能减少TOT等对大模型查询交互，甚至把迭代的过程直接放在大模型内部完成。而AOT就是旨在解决这一问题而出现。它是由弗吉尼亚 理工大学和微软今年8月刚刚提出的，研究者探究了 LLM 能否实现类似的对想法的分层探索，通过参考之前的中间步骤来筛除不可行的选项 —— 所有这些都在 LLM 的生成周 期内完成。利用 LLM 的递归能力，研究者构建了一种人类 - 算法混合方法。其实现方式是通过使用算法示例（从最初的候选项到经过验证的解决方案）。 与早期的单一查询方法（COT）和最近的多次查询策略（TOT/GOT）相比，该技术表现出更好的性能。在不增加查询请求数量的情况下，通过利用LLM的递归能力，提高其思路探索能力。与其他方法相比，AoT方法具有更高的效率和性能，比TOT高出近10%。

![](img/17eff2f798c1438118a228cfb0e64990.png)

https://browse.arxiv.org/pdf/2308.10379.pdf

官方提供了实现，下面是一个使用的示例：
