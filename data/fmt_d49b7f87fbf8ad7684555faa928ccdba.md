
# RAG 分块Chunk技术优劣、技巧、方法汇总

> 来源：[https://agijuejin.feishu.cn/docx/Lu27dSRGEofNfCx8TracmaY5nIi](https://agijuejin.feishu.cn/docx/Lu27dSRGEofNfCx8TracmaY5nIi)

> 作者：悟乙己

> 原文： [https://zhuanlan.zhihu.com/p/673906072](https://zhuanlan.zhihu.com/p/673906072)

在上一篇【 悟乙己：大模型RAG 场景、数据、应用难点与解决（四） 】提到了RAG中文档数据不够干净，召回效果也不会好，同时在第2点提到了Langchian-Chatchat中提及到的一些文档预处理方法，包括更加规范的格式、小切片等。

在这其中其实有一个也是非常关键，就是如何切分Chunk，而且PDF中结构会比较复杂，包括图片，而且有些排版比较奇怪，所以分Chunk难度还是蛮大的。

+ chunk粒度是一个句子/单词，那么更加注重局部、关键信息的查询，相应的会缺失上下文的信息
+ 完整的长篇段落或文章，chunk 被embedding出来的就是整个文章的意思，但是很难精准到个体单词

这里有几个场景的区分，首先是🧣（少字符）、知乎/📕（中小量字符数）、博客（超多字符），还有专业性较强、专有名词较多的文章 与 综述类信息总结文章；不同类型的场景会需要不同chunk分块粒度。

此时Chunk会影响到以下的信息（ RAG行业交流中发现的一些问题和改进方法 ）：

+ **上下文信息：** 例如，《统计学习的要素》这本书有18章，每一章都专注于一个主题。每章有副标题和第二层副标题等。人们习惯于在语境中理解文章。
+ **位置信息：** 文本的权重取决于它们在文档中的位置。文档开头和结尾的文字比中间的文字更重要
+ **连续的信息：** 一个故事可能以“in the beginning”开头，然后以“then”、“therefore”、“after that”继续，直到以“finally”、“finally”等结尾。使用分块策略，这种连接不再是完整的。
+ **描述信息：** 使用分块，可能无法保证描述性信息集中在一起。想象一下你正在打电话，突然电话线被切断了。这取决于你的电话有多重要，当它发生时，影响范围从微不足道到非常令人沮丧。

由此RAG任务不擅长回答的几类问题：

1. **不擅长小范围的描述性问题回答。** 例如，哪个主体具有某些特征?
1. **不擅长关系推理，** 即寻找从实体A到实体B的路径或识别实体集团。
1. **不擅长时间跨度很长的总结。** 例如，“列出所有哈利波特的战斗”或“哈利波特有多少次战斗?”

此时借由【 最详细的文本分块(Chunking)方法，直接影响LLM应用效果 】文章信息，在确定最佳分块策略时，有几个因素会对我们的选择起到至关重要的影响：

1. **被索引内容的性质是什么？** 这可能差别会很大，是处理较长的文档(如文章或书籍)，还是处理较短的内容(如🧣或即时消息)？答案将决定哪种模型更适合您的目标，从而决定应用哪种分块策略。
1. 您使用的是哪种Embedding模型，它在多大的块大小上表现最佳？ 例如，`sentence-transformer` 模型在单个句子上工作得很好，但像 `text-embedding-ada-002` 这样的模型在包含256或512个tokens的块上表现得更好。
1. 你对用户查询的长度和复杂性有什么期望？ 用户输入的问题文本是简短而具体的还是冗长而复杂的？这也直接影响到我们选择分组内容的方式，以便在嵌入查询和嵌入文本块之间有更紧密的相关性。
1. 如何在您的特定应用程序中使用检索结果？例如，它们是否用于语义搜索、问答、摘要或其他目的？例如，和你底层连接的LLM是有直接关系的，LLM的tokens限制会让你不得不考虑分块的大小。

没有最好的分块策略，只有适合的分块策略，为了确保查询结果更加准确，有时候我们甚至需要选择性的使用几种不同的策略。

本篇，就是来小结Chunk的一些基本技术点，从Langchian 、 Langchian-Chatchat等进行参考。

---

## 1 一些产品化的分块模块

第一篇先列举一些已经产品化的分块模块，大多大同小异，以Langchain为基准使用。

### 1.1 FastGPT

包括分块长度

![](img/bd7769ec1902bbcbf922ea90f016ad81.png)

### 1.2 Langchian-Chatchat

分块长度、重合长度、中文标题加强

![](img/029002711debb50bd3ef2b7caf036a8b.png)

### 1.3 Baidu千帆知识库

非结构化数据预处理这里应该是非常丰富的，自动分段；自定义分段，包括了长度、重叠长度；还有层次分级，专门针对Doc

![](img/16546bb90badd3084d9277dbd6ec446f.png)

结构化数据，可以直接导入：

![](img/c57ebbbf66633a066ae4c484cfa8acee.png)

### 1.4 星火知识库

大概也是切分符、长度几个参数

![](img/96bd62244f51f48028c899e83c346388.png)

包括针对QA对的识别符：

![](img/e3879f0d805ec9cc979a75f7dcef9470.png)

### 1.5 Jina - Dify机器人

来自文档： 如何训练出专属于“你”的问答机器人？ - Dify

![](img/dad097097ed9ecb8d1475bba3dc963cd.png)

常规的自动分段与清洗

---

## 2 Langchain/Langchain-chatchat

Langchain提供了很多文本切割的工具，其中langchain默认使用`RecursiveCharacterTextSplitter()`：

+ `RecursiveCharacterTextSplitter()`：按字符串分割文本，递归地尝试按不同的分隔符进行分割文本。
+ `CharacterTextSplitter()`：按字符来分割文本。
+ `MarkdownHeaderTextSplitter()`：基于指定的标题来分割markdown 文件。
+ `TokenTextSplitter()`：按token来分割文本。
+ `SentenceTransformersTokenTextSplitter()`：按token来分割文本
+ `Language()` - 用于 CPP、Python、Ruby、Markdown 等。
+ `NLTKTextSplitter()`：使用 `NLTK`（自然语言工具包）按句子分割文本。
+ `SpacyTextSplitter()` - 使用 `Spacy`按句子的切割文本。

### 2.1 RecursiveCharacterTextSplitter：重叠滑窗分句方法

`RecursiveCharacterTextSplitter`是Langchain的默认文本分割器，它按不同的字符递归地分割文档，同时要兼顾被分割文本的长度和重叠字符，递归分块使用一组分隔符以分层和迭代的方式将输入文本分成更小的块。如果分割文本开始的时候没有产生所需大小或结构的块，那么这个方法会使用不同的分隔符或标准对生成的块递归调用，直到获得所需的块大小或结构。这意味着虽然这些块的大小并不完全相同，但它们仍然会逼近差不多的大小。

`RecursiveCharacterTextSplitter`默认使用 `\n\n`、`\n`、` `、`""` 这四个特殊符号作为分割文本的标记，下面对`RecursiveCharacterTextSplitter()`方法的参数说明如下：

+ `chunk_size`：被切割的字符串的最大长度
+ `chunk_overlap`：如果仅仅使用`chunk_size`来切割时，前后两段字符串重叠的字符数量。
+ `add_start_index`：是否在元数据中包含每个块在原始文档中的起始位置
+ `length_function`：如何计算块的长度。默认情况下，只计算字符数，但通常在此处传递令牌计数器

重复字段的意义：块之间保持一些重叠，以确保语义上下文不会在块之间丢失。在大多数情况下，固定大小的分块将是最佳方式。与其他形式的分块相比，固定大小的分块在计算上更加经济且易于使用，因为它在分块过程中不需要使用任何NLP库。

下面我们看个例子，在这个例子中我们要对一个字符串“abcdefghijklmnopqrstuvwxyz”进行分割：

```
from langchain.text_splitter import RecursiveCharacterTextSplitter, CharacterTextSplitter

r_splitter = RecursiveCharacterTextSplitter(
    chunk_size=26, #块长度
    chunk_overlap=4, #重叠字符串长度
    separators=["\n\n", "\n", " ", ""] #默认
)

#分割字符串
text1 = 'abcdefghijklmnopqrstuvwxyz'
r_splitter.split_text(text1)
>>> ['abcdefghijklmnopqrstuvwxyz']
```

看看另外情况：

```
r_splitter = RecursiveCharacterTextSplitter(
    chunk_size=10, #块长度
    chunk_overlap=4 #重叠字符串长度
)
>>> ['abcdefghij', 'ghijklmnop', 'mnopqrstuv', 'stuvwxyz']
```

所以，这里块长度、重叠字符串，都可以体现了，首先 `abcdefghij` 和 第二个结果 `ghijklmnop` 分别是10个，然后 `ghij` 四个是重叠的

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

这个正则可以沿用到其他的地方

### 2.2 CharacterTextSplitter

按字符数来切割文档的文档切割组件,默认情况下是 `\n\n`

来看例子：

```
c_splitter = CharacterTextSplitter(
    chunk_size=5,
    chunk_overlap=4,
    separator = '\n\n'
)

text3 = "a b c d e f g h i j k l m n o p q r s t u v w x y z"
c_splitter.split_text(text3)
>>> ['a b c d e f g h i j k l m n o p q r s t u v w x y z']
```

```
c_splitter = CharacterTextSplitter(
    chunk_size=10,
    chunk_overlap=4,
    separator = ' '
)
>>> ['a b c d e',
 'd e f g h',
 'g h i j k',
 'j k l m n',
 'm n o p q',
 'p q r s t',
 's t u v w',
 'v w x y z']
```

这里要费点笔墨，此时可以看到上面 `chunk_size=5` 却没有按照`chunk_size`进行分割，是因为一定要触发 `separator` 的规则才会根据`size`进行分割；

所以，后面 `separator` 改成空格就会根据 `chunk_size` 进行分割了

### 2.3 TokenTextSplitter

`TokenTextSplitter` 是一种字节对编码（Byte Pair Encoder，BPE）方法，而BPE是一种自然语言处理领域中被经常使用的数据压缩算法。`TokenTextSplitter` 来自 `tiktoken` 库，`tiktoken` 库最先被用在 `GPT-2` 中，并随之被开源。我们首先介绍 `LangChain` 中内置的 `TokenTextSplitter` 实现和 `TokenTextSplitter` 中可以调用的 `tokenizer` 方法。

`Langchain`还提供了按token来分割文本的方法，所谓token可以理解为含义简单的最小词语单位，这里一个token大约由4个字符组成。因为大型语言模型(LLM),通常是以token的数量作为其计量(或收费)的依据。所以采用token分割也有助于我们在使用像chatGPT这样的LLM的同时更好的控制成本。

```
from langchain.text_splitter import TokenTextSplitter

text_splitter = TokenTextSplitter(chunk_size=1, chunk_overlap=0)

text1 = "foo bar bazzyfoo"
text_splitter.split_text(text1)
>>> ['foo', ' bar', ' b', 'az', 'zy', 'foo']
```

在使用token分割文本时英语中的一些非常用单词如这里的`bazzyfoo`会被分割成多个token,如这里将`bazzyfoo`分割成了4个token,它们分别是`b`, `az`, `zy`, `foo`。

另一种形态是 `tiktoken`

`tiktoken` 是由 `OpenAI` 开源的快速 `BPE` 分词器。

可以使用它来估算使用的token。对于 `OpenAI` 模型来说，它可能更准确。

1. 文本如何拆分：按照传入的字符
1. 块大小如何测量：通过 `tiktoken` 分词器

使用前需要先安装：

```
pip install tiktoken
```

需要配合`CharacterTextSplitter`来使用：

```
from langchain.text_splitter import CharacterTextSplitter


# 根据token拆分文本
text_splitter = CharacterTextSplitter.from_tiktoken_encoder(
    chunk_size=2, chunk_overlap=0,encoding_name = 'gpt2'
)
text = 'abcdefghi jklmno pqrstuvwxyz'
texts = text_splitter.split_text(text)

print(texts[0])
>>> abcdefghi jklmno pqrstuvwxyz

# 修改一下chunk_size
text_splitter = CharacterTextSplitter.from_tiktoken_encoder(
    chunk_size=20, chunk_overlap=0,encoding_name = 'gpt2'
)
text = 'abcdefghi jklmno pqrstuvwxyz'
texts = text_splitter.split_text(text)

print(texts[0])
>>> abcdefghi jklmno pqrstuvwxyz
```

可以看到这里其实先根据空格进行分割，然后如果分割后，为一个单词合并在一起，则会认为是一个单词，不会分开

在文章 [FurtheBBMachine：分词 -- 从源码解读LangChain-ChatGLM(二)](https://zhuanlan.zhihu.com/p/638929185) 提到：

`TokenTextSplitter` 主要可以通过两种方法初始化 `encoder`：
