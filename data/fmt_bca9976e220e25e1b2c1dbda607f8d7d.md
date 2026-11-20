
# NL2SQL（Text-to-SQL）技术方案与企业应用优化

```
&gt; Entering new AgentExecutor chain...

Invoking: `search_proper_nouns` with `{&#x27;query&#x27;: &#x27;alis in chain&#x27;}`


Alice In Chains

Aisha Duo

Xis

Da Lama Ao Caos

A-Sides
Invoking: `sql_db_query` with `SELECT COUNT(*) FROM Album WHERE ArtistId = (SELECT ArtistId FROM Artist WHERE Name = &#x27;Alice In Chains&#x27;)`


[(1,)]Alice In Chains has 1 album.

&gt; Finished chain.
```

最终结果

```
{&#x27;input&#x27;: &#x27;How many albums does alis in chain have?&#x27;,
 &#x27;output&#x27;: &#x27;Alice In Chains has 1 album.&#x27;}
```

正如我们所看到的，该`agent`使用了`search_proper_nouns`工具来检查如何正确地查询指定艺术家的数据库。

## 难点与挑战

+ 当前AI模型输出`SQL`的准确性还远无法达到人类工程师的输出精度。

+ 自然语言表达本身的歧义性，而`SQL`是一种精确编程语言。因此在实际应用中，可能会出现无法理解，或者错误理解的情况。比如，“谁是这个月最厉害的销售”，那么AI是理解成订单数量最多，还是订单金额最大？

+ 尽管可以通过`Prompt`输入数据结构信息帮助AI模型来理解，但有时候AI可能会由于缺乏外部行业知识导致错误。比如，“分析去年的整体客户流失率？”，如果AI缺乏对“客户流失率”的理解，自然就会出错或者编造。

`NL2SQL`的方案在企业应用中还会面临两个严重的挑战：

+ 可能会出现正常运行的“假象”

即正常完成了任务，但实际结果是错误的。由于`NL2SQL`是直接输出用于数据库访问的语句，理论上只要不存在基本的语法错误，就可以执行成功，即使转换的`SQL`在语义上是错误的！

`NL2SQL`输出语义准确性衡量的复杂性本质上来自于这样一个事实：判断AI输出的一段代码是否正确，要比判断一个选择题答案是否正确，或者一段字符串的相似度要复杂的多。

评估`NL2SQL`模型输出正确性的复杂所在：你既不能用输出`SQL`的执行结果来判断，也不能简单的把输出`SQL`与标准答案对比来判断。

+ 企业应用的特点会加大错误输出的概率

## 优化方向

### 选择合适的大模型

`DB-GPT-Hub`

项目地址：[DB-GPT-Hub](https://link.zhihu.com/?target=https%3A//github.com/eosphoros-ai/DB-GPT-Hub/tree/main)

`DB-GPT-Hub`是一个利用`LLMs`实现`Text-to-SQL`解析的实验项目，主要包含数据集收集、数据预处理、模型选择与构建和微调权重等步骤，通过这一系列的处理可以在提高`Text-to-SQL`能力的同时降低模型训练成本，让更多的开发者参与到`Text-to-SQL`的准确度提升工作当中，最终实现基于数据库的自动问答能力，让用户可以通过自然语言描述完成复杂数据库的查询操作等工作。目前我们已经基于多个大模型打通从数据处理、模型`SFT`训练、预测输出和评估的整个流程，代码在本项目中均可以直接复用。

### `SQLCoder`

项目地址：[SQLCoder](https://link.zhihu.com/?target=https%3A//github.com/defog-ai/sqlcoder)

| `date` | `group_by` | `order_by` | `ratio` | `join` | `where` |  |

| --- | --- | --- | --- | --- | --- | --- |

| `sqlcoder-70b` | 96 | 91.4 | 97.1 | 85.7 | 97.1 | 91.4 |

| `sqlcoder-7b-2` | 96 | 91.4 | 94.3 | 91.4 | 94.3 | 77.1 |

| `sqlcoder-34b` | 80 | 94.3 | 85.7 | 77.1 | 85.7 | 80 |

| `gpt-4` | 72 | 94.3 | 97.1 | 80 | 91.4 | 80 |

| `gpt-4-turbo` | 76 | 91.4 | 91.4 | 62.8 | 88.6 | 77.1 |

| `natural-sql-7b` | 56 | 88.6 | 85.7 | 60 | 88.6 | 80 |

| `sqlcoder-7b` | 64 | 82.9 | 74.3 | 54.3 | 74.3 | 74.3 |

| `gpt-3.5` | 72 | 77.1 | 82.8 | 34.3 | 65.7 | 71.4 |

| `claude-2` | 52 | 71.4 | 74.3 | 57.1 | 65.7 | 62.9 |

从以上项目的测试结果看，`codeLlama-13b`与`SQLCoder-34b`是当前开源领域比较合适的`text2SQL`的模型选择。

### 提示工程

`DAIN-SQL`

项目地址：[DAIN-SQL](https://link.zhihu.com/?target=https%3A//github.com/BeachWang/DAIL-SQL)

阿里推出的一种针对`LLM`在`text2SQL`应用的提示工程优化技术。在Spider基准测试中取得当时的最高分86.6分，在更高难度的`BIRD`数据集测试中也取得了57.41分。其优化方法本质上是：在传统的`text2SQL`的提示中通过注入一些相似的样例，利用`LLM`的上下文学习能力，以提高输出`SQL`的精度。

原理是通过用户的提问“骨架”找到相似的问句；再通过预生成的`SQL`“骨架”找到相似的`SQL`，进而通过算法进行相似度排序，取出最相似的样例（包括提问与`SQL`），组装到`Prompt`。

![](img/f0bfe16e49d31b2f9b51dad0cac393b1.png)

`C3-SQL`

浙江大学推出一种针对`text2SQL`场景的提示工程优化方法，与`DAIL-SQL`不同的是，这是一种`zero-shot`的提示方法（零样例），即不在`prompt`增加样例，而是通过三个方面的提示或优化来提升输出的质量：

+ `Clear Prompting`：通过对`Prompt`提供更清晰的层次，并只嵌入必要的数据结构信息，来优化提示。一个重要的思想是，在构建`Prompt`之前，先通过大语言模型来分析本次输入最可能相关的数据实体(`table`)及其列(`column`)信息，即仅召回本次最可能用到的`table`和`column`，然后组装到`Prompt`，而不是把整个数据库的结构全部组装进入。

+ `Claibration Bias Prompting`：通过在上下文信息中嵌入一些偏差提示，可以简单的理解为指示大模型在一些场景下需要遵循的一些规则或者“注意点”。

+ `Consistent Output`: 这是一种输出处理方式，也是解决大模型输出不确定性的一种方案。可以解释为：让大模型输出多次`SQL`，然后根据输出的`SQL`执行结果进行“投票”。比如让`LLM`输出四次`SQL`，其中三个的执行结果都是一致的，另一个结果不一致，那么就认为这三次的输出是正确的。

![](img/d65bed692653bdfaf68899a1a15398cb.png)

## 开源方案

### `Data-Copilot`

项目地址：[Data-Copilot](https://link.zhihu.com/?target=https%3A//github.com/zwq2018/Data-Copilot)

论文地址：https://arxiv.org/abs/2306.07209

Demo地址：https://huggingface.co/spaces/zwq2018/Data-Copilot

`Data-Copilot`是一个基于`LLM`的系统，用于处理与数据相关的任务，连接了数十亿条数据和多样化的用户需求。它独立设计接口工具，以高效地管理、调用、处理和可视化数据。在接收到复杂请求时，`Data-Copilot`会自主调用这些自设计的接口，构建一个工作流程来满足用户的意图。在没有人类协助的情况下，它能够熟练地将来自不同来源、不同格式的原始数据转化为人性化的输出，如图形、表格和文本。

### `DeepBI`

项目地址：[DeepBI](https://link.zhihu.com/?target=https%3A//github.com/DeepInsight-AI/DeepBI?tab=readme-ov-file)

`DeepBI`是一款AI原生的数据分析平台。`DeepBI`充分利用大语言模型的能力来探索、查询、可视化和共享来自任何数据源的数据。用户可以使用`DeepBI`洞察数据并做出数据驱动的决策。

## 参考资料

大模型+`BI`的实现方式与金融领域落地案例

[构建Data Agent：探讨企业应用](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/zortAGyRivKq1iz6odsgSw)中基于大模型的交互式数据分析及方案【中】

`Agents | ️ `Langchain``

实现AI大模型+BI数据分析的5种路径，`Text2Sql`只是其中一种

`DAIL-SQL`: 发掘`LLM`的`NL2SQL`能力

`LLM`时代，探索式数据分析的升级之路有哪些新助攻？ - MSRA

[LLM时代，探索式数据分析的升级之路有哪些新助攻？ - MSRA](https://link.zhihu.com/?target=https%3A//mp.weixin.qq.com/s/9FK0VWlDKRMniuOzYKnkzA)
