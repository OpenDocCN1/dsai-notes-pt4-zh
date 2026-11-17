
# RAG中涉及的 `Chunk` 方法介绍

> 来源：https://agijuejin.feishu.cn/docx/LNpvdHO2QoiScWxuTRNcsb0bn6g  
> 作者：王大锤  
> 原文：https://zhuanlan.zhihu.com/p/711763073  

## 一、背景

在 RAG 系统中，如何切分 `Chunk` 对 RAG 系统的影响还是挺大的。而且 PDF 中结构会比较复杂，包括图片，而且有些排版比较奇怪，所以 分 `Chunk` 还是一个相对有难度的事情。如果：

- `chunk` 粒度是一个句子/单词，那么更加注重局部、关键信息的查询，相应的会缺失上下文的信息  
- 完整的长篇段落或文章，`chunk` 被 `embedding` 出来的就是整个文章的意思，但是很难精准到个体单词  

这里有几个场景的区分，首先是 🧣（少字符）、知乎/📕（中小量字符数）、博客（超多字符），还有专业性较强、专有名词较多的文章 与 综述类信息总结文章；不同类型的场景会需要不同 `chunk` 分块粒度。

此时 `Chunk` 会影响到以下的信息（RAG 行业交流中发现的一些问题和改进方法）：

- **上下文信息**：例如，《统计学习的要素》这本书有 18 章，每一章都专注于一个主题。每章有副标题和第二层副标题等。人们习惯于在语境中理解文章。  
- **位置信息**：文本的权重取决于它们在文档中的位置。文档开头和结尾的文字比中间的文字更重要  
- **连续的信息**：一个故事可能以 “in the beginning” 开头，然后以 “then”、“therefore”、“after that” 继续，直到以 “finally”、“finally” 等结尾。使用 分块策略，这种连接不再是完整的。  
- **描述信息**：使用分块，可能无法保证描述性信息集中在一起。想象一下你正在打电话，突然电话线被切断了。这取决于你的电话有多重要，当它发生时，影响范围从微不足道到非常令人沮丧。  

由此 RAG 任务不擅长回答的几类问题：

1. **不擅长小范围的描述性问题回答**。例如，哪个主体具有某些特征？  
1. **不擅长关系推理**，即寻找从实体 A 到实体 B 的路径或识别 实体集团。  
1. **不擅长时间跨度很长的总结**。例如，“列出所有哈利波特的战斗”或“哈利波特有多少次战斗？”  

在确定最佳分块策略时，有几个因素会对我们的选择起到至关重要的影响：

被索引内容的性质是什么？这可能差别会很大，是处理较长的文档（如文章或书籍），还是处理较短的内容（如 🧣 或即时消息）？答案将决定哪种模型更适合您的目标，从而决定应用哪种分块策略。

您使用的是哪种 `Embedding` 模型，它在多大的块大小上表现最佳？例如，`sentence-transformer` 模型在单个句子上工作得很好，但像 `text-embedt-ada-002` 这样的模型在包含 256 或 512 个 `tokens` 的块上表现得更好。

你对用户查询的长度和复杂性有什么期望？用户输入的问题文本是简短而具体的还是冗长而复杂的？这也直接影响到我们选择分组内容的方式，以便在嵌入查询和嵌入文本块之间有更紧密的相关性。

如何在您的特定应用程序中使用检索结果？例如，它们是否用于语义搜索、问答、摘要或其他目的？例如，和你底层连接的 `LLM` 是有直接关系的，`LLM` 的 `tokens` 限制会让你不得不考虑分块的大小。

没有最好的分块策略，只有适合的分块策略，为了确保查询结果更加准确，有时候我们甚至需要选择性的使用几种不同的策略。

## 二、相关产品

### 2.1 FastGPT

![](img/d342473eda21b84bcb8e9e3c2bdeac0c.png)

### 2.2 Langchain-Chatchat

![](img/d67ebbeb67363cfd0ebb67e1ab0c5f69.png)

### 2.3 千帆知识库

![](img/d61319a37ae100593a48db4f2a3d28a2.png)

### 2.4 星火知识库

![](img/7419f026743c57c3a4df048b4e57d436.png)

## 三、LangChain 工具

`Langchain` 提供了很多文本切割的工具，其中 `langchain` 默认使用 `RecursiveCharacterTextSplitter`：

- `RecursiveCharacterTextSplitter()`：按字符串分割文本，递归地尝试按不同的分隔符进行分割文本。  
- `CharacterTextSplitter()`：按字符来分割文本。  
- `MarkdownHeaderTextSplitter()`：基于指定的标题来分割 `markdown` 文件。  
- `TokenTextSplitter()`：按 `token` 来分割文本。  
- `SentenceTransformersTokenTextSplitter()`：按 `token` 来分割文本。  
- `Language()` - 用于 `CPP`、`Python`、`Ruby`、`Markdown` 等。  
- `NLTKTextSplitter()`：使用 `NLTK`（自然语言工具包）按句子分割文本。  
- `SpacyTextSplitter()` - 使用 `Spacy` 按句子的切割文本。  

### 3.1 RecursiveCharacterTextSplitter：重叠滑窗分句方法

`RecursiveCharacterTextSplitter` 是 `Langchain` 的默认文本分割器，它按不同的字符递归地分割文档，同时要兼顾被分割文本的长度和重叠字符，递归分块使用一组分隔符以分层和迭代的方式将输入文本分成更小的块。如果分割文本开始的时候没有产生所需大小或结构的块，那么这个方法会使用不同的分隔符或标准对生成的块递归调用，直到获得所需的块大小或结构。这意味着虽然这些块的大小并不完全相同，但它们仍然会逼近差不多的大小。

`RecursiveCharacterTextSplitter` 默认使用 `'\n\n'`、`'\n'`、`' '`、`''` 这四个特殊符号作为分割文本的标记，下面对 `RecursiveCharacterTextSplitter()` 方法的参数说明如下：

- `chunk_size`：被切割的字符串的最大长度  
- `chunk_overlap`：如果仅仅使用 `chunk_size` 来切割时，前后两段字符串重叠的字符数量。  
- `add_start_index`：是否在元数据中包含每个块在原始文档中的起始位置  
- `length_function`：如何计算块的长度。默认情况下，只计算字符数，但通常在此处传递令牌计数器  

重复字段的意义 ： 块之间保持一些重叠，以确保语义上下文不会在块之间丢失 。在大多数情况下，固定大小的分块将是最佳方式 。与其他形式的分块相比，固定大小的分块在计算上更加经济且易于使用，因为它在分块过程中不需要使用任何 `NLP` 库 。

下面我们看个例子，在这个例子中我们要对一个字符串 `“abcdefghijklmnopqrstuvwxyz”` 进行分割：

```
from langchain.text_splitter import RecursiveCharacterTextSplitter, CharacterTextSplitter

r_splitter = RecursiveCharacterTextSplitter(
    chunk_size=26, # 块长度
    chunk_overlap=4, # 重叠字符串长度
    separators=["\n\n", "\n", " ", ""] # 默认
)

# 分割字符串
text1 = 'abcdefghijklmnopqrstuvwxyz'
r_splitter.split_text(text1)
>>> ['abcdefghijklmnopqrstuvwxyz']
```

看看另外情况：

```
r_splitter = RecursiveCharacterTextSplitter(
    chunk_size=10, # 块长度
    chunk_overlap=4 # 重叠字符串长度
)
>>> ['abcdefghij', 'ghijklmnop', 'mnopqrstuv', 'stuvwxyz']
```

所以，这里块长度、重叠字符串，都可以体现了，首先 `abcdefghij` 和 第二个结果 `ghijklmnop` 分别是 10 个，然后 `ghij` 四个是重叠的

另外可以根据 `separators` 自定义一些正则表达：

```
r_splitter = RecursiveCharacterTextSplitter(
    chunk_size=150,
    chunk_overlap=0,
    separators=["\n\n", "\n", "(?<=\. )", " ", ""]
)
r_splitter.split_text(some_text)
```

正则表达式 `(?<=\. )` ，它的意思是保证句号 `.` 前面一定会存在字符，这样就避免了句号被保留在句首的情况。

这个正则可以沿用到其他的地方。

### 3.2 CharacterTextSplitter

按字符数来切割文档的文档切割组件，默认情况下是 `\n\n`

来看例子：
