# 使用langchain与你自己的数据对话

> 来源：[https://docs.feishu.cn/docx/ArFUdnnqqoTQxkx9TVFcWUd6nCg](https://docs.feishu.cn/docx/ArFUdnnqqoTQxkx9TVFcWUd6nCg)

**核心点：**

Loader加载器：PyPDFLoader，YoutubeAudioLoader，WebBaseLoader，DirectoryLoader

文档切割器： RecursiveCharacterTextSplitter, CharacterTextSplitter，TokenTextSplitter，MarkdownHeaderTextSplitter等

**核心点：**

Chroma向量数据库的持久化

```
#创建向量数据库
vectordb = Chroma.from_documents(
    documents=splits,
    embedding=embedding,
    persist_directory=persist_directory
)
vectordb.persist()
```

FAISS向量数据库的持久化

```
from langchain.vectorstores.faiss import FAISS
import pickle

vectorstore = FAISS.from_documents(documents, embeddings)

with open(&quot;vectorstore.pkl&quot;, &quot;wb&quot;) as f:
    pickle.dump(vectorstore, f)
```

加载向量存储，并将其放入内存中

```
with open(&quot;vectorstore.pkl&quot;, &quot;rb&quot;) as f:
    vectorstore = pickle.load(f)
```

**核心点：**检[索(R](https://so.csdn.net/so/search?q=%E7%9B%B8%E5%85%B3%E6%80%A7&spm=1001.2101.3001.7020)etrieval) 中我们介绍了基本语义相似度(Basic semantic similarity)，最大边际相关性(Maximum marginal relevance，MMR), 过滤元数据, LLM辅助检索

**核心点: **介绍了几种RetrievalQA检索向量数据库的工作方式，也就是chain type方式，其实默认方式是stuff，除此之外还有map_reduce，refine, map_rerank等几种方式

**使用提词**

```
from langchain.prompts import PromptTemplate
 
# Build prompt
template = &quot;&quot;&quot;Use the following pieces of context to answer the question at the end. \
If you don&#x27;t know the answer, just say that you don&#x27;t know, don&#x27;t try to make up an answer. \
Use three sentences maximum. Keep the answer as concise as possible. Always say &quot;thanks for asking!&quot; \
at the end of the answer. 
{context}
Question: {question}
Helpful Answer:&quot;&quot;&quot;
 
QA_CHAIN_PROMPT = PromptTemplate.from_template(template)

# Run chain
qa_chain = RetrievalQA.from_chain_type(
    llm,
    retriever=vectordb.as_retriever(),
    return_source_documents=True,
    chain_type_kwargs={&quot;prompt&quot;: QA_CHAIN_PROMPT}
)

question = &quot;What are major topics for this class?&quot;
result = qa_chain({&quot;query&quot;: question})
result[&quot;result&quot;]
```

**核心点：**ConversationalRetrievalChain实现上下文聊天记忆功能

ConversationalRetrievalQA chain 是建立在 RetrievalQAChain 之上，提供聊天历史记录的组件。

它首先将聊天记录（显式传入或从提供的内存中检索）和问题组合成一个独立的问题，然后从检索器中查找相关文档，最后将这些文档和问题传递到问答链以返回一个响应。

想要创建一个，那我们就需要retriever。在下面的示例中，我们将从向量存储创建一个，该向量存储可以从embeddings中创建。

```
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.vectorstores import Chroma
from langchain.text_splitter import CharacterTextSplitter
from langchain.llms import OpenAI
from langchain.chains import ConversationalRetrievalChain

# 加载文档。您可以将其替换为您想要的任何类型数据的加载器：
from langchain.document_loaders import TextLoader
loader = TextLoader(&quot;../../state_of_the_union.txt&quot;)
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
vectorstore= Chroma.from_documents(documents, embeddings)
```

为了追踪输入、输出，我们需要创建一个memory对象。

```
from langchain.memory import ConversationBufferMemory
memory = ConversationBufferMemory(memory_key=&quot;chat_history&quot;, return_messages=True)
```

初始化 ConversationalRetrievalChain：

```
from langchain.chains import ConversationalRetrievalChain

chain = ConversationalRetrievalChain.from_llm(OpenAI(temperature=0), vectorstore.as_retriever(), memory=memory)
from langchain.chains import ConversationalRetrievalChain

query = &quot;总统对科坦吉·布朗·杰克逊说了些什么&quot;
result = chain ({&quot;question&quot;: query})
# 结果
&quot;&quot;&quot; 
总统表示，科坦吉·布朗·杰克逊是美国顶尖的法律专家之一、前私人执业顶级诉讼律师、前联邦公设辩护律师，出身于公立学校教育工作者和警察家庭。
他还表示，她是共识的缔造者，得到了从警察兄弟会到民主党和共和党任命的前法官的广泛支持。
&quot;&quot;&quot;

query = &quot;他有没有提到她继承了谁&quot;
result = chain ({&quot;question&quot;: query})

result[&#x27;answer&#x27;]

&quot;&quot;&quot; 
科坦吉·布朗·杰克逊 (Ketanji Brown Jackson) 接替斯蒂芬·布雷耶 (Stephen Breyer) 大法官出任美国最高法院法官。
&quot;&quot;&quot;
```

在上面的示例中，我们使用 Memory 对象来追踪聊天历史记录。**

我们也可以直接将其显式传递**。 为了做到这一点，我们需要初始化一个没有任何历史记录的链。

```
chain = ConversationalRetrievalChain.from_llm(OpenAI(temperature=0), vectorstore.as_retriever())
# 这是在没有聊天记录的情况下提问的示例
chat_history = []
query = &quot;总统对科坦吉·布朗·杰克逊说了些什么&quot;
result = chain ({&quot;question&quot;: query, &quot;chat_history&quot;: chat_history})
result[&quot;answer&quot;]

# 结果
&quot;&quot;&quot; 
总统表示，科坦吉·布朗·杰克逊是美国顶尖的法律专家之一、前私人执业顶级诉讼律师、前联邦公设辩护律师，出身于公立学校教育工作者和警察家庭。
他还表示，她是共识的缔造者，得到了从警察兄弟会到民主党和共和党任命的前法官的广泛支持。
&quot;&quot;&quot;

# 这是一个使用一些聊天记录提问的示例
chat_history = [(query, result[&quot;answer&quot;])]
query = &quot;他有没有提到她继承了谁&quot;
result = chain ({&quot;question&quot;: query, &quot;chat_history&quot;: chat_history})
result[&#x27;answer&#x27;]
# 结果
&quot;&quot;&quot; 科坦吉·布朗·杰克逊 (Ketanji Brown Jackson) 接替斯蒂芬·布雷耶 (Stephen Breyer) 大法官出任美国最高法院法官 &quot;&quot;&quot;
```

1.  对话式问答chain是建立在问答chain的基础之上，提供了聊天历史记录的功能，对话式问答主要由ConversationBufferMemory类构建。

1.  聊天记录我们可以使用memory来记录；在ConversationalRetrievalChain.from_llm()方法中指定；也可以在执行时候指定：qa({"question": query, "chat_history": chat_history})。

1.  针对问题、答案，可以使用不同的模型；用便宜的模型来提炼问题，用昂贵的模型回答问题；提炼问题是在ConversationalRetrievalChain.from_llm()方法中指定参数：condense_question_llm。

1.  如果向量存储支持距离搜索，可以使用vectordbkwargs来指定阈值。

1.  可以将不同类型的文档链与 ConversationalRetrievalChain 链结合使用。

1.  想要获取历史记录，可以使用get_chat_history函数；

#### 聊天机器人

```
import openai

openai.api_key = &quot;EMPTY&quot;  # Not support yet
openai.api_base = &quot;http://localhost:8000/v1&quot;
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
        &quot;The following is a friendly conversation between a human and an AI. The AI is talkative and &quot;
        &quot;provides lots of specific details from its context. If the AI does not know the answer to a &quot;
        &quot;question, it truthfully says it does not know.&quot;
    ),
    MessagesPlaceholder(variable_name=&quot;history&quot;),
    HumanMessagePromptTemplate.from_template(&quot;{input}&quot;)
])

llm = ChatOpenAI(model=&quot;text-embedding-ada-002&quot;,temperature=0)
memory = ConversationBufferMemory(return_messages=True)
conversation = ConversationChain(memory=memory, prompt=prompt, llm=llm)

answer = conversation.predict(input=&quot;Hi there!&quot;)
print(answer)

answer = conversation.predict(input=&quot;I&#x27;m doing well! Just having a conversation with an AI.&quot;)
print(answer)

answer = conversation.predict(input=&quot;Tell me about yourself.&quot;)
print(answer)

# &gt;&gt; Hello! How can I assist you today?
# &gt;&gt;That&#x27;s great to hear! What would you like to talk about?
# &gt;&gt;I&#x27;m a language model called Assistant. I was trained on a large dataset of text and can generate human-like responses to a variety of prompts. My purpose is to assist and provide information to users like you.
```