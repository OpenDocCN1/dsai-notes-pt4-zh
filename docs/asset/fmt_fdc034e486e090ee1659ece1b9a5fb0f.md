
```
Prompt
experiment_name = "Answer the query using the context provided. Be succinct. Contexts are organized in a list of dictionaries [{'text': <context>}, {'text': <context>}, ...]. Feel free to ignore any contexts in the list that don't seem relevant to the query. "
Evaluate
experiment_name = "prompt-ignore-contexts"
run_experiment(
    experiment_name=experiment_name,
    generation_system_content=generation_system_content,  # new prompt
    **kwargs)
```

```
prompt-ignore-contexts
retrieval score: 0.6892655367231638
quality score: 3.559322033898305
```

看来这种特定的提示工程努力并没有帮助提高我们系统的质量。正如我们之前提到的，我们可以通过很多其他方式来设计我们的提示，我们鼓励你探索更多。这里重要的是我们有一个干净而简单的方法来评估我们想要尝试的任何东西。然而，我们凭经验发现，提高检索系统和数据飞轮（我们修复文档本身）的质量对我们系统的整体质量产生了更大的影响。

## 8、词汇搜索

现在，我们将用传统的词法搜索（Lexical Search）来补充基于向量嵌入的搜索，传统的词法搜索搜索查询和文档块之间的精确标记匹配。我们的直觉是，词法搜索可以帮助识别具有精确关键字匹配的块，而语义表示可能无法捕获这些块。特别是对于我们的嵌入模型中词汇表之外的标记（因此通过子标记表示）。但是我们基于嵌入的方法对于捕获隐含含义仍然非常有利，因此我们将组合来自基于向量嵌入的搜索和词汇搜索的多个检索块。

![](img/fef27fafa5b05508d676e0a83aa9b0e7.png)

### 8.1 BM25

让我们使用 BM25 应用词法搜索，这是一种排名算法，奖励查询和上下文之间的唯一标记匹配。

```
import re
from rank_bm25 import BM25Okapi
BM25 index
texts = [re.sub(r"[^a-zA-Z0-9]", " ", chunk[1]).lower().split() for chunk in chunks]
lexical_index = BM25Okapi(texts)
```

与我们检索相关上下文的语义搜索函数类似，我们可以实现一个搜索函数来使用我们的词汇索引来检索相关上下文。

```
def lexical_search(index, query, chunks, k):
    query_tokens = query.lower().split()  # preprocess query
    scores = index.get_scores(query_tokens)  # get best matching (BM) scores
    indices = sorted(range(len(scores)), key=lambda i: -scores[i])[:k]  # sort and get top k
    lexical_context = [{
            "id": chunks[i][0],
            "text": chunks[i][1],
            "source": chunks[i][2],
            "score": scores[i]} for i in indices]
    return lexical_context
```

```
k = 3
query = "What is the default batch size for map_batches?"
top_docs = lexical_search(lexical_index, query, chunks, k=k)
for item in top_docs:
    print (item["source"])
    print (item["text"])
    print ()
```

```
Transforming Data — Ray 2.7.1
Configuring batch size#
The default batch size depends on your resource type. If you’re using CPUs,
the default batch size is 4096. If you’re using GPUs, you must specify an explicit batch size.
```

### 8.2 语义

将其与检索到的源与我们现有的基于向量嵌入的搜索进行比较表明，这两种方法虽然不同，但都检索到了相关源。因此，我们将结合这两种方法，并将其纳入我们的LLM生成环境中。

现在，让我们将其添加到我们的`generate.py`/`QueryAgent`类中，将其合并到我们的检索工作流程中。主要变化是包含来自词法搜索的其他来源：

```
def QueryAgent():
    def __init__(use_lexical_search=True, chunks=[...], **kwargs):
        # Lexical search
        self.chunks = chunks
        self.lexical_index = None
        if use_lexical_search:
            texts = [re.sub(r"[^a-zA-Z0-9]", " ", chunk[1]).lower().split() for chunk in chunks]
            self.lexical_index = BM25Okapi(texts)
    def __call__(lexical_search_k=1, **kwargs):
        # Add lexical search results
        if self.lexical_index:
            lexical_context = lexical_search(
                index=self.lexical_index, query=query, chunks=self.chunks, k=lexical_search_k)
            # Insert after <lexical_search_k> worth of semantic results
            context_results[lexical_search_k:lexical_search_k] = lexical_context
```

现在我们可以运行我们的实验：

```
lexical_search_k_list = [1, 3, 5]
use_lexical_search = True
for lexical_search_k in lexical_search_k_list:
    experiment_name = f"lexical-search-bm25-{lexical_search_k}"
    experiment_names.append(experiment_name)
    run_experiment(
        experiment_name=experiment_name,
        use_lexical_search=use_lexical_search,
        lexical_search_k=lexical_search_k,
        **kwargs)
```

![](img/e766f12f42a3b280d9b727bdf13a645d.png)

![](img/b3f2d150742acf2c2a3156d27f983b3e.png)

```
USE_LEXICAL_SEARCH = True
LEXICAL_SEARCH_K = 1
```

> 看起来词法搜索中最相关的来源是有影响力的，而添加更多（`k=3` 或 `k=5`）则增加的噪音多于价值（即使我们检查的样本很少，情况也是如此）。通过将语义搜索（嵌入）与词汇搜索 (BM25) 相结合，我们能够提高检索和质量分数！

> 注意：这只是我们探索的词汇搜索的一个方面（关键字匹配），但还有许多其他有用的功能，例如过滤、计数等。我们如何将词汇搜索结果与语义搜索结果结合起来也值得探索。

## 9、重排名

到目前为止，我们的所有方法都使用了嵌入模型（+词汇搜索）来识别数据集中前 `k` 个相关块。块的数量 (`k`) 一直是一个很小的数字，因为我们发现添加太多块没有帮助，而且我们的 LLM 的上下文长度受到限制。然而，这一切都是假设前 `k` 个检索到的块确实是最相关的块，并且它们的顺序也是正确的。如果增加块的数量没有帮助，因为某些相关块在有序列表中的位置要低得多，该怎么办？而且，语义表示虽然非常丰富，但并未针对此特定任务进行训练。

在本节中，我们实现重排名（Reranking），以便我们可以使用语义和词汇搜索方法在数据集上撒下更广泛的网（检索许多块），然后根据用户的查询重新排名顺序。这里的直觉是，我们可以通过特定于我们的用例的排名来解释语义表示中的差距。我们将训练一个监督模型，该模型可以预测文档的哪一部分与给定用户的查询最相关。我们将使用此预测对相关块进行重新排序，以便将文档这部分中的块移动到列表的顶部。

![](img/95e26ad572ce2543aa7b662362f02cf9.png)

### 9.1 数据集

我们将重用在微调部分创建的 QA 数据集，因为该数据集包含与特定部分映射的问题。

```
def get_tag(url):
    return re.findall(r"docs\.ray\.io/en/master/([^/]+)", url)[0].split("#")[0]
Load data
df = pd.read_json(Path(ROOT_DIR, "datasets", "embedding_qa.json"))
df["tag"] = df.source.map(get_tag)
df["section"] = df.source.map(lambda source: source.split("/")[-1])
df["question"] = df.apply(lambda row: f"{row['section'].replace('-', ' ')} {row['question']}", axis=1)
df.sample(n=5)
```

![](img/7b9821697036bfba42eb1566133857d8.png)

```
Map only what we want to keep
tags_to_keep = ["rllib", "tune", "train", "cluster", "ray-core", "data", "serve", "ray-observability"]
df["tag"] = df.tag.apply(lambda x: x if x in tags_to_keep else "other")
Counter(df.tag)
```

```
Counter({'rllib': 1269,
'tune': 979,
'train': 697,
'cluster': 690,
'data': 652,
'ray-core': 557,
'other': 406,
'serve': 302,
'ray-observability': 175})
```

### 9.2 建模

> 虽然我们可以训练基于 BERT 等的更复杂的模型，但我们将首先看看逻辑回归如何执行。我们将首先创建一些预处理函数来更好地表示我们的数据。例如，我们的文档有许多采用驼峰式大小写的变量（例如 `RayDeepSpeedStrategy`）。当使用分词器时，我们经常会丢失我们知道有用的各个标记，而是创建随机子标记。

> 注意：我们并不全知地知道如何创建这些独特的预处理函数！这都是有条理迭代的结果。我们训练模型→查看不正确的数据点→查看数据的表示方式（例如子标记化）→更新预处理→迭代↺

```
def split_camel_case_in_sentences(sentences):
    def split_camel_case_word(word):
        return re.sub("([a-z0-9])([A-Z])", r"\1 \2", word)
    processed_sentences = []
    for sentence in sentences:
        processed_words = []

        for word in sentence.split():
            processed_words.extend(split_camel_case_word(word).split())
        processed_sentences.append(" ".join(processed_words))
    return processed_sentences
Preprocess
tokenizer = BertTokenizer.from_pretrained("bert-base-uncased")
def preprocess(texts):
    texts = split_camel_case_in_sentences(texts)  # camelcase
    texts = [tokenizer.tokenize(text) for text in texts]  # subtokens
    texts = [" ".join(word for word in text) for text in texts]
    return texts
print(preprocess(["RayDeepSpeedStrategy"]))
print(preprocess(["What is the default batch_size for map_batches?"]))
```

```
['ray deep speed strategy']
['what is the default batch _ size for map _ batch ##es ?']
```

现在我们可以使用此预处理函数作为管道的一部分来训练我们的模型：

```
Train classifier
from rag.rerank import preprocess  # for pickle
reranker = Pipeline([
    ("preprocess", FunctionTransformer(preprocess)),
    ("vectorizer", TfidfVectorizer(lowercase=True)),
    ("classifier", LogisticRegression(multi_class="multinomial", solver="lbfgs"))
])
reranker.fit(train_df["question"].tolist(), train_df["tag"].tolist())
```

### 9.3 评估

我们现在准备评估我们经过训练的重新排名模型。我们将使用自定义预测函数来预测“其他”，除非最高类别的概率高于某个阈值。
