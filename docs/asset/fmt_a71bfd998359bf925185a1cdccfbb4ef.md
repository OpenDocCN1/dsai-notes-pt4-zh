
```python
text = "..." # your text

from langchain.text_splitter import CharacterTextSplitter

text_splitter = CharacterTextSplitter(
    separator = "\n\n",
    chunk_size = 256,
    chunk_overlap = 20
)

docs = text_splitter.create_documents([text])
```

在此基础上，结合它们的优劣势衍生了内容定义分块算法（CDC）、滑动窗口分块算法（SWC）和两阈值两除法分块算法（TTTD）。FSC 将文本分割成大小相同的块，而不考虑内容。这种方法简单快捷，但由于边界偏移问题，重复数据删除效率较低。CDC 根据内容的某些特征（如标点符号或哈希值）分割文本。这种方法可以实现更高的重复数据删除效率，但需要更多的计算和存储。SWC 在文本上滑动一个窗口，并在窗口内满足特定条件时标记一个块边界。这种方法可以适应不同的数据特征，但可能会生成不同大小的块。TTTD 使用两个不同的阈值和除数将文本分成两个层次：粗粒度块和细粒度块。这种方法可以在重复数据删除效率和计算成本之间取得平衡。

## 2）句子级分块

句子级分块，以句子的粒度进行拆分，最简单的方法是用句号（`.`）和新行分割句子。虽然这种方法既快又简单，但却没有考虑到所有可能的边角问题。为了解决这一情况可以使用一些 NLP 的技术感知到文本内容语义，从而更好地保留所产生的语块的上下文。`langchain` 中内置了很多句子拆分的工具包。比如 `NLTK` 和 `spaCy`。在 `langchain` 中的使用方法：

```python
text = "..." # your text

from langchain.text_splitter import NLTKTextSplitter

text_splitter = NLTKTextSplitter()

docs = text_splitter.split_text(text)
```

## 3）递归分块

递归分块，递归分块法使用一组分隔符，以分层和迭代的方式将输入文本分成较小的块。如果初次尝试分割文本时没有产生所需的大小或结构的分块，该方法就会使用不同的分隔符或标准对产生的分块进行递归调用，直到达到所需的分块大小或结构。这意味着，虽然分块的大小不会完全相同，但它们仍会倾向于达到相似的大小。在 `langchain` 中的使用方法：

```python
text = "..." # your text

from langchain.text_splitter import RecursiveCharacterTextSplitter

text_splitter = RecursiveCharacterTextSplitter(
    # Set a really small chunk size, just to show.
    chunk_size = 256,
    chunk_overlap = 20
)

docs = text_splitter.create_documents([text])
```

## 4）特定格式分块

针对特定格式的文本，如 Markdown 和 LaTeX，保留内容的原始结构，例如识别 Markdown 语法或解析 LaTeX 命令和环境。

```python
from langchain.text_splitter import MarkdownTextSplitter

markdown_text = "..."

markdown_splitter = MarkdownTextSplitter(chunk_size=100, chunk_overlap=0)

docs = markdown_splitter.create_documents([markdown_text])
```

除上面常见的分块方法之外，还有一些其它方法在探索，比如语义聚类方法分块方法，利用文本中蕴含的内在含义来指导分块。这些方法可能会利用机器学习算法来辨别上下文并推断文本的自然划分。

## 策略选择

刚才我们了解到分块有很多的方法，每种方法也有很多策略可以设置，那么什么样的策略最适合，就需要进行评估，从而找到最佳答案。以最常见的固定分块方法找出合适分块大小为例，整个过程大致分为三步：

### 1）预处理数据

在确定应用程序的最佳块大小之前，您需要先预处理数据以确保质量。例如，如果数据是从网络上获取的，就可能需要移除 HTML 标记或只会增加噪音的特定元素。

### 2）选择一定范围的块大小

数据预处理完成后，下一步就是选择一定范围的潜在块大小进行测试。如前所述，选择时应考虑内容的性质（如短信息或长文档）、将使用的嵌入模型及其功能（如标记限制）。目的是在保留上下文和保持准确性之间找到平衡。首先要探索各种块的大小，包括用于捕获更细粒度语义信息的较小块（如 128 或 256 token）和用于保留更多上下文的较大块（如 512 或 1024 token）。

### 3）评估每种分块大小的性能

为了测试各种分块大小，可以在向量数据库中使用多个索引或具有多个命名空间的单个索引。使用具有代表性的数据集，为要测试的数据块大小创建 `embeddings`，并将其保存在索引（或多个索引）中。然后，您可以运行一系列可以评估质量的查询，并比较不同块大小的性能。这是一个迭代的过程，在这个过程中，你会针对不同的查询测试不同的块大小，直到你能根据内容和预期查询确定性能最好的块大小。

`Chunking` 为后续步骤保证检索质量起奠基作用，但经过上面的介绍，可以看出，`chunking` 本身是一个权衡的产物，再好的策略也无法做到百利无一害，为了克服因为 `chunking` 带来的负面影响，需要一些全局化、系统化策略协同起来才能进一步提高生成质量，比如，`llamaindex` 等框架为 `chunk` 增加描述性 `metadata`，以及精心设计索引结构，比如 `treeindex` 等，进而解决因为 `chunking` 导致的跨 `chunk` 的上下文丢失问题，也因此成为大模型应用研究的热点之一。
