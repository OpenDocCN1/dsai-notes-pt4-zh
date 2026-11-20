
```
from langchain import PromptTemplate
template = "What is a good name for a company that makes {product}?"
prompt = PromptTemplate(
    input_variables=["product"],
    template=template,
)
prompt.format(product="colorful socks")
```

上面的提示可以看作是零样本问题设置，您希望 LLM 在足够的相关数据上进行训练以提供令人满意的响应。

另一个改进 LLM 输出的技巧是在提示中添加一些示例，并使其成为 few-shot problem setting 。

```
from langchain import PromptTemplate, FewShotPromptTemplate
examples = [
    {"word": "happy", "antonym": "sad"},
    {"word": "tall", "antonym": "short"},
]
example_template = """
Word: {word}
Antonym: {antonym}\n
"""
example_prompt = PromptTemplate(
    input_variables=["word", "antonym"],
    template=example_template,
)
few_shot_prompt = FewShotPromptTemplate(
    examples=examples,
    example_prompt=example_prompt,
    prefix="Give the antonym of every input",
    suffix="Word: {input}\nAntonym:",
    input_variables=["input"],
    example_separator="\n",
)
few_shot_prompt.format(input="big")
```

上面的代码将生成一个提示模板，并根据提供的示例和输入组成以下提示：

```
Give the antonym of every input
Word: happy
Antonym: sad


Word: tall
Antonym: short

Word: big
Antonym:
```

## 链(Chain)：将 LLM 与其他组件相结合

LangChain 中的 Chaining 简单描述了将 LLM 与其他组件相结合以创建应用程序的过程。一些例子是：

+   将 LLM 与提示模板相结合（参见本节）
+   通过将第一个 LLM 的输出作为第二个 LLM 的输入来顺序组合多个 LLM（请参阅本节）
+   将 LLM 与外部数据相结合，例如，用于问答（参见索引）
+   将 LLM 与长期记忆相结合，例如聊天记录（参见记忆）

在上一节中，我们创建了一个提示模板。当我们想将它与我们的 LLM 一起使用时，我们可以 `LLMChain` 按如下方式使用：

```
from langchain.chains import LLMChain
chain = LLMChain(llm = llm, 
                  prompt = prompt)
# Run the chain only specifying the input variable.
chain.run("colorful socks")
```

如果我们想使用第一个 LLM 的输出作为第二个 LLM 的输入，我们可以使用 `SimpleSequentialChain`：

```
from langchain.chains import LLMChain, SimpleSequentialChain
# Define the first chain as in the previous code example
# ...
# Create a second chain with a prompt template and an LLM
second_prompt = PromptTemplate(
    input_variables=["company_name"],
    template="Write a catchphrase for the following company: {company_name}",
)
chain_two = LLMChain(llm=llm, prompt=second_prompt)
# Combine the first and the second chain 
overall_chain = SimpleSequentialChain(chains=[chain, chain_two], verbose=True)
# Run the chain specifying only the input variable for the first chain.
catchphrase = overall_chain.run("colorful socks")
```

![](img/41a5d2e861c36d8bc193feb2f7e0e1d0.png)

## 索引：访问外部数据

[LLM](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/getting_started.html)的局限性之一是缺乏上下文信息（例如，无法访问某些特定文档或电子邮件）。您可以通过授予 LLM 访问特定外部数据的权限来解决这个问题。

为此，您首先需要使用文档加载器加载外部数据。LangChain 为不同类型的文档提供了多种加载器，从 PDF 和电子邮件到网站和 YouTube 视频。

让我们从 YouTube 视频加载一些外部数据。如果你想加载一个大文本文档并使用 Text Splitter 拆分它，你可以参考官方文档。

```
# pip install youtube-transcript-api 
# pip install pytube 

from langchain.document_loaders import YoutubeLoader 

loader = YoutubeLoader.from_youtube_url( "https://www.youtube.com/watch?v=dQw4w9WgXcQ" )
 
documents = loader.load()
```

现在您已经准备好外部数据作为 `documents`，您可以使用矢量数据库（[`VectorStore`](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%23bd03)）中的文本嵌入模型（请参阅 [模型](https://www.pinecone.io/)）对其进行索引。流行的矢量数据库包括 Pinecone、Weaviate 和 Milvus。在本文中，我们使用 Faiss，因为它不需要 API 密钥。

```
# pip install faiss-cpu 
from langchain.vectorstores import FAISS 

# 创建 vectorestore 用作索引
db = FAISS.from_documents(documents, embeddings)
```

您的文档（在本例中为视频）现在作为嵌入存储在矢量存储中。

现在您可以使用这些外部数据做很多事情。让我们将它用于带有信息检索器的问答任务：

```
from langchain.chains import RetrievalQA 

retriever = db.as_retriever() 

qa = RetrievalQA.from_chain_type( 
    llm=llm, 
    chain_type= "stuff" , 
    retriever=retriever, 
    return_source_documents= True ) 

query = "What am I never going to do?" 
result = qa({ "query" : query}) 

print (result[ 'result' ])
```

![](img/8048fd0b2165d654c617d7b22f7e3d8c.png)

## 记忆：记住以前的对话

对于像聊天机器人这样的应用程序，它们必须能够记住以前的对话。但默认情况下，LLM 没有任何长期记忆，除非你输入聊天记录。

![](img/7c74fa118d8f333f853e8ce4f46a3fc1.png)

LangChain 通过提供几种不同的选项来处理聊天记录来解决这个问题：

+   保留所有对话，
+   保留最近的 `k` 个对话，
+   总结谈话。

在此示例中，我们将使用 `ConversationChain` 为该应用程序提供对话记忆。

```
from langchain import ConversationChain

conversation = ConversationChain(llm=llm, verbose=True)

conversation.predict(input="Alice has a parrot.")

conversation.predict(input="Bob has two cats.")

conversation.predict(input="How many pets do Alice and Bob have?")
```

这将导致上图中右侧的对话。如果没有 `ConversationChain` 保留对话记忆，对话将看起来像上图中左侧的对话。

## 代理：访问其他工具

尽管 [非常强大](https://link.zhihu.com/?target=https%3A//openai.com/research/gpt-4)，LLM 也有一些局限性：它们缺乏上下文信息（例如，访问训练数据中未包含的特定知识），它们可能很快就会过时（例如，GPT-4 在 2021 年 9 月之前接受了数据训练），并且它们数学不好。

![](img/18ea86962edfcf5b6ca0a3566f0e7e68.png)

LLM不擅长数学。由于 LLM 可能会对自己无法完成的任务产生幻觉，因此我们需要让他们使用辅助工具，例如搜索（例如，[Google 搜索](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/agents/tools/examples/google_search.html)）、[计算器（例如，Python REPL 或 Wolfram Alpha）](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/agents/tools/examples/wolfram_alpha.html)）和查找（例如，[维基百科](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/agents/tools/examples/wikipedia.html)）。

此外，我们需要代理根据 LLM 的输出来决定使用哪些工具来完成任务。

下面是一个示例，其中代理首先使用维基百科查找 Barack Obama 的出生日期，然后使用计算器计算他在 2022 年的年龄。

```
# pip install wikipedia
from langchain.agents import load_tools
from langchain.agents import initialize_agent
from langchain.agents import AgentType
tools = load_tools(["wikipedia", "llm-math"], llm=llm)
agent = initialize_agent(tools, 
                         llm, 
                         agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, 
                         verbose=True)

agent.run("When was Barack Obama born? How old was he in 2022?")
```

![](img/d3161f91f562554b6ccfaa954e1fffba.png)

## 总结

就在几个月前，我们所有人（或至少我们中的大多数人）都对 ChatGPT 的功能印象深刻。现在，像 LangChain 这样的新开发人员工具使我们能够在几个小时内在我们的笔记本电脑上构建同样令人印象深刻的原型——这是一些真正令人兴奋的时刻！

LangChain 是一个开源 Python 库，任何可以编写代码的人都可以使用它来构建 LLM 支持的应用程序。该包为许多基础模型提供了通用接口，支持提示管理，并在撰写本文时充当其他组件（如提示模板、其他 LLM、外部数据和其他工具）的中央接口。

该库提供的功能比本文中提到的要多得多。以目前的发展速度，这篇文章也可能在一个月内过时。

主要内容翻译自：Getting Started with LangChain: A Beginner’s Guide to Building LLM-Powered Applications。翻译的时候有删改。

原文链接：[Getting Started with LangChain: A Beginner’s Guide to Building LLM-Powered Applications](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%23bd03)
