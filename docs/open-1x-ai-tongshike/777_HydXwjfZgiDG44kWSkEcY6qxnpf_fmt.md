
# RAG 的检索优化：MMR 平衡相关性与多样性

> 来源：[https://datawhaler.feishu.cn/docx/Yz2AdHcliocN3Mxf0HHckp4indf](https://datawhaler.feishu.cn/docx/Yz2AdHcliocN3Mxf0HHckp4indf)

## 🎯 文章目标

本文面向 **信息检索、推荐系统**、自然语言处理领域的工程师以及对 `RAG(Retrieval-Augmented Generation)` 技术感兴趣的实践者，旨在帮助读者：

+   深入理解**信息冗余**问题及其对信息获取效率和用户体验的影响。

+   掌握**最大边际相关性**(MMR) **算法**的数学原理、核心机制与运作流程。

+   了解 MMR 算法在不同场景下的应用实例与代码实现考量。

+   启发如何在具体业务中运用**多样性优化**策略。

> **💡 小提示**
>
> MMR 不仅是一种算法，更体现了一种在信息排序中平衡多个目标的策略性思维。掌握它有助于构建更智能、用户体验更佳的信息系统。
>
> [本次](https://github.com/li-xiu-qi/XiaokeAILabs/blob/main/datas/test_mmr_search/test_mmr_search.py)文章配套代码：
>
> `https://github.com/li-xiu-qi/XiaokeAILabs/blob/main/datas/test_mmr_search/test_mmr_search.py`

## 📄 主题

**本次主题**：系统性解析最大边际相关性 (MMR) 算法，探讨其在抑制信息冗余、增强结果多样性方面的核心原理、实现细节与应用价值。

## 📚 摘要

+   **面临挑战**：传统排序方法侧重于最大化相关性，易导致结果列表内容高度同质化，降低信息获取的边际效用。

+   **MMR 方案**：MMR 引入**边际相关性**概念，在迭代选择过程中，显式地平衡候选项目与查询的相关性以及候选项目与已选项目集合的差异性。

+   **核心机制**：通过调节参数 `Lambda`，灵活控制相关性与多样性在排序决策中的相对权重。

+   **应用领域**：MMR 已在搜索引擎排序、推荐系统、自动文本摘要、计算广告等多个领域得到验证和应用。

---

## 🚁 前言

让我们考虑一个常见的信息检索场景：当用户在搜索引擎中输入查询词，例如“苹果”，期望获得什么样的结果？如果返回的顶部结果清一色指向苹果公司的官方网站，尽管相关性极高，用户可能并不会满意。同样，在推荐系统中，若系统基于用户的某次点击，反复推荐风格、主题极其相似的内容，也会限制用户的发现空间。

**这些例子**揭示了信息服务中的一个核心挑战：信息冗余。单纯追求相关性的排序策略，虽然能找到匹配查询的结果，但当这些结果彼此间内容重叠度高时，用户从中获取的增量信息（Incremental Information）十分有限。有效的策略不仅应提供相关的结果，还应确保结果具有多样性（Diversity），覆盖用户潜在需求的多个方面，优化信息探索的过程。

为了应对这一挑战，最大边际相关性（Maximal Marginal Relevance, MMR）算法被提出。它提供了一种结构化且有效的方法来同时优化相关性与多样性，构成了现代信息检索与推荐技术的重要组成部分。接下来，我们将深入探讨 MMR 的运作原理。

---

> ✨ 你好，我是筱可，欢迎来到「筱可 AI 研习社」！

> **🚀 标签关键词**

> | AI 实战派开发者 | 技术成长陪伴者 | RAG 前沿探索者 | 文档处理先锋 |

## 🐱 一、什么是最大边际相关性算法？

### 🐷 1.1 定义与核心理念

最大边际相关性 (MMR) 是一种用于对项目集合进行排序或选择的算法，其根本目标是在确保所选项目与用户查询（`Query`）高度相关的基础上，最大化这些项目之间的内容差异性，从而减少输出结果的冗余度。

我们可以从第一性原理的角度来理解 MMR：在从一个较大的候选池中选择一系列项目（如文档、产品、新闻摘要）构建最终列表时，理想情况下，每一步新加入的项目，不仅要满足与原始查询的相关性要求，还应提供与已选项目集合不同的新视角或信息。

这里的关键词是“**边际**”。它关注的是向当前已选集合中添加一个新项目时所带来的综合价值增量。这个增量是衡量与查询的相关性以及与已有项目的差异性的综合体现。

### 🐰 1.2 “边际”价值的构成

在 MMR 框架内，“边际相关性”具体衡量了一个候选项目在被考虑加入已选集合时，所能贡献的“净价值”。这个价值由两个相互作用的部分组成：

1.  **相关性贡献** (Relevance Contribution)：项目自身与用户查询的匹配程度。这通常由一个相关性评分函数来量化。

1.  多样性贡献 (Diversity Contribution)：项目与集合中已有项目的不相似程度。这通常通过惩罚与中最相似项目的相似度来体现，即减少冗余。

MMR 算法的设计思路就是在每一次迭代选择中，精确地挑出那个能最大化这种“边际综合价值”的候选项目。

---

## 🐶 二、为什么需要最大边际相关性算法？

对 MMR 的需求，根植于信息交互的现实逻辑和用户的信息处理特性：

1.  **优化认知负荷与信息获取效率**：面对信息流，人类的处理带宽有限。大量重复或高度相似的信息会增加认知负担，降低信息吸收效率。多样化的结果能使用户在有限交互次数内接触到更宽广、更多维度的信息，特别是在处理探索性或多方面查询（如“气候变化的影响”）时尤为重要，用户可能期望了解环境、经济、社会等不同层面的信息。

1.  **提升用户满意度与探索体验**：过度个性化或单一维度的相关性排序可能导致“信息茧房”（Filter Bubble）效应，限制用户的视野。引入多样性不仅能满足用户明确表达的需求，还能带来意外发现（Serendipity），增加探索的乐趣，从而提高用户对系统的长期满意度和粘性。

1.  **支持更优的业务决策**：在电子商务推荐或在线广告等场景，展示多样化的商品或广告创意有助于触达用户更广泛的潜在兴趣点，可能促进长尾商品的销售或提高广告活动的整体转化效果，避免流量过度集中于少数头部项目。

1.  **处理查询意图的不确定性**：用户查询往往存在歧义（Ambiguity）。例如，搜索“Python”，用户可能指编程语言，也可能指蟒蛇。通过 MMR 引入多样性，可以在结果中适当包含与不同潜在意图相关的代表性内容，从而提高覆盖用户真实意图的概率。

因此，MMR 不仅是一种排序技术的改进，它更是对“如何设计信息呈现方式以更有效地服务于复杂的用户需求和认知模式”这一问题的解答。它承认并试图解决信息需求的多维性和用户对信息效率的内在偏好。

---

## 🐠 三、平衡相关性与多样性的相关方法论

虽然本文的核心是 MMR，但了解其在更广泛的多样性优化技术谱系中的位置是有益的。平衡相关性与多样性的方法大致可分为几类：

1.  基于重排（Re-ranking）的策略：

+   MMR 是此类方法的经典代表：首先获取一个基于相关性的初始排序列表，然后应用 MMR 准则进行迭代选择和重排序，将多样性因素整合进来。

+   其他启发式或基于分类的技术：例如，可以先对初始结果进行聚类或主题建模，再从每个簇/主题中选择高质量、有代表性的项目构成最终列表。

1.  **直接优化包含多样性的目标函数**：

+   一些更复杂的模型，如**行列式点过程**(`Determinantal Point Processes`, `DPP`)，试图在模型训练或推断阶段直接优化一个同时包含相关性和多样性度量的全局目标函数。`DPP` 利用行列式的性质来优雅地刻画集合的“容量”或“多样性”，是另一个重要的多样性建模工具。

1.  **面向意图的多样化**（Intent-aware Diversification）：

+   针对具有多种解释的查询，系统首先识别出几个可能的子意图（Subtopics），然后为每个子意图检索相关结果，并根据意图的概率或重要性将这些结果组合起来，确保覆盖主要的查询方面。

MMR 因其概念直观、实现相对简单、计算开销可控而在工业界获得了广泛应用。它提供了一个清晰、可操作的框架来量化和平衡相关性与多样性这两个核心指标。

---

## 🦀 四、MMR 算法详解

### 🐙 4.1 核心数学表达

MMR 算法通过一个迭代过程来构建最终的结果列表。假设我们有一个初始候选项目集合（通常是按相关性预排序的），以及一个初始化为空的目标集合，用于存储最终被选中的项目。算法重复执行以下步骤，直至达到预定的大小：

在每次迭代中，算法从尚未被选入的候选项目集合中，选择能够最大化以下 MMR 分数的项目，并将其加入：

选择规则表达为：

这里的各个符号代表：

: 当前正在被评估的候选项目。

: 用户的查询或上下文信息。

: 已经选入最终结果列表的项目集合。

: 剩余的候选项目集合。

: **相关性评分函数**。用于计算候选项目与查询的相似度或匹配度。具体实现可以是余弦相似度、`BM25` 分数或其他任何衡量相关性的指标。值越大表示越相关。

: **冗余性**（或相似性）度量函数。计算候选项目与已选项目之间的相似度。通常采用与相同或兼容的度量方式（例如，都使用向量空间模型的余弦相似度）。值越大，表示与内容越接近，加入可能带来的冗余越高。

: 这个表达式计算了候选项目与**所有**已选项目（集合中）的相似度，并取出其中的最大值。这代表了相对于当前已选集合的最大潜在冗余度。

(`Lambda`): **平衡参数**，取值范围为 。它控制着相关性与多样性之间的权衡：

+   当  时，公式只考虑  项，MMR 退化为纯粹的相关性排序。

+   当  时，公式变为最小化与已选项目集合的最大相似度（即  ），极端情况下可能优先选择相关性较低但差异性极大的项目（不过，初始候选集  通常已保证了一定的相关性）。

+   当  时，算法在相关性得分和多样性惩罚（由最大冗余度体现）之间进行权衡。  值越高，结果越偏向相关性；值越低，结果越偏向多样性。  表示两者同等重要。

### 🦑 4.2 迭代选择流程

MMR 算法的执行流程可以概括为以下步骤：

1.  **初始化**:

+   获取初始候选项目列表  。这通常是搜索引擎或推荐系统召回和初步排序的结果，例如 `Top-K` 相关文档。

+   初始化一个空的结果列表  。

+   设定平衡参数  和期望选择的项目总数  。

1.  **选择第一个项目**:

+   由于初始时  为空，公式中的  项通常被视为 0 或一个非常小的值（不影响比较）。

+   此时，MMR 分数主要由  决定。

+   因此，选择  中  相关性最高的项目  ，将其加入  。更新  。

1.  **迭代选择后续项目 (执行  次)**:

+   对于当前候选集  中的每一个项目  ：

+   计算其与查询  的相关性得分  。

+   计算其与**当前已选集合**  中每个项目  的相似度  。

+   找到这些相似度中的最大值：  。

+   计算  的 MMR 分数：  。

+   从  中选择具有最高 MMR 分数的项目  。

+   将  从  移动到  。更新  。

1.  **终止**: 当  中的项目数量达到  时，或者候选集  为空时，算法停止。集合  中的项目，按照它们被选入的顺序，构成了最终的、兼顾相关性与多样性的结果列表。

---

## 💻 五、MMR 算法代码实现 (`Python` 示例)

**设计思路**：我们将实现一个函数 `mmr_selection`，它接收查询表示、候选项目表示、初始排名、  参数和目标数量  作为输入。函数内部将模拟 MMR 的迭代选择过程：首先选出最相关的项目，然后在后续步骤中，为每个剩余候选者计算 MMR 分数（结合其与查询的相关性及与已选项目的最大相似度），并选择分数最高的项目加入结果集，直至达到数量  。

**示例代码**：我们使用 `FlagEmbedding` 库加载 `bge-large-zh-v1.5` 模型来获取句子向量，并使用余弦相似度作为  和  的度量。



```
import numpy as np
from FlagEmbedding import FlagModel # 导入 FlagModel

def cosine_similarity(vec1, vec2):
  """计算两个 NumPy 向量的余弦相似度"""
  vec1 = np.asarray(vec1)
  vec2 = np.asarray(vec2)
  dot_product = np.dot(vec1, vec2)
  norm_vec1 = np.linalg.norm(vec1)
  norm_vec2 = np.linalg.norm(vec2)
  if norm_vec1 == 0 or norm_vec2 == 0:
    return 0.0
  similarity = dot_product / (norm_vec1 * norm_vec2)
  # 根据需要，可以考虑相似度范围，例如映射到 [0, 1]
  # return max(0.0, similarity) # 如果需要非负相似度
  return similarity # 使用原始余弦相似度 [-1, 1]

def mmr_selection(query_embedding, item_embeddings, item_ids, lambda_param, num_results):
  """
  使用 MMR 算法执行项目选择

  Args:
    query_embedding (np.array): 查询的向量表示。
    item_embeddings (dict): 候选项目向量表示的字典 {item_id: np.array}。
    item_ids (list): 初始候选项目 ID 列表 (通常是字符串ID)。
    lambda_param (float): MMR 的权衡参数 lambda (0 <= lambda <= 1)。
    num_results (int): 需要选择的结果数量 N。

  Returns:
    list: 最终选出的项目 ID 列表 (字符串ID)。
  """

  if not item_ids or not item_embeddings or num_results <= 0:
      return []

  # 筛选出有效的候选ID（存在于embeddings字典中）
  valid_candidate_ids = [id for id in item_ids if id in item_embeddings]
  if not valid_candidate_ids:
      return []

  candidate_pool = set(valid_candidate_ids)
  selected_item_ids = []

  # 预计算所有有效候选项目与查询的相关性 (Sim_1)
  candidate_relevance = {
      id: cosine_similarity(query_embedding, item_embeddings[id])
      for id in valid_candidate_ids
  }

  # 确保 N 不超过有效候选者数量
  num_results = min(num_results, len(valid_candidate_ids))

  # 第一步：选择最相关的项目
  if valid_candidate_ids:
      first_selection_id = max(candidate_relevance, key=candidate_relevance.get)
      selected_item_ids.append(first_selection_id)
      candidate_pool.remove(first_selection_id)

  # 后续迭代选择
  while len(selected_item_ids) < num_results and candidate_pool:
    mmr_scores = {}
    selected_embeddings_list = [item_embeddings[id] for id in selected_item_ids] # 获取已选项目的向量

    for candidate_id in candidate_pool:
        candidate_emb = item_embeddings[candidate_id]

        # Sim_1: 获取预计算的相关性
        relevance_score = candidate_relevance.get(candidate_id, -1.0) # 使用预计算的相关性, -1.0作为默认值

        # Sim_2: 计算与已选项目的最大相似度
        max_similarity_with_selected = -1.0 # 初始化为可能的最低余弦相似度
        if selected_item_ids: # 仅当 S 非空时计算
             similarities_to_selected = [cosine_similarity(candidate_emb, sel_emb) for sel_emb in selected_embeddings_list]
             if similarities_to_selected:
                 max_similarity_with_selected = max(similarities_to_selected)

        # 计算 MMR 分数
        # MMR Score = λ * Sim1(Di, Q) - (1 - λ) * max(Sim2(Di, Dj)) for Dj in S
        # 注意：如果 Sim1 和 Sim2 可能为负，需要确保公式逻辑正确
        mmr_score = lambda_param * relevance_score - (1 - lambda_param) * max_similarity_with_selected
        mmr_scores[candidate_id] = mmr_score

    if not mmr_scores: # 如果没有更多可计算分数的候选者
        break

    # 选择当前迭代中 MMR 分数最高的项目
    best_next_id = max(mmr_scores, key=mmr_scores.get)
    selected_item_ids.append(best_next_id)
    candidate_pool.remove(best_next_id) # 从候选池中移除

  return selected_item_ids

# --- 使用 FlagEmbedding 获取向量并运行 MMR ---

# 1. 加载模型 (请确保模型路径正确)
model_path = r"C:\Users\k\Desktop\BaiduSyncdisk\baidu_sync_documents\hf_models\bge-large-zh-v1.5"
try:
    model = FlagModel(model_path, use_fp16=True) # 尝试使用 FP16 加速
    print("模型加载成功。")
except Exception as e:
    print(f"模型加载失败: {e}")
    # 在此可以添加退出或使用备用逻辑
    exit() # 或者 return, raise e 等

# 2. 定义查询和候选句子
query_sentence = "大型语言模型有哪些应用？"

candidate_sentences = [
    # 与查询直接相关 - 应用类
    "大语言模型可用于文本生成，例如写诗歌或代码。", # id=s1
    "机器翻译是大语言模型的常见应用场景之一。",     # id=s2
    "聊天机器人和智能客服常常基于大型语言模型构建。",# id=s3
    "大型模型能够进行文本摘要和信息抽取。",         # id=s4

    # 与查询相关 - 原理/定义类 (与应用类有差异)
    "大型语言模型通常指参数量巨大的深度学习模型。", # id=s5
    "Transformer架构是现代大语言模型的基础。",       # id=s6
    "训练大型语言模型需要海量的文本数据和计算资源。",# id=s7

    # 不太相关或离题
    "今天天气真不错。",                           # id=s8
    "人工智能的研究历史悠久。",                   # id=s9
]
# 为句子分配 ID
candidate_ids = [f"s{i+1}" for i in range(len(candidate_sentences))]

# 创建ID到句子的映射字典
id_to_sentence = {candidate_ids[i]: candidate_sentences[i] for i in range(len(candidate_sentences))}

# 3. 获取所有句子的嵌入向量
all_sentences = [query_sentence] + candidate_sentences
print("开始计算嵌入向量...")
embeddings = model.encode(all_sentences)
print(f"嵌入向量计算完成，形状: {embeddings.shape}") # 应为 (1 + len(candidate_sentences), 1024)

query_embedding = embeddings[0]
item_embeddings_dict = {candidate_ids[i]: embeddings[i+1] for i in range(len(candidate_sentences))}

# 4. 设定参数并运行 MMR
# 假设初始列表基于某种粗排得到（这里简化为原始顺序）
initial_ranked_ids = candidate_ids
num_select = 5 # 期望选出5个结果

# 场景1: 更注重相关性
lambda_high = 0.7
selected_high_lambda = mmr_selection(query_embedding, item_embeddings_dict, initial_ranked_ids, lambda_high, num_select)
print(f"\n--- MMR 选择结果 (lambda={lambda_high}, N={num_select}) ---")
print("选定句子ID:", selected_high_lambda)
print("选定句子内容:")
for i, item_id in enumerate(selected_high_lambda):
    print(f"{i+1}. ID={item_id}: {id_to_sentence[item_id]}")

# 场景2: 更注重多样性
lambda_low = 0.3
selected_low_lambda = mmr_selection(query_embedding, item_embeddings_dict, initial_ranked_ids, lambda_low, num_select)
print(f"\n--- MMR 选择结果 (lambda={lambda_low}, N={num_select}) ---")
print("选定句子ID:", selected_low_lambda)
print("选定句子内容:")
for i, item_id in enumerate(selected_low_lambda):
    print(f"{i+1}. ID={item_id}: {id_to_sentence[item_id]}")
```

## 代码说明与分析

上述代码首先加载了 `bge-large-zh-v1.5` 模型。然后，定义了一个查询句和一组包含不同类别（应用、原理、无关）的候选句子。通过 `model.encode()` 计算得到所有句子的嵌入向量。最后，调用 `mmr_selection` 函数两次，分别使用高 (0.7) 和低 (0.3) 来选择 Top 5 的句子。

输出：

```
嵌入向量计算完成，形状: (10, 1024)

--- MMR 选择结果 (lambda=0.7, N=5) ---
选定句子ID: ['s5', 's1', 's2', 's3', 's7']
选定句子内容:
1. ID=s5: 大型语言模型通常指参数量巨大的深度学习模型。
2. ID=s1: 大语言模型可用于文本生成，例如写诗歌或代码。
3. ID=s2: 机器翻译是大语言模型的常见应用场景之一。
4. ID=s3: 聊天机器人和智能客服常常基于大型语言模型构建。
5. ID=s7: 训练大型语言模型需要海量的文本数据和计算资源。

--- MMR 选择结果 (lambda=0.3, N=5) ---
选定句子ID: ['s5', 's8', 's9', 's1', 's6']
选定句子内容:
1. ID=s5: 大型语言模型通常指参数量巨大的深度学习模型。
2. ID=s8: 今天天气真不错。
3. ID=s9: 人工智能的研究历史悠久。
4. ID=s1: 大语言模型可用于文本生成，例如写诗歌或代码。
5. ID=s6: Transformer架构是现代大语言模型的基础。
```

## 结果分析

通过对比 `λ=0.7` 和 `λ=0.3` 的 `MMR` 选择结果，我们可以清晰观察算法如何在相关性与多样性之间权衡：

### 1. 高 `λ` 值 (0.7)：偏向相关性

+   **特点**：选出的 5 个结果全部与查询“大型语言模型有哪些应用？”高度相关。

+   **内容分布**：包括“大语言模型可用于文本生成，例如写诗歌或代码”、“机器翻译是大语言模型的常见应用场景之一”、“聊天机器人和智能客服常常基于大型语言模型构建”等应用场景，以及“大型语言模型通常指参数量巨大的深度学习模型”和“训练大型语言模型需要海量的文本数据和计算资源”等原理描述。

+   **效果**：结果聚焦查询主题，提供实用且互补的信息，适合需要深度相关内容的场景。

### 2. 低 `λ` 值 (0.3)：偏向多样性

+   **特点**：结果既有与查询相关的句子，也有完全无关的内容，但是很明显，无关的内容直接排到第二了，说明这里的参数设置得过于多样化了。

+   **内容分布**：首选“大型语言模型通常指参数量巨大的深度学习模型”作为最相关项，随后加入“今天天气真不错”和“人工智能的研究历史悠久”等无关句子，最后补选“大型语言模型可用于文本生成，例如写诗歌或代码”和“`Transformer` 架构是现代大语言模型的基础”等相关但主题分散的内容。

+   **效果**：牺牲部分相关性换取更高多样性，适合探索性场景或查询有歧义时覆盖多方面信息。

### 3. 共同点与机制

+   **首选一致**：两种 `λ` 值下，首个选择均为“大型语言模型通常指参数量巨大的深度学习模型”，因初始步骤只考虑相关性（`Sim_1`）。

+   **迭代逻辑**：后续选择中，`λ` 控制相关性 (`λ` `Sim_1`) 与多样性 ((1-`λ`) `max(Sim_2)`) 的权重：

+   `λ=0.7`：相关性权重 0.7，多样性 0.3，倾向选择与查询紧密相关的句子。

+   `λ=0.3`：多样性权重 0.7，相关性 0.3，倾向选择与已选内容差异大的句子。

需要注意的是：不同的模型需要使用适当调整多样性权重。

### 4. 核心优势

`MMR` 通过 `λ` 参数灵活调节相关性与多样性的平衡：

+   高 `λ` 适合精准信息检索，如聚焦大型语言模型的具体应用。

+   低 `λ` 适用于多样化推荐，如探索不同主题或背景知识。

在实际应用中，`λ` 的选择应结合业务需求（如深度 vs 广度）和多数用户意图动态调整。

## 🚀 六、`MMR` 算法的应用场景

`MMR` 的核心思想——平衡相关性与多样性——使其在众多信息服务场景中具有广泛的应用价值：

1.  搜索引擎结果页 (`SERP`) 优化：

+   当用户查询词具有多重含义或指向多个子主题时（例如，“Jaguar”可能指汽车品牌或动物），`MMR` 可以帮助确保搜索结果不仅相关，而且能覆盖这些不同的方面，提供更全面的信息视图。

1.  **推荐系统** (`Recommender Systems`):

+   **商品推荐**：避免向用户展示一系列极其相似的商品。例如，在推荐服装时，可以利用 `MMR` 推荐不同款式、颜色或品牌的衣服，而不是多件同款不同色的 T 恤。

+   **内容（新闻/视频/音乐）推荐**：对于同一事件或主题，推送不同视角、来源或风格的内容，避免信息茧房，提升用户发现新内容的体验。

1.  **自动文本摘要** (`Text Summarization`):



# MMR 最大边际相关性：应用、总结与实践

## 典型应用场景

在抽取式摘要任务中，需要从原文中选择若干句子构成摘要。`MMR` 可用于选择那些既能反映原文核心要点（与原文或主题具有高  ），又彼此间信息重叠较少（句子间   较低）的关键句子。

### 计算广告 (Computational Advertising)

广告创意轮播与多样化：对于同一广告系列，系统可以使用 `MMR` 来选择向特定用户展示的广告创意组合，确保创意之间具有一定的差异性（如不同的视觉元素、文案或促销点），以减少用户疲劳，并探索不同创意的有效性。

### 检索增强生成 (RAG) 中的上下文选择

在 `RAG` 架构中，检索模块获取的文档片段（`Chunks`）若高度相似，将为后续的大语言模型（`LLM`）提供冗余的上下文信息。应用 `MMR` 对检索到的 `Chunks` 进行重排，可以选择出既与原始查询相关、又包含互补信息的 `Chunks` 集合，从而提升 `LLM` 生成答案的质量和全面性。

---

> 🙋♂️ 入群交流

+ 👸号菜单点击「社群」，扫码直接入群

+ 回复关键词「入群」，添加作者🛰️人工邀请，注意备注：入群

## 📒 总结与展望

### 🚀 技术全景图

`MMR` 是在信息过载背景下，对传统相关性至上排序范式进行修正和优化的关键技术之一，其核心在于引入并量化了多样性维度。

它与多样性度量方法、重排序算法框架、用户意图识别等研究领域密切相关，是构建智能信息系统的重要工具。

在现代多阶段排序架构（如召回-粗排-精排）中，`MMR` 常被应用于精排或重排阶段，对候选结果进行最终的优化调整。

尽管存在如 `DPP` 等更复杂的概率模型来建模多样性，`MMR` 凭借其直观性、计算效率和易于部署的特点，在工业界仍然保持着广泛应用和持续影响力。

### 📓 学习汇总

+ 核心权衡：在信息排序中平衡相关性与多样性，避免结果单一化和信息冗余。

+ `MMR` 机制：通过参数  ，在每次迭代选择中对候选项目的相关性贡献 ( ) 和冗余惩罚 (基于  ) 进行线性组合，选出边际价值最高的项目。

+ 关键组件：查询  ，候选集  ，已选集  ，相关性函数  ，相似性（冗余）函数  ，平衡参数  。

+ 实践价值：应用于搜索、推荐、摘要等场景，能够显著提升结果的信息覆盖度、用户满意度以及探索发现的效率。

### 🔥 动手挑战

1. 动手实现与参数探索：利用提供的 `Python` 代码框架，尝试结合实际的文本或项目嵌入（如使用 `TF-IDF`、`Word2Vec` 或 `Sentence-BERT` (`SBERT`) 生成的向量），在特定数据集上实现 `MMR`。通过调整  值，观察并分析最终排序列表内容的变化规律。

1. 效果评估对比：设计实验，将 `MMR` 排序结果与基线方法（如纯粹按   相关性排序）进行比较。考虑使用包含多样性考量的评估指标（如  -`NDCG` 或 `Precision/Recall@K` 的用户感知多样性版本）来量化 `MMR` 带来的改进。

1. 场景迁移与应用：反思您当前的工作中，是否存在可以引入 `MMR` 或类似多样性优化思想的环节？如何针对具体场景定义恰当的  （相关性）和  （相似性/冗余性）度量？

### ♻️ 互动问题

1. 问题 1：`MMR` 算法致力于解决的核心问题是什么？它是通过何种机制来实现这一目标的？

1. 问题 2：在 `MMR` 的数学表达式中，平衡参数  扮演了什么角色？当  的值分别设定为 1 和 0 时，算法的行为特征是怎样的？

1. 问题 3：请列举一个除搜索引擎结果排序之外的 `MMR` 应用实例，并具体说明在该场景下，  和  可以如何定义？

> 欢迎在留言区分享你的想法，每条留言我都会认真阅读！

> 你的反馈是我创作的最大动力 ❤️

> 💗 立即行动

1. 点赞、收藏、关注，分享给朋友。

1. 为账号加星标，获取更多内容。

1. 以本文为灵感，整理一份属于自己的笔记。

> 🚗 行动召唤

> "与其等着 AI 改变世界，不如自己参与变革！在这里，让 AI 成为你弯道超车的秘密武器。"

## 📚 往期精选

+ 你的`RAG`检索太“笨”？用`K-Means`聚类来“调教”

+ [`RAG`之延迟交互与残差压缩：从`ColBERT`到`ColBERTv2`的演进及其应用](https://mp.weixin.qq.com/s/2-9FqIFxgWM10X9lnora4A)

+ `RAG`：`ColBERT`原理、延迟交互机制与稠密向量的对比分析

+ `RAG`背后的数学：向量点积与余弦的秘密

+ `RAG`系统召回率低？ `BGE-M3` 多语言多功能重塑文本检索
