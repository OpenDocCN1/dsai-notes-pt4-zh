
# 万字长文深度解析LLM Agent反思工作流框架Reflexion下篇：ReflectionAgent workflow

> 来源：[https://agijuejin.feishu.cn/docx/HfxNdXldMoHrGhx2fsxcC7XRngg](https://agijuejin.feishu.cn/docx/HfxNdXldMoHrGhx2fsxcC7XRngg)

> 作者：还就可 | 深入LLM Agent应用开发

> 原文：[https://mp.weixin.qq.com/s/J8QbyJHIJ2NNv2dwy2yBZw](https://mp.weixin.qq.com/s/J8QbyJHIJ2NNv2dwy2yBZw)

在前文 [LM-Agents 万字长文深度解析：ReactAgent workflow](http://mp.weixin.qq.com/s?__biz=MzI0OTAzNTEwMw==&mid=2247483722&idx=1&sn=71c9dcd3579e5cf7bce3319e3d7b8fed&chksm=e996e17adee1686cb17bfc8da85fd70e54b292247f3a5b06acf3acbe3fdd873b91d16d88815c&scene=21#wechat_redirect) 中，我们详细解析了`ReactAgent`的工作流程，而本文则将在此基础上探讨反思技巧的应用。之前的文章中 LLM Agent提效进阶1：反思工作流—91%精度大超GPT-4 24% 我们已经对反思技巧进行了探讨，并展示了在一些数据集上性能表现。现在，我们将深入探讨在`ReactAgent`上构建反思工作流的具体实现。

话不多说，先上流程设计图。

## 1. 如何设计反思

依然是老规矩，先从Prompt设计着手，然后进行流程设计

### 1.1 Prompt设计

就像在ReAct的Prompt设计一样，首先进行任务说明（强烈的鼓励它），然后说明任务的输入和输出，给定样例。对于反思Agent来说，它的输入就是之前的尝试记录。

```
你是一个能够通过自我反思来改进的高级推理代理。你将获得之前的推理测试流程，在之前的测试中你尝试访问Docstore API并回答了问题。由于你猜错了答案（使用了Finish[<answer>]），或者用完了设定的推理步骤数，你未能成功回答问题。诊断失败的可能原因，并制定一个新的、简明的高级计划，旨在减轻同样的失败。要求必须使用完整的语句来回复。
这里有一些例子：
{examples}
之前的尝试：
问题：{question}{scratchpad}
反思：
```

我认为一个好的示例也同等重要，LLM会按照你的设想进行回复。

```
之前的尝试:
Question: The Rome Protocols were signed by three Prime Ministers one of which was assassinated as part of what?
Thought 1: I need to search Rome Protocols, find the three Prime 
....
Action 3: Finish[World War II]

反思: I searched one of the prime ministers involved in the signing, then attempted to answer right away. I should have searched each of the prime ministers, then looked up 'death' on each of their pages in order to get more information before answering.
```

### 1.2 流程设计

![](img/d32838f09e5ce21c7017cc0c509bbe95.png)

你可能注意到了`Reflect`也有使用`Scratchpad`来记录反思过程，并且`ReAct`的`Prompt`我们加入了`reflection`，这有助于防止`Thought`在根据反思结果推理的时候陷入循环，反复推倒出同一结果，相当于给`ReAct`加了一层记忆。接下来，我们深入`Reflexion`的 `ReactReflectAgent` 查看反思具体实现。

## 2. `ReactReflectAgent.run`

在 `ReactReflectAgent` 的 `run` 方法中调用了 `ReactAgent.run(self, reset)` 进行迭代。上文我们有详细描述过 `ReactAgent.run` 这一迭代TAO的过程。在`ReactAgent`多次迭代仍无法获取正确答案时候，进入反思流程。

> 我觉得这里在实际应用的时候有问题的，问答类的你是法知道结果到底正确与否，并不能知晓`self.is_correct()`，这像是泄露答案一样，除非让用户主动参与进行互动表示不正确，但如果用户都知道了，还问你干啥呢？但如果是编程任务，可以设定输入输出并通过编写测试用例来对程序进行测试，从而确认结果是否正确。不知道你们怎么看？

```
def run(self, reset=True, reflect_strategy: ReflexionStrategy = ReflexionStrategy.REFLEXION) -> None:
    if (self.is_finished() or self.is_halted()) and not self.is_correct():
        self.reflect(reflect_strategy)

    ReactAgent.run(self, reset)
```

在前文中我们最后有给出整个流程，设置5次尝试来调用 `ReactReflectAgent.run` 。

```
n = 5
for i in range(n):
    for agent in [a for a in agents if not a.is_correct()]:
        agent.run(reflect_strategy=strategy)
        print(f'Answer: {agent.key}')
```

所以当 `ReactAgent.run` 执行完5次step结束之后，在外部的for 循环5次尝试，会再次进入 `ReactReflectAgent.run` 方法。此次会判定出该`agent`的 `self.is_finished()` or `self.is.halted()` 就是true了，至于 `self.is_correct()` 我只能说提前知晓答案正确与否了。假设 `self.is_correct()` 为false，执行 `reflect(reflect_strategy)` 。

```
def reflect(self, strategy: ReflexionStrategy) -> None:
    if ...
    elif strategy == ReflexionStrategy.REFLEXION:
        self.reflections += [self.prompt_reflection()]
        self.reflections_str = format_reflections(self.reflections)
    ....

def prompt_reflection(self) -> str:
return format_step(self.reflect_llm(self._build_reflection_prompt()))
```

反思策略为`REFLEXION`, 构建反思Prompt。 注意这里将提取的`reflection`结果保存到list `self.reflections`, 并且将结果格式化为`reflection_str`，在`ReAct`中构建Prompt会填入该反思结果，从而避免陷入相同的推理中，可以认为`self.reflections`就是`reflect`的`scratchpad`。

其中`reflect_prompt`定义如下，然后给定一些`examples`，并将之前错误的问题和推理步骤附上来。

```
REFLECT_INSTRUCTION = """You are an advanced reasoning agent that can improve based on self refection. You will be given a previous reasoning trial in which you were given access to an Docstore API environment and a question to answer. You were unsuccessful in answering the question either because you guessed the wrong answer with Finish[<answer>], or you used up your set number of reasoning steps. In a few sentences, Diagnose a possible reason for failure and devise a new, concise, high level plan that aims to mitigate the same failure. Use complete sentences.  
Here are some examples:
{examples}
Previous trial:
Question: {question}{scratchpad}
Reflection:"""
```

其中`reflect_examples`就是`REFLECTIONS`定义的Prompt，该Prompt定义了两个案例。

```
REFLECTIONS = """
Previous Trial:
Question: Kam Heskin plays Paige Morgan in a 2004 film directed by who?
...
Observation 6: ....
Reflection: I got stuck in a loop where I kept trying to search 'The Prince & Me (2004 film)' but the page could not be found. Instead I should have tried to search the similar results that had a similar name to see and they were made in 2004.
"""
```

既然Prompt已经定义好，接下来就是调用 `self.reflect_llm(self._build_reflection_prompt())` ，LLM就会按照给定的example来反思并给出新的思路，如下文所示反思的回复。

> I got stuck on the first search and kept trying to find information about VIVA Media AG, when I should have checked the similar results instead. The similar results that came up were not useful either, as they did not talk about a name change. I should have tried another initial search term instead of persevering with a dead end.

其中 `reflect_llm` 在初始化的时候，没有设定遇到换行停止，max token也增加到250，是因为反思不需要严格格式执行，并且应该让其尽可能的反思出具体的步骤。

```
AnyOpenAILLM(
   temperature=0,
   max_tokens=250,
   model_name="gpt-3.5-turbo",
   openai_api_key="sk"),
)
```

之后，使用`format_reflections`将反思的结果格式化。 `header + 'Reflections:\n- ' + '\n- '.join([r.strip() for r in reflections])`

其中`header`为固定字符串，最终格式化后的输出就是

```
You have attempted to answer following question before and failed. The following reflection(s) give a plan to avoid failing to answer the question in the same way you did previously. Use them to improve your strategy of correctly answering the given question.
Reflections:
I got stuck on the first search and kept trying to find information about VIVA Media AG, when I should have checked the similar results instead. The similar results that came up were not useful either, as they did not talk about a name change. I should have tried another initial search term instead of persevering with a dead end.
```

所以 `ReactAgent.run` 的Prompt会填充该段反思内容，重新迭代TAO的流程。由于反思尝试设置的是5次，我们看到最终输出的反思条例都装载到Prompt中，以便后面的推理能够吸取前面的教训。

```
Solve a question answering task with interleaving Thought, Action, Observation steps. Thought can reason about the current situation, and Action must be one of the three types: 
...
(END OF EXAMPLES)
You have attempted to answer following question before and failed....
Reflections:
- The search results ...
- I should have read the question more carefully and noticed that it ...
Question: {question}{scratchpad}
```

## 3. 总结

`Reflect`是在`React` 5轮TAO迭代之后的总结和反思，给出下一步的思考方向。同时尝试多次反思，并将每次反思的内容给到LLM，使之避免再次走入已经反思过的方向。综合来看，反思`Reflect`是一种通过自动化迭代LLM，不断试错，从而得出更好的结果的思路。比如在编写代码的任务中，通过LLM编写代码，再编写测试代码，这样我们就能够通过TAO得到编写的代码是否正确，从而决定是否需要再次循环TAO或者进一步的反思来优化提升代码。下一篇，我们开始讲述LLM中工具调用工作流。

如果您意犹未尽，想要参与LangChain实战课程，可以点击原文链接查看购买，亲历LLM应用开发之旅。关注我，及时获得更新。
