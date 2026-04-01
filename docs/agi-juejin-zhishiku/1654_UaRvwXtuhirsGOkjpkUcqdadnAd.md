# LangChain 入门：构建 LLM 支持的应用程序的初学者指南

> 来源：[https://agijuejin.feishu.cn/docx/WD2NdYF2jo801cxh0mMcgqffnAb](https://agijuejin.feishu.cn/docx/WD2NdYF2jo801cxh0mMcgqffnAb)

> 作者：北方的郎

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/631948940)hihu.com/p/631948...

自 ChatGPT 发布以来，大型语言模型 (LLM) 广受欢迎。尽管您可能没有足够的资金和计算资源从头开始训练 LLM，但您仍然可以使用预训练的 LLM 来构建一些很酷的东西，例如：

+   可以根据您的数据与外界互动的 [个人助理](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/use_cases/personal_assistants.html)

+   为您的目的定制的 [聊天机器人](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/use_cases/chatbots.html)

+   [分析 ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/use_cases/question_answering.html)或 [总结 ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/use_cases/summarization.html)您的文档或 [代码](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/use_cases/code.html)

LLM正在改变我们构建人工智能产品的方式

凭借其怪异的 API 和及[时的工程设计，LLM](https://link.zhihu.com/?target=https%3A//github.com/hwchase17/langchain)[ ](https://link.zhihu.com/?target=https%3A//github.com/hwchase17/langchain)正在改变我们构建 AI 驱动产品的方式。 这就是为什么新的开发人员工具以“LLMOps” 一词出现在各处的原因。

这些新工具之一是 LangChain 。

GitHub - hwchase17/langchain: ⚡ 通过可组合性使用 LLM 构建应用程序 ⚡

## LangChain是什么？

LangChain 是一个框架，旨在通过为您提供以下功能来帮助您更轻松地构建 LLM 支持的应用程序：

+   [各种不](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%23bd03)同基础模型的通用接口（参见 模型 ），

+   帮助您管理提示的框架（请参阅 [提示 ](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%23b1a5)），以及

+   长期记忆（参见 [Memory ](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%233bc6)）、外部数据（参见 [Indexes ](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%23806f)）、其他 LLM（参见 [Chains ](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%23fcac)[）以及 LLM](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%23b842) 无法处理的任务的其他代理（例如，计算或搜索）（参见 Agents ）的中央接口.

它是由Harris[on Chase ](https://link.zhihu.com/?target=https%3A//github.com/hwchase17/langchain)创建的开源项目（ GitHub 库 ）。

因为 LangChain 有很多不同的功能，所以一开始可能很难理解它的作用。这就是为什么我们将在本文中介绍 LangChain（当前）的六个关键模块，以便您更好地了解其功能。

##  先决条件

要继续学习本教程，您需要 langchain 安装 Python 包并准备好使用所有相关的 API 密钥。

##  安装LangChain

在安装 langchain 软件包之前，请确保您的 Python 版本≥ 3.8.[1 且 <4.0。

要](https://link.zhihu.com/?target=https%3A//github.com/hwchase17/langchain)安装 langchain Python包，你可以 pip 安装它。

pip install langchain

在本教程中，我们使用版本 0.0.147。 GitHub 存储库 非常活跃；因此，请确保您拥有最新版本。

完成所有设置后，导入 langchain Python 包。

import LangChain

##  API密钥

**

使用 LLM 构建应用**程序需要您要使用的某些服务的 API 密钥，[并且某些 API 会产生相关费](https://link.zhihu.com/?target=https%3A//hazyresearch.stanford.edu/blog/2023-01-30-ai-linux)用。

LLM 提供者（必需） ——您首先需要一个 API 密钥，用于您要使用的 LLM 提供者。我们目前正在经历 “AI 的 Linux 时刻 ”，开发人员必须主要根据性能和成本之间的权衡， 在专有或开源基础模型之间做出选择。

![](img/0f85159fdee78447ddeb25b42bc60b81.png)

**专有模型 **是拥有大量专家团队和大量 AI 预算的公司所拥有的闭源基础模型。它们通常比开[源模型更大，因](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/models/llms/integrations/openai.html)此性[能更好，但它们的](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/models/llms/integrations/cohere.html) A[PI 也很昂贵。专有](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/models/llms/integrations/ai21.html)模型[提供商的示例包括 O](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/models/llms/integrations/anthropic_example.html)penAI 、 co:here 、 AI[21 Labs 或](https://link.zhihu.com/?target=https%3A//platform.openai.com/account/api-keys) Anthropic 。

大多数可用的 LangChain 教程都使用 OpenAI，但请注意 OpenAI API（实验并不昂贵，但它）不是免费的 。要获取 OpenAI API Key，您需要一个 OpenAI 帐户，然后在 API keys 下“Create new secret key” 。

```
import os 
os.environ[ &quot;OPENAI_API_KEY&quot; ] = ... # 在这里插入你的 API_TOKEN
```

**

开源模型** 通常是比专有模型功能更小的模型，但它们比专有模型更具成本效益。开源模型的例子是：

+   BigScience 的 [BLOOM](https://link.zhihu.com/?target=https%3A//huggingface.co/bigscience/bloom)

+   Meta AI 的 [LLaMA](https://link.zhihu.com/?target=https%3A//huggingface.co/docs/transformers/main/en/model_doc/llama)

+   谷歌的 [Flan-T5](https://link.zhihu.com/?target=https%3A//huggingface.co/google/flan-t5-xl)

+   Eleuther AI 的 [GPT-J](https://link.zhihu.com/?target=https%3A//huggingface.co/EleutherAI/gpt-j-6b)

许多开源模型都在作为社区中心的 Hugging Face[上组织和托管。 要获取 Hu](https://link.zhihu.com/?target=https%3A//huggingface.co/settings/tokens)gging Face API 密钥，您需要一个 Hugging Face 帐户并在 Access Tokens 下创建一个“New token” 。

import os

os.environ[ "HUGGINGFACEHUB_API_TOKEN" ] = ... # 在这里插入你的 API_TOKEN

您可以免费为开源 LLM使用 Hugging Face ，但您将仅限于性能较低的小型 LLM。

[矢量数据库(Vec](https://link.zhihu.com/?target=https%3A//www.pinecone.io/)to[r Databas](https://link.zhihu.com/?target=https%3A//weaviate.io/)e)[（可选） ——](https://link.zhihu.com/?target=https%3A//milvus.io/)如果你想使用特定的矢量数据库，如 Pinecon[e 、 Wea](https://link.zhihu.com/?target=https%3A//engineering.fb.com/2017/03/29/data-infrastructure/faiss-a-library-for-efficient-similarity-search/)viate 或 Milvus ，你需要向他们注册以获得 API 密钥并查看他们的定价。在本教程中，我们使用无需注册的 Faiss 。

**

工具（可选）**[ ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/agents/tools.html)——根据您希望 LLM 与之交互的 工具，例如 OpenWeatherMap 或 SerpAPI，您可能需要向它们注册以获取 API 密钥并检查它们的定价。 在本教程中，我们只使用不需要 API 密钥的工具。

你能用 LangChain 做什么？

该包为许多基础模型提供了一个通用接口，支持提示管理，并通过代理充当其他组件（如提示模板、其他 LLM、外部数据和其他工具）的中央接口。

在撰写本文时，LangChain（版本 0.0.147）涵盖六个模块：

+   模型(Models)：从不同的 LLM 和嵌入模型中进行选择

+   提示(Prompts)：管理 LLM 输入

+   链(Chains)：将 LLM 与其他组件相结合

+   索引(Indexs)：访问外部数据

+   记忆(Memory)：记住以前的对话

+   代理(Agents)：访问其他工具

以下部分的代码示例是从 [LangChain 文档 ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/index.html)中复制和修改的。

模型：从不同的 LLM 和嵌入模型中进行选择

目前，许多不同的LLM正在涌现。LangChain 提供与各种模型的集成，并为所有模型提供简化的界面。

LangChain 区分三种输入和输出不同的模型：

+   **LLM **将一个字符串作为输入（提示）并输出一个字符串（完成）。

```
# Proprietary LLM from e.g. OpenAI
# pip install openai
from langchain.llms import OpenAI
llm = OpenAI(model_name=&quot;text-davinci-003&quot;)
# Alternatively, open-source LLM hosted on Hugging Face
# pip install huggingface_hub
from langchain import HuggingFaceHub
llm = HuggingFaceHub(repo_id = &quot;google/flan-t5-xl&quot;)
# The LLM takes a prompt as an input and outputs a completion
prompt = &quot;Alice has a parrot. What animal is Alice&#x27;s pet?&quot;
completion = llm(prompt)
```

![](img/f1fd779d533489f719e62ab71766b8d6.png)

+   **聊天模型 **类似于 LLM。他们将聊天消息列表作为输入并返回一条聊天消息。

+   **文本嵌入模型 **接受文本输入并返回浮点数（嵌入）列表，这是输入文本的数字表示。嵌入有助于从文本中提取信息。然后可以稍后使用该信息，例如，用于计算文本（例如，电影摘要）之间的相似性。

```
# Proprietary text embedding model from e.g. OpenAI
# pip install tiktoken
from langchain.embeddings import OpenAIEmbeddings
embeddings = OpenAIEmbeddings()
# Alternatively, open-source text embedding model hosted on Hugging Face
# pip install sentence_transformers
from langchain.embeddings import HuggingFaceEmbeddings
embeddings = HuggingFaceEmbeddings(model_name = &quot;sentence-transformers/all-MiniLM-L6-v2&quot;)
# The embeddings model takes a text as an input and outputs a list of floats
text = &quot;Alice has a parrot. What animal is Alice&#x27;s pet?&quot;
text_embedding = embeddings.embed_query(text)
```

![](img/4cbf130750ab42e036d77c1e3f990afe.png)

## 提示：管理 LLM 输入

LLM 有奇怪的 API。虽然用自然语言向 LLM 输入提示应该很直观，但在从 LLM 获得所需的输出之前需要对提示进行相当多的调整。这个过程称为 prompt 工程 。( prompt engineering .)

一旦你有了一个好的提示，你可能想将它用作其他目的的模板。因此，LangChain 为您提供了所谓的 PromptTemplates ，它可以帮助您从多个组件构建提示。

```
from langchain import PromptTemplate
template = &quot;What is a good name for a company that makes {product}?&quot;
prompt = PromptTemplate(
    input_variables=[&quot;product&quot;],
    template=template,
)
prompt.format(product=&quot;colorful socks&quot;)
```

上面的提示可以看作是** 零样本问题设置** ，您希望 LLM 在足够的相关数据上进行训练以提供令人满意的响应。

另一个改进 LLM 输出的技巧是在提示中添加一些示例，并使其成为 few-shot problem setting 。

```
from langchain import PromptTemplate, FewShotPromptTemplate
examples = [
    {&quot;word&quot;: &quot;happy&quot;, &quot;antonym&quot;: &quot;sad&quot;},
    {&quot;word&quot;: &quot;tall&quot;, &quot;antonym&quot;: &quot;short&quot;},
]
example_template = &quot;&quot;&quot;
Word: {word}
Antonym: {antonym}\n
&quot;&quot;&quot;
example_prompt = PromptTemplate(
    input_variables=[&quot;word&quot;, &quot;antonym&quot;],
    template=example_template,
)
few_shot_prompt = FewShotPromptTemplate(
    examples=examples,
    example_prompt=example_prompt,
    prefix=&quot;Give the antonym of every input&quot;,
    suffix=&quot;Word: {input}\nAntonym:&quot;,
    input_variables=[&quot;input&quot;],
    example_separator=&quot;\n&quot;,
)
few_shot_prompt.format(input=&quot;big&quot;)
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

##  链(Chain)：将 LLM 与其他组件相结合

LangChain 中的 Chaining 简单描述了将 LLM 与其他组件相结合以创建应用程序的过程。一些例子是：

+   将 LLM 与提示模板相结合（参见本节）

+   通过将第一个 LLM 的输出作为第二个 LLM 的输入来顺序组合多个 LLM（请参阅本节）

+   将 LLM 与外部数据相结合，例如，用于问答（参见索引）

+   将 LLM 与长期记忆相结合，例如聊天记录（参见记忆）

在上一节中，我们创建了一个提示模板。当我们想将它与我们的 LLM 一起使用时，我们可以 LLMChain 按如下方式使用：

```
from langchain.chains import LLMChain
chain = LLMChain(llm = llm, 
                  prompt = prompt)
# Run the chain only specifying the input variable.
chain.run(&quot;colorful socks&quot;)

如果我们想使用第一个 LLM 的输出作为第二个 LLM 的输入，我们可以使用SimpleSequentialChain：


from langchain.chains import LLMChain, SimpleSequentialChain
# Define the first chain as in the previous code example
# ...
# Create a second chain with a prompt template and an LLM
second_prompt = PromptTemplate(
    input_variables=[&quot;company_name&quot;],
    template=&quot;Write a catchphrase for the following company: {company_name}&quot;,
)
chain_two = LLMChain(llm=llm, prompt=second_prompt)
# Combine the first and the second chain 
overall_chain = SimpleSequentialChain(chains=[chain, chain_two], verbose=True)
# Run the chain specifying only the input variable for the first chain.
catchphrase = overall_chain.run(&quot;colorful socks&quot;)
```

![](img/41a5d2e861c36d8bc193feb2f7e0e1d0.png)

## 索引：访问外部数据

[

LLM ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/getting_started.html)的局限性之一是缺乏上下文信息（例如，无法访问某些特定文档或电子邮件）。您可以通过授予 LLM 访问特定外部数据的权限来解决这个问题。

为此，您首先需要使用文档加载器加载外部数据。LangChain 为不同类型的文档提供了多种加载器，从 PDF 和电子邮件到网站和 YouTube 视频。

让我们从 YouTube 视频加载一些外部数据。 如果你想加载一个大文本文档并使用Text Splitter 拆分它， 你可以参考官方文档。

```
# pip install youtube-transcript-api 
# pip install pytube 

from langchain.document_loaders import YoutubeLoader 

loader = YoutubeLoader.from_youtube_url( &quot;https://www.youtube.com/watch?v=dQw4w9WgXcQ&quot; )
 
documents = loader.load()
```

现在您已经准备好外部数据作为 document**s ，您可以使**用矢量数据[库（VectorSt](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%23bd03)**or**e）中的 文本嵌入模型[ （请参阅 模型）](https://link.zhihu.com/?target=https%3A//www.pinecone.io/)对其[进行索引 。 流行](https://link.zhihu.com/?target=https%3A//weaviate.io/)的矢[量数据库包括 ](https://link.zhihu.com/?target=https%3A//milvus.io/)Pinecone 、 Weaviate 和 Milvus 。在本文中，我们使用 Faiss，因为它不需要 API 密钥。

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
    chain_type= &quot;stuff&quot; , 
    retriever=retriever, 
    return_source_documents= True ) 

query = &quot;What am I never going to do?&quot; 
result = qa({ &quot;query&quot; : query}) 

print (result[ &#x27;result&#x27; ])
```

![](img/8048fd0b2165d654c617d7b22f7e3d8c.png)

记忆：记住以前的对话

对于像聊天机器人这样的应用程序，它们必须能够记住以前的对话。但默认情况下，LLM 没有任何长期记忆，除非你输入聊天记录。

![](img/7c74fa118d8f333f853e8ce4f46a3fc1.png)

LangChain 通过提供几种不同的选项来处理聊天记录来解决这个问题：

+   保留所有对话，

+   保留最近的 k 个对话，

+   总结谈话。

在此示例中，我们将使用 a ConversationChain 为该应用程序提供对话记忆。

```
from langchain import ConversationChain

conversation = ConversationChain(llm=llm, verbose=True)

conversation.predict(input=&quot;Alice has a parrot.&quot;)

conversation.predict(input=&quot;Bob has two cats.&quot;)

conversation.predict(input=&quot;How many pets do Alice and Bob have?&quot;)
```

这将导致上图中右侧的对话。如果没有 ConversationChain 保留对话记忆，对话将看起来像上图中左侧的对话。

##  代理：访问其他工具

尽[管非常](https://link.zhihu.com/?target=https%3A//openai.com/research/gpt-4)强大，LLM 也有一些局限性：它们缺乏上下文信息（例如，访问训练数据中未包含的特定知识），它们可能很快就会过时（例如，GPT-4 在 2021 年 9 月之前接受了数据训练）， 并且 它们数学不好。

![](img/18ea86962edfcf5b6ca0a3566f0e7e68.png)

LLM不擅长数学由于 LLM 可能会对自己无法完成**的任务**产生幻觉，因此我们需[要让他们使用辅助 工](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/agents/tools/examples/google_search.html)具 ，例如搜索（例如[， Google 搜索 ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/agents/tools/examples/python.html)）、[计算器（例如， Python](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/agents/tools/examples/wolfram_alpha.html) REPL 或 W[olfra](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/agents/tools/examples/wikipedia.html)m Alpha ）和查找（例如， 维基百科 ）。

此外，我们需要代理根据 LLM 的输出来决定使用哪些工具来完成任务。

```
下面是一个示例，其中代理首先使用维基百科查找 Barack Obama 的出生日期，然后使用计算器计算他在 2022 年的年龄。
 # pip install wikipedia
from langchain.agents import load_tools
from langchain.agents import initialize_agent
from langchain.agents import AgentType
tools = load_tools([&quot;wikipedia&quot;, &quot;llm-math&quot;], llm=llm)
agent = initialize_agent(tools, 
                         llm, 
                         agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION, 
                         verbose=True)

agent.run(&quot;When was Barack Obama born? How old was he in 2022?&quot;)
```

![](img/d3161f91f562554b6ccfaa954e1fffba.png)

## 总结

就在几个月前，我们所有人（或至少我们中的大多数人）都对 ChatGPT 的功能印象深刻。现在，像 LangChain 这样的新开发人员工具使我们能够在几个小时内在我们的笔记本电脑上构建同样令人印象深刻的原型——这是一些真正令人兴奋的时刻！

LangChain 是一个开源 Python 库，任何可以编写代码的人都可以使用它来构建 LLM 支持的应用程序。该包为许多基础模型提供了通用接口，支持提示管理，并在撰写本文时充当其他组件（如提示模板、其他 LLM、外部数据和其他工具）的中央接口。

该库提供的功能比本文中提到的要多得多。以目前的发展速度，这篇文章也可能在一个月内过时。

主要内容翻译自：Getting Started with LangChain: A Beginner’s Guide to Building LLM-Powered Applications。翻译的时候有删改。

原文链接： [htt](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c)ps:// towardsdatascience.com/ getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c