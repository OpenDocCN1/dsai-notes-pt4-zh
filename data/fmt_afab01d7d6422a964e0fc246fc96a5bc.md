
因为`FlagEmbedding`并不能够支持所有的`embedding`模型，所以这里给大家看看如何使用`SentenceTransformer`调用`BGE-M3`模型

```
from sentence_transformers import SentenceTransformer
model = SentenceTransformer(model_path)
embeddings = model.encode(sentences, normalize_embeddings=True)
print(f"嵌入向量：\n{embeddings.shape}")
print(f"相似度分数：\n{embeddings @ embeddings.T}")
```

输出：

```
嵌入向量：
(3, 1024)
相似度分数：
[[1.         0.7747749  0.5957686 ]
 [0.7747749  0.9999999  0.64835596]
 [0.5957686  0.64835596 1.0000001 ]]
```

输出几乎一致，验证了两者推理的`bge-m3`模型效果是一致的。

### OpenAI API 兼容调用

这个部分主要是给大家看看如何使用别人的`embedding`云服务生成文本`embedding`。

```
from openai import OpenAI
client = OpenAI(api_key=os.getenv("API_KEY"), base_url=os.getenv("BASE_URL"))
response = client.embeddings.create(input=sentences, model="BAAI/bge-m3")
embeddings = np.asarray([response.data[i].embedding for i in range(len(sentences))])
print(f"相似度分数：\n{embeddings @ embeddings.T}")
```

输出：

```
[[0.99999997 0.77488069 0.59606279]
 [0.77488069 1.00000009 0.6488824 ]
 [0.59606279 0.6488824  0.99999999]]
```

#### 分析

三种方式结果接近，句 1 和句 2 高相似度（~0.77）反映“快乐”的语义一致性。

## 3.3 查询与语料编码

在检索任务中，如何让查询和语料的嵌入表示更契合呢？这里我们介绍一种方法：通过指令提示优化查询的表示。`FlagModel` 是一个嵌入模型，在初始化时可以通过参数 `query_instruction_for_retrieval` 设置提示，比如 `Represent this sentence for searching relevant passages:`。这个提示的作用是什么？它指导模型生成查询嵌入时，聚焦于与语料库中相关段落的语义匹配，而不是仅仅输出通用的句子表示。

不过需要注意，我这里使用的是 `BGE-M3` 模型，而代码示例是基于初代或第二代 BGE 模型的调用方式。`BGE-M3` 不需要指令也能很好地完成检索任务，这是它相较于早期模型的一个改进点。但如果你追求极致的性能，尤其是在特定领域或复杂查询场景下，可以尝试添加指令并测试效果。建议在你的实际应用中对比两种方式，选择最优方案。来看看代码：

```
model = FlagModel(model_path, query_instruction_for_retrieval="Represent this sentence for searching relevant passages:")
queries = ["query 1", "query 2"]
corpus = ["passage 1", "passage 2"]
q_embeddings = model.encode_queries(queries)
p_embeddings = model.encode_corpus(corpus)
print(q_embeddings @ p_embeddings.T)
```

最后一行 `q_embeddings @ p_embeddings.T` 是什么意思呢？它计算了查询和语料嵌入之间的相似度矩阵。我们一步步拆解它的数学原理。

### 数学原理

假设：

是   的查询嵌入矩阵，每行   表示一个查询的嵌入向量。

是   的语料嵌入矩阵，每行   表示一个段落的嵌入向量。

是   的转置，尺寸为  。

计算过程为：

结果   是一个   的矩阵，其中每个元素   表示第   个查询与第   个段落的相似度，公式为：

如果嵌入向量已归一化（即  ， ），则   等于余弦相似度：

这里的   是两个向量之间的夹角。

### 一个简单例子

假设我们有 2 个查询和 2 个段落，嵌入维度  ：

计算  ：

结果矩阵：

这表示 `query 1` 与 `passage 2` 的相似度 (0.68) 高于 `passage 1` (0.50)，而 `query 2` 与 `passage 2` 的相似度 (1.67) 最高。

## 3.4 多粒度多功能实验

`BGE-M3` 的一个亮点是支持多粒度表示，包括密集向量、稀疏向量和 `ColBERT` 多向量。那这些表示有什么不同呢？我们通过一个实验来展示它的灵活性。

先看一个简单的编码示例：

```
from FlagEmbedding import BGEM3FlagModel
model = BGEM3FlagModel(model_path, use_fp16=True)
sentences = ["What is BGE M3?", "Definition of BM25"]
embeddings = model.encode(sentences, batch_size=12, max_length=8192, return_dense=True, return_sparse=False, return_colbert_vecs=False)
print(embeddings)
```

输出结果：

```
{
  'dense_vecs': array([[-0.03411703, -0.0470783, ..., 0.04828535, -0.02961658],
                       [-0.01041743, -0.04479258, ..., 0.01503996, 0.011138]], dtype=float32),
  'lexical_weights': None,
  'colbert_vecs': None
}
```

这里我们只返回了密集向量（`dense_vecs`），每个句子对应一个固定维度的嵌入表示。`max_length=8192` 设置了最大输入长度，但如果你的句子很短，这个值可以调小以加快编码速度，比如调整 `max_length=10`：

```
embeddings = model.encode(sentences, max_length=10, return_dense=True, return_sparse=True, return_colbert_vecs=True)
print(embeddings)
```

输出结果：

```
{
  'dense_vecs': array([[-0.03411703, -0.0470783, ..., 0.04828535, -0.02961658],
                       [-0.01041743, -0.04479258, ..., 0.01503996, 0.011138]], dtype=float32),
  'lexical_weights': [
    {'What': 0.08362088, 'is': 0.08146952, 'B': 0.12964639, 'GE': 0.25186998, 'M': 0.1700173, '3': 0.2695788, '?': 0.040755093},
    {'De': 0.050144292, 'fin': 0.1368939, 'ation': 0.04513465, 'of': 0.06342196, 'BM': 0.251676, '25': 0.3335321}
  ],
  'colbert_vecs': [
    array([[-0.00867264, -0.04892197, ..., 0.04389409]], dtype=float32),
    array([[0.01715664, 0.03835307, ..., 0.00310116]], dtype=float32)
  ]
}
```

### 结果解读

+ 密集向量：和之前一样，每个句子一个固定向量，适合整体语义匹配。

+ 稀疏向量：`lexical_weights` 显示每个 token 的权重，比如 “What is BGE M3?” 中 “M” 的权重是 0.1700173，而 “3” 是 0.2695788，反映了词汇在上下文中的重要性。

+ ColBERT 向量：`colbert_vecs` 为每个句子生成 token 级别的向量列表，适合细粒度匹配。

## 3.5 稀疏检索与词汇匹配

将 `return_sparse` 设置为 `true`，使模型返回稀疏向量。如果一个词标记在句子中多次出现，它只保留其最大权重。

`BGE-M3`通过在隐藏状态后添加一个线性层和一个`ReLU`激活函数来生成稀疏嵌入：

其中  表示线性层的权重， 是第  个标记的编码器输出。

基于查询和段落中标记的权重，它们之间的相关性分数是通过查询和段落中共存词项的联合重要性计算的：

其中  分别是查询和段落中每个共存词项  的重要性权重。

### BGE-M3 稀疏向量的特点与优势

你有没有想过，传统的词汇匹配和现代语义理解能否结合在一起？`BGE-M3` 就做到了这一点。作为一种深度学习嵌入模型，它生成的稀疏向量不仅记录词汇出现，还通过语义建模为每个词汇分配动态权重。这种方式融合了词汇匹配的直观性和语义理解的深度。比如，在代码示例中，`lexical_weights` 会输出一个词典，每个词（如 `What`、`is`、`B`）对应一个浮点权重，这些权重由模型根据上下文和语义重要性动态计算得出。

相比传统的词频统计，`BGE-M3` 的优势在哪里呢？它能更好地处理同义词、语义相近的表达以及上下文关联性。例如，“`BGE M3 is an embedding model...`” 和 “`What is BGE M3?`” 的匹配分数达到 0.1955，这个分数不仅反映词汇重叠，还捕捉到了语义上的相关性。更妙的是，`BGE-M3` 还支持跨语言任务，生成的匹配分数非常适合现代检索系统，能显著提升召回率和精确度。

### 传统词频表示的特点与局限

再来看看传统方法。传统词频表示，比如 `TF-IDF`，依靠统计手段，通过词频（`TF`）和逆文档频率（`IDF`）来评估词汇的重要性。这种方法简单高效，在纯词汇匹配任务中表现不错。但问题来了：它真的能理解语义吗？显然不能。像 “`dog`” 和 “`puppy`” 这样的同义词，它无法识别语义相似性；而且，它对上下文也不敏感，容易被停用词（如 “`is`”、“`the`”）或高频无关词干扰。在复杂查询或语义检索场景中，这种表面统计的局限性让它比不上深度学习模型。

### 代码示例与效果

想看看 `BGE-M3` 的实际表现吗？下面这段代码展示了它的词权重生成和匹配分数计算：

```
sentences_1 = ["What is BGE M3?", "Definition of BM25"]
sentences_2 = ["BGE M3 is an embedding model...", "BM25 is a bag-of-words..."]
output_1 = model.encode(sentences_1, return_sparse=True)
output_2 = model.encode(sentences_2, return_sparse=True)
print(model.convert_id_to_token(output_1['lexical_weights']))
print(model.compute_lexical_matching_score(output_1['lexical_weights'][0], output_2['lexical_weights'][0]))
```

输出结果如下：

```
[{'What': 0.08362088, 'is': 0.08146952, 'B': 0.12964639, ...}, {...}]
0.19554441515356302
```

从这里可以看出，`BGE-M3` 的稀疏向量不仅捕捉到了词汇，还通过权重反映了语义重要性，表达力明显强于传统词频。

### 上下文如何影响词汇权重？

我们再深入一点。你有没有好奇同一个词在不同句子中的权重会有什么变化？来看这个实验：

```
sentences = [
    "The cat is cute",
    "Is this a test?",
    "The sky is blue and vast"
]
output = model.encode(sentences, return_dense=False, return_sparse=True, return_colbert_vecs=False)
weights = model.convert_id_to_token(output['lexical_weights'])
for i, sentence in enumerate(sentences):
    print(f"Sentence {i+1}: '{sentence}'")
    print(f"Weights: {weights[i]}")
    if 'is' in weights[i]:
        print(f"'is' in Sentence {i+1}: {weights[i]['is']}")
    else:
        print(f"'is' not found in Sentence {i+1}")
```

输出结果如下：

```
Sentence 1: 'The cat is cute'
Weights: {'The': 0.19849324, 'cat': 0.336685, 'is': 0.2108174, 'cute': 0.29809025}
'is' in Sentence 1: 0.2108173966407776

Sentence 2: 'Is this a test?'
Weights: {'Is': 0.15789574, 'this': 0.23845498, 'a': 0.15951744, 'test': 0.34180185, '?': 0.08654297}
'is' not found in Sentence 2

Sentence 3: 'The sky is blue and vast'
Weights: {'The': 0.16425258, 'sky': 0.27996635, 'is': 0.18102533, 'blue': 0.28520262, 'and': 0.16542327, 'vast': 0.2636472}
'is' in Sentence 3: 0.18102532625198364
```

分析一下：同一词 “`is`” 的权重在不同句子中有所变化（0.2108 vs. 0.1810），而且当它出现在句首大写 “`Is`” 时，被视为不同的 `token`。这说明 `BGE-M3` 对上下文和词形非常敏感。

---

## 3.6 多向量检索实验

接下来，我们聊聊 `ColBERT`——一种通过多向量表示提升检索精度和效率的方法。你可能听说过传统的单向量模型，比如 `BERT`，它把文本压缩成一个向量。但如果让你把一篇长文浓缩成一个点，会不会觉得有些细节被忽略了？`ColBERT` 正是为了解决这个问题而设计的。

### 多向量是什么？

与单向量不同，`ColBERT` 为文本中的每个 token 生成一个独立的向量表示。这样，一段查询或文档就变成了一个向量列表，而不是单一向量。这种方式保留了 token 级别的上下文信息，让匹配更加细腻。

多向量方法使用整个输出嵌入来表示查询  和段落 。

其中  是可学习的投影矩阵。

按照 `ColBERT` 的方法，我们使用后期交互计算细粒度相关性分数：

其中  分别是查询和段落的整个输出嵌入。

这是  中每个  与  中向量的最大相似性的平均总和。

### 多向量有什么好处？
