
# RAG：LLMs + LangChain app 开发关键技术整理

> 来源：[https://agijuejin.feishu.cn/docx/QrvpdYmZkovRzpxtfaPcB8rXn3K](https://agijuejin.feishu.cn/docx/QrvpdYmZkovRzpxtfaPcB8rXn3K)

> 作者：王几行XING

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/664097125)hihu.com/p/664097...

## 第零部分：文献综述

### RAG 理论介绍

+   RAG 理论论文1： [https:// arxiv.org/p](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/1906.00300.pdf)df/1906.0030 0.pdf  , Lee, K., Chang, M. W., & Toutanova, K. (2019). Latent retrieval for weakly supervised open domain question answering. arXiv preprint arXiv:1906.00300 .

+   RAG 理论论文2： [ht](https://link.zhihu.com/?target=https%3A//proceedings.neurips.cc/paper/2020/file/6b493230205f780e1bc26945df7481e5-Paper.pdf)tps:// proceedings.neurips.cc/ paper/2020/file/6b493230205f780e1bc26945df7481e5-Paper.pdf  , Lewis, P., Perez, E., Piktus, A., Petroni, F., Karpukhin, V., Goyal, N., ... & Kiela, D. (2020). Retrieval-augmented generation for knowledge-intensive nlp tasks. Advances in Neural Information Processing Systems , 33 , 9459-9474.

+   RAG 理论论文3： [h](https://link.zhihu.com/?target=http%3A//proceedings.mlr.press/v119/guu20a/guu20a.pdf)ttp:// proceedings.mlr.press/v 119/guu20a/guu20a.pdf  , Guu, K., Lee, K., Tung, Z., Pasupat, P., & Chang, M. (2020, November). Retrieval augmented language model pre-training. International conference on machine learning (pp. 3929-3938). PMLR.

### 关键案例论文

+   案例论文 1： Chen, 2023, 基于购买记录的个性化产品推荐

+   案例论文 2： Ciuca and Ting, 2023，天文知识的问答机器人

+   案例论文 3： Semnani et al., 2023，Wiki 问题回答机器人

+   案例论文 4: Siriwardhana et al., 2023，基于维基百科的知识问答机器人

+   案例论文 5: Vaghef et al., 2023，关于气候变化的知识问答

+   案例论文 6: Yue & Au, 2023，金融数据回测和问答

## 1 文本处理任务

### 1.1 长文本摘要

+   LangChain 官方说明： Summarization | ️ Langchain

+   关键函数： `load_summarize_chain`

+   选项一：`map reduce`，每一块先总结，然后拼在一起，最后总结

+   选项二：`refine`，每小结一次，加上新的文件块再次总结，相当于累计信息的做法

+   [具体可以参考（和官网说明类似）： 李祺 Varman：Azur](https://zhuanlan.zhihu.com/p/646778848)e OpenAI 入门教程 - LangChain 篇 ： 长文本内容总结

+   主要步骤：

```
- 预处理文本:先对原始长文本进行预处理,包括清理格式、分割句子、去除停用词等,得到适合输入到LangChain的纯文本。
- 载入文本，UnstructuredFileLoader
- 设置prompt:根据摘要生成的需求,设计合适的prompt,提示LangChain需要生成什么样的摘要。prompt可以指明想要生成的摘要长度、风格等特征。
- 输入文本:将预处理后的长文本输入到LangChain中。
- 生成摘要:让LangChain根据prompt去生成针对该长文本的摘要。可以生成多轮摘要,adj问不同prompt,生成不同角度的摘要。
- 后处理:获取LangChain生成的摘要后,可以进行后处理,如修正语法、重复内容等,进一步提升摘要质量。
- 评估:可以通过一定的自动评估指标或人工评估来判断生成摘要的质量,从而反馈优化prompt设计。
- 迭代优化:重复上述步骤,调整prompt,迭代生成,直到得到满意的摘要。
```

+   长文本摘要生成案例： [王几行XING：LangChain+ChatGPT 长文本摘要生成（附完整代码）](https://zhuanlan.zhihu.com/p/663903128)

```
!pip install langchain -q
!pip install openai -q
!pip install unstructured -q

from langchain.document_loaders import UnstructuredFileLoader
from langchain.chains.summarize import load_summarize_chain
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain import OpenAI
openai_api_key = "sk-xxx"

## Load the long test
loader = UnstructuredFileLoader("/content/test.txt")

## Transform the text to document
document = loader.load()
print(f'The number of documents before cutting: {len(document)}')

## Initial the text_splitter
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size = 500,
    chunk_overlap = 0
)

## Cut the documents
split_documents = text_splitter.split_documents(document)
print(f'The number of documents after cutting: {len(split_documents)}')

## Initial the LLM
llm = OpenAI(model_name="text-davinci-003", max_tokens=1500,
             openai_api_key=openai_api_key)

## Create the summerize chain 
chain = load_summarize_chain(llm, chain_type="refine", verbose=True)

## Sum the first 10 documents
chain.run(split_documents[:10])
```

## 2 LangChain 关键技术

### 2.1 Prompt Template

### 2.2 Chain

+   RAG对话Chain： `qa = ConversationalRetrievalChain.from_llm(llm, retriever=docsearch.as_retriever(), memory=memory)`

+   这里`retriever` 来自 Embedding： `docsearch = FAISS.from_documents(text_chunks, embeddings)`

+   启用了 `memory`，可以实现多轮对话

### 2.3 Memory

+   `ConversationBufferMemory` 启用聊天记录历史，`memory = ConversationBufferMemory(memory_key="chat_history", return_messages=True)`

+   `memory.chat_memory.add_user_message("Hi")`，添加用户的发言记录

+   `memory.chat_memory.add_ai_message("Hi")`，添加 AI 的发言记录

+   `memory.load_memory_variables({})`，返回所有的对话记录

+   `ConversationBufferWindowMemory(k=3)`，滑动窗口查看 `memory`

### 2.4 Agent（生产环境中变数太多，慎重使用）

+   ChatBot: Conversational Retrieval Agent

+   SQL: `create_sql_agent`

+   pandas: `create_pandas_dataframe_agent`

## 3 SQL/cvs等表格数据

### 3.1 数据导入

+   LangChain 的 `CSV Loader`

+   将每行数据导入成为一个个 `document`

+   类似于 JSON 的键值对格式

+   文件来源和行号，作为每个 `document` 的 `metadata`

### 3.2 SQL RAG

+   SQL Agent

+   [SQL Database Chain： 王几行XING：惊艳的LangChain：SQL自动生成、执行、完成回答！（附完整代码）](https://zhuanlan.zhihu.com/p/665222497)

## 4 RAG 性能评估

### 4.1 ragas，无参考答案的评估

+   论文地址： [https:// arxiv.org/p](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2309.15217.pdf)df/2309.1521 7.pdf  ，《RAGAS: Automated Evaluation of Retrieval Augmented Generation》

+   GitHub： [Install | Ragas](https://link.zhihu.com/?target=https%3A//docs.ragas.io/en/latest/getstarted/install.html)

+   论文解释： [LinguaMind：再看大模型RAG检索增强如何评估：RAGAS开源自动化评估框架](https://zhuanlan.zhihu.com/p/664052784)

+   三个维度的评估 (Es et al., 2023)，讨论的是 context 、 answer 的质量，及两者的相关程度：

+   `Faithfulness`，答案应该基于语境， the answer should be grounded in the given context（answer 和 context 的相关程度）

+   Answer Relevance ，答案的相关性，即答案和问题应该对应。具体操作是惩罚答案的不完整和信息冗余

+   Context Relevance ，检索返回的 context 必须保持专注，不应该提供无关的信息。具体操作是惩罚信息的冗余

+   三个指标构建之后，利用 ChatGPT 生成 context 和 answer 数据，进行RAGAS判断，并与人工判断进行对比。

+   RAGAS 算是基于 LLMs 自动评估的一种方法，本质上也是在没有参考答案的情况下，依赖大模型自动完成 RAG 的评估。

![](img/6c3e8aa8f9eec6590ba181ff1524a820.png)

### 4.2 LangChain 的 auto-evaluator：利用大模型对答案进行评估

+   LangChain 自带： [Auto-Evaluator](https://link.zhihu.com/?target=https%3A//autoevaluator.langchain.com/)

![](img/f90d7c390fc74dfbf821e544e9c7a976.png)

### 4.3 [LangChain 官网的评估中心](https://link.zhihu.com/?target=https%3A//js.langchain.com/docs/api/evaluation/)：Evaluation | ️ Langchain ； `langchain/evaluation APIs` | ️ Langchain

+   基于参考答案的评估， `QAEvalChain` ，将得到的回答和真实答案进行对比

+   利用 ChatGPT 来评估其它 LLMs

+   官方案例： [h](https://link.zhihu.com/?target=https%3A//blog.langchain.dev/usin g-chatgpt-api-to-evaluate-chatgpt/)ttps:// blog.langchain.dev/usin g-chatgpt-api-to-evaluate-chatgpt/

![](img/204b4ddd02b6deb5ed2dbfb3c3ccd2df.png)

+   `PairwiseStringEvalChain` ，`PairwiseStringEvaluator` 的一种方法。利用某个大模型（比如 GPT-4 ），比较两个不同 LLMs 输出的结果。评估具体需要的三个部件：

1.  An evaluator，一个评估器，相当于裁判，比如 GPT-4

2.  A dataset of inputs，一系列的输入

3.  (2 or more) LLMs, Chains, or Agents to compare - 两个或者两个以上的被评估对象

4.  结果还可以计算 置信区间 和 p 值（这个真的挺特别！）

