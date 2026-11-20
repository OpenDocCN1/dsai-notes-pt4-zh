
# 使用langchain与你自己的数据对话

> 来源：[https://docs.feishu.cn/docx/ArFUdnnqqoTQxkx9TVFcWUd6nCg](https://docs.feishu.cn/docx/ArFUdnnqqoTQxkx9TVFcWUd6nCg)

## 加载器与文档切割

**核心点：** Loader加载器：`PyPDFLoader`，`YoutubeAudioLoader`，`WebBaseLoader`，`DirectoryLoader`

**核心点：** 文档切割器：`RecursiveCharacterTextSplitter`，`CharacterTextSplitter`，`TokenTextSplitter`，`MarkdownHeaderTextSplitter`等

## 向量数据库持久化

### Chroma向量数据库持久化

**核心点：** Chroma向量数据库的持久化

```
# 创建向量数据库
vectordb = Chroma.from_documents(
    documents=splits,
    embedding=embedding,
    persist_directory=persist_directory
)
vectordb.persist()
```

### FAISS向量数据库持久化

FAISS向量数据库的持久化

```
from langchain.vectorstores.faiss import FAISS
import pickle

vectorstore = FAISS.from_documents(documents, embeddings)

with open("vectorstore.pkl", "wb") as f:
    pickle.dump(vectorstore, f)
```

### 加载向量存储

加载向量存储，并将其放入内存中

```
with open("vectorstore.pkl", "rb") as f:
    vectorstore = pickle.load(f)
```

## 检索 (Retrieval)

**核心点：**在检索中我们介绍了基本语义相似度（Basic semantic similarity），最大边际相关性（Maximum marginal relevance，MMR），过滤元数据，LLM辅助检索。

## RetrievalQA链类型

**核心点：**介绍了几种`RetrievalQA`检索向量数据库的工作方式，也就是chain type方式，其实默认方式是`stuff`，除此之外还有`map_reduce`，`refine`，`map_rerank`等几种方式。

## 提示词模板

### 使用提词

```
from langchain.prompts import PromptTemplate

# Build prompt
template = """Use the following pieces of context to answer the question at the end. \
If you don't know the answer, just say that you don't know, don't try to make up an answer. \
Use three sentences maximum. Keep the answer as concise as possible. Always say "thanks for asking!" \
at the end of the answer. 
{context}
Question: {question}
Helpful Answer:"""

QA_CHAIN_PROMPT = PromptTemplate.from_template(template)

# Run chain
qa_chain = RetrievalQA.from_chain_type(
    llm,
    retriever=vectordb.as_retriever(),
    return_source_documents=True,
    chain_type_kwargs={"prompt": QA_CHAIN_PROMPT}
)

question = "What are major topics for this class?"
result = qa_chain({"query": question})
result["result"]
```

## ConversationalRetrievalChain

**核心点：**`ConversationalRetrievalChain`实现上下文聊天记忆功能。

`ConversationalRetrievalQA` chain 是建立在 `RetrievalQAChain` 之上，提供聊天历史记录的组件。

它首先将聊天记录（显式传入或从提供的内存中检索）和问题组合成一个独立的问题，然后从检索器中查找相关文档，最后将这些文档和问题传递到问答链以返回一个响应。

想要创建一个，那我们就需要`retriever`。在下面的示例中，我们将从向量存储创建一个，该向量存储可以从`embeddings`中创建。

```
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.vectorstores import Chroma
from langchain.text_splitter import CharacterTextSplitter
from langchain.llms import OpenAI
from langchain.chains import ConversationalRetrievalChain

# 加载文档。您可以将其替换为您想要的任何类型数据的加载器：
from langchain.document_loaders import TextLoader
loader = TextLoader("../../state_of_the_union.txt")
documents = loader.load()

# 如果您想要组合多个加载器，您可以执行以下操作：
# loaders = [....]
# docs = []
# for loader in loaders:
#     docs.extend(loader.load())
# 现在，我们分割文档，为它们创建嵌入，并将它们放入矢量存储中。这使我们能够对它们进行语义搜索。
# 创建拆分器
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
# 拆分文档
documents = text_splitter.split_documents(documents)

# 创建embeddings
embeddings = OpenAIEmbeddings()
# 创建向量存储，使得我们可以进行相关性搜索
vectorstore = Chroma.from_documents(documents, embeddings)
```

为了追踪输入、输出，我们需要创建一个`memory`对象。

```
from langchain.memory import ConversationBufferMemory
memory = ConversationBufferMemory(memory_key="chat_history", return_messages=True)
```

初始化`ConversationalRetrievalChain`：

```
from langchain.chains import ConversationalRetrievalChain

chain = ConversationalRetrievalChain.from_llm(OpenAI(temperature=0), vectorstore.as_retriever(), memory=memory)

query = "总统对科坦吉·布朗·杰克逊说了些什么"
result = chain({"question": query})
# 结果
"""
总统表示，科坦吉·布朗·杰克逊是美国顶尖的法律专家之一、前私人执业顶级诉讼律师、前联邦公设辩护律师，出身于公立学校教育工作者和警察家庭。
他还表示，她是共识的缔造者，得到了从警察兄弟会到民主党和共和党任命的前法官的广泛支持。
"""

query = "他有没有提到她继承了谁"
result = chain({"question": query})

result["answer"]

"""
科坦吉·布朗·杰克逊 (Ketanji Brown Jackson) 接替斯蒂芬·布雷耶 (Stephen Breyer) 大法官出任美国最高法院法官。
"""
```

在上面的示例中，我们使用 `Memory` 对象来追踪聊天历史记录。

我们也可以直接将其显式传递。为了做到这一点，我们需要初始化一个没有任何历史记录的链。

```
chain = ConversationalRetrievalChain.from_llm(OpenAI(temperature=0), vectorstore.as_retriever())
# 这是在没有聊天记录的情况下提问的示例
chat_history = []
query = "总统对科坦吉·布朗·杰克逊说了些什么"
result = chain({"question": query, "chat_history": chat_history})
result["answer"]

# 结果
"""
总统表示，科坦吉·布朗·杰克逊是美国顶尖的法律专家之一、前私人执业顶级诉讼律师、前联邦公设辩护律师，出身于公立学校教育工作者和警察家庭。
他还表示，她是共识的缔造者，得到了从警察兄弟会到民主党和共和党任命的前法官的广泛支持。
"""

# 这是一个使用一些聊天记录提问的示例
chat_history = [(query, result["answer"])]
query = "他有没有提到她继承了谁"
result = chain({"question": query, "chat_history": chat_history})
result["answer"]
# 结果
"""
科坦吉·布朗·杰克逊 (Ketanji Brown Jackson) 接替斯蒂芬·布雷耶 (Stephen Breyer) 大法官出任美国最高法院法官
"""
```

1. 对话式问答`chain`是建立在问答`chain`的基础之上，提供了聊天历史记录的功能，对话式问答主要由`ConversationBufferMemory`类构建。
2. 聊天记录我们可以使用`memory`来记录；在`ConversationalRetrievalChain.from_llm()`方法中指定；也可以在执行时指定：`qa({"question": query, "chat_history": chat_history})`。
3. 针对问题、答案，可以使用不同的模型；用便宜的模型来提炼问题，用昂贵的模型回答问题；提炼问题是在`ConversationalRetrievalChain.from_llm()`方法中指定参数：`condense_question_llm`。
4. 如果向量存储支持距离搜索，可以使用`vectordbkwargs`来指定阈值。
5. 可以将不同类型的文档链与`ConversationalRetrievalChain`链结合使用。
6. 想要获取历史记录，可以使用`get_chat_history`函数。

## 聊天机器人

```
import openai

openai.api_key = "EMPTY"  # Not support yet
openai.api_base = "http://localhost:8000/v1"
from langchain.prompts import (
    ChatPromptTemplate,
    MessagesPlaceholder,
    SystemMessagePromptTemplate,
    HumanMessagePromptTemplate
)
from langchain.chains import ConversationChain
from langchain.chat_models import ChatOpenAI
from langchain.memory import ConversationBufferMemory

prompt = ChatPromptTemplate.from_messages([
    SystemMessagePromptTemplate.from_template(
        "The following is a friendly conversation between a human and an AI. The AI is talkative and "
        "provides lots of specific details from its context. If the AI does not know the answer to a "
        "question, it truthfully says it does not know."
    ),
    MessagesPlaceholder(variable_name="history"),
    HumanMessagePromptTemplate.from_template("{input}")
])

llm = ChatOpenAI(model="text-embedding-ada-002", temperature=0)
memory = ConversationBufferMemory(return_messages=True)
conversation = ConversationChain(memory=memory, prompt=prompt, llm=llm)

answer = conversation.predict(input="Hi there!")
print(answer)

answer = conversation.predict(input="I'm doing well! Just having a conversation with an AI.")
print(answer)

answer = conversation.predict(input="Tell me about yourself.")
print(answer)

# >> Hello! How can I assist you today?
# >>That's great to hear! What would you like to talk about?
# >>I'm a language model called Assistant. I was trained on a large dataset of text and can generate human-like responses to a variety of prompts. My purpose is to assist and provide information to users like you.
```
