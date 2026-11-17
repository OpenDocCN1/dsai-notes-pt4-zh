
# 从0开始学RAG之快速入门

> 来源：[https://agijuejin.feishu.cn/docx/Pdjtd57vCo6L5zxWdjFcU3pCnZK](https://agijuejin.feishu.cn/docx/Pdjtd57vCo6L5zxWdjFcU3pCnZK)
> 作者：lucas大叔
> 原文：[https://zhuanlan.zhihu.com/p/685411934](https://zhuanlan.zhihu.com/p/685411934)

检索增强生成（RAG）作为一种将 LLM 与外部数据源打通的方法，可以有效弥补 LLM 的实时性和幻觉等短板。常规的 RAG 流程包括 `indexing`、`retrieval` 和 `generation` 三部分，但当面对真实使用场景时，还需要采用更先进的技术和方案来解决现实中的挑战。

下图汇总了 RAG 全链路上各个环节可应用的技术，可以看到林林总总的还不少，接下来我们会用一个系列对图中的每项技术进行讲解和代码实践。有条件的同学可以 follow 视频教程。

![](img/26e0ef18d63630c62d9ac7ef41946038.png)

本节首先讲解如何利用 ChatGPT 快速构建 RAG 应用，大家可以根据自己的使用需求将代码中 `embedding` 模型和 LLM 替换成开源大模型。

---

以下是 RAG 应用的快速实现示例，初学者可以运行体验一下。

```
import bs4
from langchain import hub
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.document_loaders import WebBaseLoader
from langchain_community.vectorstores import Chroma
from langchain_core.output_parsers import StrOutputParser
from langchain_core.runnables import RunnablePassthrough
from langchain_openai import ChatOpenAI, OpenAIEmbeddings

#### INDEXING ####

# Load Documents
loader = WebBaseLoader(
    web_paths=("https://lilianweng.github.io/posts/2023-06-23-agent/",),
    bs_kwargs=dict(
        parse_only=bs4.SoupStrainer(
            class_=("post-content", "post-title", "post-header")
        )
    ),
)
docs = loader.load()

# Split
text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
splits = text_splitter.split_documents(docs)

# Embed
vectorstore = Chroma.from_documents(documents=splits, 
                                    embedding=OpenAIEmbeddings())

retriever = vectorstore.as_retriever()

#### RETRIEVAL and GENERATION ####

# Prompt
prompt = hub.pull("rlm/rag-prompt")

# LLM
llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0)

# Post-processing
def format_docs(docs):
    return "\n\n".join(doc.page_content for doc in docs)

# Chain
rag_chain = (
    {"context": retriever | format_docs, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)

# Question
rag_chain.invoke("What is Task Decomposition?")
```

## Indexing

构建 RAG 应用首先要通过相应的 `loader` 加载数据，不论是 pdf、word、markdown，亦或是网页。加载完成后再选择 `TextSplitter` 将文档切割为 `chunk` 用于 `embedding` 和向量存储。

![](img/87d47cae88b8cb114a5eb861e91dce00.png)

## Retrieval

最简单的 `retrieval` 是将向量存储作为检索器，检索获取相似文档。此外，还有各种类型的 `retriever`，后面我们会一一介绍。

## Generation

将 `retriever` 检索到的候选文档和 `query` 与 `prompt` 一并扔给 LLM，让它根据 `prompt` 中的指令生成答案，生成的答案再用 `parser` 解析成人类方便阅读的格式。

![](img/9834172e422dbc41ef6cf8b52a020ea1.png)

通过上面的示例代码，可以快速构建一个简单的 RAG demo，体验 LLM 的乐趣。若想打造能够工程落地的 RAG 应用，需要针对具体数据和 bad case 确定总的解决方案，在 `indexing`、`retriever` 和 `generation` 等环节上进行联合优化。
