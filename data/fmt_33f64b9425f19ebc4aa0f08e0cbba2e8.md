
# 自定义在线嵌入模型

```python
from lazyllm.module import OnlineEmbeddingModuleBase

# 配置新平台支持的api_key到全局变量
# 通过lazyllm.config.add(变量名，类型，默认值，环境变量名)进行添加
class CustomOnlineEmbeddingModule(OnlineEmbeddingModuleBase):
    def __init__(self, embed_url: str = '<new platform embedding url>',
                 embed_model_name: str = '<new platform embedding model name>'):
        super().__init__("<new_platform_name>", embed_url, lazyllm.config['new_platform_api_key'], embed_model_name)
        
    def _encapsulated_data(self, text:str, **kwargs):
        # 将数据包装为您的服务需要的数据格式
        json_data = {
            "input": [text],
            "model": self._embed_model_name
        }
        if len(kwargs) > 0:
            json_data.update(kwargs)
        return json_data

    def _parse_response(self, response: dict[str, any]):
        # 解析您的服务返回的数据，返回 embedding 部分即可
        return response['embeddings'][0]['embedding']
```

使用 LazyLLM 为文档进行嵌入时无需手动遍历，只需在实例化 `Document` 对象时传入上述定义的嵌入模型即可，方法如下所示：

```python
import lazyllm

# 定义嵌入模型
online_embed = lazyllm.OnlineEmbeddingModule()

# 加载文档
documents = lazyllm.Document('/path/to/your/document', embed=online_embed)
```

# 存储组件 - Store

LazyLLM提供了基于内存和向量数据库的`Store`存储组件，以供开发者灵活选择。处于技术探索阶段的开发者，可以直接使用系统默认的`MapStore`，将文档处理后的切片与向量一并保存在内存中，以实现从文档入库至向量检索的最快读写速度；对于处于成熟阶段的RAG系统，LazyLLM提供了多种向量数据库支持，确保数据持久稳定存储，并显著缩短RAG系统的二次启动时间，提高系统的稳定性和可扩展性。

| 组件名 | 存储介质 | 功能描述 | 适用场景 |
| --- | --- | --- | --- |
| `MapStore` | 内存 | 基于内存的存储，提供节点存储的基本能力，能以最快的速度实现数据读写 | 技术探索阶段，将少量文档处理数据保存在内存中，以技术选型与算法迭代的快速验证 |
| `ChromadbStore` | 硬盘 | 使用 `Chroma` 向量数据库，实现数据的持久化存储。 | 轻量级实验，快速POC，缩短从构思到可运行原型的研发周期 |
| `MilvusStore` | 硬盘 | 使用 `Milvus` 向量数据库，实现数据的本地存储/分布式远程存储。 | 企业级部署，适用于数据体量大，对并发、检索性能都有较高要求的场景 |

在实际的使用过程中，用户只需要在`Document`定义阶段配置`store_conf`参数，进行简单的存储配置，即可利用这两种数据库将文档处理后的数据存储于本地，并在下次系统启动时直接从本地加载这些数据，避免文档的重复入库，实现知识库持久化存储。使用`store_conf`配置`Document`存储的实现方式如下：

```python
document = lazyllm.Document(dataset_path='/path/to/your/document',
           embed=lazyllm.OnlineEmbeddingModule(),
           ...,
           store_conf=store_conf)
```

参数`store_conf`接收字典形式的存储配置，配置时需要传入`type`和`kwargs`参数，具体字段解释如下：

+   `type`：使用存储类型。如之前介绍，LazyLLM目前支持的存储类型有`map`（基于内存）、`chroma`（基于`ChromaDB`向量数据库）以及`milvus`（基于`Milvus`向量数据库）：
+   `map`：仅使用内存 `key/value` 存储暂存所有文档处理数据，系统重启后数据消失；
+   `chroma`：使用 [`ChromaDB`](https://www.trychroma.com/) 存储数据，`ChromaDB` 是一个更轻量的向量数据库，相比于 `Milvus` 能处理的数据量级有限，适合调试，更多 `ChromaDB` 相关信息可以参考 [`ChromaDB` 官方文档](https://www.trychroma.com/)；
+   `milvus`：使用 [`Milvus`](https://www.milvus-io.com/overview) 存储数据，支持海量数据的检索和存储，支持分布式部署，更适合生产级应用，更多 `Milvus` 相关信息可以参考 [`Milvus` 官方文档](https://www.milvus-io.com/overview)；
+   `kwargs`：具体存储所需的额外配置参数，同样以字典的形式输入。
+   当`type`为`chroma`时，所需配置参数为：
+   `dir`（必填）：存储数据的目录；
+   当`type`为`milvus`时，所需配置参数为：
+   `uri`（必填）：存储数据地址，可以是一个文件路径或者远程连接的url；
+   `index_kwargs`（可选）：`Milvus`数据库的索引配置，可以是一个 `dict` 或者 `list[dict]`，索引配置主要包含两个字段：索引类型`index_type`和度量类型`metric_type`（即`Index`和`Similarity`，和`LazyLLM`的定义很像）。参数如果是一个 `dict` 表示所有向量模型使用相同配置；如果需要指定不同的向量模型以不同索引配置，则可以以列表的方式传入，由 `embed_key` 指定所使用配置即可。当前只支持 `floating point embedding` 和 `sparse embedding` 两种 `embedding` 类型，分别支持的参数如下，感兴趣的开发者可以前往官网查看：
+   `floating point embedding`
+   `sparse embedding`

例如我们使用`Milvus`向量数据库作为存储后端，需要传入如下配置：

```python
milvus_store_conf = {
  'type': 'milvus',  # 指定存储后端类型
  'kwargs': {
    'uri': 'test.db',  # 存储后端地址，本例子使用的是本地文件 test.db，文件不存则创建新文件
    'index_kwargs': {  # 存储后端的索引配置
      'index_type': 'FLAT',  # 索引类型
      'metric_type': 'COSINE',  # 相似度计算方式
    }
  },
}

document = lazyllm.Document(
    dataset_path='/path/to/your/document',
    embed=embedding_model,
    store_conf=milvus_store_conf 
)
```

> 请注意，目前 LazyLLM 支持 Milvus 2.4.x，请注意您的`pymilvus` >= 2.4.11 , `milvus-lite` == 2.4.10。使用`Milvus`时，检索会根据输入配置中的索引配置进行对应算法检索，`Retriever`中无需指定`similarity`参数。

# 重排组件 - Reranker

**重排序模型**通常应用于两阶段检索算法中的第二个阶段，对应 LazyLLM 中的 `Reranker` 组件。召回重排策略的基本思想是：在初步检索后，通过一个额外的模型对检索结果进行再次排序以提高最终的效果。下图对比了直接使用检索器召回 top 3 文档和先通过检索器召回 top 5 文档再对其进行重排序返回 top 3 文档的流程。可见检索流程并没有对文档进行排序，只是将满足需求的 top 5 个文档返回，而通过重排序流程，可以对其进行更精细的排序，得到排序后的文档，使得更重要的文档在返回列表中更靠前。

![](img/297b347773f7485ee53f465fb949052e.png)

LazyLLM 分别提供了在线和离线两种重排序模型的调用途径，其中在线模型通过`OnlineEmbeddingModule(type="rerank")`进行调用，离线重排模型则仍然通过 `TrainableModule` 进行调用。使用 `Reranker` 时的可调整参数有：

+   `name` (`str`, default: `'ModuleReranker'`) ： 实现重排序时必须为 `ModuleReranker`
+   `model`（`Union[Callable, str]`） ： 实现重排序的具体模型名称或可调用对象
+   `Callable` 情形：
+   `OnlineEmbeddingModule` (`type="rerank"`)：目前支持`qwen`和`glm`的在线重排序模型，使用前需要指定 `api key`
+   `TrainableModule`(`model="str"`)：需要传入本地模型名称，常用的开源重排序模型为`bge-reranker`系列
+   `str`情形：模型名称，与上 `Callable` 情形 `TrainableModule` 对应的 `model` 参数要求相同
+   `topk`（`int`）： 最终需要返回的 k 个节点数
+   `output_format`（`str`, default: `None`）：输出格式，默认为`None`，可选值有 `'content'` 和 `'dict'`，其中 `content` 对应输出格式为字符串，`dict` 对应字典
+   `join`（`boolean`, default: `False`）：是否联合输出的 k 个节点，当输出格式为 `content` 时，如果设置该值为 `True`，则输出一个长字符串，如果设置为 `False` 则输出一个字符串列表，其中每个字符串对应每个节点的文本内容。

目前常用的开源重排序模型是 `bge-reranker`，可以通过 `TrainableModule` 启动相关服务并调用。在线模型服务商有`qwen`和`glm`提供了重排序模型服务，通过`OnlineEmbeddingModule`（`type="rerank"`）进行调用。

```python
from lazyllm import OnlineEmbeddingModule, TrainableModule
# 如果您要使用在线重排模型
# 目前 LazyLLM 支持 qwen和glm 在线重排模型，使用前请指定相应的 API key。
online_rerank = OnlineEmbeddingModule(type="rerank")
reranker = Reranker('ModuleReranker', model=online_rerank, topk=3)

# 启动本地重排序模型
offline_rerank = TrainableModule('bge-reranker-large').start()
reranker = Reranker('ModuleReranker', model=offline_rerank, topk=3)
# 或者直接传入模型名称使用本地重排序模型
reranker = Reranker('ModuleReranker', model='bge-reranker-large', topk=3)
```

对于LazyLLM RAG相关组件的更多高级用法，可以参考官方API文档：https://docs.lazyllm.ai/zh-cn/latest/

# 4.2 LazyLLM的数据流组件 - Flow

LazyLLM是一款以数据流（Data Flow）为核心的大模型应用开发框架，其中定义了大量的数据流组件，配合 LazyLLM 中提供的工具和组件，可以让您像搭积木一样搭建复杂的大模型应用。

数据流的核心设计思路是**无需您手动对数据流动进行指定**，而是通过数据流内部将上一阶段的数据直接分发给对应的下游接收组件。LazyLLM支持`Pipeline`、`Parallel`、`Switch`、`If`、`Loop`、`Diverter`、`Warp`、`Graph`等数据流。

![](img/e98cf3aad18d45c8c65b4f99ff3dee9f.png)

本节将详细为您介绍 LazyLLM 中典型的`Flow`组件：`Pipeline`、`Parallel`、`Warp`以及`Bind`，方便您在接下来的教程中理解和使用。其余的组件介绍可见API文档：https://docs.lazyllm.ai/zh-cn/latest/API%20Reference/flow/

> 注意：
>
> LazyLLM 支持数据流的 `with` 语句。`with` 语句可以让数据流定义代码更加的简洁和清晰。下面关于数据流的所有例子不仅展示了基于函数式的数据流构建，同时在右侧也对应展示了通过 `with` 语法定义的数据流，`with`语句的巧妙之处在于定义代码的缩进与目标数据流结构有一定的关联性，可以清晰的看出整体结构。

## Pipeline

`Pipeline` 是顺次执行的数据流，上一个阶段的输出成为下一个阶段的输入。`pipeline` 支持函数和仿函数（或仿函数的 `type`），其工作流如下所示：

```
input -> module1 -> ... -> moduleN -> out
```

下面是通过 `Pipeline` 执行顺序结构程序的例子, 任意函数、匿名函数以及可调用实例（含`__call__()`函数的类）都可以作为 `pipeline` 中的一员。

函数式：

```python
import lazyllm

f1 = lambda x: x * 2

def f2(input):
  return input - 1

class AddOneFunctor(object):
  def __call__(self, x): return x + 1

f3 = AddOneFunctor()

# 手动调用：
inp = 2
x1 = f1(inp)
x2 = f2(x1)
x3 = f3(x2)
out_normal = AddOneFunctor()(x3)

# 使用数据流
ppl = lazyllm.pipeline(f1, f2, f3, AddOneFunctor)
out_ppl1 = ppl(inp)

print(f"输入为{inp},手动调用输出:", out_normal)
print(f"输入为{inp},数据流输出:", out_ppl1)
```

输出：

```
输入为2,手动调用输出: 5
输入为2,数据流输出: 5
```

with式：

```python
import lazyllm

f1 = lambda x: x * 2

def f2(input):
  return input - 1

class AddOneFunctor(object):
  def __call__(self, x): return x + 1

f3 = AddOneFunctor()

# 使用with方式的数据流
with lazyllm.pipeline() as ppl:
    ppl.func1 = f1
    ppl.func2 = f2
    ppl.func3 = f3
    ppl.func4 = AddOneFunctor

inp = 2
out_ppl1 = ppl(inp)

print(f"输入为{inp},数据流输出:", out_ppl1)
```

输出：

```
输入为2,数据流输出: 5
```

[1pipeline.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/PCTfbLIIQosuMwxphPGcp3tInrh)

[1pipeline_with.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YWoVbUMy4oEHgqxvtAGcEbPsnwc)

## Parallel

`Parallel`支持我们并行调用多个`pipeline`，其数据流示意如下：

```
      /> module11 -> ... -> module1N -> out1 \
input ->  module21 -> ... -> module2N -> out2 -> (out1, out2, out3)
      \> module31 -> ... -> module3N -> out3 /
```
