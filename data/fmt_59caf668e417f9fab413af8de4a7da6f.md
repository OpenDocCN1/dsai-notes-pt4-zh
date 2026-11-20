
# 如何在 LangChain 中使用 RAGatouille

> 来源：[https://agijuejin.feishu.cn/docx/Z0PfdnDcDo8NvTxZdavco8XHn5c](https://agijuejin.feishu.cn/docx/Z0PfdnDcDo8NvTxZdavco8XHn5c)

> 作者：向量检索实验室 | AI 搜索引擎

> 原文：[https://mp.weixin.qq.com/s/6RTnZZDEXg3x72nf_xsLYQ](https://mp.weixin.qq.com/s/6RTnZZDEXg3x72nf_xsLYQ)

原文链接：https://python.langchain.com/docs/integrations/retrievers/ragatouille

如何在 LangChain 中使用 RAGatouille [1] 检索器。RAGatouille 极大简化了使用 ColBERT 的流程！ColBERT [2] 是一种既快速又精确的检索模型，它能在几十毫秒内完成对大型文本集合的基于 BERT 的可扩展搜索。

我们可以把它作为一个检索器 [3] 来使用。这里将展示与该集成相关的特定功能。阅读完毕后，您可能会想要访问相关的用例页面 [4]，学习如何将这种向量存储技术融入到更广泛的处理链中。

## 安装

集成在 `ragatouille` 包中。

```
pip install -U ragatouille
```

## 使用

这个例子来自他们的官方文档

```
from ragatouille import RAGPretrainedModel

RAG = RAGPretrainedModel.from_pretrained("colbert-ir/colbertv2.0")
```

```
import requests


def get_wikipedia_page(title: str):
    """
    Retrieve the full text content of a Wikipedia page.

    :param title: str - Title of the Wikipedia page.
    :return: str - Full text content of the page as raw string.
    """
    # Wikipedia API endpoint
    URL = "https://en.wikipedia.org/w/api.php"

    # Parameters for the API request
    params = {
        "action": "query",
        "format": "json",
        "titles": title,
        "prop": "extracts",
        "explaintext": True,
    }

    # Custom User-Agent header to comply with Wikipedia's best practices
    headers = {"User-Agent": "RAGatouille_tutorial/0.0.1 (ben@clavie.eu)"}

    response = requests.get(URL, params=params, headers=headers)
    data = response.json()

    # Extracting page content
    page = next(iter(data["query"]["pages"].values()))
    return page["extract"] if "extract" in page else None
```

```
full_document = get_wikipedia_page("Hayao_Miyazaki")
```

```
RAG.index(
    collection=[full_document],
    index_name="Miyazaki-123",
    max_document_length=180,
    split_documents=True,
)
```

```
[Jan 07, 10:38:18] > Creating directory .ragatouille/colbert/indexes/Miyazaki-123 


> Starting...
[Jan 07, 10:38:23] Loading segmented_maxsim_cpp extension (set COLBERT_LOAD_TORCH_EXTENSION_VERBOSE=True for more info)...
[Jan 07, 10:38:24] [0]       > Encoding 81 passages..
[Jan 07, 10:38:27] [0]       avg_doclen_est = 129.9629669189453      len(local_sample) = 81
[Jan 07, 10:38:27] [0]       Creating 1,024 partitions.
[Jan 07, 10:38:27] [0]       *Estimated* 10,527 embeddings.
[Jan 07, 10:38:27] [0]       > Saving the indexing plan to .ragatouille/colbert/indexes/Miyazaki-123/plan.json ..
Clustering 10001 points in 128D to 1024 clusters, redo 1 times, 20 iterations
  Preprocessing in 0.00 s
  Iteration 0 (0.02 s, search 0.02 s): objective=3772.41 imbalance=1.562 nsplit=0         Iteration 1 (0.02 s, search 0.02 s): objective=2408.99 imbalance=1.470 nsplit=1         Iteration 2 (0.03 s, search 0.03 s): objective=2243.87 imbalance=1.450 nsplit=0         Iteration 3 (0.04 s, search 0.04 s): objective=2168.48 imbalance=1.444 nsplit=0         Iteration 4 (0.05 s, search 0.05 s): objective=2134.26 imbalance=1.449 nsplit=0         Iteration 5 (0.06 s, search 0.05 s): objective=2117.18 imbalance=1.449 nsplit=0         Iteration 6 (0.06 s, search 0.06 s): objective=2108.43 imbalance=1.449 nsplit=0         Iteration 7 (0.07 s, search 0.07 s): objective=2102.62 imbalance=1.450 nsplit=0         Iteration 8 (0.08 s, search 0.08 s): objective=2100.68 imbalance=1.451 nsplit=0         Iteration 9 (0.09 s, search 0.08 s): objective=2099.66 imbalance=1.451 nsplit=0         Iteration 10 (0.10 s, search 0.09 s): objective=2099.03 imbalance=1.451 nsplit=0         Iteration 11 (0.10 s, search 0.10 s): objective=2098.67 imbalance=1.453 nsplit=0         Iteration 12 (0.11 s, search 0.11 s): objective=2097.78 imbalance=1.455 nsplit=0         Iteration 13 (0.12 s, search 0.12 s): objective=2097.31 imbalance=1.455 nsplit=0         Iteration 14 (0.13 s, search 0.12 s): objective=2097.13 imbalance=1.455 nsplit=0         Iteration 15 (0.14 s, search 0.13 s): objective=2097.09 imbalance=1.455 nsplit=0         Iteration 16 (0.14 s, search 0.14 s): objective=2097.09 imbalance=1.455 nsplit=0         Iteration 17 (0.15 s, search 0.15 s): objective=2097.09 imbalance=1.455 nsplit=0         Iteration 18 (0.16 s, search 0.15 s): objective=2097.09 imbalance=1.455 nsplit=0         Iteration 19 (0.17 s, search 0.16 s): objective=2097.09 imbalance=1.455 nsplit=0       [0.037, 0.038, 0.041, 0.036, 0.035, 0.036, 0.034, 0.036, 0.034, 0.034, 0.036, 0.037, 0.032, 0.039, 0.035, 0.039, 0.033, 0.035, 0.035, 0.037, 0.037, 0.037, 0.037, 0.037, 0.038, 0.034, 0.037, 0.035, 0.036, 0.037, 0.036, 0.04, 0.037, 0.037, 0.036, 0.034, 0.037, 0.035, 0.038, 0.039, 0.037, 0.039, 0.035, 0.036, 0.036, 0.035, 0.035, 0.038, 0.037, 0.033, 0.036, 0.032, 0.034, 0.035, 0.037, 0.037, 0.041, 0.037, 0.039, 0.033, 0.035, 0.033, 0.036, 0.036, 0.038, 0.036, 0.037, 0.038, 0.035, 0.035, 0.033, 0.034, 0.032, 0.038, 0.037, 0.037, 0.036, 0.04, 0.033, 0.037, 0.035, 0.04, 0.036, 0.04, 0.032, 0.037, 0.036, 0.037, 0.034, 0.042, 0.037, 0.035, 0.035, 0.038, 0.034, 0.036, 0.041, 0.035, 0.036, 0.037, 0.041, 0.04, 0.036, 0.036, 0.035, 0.036, 0.034, 0.033, 0.036, 0.033, 0.037, 0.038, 0.036, 0.033, 0.038, 0.037, 0.038, 0.037, 0.039, 0.04, 0.034, 0.034, 0.036, 0.039, 0.033, 0.037, 0.035, 0.037]
[Jan 07, 10:38:27] [0]       > Encoding 81 passages..
[Jan 07, 10:38:30] > Optimizing IVF to store map from centroids to list of pids..
[Jan 07, 10:38:30] > Building the emb2pid mapping..
[Jan 07, 10:38:30] len(emb2pid) = 10527
[Jan 07, 10:38:30] > Saved optimized IVF to .ragatouille/colbert/indexes/Miyazaki-123/ivf.pid.pt

> Joined...
Done indexing!
```

```
/Users/harrisonchase/.pyenv/versions/3.10.1/envs/langchain/lib/python3.10/site-packages/torch/cuda/amp/grad_scaler.py:125: UserWarning: torch.cuda.amp.GradScaler is enabled, but CUDA is not available.  Disabling.
  warnings.warn(
  0%|          | 0/2 [00:00<?, ?it/s]/Users/harrisonchase/.pyenv/versions/3.10.1/envs/langchain/lib/python3.10/site-packages/torch/amp/autocast_mode.py:250: UserWarning: User provided device_type of 'cuda', but CUDA is not available. Disabling
  warnings.warn(
 50%|█████     | 1/2 [00:02<00:02,  2.85s/it]/Users/harrisonchase/.pyenv/versions/3.10.1/envs/langchain/lib/python3.10/site-packages/torch/amp/autocast_mode.py:250: UserWarning: User provided device_type of 'cuda', but CUDA is not available. Disabling
  warnings.warn(
100%|██████████| 2/2 [00:03<00:00,  1.74s/it]
WARNING clustering 10001 points to 1024 centroids: please provide at least 39936 training points
0it [00:00, ?it/s]
  0%|          | 0/2 [00:00<?, ?it/s]
 50%|█████     | 1/2 [00:02<00:02,  2.53s/it]
100%|██████████| 2/2 [00:03<00:00,  1.56s/it]
1it [00:03,  3.16s/it]
100%|██████████| 1/1 [00:00<00:00, 4017.53it/s]
100%|██████████| 1024/1024 [00:00<00:00, 306105.57it/s]
```

```
results = RAG.search(query="What animation studio did Miyazaki found?", k=3)
```
