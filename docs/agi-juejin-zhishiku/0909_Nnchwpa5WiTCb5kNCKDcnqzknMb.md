
# RAG 快速上手 3：LangChain `RunnablePassthrough()`

> 来源：[https://agijuejin.feishu.cn/docx/PffMdasSpoz3vOxSA8kcp8tUnaf](https://agijuejin.feishu.cn/docx/PffMdasSpoz3vOxSA8kcp8tUnaf)
>
> 作者：[王几行XING](https://www.zhihu.com/people/brycewang1898)
>
> 原文链接：[https://zhuanlan.zhihu.com/p/691278216](https://zhuanlan.zhihu.com/p/691278216)

## RunnablePassthrough 介绍

`RunnablePassthrough` on its own allows you to pass inputs unchanged. This typically is used in conjunction with `RunnableParallel` to pass data through to a new key in the map.

翻译一下：`RunnablePassthrough` 本身允许您不变地传递输入。这通常与 `RunnableParallel` 结合使用，将数据（变量）传递到映射中的新键。

## 具体案例

### 1. 环境准备

```
!pip install langchain_community langchain_core langchain_openai faiss-cpu -q
import os
import getpass

os.environ['OPENAI_API_KEY'] = getpass.getpass("Input OpenAI Key")

from langchain_community.vectorstores import FAISS
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.runnables import RunnablePassthrough
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
```

### 2. 定义向量数据库并存入数据

```
## 存入数据
vectorstore = FAISS.from_texts(
    ["王几行生活在大农村"], embedding=OpenAIEmbeddings()
)
retriever = vectorstore.as_retriever()

## 定义大模型
llm = ChatOpenAI()
```

### 3. 基于 retrieval chain 建立 RAG

```
template = """Answer the question based only on the following context:
{context}
Question: {question}
"""

prompt = ChatPromptTemplate.from_template(template)
llm = ChatOpenAI()

retrieval_chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)

retrieval_chain.invoke("王几行在哪里生活?")

# 回答如下
# 王几行生活在大农村。
```

### 4. RunnablePassthrough 说明

+ `retrieval_chain.invoke("王几行在哪里生活?")` 表示，该函数接收到了变量 `question = "王几行在哪里生活？"`

+ `"question": RunnablePassthrough()` 使用变量 `question` 做键，所以它的 `value` 接收到了 `question` 的取值，也就是 `"王几行在哪里生活?"`

#### 补充 retrieval_chain

```
retrieval_chain

{
  context: VectorStoreRetriever(tags=['FAISS', 'OpenAIEmbeddings'],
  vectorstore=<langchain_community.vectorstores.faiss.FAISS object at 0x7a4d3b624e80>),
  question: RunnablePassthrough()
}

| ChatPromptTemplate(input_variables=['context', 'question'],
   messages=[HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=['context', 'question'],
        template='Answer the question based only on the following context:\n{context}\n\nQuestion:
             {question}\n'))])
| ChatOpenAI(client=<openai.resources.chat.completions.Completions object at 0x7a4d3b6846d0>,
     async_client=<openai.resources.chat.completions.AsyncCompletions object at 0x7a4d3b6860b0>,
     openai_api_key=SecretStr('**********'), openai_proxy='')
| StrOutputParser()
```

Ref：[Passthrough: Pass through inputs | LangChain](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/expression_language/primitives/passthrough/)
