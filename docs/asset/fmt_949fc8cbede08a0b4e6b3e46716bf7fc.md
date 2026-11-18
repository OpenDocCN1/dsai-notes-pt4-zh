
1. 数据增强生成：LangChain 使链能够与外部数据源交互以收集生成步骤的数据。例如，它可以帮助总结长文本或使用特定数据源回答问题。

2. Agents：Agents 让 LLM 做出有关行动的决定，采取这些行动，检查结果，并继续前进直到工作完成。LangChain 提供了代理的标准接口，多种代理可供选择，以及端到端的代理示例。

3. 内存：LangChain 有一个标准的内存接口，有助于维护链或代理调用之间的状态。它还提供了一系列内存实现和使用内存的链或代理的示例。

4. 评估：很难用传统指标评估生成模型。这就是为什么 LangChain 提供提示和链来帮助开发者自己使用 LLM 评估他们的模型。

# LangChain 如何使用?

## LangChain 如何调用 LLMs 生成回复？

要调用 LLMs 生成回复，可以使用 LangChain 框架提供的 `LLMChain` 类。

`LLMChain` 类是 LangChain 的一个组件，用于与语言模型进行交互并生成回复。

以下是一个示例代码片段，展示了如何使用 `LLMChain` 类调用 LLMs 生成回复：

```
from langchain.llms import OpenAI
from langchain.chains import LLMChain

llm = OpenAI(temperature=0.9)  # 创建 LLM 实例
prompt = "用户的问题"  # 设置用户的问题

# 创建 LLMChain 实例
chain = LLMChain(llm=llm, prompt=prompt)

# 调用 LLMs 生成回复
response = chain.generate()

print(response)  # 打印生成的回复
```

在上面的代码中，首先创建了一个 LLM 实例，然后设置了用户的问题作为 `LLMChain` 的 `prompt`。

接下来，调用 `LLMChain` 的 `generate()` 方法来生成回复。

最后，打印生成的回复。

> 请注意，可以根据需要自定义 LLM 的参数，例如 `temperature`、`max_tokens` 等。LangChain 文档中有关于 `LLMChain` 类和 LLM 参数的更多详细信息。

## LangChain 如何修改提示模板？

要修改 LangChain 的提示模板，可以使用 LangChain 框架提供的 `ChatPromptTemplate` 类。`ChatPromptTemplate` 类允许您创建自定义的聊天消息提示，并根据需要进行修改。以下是一个示例代码片段，展示了如何使用 `ChatPromptTemplate` 类修改提示模板：

```
from langchain.prompts import ChatPromptTemplate

# 创建一个空的 ChatPromptTemplate 实例
template = ChatPromptTemplate()

# 添加聊天消息提示
template.add_message("system", "You are a helpful AI bot.")
template.add_message("human", "Hello, how are you doing?")
template.add_message("ai", "I'm doing well, thanks!")
template.add_message("human", "What is your name?")

# 修改提示模板
template.set_message_content(0, "You are a helpful AI assistant.")
template.set_message_content(3, "What is your name? Please tell me.")

# 格式化聊天消息
messages = template.format_messages()

print(messages)
```

在上面的代码中：

- 首先，创建了一个空的 `ChatPromptTemplate` 实例。
- 然后，使用 `add_message()` 方法添加了聊天消息提示。接下来，我们使用 `set_message_content()` 方法修改了第一个和最后一个聊天消息的内容。
- 最后，我们使用 `format_messages()` 方法格式化聊天消息，并打印出来。

> 请注意，可以根据需要添加、删除和修改聊天消息提示。`ChatPromptTemplate` 类提供了多种方法来操作提示模板。更多详细信息和示例代码可以在 LangChain 文档中找到。

## LangChain 如何连接多个组件处理一个特定的下游任务？

要链接多个组件处理一个特定的下游任务，可以使用 LangChain 框架提供的 `Chain` 类。`Chain` 类允许您将多个组件连接在一起，以便按顺序处理任务。以下是一个示例代码片段，展示了如何使用 `Chain` 类链接多个组件处理下游任务：

```
from langchain.chains import Chain
from langchain.components import Component1, Component2, Component3

# 创建组件实例
component1 = Component1()
component2 = Component2()
component3 = Component3()

# 创建 Chain 实例并添加组件
chain = Chain()
chain.add_component(component1)
chain.add_component(component2)
chain.add_component(component3)

# 处理下游任务
output = chain.process_downstream_task()

print(output)
```

在上面的代码中：

- 首先，创建了多个组件的实例，例如 `Component1`、`Component2` 和 `Component3`。
- 然后，创建了一个 `Chain` 实例，并使用 `add_component()` 方法将这些组件添加到链中。
- 最后，我们调用 `process_downstream_task()` 方法来处理下游任务，并打印输出结果。

> 请注意，可以根据需要添加、删除和修改组件。`Chain` 类提供了多种方法来操作链。

## LangChain 如何 Embedding & vector store？

要在 LangChain 中进行嵌入和向量存储，可以使用 LangChain 框架提供的 `Embedding` 和 `VectorStore` 类。`Embedding` 类用于将文本嵌入到向量空间中，而 `VectorStore` 类用于存储和检索嵌入向量。以下是一个示例代码片段，展示了如何在 LangChain 中进行嵌入和向量存储：

```
from langchain.embeddings import Embedding
from langchain.vectorstore import VectorStore

# 创建 Embedding 实例
embedding = Embedding()

# 将文本嵌入到向量空间中
embedding.embed("Hello, world!")

# 创建 VectorStore 实例
vector_store = VectorStore()

# 存储嵌入向量
vector_store.store("hello", embedding.get_embedding())

# 检索嵌入向量
vector = vector_store.retrieve("hello")

print(vector)
```

在上面的代码中：

- 首先，创建了一个 `Embedding` 实例，并使用 `embed()` 方法将文本嵌入到向量空间中。
- 然后，我们创建了一个 `VectorStore` 实例，并使用 `store()` 方法将嵌入向量存储到向量存储中。
- 最后，使用 `retrieve()` 方法检索嵌入向量，并打印出来。

> 请注意，可以根据需要添加、删除和修改嵌入向量。`Embedding` 类和 `VectorStore` 类提供了多种方法来操作嵌入和向量存储。

# LangChain 知识问答实践

基于 LangChain 的知识问答系统框架如图所示：

![](img/42b5dea3eb44ff405d0b5902dfe878c4.png)

知识库问答系统主要包含以下几个主要步骤：

1. 收集领域知识数据构造知识库，这些数据应当能够尽可能的全面覆盖问答需求。
2. 将知识库中的对非结构数据进行文本提取和文本拆分，得到文本块。
3. 利用嵌入向量表示模型给出文本块嵌入表示，并利用向量数据库进行保存。
4. 根据用户输入信息的嵌入表示，通过向量数据库检索得到最相关文本片段，利用提示词模板与用户输入以及历史消息合并输入大语言模型。
5. 将大语言模型结果返回用户。

上述过程的代码示例如下所示：

```
from langchain.document_loaders import DirectoryLoader
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.text_splitter import CharacterTextSplitter
from langchain.vectorstores import Chroma
from langchain.chains import ChatVectorDBChain, ConversationalRetrievalChain
from langchain.chat_models import ChatOpenAI
from langchain.chains import RetrievalQA

# 从本地读取相关数据
loader = DirectoryLoader(
  './Langchain/KnowledgeBase/', glob='**/*.pdf', show_progress=True
)

docs = loader.load()

# 将文件进行切分
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
docs_split = text_splitter.split_documents(docs)

# 初始化 OpenAI Embeddings
embeddings = OpenAIEmbeddings()

# 将数据存入 Chroma 向量存储
vector_store = Chroma.from_documents(docs, embeddings)

# 初始化检索器，使用向量存储
retriever = vector_store.as_retriever()
system_template = """
Use the following pieces of context to answer the users question. If you don't know the answer, just say that you don't know, don't try to make up an answer. Answering these questions in Chinese.
----------
{question}
----------
{chat_history}
"""

# 构建初始 Messages 列表
messages = [
  SystemMessagePromptTemplate.from_template(system_template),
  HumanMessagePromptTemplate.from_template('{question}')
]

# 初始化 Prompt 对象
prompt = ChatPromptTemplate.from_messages(messages)

# 初始化大语言模型，使用 OpenAI API
llm=ChatOpenAI(temperature=0.1, max_tokens=2048)

# 初始化问答链
qa = ConversationalRetrievalChain.from_llm(llm,retriever,condense_question_prompt=prompt)

chat_history = []
while True:
  question = input('问题：')
  # 开始发送问题 chat_history 为必须参数, 用于存储对话历史
  result = qa({'question': question, 'chat_history': chat_history})
  chat_history.append((question, result['answer']))
  print(result['answer'])
```
