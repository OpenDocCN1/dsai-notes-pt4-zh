
# 使用`llama_index`，一分钟帮你读一本书

> 来源：[https://aiyanxishe.feishu.cn/docx/SUo7doXJ0oMh6Mx1bwxcAblUnFL](https://aiyanxishe.feishu.cn/docx/SUo7doXJ0oMh6Mx1bwxcAblUnFL)

## 程序员AI破局简介

> [《程序员AI破局指南》](https://t.zsxq.com/19EOflzkQ)由字节、百度、阿里等互联网大厂的AI业务相关高管共创，分享开箱即用、面向市场的AI编程知识和工具。
>
> 指南包括六个专栏：**AI编程实战、AI编程工具、AI高效办公、AI副业赚钱、AI提示词工程、AI私人助手**。
>
> [程序员AI破局指南](https://t.zsxq.com/19EOflzkQ) 原价 599 元，限时特惠 199 元/年，5月1日恢复原价。
>
> 加入我们：https://t.zsxq.com/19EOflzkQ
>
> 购买后，添加群主🛰️加入 AI研习社陪伴社群，限时领取超值福利。

## 一、大模型的局限

### 一）局限性

#### 1、领域信息不足

虽然 LLM 只基于公开的数据集进行训练，但这也意味着它缺乏特定领域或专有的非公开信息。在某些情况下，这可能导致 LLM 无法提供更准确或更全面的回答。

#### 2、可能产生误导

虽然 LLM 尽力依据现有的数据提供信息和答案，但在超出其知识范围的情况下，它可能给出不正确或虚构的信息。这是因为 LLM 无法直接获取新信息或与实时数据同步。

#### 3、无法获取实时信息

由于 LLM 的训练成本极高，它无法实时更新其知识库。因此，LLM 可能无法提供最新的信息或应对快速变化的情况。

#### 4、预训练数据不可更改

LLM 使用的预训练数据可能包含错误或过时的信息，并且无法进行更正或删除。这意味着 LLM 可能会基于不准确或过时的数据进行回答。

#### 5、缺乏长期记忆

LLM 的设计目标是根据输入的数据给出准确的回答，但它并没有真正的长期记忆能力。这可能导致 LLM 在处理需要上下文理解的复杂问题时表现不佳。

**详细解决方案这篇非常详细**：向量检索构建智能知识库：实战与进阶

## 二、入门`llama_index`

### 一）`llama_index`是什么

LlamaIndex是一个数据框架，适用于基于LLM的应用程序，用于接收、构建和访问私有或特定领域的数据。它提供了Python和Typescript的版本。

### 二）`llama_index`解决什么问题

LLMs提供了一个人与数据之间的自然语言界面。广泛可用的模型预先训练在大量的公开可用数据上，如维基百科，邮件列表，教科书，源代码等。

然而，虽然 LLMs 在大量的数据上接受了训练，但它们并未针对你的私有数据进行训练。它们隐藏在API中，SQL数据库中，或在PDF和PPT中。

LlamaIndex通过连接到这些数据源并将你的数据添加到LLMs已有的数据中来解决这个问题。这通常被称为检索增强生成（RAG）。RAG使你能够使用LLMs查询你的数据，转化它，并生成新的洞察。你可以提问你的数据，创建聊天机器人，构建半自主的智能体。

### 三）`llama_index`如何使用

LlamaIndex提供以下工具：

- **数据连接器**：从原始来源和格式中摄取您现有的数据。这些可以是API、PDF、SQL等等。
- **数据索引**：将您的数据结构化成便于LLM消费的中间表示形式。
- **引擎**：为您的数据提供自然语言访问。例如：
  - 查询引擎是知识增强输出的强大检索接口。
  - 聊天引擎是与您的数据进行多信息、“来回”互动的会话接口。
- **数据代理**：是由工具增强的LLM驱动的知识工作者，从简单的辅助函数到API集成等等。
- **应用集成**：将LlamaIndex与您的其他生态系统重新连接。这可以是LangChain、Flask、Docker、ChatGPT等任何其他东西！

## 三、`llama_index`实战

一个没有经过阅读训练的人读东西是很慢的，完全赶不上专业大佬（**比如罗振宇**老师）。但是用AI可以帮我们技术平权，快速追赶上大佬。

我们可以利用`llama_index`一分钟读一本书，甚至是一堆书。

### 一）准备工作

数据准备：

我们在`data`目录下准备了一个文本类型的私有数据

![](img/3c25245c1a1b8526d9c995fad8a0cae2.png)

环境准备：

分别安装`openai`包和`llama-index` Python包

```
pip3 install openai
pip3 install llama-index
```

### 二）代码运行

#### 1、运行流程

代码的执行流程分三步，下面的注释里面写的非常清晰。

1. 构建给定目录的索引存储到本地文件
2. 加载索引到内存
3. 先通过索引进行向量召回，再进行对话

完整代码：[Github代码链接](https://github.com/hehan-wang/openai-demo/blob/main/llama_index_demo.py)

```python
import openai, os
from llama_index import GPTVectorStoreIndex, SimpleDirectoryReader
from llama_index import StorageContext, load_index_from_storage

os.environ["OPENAI_API_KEY"] = 'sk-NYsoG3VBKDiTuvdtC969F95aFc4f45379aD3854a93602327'
os.environ["OPENAI_API_BASE"] = 'https://key.wenwen-ai.com/v1'

# 1. 构建索引存储到本地文件
documents = SimpleDirectoryReader('./data').load_data()
index = GPTVectorStoreIndex.from_documents(documents)
index.storage_context.persist('index_data')

# 2. 加载索引
storage_context = StorageContext.from_defaults(persist_dir="./index_data")
index = load_index_from_storage(storage_context)

# 3. 进行查询
query_engine = index.as_query_engine()
response = query_engine.query("什么是八爪鱼?")
print(response)
```

上面的代码中的key是我自己维护的，免费给大家使用，代码可以直接运行！

```
python3 llama_index_demo.py
```

#### 2、运行结果

`llama_index`运行结果如下：

![](img/d948ff5b478ffb326cf0c6dad0f2adb2.png)

GPT4运行结果如下：

![](img/596aeebfe6e59bf1854be673f4a58e46.png)

总结：

我们使用`llama_index`的结果是基于私有数据的，所以才知道八爪鱼是个爬虫工具，而不是一种动物。

我们在索引目录放一本书，让它帮我们读书笔记，读书总结也是分分钟的事！

更多内容可以点击目录：[手册内容汇总](https://aiyanxishe.feishu.cn/wiki/LBXcwPv6UiZgdfkp14Xc1Wo1n3b)

✨ 如果觉得有收获，请点一点下方的“有启发” ✨

🌱 我也会及时优化迭代内容，给大家更好的阅读体验 🌱

https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb
