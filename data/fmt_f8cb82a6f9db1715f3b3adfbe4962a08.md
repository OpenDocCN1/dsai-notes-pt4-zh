
```
llm = ChatOpenAI(
    api_key=api_key,
    model=llm_model,
    api_base="https://ai.devtool.tech/proxy/v1",
    api_type=OpenaiApiType.OpenAI,
    max_retries=20,
)

token_encoder = tiktoken.get_encoding("cl100k_base")

text_embedder = OpenAIEmbedding(
    api_key=api_key,
    api_base="https://ai.devtool.tech/proxy/v1",
    api_type=OpenaiApiType.OpenAI,
    model=embedding_model,
    deployment_name=embedding_model,
    max_retries=20,
)
```

### Step 9. 创建`LocalSearch`上下文构建器

```
context_builder = LocalSearchMixedContext(
    community_reports=reports,
    text_units=text_units,
    entities=entities,
    relationships=relationships,
    covariates=None,
    entity_text_embeddings=description_embedding_store,
    embedding_vectorstore_key=EntityVectorStoreKey.ID,
    text_embedder=text_embedder,
    token_encoder=token_encoder,
)
```

### Step 10. 创建本地搜索引擎参数组

```
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

### Step 11. 创建本地搜索引擎

```
search_engine_local = LocalSearch(
    llm=llm,
    context_builder=context_builder,
    token_encoder=token_encoder,
    llm_params=llm_params,
    context_builder_params=local_context_params,
    response_type="multiple paragraphs",
)
```

### Step 12. 基于本地搜索的问答流程

```
result = await search_engine_local.asearch("请帮我介绍下CART决策树算法")
```

```
display(Markdown(result.response))
```

CART（Classification and Regression Trees）决策树算法是一种用于分类和回归的机器学习方法。它由Breiman等人在1984年提出，是一种基于树结构的模型。CART算法的核心思想是通过递归地将数据集划分成更小的子集，最终形成一个树状结构，以便于进行预测和决策。

### 算法原理

CART决策树通过二元分裂（binary splitting）来构建树结构。对于分类问题，CART使用基尼不纯度（Gini impurity）作为分裂标准，而对于回归问题，CART使用方差（variance）来衡量分裂的效果。具体来说，算法从根节点开始，选择一个特征及其对应的阈值来最大化分裂后的信息增益。然后，递归地对每个子节点重复这一过程，直到满足停止条件（如达到最大深度或节点中的样本数小于某个阈值）。

### 特点和优势

1. **易于理解和解释**：决策树模型的结构类似于人类的决策过程，易于可视化和解释。
2. **处理多种数据类型**：CART可以处理数值型和分类型数据，适应性强。
3. **无需数据预处理**：不需要对数据进行标准化或归一化处理。
4. **鲁棒性**：对缺失值和异常值具有一定的鲁棒性。

### 应用场景

CART决策树广泛应用于各种领域，包括金融风险评估、医学诊断、市场营销等。在这些领域中，CART可以用于分类任务（如客户分类、疾病诊断）和回归任务（如房价预测、销售额预测）。

### 局限性

尽管CART决策树有许多优点，但也存在一些局限性。单一的决策树模型可能容易过拟合，尤其是在数据量较小或特征较多的情况下。此外，决策树对数据的微小变化较为敏感，可能导致树结构的显著变化。

为了克服这些局限性，通常会结合集成学习方法，如随机森林（Random Forest）和梯度提升树（Gradient Boosting Trees），以提高模型的稳定性和预测性能。

```
result
```

```
SearchResult(response='CART（Classification and Regression Trees）决策树算法是一种用于分类和回归的机器学习方法。它由Breiman等人在1984年提出，是一种基于树结构的模型。CART算法的核心思想是通过递归地将数据集划分成更小的子集，最终形成一个树状结构，以便于进行预测和决策。\n\n### 算法原理\n\nCART决策树通过二元分裂（binary splitting）来构建树结构。对于分类问题，CART使用基尼不纯度（Gini impurity）作为分裂标准，而对于回归问题，CART使用方差（variance）来衡量分裂的效果。具体来说，算法从根节点开始，选择一个特征及其对应的阈值来最大化分裂后的信息增益。然后，递归地对每个子节点重复这一过程，直到满足停止条件（如达到最大深度或节点中的样本数小于某个阈值）。\n\n### 特点和优势\n\nCART决策树算法具有以下几个特点和优势：\n\n1. **易于理解和解释**：决策树模型的结构类似于人类的决策过程，易于可视化和解释。\n2. **处理多种数据类型**：CART可以处理数值型和分类型数据，适应性强。\n3. **无需数据预处理**：不需要对数据进行标准化或归一化处理。\n4. **鲁棒性**：对缺失值和异常值具有一定的鲁棒性。\n\n### 应用场景\n\nCART决策树广泛应用于各种领域，包括金融风险评估、医学诊断、市场营销等。在这些领域中，CART可以用于分类任务（如客户分类、疾病诊断）和回归任务（如房价预测、销售额预测）。\n\n### 局限性\n\n尽管CART决策树有许多优点，但也存在一些局限性。单一的决策树模型可能容易过拟合，尤其是在数据量较小或特征较多的情况下。此外，决策树对数据的微小变化较为敏感，可能导致树结构的显著变化。\n\n为了克服这些局限性，通常会结合集成学习方法，如随机森林（Random Forest）和梯度提升树（Gradient Boosting Trees），以提高模型的稳定性和预测性能。', context_data={}, context_text='', completion_time=15.266694784164429, llm_calls=1, prompt_tokens=599, output_tokens=766, llm_calls_categories={'build_context': 0, 'response': 1}, prompt_tokens_categories={'build_context': 0, 'response': 599}, output_tokens_categories={'build_context': 0, 'response': 766})
```

然后我们再尝试使用全局搜索

### Step 13. 全局搜索时导入知识图谱相关内容

```
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

COMMUNITY_TABLE = "create_final_communities"
COMMUNITY_REPORT_TABLE = "create_final_community_reports"
ENTITY_TABLE = "create_final_nodes"
ENTITY_EMBEDDING_TABLE = "create_final_entities"

community_df = pd.read_parquet(f"{INPUT_DIR}/{COMMUNITY_TABLE}.parquet")
entity_df = pd.read_parquet(f"{INPUT_DIR}/{ENTITY_TABLE}.parquet")
report_df = pd.read_parquet(f"{INPUT_DIR}/{COMMUNITY_REPORT_TABLE}.parquet")
entity_embedding_df = pd.read_parquet(f"{INPUT_DIR}/{ENTITY_EMBEDDING_TABLE}.parquet")

communities = read_indexer_communities(community_df, entity_df, report_df)
reports = read_indexer_reports(report_df, entity_df, COMMUNITY_LEVEL)
entities = read_indexer_entities(entity_df, entity_embedding_df, COMMUNITY_LEVEL)
```

### Step 14. 创建Global Search模式的上下文构建器 (`context_builder`)

```
context_builder = GlobalCommunityContext(
    community_reports=reports,
    communities=communities,
    entities=entities,
    token_encoder=token_encoder,
)
```

### Step 15. 配置全局搜索参数

```
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

### Step 16. 构建全局搜索引擎

```
search_engine = GlobalSearch(
    llm=llm,
    context_builder=context_builder,
    token_encoder=token_encoder,
    max_data_tokens=12_000,
    map_llm_params=map_llm_params,
    reduce_llm_params=reduce_llm_params,
    allow_general_knowledge=False,
    json_mode=True,
    context_builder_params=context_builder_params,
    concurrent_coroutines=32,
    response_type="multiple paragraphs",
)
```

### Step 17. 执行全局搜索

```
result1 = await search_engine.asearch("请帮我介绍下CART决策树算法")
```

```
display(Markdown(result1.response))
```

### CART决策树算法简介

CART（Classification and Regression Trees）是一种重要的决策树算法，由Breiman、Friedman、Olshen和Stone开发。它以其处理分类和回归问题的能力而闻名，是机器学习中一个多功能的工具。CART算法的一个关键特性是其寻找相邻值的中间值作为分裂点的方法，这一特性增强了其性能 [Data: Reports (3)]。

### 特点与优势

CART算法的一个显著特点是其能够处理连续变量，这使其与ID3算法区别开来。CART树以二叉树的形式生长，便于提取详细的规则，并采用自底向上的剪枝技术来优化其结构。这种多功能性使其成为减少数据集杂质和提高预测准确性的宝贵工具 [Data: Reports (0)]。

### 实现与应用

Scikit-learn是Python中一个著名的机器学习库，它实现了CART算法，提供了构建和分析决策树模型的强大工具。这种集成使用户能够在各种数据分析任务中利用CART的能力 [Data: Reports (3)]。此外，Scikit-learn还包括实现成本复杂度剪枝的方法的参数，允许用户优化其决策树模型以获得更好的性能 [Data: Reports (3)]。

### 回归树的应用

在统计学习领域，CART回归树是一个基本组成部分，特别适用于回归任务。它们利用均方误差（MSE）或平均绝对误差（MAE）等标准来分裂节点和进行预测。这使得它们在数据分析和机器学习应用中成为一个多功能的工具 [Data: Reports (4)]。

综上所述，CART决策树算法因其多功能性和强大的处理能力而在机器学习领域占据重要地位。其在Scikit-learn中的实现进一步增强了其在数据分析任务中的应用潜力。

### 查看全局搜索调用的社区报告

```
result1.context_data["reports"]
```

|  | id | title | occurrence weight | content | rank |
