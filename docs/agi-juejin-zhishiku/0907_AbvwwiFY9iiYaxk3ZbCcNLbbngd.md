
# RAG 快速上手 1: LangChain 之 `retrieval_chain`

> 来源：[https://agijuejin.feishu.cn/docx/CgUXdPmhTo5TKCxpfALc8kqBnMV](https://agijuejin.feishu.cn/docx/CgUXdPmhTo5TKCxpfALc8kqBnMV)

> 作者：[王几行XING](https://www.zhihu.com/people/brycewang1898)

> 原文：[https://zhuanlan.zhihu.com/p/691198362](https://zhuanlan.zhihu.com/p/691198362)

## 1，切割文本

```
## 假设已经导入相关的 langchain API
# Split text into chunks
text_splitter = RecursiveCharacterTextSplitter() ## 相对直接的文本切割方法”高级“一点
documents = text_splitter.split_documents(docs) ## 假设前面已经导入文本文档 docs
```

这里 `docs` 是一个文本列表，假设已经通过 `loader` 导入。

## 2，向量数据库 + embedding + `Retriever`

```
## 定义 embedding
embeddings = MistralAIEmbeddings(model="mistral-embed")

## 定义数据库
vector = FAISS.from_documents(documents, embeddings)

## 定义 Retriever
retriever = vector.as_retriever()
```

## 3，定义大模型 + `prompt`

```
# Define LLM
model = "open-mixtral-8x7b"

llm = ChatMistralAI(model=model) # mistral_api_key=api_key

## 定义 prompt，可以随时修改
prompt = ChatPromptTemplate.from_template("""Answer the following question based only on the provided context.
And if you can not do this, please say you can not answer the question based on the given context. If you the context is helpful, please show the source of the context.
<context>
{context}
</context>
Question: {input}
""")
```

## 4，定义 retriever chain

```
## 先定义 document chain
document_chain = create_stuff_documents_chain(llm, prompt)

## 然后定义 Retriever Chain
retrieval_chain = create_retrieval_chain(retriever, document_chain)
```

## 5，通过 retriever chain 进行提问

```
response = retrieval_chain.invoke({"input": "What is akaxkskakasdfadsfadsf?"})
print(response["answer"])

# 回答如下
# Based on the provided context, there is no information about what "akaxkskakasdfadsfadsf" 
# refers to. It's possible that it is a typo or a nonsensical phrase, as it does not appear
# to be related to the Centers for Disease Control and Prevention (CDC) website or any of the 
# linked categories.
```

参考 1：<https://api.python.langchain.com/en/latest/chains/langchain.chains.retrieval.create_retrieval_chain.html>
