
# RAG 快速上手 2: LangChain 之 rag_chain

> 来源：[https://agijuejin.feishu.cn/docx/Dy9hdPOOBowoFPxg2DycR6N7nYe](https://agijuejin.feishu.cn/docx/Dy9hdPOOBowoFPxg2DycR6N7nYe)
> 作者：[王几行XING](https://www.zhihu.com/people/brycewang1898)
> 原文：[https://zhuanlan.zhihu.com/p/691198743](https://zhuanlan.zhihu.com/p/691198743)

## 1，Text，embedding，vector db，retriever

```
## 假设已经安装并导入相关的库

## 读取网页
loader = WebBaseLoader('https://www.google.com', continue_on_failure=True)
docs = loader.load() ## 载入网页的文本

## 文本切割
text_splitter = RecursiveCharacterTextSplitter() ## 相对直接的文本切割方法”高级“一点
documents = text_splitter.split_documents(docs) ## 假设前面已经导入文本文档 docs

## 定义 embedding
embeddings = MistralAIEmbeddings(model="mistral-embed")
## 定义数据库
vector = FAISS.from_documents(documents, embeddings)
## 定义 Retriever
retriever = vector.as_retriever()
```

## 2，Prompt

```
# Define prompt template
template = """You are an assistant for question-answering tasks.
Use the following pieces of retrieved context to answer the question.
If you don't know the answer, just say that you don't know.
Use two sentences maximum and keep the answer concise.
Question: {question}
Context: {context}
Answer:
"""
from langchain.prompts import ChatPromptTemplate
prompt = ChatPromptTemplate.from_template(template)
```

## 3, LLMs

```
# Define LLM
llm = ChatMistralAI(model="open-mixtral-8x7b") #mistral_api_key=api_key
#llm = ChatOpenAI()

## Llama 2
from langchain_community.chat_models import ChatAnyscale
llm = ChatAnyscale(model="meta-llama/Llama-2-13b-chat-hf", anyscale_api_key='xxx')

## GPT 3.5 
from langchain_community.chat_models import ChatOpenAI
llm = ChatOpenAI()
```

## 4, RAG Chain

```
# Setup RAG pipeline
from langchain.schema.runnable import RunnablePassthrough
from langchain.schema.output_parser import StrOutputParser

rag_chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)
```

## 5, 启用 RAG Chain 获得回答

```
rag_chain.invoke("What is 'safdfasdfadf'?")

#  Based on the retrieved context, 'safdfasdfadf' is not a known or recognized term. 
#I couldn't find any information or definition related to this phrase in the provided context.
```

(文章结束)

参考 1：[https://github.com/streamlit/example-app-langchain-rag/blob/main/rag_chain.py](https://link.zhihu.com/?target=https%3A//github.com/streamlit/example-app-langchain-rag/blob/main/rag_chain.py)
