
# 模块化方法构建 RAG 系统

> 来源：[https://v11enp9ok1h.feishu.cn/docx/X2Qtdv1D6owNP7xtlvOcsvssnre](https://v11enp9ok1h.feishu.cn/docx/X2Qtdv1D6owNP7xtlvOcsvssnre)

## 数据集准备

> 在深入构建 RAG 系统之前，我们需要准备一个数据集。

> 在本教程中，我们将使用 SQuAD 数据集（斯坦福问答数据集），其中包含问题、上下文和答案。

以下是加载和预处理该数据集的代码：

```
from datasets import load_dataset

# Step 1: Load the SQuAD dataset
dataset = load_dataset("squad")

# Step 2: Extract unique contexts from the dataset
data = [item["context"] for item in dataset["train"]]
texts = list(set(data))
```

首先，我们使用该库下载数据集`datasets`（确保它已安装— `pip install datasets`）。

在这个数据集中，每个上下文都有多个不同的问答对。这就是为什么我们的`data`对象会有重复的上下文。因此，我们只提取唯一`context`字段。这些单独的上下文稍后会被嵌入并存储在向量数据库中。

![](img/c8f06177a2e88b615dab87e521ae83ad.png)

打印列表的一个元素`texts`，我们得到：

```
>>> texts[0]

"""In Ireland comprehensive schools were an earlier model of 
state schools, introduced in the late 1960s and largely 
replaced by the secular community model of the 1970s. The 
comprehensive model generally incorporated older schools 
that were under Roman Catholic or Protestant ownership, and 
the various denominations still manage the school as patrons 
or trustees. The state owns the school property, which is 
vested in the trustees in perpetuity. The model was adopted 
to make state schools more acceptable to a largely conservative
society of the time."""
```

## 嵌入数据集

现在我们有了数据集，我们需要创建它的嵌入。因此，在本例中，我们将在上下文级别定义一个嵌入，也就是说，上面`texts`列表中的每个元素都将嵌入到一个向量中。

![](img/518a3eb9bcbe2f308b092a7f302d7368.png)

我们将把这个功能封装到一个`EmbedData`类中。这个类将：

1. 处理嵌入模型的加载。
2. 提供批量处理数据集的方法，以实现高效的嵌入生成。
3. 存储生成的嵌入以供检索系统使用。

首先，让我们定义具有所有必要的实例级属性的`EmbedData`类和方法。`__init__`

```
class EmbedData:

    def __init__(self, 
                 embed_model_name="nomic-ai/nomic-embed-text-v1.5",
                 batch_size=32):
        
        self.embed_model_name = embed_model_name
        self.embed_model = self._load_embed_model()
        self.batch_size = batch_size
        self.embeddings = []
```

在上面的代码中：

- `self.embed_model_name`存储嵌入模型的名称以供参考。
- `self.embed_model`将存储我们即将定义的方法返回的嵌入模型，该方法将加载嵌入模型并返回它。
- `self.batch_size`指定嵌入的每个批次的大小。
- `self.embeddings`是一个用于存储生成的嵌入的列表。

接下来我们定义加载嵌入模型的方法：

```
from llama_index.embeddings.huggingface import HuggingFaceEmbedding

class EmbedData:

    def __init__(...):
        ...
        
    def _load_embed_model(self):
        embed_model = HuggingFaceEmbedding(model_name=self.embed_model_name,
                                           trust_remote_code=True,
                                           cache_folder='./hf_cache')
        return embed_model
```

虽然上面的部分直观且不言自明，`cache_folder='./hf_cache'` 指定了一个本地文件夹来缓存模型以供将来使用，这样我们以后就不必重新下载它。

接下来，我们需要一种方法来实际使用上述嵌入模型，并为一批数据生成嵌入。`generate_embedding`下面定义的方法可以实现这一点：

```
class EmbedData:

    def __init__(...):
        ...
        
    def _load_embed_model(self):
        ...
    
    def generate_embedding(self, context):
        return self.embed_model.get_text_embedding_batch(context)
```

这里的参数`context`是我们之前看到的段落列表。

`get_text_embedding_batch`该方法接受列表并使用嵌入模型的方法返回嵌入列表。

最后，我们有一种`embed`方法，通过批处理该过程并为每个批次调用`generate_embedding`方法来协调所有上下文的嵌入过程。

```
def batch_iterate(lst, batch_size):
    for i in range(0, len(lst), batch_size):
        yield lst[i : i + batch_size]

class EmbedData:

    ...
        
    def embed(self, contexts):
        self.contexts = contexts
        
        for batch_context in tqdm(batch_iterate(contexts, self.batch_size),
                                  total=len(contexts)//self.batch_size,
                                  desc="Embedding data in batches"):
                                  
            batch_embeddings = self.generate_embedding(batch_context)
            
            self.embeddings.extend(batch_embeddings)
```

- 首先，我们将的完整列表存储`contexts`为属性（`self.contexts`）。
- 接下来，我们使用函数`batch_iterate`分批迭代列表。对于每个批次
- 我们调用它`generate_embedding`来创建嵌入。
- 我们将生成的嵌入附加到`self.embeddings`列表中。
- 该过程完成后，`self.embeddings`包含所有输入上下文的向量表示。

快完成了！

最后，我们按如下方式实例化此类并生成嵌入：

```
batch_size = 32

embeddata = EmbedData(batch_size=batch_size)

embeddata.embed(texts)
```
