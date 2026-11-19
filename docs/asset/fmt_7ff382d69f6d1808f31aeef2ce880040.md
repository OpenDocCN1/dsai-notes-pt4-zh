
# 从零开始编写一个简单的 RAG

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Uw48dviHqoNcekxqOaMc6ZTPn6b](https://v11enp9ok1h.feishu.cn/docx/Uw48dviHqoNcekxqOaMc6ZTPn6b)

## 什么是 RAG

首先，让我们看一个没有 RAG 的简单聊天机器人系统

![](img/cefa25919002975fcf057510f3822fae.png)

虽然聊天机器人可以根据其训练数据集回答常见问题，但它可能无法获取最新或特定领域的知识。

> 一个现实世界的例子是问 ChatGPT“我妈妈的名字是什么？”。ChatGPT 无法回答这个问题，因为它无法访问外部知识，比如你的家庭成员信息。

![](img/a795331d91908eb5de95b225669a3b48.png)

为了解决这个限制，我们需要向模型提供外部知识（在这个例子中，是一个家庭成员姓名的列表）。

![](img/9ef87538e0a24606363b5cf849820680.png)

一个 RAG 系统由两个关键组件组成

+ 一个检索模型，它从外部知识源获取相关信息，知识源可以是数据库、搜索引擎或任何其他信息库。

+ 一个语言模型，它根据检索到的知识生成回应。

实现 RAG 的方法有多种，包括 (Graph RAG)、(Hybrid RAG) 和 (Hierarchical RAG)，我们将在本文末尾讨论这些。

## 简单的 RAG

创建一个简单的 RAG 系统，它可以从一个预定义的数据集中检索信息，并根据检索到的知识生成回应。该系统将包括以下组件

![](img/4cf44862f4dc19ec2f61a828c3e235b4.png)

1. **嵌入模型：**一个预训练的语言模型，将输入文本转换为嵌入（embedding）——即捕捉语义的向量表示。这些向量将用于在数据集中搜索相关信息。

2. **向量数据库：**一个用于存储知识及其对应嵌入向量的系统。虽然有许多向量数据库技术，如 `Qdrant`、`Pinecone` 和 `pgvector`，但我们将从零开始实现一个简单的内存数据库。

3. **聊天机器人：**一个根据检索到的知识生成回应的语言模型。这可以是任何语言模型，如 Llama、Gemma 或 GPT。

### 索引阶段

索引阶段是创建 RAG 系统的第一步。它涉及将数据集（或文档）分解成小的数据块 (chunks)，并为每个数据块计算一个向量表示，以便在生成过程中能被高效地搜索。

![](img/cd785fc7af6bae79e16e52c53ec36c42.png)

每个数据块的大小可以根据数据集和应用而变化。例如，在文档检索系统中，每个数据块可以是一个段落或一个句子。在对话系统中，每个数据块可以是一轮对话。

在索引阶段之后，每个数据块及其对应的嵌入向量将被存储在向量数据库中。下面是一个索引后向量数据库可能的样子：

| **数据块** | **嵌入向量** |
| --- | --- |
| 意大利和法国生产了世界上超过 40% 的葡萄酒。 | `[0.1, 0.04, -0.34, 0.21, ...]` |
| 印度的泰姬陵完全由大理石建成。 | `[-0.12, 0.03, 0.9, -0.1, ...]` |
| 世界上 90% 的淡水在南极洲。 | `[-0.02, 0.6, -0.54, 0.03, ...]` |
| ... | ... |

嵌入向量稍后可用于根据给定的查询检索相关信息。可以把它想象成 `SQL` 的 `WHERE` 子句，但我们不是通过精确的文本匹配来查询，而是可以根据它们的向量表示来查询一组数据块。

![](img/8b65588e4cceb64118dc5df3dd93f029.png)

为了比较两个向量的相似度，我们可以使用余弦相似度、欧几里得距离或其他距离度量。在这个例子中，我们将使用余弦相似度。以下是两个向量 `A` 和 `B` 的余弦相似度公式：

如果你不熟悉上面的公式，别担心，我们将在下一节中实现它。

### 检索阶段

在下图中，我们将以一个来自用户的给定输入查询为例。然后我们计算查询向量来表示该查询，并将其与数据库中的向量进行比较，以找到最相关的数据块。

向量数据库返回的结果将包含与查询最相关的 `N` 个数据块。这些数据块将被聊天机器人用来生成回应。

![](img/8354bd8b19aecbfd552f389e5807b88c.png)

## 开始编码

在这个例子中，我们将用 `Python` 编写一个简单的 RAG 实现。

为了运行模型，我们将使用 [`ollama`](https://ollama.ac.cn/)，这是一个命令行工具，可以让你运行来自 Hugging Face 的模型。有了 `ollama`，你不需要访问服务器或云服务来运行模型。你可以直接在你的电脑上运行模型。

对于模型，我们使用以下这些：

+ 嵌入模型：[`hf.co/CompendiumLabs/bge-base-en-v1.5-gguf`](https://hugging-face.cn/CompendiumLabs/bge-base-en-v1.5-gguf)

+ 语言模型：[`hf.co/bartowski/Llama-3.2-1B-Instruct-GGUF`](https://hugging-face.cn/bartowski/Llama-3.2-1B-Instruct-GGUF)

至于数据集，我们将使用 [一个关于猫的事实的简单列表](https://hugging-face.cn/ngxson/demo_simple_rag_py/blob/main/cat-facts.txt)。每个事实在索引阶段将被视为一个数据块。

### 下载 ollama 和模型

首先，让我们从 [项目网站](https://ollama.ac.cn/) 安装 `ollama`：`ollama.com`

安装后，打开一个终端并运行以下命令以下载所需的模型

```
ollama pull hf.co/CompendiumLabs/bge-base-en-v1.5-gguf
ollama pull hf.co/bartowski/Llama-3.2-1B-Instruct-GGUF
```

如果你看到以下输出，说明模型已成功下载

```
pulling manifest
...
verifying sha256 digest
writing manifest
success
```

在继续之前，为了在 `python` 中使用 `ollama`，我们还需要安装 `ollama` 包

```
pip install ollama
```

### 加载数据集

接下来，创建一个 Python 脚本并将数据集加载到内存中。该数据集包含一系列关于猫的事实，这些事实将在索引阶段用作数据块。

你可以从 [这里](https://hugging-face.cn/ngxson/demo_simple_rag_py/blob/main/cat-facts.txt) 下载示例数据集。下面是加载数据集的示例代码

```
dataset = []
with open('cat-facts.txt', 'r') as file:
  dataset = file.readlines()
  print(f'Loaded {len(dataset)} entries')
```

### 实现向量数据库

使用 `ollama` 的嵌入模型将每个数据块转换为一个嵌入向量，然后将数据块及其对应的向量存储在一个列表中。

下面是一个计算给定文本嵌入向量的示例函数：

```
import ollama

EMBEDDING_MODEL = 'hf.co/CompendiumLabs/bge-base-en-v1.5-gguf'
LANGUAGE_MODEL = 'hf.co/bartowski/Llama-3.2-1B-Instruct-GGUF'
# Each element in the VECTOR_DB will be a tuple (chunk, embedding)
# The embedding is a list of floats, for example: [0.1, 0.04, -0.34, 0.21, ...]
VECTOR_DB = []

def add_chunk_to_database(chunk):
  embedding = ollama.embed(model=EMBEDDING_MODEL, input=chunk)['embeddings'][0]
  VECTOR_DB.append((chunk, embedding))
```

在这个例子中，为简单起见，我们将数据集中的每一行视为一个数据块。

```
for i, chunk in enumerate(dataset):
  add_chunk_to_database(chunk)
  print(f'Added chunk {i+1}/{len(dataset)} to the database')
```

### 检索函数

接下来，让我们实现检索函数，它接受一个查询并根据余弦相似度返回前 `N` 个最相关的数据块。我们可以想象，两个向量之间的余弦相似度越高，它们在向量空间中就越“接近”。这意味着它们在意义上更相似。

下面是计算两个向量之间余弦相似度的示例函数：

```
def cosine_similarity(a, b):
  dot_product = sum([x * y for x, y in zip(a, b)])
  norm_a = sum([x ** 2 for x in a]) ** 0.5
  norm_b = sum([x ** 2 for x in b]) ** 0.5
  return dot_product / (norm_a * norm_b)
```

实现检索函数：

```
def retrieve(query, top_n=3):
  query_embedding = ollama.embed(model=EMBEDDING_MODEL, input=query)['embeddings'][0]
  # temporary list to store (chunk, similarity) pairs
  similarities = []
  for chunk, embedding in VECTOR_DB:
    similarity = cosine_similarity(query_embedding, embedding)
    similarities.append((chunk, similarity))
  # sort by similarity in descending order, because higher similarity means more relevant chunks
  similarities.sort(key=lambda x: x[1], reverse=True)
  # finally, return the top N most relevant chunks
  return similarities[:top_n]
```

### 生成阶段

在这个阶段，聊天机器人将根据上一步检索到的知识生成一个回应。这只需将数据块添加到将作为聊天机器人输入的提示 (prompt) 中即可完成。

例如，可以像这样构建一个提示：

```
input_query = input('Ask me a question: ')
retrieved_knowledge = retrieve(input_query)

print('Retrieved knowledge:')
for chunk, similarity in retrieved_knowledge:
  print(f' - (similarity: {similarity:.2f}) {chunk}')

instruction_prompt = f'''You are a helpful chatbot. Use only the following pieces of context to answer the question. Don't make up any new information: {'\n'.join([f' - {chunk}' for chunk, similarity in retrieved_knowledge])}'''
```

然后我们使用 `ollama` 来生成回应。在这个例子中，我们将使用 `instruction_prompt` 作为系统消息。

```
stream = ollama.chat(
  model=LANGUAGE_MODEL,
  messages=[
    {'role': 'system', 'content': instruction_prompt},
    {'role': 'user', 'content': input_query},
  ],
  stream=True,
)

# print the response from the chatbot in real-time
print('Chatbot response:')
for chunk in stream:
  print(chunk['message']['content'], end='', flush=True)
```

## 整合所有部分

你可以在 [这个文件](https://hugging-face.cn/ngxson/demo_simple_rag_py/blob/main/demo.py) 中找到最终代码。要运行代码，请将其保存为名为 `demo.py` 的文件并运行以下命令：

```
python demo.py
```

你现在可以向聊天机器人提问了，它将根据从数据集中检索到的知识生成回应。

```
Ask me a question: tell me about cat speed
Retrieved chunks: ...
Chatbot response:
According to the given context, cats can travel at approximately 31 mph (49 km) over a short distance. This is their top speed.
```

## 可改进的空间

目前为止，我们已经用一个小数据集实现了一个简单的 RAG 系统。然而，仍然存在许多局限性：

+ 如果问题同时涉及多个主题，系统可能无法提供好的答案。这是因为系统仅根据查询与数据块的相似性来检索数据块，而没有考虑查询的上下文。

解决方案可以是让聊天机器人根据用户的输入编写自己的查询，然后根据生成的查询检索知识。我们也可以使用多个查询来检索更多相关信息。

+ 前 `N` 个结果是根据余弦相似度返回的。这并不总能得到最好的结果，特别是当每个数据块包含大量信息时。

为了解决这个问题，我们可以使用重排模型 (reranking model) 来根据与查询的相关性对检索到的数据块进行重新排序。

+ 数据库存储在内存中，对于大型数据集可能不具备可扩展性。我们可以使用更高效的向量数据库，如 `Qdrant`、`Pinecone`、`pgvector`。

+ 我们目前将每个句子视为一个数据块。对于更复杂的任务，我们可能需要使用更复杂的技术来将数据集分解成更小的数据块。我们也可以在将每个数据块添加到数据库之前对其进行预处理。

+ 本例中使用的语言模型是一个只有 1B 参数的简单模型。对于更复杂的任务，我们可能需要使用更大的语言模型。

## 其他类型的 RAG

在实践中，实现 RAG 系统的方法有很多。以下是一些常见的 RAG 系统类型：

+ (Graph RAG)：在这种类型的 RAG 中，知识源被表示为一个图，其中节点是实体，边是实体之间的关系。语言模型可以遍历图来检索相关信息。关于这类 RAG 有许多活跃的研究。[这里是关于图 RAG 的论文集。](https://hugging-face.cn/collections/graphrag/graphrag-papers-667566a057208377a1489c82)

+ (Hybrid RAG)：一种结合了知识图谱 (KGs) 和向量数据库技术以改进问答系统的 RAG。要了解更多信息，你可以阅读 [这篇论文](https://arxiv.org/html/2408.04948v1)。

+ RAG (Modular RAG)：一种超越了基本的“检索后生成”过程的 RAG，它采用路由、调度和融合机制来创建一个灵活且可重新配置的框架。这种模块化设计允许各种 RAG 模式（线性、条件、分支和循环），从而实现更复杂和适应性强的知识密集型应用。要了解更多信息，你可以阅读 [这篇论文](https://arxiv.org/html/2407.21059v1)。

---

RAG 将信息检索与文本生成相结合，通过整合外部知识源来增强语言模型的性能。这种方法在各种应用中都显示出有希望的结果，例如问答、对话系统和内容生成。

在这篇博文中，我们将探讨 RAG，并使用 `Python` 和 `ollama` 从零开始构建一个简单的 RAG 系统。这个项目将帮助你理解 RAG 系统的关键组件，以及如何使用基本的编程概念来实现它们。
