# RAG 快速上手 2: LangChain 之 rag_chain

> 来源：[https://agijuejin.feishu.cn/docx/Dy9hdPOOBowoFPxg2DycR6N7nYe](https://agijuejin.feishu.cn/docx/Dy9hdPOOBowoFPxg2DycR6N7nYe)

> 作者：[王几行XING](https://www.zhihu.com/people/brycewang1898)

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/691198743)hihu.com/p/691198...

## 1，Text，embedding，vector db，retriever

```
## 假设已经安装并导入相关的库

## 读取网页
loader = WebBaseLoader(&#x27;https://www.google.com&#x27;, continue_on_failure=True)
docs = loader.load() ## 载入网页的文本

## 文本切割
text_splitter = RecursiveCharacterTextSplitter() ## 相对直接的文本切割方法”高级“一点
documents = text_splitter.split_documents(docs) ## 假设前面已经导入文本文档 docs


## 定义 embedding
embeddings = MistralAIEmbeddings(model=&quot;mistral-embed&quot;) 
## 定义数据库
vector = FAISS.from_documents(documents, embeddings)
## 定义 Retriever
retriever = vector.as_retriever()
```

## 2，Prompt

```
# Define prompt template
template = &quot;&quot;&quot;You are an assistant for question-answering tasks.
Use the following pieces of retrieved context to answer the question.
If you don&#x27;t know the answer, just say that you don&#x27;t know.
Use two sentences maximum and keep the answer concise.
Question: {question}
Context: {context}
Answer:
&quot;&quot;&quot;
from langchain.prompts import ChatPromptTemplate
prompt = ChatPromptTemplate.from_template(template)
```

## 3, LLMs

```
# Define LLM
llm = ChatMistralAI(model=&quot;open-mixtral-8x7b&quot;) #mistral_api_key=api_key
#llm = ChatOpenAI()

## Llama 2
from langchain_community.chat_models import ChatAnyscale
llm = ChatAnyscale(model=&quot;meta-llama/Llama-2-13b-chat-hf&quot;, anyscale_api_key=&#x27;xxx&#x27;)

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
    {&quot;context&quot;: retriever,  &quot;question&quot;: RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)
```

## 5, 启用 RAG Chain 获得回答

```
rag_chain.invoke(&quot;What is &#x27;safdfasdfadf&#x27;?&quot;)

#  Based on the retrieved context, &#x27;safdfasdfadf&#x27; is not a known or recognized term. 
#I couldn&#x27;t find any information or definition related to this phrase in the provided context.
```

(文章结束)

参考 1： [https:// github.com/streamlit/ex ample-a](https://link.zhihu.com/?target=https%3A//github.com/streamlit/example-app-langchain-rag/blob/main/rag_chain.py)pp-langchain-rag/blob/main/rag_chain.py