
| 学习方式 | 所有下游任务使用一个模型，通过提示示例来学习 | 可针对不同下游 NLP 任务在自定义数据上进一步微调 |

| 学习样例 | 零示例、单个示例、少量示例方法 | 不特定，依赖于微调数据 |

**启发**：在 [RAG](https://github.com/FlagOpen/FlagEmbedding/blob/master/Tutorials/1_Embedding/1.2.1_BGE_Series.ipynb) 系统中，`BERT` 更适合用于检索阶段的语义匹配，因为其双向建模能力能更好地捕捉上下文语义；而 `GPT` 更适合用于生成阶段，因为其单向生成能力更适合生成连贯的文本。两者结合使用，可以进一步提升 `RAG` 系统的性能。不过目前确实有基于大语言模型构建的，后续使用`bge`模型的时候会有一个部分涉及到，比如`BAAI/bge-multilingual-gemma2`就是一个基于`LLM`的多语言嵌入模型，并在多语言基准测试中取得 `SOTA` 结果。

参考地址：`https://github.com/FlagOpen/FlagEmbedding/blob/master/Tutorials/1_Embedding/1.2.1_BGE_Series.ipynb`

**代码实战**：由于 `BERT` 的训练成本极高，需要几十 G 显存进行预训练，所以我们这里不做训练，而是使用预训练的 `BERT` 模型，通过 `transformers` 库进行推理。下面代码展示如何加载预训练的 `BERT` 模型并完成以下任务：

1. 获取句子的嵌入向量；
2. 计算句子之间的相似度（语义搜索）；
3. 解决问答任务。

```
# 运行环境说明：
# - Python 版本：3.10 或以上
# - 依赖库安装命令：
#   pip install transformers torch numpy scipy jieba

# 导入必要的库
from transformers import BertTokenizer, BertModel, pipeline
import torch
import numpy as np
from scipy.spatial.distance import cosine

# 加载预训练的 BERT 模型和分词器（英文模型）
# 如果需要处理中文，请更换为中文模型，例如：
# tokenizer = BertTokenizer.from_pretrained('bert-base-chinese')
# model = BertModel.from_pretrained('bert-base-chinese')
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
model = BertModel.from_pretrained('bert-base-uncased')

# 定义一个函数，用于获取句子的嵌入向量
def get_sentence_embedding(sentence):
    inputs = tokenizer(sentence, return_tensors='pt', padding=True, truncation=True, max_length=128)
    with torch.no_grad():
        outputs = model(**inputs)
    # 取 [CLS] token 的隐藏状态作为句子的嵌入
    sentence_embedding = outputs.last_hidden_state[:, 0, :].numpy()
    return sentence_embedding

# 任务 1：获取句子的嵌入向量
print("\n任务 1：获取句子的嵌入向量")
sentence = "Cats love to eat fish."  # 使用英文句子以匹配模型
# 如果使用中文模型，可以改为中文句子，例如：
# sentence = "猫喜欢吃鱼。"
embedding = get_sentence_embedding(sentence)
print("‘Cats love to eat fish.’的嵌入向量（前 5 维）：", embedding[0, :5])

# 任务 2：计算句子之间的相似度（语义搜索）
print("\n任务 2：计算句子之间的相似度（语义搜索）")
def cosine_similarity(vec1, vec2):
    return 1 - cosine(vec1, vec2)

query = "Different types of cats have different personalities."  # 使用英文查询以匹配模型
# 如果使用中文模型，可以改为中文查询，例如：
# query = "不同种类的猫有不同的性格。"
documents = [
    "Various breeds of cats exhibit unique traits.",
    "Dogs are loyal and friendly pets.",
    "Cats love to eat fish."
]
query_embedding = get_sentence_embedding(query)
document_embeddings = [get_sentence_embedding(doc) for doc in documents]
similarities = [cosine_similarity(query_embedding[0], doc_embedding[0]) for doc_embedding in document_embeddings]
most_similar_idx = np.argmax(similarities)
print(f"查询：'{query}'")
print(f"最相似的文档：'{documents[most_similar_idx]}'，相似度：{similarities[most_similar_idx]}")

# 任务 3：问答任务
print("\n任务 3：问答任务")
# 如果需要处理中文问答任务，请更换为中文模型，例如：
# qa_pipeline = pipeline("question-answering", model="uer/roberta-base-chinese-extractive-qa")
qa_pipeline = pipeline("question-answering", model="bert-large-uncased-whole-word-masking-finetuned-squad")
question = "What is the average lifespan of a cat?"  # 使用英文问题以匹配模型
# 如果使用中文模型，可以改为中文问题，例如：
# question = "猫的平均寿命是多少？"
document = "The average lifespan of a domestic cat is 15 years."
result = qa_pipeline(question=question, context=document)
print(f"问题：{question}")
print(f"文档：{document}")
print(f"答案：{result['answer']}")

# 注意：如果需要处理中文，请参考下面的建议
print("\n重要提示：")
print("当前代码使用的是英文模型（bert-base-uncased 和 bert-large-uncased-whole-word-masking-finetuned-squad），")
print("因此示例中使用了英文句子。如果需要处理中文，建议更换为中文预训练模型，例如 bert-base-chinese 或 uer/roberta-base-chinese-extractive-qa。")
```

**运行结果**：
```
任务 1：获取句子的嵌入向量
‘Cats love to eat fish.’的嵌入向量（前 5 维）： [ 0.24298148  0.11151051 -0.43524572 -0.02778768 -0.3984919 ]

任务 2：计算句子之间的相似度（语义搜索）
查询：'Different types of cats have different personalities.'
最相似的文档：'Various breeds of cats exhibit unique traits.'，相似度：0.9110014683942056

任务 3：问答任务
问题：What is the average lifespan of a cat?
文档：The average lifespan of a domestic cat is 15 years.
答案：15 years

重要提示：
当前代码使用的是英文模型（`bert-base-uncased` 和 `bert-large-uncased-whole-word-masking-finetuned-squad`），
因此示例中使用了英文句子。如果需要处理中文，建议更换为中文预训练模型，例如 `bert-base-chinese` 或 `uer/roberta-base-chinese-extractive-qa`。
```

**结果分析**：`BERT` 的强大之处在于其动态嵌入能力。例如，在语义搜索任务中，`BERT` 能准确抓住`"Different types of cats have different personalities."`和`"Various breeds of cats exhibit unique traits."`的语义相似性，相似度高达 `0.911`。在问答任务中，`BERT` 也能从文档中精准提取答案`"15 years"`。不过，由于 `BERT` 的计算复杂度高，建议在有 GPU 的设备上运行上述代码，我这里运行是基于`CPU`环境，内存占用大概是`32GB * 5%`，运行时间是一分钟，基于`jupyter`环境。如果要处理中文，记得换成中文的预训练模型（比如 `bert-base-chinese`）。

最后我们用`bert`测试下`“国王:王后 :: 男人: ?”`是什么效果。

```
#
word_king = "king"
word_queen = "queen"
word_man = "man"
word_woman = "woman"  # 预期结果应该是woman

# 获取词的向量表示
def get_word_embedding(word):
    inputs = tokenizer(word, return_tensors='pt', padding=True, truncation=True)
    with torch.no_grad():
        outputs = model(**inputs)
    # 使用第一个token的embedding（因为是单个词，所以这就是词的embedding）
    word_embedding = outputs.last_hidden_state[:, 0, :].numpy()
    return word_embedding[0]

# 获取各个词的向量
king_vec = get_word_embedding(word_king)
queen_vec = get_word_embedding(word_queen)
man_vec = get_word_embedding(word_man)

# 计算类比向量：king - queen ≈ man - x，所以 x ≈ man - (king - queen)
analogy_vec = man_vec - (king_vec - queen_vec)

# 使用余弦相似度找到最接近的词
test_words = ["woman", "girl", "lady", "female", "princess", "cat", "dog"]
similarities = []

for word in test_words:
    word_vec = get_word_embedding(word)
    similarity = cosine_similarity(analogy_vec, word_vec)
    similarities.append((word, similarity))

# 按相似度排序
similarities.sort(key=lambda x: x[1], reverse=True)

# 打印结果
print("\n类比结果：")
print(f"国王:王后 :: 男人:?")
for word, sim in similarities:
    print(f"{word}: {sim:.4f}")
```

运行结果：
```
类比结果：
国王:王后 :: 男人:?
woman: 0.9566
girl: 0.9476
princess: 0.9436
cat: 0.9372
dog: 0.9325
lady: 0.9178
female: 0.8705
```

**结果分析**：从结果可以看出，`BERT` 成功地解决了这个经典的类比问题，`"woman"`与我们计算的类比向量的相似度高达 `0.9566`，远高于其他候选词。这表明 `BERT` 的嵌入向量确实捕捉到了词语之间的语义关系和模式，不仅仅是表面的共现关系。值得注意的是，与`"woman"`语义相近的其他词如`"girl"`、`"princess"`和`"lady"`也具有较高的相似度，这进一步证明了 `BERT` 的嵌入空间具有连贯的语义结构。

这个结果与之前测试的 `Word2Vec` 和 `GloVe` 模型表现相似，都能正确识别出`"woman"`作为答案，这说明不同的现代嵌入技术在捕捉语义关系方面都相当有效。然而，`BERT` 的优势在于它能根据上下文生成动态的嵌入向量，这使它在处理多义词和复杂语境时表现更为出色。这个简单的类比测试虽然没有完全展示 `BERT` 的这一优势，但其高精度的结果仍然证明了 `BERT` 嵌入的强大语义捕捉能力。

### 4.5 嵌入技术的实际应用案例

一个典型的例子是 Airbnb 推荐系统中对 `Item Embedding` 的应用（案例来源：《深入浅出Embedding：原理解析与应用实践》）。

作为全球最大的短租平台，Airbnb 的核心业务是为房主和租客提供一个中介平台，帮助租客找到合适的短租房（`listing`）。在这个过程中，推荐系统的作用至关重要，因为它直接影响客户的订购率。Airbnb 的推荐系统通过以下方式使用 `Item Embedding` 优化推荐效果：

1. **房源的嵌入表示**：Airbnb 将每个房源（`listing`）表示为一个低维密集向量（即 `Item Embedding`）。这些向量通过神经网络训练，能够捕捉房源的内在特征，例如地理位置、价格、房型、关键词描述等。

2. **用户行为的嵌入学习**：通过分析用户的浏览和点击行为，Airbnb 学习房源之间的相似性。例如，如果用户浏览了房源 A 后又点击了房源 B，则 A 和 B 在嵌入空间中的向量会更接近。

3. **推荐流程**：在推荐过程中，Airbnb 使用中心房源（用户当前浏览的房源）的嵌入向量，映射到其他相似的房源，最终生成推荐列表。

为了更直观地理解这一流程，可以参考下图（图片来源：《深入浅出Embedding：原理解析与应用实践》）：

![](img/fc12870d062cc60a28a2ef0ead885e88.png)

**启发**：这个案例说明，嵌入技术不仅适用于自然语言处理（`NLP`），还可以广泛应用于推荐系统、图像处理等领域。通过将复杂的对象（如房源、商品、用户行为）表示为低维密集向量，嵌入技术能够有效捕捉对象之间的内在关系，从而提升系统的性能。在你的 `RAG` 系统中，也可以借鉴类似的思路，例如将文档或查询表示为嵌入向量，用于优化检索和生成。

---

### 4.6 传统方法与现代嵌入技术的对比

与传统方法相比，现代嵌入技术（如 `Word2Vec`、`GloVe`、`ELMo`、`BERT`）具有以下优势：

1. **低维密集表示**：将单词或句子表示为低维密集向量（通常几十到几百维），解决了维度爆炸问题。例如，`Word2Vec` 和 `GloVe` 通常使用 `300` 维向量，`BERT` 的隐藏层维度为 `768` 维，远低于传统方法的几万维。

2. **语义捕捉**：能捕捉单词和句子之间的语义相似性。例如，“猫”和“狗”的向量在 `Word2Vec`、`GloVe`、`ELMo` 和 `BERT` 中都非常接近，而“看表演”和“表演看”的向量则能被区分开来。

3. **上下文建模**：特别是 `ELMo` 和 `BERT`，通过引入上下文相关的动态嵌入，解决了多义词和长距离依赖问题。

这些优势使现代嵌入技术在各种 `NLP` 任务中大放异彩，例如：

+ **语义搜索**：使用 `BERT`，搜索引擎能返回语义相关的结果，即使查询和文档使用的不是同一个词。例如，搜索“如何训练猫”时，包含“教猫新技能”的文档也会被返回。
