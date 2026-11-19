
# LangChain:万能的非结构化文档载入详解（一）

> 来源：[https://agijuejin.feishu.cn/docx/S49qdB1JwoTMctxQiXDcJ82Bnfb](https://agijuejin.feishu.cn/docx/S49qdB1JwoTMctxQiXDcJ82Bnfb)

> 作者：悟乙己

> 原文：[https://zhuanlan.zhihu.com/p/624812261](https://zhuanlan.zhihu.com/p/624812261)

chatpdf等开源项目需要有非结构化文档载入，这边来看一下`langchain`自带的模块

[Unstructured File Loader](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/document_loaders/examples/unstructured_file.html)

## 1 最头疼的依赖安装

如果要使用需要安装：

```
# # Install package
!pip install "unstructured[local-inference]"
!pip install "detectron2@git+https://github.com/facebookresearch/detectron2.git@v0.6#egg=detectron2"
!pip install layoutparser[layoutmodels,tesseract]
```

`Layout Detection`模块主要是对文档图片进行目标检测识别，比如使用`Faster R-CNN`、`Mask R-CNN`。`LayoutParser`是基于`Detectron2`提供最小的接口,是一个版面分析工具包，它提供了布局检测、`OCR`识别、布局分析等接口

`tesseract`也是比较常见的`OCR`识别库

所以，以上在识别PDF的时候会使用到

不过呢，最烦的是其中会使用`nltk`

```
# import nltk
# nltk.download('punkt')
```

笔者就是不能顺利下载，所以只能手动了。。

从 [nltk_data](https://github.com/nltk/nltk_data) 下载packages，当然可能连github也不能下载，那就百度云：

```
https://pan.baidu.com/s/1LWM3o7iRZMF8XaD91vx9Dw
cnpf
```

下载之后，需要把`packages`文件夹名换成：`nltk_data`，放在`/root/nltk_data`

然后把`tokenizers`文件夹下的`punkt.zip`进行解压，才可能使用

## 2 TXT文档载入

```
from langchain.document_loaders import UnstructuredFileLoader
loader = UnstructuredFileLoader("./example_data/state_of_the_union.txt")
docs = loader.load()
docs[0].page_content[:400]
```

按分段读入txt数据

## 3 识别PDF

PDF文件就需要`OCR`的技术

### 3.1 `unstructured`库

就是之前提到的：

```
# # Install package
!pip install "unstructured[local-inference]"
!pip install "detectron2@git+https://github.com/facebookresearch/detectron2.git@v0.6#egg=detectron2"
!pip install layoutparser[layoutmodels,tesseract]
```

参考：

```
!wget https://raw.githubusercontent.com/Unstructured-IO/unstructured/main/example-docs/layout-parser-paper.pdf -P "../../"
loader = UnstructuredFileLoader("./example_data/layout-parser-paper.pdf", mode="elements")
docs = loader.load()
docs[:5]
```

原文是一个论文

后面识别出来是一个文档：

```
[Document(page_content='LayoutParser : A Uniﬁed Toolkit for Deep Learning Based Document Image Analysis', lookup_str='', metadata={'source': '../../layout-parser-paper.pdf'}, lookup_index=0),
 Document(page_content='Zejiang Shen 1 ( (ea)\n ), Ruochen Zhang 2 , Melissa Dell 3 , Benjamin Charles Germain Lee 4 , Jacob Carlson 3 , and Weining Li 5', lookup_str='', metadata={'source': '../../layout-parser-paper.pdf'}, lookup_index=0),
 Document(page_content='Allen Institute for AI shannons@allenai.org', lookup_str='', metadata={'source': '../../layout-parser-paper.pdf'}, lookup_index=0),
 Document(page_content='Brown University ruochen zhang@brown.edu', lookup_str='', metadata={'source': '../../layout-parser-paper.pdf'}, lookup_index=0),
 Document(page_content='Harvard University { melissadell,jacob carlson } @fas.harvard.edu', lookup_str='', metadata={'source': '../../layout-parser-paper.pdf'}, lookup_index=0)]
```

### 3.2 `PyPDF` package

需要安装：

```
pip install pypdf
```

需要安装，然后使用方式：

```
from langchain.document_loaders import PyPDFLoader

loader = PyPDFLoader("example_data/layout-parser-paper.pdf")
pages = loader.load_and_split()
```

这种方法的一个优点是可以使用页码检索文档：

```
from langchain.vectorstores import FAISS
from langchain.embeddings.openai import OpenAIEmbeddings

faiss_index = FAISS.from_documents(pages, OpenAIEmbeddings())
docs = faiss_index.similarity_search("How will the community be engaged?", k=2)
for doc in docs:
    print(str(doc.metadata["page"]) + ":", doc.page_content)
```

### 3.3 在线读取PDF：`OnlinePDFLoader`

```
from langchain.document_loaders import OnlinePDFLoader
loader = OnlinePDFLoader("https://arxiv.org/pdf/2302.03803.pdf")
data = loader.load()
print(data)
```

### 3.4 `PDFMiner` 通过`BeautifulSoup`解析成`HTML`

`PDFMiner`的方式

```
from langchain.document_loaders import PDFMinerLoader
loader = PDFMinerLoader("example_data/layout-parser-paper.pdf")
data = loader.load()
```

由于输出的`html`内容可以通过`BeautifulSoup`进行解析，从而获得有关字体大小、页码、`pdf`页眉/页脚等更结构化和丰富的信息，因此这有助于将文本在语义上划分为几个部分。



## 3 读取 PDF 文档

```
from langchain.document_loaders import PDFMinerPDFasHTMLLoader
loader = PDFMinerPDFasHTMLLoader("example_data/layout-parser-paper.pdf")
data = loader.load()[0]   # entire pdf is loaded as a single Document

#
from bs4 import BeautifulSoup
soup = BeautifulSoup(data.page_content, 'html.parser')
content = soup.find_all('div')

import re
cur_fs = None
cur_text = ''
snippets = []   # first collect all snippets that have the same font size
for c in content:
    sp = c.find('span')
    if not sp:
        continue
    st = sp.get('style')
    if not st:
        continue
    fs = re.findall('font-size:(\d+)px', st)
    if not fs:
        continue
    fs = int(fs[0])
    if not cur_fs:
        cur_fs = fs
    if fs == cur_fs:
        cur_text += c.text
    else:
        snippets.append((cur_text, cur_fs))
        cur_fs = fs
        cur_text = c.text
snippets.append((cur_text, cur_fs))

from langchain.docstore.document import Document
cur_idx = -1
semantic_snippets = []
# Assumption: headings have higher font size than their respective content
for s in snippets:
    # if current snippet's font size > previous section's heading => it is a new heading
    if not semantic_snippets or s[1] > semantic_snippets[cur_idx].metadata['heading_font']:
        metadata = {'heading': s[0], 'content_font': 0, 'heading_font': s[1]}
        metadata.update(data.metadata)
        semantic_snippets.append(Document(page_content='', metadata=metadata))
        cur_idx += 1
        continue

    # if current snippet's font size <= previous section's content => content belongs to the same section (one can also create
    # a tree like structure for sub sections if needed but that may require some more thinking and may be data specific)
    if not semantic_snippets[cur_idx].metadata['content_font'] or s[1] <= semantic_snippets[cur_idx].metadata['content_font']:
        semantic_snippets[cur_idx].page_content += s[0]
        semantic_snippets[cur_idx].metadata['content_font'] = max(s[1], semantic_snippets[cur_idx].metadata['content_font'])
        continue

    # if current snippet's font size > previous section's content but less than previous section's heading than also make a new
    # section (e.g. title of a pdf will have the highest font size but we don't want it to subsume all sections)
    metadata = {'heading': s[0], 'content_font': 0, 'heading_font': s[1]}
    metadata.update(data.metadata)
    semantic_snippets.append(Document(page_content='', metadata=metadata))
    cur_idx += 1
```

## 4 读入word docx

[ ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/document_loaders/examples/word_document.html)

```
from langchain.document_loaders import UnstructuredWordDocumentLoader
loader = UnstructuredWordDocumentLoader("example_data/fake.docx")
data = loader.load()
data
```

## 5 有意思的Bilibili API

[ ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/document_loaders/examples/bilibili.html)

使用 `bilibili-api` 获取视频文本信息：

```
from langchain.document_loaders.bilibili import BiliBiliLoader
#!pip install bilibili-api
loader = BiliBiliLoader(
    ["https://www.bilibili.com/video/BV1xt411o7Xu/"]
)
loader.load()
```

## 6 直接读取邮件Email

第一种方式：

[ ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/document_loaders/examples/email.html)

```
from langchain.document_loaders import UnstructuredEmailLoader
loader = UnstructuredEmailLoader('example_data/fake-email.eml')
data = loader.load()
```

第二种方式：

```
from langchain.document_loaders import OutlookMessageLoader
loader = OutlookMessageLoader('example_data/fake-email.msg')
data = loader.load()
```

输出：

```
Document(page_content='This is a test email to experiment with the MS Outlook MSG Extractor\r\n\r\n\r\n-- \r\n\r\n\r\nKind regards\r\n\r\n\r\n\r\n\r\nBrian Zhou\r\n\r\n', metadata={'subject': 'Test for TIF files', 'sender': 'Brian Zhou <brizhou@gmail.com>', 'date': 'Mon, 18 Nov 2013 16:26:24 +0800'})
```

## 7 神奇-印象笔记的格式

[ ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/document_loaders/examples/evernote.html)

```
# !pip install pypandoc
# import pypandoc

# pypandoc.download_pandoc()

from langchain.document_loaders import EverNoteLoader

loader = EverNoteLoader("example_data/testing.enex")
loader.load()
```

输出内容：

```
[Document(page_content='testing this\n\nwhat happens?\n\nto the world?\n', lookup_str='', metadata={'source': 'example_data/testing.enex'}, lookup_index=0)]
```

## 8 GitBook

[ ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/document_loaders/examples/gitbook.html)

`GITBOOK` 也是可以输入：

```
from langchain.document_loaders import GitbookLoader
loader = GitbookLoader("https://docs.gitbook.com")
page_data = loader.load()
page_data
```

输出：

```
[Document(page_content='Introduction to GitBook\nGitBook is a modern documentation platform where teams can document everything from products to internal knowledge bases and APIs.\nWe want to help \nteams to work more efficiently\n by creating a simple yet powerful platform for them to \nshare their knowledge\n.\nOur mission is to make a \nuser-friendly\n and \ncollaborative\n product for everyone to create, edit and share knowledge through documentation.\nPublish your documentation in 5 easy steps\nImport\n\nMove your existing content to GitBook with ease.\nGit Sync\n\nBenefit from our bi-directional synchronisation with GitHub and GitLab.\nOrganise your content\n\nCreate pages and spaces and organize them into collections\nCollaborate\n\nInvite other users and collaborate asynchronously with ease.\nPublish your docs\n\nShare your documentation with selected users or with everyone.\nNext\n - Getting started\nOverview\nLast modified \n3mo ago', lookup_str='', metadata={'source': 'https://docs.gitbook.com', 'title': 'Introduction to GitBook'}, lookup_index=0)]
```

## 9 图片加载

[ ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/document_loaders/examples/image.html)

这涵盖了如何将 `jpg`、`png` 等图片加载为我们可以在下游使用的文档格式。

主要是图片的 `OCR` 文字识别

```
from langchain.document_loaders.image import UnstructuredImageLoader
loader = UnstructuredImageLoader("layout-parser-paper-fast.jpg")
data = loader.load()
```

图片输出示例：



```
Document(page_content=&quot;LayoutParser: A Unified Toolkit for Deep\nLearning Based Document Image Analysis\n\n\n‘Zxjiang Shen&#x27; (F3}, Ruochen Zhang”, Melissa Dell*, Benjamin Charles Germain\nLeet, Jacob Carlson, and Weining LiF\n\n\nsugehen\n\nshangthrows, et\n\n“Abstract. Recent advanocs in document image analysis (DIA) have been\n‘pimarliy driven bythe application of neural networks dell roar\n{uteomer could be aly deployed in production and extended fo farther\n[nvetigtion. However, various factory ke lcely organize codebanee\nsnd sophisticated modal cnigurations compat the ey ree of\n‘erin! innovation by wide sence, Though there have been sng\n‘Hors to improve reuablty and simplify deep lees (DL) mode\n‘aon, sone of them ae optimized for challenge inthe demain of DIA,\nThis roprscte a major gap in the extng fol, sw DIA i eal to\nscademic research acon wie range of dpi in the social ssencee\n[rary for streamlining the sage of DL in DIA research and appicn\n‘tons The core LayoutFaraer brary comes with a sch of simple and\nIntative interfaee or applying and eutomiing DI. odel fr Inyo de\npltfom for sharing both protrined modes an fal document dist\n{ation pipeline We demonutate that LayootPareer shea fr both\nlightweight and lrgeseledgtieation pipelines in eal-word uae ces\nThe leary pblely smal at Btspe://layost-pareergsthab So\n\n\n\n‘Keywords: Document Image Analysis» Deep Learning Layout Analysis\n‘Character Renguition - Open Serres dary « Tol\n\n\nIntroduction\n\n\n‘Deep Learning(DL)-based approaches are the state-of-the-art for a wide range of\ndoctiment image analysis (DIA) tea including document image clasiffeation [I]\n&quot;, lookup_str=&#x27;&#x27;, metadata={&#x27;source&#x27;: &#x27;layout-parser-paper-fast.jpg&#x27;}, lookup_index=0)
```

## 10 Markdown

[  ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/document_loaders/examples/markdown.html)

```
from langchain.document_loaders import UnstructuredMarkdownLoader
loader = UnstructuredMarkdownLoader(&quot;../../../../README.md&quot;)
data = loader.load()
```

### 20231230 更新

补充一下，刚好把我之前写的代码当做反面教材，如果按照我之前代码的跑的话，`markdown` 格式中的 `##` 就会被忽略。

```
from langchain.document_loaders import UnstructuredMarkdownLoader
loader = UnstructuredMarkdownLoader(&quot;../../../../README.md&quot;)
data = loader.load()
&gt;&gt;&gt; 特色功能 ABC


# 正确的：
loader = document_loaders.UnstructuredFileLoader(filepath,mode=&quot;elements&quot;,autodetect_encoding=True)
docs = loader.load()
&gt;&gt;&gt; 特色功能 ABC
```

### 需要加上 mode

具体参考： [l](https://link.zhihu.com/?target=https%3A//api.python.langchain.com/en/v0.0.341/_modules/langchain/document_loaders/markdown.html%23) `angchain.document_loaders.markdown` - LangChain 0.0.341

### 区别 `TextLoader`

```
from langchain.document_loaders import TextLoader
from langchain.text_splitter import MarkdownHeaderTextSplitter

md_loader = TextLoader(filepath, encoding=&#x27;utf8&#x27;)
md_doc = md_loader.load()
&gt;&gt;&gt; ### 特色功能 ABC
```

`TextLoader` 是把 `md` 里面所有带 `#` 符号的内容都加载进来，然后后续进行分块。

### 另外

`MarkdownHeaderTextSplitter` 还有一个额外的参数 `strip_headers = False`（20240106 补充，`strip_headers` 过期了，需要设置：`return_each_line = True`）。

```
markdown_splitter = MarkdownHeaderTextSplitter(
    headers_to_split_on=headers_to_split_on, strip_headers=False
)
md_header_splits = markdown_splitter.split_text(markdown_document)
md_header_splits
```

&gt;&gt;&gt;
[Document(page_content=&#x27;# Foo  \n## Bar  \nHi this is Jim  \nHi this is Joe&#x27;, metadata={&#x27;Header 1&#x27;: &#x27;Foo&#x27;, &#x27;Header 2&#x27;: &#x27;Bar&#x27;}),
 Document(page_content=&#x27;### Boo  \nHi this is Lance&#x27;, metadata={&#x27;Header 1&#x27;: &#x27;Foo&#x27;, &#x27;Header 2&#x27;: &#x27;Bar&#x27;, &#x27;Header 3&#x27;: &#x27;Header 3&#x27;}),
 Document(page_content=&#x27;## Baz  \nHi this is Molly&#x27;, metadata={&#x27;Header 1&#x27;: &#x27;Foo&#x27;, &#x27;Header 2&#x27;: &#x27;Baz&#x27;})]
```

因为 `MarkdownHeaderTextSplitter` 默认是把标题从正文中进行删除，放在 `metadata` 当中，如果要恢复，需要设置该参数。

## 11 PowerPoint 幻灯片载入

[  ](https://link.zhihu.com/?target=https%3A//python.langchain.com/en/latest/modules/indexes/document_loaders/examples/powerpoint.html)

```
from langchain.document_loaders import UnstructuredPowerPointLoader
loader = UnstructuredPowerPointLoader(&quot;example_data/fake-power-point.pptx&quot;)
data = loader.load()
```

### 导出

```
[Document(page_content=&#x27;Adding a Bullet Slide\n\nFind the bullet slide layout\n\nUse _TextFrame.text for first bullet\n\nUse _TextFrame.add_paragraph() for subsequent bullets\n\nHere is a lot of text!\n\nHere is some text in a text box!&#x27;, lookup_str=&#x27;&#x27;, metadata={&#x27;source&#x27;: &#x27;example_data/fake-power-point.pptx&#x27;}, lookup_index=0)]
```
