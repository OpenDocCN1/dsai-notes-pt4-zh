
# AutoGPT与LLM Agent解析

> 来源：[https://agijuejin.feishu.cn/docx/NH75duOOlo3m9bxbAXacF50pnCe](https://agijuejin.feishu.cn/docx/NH75duOOlo3m9bxbAXacF50pnCe)

> 作者：zijie0 | RandomGenerator

> 原文：https://mp.weixin.qq.com/s/4HDnCHylUof3IMr66UCeXw

前两周 `AutoGPT`，`BabyAGI` 等项目异常火爆，周末也正好花了点时间来看了下这些 AI agent 类项目的代码，写篇文章来总结一下对于当前这类项目进展的技术角度认识和思考，与大家一同交流。

## 从语言理解到任务执行

之前大多相关项目和产品都主要利用了 GPT 模型的语言理解方面的能力，例如生成文案的 Jasper，Notion AI，帮忙做网页、文档总结的 Glarity，Bearly.ai，做问答的 New Bing，ChatPDF 等。后续想要拓展 GPT 的应用范围，一个很自然的方向就是让 GPT 能够学会自己使用各种外部工具，来进行更广泛的任务类型的执行，做到“知行合一”😊。除了上面提到的 `AutoGPT` 和 `BabyAGI`，还有很多有意思的项目如 `Toolformer` [1] ， `HuggingGPT` [2] ， `Visual ChatGPT` [3] 等都在尝试这个方向。

这个任务执行说起来原理也不复杂，基本的套路还是让 GPT 去做生成，只不过我们会在 Prompt 中告诉 GPT，如果你需要调用一些外部工具，那么就按照特定的格式来生成一些指令/代码，程序接收到之后，再根据 GPT 生成的内容去调用外部工具并获得相应结果，这个结果再作为输入可以由 GPT 去做进一步的理解和生成，循环往复。以 `LangChain` 里最常见的 ReAct prompt 为例，输入给模型的内容如下：

```
...
你可以使用如下工具来完成任务：

1. 计算器，用来执行各种数学计算获取精确结果，输入表达式，例如 1 + 1，得到结果
...

问题：123 乘以 456 的结果是多少？
...
```

模型生成的内容如下：

```
思考：我需要使用计算器来计算 123 乘以 456 的结果
动作：调用计算器
动作输入：123 * 456
观测结果：
```

然后我们可以处理这段返回，调用计算器程序，拿到 123 * 456 的结果，然后将结果填写到观测结果后面，再让模型继续生成下一段内容。

这就是基本的任务执行的方法。更多内容也可以参考我之前对于 `LangChain` 的一些分享：微软 365 Copilot 是如何实现的？揭秘 LLM 如何生成指令 [4] 。

![](img/e0ff6938b72707f74b32e96a99693429.png)

典型的 ReAct prompt

## 模型记忆

另外一类非常常见的模式是通过外部存储来增强模型记忆。其中一个典型场景是长 session 的聊天过程，由于 GPT API 本身的输入信息有 4000 个 token 的限制，所以当聊天进行比较久之后，用户经常会发现 ChatGPT 已经“忘了”之前讲过的内容。另外一个典型场景是给 LLM 提供更多的新信息，像一些产品里能够对一整篇 PDF 甚至一整个知识库里的内容做理解和问答，那么自然不可能直接把所有这些额外信息都直接在 prompt 里扔给 GPT 去处理。

这时候就需要通过外部存储来帮助 GPT 拓展记忆。最简单的方法就是直接把这些对话记录，外部信息等以文本形式保存到文件或者数据库系统里，后续在与模型进行交互时，可以按需去获取这些外部存储中的信息。我们可以把 prompt 里的内容当成模型的“短期记忆”，那么这些外部存储自然就成为了“长期记忆”。除了前面提到的好处外，这种记忆系统模式还能一定程度上起到降低模型 hallucinations 的作用，避免纯粹依靠“生成”来实现任务目标。

获取长期记忆的方法，目前最常见的方式是通过“语义搜索”。大概意思就是利用一个 embedding 模型，将所有的记忆文本都转化为一个向量。而后续跟模型的交互信息也可以通过同样的 embedding 模型转化为向量，然后通过计算相似度来找到最相似的记忆文本。最后再将这些记忆文本拼接到 prompt 里，作为模型的输入。这类方法最热门的开源项目可以参考 OpenAI 官方的 `ChatGPT Retrieval Plugin` [5] 和 `LlamaIndex` [6] 。

![](img/63d78bb423e42480d48d4af9d8cfa412.png)

Retrieval Pattern

这种拓展模型记忆的模式相比人类大脑的运作方式来说感觉还有些“粗糙”，所谓的长期与短期记忆（包括 `LangChain` 与 `LlamaIndex` 中一些更复杂的实现），仍然是比较“hard coded”的感觉。如果未来在模型 context size 上有突破性的研究进展，那么当前的这类模式或许就不再需要了。

从整体的交互流程来看，这类模型记忆实现模式也可以看作是一种“任务执行”的方式，只不过这里的任务是“写入/获取记忆”，而不是“执行某个外部工具”。我们可以把两者统一来看，也就是当前大语言模型最常用的应用开发模式。后面我们也会看到，各种所谓的智能 agent 也都是在这个思路下进行拓展实现的。

![](img/880301ced6a7eb44ead921d25316d2e2.png)

LLM 调用外部工具的应用模式

有意思的是，OpenAI 的 Jack Rae 和 Ilya Sutskever 在之前的分享中也分别提到了 压缩即智慧 [7] 的理念。对于模型的“压缩率”来说，如果能更有效地使用这些“外部工具”，就能大幅提升很多特定任务 next token 预测的准确率。个人感觉这个方向的发展还有非常大的空间，例如从“有效数据”角度看，人类执行各类任务使用工具，甚至思维过程等数据会有非常高的价值。而从模型训练角度来看，如何能在过程中把模型利用工具的能力也体现在 loss function 里，可能也是个很有趣的方向。

![](img/49c954a30777db15a3371cacd4e28236.png)

提升“压缩率”的手段

## AutoGPT

有了前面的铺垫信息，我们来理解 `AutoGPT` 这类 AI agent 工作的内部结构与核心逻辑就会比较容易了。这类项目绝大多数的主要创新还是在 prompt 层面，通过更好的提示词来激发模型的能力，把更多原先需要通过代码来实现的流程“硬逻辑”转化为模型自动生成的“动态逻辑”。以 `AutoGPT` 为例，它的核心 prompt 如下：

```
You are Guandata-GPT, &#x27;an AI assistant designed to help data analysts do their daily work.&#x27;
Your decisions must always be made independently without seeking user assistance. Play to your strengths as an LLM and pursue simple strategies with no legal complications.

GOALS:

1. &#x27;Process data sets&#x27;
2. &#x27;Generate data reports and visualizations&#x27;
3. &#x27;Analyze reports to gain business insights&#x27;

Constraints:
1. ~4000 word limit for short term memory. Your short term memory is short, so immediately save important information to files.
2. If you are unsure how you previously did something or want to recall past events, can be parsed by Python json.loads
```

从这大段的 prompt 可以看出来，`AutoGPT` 的确算得上是提示词应用模式当前比较先进的“集大成者”了，有很多可以学习的地方。相比经典的 `reason + act` 模式，我们可以分别来看看它都做了哪些进一步的发展改进。

### Constraints & Resources

在这里告诉了模型你自己的各种局限性，也是很有喜感。例如模型的输入 context size 有限制，所以你需要把重要的信息保存到文件里。尤其在代码生成场景中这个动作非常重要，否则无法实现长代码的生成和执行。另外 AutoGPT 里也给模型提供了长期记忆的管理功能，当前这类复杂 prompt 生成的解决任务的流程往往比较冗长，没有这类长期记忆的管理很容易就会导致模型的输出变得不连贯协调。

另外像默认的模型是“没有联网”的，所有的知识只更新到训练数据的截止日期。所以也明确告诉模型可以通过网络搜索来获取更多时效性的外部信息。

### Commands

在 commands 也就是各类工具的选择上，这里给出的选项非常丰富。这也是为何很多文章宣传里提到 `AutoGPT` 能够完成多种不同任务的原因之一，灵活性与通用性很高。
