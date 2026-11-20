
# 本地搜索与全局搜索配置

## 本地搜索参数配置

### entities

传入知识图谱中的实体（`entities`）。

提供实体的详细信息和属性，帮助本地搜索定位特定实体。

### relationships

传入知识图谱中的关系（`relationships`）。

用于描述实体之间的交互和连接，有助于构建更复杂的答案。

### covariates

传入协变量数据（`covariates`），如果索引阶段未启用协变量，设置为 `None`。

#### 用途

提供附加声明性元数据或上下文信息。

如果协变量未启用，仍可正常运行本地搜索，但答案可能少一些附加的语义信息。

### entity_text_embeddings

传入实体嵌入的存储对象（`description_embedding_store`）。

#### 用途

将实体的文本嵌入存储在向量数据库中。

用于通过语义搜索快速找到相关实体。

### embedding_vectorstore_key

设置嵌入向量存储的键类型。

`EntityVectorStoreKey.ID`：

如果向量存储中的实体是按 ID 存储的，使用此键。

`EntityVectorStoreKey.TITLE`：

如果向量存储按实体标题存储，则改为设置为 `TITLE`。

### text_embedder

传入嵌入生成器对象（`text_embedder`）。

用于生成文本或问题的语义嵌入，支持与存储的实体嵌入进行相似度比较。

### token_encoder

传入分词器（`token_encoder`）。

#### 用途

确保生成的上下文不会超过 LLM 的上下文窗口限制。

有助于在构建查询时分配合理的上下文。

## 上下文构建器的作用

上下文构建器是本地搜索的核心组件，它负责将加载的数据整合成结构化的上下文，包括：

1. 结合社区级摘要（Community Reports）：

提供更高层次的背景信息，用于回答主题性问题。

2. 整合文本单元（Text Units）：

使用来自原始文档的文本片段，为问题构建直接的上下文。

3. 集成知识图谱的实体和关系：

为本地搜索中的实体相关问题提供详细的语义信息。

4. 利用嵌入和分词：

嵌入存储和分词器确保上下文生成语义准确，符合 LLM 的上下文限制。

## 创建本地搜索引擎参数组

```python
local_context_params = {
    "text_unit_prop": 0.5,
    "community_prop": 0.1,
    "conversation_history_max_turns": 5,
    "conversation_history_user_turns_only": True,
    "top_k_mapped_entities": 10,
    "top_k_relationships": 10,
    "include_entity_rank": True,
    "include_relationship_weight": True,
    "include_community_rank": True,
    "return_candidate_context": True,
    "embedding_vectorstore_key": EntityVectorStoreKey.ID,
    "max_tokens": 12_000,
}

llm_params = {
    "max_tokens": 2_000,
    "temperature": 0.0,
}
```

## 本地搜索引擎参数解析

### text_unit_prop

上下文窗口中分配给文本单元（Text Units）的比例（50%）。

控制原始文档中的文本内容在上下文中的权重。

### community_prop

上下文窗口中分配给社区报告（Community Reports）的比例（10%）。

用于为回答提供更高层次的社区背景。

### conversation_history_max_turns

上下文中包含的最大对话轮数（5轮）。

控制是否包括之前的对话历史，以及保留的对话轮数。

### conversation_history_user_turns_only

如果为 `True`，则只包括用户的提问，而忽略模型的回答。

### top_k_mapped_entities

从嵌入向量存储中检索的相关实体的数量（10个）。

### top_k_relationships

检索到上下文窗口中的最大关系数（10个）。

### include_entity_rank

是否在上下文中包含实体的排名（默认是根据实体的节点度数）。

### include_relationship_weight

是否在上下文中包含关系权重（例如，表示关系的重要性）。

### include_community_rank

是否在上下文中包含社区排名。这里设置为 `False`。

### return_candidate_context

如果为 `True`，返回一个数据帧集合，包含所有可能相关的实体、关系和协变量记录。这些记录的上下文中会有一列 `in_context` 表示是否被纳入上下文窗口。

### embedding_vectorstore_key

嵌入向量存储的键类型。

如果向量存储的标识符是实体标题而非 ID，则设置为 `EntityVectorStoreKey.TITLE`。

### max_tokens

上下文窗口允许的最大令牌数（12,000个）。

需要根据所用 LLM 的令牌限制调整（例如，GPT-4 支持 8,192 或 32,768）。

## 创建本地搜索引擎

```python
search_engine = LocalSearch(
    llm=llm,
    context_builder=context_builder,
    token_encoder=token_encoder,
    llm_params=llm_params,
    context_builder_params=local_context_params,
    response_type="multiple paragraphs",
)
```

## 本地搜索引擎参数解析

### llm

已初始化的 LLM 对象（`ChatOpenAI`），用于生成答案。

### context_builder

上一段代码中创建的上下文构建器（`LocalSearchMixedContext`），负责生成语义丰富的上下文。

### token_encoder

分词器，用于计算上下文和生成回答的令牌数，确保不会超过 LLM 的限制。

### llm_params

用于 LLM 的参数，包括生成答案时的令牌数和温度。

### context_builder_params

上下文构建器的配置，控制上下文中每种数据的比例和数量限制。

### response_type

指定回答的格式。

此处为 `"multiple paragraphs"`，表示生成的回答以多段文字形式呈现。

可以根据需求设置为 `"single paragraph"` 或 `"prioritized list"` 等。

## 基于本地搜索的问答流程

```python
result = await search_engine.asearch("请帮我介绍下ID3决策树算法")
```

```python
display(Markdown(result.response))
```

ID3（Iterative Dichotomiser 3）决策树算法是一种用于分类的机器学习算法，由Ross Quinlan在1986年提出。它是一种基于信息增益的决策树生成算法，广泛应用于数据挖掘和知识发现领域。以下是对ID3算法的详细介绍。

### 算法概述

ID3算法的核心思想是通过选择具有最大信息增益的特征来分割数据集，从而构建决策树。信息增益是衡量一个特征在数据集上对分类结果的影响程度的指标。ID3算法通过递归地选择特征并分割数据集，最终生成一棵决策树。

### 信息增益

信息增益是ID3算法选择特征的标准。它基于信息论中的熵概念。熵用于衡量数据集的纯度或不确定性。信息增益计算如下：

1. 计算数据集的熵：熵越高，数据集的不确定性越大。

2. 计算特征的条件熵：在给定特征的条件下，数据集的熵。

3. 计算信息增益：信息增益等于数据集的熵减去特征的条件熵。

选择信息增益最大的特征作为当前节点的分裂特征。

### 算法步骤

1. 初始化：从根节点开始，计算当前数据集的熵。

2. 选择特征：计算每个特征的信息增益，选择信息增益最大的特征进行分裂。

3. 分裂数据集：根据选择的特征，将数据集分裂成子集。

4. 递归构建子树：对每个子集，重复上述步骤，直到满足停止条件（如所有实例属于同一类，或没有剩余特征可分裂）。

5. 生成决策树：当所有节点都满足停止条件时，生成完整的决策树。

### 优点

- 简单易懂，易于实现。
- 生成的决策树具有良好的可解释性。

### 缺点

- 对噪声数据敏感，容易过拟合。
- 只能处理离散特征，连续特征需要离散化。
- 不支持缺失值处理。

### 应用场景

ID3算法适用于需要解释性强的分类任务，如信用评分、医疗诊断等。然而，由于其对噪声敏感和过拟合的缺点，在实际应用中常与其他技术结合使用，如剪枝技术或使用改进版本（如C4.5算法）。

总之，ID3决策树算法是机器学习中一种经典的分类算法，尽管存在一些局限性，但其简单性和可解释性使其在许多领域得到了广泛应用。

## 构建GlobalSearch（全局搜索）搜索引擎并进行问答

### 导入知识图谱相关内容

```python
from graphrag.query.indexer_adapters import (
    read_indexer_communities,
    read_indexer_entities,
    read_indexer_reports,
)
from graphrag.query.llm.oai.chat_openai import ChatOpenAI
from graphrag.query.llm.oai.typing import OpenaiApiType
from graphrag.query.structured_search.global_search.community_context import (
    GlobalCommunityContext,
)
from graphrag.query.structured_search.global_search.search import GlobalSearch
```

```python
COMMUNITY_TABLE = "create_final_communities";
COMMUNITY_REPORT_TABLE = "create_final_community_reports";
ENTITY_TABLE = "create_final_nodes";
ENTITY_EMBEDDING_TABLE = "create_final_entities";
```

```python
community_df = pd.read_parquet(f"{INPUT_DIR}/{COMMUNITY_TABLE}.parquet")
entity_df = pd.read_parquet(f"{INPUT_DIR}/{ENTITY_TABLE}.parquet")
report_df = pd.read_parquet(f"{INPUT_DIR}/{COMMUNITY_REPORT_TABLE}.parquet")
entity_embedding_df = pd.read_parquet(f"{INPUT_DIR}/{ENTITY_EMBEDDING_TABLE}.parquet")

communities = read_indexer_communities(community_df, entity_df, report_df)
reports = read_indexer_reports(report_df, entity_df, COMMUNITY_LEVEL)
entities = read_indexer_entities(entity_df, entity_embedding_df, COMMUNITY_LEVEL)
```

### 创建Global Search模式的上下文构建器 (context_builder)

```python
context_builder = GlobalCommunityContext(
    community_reports=reports,
    communities=communities,
    entities=entities,
    token_encoder=token_encoder,
)
```

代码解释如下：

### GlobalCommunityContext

这是一个为 `Global Search` 模式创建的上下文构建器。它负责从不同的数据源（如社区报告、实体、社区信息等）中提取相关数据，并构建一个用于查询的大范围上下文。

这个构建器帮助为整个文档集合（而不是某个特定实体）构建一个全局的知识背景，以便模型能够对广泛的问题做出响应。

### community_reports=reports

这里传入的是之前从文件中读取的 `reports` 数据（也就是 `COMMUNITY_REPORT_TABLE` 表格的数据）。

这些报告包含了关于不同社区的详细信息，如主题、摘要、影响力、发现等。

在全局搜索中，这些社区报告有助于构建整个数据集的高层次概览。

### communities=communities

`communities` 数据包含社区的结构和信息。在图谱中，社区可能代表了不同的主题、领域或相关性较高的实体群体。

这部分数据通常用于在全局搜索时进行分组和排名，比如通过社群的重要性或与查询的相关性来排序。

### entities=entities

`entities` 是从之前的索引步骤中提取的实体数据（来自 `ENTITY_TABLE` 表）。

这些实体（如人物、地点、事件等）可以用来扩展全局搜索的范围。它们提供了具体的名词、对象和概念，有助于为全局查询提供上下文。

**注意：** 如果不希望在全局搜索中使用社区权重来影响排名（例如，不考虑某个社区对某个实体的影响），可以将 `entities` 参数设为 `None`。

### token_encoder=token_encoder

`token_encoder` 是用于对文本进行编码的工具。它将文本转化为可以输入到模型中的 token 序列。

在之前的代码中，我们已经看到 `token_encoder` 是通过 `tiktoken.get_encoding("cl100k_base")` 获取的，它的作用是把文本切分为一定的 token 数量，以便在模型中使用。

### 配置全局搜索参数

```python
context_builder_params = {
    "use_community_summary": False,
    "shuffle_data": True,
    "include_community_rank": True,
    "min_community_rank": 0,
    "community_rank_name": "rank",
    "include_community_weight": True,
    "community_weight_name": "occurrence weight",
    "normalize_community_weight": True,
    "max_tokens": 12_000,
    "context_name": "Reports",
}

map_llm_params = {
    "max_tokens": 1000,
    "temperature": 0.0,
    "response_format": {"type": "json_object"},
}

reduce_llm_params = {
    "max_tokens": 2000,
    "temperature": 0.0,
}
```

## 构建全局搜索引擎
