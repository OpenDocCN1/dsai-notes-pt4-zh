
# RAG优化: 非结构化文档解析方案汇总

> 来源：https://agijuejin.feishu.cn/docx/Z61qdGFoXoP7JcxGSyucVp0Hngb

> 作者：王大锤

> 原文：https://zhuanlan.zhihu.com/p/704631960

## 一、背景

RAG（Retrieval-Augmented Generation）是一种先进的人工智能技术，由Facebook AI Research（FAIR）团队在2020年提出。它结合了检索（Retrieval）和生成（Generation）两个步骤，通过从大量数据中检索相关信息来辅助语言模型生成更准确、更丰富的文本。

RAG技术的优势在于：

- 能够利用外部知识库，提供更准确、深入的回答。
- 实现知识的即时更新，无需重新训练模型。
- 生成的回答具有较高的可解释性，因为它们直接引用了检索到的来源。

RAG技术可以应用于多种自然语言处理任务，如问答系统、文档生成、智能助手、信息检索和知识图谱填充等。通过这种方式，RAG能够显著提升大型语言模型在知识密集型任务中的性能。

RAG的优化主要可以从知识库的处理、词向量模型、检索算法、重排算法、推理生成等几个方向展开进行。本文主要是介绍基于知识库解析细分方向的优化工作。

![](img/af29228915fcf4a06acb69656c694d27.png)

---

## 二、解析方法

### 2.1 txt文档解析

文档链接：https://python.langchain.com/en/latest/modules/indexes/document_loaders/examples/unstructured_file.html

参考代码块：

```
from langchain.document_loaders import UnstructuredFileLoader
loader = UnstructuredFileLoader("./test/test_file1.txt")
docs = loader.load()
docs[0].page_content[:400]
```

### 2.2 word文档解析

参考文档：https://python.langchain.com/en/latest/modules/indexes/document_loaders/examples/word_document.html

核心代码块：

```
from langchain.document_loaders import UnstructuredWordDocumentLoader
loader = UnstructuredWordDocumentLoader("example_data/fake.docx")
data = loader.load()
data
```

### 2.3 PDF文档解析

#### 2.3.1 基于unstructured库

参考文档：https://python.langchain.com/en/latest/modules/indexes/document_loaders/examples/unstructured_file.html

参考代码块：

首先，解析PDF需要用到OCR技术，需要安装一下OCR相关的函数库。

```
# Install package
!pip install "unstructured[local-inference]"
!pip install "detectron2@git+https://github.com/facebookresearch/detectron2.git@v0.6#egg=detectron2"
!pip install layoutparser[layoutmodels,tesseract]
```

解析代码块：

```
!wget https://raw.githubusercontent.com/Unstructured-IO/unstructured/main/example-docs/layout-parser-paper.pdf -P "../../"
loader = UnstructuredFileLoader("./example_data/layout-parser-paper.pdf", mode="elements")
docs = loader.load()
docs[:5]
```

[参考文献]：

![](img/55c93e8a196a145e4d59912ab50a4027.png)

#### 2.3.2 PyPDF工具

安装工具：

```
pip install pypdf
```

核心代码块：

```
from langchain.document_loaders import PyPDFLoader

loader = PyPDFLoader("example_data/layout-parser-paper.pdf")
pages = loader.load_and_split()
```

按页码对文档进行检索：

```
from langchain.vectorstores import FAISS
from langchain.embeddings.openai import OpenAIEmbeddings

faiss_index = FAISS.from_documents(pages, OpenAIEmbeddings())
docs = faiss_index.similarity_search("How will the community be engaged?", k=2)
for doc in docs:
    print(str(doc.metadata["page"]) + ":", doc.page_content)
```

![](img/6db4d0b5f314b6c2ca681e32f9025bfb.png)

#### 2.3.3 在线读取工具

```
from langchain.document_loaders import OnlinePDFLoader
loader = OnlinePDFLoader("https://arxiv.org/pdf/2302.03803.pdf")
data = loader.load()
print(data)
```

#### 2.3.4 PDFMiner

```
from langchain.document_loaders import PDFMinerLoader
loader = PDFMinerLoader("example_data/layout-parser-paper.pdf")
data = loader.load()
```

### 2.4 Email邮寄解析

```
from langchain.document_loaders import UnstructuredEmailLoader
loader = UnstructuredEmailLoader('example_data/fake-email.eml')
data = loader.load()
```

### 2.5 图片内容解析

主要处理jpg、png等类型的图片数据为可以在RAG下游任务使用的文档数据格式。在langchain的官方文档中提供了相关的API。

```
from langchain.document_loaders.image import UnstructuredImageLoader
loader = UnstructuredImageLoader("layout-parser-paper-fast.jpg")
data = loader.load()
```

### 2.6 Markdown内容解析

```
loader = document_loaders.UnstructuredFileLoader(filepath, mode="elements", autodetect_encoding=True)
docs = loader.load()
```

Markdown的解析和 `TextLoader` 是存在区别的，主要是需要设置 `mode` 和 `autodetect_encoding` 参数，以区分文件中不同块的信息。另外，对Markdown中标题的处理，使用 `MarkdownHeaderTextSplitter` 需要配置一个额外的参数 `strip_headers = False`。

```
markdown_splitter = MarkdownHeaderTextSplitter(
    headers_to_split_on=headers_to_split_on, strip_headers=False
)
md_header_splits = markdown_splitter.split_text(markdown_document)
md_header_splits
```

### 2.7 PPT内容解析

```
from langchain.document_loaders import UnstructuredPowerPointLoader
loader = UnstructuredPowerPointLoader("example_data/fake-power-point.pptx")
data = loader.load()
```

### 2.8 DeepDoc解析

`deepdoc` 是 RAGFlow 框架中提出的一个支持多种文本切片的解析模版，包括：Q&A、ReSume、Paper、Manual、Table、Book、Law 等等。以更好地结合业务场景，确保结果的可控性和可解释性。

RAGFlow链接：https://github.com/infiniflow/ragflow/blob/main/README_zh.md

![](img/499ac4934c7ade46863d493bc80f1be7.png)
