
# LangChain + ChatGLM2-6B 构建本地知识库代码实现

> 来源：[https://docs.feishu.cn/docx/GBFcdRkiPoJ86DxfxpAc0XfeniZ](https://docs.feishu.cn/docx/GBFcdRkiPoJ86DxfxpAc0XfeniZ)
>
> 官方网站https://github.com/chatchat-space/Langchain-Chatchat

![](img/247d9d4cc075c360e600eef65c381b6a.png)

## 如何实现基于本地知识的问答？

![](img/5b9482062c102a7f7979b3778a768f98.png)

### 基于单一文档问答的实现原理

1. 加载本地文档
1. 文档拆分
1. 根据提问匹配文本
1. 构建`prompt`
1. LLM生成回答

![](img/3288b58fa512a9d618decf6510cd0c43.png)

### 基于本地知识库问答的实现原理

![](img/d4cb0911b77699712ea799c7437d8edc.png)

![](img/046155b7d6bcd43e0b5883897c5c92aa.png)

```
如上图，本地知识库搭建的流程如下：
#创建知识库
（1-2）准备本地知识库文档目前支持 txt、docx、md、pdf 格式文件，使用Unstructured Loader类加载文件，获取文本信息，loader类的使用参考https://python.langchain.com/en/latest/modules/indexes/document_loaders/examples/unstructured_file.html
（3-4）对文本进行分割，将大量文本信息切分为chunks
（5）选择一种embedding算法，对文本向量化
（6）将知识库得到的embedding结果保存到数据库，就不用每次应用都进行前面的步骤

#查询知识库
（8-9）将问题也用同样的embedding算法，对问题向量化
（10）从数据库中查找和问题向量最相似的N个文本信息
（11）得到和问题相关的上下文文本信息
（12）获取提示模板
（13）得到输入大模型的prompt比如：问题：***，通过以下信息***汇总得到答案。
（14）将prompt输入到LLM得到答案
以上只是简单的步骤流程，实际应用中每一步都会有遇到很多问题，比如文本格式如何加载；文本如何分割才能尽可能少的不分开紧密相关上下文；embedding算法的选取；关于提问信息的相关上下文的召回和排序问题；提示模板的形式；大模型的选取等等等等。本文只是尝鲜搭建整套流程，所以不探索这些问题。
```

### 文档处理实现流程

从文档处理角度来看，实现流程如下：

![](img/74084ab50898bb4f7b5d1102bf7ccdfc.png)

## 代码实现

![](img/45294a0886ade321f922867b2a4a8380.png)

## 知识库搭建步骤

### 2.1 硬件需求

首先确定机器性能是否满足要求，仍然使用`ChatGLM-6B`模型

![](img/06fd9d13718c01939602fbab6cdbb878.png)

### 2.2 环境安装

首先安装后续要用到的相关库

```
pip install langchain
--chatGLM相关的上一篇已介绍
-- 加载文档
pip install unstructured
-- 解析表格
pip install tabulate
-- 我使用的sentence_transformers进行embedding
pip install sentence_transformers
-- 向量数据库
pip install chromadb
```

### Langchain代码调用一：使用`OpenAIEmbeddings`

```
from langchain.document_loaders import UnstructuredFileLoader
from langchain.text_splitter import CharacterTextSplitter
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.vectorstores import Chroma

# Set the OpenAI API key
os.environ['OPENAI_API_KEY'] = "sk-xxxxxxxxxxxxxxx"  # get api_key from openai official website
openai.api_key = os.getenv("OPENAI_API_KEY")

#定义文件路径
filepath = "test.txt"

# 加载文件
loader = UnstructuredFileLoader(filepath)
docs = loader.load()

#文本分割
text_splitter = CharacterTextSplitter(chunk_size=500,chunk_overlap=200)
docs = text_splitter.split_text(docs)

#构建向量库
persist_directory = 'docs/chroma/'
embeddings = OpenAIEmbeddings()
vector_store = Chroma.from_documents(documents=docs ,embedding=embedding,persist_directory=persist_directory)


#使用向量知识库
from langchain.vectorstores import Chroma
from langchain.embeddings.openai import OpenAIEmbeddings
 
#这里我们加载了之前保存在本地的向量数据库
persist_directory = 'docs/chroma/'
embedding = OpenAIEmbeddings()
vector_store = Chroma(persist_directory=persist_directory, embedding_function=embedding)

#根据提问匹配上下文
query = "Langchain能够接入哪些数据类型?"
docs = vector_store.similarity_search(query)
context = [doc.page_content for doc in docs]

#启动模型
from transformers import AutoTokenizer, AutoModel
model_config = AutoConfig.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True)
self.tokenizer = AutoTokenizer.from_pretrained("THUDM/chatglm-6b", trust_remote_code=True)
self.model = AutoModel.from_pretrained(model_name_or_path, config=model_config, trust_remote_code=True).half().cuda()

chatglm = model.eval()
#构造Prompt
prompt = f"已知信息：\n{'\n'.join(context)}\n根据已知信息回答问题：\n{query}"
#llm生成回答
chatglm.chat(tokenizer, prompt, history=[])

#第二种方式使用模型
from langchain.chains import RetrievalQA
retriever = vector_store.as_retriever() 
qa = RetrievalQA.from_chain_type(llm=ChatGLM(temperature=0.1), chain_type="stuff", retriever=retriever) 
# 进行问答 query = "2022年腾讯营收多少" 
print(qa.run(query))
```

### Langchain代码调用二：使用`HuggingFaceEmbeddings`

```
from langchain.document_loaders import UnstructuredFileLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter

#1、构建知识库
# 导入文本
loader = UnstructuredFileLoader("./data/test_doc.txt")
# 将文本转成 Document 对象
data = loader.load()
print(f'documents:{len(data)}')

# 初始化加载器
# chunk_size每个分片的最大大小，chunk_overlap分片之间的覆盖大小，可以保持连贯性
text_splitter = RecursiveCharacterTextSplitter(chunk_size=100, chunk_overlap=0)
# 切割加载的 document
split_docs = text_splitter.split_documents(data)
print("split_docs size:", len(split_docs), type(split_docs))

from langchain.vectorstores import Chroma
from langchain.embeddings.huggingface import HuggingFaceEmbeddings
import IPython
import sentence_transformers

# 选择模型---https://huggingface.co/models?pipeline_tag=sentence-similarity&sort=downloads
embedding_model_dict = {
    "ernie-tiny": "nghuyong/ernie-3.0-nano-zh",
    "ernie-base": "nghuyong/ernie-3.0-base-zh",
    "text2vec": "GanymedeNil/text2vec-large-chinese",
    "text2vec2": "uer/sbert-base-chinese-nli",
    "text2vec3": "shibing624/text2vec-base-chinese",
}

EMBEDDING_MODEL = "text2vec3"
# 初始化 hugginFace 的 embeddings 对象
embeddings = HuggingFaceEmbeddings(model_name=embedding_model_dict[EMBEDDING_MODEL], )
embeddings.client = sentence_transformers.SentenceTransformer(
        embeddings.model_name, device='mps')
print("embeddings", embeddings)
print("embeddings model_name", embeddings.model_name)

from langchain.vectorstores import Chroma
#构建向量库
vector_store  = Chroma.from_documents(split_docs, embeddings, persist_directory="./data/doc_embeding")
# 持久化
vector_store .persist()
#######################################
#2、根据提问匹配上下文
# 加载之前持久化数据
vector_store  = Chroma(persist_directory="./data/doc_embeding", embedding_function=embeddings)

question = "什么是用户画像"
similarDocs = db.similarity_search(question, include_metadata=True, k=4)
info = ""
for similardoc in similarDocs:
    info = info + similardoc.page_content

#启动模型
from transformers import AutoTokenizer, AutoModel
tokenizer = AutoTokenizer.from_pretrained("THUDM/chatglm-6b-int8", trust_remote_code=True)
# INT8 量化的模型将"THUDM/chatglm-6b-int4"改为"THUDM/chatglm-6b-int8"
model = AutoModel.from_pretrained("THUDM/chatglm-6b-int8", trust_remote_code=True).half().cuda()
chatglm = model.eval()
history = []
question = "结合以下信息：" + info + "回答" + question
response, history = chatglm .chat(tokenizer, question, history=history)
print("\033[31m 提问：{}\033[0m".format(question))
print("\033[32m 回答：{}\033[0m".format(response))
```
