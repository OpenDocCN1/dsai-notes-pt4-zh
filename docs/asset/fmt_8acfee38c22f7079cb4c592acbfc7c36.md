
```
Run experiment
run_experiment(
    experiment_name=experiment_name,
    chunk_size=CHUNK_SIZE,
    chunk_overlap=CHUNK_OVERLAP,
    num_chunks=NUM_CHUNKS,
    embedding_model_name=EMBEDDING_MODEL_NAME,
    llm=LLM,
    evaluator=EVALUATOR,
    docs_dir=DOCS_DIR,
    experiments_dir=EXPERIMENTS_DIR,
    references_fp=REFERENCES_FILE_PATH,
    num_samples=NUM_SAMPLES)
```

## 5.5 上下文

我们现在准备开始我们的实验了！我们将首先测试我们提供的附加上下文是否有帮助。这是为了验证 RAG 系统确实值得付出努力。我们可以通过设置 `num_chunks=0`（无上下文）并将其与 `num_chunks=5` 进行比较来做到这一点。

```
Without context
num_chunks = 0
experiment_name = f"without-context"
run_experiment()
With context
num_chunks = 5
experiment_name = f"with-context"
run_experiment()
```

![](img/accdb039600fcd3d693b4b0aac023b07.png)

![](img/8c9ea6fa34cb7ac3990cc9781895ca76.png)

健全性检查：由于我们使用任何上下文，因此无上下文的检索分数为零。

正如我们所看到的，使用上下文 (RAG) 确实有助于提高我们答案的质量（并且有很大的帮助）。

## 5.6 块大小

接下来，我们将访问各种块大小。较小的块（但不能太小！）能够封装原子概念，从而产生更精确的检索。而较大的块更容易受到噪声的影响。流行的策略包括使用小块，但检索其周围的一些块（因为它可能具有相关信息）或为每个文档存储多个嵌入（例如每个文档的摘要嵌入）。

```
chunk_sizes = [100, 300, 500, 700, 900]
for chunk_size in chunk_sizes:
    experiment_name = f"chunk-size-{chunk_size}"
    run_experiment(...)
```

![](img/ee96b89a170a1dc464ae2c3b5f7af2f9.png)

![](img/38f78da737ee04b0d3bdf6c91cb280e3.png)

> 看来较大的块大小确实有帮助，但会逐渐减弱（太多的上下文可能会太吵闹）。块大小并不总是越大越好。

> 注意：如果我们要使用更大的块大小（我们的块大小基于字符），请记住大多数开源嵌入模型的最大序列长度为 512 个子词标记。这意味着，如果我们的块包含超过 512 个子词标记（4 个字符 ≈ 1 个标记），则嵌入无论如何都不会考虑它（除非我们微调嵌入模型以具有更长的序列长度）。

```
CHUNK_SIZE = 700
CHUNK_OVERLAP = 50
```

## 5.7 块数量

> 接下来，我们将试验要使用的块的数量。更多的块将允许我们添加更多的上下文，但太多可能会引入大量的噪音。

> 注意：我们选择的 `chunk_size` 乘以 `num_chunks` 需要适合我们的 LLM 上下文长度。我们正在试验块大小和块数量，就好像它们是自变量一样，但它们密切相关。特别是因为我们所有的 LLM 都有有限的最大上下文长度。因此，理想情况下，我们会调整 `chunk_size * num_chunks` 的组合。

```
num_chunks_list = [1, 3, 5, 7, 9]
for num_chunks in num_chunks_list:
    experiment_name = f"num-chunks-{num_chunks}"
    run_experiment(...)
```

![](img/a7fe28697befe76a0c89acd80d99b176.png)

![](img/4459adbface51923e18b18e879fd3a57.png)

一般来说，增加块的数量可以提高我们的检索和质量得分。然而，将块数量增加到超过七个块的好处开始增加比信号更多的噪声，并且反映在我们的质量得分上。

健全性检查：随着块数量的增加，我们的检索分数（一般来说）应该会增加。

```
NUM_CHUNKS = 7
```

## 5.8 嵌入模型

目前为止，我们使用 `thenlper/gte-base` 作为我们的嵌入模型，因为它是一个相对较小（0.22 GB）且高性能的选项。但现在，让我们探索其他流行的选项，例如 MTEB 排行榜上当前的领先者 `thenlper/gte-large` (0.67 GB)、`BAAI/bge-large-en` (1.34 GB) 和 OpenAI 的 `text-embedding-ada-002`。

```
embedding_model_names = ["thenlper/gte-base", "thenlper/gte-large", "BAAI/bge-large-en", "text-embedding-ada-002"]
for embedding_model_name in embedding_model_names:
    experiment_name = f"{embedding_model_name.split('/')[-1]}"
    run_experiment(...)
```

![](img/281796737a036b0be9b86ef8801ba502.png)

![](img/fcc73ed5d19c3066d1f9fc0cbdbc7e2a.png)

这是一个有趣的结果，因为当前排行榜上的 #1 (`BAAI/bge-large-en`) 不一定最适合我们的特定任务。在我们的实验中，使用较小的 `thenlper/gte-large` 产生了最佳的检索和质量分数。

```
EMBEDDING_MODEL_NAME = "thenlper/gte-large"
```

## 5.9 开源LLM vs. 闭源LLM

> 我们现在将使用上面的最佳配置来评估主要法学硕士的不同选择。

> 注意：

+ 目前为止，我们一直在使用特定的 LLM 来决定配置，因此特定的 LLM 在这里的表现会有点偏差。

+ 这个列表并不详尽，即使对于我们使用的 LLM，也有一些具有更长上下文窗口的版本。

```
llms = ["gpt-3.5-turbo",
        "gpt-4",
        "meta-llama/Llama-2-7b-chat-hf",
        "meta-llama/Llama-2-13b-chat-hf",
        "meta-llama/Llama-2-70b-chat-hf",
        "codellama/CodeLlama-34b-Instruct-hf",
        "mistralai/Mistral-7B-Instruct-v0.1"]
for llm in llms:
    experiment_name = f"{llm.split('/')[-1].lower()}"
    run_experiment(...)
```

![](img/0c380abe8b10577a9ad9b30e09ae6971.png)

健全性检查：检索分数都是相同的，因为我们选择的 LLM 不会影响我们应用的这一部分。

正如预期的那样，`codellama-34b` 的性能优于我们的 OSS LLM，因为 Llama 2 在编码数据集上经过了进一步的训练。我们这里的用例通常涉及与代码相关的查询或上下文。它还优于 `gpt-3.5-turbo`，并且非常接近 `gpt-4` 的性能。

```
LLM = "codellama/CodeLlama-34b-Instruct-hf"
```

> 注意：我们的一些 LLM 的上下文长度要大得多，例如。 `gpt-4` 是 8,192 个令牌，`gpt-3.5-turbo-16k` 是 16,384 个令牌。我们可以增加用于这些的块的数量，因为我们看到增加 `num_chunks` 继续提高检索和质量分数。然而，我们暂时保持这个值固定，因为性能无论如何都开始逐渐减弱，因此我们可以在完全相同的配置下比较这些性能。

## 6、微调

目前为止，我们探索的所有内容都涉及优化数据预处理方式以及按原样使用我们的模型（嵌入、LLM 等）。然而，还值得探索使用我们用例特有的数据来微调我们的模型。这可以帮助我们更好地表示我们的数据，并最终提高我们的检索和质量分数。在本节中，我们将微调我们的嵌入模型。这里的直觉是，学习比默认嵌入模型更符合上下文的令牌表示可能是值得的。如果我们有很多：

默认标记化过程创建子标记的新标记失去了标记的重要性

在我们的用例中具有上下文不同含义的现有标记：

![](img/15116645e51ad41328f2dbfbcfff9bf3.png)

> 当谈到微调我们的嵌入模型时，我们将探索两种方法：

+ 完整参数：包括嵌入层和所有后续编码器层（transformer 块）

+ 嵌入层：更好地表示我们独特的子令牌并避免过度拟合（线性适配器的版本）

> [注意：我们不会在本节中探索微调我们的 LLM](https://www.anyscale.com/blog/fine-tuning-llms-lora-or-full-parameter-an-in-depth-analysis-with-llama-2)，因为我们之前的实验（LoRa vs. 全参数）已经表明微调对形式而非事实有很大帮助，在我们的例子中也没有帮助 很多（与例如 SQL 生成相比）。但是，你的用例可能会受益于微调。

## 6.1 合成数据集

我们的第一步是创建一个数据集来微调我们的嵌入模型。我们当前的嵌入模型已经通过自监督学习（`word2vec`、`GloVe`、下一个/屏蔽标记预测等）进行了训练，因此我们将继续通过自监督工作流程进行微调。我们将重复使用与之前的冷启动 QA 数据集部分非常相似的方法，以便我们可以将数据中的部分映射到问题。这里的微调任务是让模型确定数据集中的哪些部分最适合输入查询。此优化任务将使我们的嵌入模型能够学习数据集中标记的更好表示。

> 注意：虽然我们可以创建一个将部分标题与部分文本映射的数据集，但我们正在创建一个综合问答数据集，因为它将最能代表我们想要学习如何嵌入的数据类型。

我们的提示会有点不同，因为我们想要生成各种不同的问题，并且我们将在此处使用 `llama-70b`，以便我们可以扩展此 QA 生成过程（并避免任何速率限制）。为了彻底起见，我们将从数据集中的每个部分生成一个问题，以便我们可以尝试捕获尽可能多的独特标记。

```
system_content = f"""
Create one question using only the context provided starting with "What", "How" or "Why". Only respond with the question, don't say anything else (unecessary starting words, hints, etc.)
"""
```

```
Generate questions
embedding_qa = []
sections = sections_ds.take_all()
max_context_length = int(0.5*MAX_CONTEXT_LENGTHS[LLM]-get_num_tokens(system_content))
for section in tqdm(sections):
    user_content = trim(
        text=f"context: {section['text']}",
        max_context_length=max_context_length)
    response = generate_response(
        llm="meta-llama/Llama-2-70b-chat-hf",
        temperature=0.0,
        stream=False,
        system_content=system_content,
        user_content=user_content,
        max_retries=1)
    if response:
        embedding_qa.append({"question": response, "source": section["source"]})
print (len(embedding_qa))
```

## 6.2 训练数据

我们现在将把数据集分成训练和验证部分。

```
from sentence_transformers import InputExample
Split counts
num_train_samples = int(len(embedding_qa)*0.8)
emb_qa_train = embedding_qa[:num_train_samples]
emb_qa_val = embedding_qa[num_train_samples:]
Training dataset
train_dataset = []
for item in tqdm(emb_qa_train):
    query = item["question"]
    source_text = fetch_text(item["source"])
    example = InputExample(texts=[query, source_text])
    train_dataset.append(example)
```

## 6.3 验证

我们的验证评估标准涉及一个信息检索 (IR) 评估器，该评估器将从每个查询的语料库中检索前 k 个相似文档。`InformationRetrievalEvaluator` 需要以下输入：

+ 查询：`Dict[str, str]` # qid => 查询

+ 语料库：`Dict[str, str]` # cid => doc

+ `related_docs`: `Dict[str, Set[str]]` # qid => Set[cid]

> 注意：虽然我们的数据集对于特定查询可能有多个有效部分，但我们会将除用于生成查询的部分之外的所有其他部分视为负样本。这不是一个理想的场景，但引入的噪声很小，特别是因为我们使用它来调整表示层（而不是用于分类任务）。

```
from sentence_transformers.evaluation import InformationRetrievalEvaluator
Validation dataset
queries, corpus, relevant_docs = {}, {}, {}
for i, item in tqdm(enumerate(emb_qa_val), total=len(emb_qa_val)):
    queries[f"qid_{i}"] = item["question"]
    corpus[f"cid_{i}"] = fetch_text(item["source"])
    relevant_docs[f"qid_{i}"] = set([f"cid_{i}"])
evaluator = InformationRetrievalEvaluator(queries, corpus, relevant_docs)
```

> 我们将使用 `MultipleNegativesRankingLoss` 作为损失函数。它将使用训练数据中的数据点 (`InputExample(texts=[query, source_text])` 作为正对，所有其他组合作为负对。目标是增加正对的余弦相似度（默认相似度 `fct`）并减少其他对的值。

## 6.4 嵌入模型

现在我们准备初始化嵌入模型以进行微调。

```
from sentence_transformers import SentenceTransformer
embedding_model = SentenceTransformer(EMBEDDING_MODEL_NAME)  # gte-large
```
