
# 用 10 行 Python 从头实现 RAG - 无需框架！

> 来源：[https://agijuejin.feishu.cn/docx/POGndkOdSomAk1xfW8scfzAQngc](https://agijuejin.feishu.cn/docx/POGndkOdSomAk1xfW8scfzAQngc)

> 作者：Aitrainee | AI进修生

> 原文：https://mp.weixin.qq.com/s/h2kPbxwHJ_t67BjM0OdOGw

> 🌟 创建一个简单的文档聊天系统：基础知识和实现步骤。

![](img/28c4bb718e37b749db7060c4dc9e2e26.png)

Hi，这里是 Aitrainee，欢迎阅读本期新文章。

在这里我会告诉你如何用 10 行代码实现一个完整的文档聊天系统，这一切都在 Python 中完成，不使用任何框架如 `LangChain`、`LamaIndex` 或向量存储如 `Chroma`。

首先，我们来看一下这个系统是如何工作的。RAG，或者说检索增强生成，其实是个老概念，就是从文档中提取信息。

你需要一个知识库或一堆文档，然后将它们分成小块，每个小块都要计算嵌入向量。这些嵌入向量是用来找到最相关文档的数值表示，通常会存储在向量存储中，但大多数情况下你不需要向量存储。

![](img/ebcddaf82bdbecbbe38b79505f9a57ec.png)

知识库创建部分完成后，当有新查询时，你需要为该查询计算嵌入，然后进行检索，找出与用户查询最接近的文档块。这些块会被附加到初始用户查询中，然后输入到 LLM 中生成响应。整个过程称为 检索增强生成。

实现这个流程的框架有很多，最流行的是 `LangChain` 和 `LamaIndex`，但你只需要 Python、一个嵌入模型和一个 LLM 即可实现一个有效的 RAG 管道。

让我们把上面的步骤整理一下，系统的核心部分分为两个步骤：知识库创建和检索。

## 系统核心步骤

### 知识库创建

- 将文档分成多个小块，通常是段落。
- 为每个文档块计算嵌入向量。嵌入向量是这些文档块的数值表示，用于比较和检索。
- 将这些嵌入向量存储起来，通常使用向量存储，但我们将展示如何不使用向量存储来完成这一过程。

### 检索和生成

- 当用户输入查询时，为该查询计算嵌入向量。
- 使用这个嵌入向量去检索与之最相似的文档块。
- 将这些相关文档块与用户的查询一起输入到一个大型语言模型（LLM）中，生成最终的响应。

## 实现步骤

### 第一步：安装必要的库

我们需要安装一些 Python 库来实现这个过程。可以使用以下命令在 `Google Colab` 中安装这些库：

```python
!pip install -q sentence-transformers
!pip install -q wikipedia-api
!pip install -q numpy
!pip install -q scipy
!pip install -q openai
```

### 第二步：加载嵌入模型

我们将使用 `SentenceTransformer` 加载嵌入模型。这个模型将帮助我们将文本转换为嵌入向量。

```python
from sentence_transformers import SentenceTransformer
model = SentenceTransformer("Alibaba-NLP/gte-base-en-v1.5", trust_remote_code=True)
```

### 第三步：获取维基百科内容并分块

接下来，我们从维基百科获取文本内容，并将其分块。我们使用段落作为文档块。

```python
from wikipediaapi import Wikipedia
wiki = Wikipedia('RAGBot/0.0', 'en')
doc = wiki.page('Hayao_Miyazaki').text
paragraphs = doc.split('\n\n')  # 按段落分块

import textwrap

for i, p in enumerate(paragraphs):
    wrapped_text = textwrap.fill(p, width=100)
    print("-----------------------------------------------------------------")
    print(wrapped_text)
    print("-----------------------------------------------------------------")
```

![](img/6fef60fae2cd015caaf09a66c6156193.png)

### 第四步：嵌入文档

我们为每个文档块计算嵌入向量。

```python
docs_embed = model.encode(paragraphs, normalize_embeddings=True)
print(docs_embed.shape)  # 输出嵌入向量的形状
```

![](img/ad23f3b3ce6b3df07d4be145cc29025c.png)

### 第五步：嵌入查询并检索相关文档块

当用户输入查询时，我们计算查询的嵌入向量，并与文档块的嵌入向量进行比较，找出最相似的文档块。

```python
query = "What was Studio Ghibli's first film?"
query_embed = model.encode(query, normalize_embeddings=True)

import numpy as np
similarities = np.dot(docs_embed, query_embed.T)  # 计算相似度（点积）

top_3_idx = np.argsort(similarities, axis=0)[-3:][::-1].tolist()  # 获取最相似的三个文档块索引
most_similar_documents = [paragraphs[idx] for idx in top_3_idx]
```

![](img/5129085747c656c5e8aad76bfc9db2ff.png)

### 第六步：生成响应

我们将最相关的文档块与用户的查询一起输入到 LLM 中，生成最终的响应。

```python
CONTEXT = ""
for i, p in enumerate(most_similar_documents):
    wrapped_text = textwrap.fill(p, width=100)
    print("-----------------------------------------------------------------")
    print(wrapped_text)
    print("-----------------------------------------------------------------")
    CONTEXT += wrapped_text + "\n\n"

prompt = f"""
Use the following CONTEXT to answer the QUESTION at the end.
If you don't know the answer, just say that you don't know, don't try to make up an answer.

CONTEXT: {CONTEXT}
QUESTION: {query}
"""

import openai

client = openai.OpenAI(api_key="YOUR_API_KEY")

response = client.ChatCompletion.create(
    model="gpt-4",
    messages=[
        {"role": "user", "content": prompt},
    ]
)

print(response.choices[0].message.content)
```

通过以上步骤，你可以用纯 Python 实现一个简单的 RAG 系统。这不仅有助于理解 RAG 的工作原理，还能帮助你在不依赖框架的情况下构建定制的检索增强生成系统。

![](img/3fb9d4d76b7e209fe11cf44a75427dd2.png)

以上所有过程及中间结果：

**向上滑动 / 双击放大**

![](img/040073905f7da2b7612ed1ec5a098a4d.png)

现在，由于需要大量的辅助代码，这超过了 10 行代码，但是基本的 RAG 流水线可以用近 10 行代码实现。

如果你有兴趣了解更多高级技术，可以参考更多资料或课程，探索如何使用 `LangChain` 等框架构建更复杂的系统。

希望这篇文章对你有帮助，感谢阅读！

以上代码 文档在这里（Colab）

[https://colab.research.google.com/drive/1285hWi0mspXAcdgROe8eI5Wj🐧WlKS5v#scrollTo=QHp-zvtG55g1](https://colab.research.google.com/drive/1285hWi0mspXAcdgROe8eI5Wj🐧WlKS5v#scrollTo=QHp-zvtG55g1)

参考链接：

[1] https://www.youtube.com/watch?v=bmduzd1oY7U

**知音难求，自我修炼亦艰**

抓住前沿技术的机遇，与我们一起成为创新的超级个体

**（把握 AIGC 时代的个人力量）**

![](img/919aadd4ef244ce77f9988d73a665ba8.png)
