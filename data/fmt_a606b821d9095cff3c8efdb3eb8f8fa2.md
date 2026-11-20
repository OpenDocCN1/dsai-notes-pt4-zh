
# 释放 LangChain 的力量：构建自定义问答聊天机器人的综合指南

> 来源：[https://agijuejin.feishu.cn/docx/O7yBdwhU6oa5mJxFWArctGgVnfg](https://agijuejin.feishu.cn/docx/O7yBdwhU6oa5mJxFWArctGgVnfg)

> 作者：北方的郎

> 原文：[https://zhuanlan.zhihu.com/p/631946257](https://zhuanlan.zhihu.com/p/631946257)

随着 GPT-3 和 GPT-4 等大型语言模型 (LLM) 的出现，NLP 领域发生了革命性的变化。这些模型为创建功能强大、可扩展的应用程序奠定了坚实的基础。然而，这些模型的潜力在很大程度上受到提示质量的影响，突出了提示工程的重要性。此外，现实世界的 NLP 应用程序通常需要比单个 ChatGPT 会话所能提供的更复杂。这就是 LangChain 发挥作用的地方！

Harrison Chase 的创意 LangChain 是一个 Python 库，旨在帮助您利用 LLM 的强大功能来构建自定义 NLP 应用程序。截至 2023 年 5 月，这个改变游戏规则的库已经在 GitHub 上获得了近 40,000 颗星。

![](img/90c2f17abcbb78a9bb92caa38eb74b39.png)

这个初学者指南全面介绍了 LangChain，并对其核心功能进行了详细探索。它引导您完成使用 LangChain 构建基本应用程序的过程，并分享宝贵的技巧和行业最佳实践，以充分利用这个强大的框架。

## LangChain 模块概览

这些模块充当基本抽象，构成由语言模型 (LLM) 支持的应用程序的基础。LangChain 为每个模块提供标准化和适应性强的接口。此外，LangChain 提供外部集成或者现成的实现以实现无缝使用。

![](img/ade4a0321cca63e45d5854d9d58c0bbf.png)

### LLM

LLM 是 LangChain 的基础组成部分。它本质上是一个大型语言模型的包装器，有助于使用特定大型语言模型的功能和能力。

### Chains

如前所述，LLM（语言模型）是 LangChain 中的基本单元。然而，根据“LangChain”概念，它提供了将多个 LLM 调用链接在一起以解决特定目标的能力。

例如，您可能需要从特定 URL 检索数据，总结检索到的文本，并利用生成的摘要来回答问题。

另一方面，Chains 在本质上也可以更简单。例如，您可能想要收集用户输入，使用该输入构建提示，并根据构建的提示生成响应。

### 提示（Prompts）

提示（Prompts）已成为编程中流行的建模方法。它通过专门的类和函数（包括基本的 `PromptTemplate`）简化提示创建和管理。

### 文档加载器和实用程序（Document Loaders and Utils）

LangChain 的 Document Loaders 和 Utils 模块简化了数据访问和计算。文档加载器将不同的数据源转换为文本进行处理，而 utils 模块提供交互式系统会话和用于数学计算的代码片段。

### 向量存储（Vectorstores）

广泛使用的索引类型涉及使用嵌入模型为每个文档生成数字嵌入。这些嵌入以及相关文档存储在向量库中。这个 vectorstore 可以根据它们的嵌入高效地检索相关文档。

### 代理（Agents）

LangChain 为语言模型调用顺序不确定的任务提供了一种灵活的方法。它的“代理”可以根据用户输入和先前的响应采取行动。该库还与矢量数据库集成，并具有记忆功能以保留调用之间的状态，从而实现更高级的交互。

## 构建我们的应用程序

现在我们已经了解了 LangChain，让我们使用 LangChain 和 OpenAI 构建一个 PDF Q/A Bot 应用程序。首先让我向您展示我们应用程序的架构图，然后我们将开始创建我们的应用程序。

![](img/732ee25f85bab09d56f344523d6042d5.png)

下面是一个示例代码，演示了由新技术提供支持的 PDF 问答聊天机器人的架构。此代码使用 OpenAI 语言模型进行自然语言处理，使用 `FAISS` 数据库进行高效的相似性搜索，使用 `PyPDF2` 读取 PDF 文件，使用 `Streamlit` 创建 Web 应用程序界面。聊天机器人利用 LangChain 的会话检索链根据用户的问题从文档中找到最相关的答案。这种集成设置可为用户提供交互式和准确的问答体验。

### 导入必要的库

导入语句：这些行导入运行应用程序所需的必要库和函数。

+ `PyPDF2`：用于读取和操作 PDF 文件的 Python 库。

+ `langchain`：一个用于开发由语言模型驱动的应用程序的框架。

+ `streamlit`：一个用于快速创建 Web 应用程序的 Python 库。

![](img/6830b6abd033ce7bd45c9c4fe3b6bacc.png)

如果尚未安装 LangChain 和 OpenAI，您首先需要在终端中运行以下命令。

![](img/3625d65b4a06fa42a99cd851737a1f95.png)

设置 OpenAI API 密钥

您将用您的 OpenAI API 密钥替换占位符，你需要使用它来使用 OpenAI 的语言模型。

![](img/cbb33a841cf6f5771afc2cc2790ef243.png)

### Streamlit UI

这些代码行使用 Streamlit 创建 Web 界面。提示用户上传 PDF 文件。

![](img/1ac835d53bcc26d7a64344f64b5b59e0.png)

### 阅读PDF文件

如果文件已上传，此块将读取 PDF 文件，从每个页面中提取文本，并将其连接成一个字符串。

![](img/35d3adf169738d04b4e6596d4d8b56b8.png)

### 文本拆分

语言模型通常受到您可以传递给它们的文本数量的限制。因此，有必要将它们分成更小的块。它为此提供了几个实用程序。

![](img/3791fad005d023b9d311f4f26ff11891.png)

使用文本拆分器还可以帮助改进矢量存储搜索的结果，例如。较小的块有时可能更可能匹配查询。在这里，我们将文本拆分为 1k 个标记，其中有 200 个标记重叠。

### 嵌入（Embeddings）

在这里，`OpenAIEmbeddings` 函数用于下载嵌入，它们是文本数据的矢量表示。然后将这些嵌入与 `FAISS` 一起使用，以从文本块中创建有效的搜索索引。

![](img/9d7767062abdb8850ecb9bbfe73471c1.png)

### 创建会话检索链（Creating Conversational Retrieval Chain）

开发的链条是模块化组件，可以轻松重复使用和连接。它们由封装在一行代码中的预定义操作序列组成。使用这些链，无需显式调用 GPT 模型或定义提示属性。这个特定的链允许您在引用文档的同时参与对话并保留交互历史。

![](img/a4e2989ccb459c3aa9eea48ca2ab4c6f.png)

Streamlit 用于生成响应并在应用程序中显示

此块准备包含生成的答案和源文档的响应，并将其显示在 Web 界面上。

![](img/a3d2b22f2d4057b91c50d308ce24d05f.png)

### 运行我们的应用程序

![](img/19fc3f184c49b0dea10bebb54654ec1a.png)

在[这里](https://link.zhihu.com/?target=https%3A//github.com/hyder110/LangChain-Custom-PDF-Chatbot)，我们上传了一个 PDF，提出了一个问题，并通过源文档获得了我们需要的答案。看，这就是 LangChain 的神奇之处。

您可以在我的 GitHub 存储库 `LangChain-Custom-PDF-Chatbot` 上找到此应用程序的代码。

### 结语

结束旅程！掌握 LangChain 以创建基本的问答应用程序是成功的。我相信您已经对 LangChain 的潜力有了基本的了解。现在，主动深入 LangChain，构建更精彩的应用。享受编码冒险。

主要内容翻译自 Unleashing the power of LangChain: A comprehensive guide to building custom Q&A chatbots ，有删改。

原文链接：[https://datasciencedojo.com/blog/understanding-langchain/](https://datasciencedojo.com/blog/understanding-langchain/)
