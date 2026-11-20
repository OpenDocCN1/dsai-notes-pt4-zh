
# LangChain 入门：构建 LLM 支持的应用程序的初学者指南

> 来源：[https://agijuejin.feishu.cn/docx/WD2NdYF2jo801cxh0mMcgqffnAb](https://agijuejin.feishu.cn/docx/WD2NdYF2jo801cxh0mMcgqffnAb)
>
> 作者：北方的郎
>
> 原文：[https://zhuanlan.zhihu.com/p/631948940](https://zhuanlan.zhihu.com/p/631948940)

自 ChatGPT 发布以来，大型语言模型 (LLM) 广受欢迎。尽管您可能没有足够的资金和计算资源从头开始训练 LLM，但您仍然可以使用预训练的 LLM 来构建一些很酷的东西，例如：

+   可以根据您的数据与外界互动的 [个人助理](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/use_cases/personal_assistants.html)
+   为您的目的定制的 [聊天机器人](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/use_cases/chatbots.html)
+   [分析](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/use_cases/question_answering.html)或 [总结](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/use_cases/summarization.html)您的文档或 [代码](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/use_cases/code.html)

LLM 正在改变我们构建人工智能产品的方式

凭借其怪异的 API 和及时的工程设计，[LLM](https://link.zhihu.com/?target=https%3A//github.com/hwchase17/langchain) 正在改变我们构建 AI 驱动产品的方式。这就是为什么新的开发人员工具以“LLMOps”一词出现在各处的原因。

这些新工具之一是 LangChain。

[GitHub - hwchase17/langchain: ⚡ 通过可组合性使用 LLM 构建应用程序 ⚡](https://github.com/hwchase17/langchain)

## LangChain 是什么？

LangChain 是一个框架，旨在通过为您提供以下功能来帮助您更轻松地构建 LLM 支持的应用程序：

+   各种不同基础模型的通用接口（参见 模型），
+   帮助您管理提示的框架（请参阅 [提示](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%23b1a5)），以及
+   长期记忆（参见 [Memory](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%233bc6)）、外部数据（参见 [Indexes](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%23806f)）、其他 LLM（参见 [Chains](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%23fcac)）以及 [LLM](https://link.zhihu.com/?target=https%3A//towardsdatascience.com/getting-started-with-langchain-a-beginners-guide-to-building-llm-powered-applications-95fc8898732c%23b842) 无法处理的任务的其他代理（例如，计算或搜索）（参见 Agents）的中央接口。

它是由 [Harrison Chase](https://github.com/hwchase17/langchain) 创建的开源项目（GitHub 库）。

因为 LangChain 有很多不同的功能，所以一开始可能很难理解它的作用。这就是为什么我们将在本文中介绍 LangChain（当前）的六个关键模块，以便您更好地了解其功能。

## 先决条件

要继续学习本教程，您需要 `langchain` 安装 Python 包并准备好使用所有相关的 API 密钥。

## 安装 LangChain

在安装 `langchain` 软件包之前，请确保您的 Python 版本 ≥ 3.8 且 < 4.0。

要安装 `langchain` Python 包，你可以 `pip` 安装它。

`pip install langchain`

在本教程中，我们使用版本 0.0.147。GitHub 存储库 非常活跃；因此，请确保您拥有最新版本。

完成所有设置后，导入 `langchain` Python 包。

```
import LangChain
```

## API 密钥

使用 LLM 构建应用程序需要您要使用的某些服务的 API 密钥，并且某些 API 会产生相关费用。

LLM 提供者（必需）——您首先需要一个 API 密钥，用于您要使用的 LLM 提供者。我们目前正在经历 “AI 的 Linux 时刻”，开发人员必须主要根据性能和成本之间的权衡，在专有或开源基础模型之间做出选择。

![](img/0f85159fdee78447ddeb25b42bc60b81.png)

### 专有模型

专有模型 是拥有大量专家团队和大量 AI 预算的公司所拥有的闭源基础模型。它们通常比开源模型更大，因此性能更好，但它们的 API 也很昂贵。专有模型提供商的示例包括 OpenAI、co:here、AI21 Labs 或 Anthropic。

大多数可用的 LangChain 教程都使用 OpenAI，但请注意 OpenAI API（实验并不昂贵，但它）不是免费的。要获取 OpenAI API Key，您需要一个 OpenAI 帐户，然后在 API keys 下“Create new secret key”。

```
import os
os.environ["OPENAI_API_KEY"] = ... # 在这里插入你的 API_TOKEN
```

### 开源模型

开源模型 通常是比专有模型功能更小的模型，但它们比专有模型更具成本效益。开源模型的例子是：

+   BigScience 的 [BLOOM](https://link.zhihu.com/?target=https%3A//huggingface.co/bigscience/bloom)
+   Meta AI 的 [LLaMA](https://link.zhihu.com/?target=https%3A//huggingface.co/docs/transformers/main/en/model_doc/llama)
+   谷歌的 [Flan-T5](https://link.zhihu.com/?target=https%3A//huggingface.co/google/flan-t5-xl)
+   Eleuther AI 的 [GPT-J](https://link.zhihu.com/?target=https%3A//huggingface.co/EleutherAI/gpt-j-6b)

许多开源模型都在作为社区中心的 Hugging Face 上组织和托管。要获取 Hugging Face API 密钥，您需要一个 Hugging Face 帐户并在 Access Tokens 下创建一个“New token”。

```
import os

os.environ["HUGGINGFACEHUB_API_TOKEN"] = ... # 在这里插入你的 API_TOKEN
```

您可以免费为开源 LLM 使用 Hugging Face，但您将仅限于性能较低的小型 LLM。

### 矢量数据库（可选）

如果你想使用特定的矢量数据库，如 Pinecone、Weaviate 或 Milvus，你需要向他们注册以获得 API 密钥并查看他们的定价。在本教程中，我们使用无需注册的 Faiss。

### 工具（可选）

根据您希望 LLM 与之交互的工具，例如 OpenWeatherMap 或 SerpAPI，您可能需要向它们注册以获取 API 密钥并检查它们的定价。在本教程中，我们只使用不需要 API 密钥的工具。

你能用 LangChain 做什么？

该包为许多基础模型提供了一个通用接口，支持提示管理，并通过代理充当其他组件（如提示模板、其他 LLM、外部数据和其他工具）的中央接口。

在撰写本文时，LangChain（版本 0.0.147）涵盖六个模块：

+   模型(Models)：从不同的 LLM 和嵌入模型中进行选择
+   提示(Prompts)：管理 LLM 输入
+   链(Chains)：将 LLM 与其他组件相结合
+   索引(Indexs)：访问外部数据
+   记忆(Memory)：记住以前的对话
+   代理(Agents)：访问其他工具

以下部分的代码示例是从 [LangChain 文档](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/index.html) 中复制和修改的。

## 模型：从不同的 LLM 和嵌入模型中进行选择

目前，许多不同的 LLM 正在涌现。LangChain 提供与各种模型的集成，并为所有模型提供简化的界面。

LangChain 区分三种输入和输出不同的模型：

### LLM

LLM 将一个字符串作为输入（提示）并输出一个字符串（完成）。

```
# Proprietary LLM from e.g. OpenAI
# pip install openai
from langchain.llms import OpenAI
llm = OpenAI(model_name="text-davinci-003")
# Alternatively, open-source LLM hosted on Hugging Face
# pip install huggingface_hub
from langchain import HuggingFaceHub
llm = HuggingFaceHub(repo_id = "google/flan-t5-xl")
# The LLM takes a prompt as an input and outputs a completion
prompt = "Alice has a parrot. What animal is Alice's pet?"
completion = llm(prompt)
```

![](img/f1fd779d533489f719e62ab71766b8d6.png)

### 聊天模型

聊天模型 类似于 LLM。他们将聊天消息列表作为输入并返回一条聊天消息。

### 文本嵌入模型

文本嵌入模型 接受文本输入并返回浮点数（嵌入）列表，这是输入文本的数字表示。嵌入有助于从文本中提取信息。然后可以稍后使用该信息，例如，用于计算文本（例如，电影摘要）之间的相似性。

```
# Proprietary text embedding model from e.g. OpenAI
# pip install tiktoken
from langchain.embeddings import OpenAIEmbeddings
embeddings = OpenAIEmbeddings()
# Alternatively, open-source text embedding model hosted on Hugging Face
# pip install sentence_transformers
from langchain.embeddings import HuggingFaceEmbeddings
embeddings = HuggingFaceEmbeddings(model_name = "sentence-transformers/all-MiniLM-L6-v2")
# The embeddings model takes a text as an input and outputs a list of floats
text = "Alice has a parrot. What animal is Alice's pet?"
text_embedding = embeddings.embed_query(text)
```

![](img/4cbf130750ab42e036d77c1e3f990afe.png)

## 提示：管理 LLM 输入

LLM 有奇怪的 API。虽然用自然语言向 LLM 输入提示应该很直观，但在从 LLM 获得所需的输出之前需要对提示进行相当多的调整。这个过程称为 prompt 工程。（prompt engineering。）

一旦你有了一个好的提示，你可能想将它用作其他目的的模板。因此，LangChain 为您提供了所谓的 `PromptTemplates`，它可以帮助您从多个组件构建提示。
