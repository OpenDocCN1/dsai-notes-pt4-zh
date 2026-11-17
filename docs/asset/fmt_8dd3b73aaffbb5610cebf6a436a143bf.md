
## 五）以图搜图

激动人心的时刻到了，终于可以看到结果了！~

运行下方代码：

```python
import redis
from redis.commands.search.field import VectorField
from redis.commands.search.indexDefinition import IndexDefinition, IndexType

# 连接 Redis 数据库，地址换成你自己的 Redis 地址
client = redis.Redis(host="localhost", port=6379, decode_responses=True)
res = client.ping()
print("redis connected:", res)

# 之前模型处理的向量维度是 512
vector_dimension = 512
# 给索引起个与众不同的名字
vector_indexes_name = "idx:truman_indexes"

# 定义向量数据库的 Schema
schema = (
    VectorField(
        "$",
        "FLAT",
        {
            "TYPE": "FLOAT32",
            "DIM": vector_dimension,
            "DISTANCE_METRIC": "COSINE",
        },
        as_name="vector",
    ),
)
# 设置一个前缀，方便后续查询，也作为命名空间和可能的普通数据进行隔离
# 这里设置为 truman-，未来可以通过 truman-* 来查询所有数据
definition = IndexDefinition(prefix=["truman-"], index_type=IndexType.JSON)
# 使用 Redis 客户端实例根据上面的 Schema 和定义创建索引
res = client.ft(vector_indexes_name).create_index(
    fields=schema, definition=definition
)
print("create_index:", res)
```

我们搜索跟这张图类似的 5 张图片。

![](img/a08cbca258aceb0ff89de8b3bbcb59b5.png)

![](img/9a84f761c2e7b4a7403358085b74ba9f.png)

![](img/cb36d6d1a7bf2cfb97d1f68604d0000b.png)

首先原图是被搜出来了，然后搜出特征相似的图片了，感觉还不错的样子。

## 六）文字识图

`clip` 还具有识图能力，可以算出给出词数组和图片相似度的概率密度。这里给他一个图片和一组单词 `['dog', 'cat', 'night', 'astronaut', 'man', 'smiling', 'wave', 'smiling man wave']`，让他给我算算。

![](img/8510170257d5d6077f505fe5ba529144.png)

```python
import torch
from transformers import CLIPProcessor, CLIPModel
from PIL import Image
import time

# 默认从 HuggingFace 加载模型，也可以从本地加载，需要提前下载完毕
model_name_or_local_path = "openai/clip-vit-base-patch16"
# 加载模型
model = CLIPModel.from_pretrained(model_name_or_local_path)
processor = CLIPProcessor.from_pretrained(model_name_or_local_path)

# 记录处理开始时间
start = time.time()
# 读取待处理图片
image = Image.open("truman-170.png")
# 处理图片数量，这里每次只处理一张图片
batch_size = 1

# 要检测是否在图片中出现的内容
text = ['dog', 'cat', 'night', 'astronaut',
        'man', 'smiling', 'wave', 'smiling man wave']

with torch.no_grad():
    # 将图片使用模型加载，转换为 PyTorch 的 Tensor 数据类型
    # 相比较第一篇文章中的例子 1.how-to-embededing/app.py，这里多了一个 text 参数
    inputs = processor(text=text, images=image, return_tensors="pt", padding=True)
    # 将 inputs 中的内容解包，传递给模型，调用模型处理图片和文本
    outputs = model(**inputs)
    # 将原始模型输出转换为类别概率分布（在类别维度上执行 softmax 激活函数）
    probs = outputs.logits_per_image.softmax(dim=1)
    end = time.time()
    # 记录处理结束时间
    print('%s Seconds' % (end - start))
    # 打印所有的概率分布
    for i in range(len(text)):
        print(text[i], ":", probs[0][i])
```

结果 `smiling man wave`（微笑的男人挥手）这个词的概率最大，确实跟我们图片的特征一致。

```bash
8.use-clip-detect-element git:(main) ✗ python app.py
0.247056245803833 Seconds
dog : tensor(0.0072)
cat : tensor(0.0011)
night : tensor(0.0029)
astronaut : tensor(0.0003)
man : tensor(0.0200)
smiling : tensor(0.0086)
wave : tensor(0.0117)
smiling man wave : tensor(0.9482)
```

## 七）文字搜图

把文字做 `Embedding`，去 `Redis` 做向量检索就可以实现文章搜图的能力了。

```python
import torch
import numpy as np
from transformers import CLIPProcessor, CLIPModel, CLIPTokenizer
import time
import redis
from redis.commands.search.query import Query

model_name_or_local_path = "openai/clip-vit-base-patch16"
model = CLIPModel.from_pretrained(model_name_or_local_path)
processor = CLIPProcessor.from_pretrained(model_name_or_local_path)
# 处理文本需要引入
tokenizer = CLIPTokenizer.from_pretrained(model_name_or_local_path)

vector_indexes_name = "idx:truman_indexes"

client = redis.Redis(host="localhost", port=6379, decode_responses=True)
res = client.ping()
print("redis connected:", res)

start = time.time()

# 调用模型获取文本的 embeddings
def get_text_embedding(text): 
    inputs = tokenizer(text, return_tensors = "pt")
    text_embeddings = model.get_text_features(**inputs)
    embedding_as_np = text_embeddings.cpu().detach().numpy()
    embeddings = embedding_as_np.astype(np.float32).tobytes()
    return embeddings

with torch.no_grad():
    # 获取文本的 embeddings
    text_embeddings = get_text_embedding('Say hello')
    # text_embeddings = get_text_embedding('smiling man')

query_vector = text_embeddings
query = (
    Query("(*)=>[KNN 5 @vector $query_vector AS vector_score]")
    .sort_by("vector_score")
    .return_fields("$")
    .dialect(2)
)

def dump_query(query, query_vector, extra_params={}):
    result_docs = (
        client.ft(vector_indexes_name)
        .search(
            query,
            {
                "query_vector": query_vector
            }
            | extra_params,
        )
        .docs
    )
    print(result_docs)
    for doc in result_docs:
        print(doc['id'])

dump_query(query, query_vector, {})

end = time.time()
print('%s Seconds'%(end-start))
```

这是搜索 `smiling man` 的 top5 结果，确实是可以实现通过我们的语义进行搜索图片了！

再搜索 `Say hello`，也搜到了上面大笑打招呼的图片。

![](img/6bf3048dc2173509b6b2b4319cf3e6d3.png)

# 三、总结

完整代码链接： <https://github.com/hehan-wang/simple-image-search-engine>

AI 的快速发展可以让我们一个不是机器学习专业的也可以用模型做出一个有趣又有一些价值的 AI 项目。

下面是我的几个启发：

1. 学习 AI 从调接口开始，但不要只是调接口。

调 OPENAI 的 API 是比较简单，上手快，可以快速做出一些案例。但不要把思维框在调接口里，Huggingface 上还有大量好玩的模型，也不一定需要 GPU，CPU 上也可以玩起来，要多上手尝试，其实没想象中那么难。

2. 产品化

我们这里只是投喂了一部电影，当我们投喂大量电影，并做出 UI 界面给普通用户使用，这里还是有很大商业价值的。

更多内容可以点击目录： [手册内容汇总](https://aiyanxishe.feishu.cn/wiki/LBXcwPv6UiZgdfkp14Xc1Wo1n3b)

✨ 如果觉得有收获，请点一点下方的“有启发” ✨

🌱 我也会及时优化迭代内容，给大家更好的阅读体验 🌱

<https://aiyanxishe.feishu.cn/sync/IaSadpoOOsopKab1ajKcz6z3nAb>
