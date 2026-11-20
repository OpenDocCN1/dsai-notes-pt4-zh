
# 商用级RAG知识库智能问答系统构建

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/WvXFdxJrwoxdhAx5VEVcOLWUnfg](https://qqs7y1hozd1.feishu.cn/docx/WvXFdxJrwoxdhAx5VEVcOLWUnfg)

## 一、RAG

检索增强生成（`RAG`）通过从外部知识库检索相关文档`chunk`并进行语义相似度计算，增强了`LLM`的功能。

> 通过引用外部知识，`RAG`有效地减少了生成事实不正确内容的问题。

![](img/7560258cd37b3c6465a7a0341efffaa2.png)

## 二、RAG架构

### 2.1 RAG实现过程

`RAG`在问答系统中的一个典型应用主要包括三个步骤：

+ `Indexing（索引）：`将文档分割成`chunk`，编码成向量，并存储在向量数据库中。
+ `Retrieval（检索）：`根据语义相似度检索与问题最相关的前`k`个`chunk`。
+ `Generation（生成）：`将原始问题和检索到的`chunk`一起输入到`LLM`中，生成最终答案。

### 2.2 RAG在线检索架构

![](img/9a0b1043087f3f592faa65f376aad4fd.png)

## 三、RAG流程

接下来，将深入拆解`RAG`各个流程，并为`RAG`构建技术路线图。

### 3.1 索引

索引是将文本分解成可管理的`chunk`的过程，是组织系统的关键步骤，面临三个主要挑战：

+ 不完整的内容表示：`chunk`的语义信息受到分割方法的影响，导致在更长的上下文中重要信息的丢失或隐藏。
+ 不准确的`chunk`相似性搜索：随着数据量的增加，检索中的噪声增多，导致频繁与错误数据匹配，使检索系统变得脆弱和不可靠。
+ 不明确的引用轨迹：检索到的`chunk`可能来源于任何文档，缺乏引用路径，但包含的内容完全不同的主题。

#### 3.1.1 Chunking

> `Transformer`模型有固定的输入序列长度。

> 所以需要对数据进行分块，将初始文档分割成一定大小的`chunk`，同时不丢失它们的意义（将文本分割成句子或段落，而不是将一个句子分成两部分）。

有多种文本切分策略能够完成这项任务，采用以下3种策略：

> `直接分段：`
>
> 将文本按照一定的规则进行分段处理后，转成可以进行语义搜索的格式。这里不需要调用模型进行额外处理，成本低，适合绝大多数应用场景。
>
> `生成问答对：`
>
> 根据一定的规则，将文本拆成一段主题文本，调用`LLM`为该段主题文本生成问答对。这种处理方式有非常高的检索精度，但是会丢失部分文本细节，需要特别留意。
>
> `增强信息：`
>
> 通过子索引以及调用`LLM`生成相关问题和摘要，来增加`chunk`的语义丰富度，更加有利于后面的检索。不过需要消耗更多的存储空间和增加`LLM`调用开销。

`chunk`的大小是一个需要重点考虑的参数。

> 标准的`Transformer`编码器模型，如基于`BERT`的`Sentence Transformer`最多处理512个`token`，而`OpenAI`的`text-embedding-3-small`能够处理更长的序列（8191个`token`）。

> `LLM`提供足够的上下文以进行推理，同时给搜索提供足够具体的文本嵌入。

> 较大的`chunk`可以捕获更多的上下文，但它们也会产生更多的噪音，需要更长的处理时间和更高的成本。

> 而较小的`chunk`可能无法完全传达必要的上下文，但它们的噪音较少。

##### 直接分段：

切分后的`chunk`信息，总共10个`chunk`：

![](img/7ceb8df80d7fb8390fea240f9d66bfdd.png)

```
def split_long_section(section, max_length=1300):
    lines = section.split('\n')
    current_section = ""
    result = []
    for line in lines:
        # Add 1 for newline character when checking the length
        if len(current_section) + len(line) + 1 > max_length:
            if current_section:
                result.append(current_section)
                current_section = line  # Start a new paragraph
            else:
                # If a single line exceeds max length, treat it as its own paragraph
                result.append(line)
        else:
            if current_section:
                current_section += '\n' + line
            else:
                current_section = line
```

##### 生成问答对：

切分后的`chunk`信息，总共28个`chunk`，每个`chunk`包含一对问答：

![](img/e9d56951b370c38686cda6d84ac415d4.png)

##### 切分后的某个chunk的问答对信息：

![](img/b76dd81ead8fb0b135fee029d4b95475.png)

##### 增强信息：

切分后的`chunk`信息，总共6个`chunk`，每个`chunk`都包含一批数据索引信息：

切分后的某个`chunk`的数据索引信息：

![](img/9dfc842cc7a16c183017c69a69becd79.png)

##### 3.1.1.1 滑动窗口

> 平衡这些需求的一种简单方法是使用重叠的`chunk`。

> 通过使用滑动窗口，可以增强语义过渡。

> 然而，也存在一些限制，包括对上下文大小的控制不精确、有截断单词或句子的风险，以及缺乏语义考虑。

```
final_result = []
ast_lines = ""
for section in result:
    lines = section.split('\n')
    last_two_lines = "\n".join(lines[-2:])  # Extract the last two lines
    combined_section = last_lines + "\n" + section if last_lines else section
    final_result.append(combined_section)
    last_lines = last_two_lines
```

##### 3.1.1.2 上下文丰富化

> 获得更好的搜索质量而检索较小的`chunk`，添加周围的上下文供`LLM`进行推理。

> 有两个选项：通过在较小的检索`chunk`周围添加句子来扩展上下文，或者将文档递归地分成多个较大的父`chunk`，其中包含较小的子`chunk`。

###### 句子窗口检索

> 提供查询与上下文余弦距离搜索的高准确性。

> 为了在获取到最相关的单个句子后更好地推理出找到的上下文，我们通过在检索到的句子之前和之后添加`k`个句子来扩展上下文窗口，然后将这个扩展后的上下文发送给`LLM`。

```
from llama_index import ServiceContext, VectorStoreIndex, StorageContext
from llama_index.node_parser import SentenceWindowNodeParser

def build_sentence_window_index(
    document, llm, vector_store, embed_model="local:BAAI/bge-small-en-v1.5"
):
    # create the sentence window node parser w/ default settings
    node_parser = SentenceWindowNodeParser.from_defaults(
        window_size=3,
        window_metadata_key="window",
        original_text_metadata_key="original_text",
    )
    sentence_context = ServiceContext.from_defaults(
        llm=llm,
        embed_model=embed_model,
        node_parser=node_parser
    )
    storage_context = StorageContext.from_defaults(vector_store=vector_store)
    sentence_index = VectorStoreIndex.from_documents(
        [document], service_context=sentence_context, storage_context=storage_context
    )

    return sentence_index
```

###### 父文档检索器

> 文档被分割成一个层次结构的`chunk`，然后最小的叶子`chunk`被发送到索引中。

> 在检索时，检索`k`个叶子`chunk`，如果有`n`个`chunk`引用同一个父`chunk`，将它们替换为该父`chunk`并将其发送给`LLM`进行答案生成。

> 关键思想是将用于检索的`chunk`与用于合成的`chunk`分开。

> 具体来说，一种方法可以涉及检索较小的`chunk`，然后引用父ID以返回较大的`chunk`。或者，可以检索单个句子，并返回该句子周围的文本窗口。

![](img/caf5bdf1267c3dea6807cfdfc4d6fd35.png)

```
sub_chunk_sizes = [128, 256, 512]
sub_node_parsers = [
    SimpleNodeParser.from_defaults(chunk_size=c) for c in sub_chunk_sizes
]

all_nodes = []
for base_node in base_nodes:
    for n in sub_node_parsers:
        sub_nodes = n.get_nodes_from_documents([base_node])
        sub_inodes = [
            IndexNode.from_text_node(sn, base_node.node_id) for sn in sub_nodes
        ]
        all_nodes.extend(sub_inodes)

    # also add original node to node
    original_node = IndexNode.from_text_node(base_node, base_node.node_id)
    all_nodes.append(original_node)
all_nodes_dict = {n.node_id: n for n in all_nodes}
```

##### 3.1.1.3 元数据附加

> 可以使用元数据信息对`chunk`进行丰富。

> 例如URL、文件名、作者、时间戳、摘要，或者`chunk`可以回答的问题。

随后，可以根据这些元数据对检索进行筛选，限制搜索范围。

```
async def aadd_content_embedding(self, data):
    for item in data:
        documents_to_add = []  # Initialize a list to hold all document parts for batch processing.
        timestamp = int(time.time())
        doc_id, url, chunk_text_vec = item
        part_index = 0
        for part_content in chunk_text_vec:
            # Process each part of the content.
            # Construct metadata for each document part, ensuring each part has a unique ID.
            metadata = {"source": url, "id": f"{doc_id}-part{part_index}"}
            # Create a Document object with the part content and metadata.
            doc = Document(page_content=part_content, metadata=metadata)
            # Add the document part to the list for batch addition.
            documents_to_add.append(doc)
            part_index += 1

        # Check if there are document parts to add.
        if documents_to_add:
            # Add all document parts to Chroma in a single batch operation.
            embedding_id_vec = await self.chroma_obj.aadd_documents(documents_to_add)
            logger.info(f"[DOC_EMBEDDING] doc_id={doc_id}, url={url} added {len(documents_to_add)} document parts to Chroma., embedding_id_vec={embedding_id_vec}")
```

#### 3.1.2 向量化

在构建`RAG`应用程序时，“使用哪个`Embedding`模型”没有一个适用于所有情况的标准答案。

> 实践中，选择的是`OpenAI`的`text-embedding-3-small`来计算`chunk`的向量。

```
from langchain_openai import OpenAIEmbeddings

OPENAI_EMBEDDING_MODEL_NAME = os.getenv('OPENAI_EMBEDDING_MODEL_NAME', 'text-embedding-3-small')

# Initialize OpenAI embeddings with the specified model
g_embeddings = OpenAIEmbeddings(
    model=OPENAI_EMBEDDING_MODEL_NAME,
    openai_api_key=OPENAI_API_KEY
)
```

#### 3.1.3 搜索索引

##### 3.1.3.1 向量存储索引

> `RAG`流程中的关键部分是搜索索引，存储`chunk`的向量化内容。最简单的实现使用平面索引，在查询向量和所有`chunk`向量之间进行距离计算。

一个优秀的搜索索引，需要确保在大规模元素上的检索效率，一般使用某种近似最近邻实现，如聚类、树或`HNSW`算法。

**NOTE:**

如果使用`Chroma`作为向量存储DB，需要留意`hnsw:space`的默认值是`l2`，实践中建议调整为`cosine`

![](img/719c7c55faa23ef27b07e8898276761d.png)

修改`Chrome`的距离函数：

`create_collection`接受一个可选的`metadata`参数，该参数可以用来通过设置`hnsw:space`的值来自定义嵌入空间的距离方法。`class DocumentEmbedder`:
