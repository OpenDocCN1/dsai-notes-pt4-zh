
## 四、使用智谱API

智谱有封装好的SDK，我们调用即可。

```
from zhipuai import ZhipuAI
def zhipu_embedding(text: str):

    api_key = os.environ['ZHIPUAI_API_KEY']
    client = ZhipuAI(api_key=api_key)
    response = client.embeddings.create(
        model="embedding-3",
        input=text,
    )
    return response

text = '要生成 embedding 的输入文本，字符串形式。'
response = zhipu_embedding(text=text)
```

`response` 为 `zhipuai.types.embeddings.EmbeddingsResponded` 类型，我们可以调用 `object`、`data`、`model`、`usage` 来查看 `response` 的 `embedding` 类型、`embedding`、`embedding model` 及使用情况。

```
print(f'response类型为：{type(response)}')
print(f'embedding类型为：{response.object}')
print(f'生成embedding的model为：{response.model}')
print(f'生成的embedding长度为：{len(response.data[0].embedding)}')
print(f'embedding（前10）为: {response.data[0].embedding[:10]}')
```

```
response类型为：<class 'zhipuai.types.embeddings.EmbeddingsResponded'>
embedding类型为：list
生成embedding的model为：embedding-3
生成的embedding长度为：2048
embedding（前10）为: [-0.0042974288, 0.040918995, -0.0036798029, 0.034753118, -0.047749206, -0.015196704, -0.023666998, -0.002935019, 0.0015090306, -0.011958062]
```

# 数据处理

为构建我们的本地知识库，我们需要对以多种类型存储的本地文档进行处理，读取本地文档并通过前文描述的 `Embedding` 方法将本地文档的内容转化为词向量来构建向量数据库。在本节中，我们以一些实际示例入手，来讲解如何对本地文档进行处理。

## 一、源文档选取

我们选用 Datawhale 一些经典开源课程作为示例，具体包括：

+ [《机器学习公式详解》PDF版本](https://github.com/datawhalechina/pumpkin-book/releases)
+ [《面向开发者的LLM入门教程、第一部分Prompt Engineering》md版本](https://github.com/datawhalechina/llm-cookbook)

我们将知识库源数据放置在 `../../data_base/knowledge_db` 目录下。

## 二、数据读取

### 1. PDF 文档

我们可以使用 LangChain 的 `PyMuPDFLoader` 来读取知识库的 PDF 文件。`PyMuPDFLoader` 是 PDF 解析器中速度最快的一种，结果会包含 PDF 及其页面的详细元数据，并且每页返回一个文档。

```
from langchain_community.document_loaders import PyMuPDFLoader

# 创建一个 PyMuPDFLoader Class 实例，输入为待加载的 pdf 文档路径
loader = PyMuPDFLoader("../../data_base/knowledge_db/pumkin_book/pumpkin_book.pdf")

# 调用 PyMuPDFLoader Class 的函数 load 对 pdf 文件进行加载
pdf_pages = loader.load()
```

文档加载后储存在 `pages` 变量中：

+ `page` 的变量类型为 `List`
+ 打印 `pages` 的长度可以看到 pdf 一共包含多少页

```
print(f"载入后的变量类型为：{type(pdf_pages)}，", f"该 PDF 一共包含 {len(pdf_pages)} 页")
```

```
载入后的变量类型为：<class 'list'>， 该 PDF 一共包含 196 页
```

`page` 中的每一元素为一个文档，变量类型为 `langchain_core.documents.base.Document`，文档变量类型包含两个属性

+ `page_content` 包含该文档的内容。
+ `meta_data` 为文档相关的描述性数据。

```
pdf_page = pdf_pages[1]
print(f"每一个元素的类型：{type(pdf_page)}.", 
    f"该文档的描述性数据：{pdf_page.metadata}", 
    f"查看该文档的内容:\n{pdf_page.page_content}", 
    sep="\n------\n")
```

```
每一个元素的类型：<class 'langchain_core.documents.base.Document'>.
------
该文档的描述性数据：{'source': '../../data_base/knowledge_db/pumkin_book/pumpkin_book.pdf', 'file_path': '../../data_base/knowledge_db/pumkin_book/pumpkin_book.pdf', 'page': 1, 'total_pages': 196, 'format': 'PDF 1.5', 'title': '', 'author': '', 'subject': '', 'keywords': '', 'creator': 'LaTeX with hyperref', 'producer': 'xdvipdfmx (20200315)', 'creationDate': "D:20230303170709-00'00'", 'modDate': '', 'trapped': ''}
------
查看该文档的内容:
前言
“周志华老师的《机器学习》（西瓜书）是机器学习领域的经典入门教材之一，周老师为了使尽可能多的读
者通过西瓜书对机器学习有所了解, 所以在书中对部分公式的推导细节没有详述，但是这对那些想深究公式推
导细节的读者来说可能“不太友好”，本书旨在对西瓜书里比较难理解的公式加以解析，以及对部分公式补充
具体的推导细节。”
读到这里，大家可能会疑问为啥前面这段话加了引号，因为这只是我们最初的遐想，后来我们了解到，周
老师之所以省去这些推导细节的真实原因是，他本尊认为“理工科数学基础扎实点的大二下学生应该对西瓜书
中的推导细节无困难吧，要点在书里都有了，略去的细节应能脑补或做练习”。所以...... 本南瓜书只能算是我
等数学渣渣在自学的时候记下来的笔记，希望能够帮助大家都成为一名合格的“理工科数学基础扎实点的大二
下学生”。
使用说明
• 南瓜书的所有内容都是以西瓜书的内容为前置知识进行表述的，所以南瓜书的最佳使用方法是以西瓜书
为主线，遇到自己推导不出来或者看不懂的公式时再来查阅南瓜书；
• 对于初学机器学习的小白，西瓜书第1 章和第2 章的公式强烈不建议深究，简单过一下即可，等你学得
有点飘的时候再回来啃都来得及；
• 每个公式的解析和推导我们都力(zhi) 争(neng) 以本科数学基础的视角进行讲解，所以超纲的数学知识
我们通常都会以附录和参考文献的形式给出，感兴趣的同学可以继续沿着我们给的资料进行深入学习；
• 若南瓜书里没有你想要查阅的公式，或者你发现南瓜书哪个地方有错误，请毫不犹豫地去我们GitHub 的
Issues（地址：https://github.com/datawhalechina/pumpkin-book/issues）进行反馈，在对应版块
提交你希望补充的公式编号或者勘误信息，我们通常会在24 小时以内给您回复，超过24 小时未回复的
话可以微信联系我们（微信号：at-Sm1les）；
配套视频教程：https://www.bilibili.com/video/BV1Mh411e7VU
在线阅读地址：https://datawhalechina.github.io/pumpkin-book（仅供第1 版）
最新版PDF 获取地址：https://github.com/datawhalechina/pumpkin-book/releases
编委会
主编：Sm1les、archwalker、jbb0523
编委：juxiao、Majingmin、MrBigFan、shanry、Ye980226
封面设计：构思-Sm1les、创作-林王茂盛
致谢
特别感谢awyd234、feijuan、Ggmatch、Heitao5200、huaqing89、LongJH、LilRachel、LeoLRH、Nono17、
spareribs、sunchaothu、StevenLzq 在最早期的时候对南瓜书所做的贡献。
扫描下方二维码，然后回复关键词“南瓜书”，即可加入“南瓜书读者交流群”
版权声明
本作品采用知识共享署名-非商业性使用-相同方式共享4.0 国际许可协议进行许可。
```

### 2. MD 文档

我们可以以几乎完全一致的方式读入 markdown 文档：

```
from langchain_community.document_loaders.markdown import UnstructuredMarkdownLoader

loader = UnstructuredMarkdownLoader("../../data_base/knowledge_db/prompt_engineering/1. 简介 Introduction.md")
md_pages = loader.load()
```

读取的对象和 PDF 文档读取出来是完全一致的：

```
print(f"载入后的变量类型为：{type(md_pages)}，", f"该 Markdown 一共包含 {len(md_pages)} 页")
```

```
载入后的变量类型为：<class 'list'>， 该 Markdown 一共包含 1 页
```

```
md_page = md_pages[0]
print(f"每一个元素的类型：{type(md_page)}.", 
    f"该文档的描述性数据：{md_page.metadata}", 
    f"查看该文档的内容:\n{md_page.page_content[0:][:200]}", 
    sep="\n------\n")
```

```
每一个元素的类型：<class 'langchain_core.documents.base.Document'>.
------
该文档的描述性数据：{'source': '../../data_base/knowledge_db/prompt_engineering/1. 简介 Introduction.md'}
------
查看该文档的内容:
第一章 简介

欢迎来到面向开发者的提示工程部分，本部分内容基于吴恩达老师的《Prompt Engineering for Developer》课程进行编写。《Prompt Engineering for Developer》课程是由吴恩达老师与 OpenAI 技术团队成员 Isa Fulford 老师合作授课，Isa 老师曾开发过受欢迎的 ChatGPT 检索插件，并且在教授 LLM （Larg
```

## 三、数据清洗

我们期望知识库的数据尽量是有序的、优质的、精简的，因此我们要删除低质量的、甚至影响理解的文本数据。

可以看到上文中读取的 pdf 文件不仅将一句话按照原文的分行添加了换行符 `\n`，也在原本两个符号中间插入了 `\n`，我们可以使用正则表达式匹配并删除掉 `\n`。

```
import re
pattern = re.compile(r'[^\u4e00-\u9fff](\n)[^\u4e00-\u9fff]', re.DOTALL)
pdf_page.page_content = re.sub(pattern, lambda match: match.group(0).replace('\n', ''), pdf_page.page_content)
print(pdf_page.page_content)
```
