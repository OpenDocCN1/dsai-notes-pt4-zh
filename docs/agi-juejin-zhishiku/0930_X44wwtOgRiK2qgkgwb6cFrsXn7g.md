# RAG：ChatGPT+LangChain 案例 + CSV数据

> 来源：[https://agijuejin.feishu.cn/docx/TeNddoQUeoDoanxud91cHEq4nTe](https://agijuejin.feishu.cn/docx/TeNddoQUeoDoanxud91cHEq4nTe)

> 作者：王几行XING

> 原文：[https://zhuanlan.z](https://zhuanlan.zhihu.com/p/671564317)hihu.com/p/671564...

## 数据来源

本案例使用的数据来自： Amazon Fine Food Reviews ，仅使用了前面10条产品评论数据

(觉得案例有帮助，记得点赞加关注噢~)

![](img/8973b0e9331bd52d78e0a9353dcd682e.png)

## 第一步，数据导入

```
import pandas as pd 
df = pd.read_csv(&quot;/content/Reviews.csv&quot;, nrows=10)
```

![](img/0a220f1ff75f3e9892cfa43574846567.png)

保存这十行数据，并用 LangChain 的 CSVLoader 导入：

```
df.to_csv(&quot;review10.csv&quot;, index=False)
loader = CSVLoader(file_path=&quot;/content/review10.csv&quot;)
data = loader.load()
data
```

![](img/eae2f822ff77f54a59c3a70ba0a03ed9.png)

这里着重解释以下 CSV Loader 的作用：

+   将每一行数据导入为键值对的形式，一行数据就是一个 document（类似于 NoSQL）

+   在每个 document 中加入数据源和行号的信息，作为 meta data 的一部分

+   返回的是一个列表，每个列表元素是一个 document

下面拿出一个 document 进行展示：

```
Document(page_content=&#x27;Id: 1\nProductId: B001E4KFG0\nUserId: A3SGXH7AUHU8GW\n
ProfileName: delmartian\nHelpfulnessNumerator: 1\nHelpfulnessDenominator: 1\n
Score: 5\nTime: 1303862400\nSummary: Good Quality Dog Food\n
Text: I have bought several of the Vitality canned dog food products and have found them all 
to be of good quality. The product looks more like a stew than a processed meat and it smells 
better. My Labrador is finicky and she appreciates this product better than  most.&#x27;, 
metadata={&#x27;source&#x27;: &#x27;/content/review10.csv&#x27;, &#x27;row&#x27;: 0}),
```

## 第二步，数据嵌入和保存

+   embedding，用的是 HuggingFaceEmbeddings(model_name='sentence-transformers/all-MiniLM-L6-v2')，免费使用，但是效果可能不是特别好

+   vector database，FAISS

```
text_splitter = RecursiveCharacterTextSplitter(chunk_size=2000, chunk_overlap=20)
text_chunks = text_splitter.split_documents(data)

## Embedding and vector database
embeddings = HuggingFaceEmbeddings(model_name=&#x27;sentence-transformers/all-MiniLM-L6-v2&#x27;)
docsearch = FAISS.from_documents(text_chunks, embeddings)
DB_FAISS_PATH = &quot;vectorstore/db_faiss&quot;
docsearch.save_local(DB_FAISS_PATH)
```

## 第三步，保存记忆，定义 LLM

```
## Define the memory
memory = ConversationBufferMemory(memory_key=&quot;chat_history&quot;, return_messages=True)

## 这里我们使用 gpt 3.5
llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, openai_api_key=&#x27;sk-xx&#x27;, temperature=0)
```

注意，这里我们还可以把 llm 定义成 LlaMa 之类的本地模型，但需要下载该模型的 bin 文件。

## 第四步，定义对话检索链 ConversationalRetrievalChain

```
qa = ConversationalRetrievalChain.from_llm(llm, retriever=docsearch.as_retriever(), memory=memory)

qa({&quot;question&quot;: &quot;What is the review summary of the product with product ID = &#x27;B001E4KFG0&#x27;&quot;})[&#x27;answer&#x27;]
```

最后会返回：

```
The review summary of the product with the product ID &#x27;B001E4KFG0&#x27; is &quot;Good Quality Dog Food.&quot;
```

答案正确！