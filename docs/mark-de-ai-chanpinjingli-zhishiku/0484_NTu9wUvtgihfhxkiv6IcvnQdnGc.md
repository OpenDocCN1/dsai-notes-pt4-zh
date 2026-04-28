# 基于Agentic上下文工程，0微调实现模型优化

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Y8vjdlyENob8iPxMt5scVuHrnhe](https://qqs7y1hozd1.feishu.cn/docx/Y8vjdlyENob8iPxMt5scVuHrnhe)

## 一、基于Agentic上下文工程（ACE）

ACE（Ag**entic Context** Engineering）框架能够实现可扩展且高效的上下文自适应，并且离线（如系统提示优化）与在线（如测试时记忆自适应）场景都适用。

> 与以往将知识蒸馏为简短摘要或静态指令的方法不同，A**CE 是将上下文视为不断演化的**作战手册，能够持续积累、蒸馏与组织策略。

> **ACE 引入三种协作角色：**

+   生成器（Generator）：生成推理轨迹；

+   反思器（Reflector）：从成功与错误中蒸馏具体洞见；

+   整编器（Curator）：将这些洞见整合进结构化的上下文更新。

这**一设计模仿了人类**的学习方式，即「实验–反思–整合」，同时可避免让单一模型承担所有职能所导致的瓶颈。

![](img/5a2ba389f5dd1462f0a189af130f7c20.png)

> 为应对简约偏置与上下文塌缩问题，ACE 引入了三项关键创新：

+   专职反思者模块：将评估与洞见提取与整编（curation）过程解耦，提高上下文质量与下游性能；

+   增量式 Delta 更新机制：以局部编辑替代整体重写，显著降低延迟与计算开销；

+   grow-and-refine 机制：在持续扩充的同时抑制冗余，实现上下文的稳态演化。

> 在工作流程上，生成器首先会针对新任务生成推理轨迹，揭示出有效策略与常见陷阱；

> 反思器对这些轨迹进行评析，提炼经验并可多轮迭代优化；

> 整编器再将这些经验整合为紧凑的增量条目（delta entries），并通过轻量的、非 LLM 的逻辑机制合并至现有上下文中。

由于更新项是局部化的，多个增量可并行合并，从而实现批量适应与扩展。

## **二、增量式 Delta 更新**

ACE 的核心设计理念是：将上下文表示为结构化的条目集合（bullets），而非单一的整体提示词。

> **每个条目包含两部分：**

+   元数据（metadata）：唯一标识符，以及「有用 / 有害」计数器；

+   内容（content）：比如可复用策略、领域概念或常见错误模式。

在解决新问题时，生成器会标记哪些条目起到了帮助或误导作用，从而为反思器提供改进依据。

> **这种条目化设计带来了三大特性：**

+   局部化（localization）：只更新相关条目；

+   细粒度检索：生成器可聚焦于最相关的知识；

+   增量式适应：推理时可高效进行合并、剪枝与去重。

ACE 不会重写整个上下文，而是生成紧凑的增量上下文（delta contexts）：由反思器提炼、整编器整合的一小组候选条目。

这种方式既避免了整体重写的高计算成本与延迟，又能保持旧知识并持续吸收新见解。随着上下文的增长，该机制为长周期或高知识密度的任务提供了必要的可扩展性。

Agentic RAG（代理型 RAG） 只是与 AI 智能体架构一起使用的 RAG（检索增强生成）。

> **使用传统 RAG 和 Age**ntic RAG，都可以使用 RAG Pipeline 填充搜索索引。

**该过程如下所示：**

![](img/557715370b1a23f823306fc6422cecf2.png)

## 三、 AI 智能体具有“代理性（Agentic）”的架构基础

在实践中，应用程序需要做两件事才能被称为 AI 智能体。

1.  **首先，它需要具有一定的****自主决策能力，****也就是说，它需要具有代理性。**

> 如果你编写了一个执行一系列步骤的程序，其中一步是调用 LLM——恭喜你！你已经构建了一个调用 LLM 的程序工作流——这没有什么错。只是不要称它为 AI 智能体。AI 智能体不会协调要采取的一系列确切步骤。

>

1.  **AI 智能体还需要有某种****与环境交互的方式****。**

> 对于软件来说，这意味着进行 API 调用、检索数据、向 LLM 发送提示等。 LLM 提供商提供实现此目的的机制称为工具。事实证明，大多数 LLM 仅支持一种类型的工具：函数。

AI 智能体的基本应用程序架构如下所示：

![](img/dcec833b89f6567b496471a84d93498a.png)

# Agentic RAG 处理循环

AI 智能体使用与下图类似的方式处理循环。

> 循环首先接收用户查询并执行初始数据检索以获取有用的上下文。然后，它将此信息连同用户目标的描述一起传递给 LLM。

> 它还包括 LLM 可以使用的适当工具列表。

>

> 每个函数/工具都使用 JSON 模式定义，以描述支持的参数和允许的值。指示 LLM 仅使用函数调用进行响应，而不进行其他任何响应。

当 LLM 生成响应时，它应该采用标准结构来定义接下来应执行的函数调用。AI Agent负责执行 LLM 在其响应中指定的函数。

然后，它将函数结果连同对话历史记录一起传递给 LLM，以便进行下一次循环。

![](img/d58108d9fd509dd36b96998e18749b8a.png)

对于这个客服支持项目，需要 LLM 能够利用工具执行以下操作：

1.  从产品文档中查找产品信息；

1.  从客户支持论坛中查找与类似问题相关的数据；

1.  从内部知识库中识别可能有帮助的相关信息；

1.  终止解决过程，因为已找到解决方案；

1.  将问题升级给人工处理，因为 AI 智能体已尽其所能但无法解决问题。

# Agentic RAG 系统的关键：检索功能

直奔主题，传统 RAG 系统与 Age**ntic RA**G 系统的关键设计区别在于检索函数的概念。

> 在这两种系统中，仍然需要使用** RAG Pipeline **来填充向量数据库。

> 但在构建搜索索引时，需要提前考虑用于与搜索索引交互的函数。

> 这些函数的设计需要预见 AI 智能体可能采取的操作，以便帮助智能体解决希望解决的问题。

在尝试的第一个** Agentic RAG **实现中，创建了一个函数，它是我使用的向量数据库的直通函数。

> 将所有三个非结构化数据源加载到单个向量索引中，并填充元数据，以便我从每个系统中的多个文档中检索块。例如，设置元数据过滤器 source=docs 允许我将搜索限制在内部文档的上下文中。

![](img/50e09265ef72f3925888acb903decdb7.png)

> 然而，最初的这种方法效果非常不理想。

> 通过直通方法，从 LLM 接收到的函数调用请求经常毫无逻辑。它可能会错误地设置元数据过滤器，试图在外部数据源中搜索文档，甚至会不合理地组合各种元数据字段，这导致了糟糕的搜索效果。

**例如，创建一个名为**** search_docs ****的函数。**

> 该函数接受查询字符串并预先填充必要的元数据，这比使用一个名为 vector_search 的函数让 LLM 自行推理 source=docs（即搜索内部文档的方式）要高效得多。

即使使用了描述性JSON架构定义来描述 source 参数的允许值，情况依然如此。

**总结下来就是**

1.  **直通函数不可靠。**

> 让 LLM 直接与底层数据库交互并处理复杂的元数据过滤逻辑，容易导致混乱的搜索行为和无意义的函数调用。

>

1.  **小而具体的函数更高效。**

> 将支持复杂查询的函数拆分为小型专用函数（如 search_docs）后，LLM 可以更轻松地识别并调用适当的工具，从而提高了检索的准确性和生成的质量。

这种方法本质上是通过减少LLM的推理负担。

> 引导其以更结构化的方式使用特定的功能，这不仅提高了检索效率，还显著改善了生成结果的质量。

## 六、使用“笨”LLM 构建智能体

LLM 在默认状态下的推理能力较差，但一些模型的**表现比其他模型要好。尝试过**的最小的模型是 Llama 3.1 8B，它的表现最差。

### **6.1JSON 模式**

它经常忽略函数调用的指令，并且在生成过程中返回无效的 JSON，这导致智能体应用程序频繁需要处理 JSON 解析异常。

> **70B 版本的**** Llama **表现稍好一些，但仍未达到理想状态。

> 在我最初启动这个项目时，OpenAI 最新的公开模型还是 gpt-4-turbo。即使在启用 JSON 模式的情况下，生成的响应每 4 到 5 次中仍会返回一次无效的 JSON。

### **6.2提示工程**

> **提示工程是任何 RAG 系统中的关键组成**部分。Agentic RAG 也不例外。

> 虽然高效的数据管理策略有助于实现高性能的信息检索，但仅凭这些措施还不足以确保理想的结果。

> 作为基准测试，需要通过向 LLM 提供广泛的指令来解决用户的查询。

> 并将函数定义交给 LLM，观察这种方法能在多大程度上推动任务的完成。

**开始的提示如下：**

```
你是一家提供软件平台的公司的自动化 AI 技术支持助理。
你的职责是帮助用户解决他们的技术问题和疑问。
用户的输入如下。你的目标是尽你所能协助用户实现他们的目标。
```

![](img/8a17a59d493b42dd94053aa1f0af4c0b.png)

> 当然，调用聊天 API 时还包含了一组函数定义。

> 正如前面提到的，我在这个阶段将检索函数拆分为更受限的、细粒度的函数。

**为了解决这个问题，更新了提示。**

> 明确告诉 LLM：除非它确信答案是正确的，否则不要生成响应。

```
你是一家提供软件平台的公司的自动化 AI 技术支持助理。你负责帮助用户解决技术问题。
你可以访问产品文档，其中包含有关公司产品和服务的详细信息。你可以访问包含操作方法文章的内部知识库。你还可以访问用户经常寻求帮助的社区论坛。你可以使用来自这些来源的信息来帮助用户解决他们的问题。
除非你确定自己对用户的问题有准确的答案，否则请使用 escalate_to_human 函数将问题上报给人工支持人员。如果你不确定，请不要编造答案。
用户的输入如下。你的目标是尽最大努力帮助用户实现他们的目标。
```

### 6.3使用 Agentic RAG 系统的链式思维（Chain of Thought, CoT）

> 在这个问题中，要求 LLM 具备以下推理能力：

+   决定接下来要采取的步骤，以便更接近解决用户的问题。

+   判断何时已找到问题的解决方案。

+   判断何时应放弃并将问题交给人工处理。

**首先是参考 CoT 研究。**

> 看看是否可以将这些高级推理机制应用到这里。但是对于用例，使用 CoT 方法的效果并不明显。

![](img/e2da6ba6a3d630846ab6d9993d2f53e7.png)

> CoT技术在论文中通常用于数学文字题的解答。

> 这些问题的解决方式涉及可重复的逐步方法，并且存在明确的正确答案。

> 然而，AI 智能体的场景并不符合这一特征，因为智能体需要在不确定的环境中做出决策，而不仅仅是寻找单一的“正确答案”。

## 七、迭代构建一个可用的 Agentic RAG 解决方案

### **7.1明确历史交互记录**

尽管在每次处理循环中都会传入聊天记录，但 LLM 有时会忽视它已经得出的发现。

**例如，LLM会：**

1.  使用** Agentic RAG **在文档中搜索，得到一个响应。

1.  然后在知识库中搜索相关信息。

1.  但接着又在文档中搜索相同或非常相似的查询。

为了解决这个问题，增加了一条指令：

```
查阅历史交互记录以了解你所发现的其他内容。
```

> 这条指令的目的是让 LLM 在重复搜索之前，记住和利用之前的发现，从而减少不必要的冗余操作。

### **7.2突破时刻：结构化响应**

**当为这个用例实施**** Agentic RAG ****时，模型并引入了结构化响应。**

> 此时，智能体在确保响应中的数据完整性和使用信息检索获取所需信息方面做得越来越好。

> 经常遇到 LLM 产生无效 JSON 响应的情况，这会中断处理循环。

**使用**** strict=true ****的结构化响应可以改善这个问题。**

> OpenAI 在这一领域所做的任何科学研究似乎都得到了回报。

> **借****助结构化输出，构建 A****I** 智能体的 Agentic RAG 方法开始产生可靠且值得信赖的输出。

## 八、实操基于MCP驱动的 Agentic RAG

为了构建这个系统，将使用以下工具：

+   大规模抓取网络数据。

+   作为Faiss向量数据库。

+   Cursor 作为 MCP 客户端。

**以下是****工作流程****：**

![](img/aedb63e659a4d14dabe9c99d3ab3c35d.png)

> **工作流程**：

+   1用户通过 MCP 客户端（Cursor）输入查询。

+   2-3客户端联系 MCP 服务器以选择相关工具。

+   4-6工具输出返回到客户端以生成响应。

**安装依赖项**：

确保已安装 Python 3.11 或更高版本。

```
pip install mcp qdrant-client

```

### 8.1运行项目

首先，按如下方式启动一个 Qdrant Docker 容器（确保您已下载 Docker）：

```
 docker run -p 6333:6333 -p 6334:6334 \
 -v $(pwd)/qdrant_storage:/qdrant/storage:z \
 qdrant/qdrant

```

接下来，运行代码在向量数据库中创建一个集合。

### 8.2配置MCP服务

最后，按如下方式设置您的本地 MCP 服务器：

+   转到 Cursor 设置。

+   选择 MCP。

+   添加新的全局 MCP 服务器。

在 JSON 文件中添加以下内容：

```
{
  &quot;mcpServers&quot;: {
      &quot;mcp-rag-app&quot;: {
          &quot;command&quot;: &quot;python&quot;,
          &quot;args&quot;: [&quot;/absolute/path/to/server.py&quot;],
          &quot;host&quot;: &quot;127.0.0.1&quot;,
          &quot;port&quot;: 8080,
          &quot;timeout&quot;: 30000
      }
  }
}

```

#### 8.2.1启动一个 MCP 服务器

首先，定义一个带有主机 URL 和端口的 MCP 服务器。

![](img/70e8efe97f74ab1c40d62e1c69297042.png)

#### 8.2.2向量数据库 MCP 工具

通过 MCP 服务器暴露的工具有两个要求：

![](img/d879df5ad4206cb9232fdf796d24f775.png)

+   必须使用“tool”装饰器进行装饰。

+   **必须有清晰的文档字符串。**

在下面的代码中，有一个用于查询向量数据库的 MCP 工具。它存储了与机器学习相关的常见问题解答。

#### 8.2.3网络搜索 MCP 工具

![](img/1c7b19329c1024c2bb4aa8eae128f898.png)

如果查询与机器学习无关，需要一个回退机制。

因此，使用 Bright Data 的 SERP API 进行网络搜索，以从多个来源抓取数据，获取相关上下文。

#### 8.2.4将 MCP 服务器与 Cursor 集成

在设置中，Cursor 是一个使用 MCP 服务器暴露的工具的 MCP 客户端。

要集成 MCP 服务器，转到设置 → MCP → 添加新的全局 MCP 服务器。

在 JSON 文件中，添加如下内容👇

![](img/a7b713e97da5605f520a91640d3620d4.png)

**它有两个 MCP 工具：**

![](img/82eafd970e5cedc87ddef965973bc883.png)

+   Bright Data 网络搜索工具，用于大规模抓取数据。

+   向量数据库搜索工具，用于查询相关文档。

接下来，与 MCP 服务器进行交互。

![](img/51320fcaa2d0c26b0e92c717d027280a.png)

### 8.3详细代码

server.py

```
# server.py
from mcp.server.fastmcp import FastMCP
from rag_code import *

# Create an MCP server
mcp = FastMCP(&quot;MCP-RAG-app&quot;,
              host=&quot;127.0.0.1&quot;,
              port=8080,
              timeout=30)

@mcp.tool()
def machine_learning_faq_retrieval_tool(query: str) -&gt; str:
    &quot;&quot;&quot;Retrieve the most relevant documents from the machine learning
       FAQ collection. Use this tool when the user asks about ML.

    Input:
        query: str -&gt; The user query to retrieve the most relevant documents

    Output:
        context: str -&gt; most relevant documents retrieved from a vector DB
    &quot;&quot;&quot;

    # check type of text
    if not isinstance(query, str):
        raise ValueError(&quot;query must be a string&quot;)
    
    retriever = Retriever(QdrantVDB(&quot;ml_faq_collection&quot;), EmbedData())
    response = retriever.search(query)

    return response


@mcp.tool()
def bright_data_web_search_tool(query: str) -&gt; list[str]:
    &quot;&quot;&quot;
    Search for information on a given topic using Bright Data.
    Use this tool when the user asks about a specific topic or question 
    that is not related to general machine learning.

    Input:
        query: str -&gt; The user query to search for information

    Output:
        context: list[str] -&gt; list of most relevant web search results
    &quot;&quot;&quot;
    # check type of text
    if not isinstance(query, str):
        raise ValueError(&quot;query must be a string&quot;)
    
    import os
    import requests
    import json
    from dotenv import load_dotenv

    # Load environment variables
    load_dotenv()

    #API 配置
    url = &quot;https://api.bochaai.com/v1/web-search&quot;
    api_key = os.getenv(&quot;BOCHAAI_API_KEY&quot;)

    if not api_key:
        raise ValueError(&quot;请在 .env 文件中设置 BOCHAAI_API_KEY&quot;)

    payload = json.dumps({
        &quot;query&quot;: query,
        &quot;summary&quot;: True,
        &quot;count&quot;: 10,
        &quot;page&quot;: 1
    })

    headers = {
        &#x27;Authorization&#x27;: f&#x27;Bearer {api_key}&#x27;,
        &#x27;Content-Type&#x27;: &#x27;application/json&#x27;
    }

    response = requests.request(&quot;POST&quot;, url, headers=headers, data=payload)

    # Return organic search results
    return response.json()[&#x27;organic&#x27;]

if __name__ == &quot;__main__&quot;:
    print(&quot;Starting MCP server at http://127.0.0.1:8080 on port 8080&quot;)
    mcp.run()

```

rag.py

```
from llama_index.embeddings.huggingface import HuggingFaceEmbedding
from tqdm import tqdm
from fasiimcp import FasiClient, FasiConfig

faq_text = &quot;&quot;&quot;Question 1: What is the first step before building a machine learning model?
Answer 1: Understand the problem, define the objective, and identify the right metrics for evaluation.

Question 2: How important is data cleaning in ML?
Answer 2: Extremely important. Clean data improves model performance and reduces the chance of misleading results.

Question 3: Should I normalize or standardize my data?
Answer 3: Yes, especially for models sensitive to feature scales like SVMs, KNN, and neural networks.

Question 4: When should I use feature engineering?
Answer 4: Always consider it. Well-crafted features often yield better results than complex models.

Question 5: How to handle missing values?
Answer 5: Use imputation techniques like mean/median imputation, or model-based imputation depending on the context.

Question 6: Should I balance my dataset for classification tasks?
Answer 6: Yes, especially if the classes are imbalanced. Techniques include resampling, SMOTE, and class-weighting.

Question 7: How do I select features for my model?
Answer 7: Use domain knowledge, correlation analysis, or techniques like Recursive Feature Elimination or SHAP values.

Question 8: Is it good to use all features available?
Answer 8: Not always. Irrelevant or redundant features can reduce performance and increase overfitting.

Question 9: How do I avoid overfitting?
Answer 9: Use techniques like cross-validation, regularization, pruning (for trees), and dropout (for neural nets).

Question 10: Why is cross-validation important?
Answer 10: It provides a more reliable estimate of model performance by reducing bias from a single train-test split.

Question 11: What’s a good train-test split ratio?
Answer 11: Common ratios are 80/20 or 70/30, but use cross-validation for more robust evaluation.

Question 12: Should I tune hyperparameters?
Answer 12: Yes. Use grid search, random search, or Bayesian optimization to improve model performance.

Question 13: What’s the difference between training and validation sets?
Answer 13: Training set trains the model, validation set tunes hyperparameters, and test set evaluates final performance.

Question 14: How do I know if my model is underfitting?
Answer 14: It performs poorly on both training and test sets, indicating it hasn’t learned patterns well.

Question 15: What are signs of overfitting?
Answer 15: High accuracy on training data but poor generalization to test or validation data.

Question 16: Is ensemble modeling useful?
Answer 16: Yes. Ensembles like Random Forests or Gradient Boosting often outperform individual models.

Question 17: When should I use deep learning?
Answer 17: Use it when you have large datasets, complex patterns, or tasks like image and text processing.

Question 18: What is data leakage and how to avoid it?
Answer 18: Data leakage is using future or target-related information during training. Avoid by carefully splitting and preprocessing.

Question 19: How do I measure model performance?
Answer 19: Choose appropriate metrics: accuracy, precision, recall, F1, ROC-AUC for classification; RMSE, MAE for regression.

Question 20: Why is model interpretability important?
Answer 20: It builds trust, helps debug, and ensures compliance—especially important in high-stakes domains like healthcare.
&quot;&quot;&quot;

new_faq_text = [i.replace(&quot;\n&quot;, &quot; &quot;) for i in faq_text.split(&quot;\n\n&quot;)]
def batch_iterate(lst, batch_size):
    for i in range(0, len(lst), batch_size):
        yield lst[i : i + batch_size]

class EmbedData:

    def __init__(self, 
                 embed_model_name=&quot;nomic-ai/nomic-embed-text-v1.5&quot;,
                 batch_size=32):
        
        self.embed_model_name = embed_model_name
        self.embed_model = self._load_embed_model()
        self.batch_size = batch_size
        self.embeddings = []

    def _load_embed_model(self):
        embed_model = HuggingFaceEmbedding(model_name=self.embed_model_name,
                                           trust_remote_code=True,
                                           cache_folder=&#x27;./hf_cache&#x27;
                                           )
        return embed_model
    
    def generate_embedding(self, context):
        return self.embed_model.get_text_embedding_batch(context)
    
    def embed(self, contexts):
        self.contexts = contexts
        
        for batch_context in tqdm(batch_iterate(contexts, self.batch_size),
                                  total=len(contexts)//self.batch_size,
                                  desc=&quot;Embedding data in batches&quot;):
                                  
            batch_embeddings = self.generate_embedding(batch_context)
            
            self.embeddings.extend(batch_embeddings)



class FasiiVDB:

    def __init__(self, collection_name, vector_dim=768, batch_size=512):
        self.collection_name = collection_name
        self.batch_size = batch_size
        self.vector_dim = vector_dim
        self.config = FasiConfig(url=&quot;http://localhost:6333&quot;)
        self.client = FasiClient(self.config)

    def create_collection(self):
        if not self.client.collection_exists(self.collection_name):
            self.client.create_collection(self.collection_name, self.vector_dim)

    def ingest_data(self, embeddata):
        for batch_context, batch_embeddings in tqdm(zip(batch_iterate(embeddata.contexts, self.batch_size), 
                                                        batch_iterate(embeddata.embeddings, self.batch_size)), 
                                                    total=len(embeddata.contexts)//self.batch_size, 
                                                    desc=&quot;Ingesting in batches&quot;):
            self.client.upload_vectors(self.collection_name, batch_embeddings, [{&quot;context&quot;: context} for context in batch_context])
        
class Retriever:

    def __init__(self, vector_db, embeddata):
        
        self.vector_db = vector_db
        self.embeddata = embeddata

    def search(self, query):
        query_embedding = self.embeddata.embed_model.get_query_embedding(query)

        # select the top 3 results
        result = self.vector_db.client.search(
            collection_name=self.vector_db.collection_name,
            query_vector=query_embedding,
            limit=3
        )

        combined_prompt = [item[&quot;payload&quot;][&quot;context&quot;] for item in result]
        final_output = &quot;\n\n---\n\n&quot;.join(combined_prompt)
        return final_output

```

## 九、经验教训

### 9.1传统 RAG 的很多经验仍然适用于 Agentic RAG

与传统 RAG 一样，Agentic RAG 依靠上下文相关内容来帮助 LLM 生成更全面的响应。数据质量非常重要。

![](img/9d7b7164116d86fe331f1d383e275291.png)

> **与传统 RAG 一样，****agentic RAG**** 需要最新信息。**

> **从非结构化数据源构建下游 **rag 管道以支持全面的信息检索对于解决 agentic RAG 解决的问题是必不可少的。

不要仅仅依赖 LLM 的推理能力

> Agentic RAG 系统会犯错误。

> 需要考虑持续的质量保证机制和其他外部工具，以帮助监控智能体随时间的表现。

### 9.2Agentic RAG 的难点仍然是 RAG Pipeline

对尝试过的每个 RAG 框架都有同样的发现。

> **他们倾向于关注问题中最容易**解决的部分（数据检索和与 LLM 交互并不是一件难事）。

**处理围绕非结构化数据源的**数据工程是检索增强生成的繁琐部分。

> 过程会有诸多细微差别和挑战，需要提供针对信息检索进行优化的最新信息，是检索增强生成的繁琐部分。