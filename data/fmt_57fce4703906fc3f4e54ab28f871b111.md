
# RAGAS - RAG 自动评估，简单用一用

> 来源：[https://agijuejin.feishu.cn/docx/KaiHduD9zoCmR4xnvRRcUSYYnLh](https://agijuejin.feishu.cn/docx/KaiHduD9zoCmR4xnvRRcUSYYnLh)

> 作者：[王几行XING](https://www.zhihu.com/people/brycewang1898)

> 原文：[https://zhuanlan.zhihu.com/p/681671843](https://zhuanlan.zhihu.com/p/681671843)

## RAG 评估存在的问题

+   有许多传统的指标和基准测试用于评估问答系统，如`ROUGE`和`BLUE`，但它们与人类判断的相关性较差。

+   此在标准基准测试（如`Beir`）上的性能与生产数据之间的相关性，可能会因两者之间的分布偏移而变化。

+   而且，构建这样一个黄金测试集（参考的QA对）是一项昂贵且耗时的任务

## RAGAS 简介

`RAGAs`（检索增强生成评估框架）是一个用于评估检索增强生成（`RAG`）流程的 无需参考答案（`ground truth`） 的评估框架。

评估`RAG`架构具有挑战性，因为需要考虑多个维度：检索系统识别相关且集中的上下文段落的能力、`LLM`利用这些段落的忠实度，以及生成质量本身。

![](img/631f900a8053b3b69021c8a630f5e693.png)

### 评估的3个关键指标：

+   `faithfulness`，

+   `answer relevancy`

+   `context precision`

+   `context recall`（`LangChain` 中加入，基于 `ground truth`。原论文没有提及）

论文地址：[RAGAS: Automated Evaluation of Retrieval Augmented Generation](https://arxiv.org/abs/2309.15217)

LangChain 网址：[Evaluating RAG pipelines with Ragas + LangSmith](https://blog.langchain.dev/evaluating-rag-pipelines-with-ragas-langsmith/)

## 4大指标简介

`Ragas`是一个框架，它可以帮助你从不同的方面评估你的问答（QA）流程。它为你提供了一些指标来评估你的问答系统的不同方面，具体包括：

+   评估检索（`context`） 的指标：提供了上下文相关性（`context_relevancy`）和上下文召回率（`context_recall`），这些可以衡量你的检索系统的性能。

+   评估生成（`answer`） 的指标：提供了忠实度（`faithfulness`），用以衡量生成的信息是否准确无误；以及答案相关性（`answer_relevancy`），用以衡量答案对问题的切题程度。

特别地，正如前面也提到的，为了使用 `RAGAS`，你所需要的只是一些 `问题`。但是，如果你使用上下文召回率（`context_recall`），还需要一些 参考答案 。

## LangChain 中的评估案例

```
from langchain.document_loaders import WebBaseLoader
from langchain.indexes import VectorstoreIndexCreator
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatOpenAI

## 导入维基百科一个页面，并创建向量数据库的索引
# load the Wikipedia page and create index
loader = WebBaseLoader("https://en.wikipedia.org/wiki/New_York_City")
index = VectorstoreIndexCreator().from_loaders([loader])

## 创建 QA chain
# create the QA chain
llm = ChatOpenAI()
qa_chain = RetrievalQA.from_chain_type(
    llm, retriever=index.vectorstore.as_retriever(), return_source_documents=True
)

## 获取 RAG 回答
question = "How did New York City get its name?"
result = qa_chain({"query": question})
result["result"]

## 回答展示
# 'New York City got its name in 1664 when it was renamed after the Duke of York, who later became King James II of England. The city was originally called New Amsterdam by Dutch colonists and was renamed New York when it came under British control.'

## 基于 RAGAS 进行评估
from ragas.metrics import faithfulness, answer_relevancy, context_relevancy, context_recall
from ragas.langchain import RagasEvaluatorChain

# make eval chains
eval_chains = {
    m.name: RagasEvaluatorChain(metric=m) 
    for m in [faithfulness, answer_relevancy, context_relevancy, context_recall]
}

## 评估 RAG 的回答
for name, eval_chain in eval_chains.items():
    score_name = f"{name}_score"
    print(f"{score_name}: {eval_chain(result)[score_name]}")
```

评估得分结果：

+   `faithfulness_score`: 1.0

+   `answer_relevancy_score`: 0.9193459596511587

+   `context_relevancy_score`: 0.07480974380786602

+   `context_recall_score`: 0.9193459596511587

## 评估指标详细解释

### 指标一：回答的 `faithfulness`，评估答案的质量

论文中的“忠实度（`Faithfulness`）”定义为，如果答案中的声明可以从给定的上下文中推断出来，则认为答案对上下文是忠实的。为了估算忠实度，首先使用一个大型语言模型（`LLM`）提取出答案中的一系列声明 \(S(a_s(q))\) 。这一步骤旨在将较长的句子分解为更短、更集中的断言。然后，对于 `S` 中的每个声明 `s_i` ，使用验证函数 `v(s_i, c(q))` 来确定 `s_i` 是否可以从上下文 `c(q)` 中推断出来。最终的忠实度得分 `F` 计算为:

\(F = \frac{|V|}{|S|}\)

其中:

+   问题 + `RAG` 生成的的答案 + `GPT 3.5`，生成一系列的声明 （statements）

+   `V` 是根据 `GPT 3.5` 判断为被支持的声明数，

+   `S` 是声明总数

下面是生成 statements 的 prompt：

![](img/a43bc34a50cf2aa134b0f55171177a64.png)

以及，`GPT 3.5` 判断 statements 是否被支持的 prompt：

![](img/24d978bf8d97507c97ab9698194475f6.png)

### 指标二：`AR`，答案相关性，评估答案的质量

答案相关性（`Answer Relevance`）的数学原理基于生成 答案与初始问题 的紧密对齐程度的评估。具体来说，该指标通过以下步骤计算得出：

1.  对给定答案， 提示 大型语言模型（`LLM`）生成基于该答案可能的 n 个问题 `qi` 。

1.  使用`OpenAI API`提供的`text-embedding-ada-002`模型获取所有问题的嵌入表示。

1.  对于每个生成的问题 `qi` ， 计算它与原始问题 `q` 之间的相似度 `sim(q, qi)` 。这里的相似度是通过计算对应嵌入之间的余弦相似度来得到的。

1.  答案相关性得分（平均相似度） `AR` 通过以下公式计算：

\(AR = \frac{1}{n} \sum_{i=1}^{n} sim(q, q_i)\)

可能问题生成的 prompt 如下：

![](img/bd4fd7a8a7bb156209aeeee9e158277b.png)

### 指标三：`Context Relevance`，评估 `context` 的质量

这里的 `context`，指的是 `RAG` 检索得到的 `context`。

上下文相关性（`Context Relevance`）的数学原理是衡量检索到的上下文与问题答案所需信息的紧密度。特别是，这个指标旨在惩罚包含冗余信息的情况。为了估计给定问题 `q` 及其上下文 `c(q)` 的上下文相关性，`LLM`会 从 `c(q)` 中提取出对回答问题 `q` 至关重要的句子子集 \(S_{ext}\) 。

上下文相关性得分 `CR` 则通过下面的公式计算：

\(CR = \frac{\text{提取出的句子数量}}{\text{c(q)中的总句子数量}}\)

这个比率越高，表示检索到的上下文与问题的相关性越强，意味着提取的信息在解答问题时更为精准和高效，减少了不必要的信息干扰。

用 `GPT 3.5` 抽取句子的提示词如下：

![](img/070a14e9b042939dc72f2801c3f81341.png)

### `LangChain` 添加的指标4：`Context Recall`

`Recall` 的构建，需要用到自己准备的参考答案。

`Recall`：衡量检索器，检索 回答问题所需所有必要信息 的能力。（意思是回答这个需要这么多信息，`RAG` 检索得到的 `context` 能提供多少比例？）

`计算原理：` `Ragas` 通过使用提供的 真实答案 ，并利用 `GPT 3.5` 检查这个 真实答案 的 每个声明 ，是否能在检索到的 `上下文` 中找到。
