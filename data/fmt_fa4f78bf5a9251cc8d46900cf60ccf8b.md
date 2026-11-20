
## 3.3 文档分割与链类型

`chunk_overlap` 是指切割后的每个 `document` 里包含几个上一个 `document` 结尾的内容，主要作用是为了增加每个 `document` 的上下文关联。比如，`chunk_overlap=0` 时，第一个 `document` 为 `aaaaaa`，第二个为 `bbbbbb`；当 `chunk_overlap=2` 时，第一个 `document` 为 `aaaaaa`，第二个为 `aabbbbbb`。

`chain_type` 主要控制了将 `document` 传递给 `llm` 模型的方式，一共有 4 种方式：

### `stuff`

这种最简单粗暴，会把所有的 `document` 一次全部传给 `llm` 模型进行总结。如果 `document` 很多的话，势必会报超出最大 `token` 限制的错，所以总结文本的时候一般不会选中这个。

### `map_reduce`

这个方式会先将每个 `document` 进行总结，最后将所有 `document` 总结出的结果再进行一次总结。

![](img/3b49afd53ee17a531f7bd554ae30c187.png)

### `refine`

这种方式会先总结第一个 `document`，然后在将第一个 `document` 总结出的内容和第二个 `document` 一起发给 `llm` 模型在进行总结，以此类推。这种方式的好处就是在总结后一个 `document` 的时候，会带着前一个的 `document` 进行总结，给需要总结的 `document` 添加了上下文，增加了总结内容的连贯性。

![](img/e8e559bd0fe92b2c783dfabf68d04560.png)

### `map_rerank`

这种一般不会用在总结的 `chain` 上，而是会用在问答的 `chain` 上，他其实是一种搜索答案的匹配方式。首先你要给出一个问题，他会根据问题给每个 `document` 计算一个这个 `document` 能回答这个问题的概率分数，然后找到分数最高的那个 `document` ，在通过把这个 `document` 转化为问题的 `prompt` 的一部分（问题+`document`）发送给 `llm` 模型，最后 `llm` 模型返回具体答案。

## 3.4 构建本地知识库问答机器人

通过这个可以很方便的做一个可以介绍公司业务的机器人，或是介绍一个产品的机器人。这里主要使用了 `Embedding`（相关性）的能力。

导入 `os`，设置环境变量。导入 `OpenAI` 嵌入模型、`Chroma` 向量数据库、文本分割器、`OpenAI` 模型、向量数据库数据查询模块及文件夹文档加载器

```python
import os
os.environ["OPENAI_API_KEY"] = '你的api key'
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.vectorstores import Chroma
from langchain.text_splitter import CharacterTextSplitter
from langchain import OpenAI, VectorDBQA
from langchain.document_loaders import DirectoryLoader

# 获取当前脚本所在的目录
base_dir = os.path.dirname(os.path.abspath(__file__))

# 构建doc.txt文件的路径
doc_Directory = os.path.join(base_dir, 'static')

# 加载文件夹中的所有txt类型的文件
loader = DirectoryLoader(doc_Directory, glob='**/*.txt')

# 将数据转成 document 对象，每个文件会作为一个 document
documents = loader.load()

# 初始化加载器
text_splitter = CharacterTextSplitter(chunk_size=100, chunk_overlap=0)

# 切割加载的 document
split_docs = text_splitter.split_documents(documents)

# 初始化 openai 的 embeddings 对象
embeddings = OpenAIEmbeddings()

# 将 document 通过 openai 的 embeddings 对象计算 embedding 向量信息并临时存入 Chroma 向量数据库，用于后续匹配查询
docsearch = Chroma.from_documents(split_docs, embeddings)

# 创建问答对象
qa = VectorDBQA.from_chain_type(llm=OpenAI(), chain_type="stuff", vectorstore=docsearch, return_source_documents=True)

# 进行问答
result = qa({"query": "一年收入是多少？"})
```

![](img/8ade9825389e981776b5c27f7981ef40.png)

上图中成功的从我们的给到的数据中获取了正确的答案。

## 3.5 构建向量索引数据库

🏡 Home | `Chroma`

我们上个案例里面有一步是将 `document` 信息转换成向量信息和 `embeddings` 的信息并临时存入 `Chroma` 数据库。

因为是临时存入，所以当我们上面的代码执行完成后，上面的向量化后的数据将会丢失。如果想下次使用，那么就还需要再计算一次 `embeddings`，这肯定不是我们想要的。`LangChain` 支持的数据库有很多，这个案例介绍下通过 `Chroma` 个数据库来讲一下如何做向量数据持久化。

`chroma` 是个本地的向量数据库，他提供的一个 `persist_directory` 来设置持久化目录进行持久化。读取时，只需要调取 `from_document` 方法加载即可。

```python
from langchain.vectorstores import Chroma

# 持久化数据
docsearch = Chroma.from_documents(documents, embeddings, persist_directory="D:/vector_store")
docsearch.persist()

# 从已有文件中加载数据
docsearch = Chroma(persist_directory="D:/vector_store", embedding_function=embeddings)
```

## 3.6 基于 `LangChain` 构建的开源应用

基于 `LangChain` 的优秀项目资源库

[基于 `LangChain` 和 `ChatGLM-6B` 等](https://github.com/thomas-yanxin/LangChain-ChatGLM-Webui) 系列 `LLM` 的针对本地知识库的自动问答

# 4 总结

随着 `LangChain` 不断迭代和优化，它的功能将变得越来越强大，支持的范围也将更广泛。无论是处理复杂的语言模型还是解决各种实际问题，`LangChain` 都将展现出更高的实力和灵活性。然而，我必须承认，我的理解能力和解释能力是有限的，可能会出现错误或者解释不够清晰。因此，恳请读者们谅解。

# 5 参考文献

● `LangChain | LangChain`

● [`LangChain 中文入门教程`](https://liaokong.gitbook.io/llm-kai-fa-jiao-cheng/) - `LangChain` 的中文入门教程

---
