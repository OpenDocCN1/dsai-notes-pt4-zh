
# LLM 评估：运行所需的一切、基准 LLM 评估

> 来源：[https://agijuejin.feishu.cn/docx/GVSBd6JyUoTpDZxaqPcccXBonvD](https://agijuejin.feishu.cn/docx/GVSBd6JyUoTpDZxaqPcccXBonvD)

> 作者：北方的郎

> 原文：[https://zhuanlan.zhihu.com/p/691756684](https://zhuanlan.zhihu.com/p/691756684)

翻译自：“LLM Evaluation: Everything You Need To Run, Benchmark LLM Evals”

原文地址：[https://arize.com/blog-course/llm-evaluation-the-definitive-guide/](https://arize.com/blog-course/llm-evaluation-the-definitive-guide/)

![](img/191ea2169dc035645d31237ecd6e9d56.png)

## 为什么需要LLM评估？

大型语言模型 (LLM) 是开发人员和企业领导者为消费者创造新价值的不可思议的工具。他们提出个人建议，在结构化和非结构化数据之间进行转换，总结大量信息，等等。

随着应用程序的增加，衡量LLM应用程序性能的重要性也随之增加。需要LLM辅助评估，因为用户反馈或任何其他“事实来源”极其有限并且通常不存在；即使可能，人工标记仍然很昂贵；而且很容易使LLM应用变得复杂。

![](img/821513ae3a4ddc60ebbc1897d6d306fc.png)

幸运的是，我们可以利用LLM的力量来自动化评估。在本文中，我们将深入研究如何进行设置并确保其可靠。

![](img/ba942d74b30a11bf71986a71244575a0.png)

LLM评估的核心是AI评估AI，也称为AI辅助评估

虽然这听起来可能是循环的，但我们一直让人类智力评估人类智力（例如，在工作面试或大学期末考试中）。现在人工智能系统终于可以为其他人工智能系统做同样的事情了。

这里的过程是让LLM生成可用于评估另一个系统的综合基础事实。这就引出了一个问题：为什么不直接使用人类反馈？简而言之，因为你永远不会受够。

即使只有百分之一的输入/输出对获得人类反馈也是一项巨大的壮举。大多数团队甚至都没有意识到这一点。但为了使这个过程真正有用，对每个LLM子调用进行评估非常重要，我们已经看到这样的子调用可能有很多。

让我们探讨一下如何做到这一点。

### LLM模型评估和LLM系统评估（又名任务评估）有什么区别？

```
LLM_model_evals != LLM_System_evals
```

LLM 模型评估着眼于 LLM 在一系列任务中的整体宏观表现，LLM 系统评估（也称为 LLM 任务评估）更针对系统和用例，评估构建 LLM 应用程序的 AI 工程师可以控制的组件（即提示模板或上下文）。

[由于“LLM评估”一词](https://link.zhihu.com/?target=https%3A//arize.com/blog-course/large-language-model-evaluations-vs-llm-task-evaluations-in-llm-application-development/)可以互换使用，因此在实践中有时会丢失这种区别。 然而，了解其中的区别 至关重要。

### 什么是 LLM 模型评估 (Eval)？（What Are LLM Model Evaluations (Evals)?）

LLM 模型评估重点关注基础模型的整体性能。最初推出面向客户的LLM的公司需要一种方法来量化其在一系列不同任务中的有效性。

![](img/41d1686fe923f4ab855fff4f88e326a3.png)

在这种情况下，我们正在评估两种不同的开源基础模型。我们正在两个模型中测试相同的数据集，并查看它们的指标（例如 hellaswag 或 mmlu）如何叠加。 LLM模型评估指标

[OpenAI Eval](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/1905.07830) 库是一种具有 [LLM 模型评估](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2109.07958) 的流行库，它最初专注于[模型评估用例](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2009.03300)。有很多指标，例如 HellaSwag （评估 LLM 完成句子的能力）、 TruthfulQA （衡量模型响应的真实性）和 MMLU （衡量 LLM 执行多任务的能力）。甚至还有一个 [排行榜](https://link.zhihu.com/?target=https%3A//huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard) ，可以查看开源LLM之间的竞争情况。

![](img/e794ed90751a2ca45c51a4bd6fa721f6.png)

### 什么是LLM系统评估（Evals）？

目前为止，我们已经讨论了LLM模型评估。相比之下， LLM 系统评估 （有时也称为 LLM 任务评估）是对您系统中可以控制的组件的完整评估。这些组件中最重要的是提示（或提示模板）和上下文。LLM 系统评估评估您的输入如何确定您的输出。

例如，LLM 系统评估可以保持 LLM 不变并更改提示模板。由于提示是系统中更动态的部分，因此这种评估在项目的整个生命周期中都非常有意义。例如，LLM可以评估您的聊天机器人响应的有用性或礼貌性，并且相同的评估可以为您提供有关生产过程中性能随时间变化的信息。

![](img/fad97ababd36b4755beee3c9c0cc5a55.png)

在本例中，我们正在单个基础模型上评估两个不同的提示模板。我们正在跨两个模板测试相同的数据集，并查看它们的指标（如精确度和召回率）如何叠加。何时使用 LLM 系统评估与 LLM 模型评估：这取决于您的角色

使用LLM评估的人有不同的角色。一个是模型开发人员或负责微调核心 LLM 的工程师，另一个是组装面向用户的系统的从业者。

LLM 模型开发人员很少，他们往往在 OpenAI、Anthropic、Google、Meta 等地方工作。 模型开发人员关心 LLM 模型评估 ，因为他们的工作是提供满足各种用例的模型。

对于机器学习从业者来说，任务也从模型评估开始。开发 LLM 系统的第一步是选择模型（即 GPT 3.5、4、Palm 等）。然而，这一群体的LLM模型评估通常是一次性的。一旦解决了哪种模型在您的用例中表现最好的问题，应用程序生命周期的其余大部分将由 LLM 系统评估来定义。因此， ML 从业者关心 LLM 模型评估和 LLM 系统评估，但可能会在后者上花费更多时间。

## LLM系统评估指标（LLM System Evaluation Metrics）

使用其他机器学习系统后，您的第一个问题可能是：“[结果指标](https://link.zhihu.com/?target=https%3A//arize.com/blog-course/introduction-to-retrieval-augmented-generation/) 应该是什么？”答案取决于您想要评估的内容。

● 提取结构化信息 ：你可以看看LLM提取信息的效果如何。例如，您可以查看完整性（输入中是否存在输出中没有的信息？）。

● 问答 ：系统回答用户问题的程度如何？您可以查看答案的准确性、礼貌性或简洁性，或以上所有内容。

● 检索增强生成（ RAG ） ：检索到的文档和最终答案是否相关？

作为系统设计师，您最终对系统性能负责，因此您需要了解需要评估系统的哪些方面。例如，如果您有一个与儿童互动的LLM（例如辅导应用程序），您需要确保响应适合年龄且无毒。

### 顶级 LLM 系统评估指标是什么？（What Are the Top LLM System Evaluation Metrics?）

当今最常见的LLM评估指标是相关性、幻觉、问答准确性、毒性和特定于检索的指标。这些 LLM 系统评估中的每一项都会根据您要评估的内容而具有不同的模板。下面列出了 LLM 系统评估指标的完整列表。

| 类型 | 描述 | 指标示例 |

| --- | --- | --- |

| 多样性 | 检查基础模型在响应不同类型查询时的多功能性 | 流畅度、困惑度、ROUGE 分数 |

| 用户反馈 | 超越准确性，从连贯性和有用性方面考虑响应质量 | 连贯性、质量、相关性 |

| 基于地面实况的指标 | 将 RAG 系统的响应与一组预定义的正确答案进行比较 | 准确率、F1 分数、精确率、召回率 |

| 答案相关性 | LLM 的响应与给定用户的查询的相关程度如何。 | 二元分类（相关/不相关） |

| 质量检查正确性 | 根据检索到的数据，问题的答案是否正确？ | 二元分类（正确/错误） |

| 幻觉 | 看看关于检索上下文的LLM幻觉 | 二元分类（事实/幻觉） |

| 毒性 | 回应是否带有种族主义、偏见或有毒？ | 差异分析、公平评分、二元分类（无毒/有毒） |

### 哪些LLM评估指标对检索增强生成有用？（What LLM Evaluation Metrics are Useful for Retrieval Augmented Generation?）

[在评估LLM RAG](https://link.zhihu.com/?target=https%3A//arize.com/blog-course/llm-rag-retrieval-augmented-generation-roadmap/)评估的检索部分时，上下文相关性、忠实性和大海捞针测试可能非常有用。当与任务评估结合使用时，这些指标和测试有助于解决这些 LLM 系统的潜在复杂性。

**流行的LLM检索指标**

上下文相关性和忠实性是利用 LLM RAG 时评估检索到的文档文件的准确性和相关性的两个最广泛使用的指标。

● [上下文相关性](https://link.zhihu.com/?target=https%3A//arize.com/blog-course/monitoring-collaborative-filtering-recsys/%23metrics) 着眼于检索到的上下文与原始查询的相关性。这可以是相关/不相关的二元分类，也可以使用排名指标（即 MRR 、 Precision@K、 MAP 、 NDCG 等）

● 忠实性 或基础性着眼于基础模型的响应与检索到的上下文的一致程度。例如，这可以是忠诚或不忠诚的二元分类。

### 大海捞针测试：它是什么以及在哪里使用它（The Needle In a Haystack Test: What It Is and Where To Use It）

[大海捞针测试](https://link.zhihu.com/?target=https%3A//arize.com/blog-course/the-needle-in-a-haystack-test-evaluating-the-performance-of-llm-rag-systems/)最初由 Gregory Kamradt 创建，旨在帮助 RAG LLM 用例，是一种了解不同 LLM 检索隐藏在不同长度上下文中的信息的能力的方法。

虽然这是一项相对较新的技术，但该测试对于解决LLM人工智能工程师“忘记”信息或忽略提示中包含的部分上下文所面临的相对常见问题非常有用。它的功能是将特定的、定制的信息（“针”）放入更大、更复杂的文本主体（“大海捞针”）中。最终目标是评估LLM在海量数据中识别和利用特定花絮的能力。

**具体示例：评估上下文的相关性**

[为了简单起见，下面是一个使用](https://link.zhihu.com/?target=https%3A//phoenix.arize.com/)开源 Arize Phoenix 工具评估上下文相关性的示例。在 Phoenix 工具中，存在适用于大多数常见用例的默认模板。这是我们将在本示例中使用的：

```
Relevance Eval Template:
You are comparing a reference text to a question and trying to determine if the reference text contains information relevant to answering the question. Here is the data:
[BEGIN DATA]
************
[Question]: {query}
************
[Reference text]: {reference}
[END DATA]Compare the Question above to the Reference text. You must determine whether the Reference text
contains information that can answer the Question. Please focus on whether the very specific
question can be answered by the information in the Reference text.
Your response must be single word, either “relevant” or “irrelevant”,
and should not contain any text or characters aside from that word.
“irrelevant” means that the reference text does not contain an answer to the Question.
“relevant” means the reference text contains an answer to the Question.
```

首先，我们将导入所有必要的依赖项：

```
from phoenix.experimental.evals import (
   RAG_RELEVANCY_PROMPT_TEMPLATE_STR,
   RAG_RELEVANCY_PROMPT_RAILS_MAP,
   OpenAIModel,
   download_benchmark_dataset,
   llm_eval_binary,
)
from sklearn.metrics import precision_recall_fscore_support
```

现在，让我们引入数据集：

```
# Download a &quot;golden dataset&quot; built into Phoenix
benchmark_dataset = download_benchmark_dataset(
   task=&quot;binary-relevance-classification&quot;, dataset_name=&quot;wiki_qa-train&quot;
)
# For the sake of speed, we&#x27;ll just sample 100 examples in a repeatable way
benchmark_dataset = benchmark_dataset.sample(100, random_state=2023)
benchmark_dataset = benchmark_dataset.rename(
   columns={
       &quot;query_text&quot;: &quot;query&quot;,
       &quot;document_text&quot;: &quot;reference&quot;,
   },
)
# Match the label between our dataset and what the eval will generate
y_true = benchmark_dataset[&quot;relevant&quot;].map({True: &quot;relevant&quot;, False: &quot;irrelevant&quot;})
```

现在我们来进行一下评估：
