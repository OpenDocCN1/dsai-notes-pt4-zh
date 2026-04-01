# RAG优化: 非结构化文档解析方案汇总

> 来源：[https://agijuejin.feishu.cn/docx/Z61qdGFoXoP7JcxGSyucVp0Hngb](https://agijuejin.feishu.cn/docx/Z61qdGFoXoP7JcxGSyucVp0Hngb)

> 作者：王大锤

> 原文： [https://zhuanlan.z](https://zhuanlan.zhihu.com/p/704631960)hihu.com/p/704631...

## 一、背景

RAG（Retrieval-Augmented Generation）是一种先进的人工智能技术，由Facebook AI Research（FAIR）团队在2020年提出。它结合了检索（Retrieval）和生成（Generation）两个步骤，通过从大量数据中检索相关信息来辅助语言模型生成更准确、更丰富的文本。

RAG技术的优势在于：

- 能够利用外部知识库，提供更准确、深入的回答。

- 实现知识的即时更新，无需重新训练模型。

- 生成的回答具有较高的可解释性，因为它们直接引用了检索到的来源。

RAG技术可以应用于多种 [自然语言处理 ](https://www.zhihu.com/search?q=%E8%87%AA%E7%84%B6%E8%AF%AD%E8%A8%80%E5%A4%84%E7%90%86&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704631960%7D)[任务，如问](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%9B%BE%E8%B0%B1&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704631960%7D)答系统、文档生成、智能助手、信息检索和 知识图谱 填充等。通过这种方式，RAG能够显著提升大型语言模型在知识密集型任务中的性能。

[RAG的优](https://www.zhihu.com/search?q=%E5%90%91%E9%87%8F%E6%A8%A1%E5%9E%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704631960%7D)化主要可以从知[识库的处理](https://www.zhihu.com/search?q=%E9%87%8D%E6%8E%92%E7%AE%97%E6%B3%95&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704631960%7D)[、词 向](https://www.zhihu.com/search?q=%E7%9F%A5%E8%AF%86%E5%BA%93&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704631960%7D)量模型 、检索算法、 重排算法 、推理生成等几个方向展开进行。本文主要是介绍基于 知识库 解析细分方向的优化工作。

![](img/af29228915fcf4a06acb69656c694d27.png)

---

## 二、解析方法

### 2.1 txt文档解析

文档链接:

[  ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/document_loaders/examples/unstructured_file.html)

参考代码块:

```
from langchain.document_loaders import UnstructuredFileLoader
loader = UnstructuredFileLoader(&quot;./test/test_file1.txt&quot;)
docs = loader.load()
docs[0].page_content[:400]
```

### 2.2 word文档解析

参考文档:

[  ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/document_loaders/examples/word_document.html)

核心代码块:

```
from langchain.document_loaders import UnstructuredWordDocumentLoader
loader = UnstructuredWordDocumentLoader(&quot;example_data/fake.docx&quot;)
data = loader.load()
data   LangChain 0.0.148from langchain.document_loaders import UnstructuredWordDocumentLoader
loader = UnstructuredWordDocumentLoader(&quot;example_data/fake.docx&quot;)
data = loader.load()
data
```

### 2.3 PDF文档解析

2.3.1 基于unstructured库

参考文档:

[  ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/document_loaders/examples/unstructured_file.html)

参考代码块:

首先，解析PDF需要用到OCR技术，需要安装一下OCR相关的函数库。

```
# # Install package
!pip install &quot;unstructured[local-inference]&quot;
!pip install &quot;detectron2@git+https://github.com/facebookresearch/detectron2.git@v0.6#egg=detectron2&quot;
!pip install layoutparser[layoutmodels,tesseract]
```

解析代码块:

```
!wget  https://raw.githubusercontent.com/Unstructured-IO/unstructured/main/example-docs/layout-parser-paper.pdf -P &quot;../../&quot;
loader = UnstructuredFileLoader(&quot;./example_data/layout-parser-paper.pdf&quot;, mode=&quot;elements&quot;)
docs = loader.load()
docs[:5]
```

[参考文献 ](https://www.zhihu.com/search?q=%E5%8F%82%E8%80%83%E6%96%87%E7%8C%AE&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704631960%7D):

![](img/55c93e8a196a145e4d59912ab50a4027.png)

**2.3.2 PyPDF工具**

安装工具:

```
pip install pypdf
```

核心代码块:

```
from langchain.[document_loade](https://www.zhihu.com/search?q=load_and_split&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704631960%7D)rs import PyPDFLoader

loader = PyPDFLoader(&quot;example_data/layout-parser-paper.pdf&quot;)
pages = loader.load_and_split()
```

按页码对文档进行检索:

```
[from langch](https://www.zhihu.com/search?q=faiss_index&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704631960%7D)ain.vectorstores import FAISS
from langchain.embeddings.openai import OpenAIEmbeddings

faiss_index = FAISS.from_documents(pages, OpenAIEmbeddings())
docs = faiss_index.similarity_search(&quot;How will the community be engaged?&quot;, k=2)
for doc in docs:
    print(str(doc.metadata[&quot;page&quot;]) + &quot;:&quot;, doc.page_content)
```

![](img/6db4d0b5f314b6c2ca681e32f9025bfb.png)

**2.3.3 在线读取工具**

```
from langchain.document_loaders import OnlinePDFLoader
loader = OnlinePDFLoader(&quot;https://arxiv.org/pdf/2302.03803.pdf&quot;)
data = loader.load()
print(data)
```

**2.3.4 PDFMiner**

```
from langchain.document_loaders import PDFMinerLoader
loader = PDFMinerLoader(&quot;example_data/layout-parser-paper.pdf&quot;)
data = loader.load()
```

### 2.4 Email邮寄解析

```
from langchain.document_loaders import UnstructuredEmailLoader
loader = UnstructuredEmailLoader(&#x27;example_data/fake-email.eml&#x27;)
data = loader.load()
```

### 2.5 图片内容解析

主要处理jpg、png等类型的图片数据为可以在RAG下游任务使用的文档数据格式。在langchain的官方文档中提供了相关的API。

```
from langchain.document_loaders.image import UnstructuredImageLoader
loader = UnstructuredImageLoader(&quot;layout-parser-paper-fast.jpg&quot;)
data = loader.load()
```

### 2.6 Markdown内容解析

```
loader = document_loaders.UnstructuredFileLoader(filepath,mode=&quot;elements&quot;,autodetect_encoding=True)
docs = loader.load()
```

Markdown的解析和TextLoader是存在区别的，主要是需要设置mode和autodetect_encoding参数，以区分文件中不同块的信息。另外，对Markdown中标题的处理，使用MarkdownHeaderTextSplitter需要配置一个额外的参数strip_headers = False。

```
mar[kdown_splitte](https://www.zhihu.com/search?q=header_splits&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704631960%7D)[r = Markdo](https://www.zhihu.com/search?q=split_text&amp;search_source=Entity&amp;hybrid_search_source=Entity&amp;hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704631960%7D)wnHeaderTextSplitter(
    headers_to_split_on=headers_to_split_on, strip_headers=False
)
md_header_splits = markdown_splitter.split_text(markdown_document)
md_header_splits
```

### 2.7 PPT内容解析

```
from langchain.document_loaders import UnstructuredPowerPointLoader
loader = UnstructuredPowerPointLoader(&quot;example_data/fake-power-point.pptx&quot;)
data = loader.load()
```

### 2.8 DeepDoc解析

[deepdoc ](https://www.zhihu.com/search?q=deepdoc&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22article%22%2C%22sourceId%22%3A704631960%7D)是RAGFlow框架中提出的一个支持多种文本切片的解析模版，包括:Q&A、ReSume、Paper、Manual、Table、Book、Law等等。以更好地结合业务场景，确保结果的可控性和可解释性。

RAGFlow链接:

[  ](https://link.zhihu.com/?target=https%3A//github.com/infiniflow/ragflow/blob/main/README_zh.md)

![](img/499ac4934c7ade46863d493bc80f1be7.png)