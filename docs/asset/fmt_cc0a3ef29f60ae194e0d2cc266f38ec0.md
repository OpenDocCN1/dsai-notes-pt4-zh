
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
