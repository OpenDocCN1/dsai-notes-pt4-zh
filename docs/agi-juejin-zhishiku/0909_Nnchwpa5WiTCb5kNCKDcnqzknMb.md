# RAG 快速上手 3：LangChain RunnablePassthrough()

> 来源：[https://agijuejin.feishu.cn/docx/PffMdasSpoz3vOxSA8kcp8tUnaf](https://agijuejin.feishu.cn/docx/PffMdasSpoz3vOxSA8kcp8tUnaf)

> 作者：[王几行XING](https://www.zhihu.com/people/brycewang1898)

> 原文链接： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/691278216)hihu.com/p/691278...

## RunnablePassthrough 介绍

RunnablePassthrough on its own allows you to pass inputs unchanged. This typically is used in conjuction with RunnableParallel to pass data through to a new key in the map.

**翻译一下：Run****nablePa**ssthroug**h 本**身允许您 不变地传递输入 。这通常与 RunnableParallel 结合使用，将 数据（变量） 传递到映射中的 新键 。

## 具体案例

### 1，环境准备

```
!pip install langchain_community langchain_core langchain_openai faiss-cpu -q 
import os
import getpass

os.environ[&#x27;OPENAI_API_KEY&#x27;] = getpass.getpass(&quot;Input OpenAI Key&quot;)

from langchain_community.vectorstores import FAISS
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.runnables import RunnablePassthrough
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
```

### 2, 定义向量数据库并存入数据

```
## 存入数据
vectorstore = FAISS.from_texts(
    [&quot;王几行生活在大农村&quot;], embedding=OpenAIEmbeddings()
)
retriever = vectorstore.as_retriever()

## 定义大模型
llm = ChatOpenAI()
```

### 3，基于 retrieval chain 建立 RAG

```
template = &quot;&quot;&quot;Answer the question based only on the following context:
{context}
Question: {question}
&quot;&quot;&quot;

prompt = ChatPromptTemplate.from_template(template)
llm = ChatOpenAI()

retrieval_chain = (
    {&quot;context&quot;: retriever, &quot;question&quot;: RunnablePassthrough()}
    | prompt
    | llm
    | StrOutputParser()
)

retrieval_chain.invoke(&quot;王几行在哪里生活?&quot;)

# 回答如下
# 王几行生活在大农村。
```

### 4， RunnablePassthrough 说明

+   retrieval_chain.invoke("王几行在哪里生活?") 表示, 该函数接收到了变量 question = "王几行在哪里生活？"

+   "question": RunnablePassthrough() 使用变量 question 做键，所以它的 value 接收到了 question 的取值，也就是 "王几行在哪里生活?"

补充 retrieval_chain：

```
retrieval_chain

{
  context: VectorStoreRetriever(tags=[&#x27;FAISS&#x27;, &#x27;OpenAIEmbeddings&#x27;], 
  vectorstore=&lt;langchain_community.vectorstores.faiss.FAISS object at 0x7a4d3b624e80&gt;),
  question: RunnablePassthrough()
}

| ChatPromptTemplate(input_variables=[&#x27;context&#x27;, &#x27;question&#x27;], 
   messages=[HumanMessagePromptTemplate(prompt=PromptTemplate(input_variables=[&#x27;context&#x27;, &#x27;question&#x27;], 
        template=&#x27;Answer the question based only on the following context:\n{context}\n\nQuestion: 
             {question}\n&#x27;))])
| ChatOpenAI(client=&lt;openai.resources.chat.completions.Completions object at 0x7a4d3b6846d0&gt;, 
     async_client=&lt;openai.resources.chat.completions.AsyncCompletions object at 0x7a4d3b6860b0&gt;, 
     openai_api_key=SecretStr(&#x27;**********&#x27;), openai_proxy=&#x27;&#x27;)
| StrOutputParser()
```

Ref： [Passthrough: Pass through ](https://link.zhihu.com/?target=https%3A//python.langchain.com/docs/expression_language/primitives/passthrough/)inputs | ️ LangChain