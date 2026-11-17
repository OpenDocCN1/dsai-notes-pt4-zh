
```python
def search_vectors_expanded(
    store = None,
    query: str = "",
    top_k: int = 8,
    rag_namespace: Optional[str] = None,
    only_rag_data: bool = True,
    score_threshold: Optional[float] = None,
    enable_mqe: bool = False,
    mqe_expansions: int = 2,
    enable_hyde: bool = False,
    candidate_pool_multiplier: int = 4,
) -> List[Dict]:
    """
    Search with query expansion using unified embedding and Qdrant.
    """
    if not query:
        return []
    # 创建默认存储
    if store is None:
        store = _create_default_vector_store()
    # 查询扩展
    expansions: List[str] = [query]
    if enable_mqe and mqe_expansions > 0:
        expansions.extend(_prompt_mqe(query, mqe_expansions))
    if enable_hyde:
        hyde_text = _prompt_hyde(query)
        if hyde_text:
            expansions.append(hyde_text)
    # 去重和修剪
    uniq: List[str] = []
    for e in expansions:
        if e and e not in uniq:
            uniq.append(e)
    expansions = uniq[: max(1, len(uniq))]
    # 分配候选池
    pool = max(top_k * candidate_pool_multiplier, 20)
    per = max(1, pool // max(1, len(expansions)))
    # 构建RAG数据过滤器
    where = {"memory_type": "rag_chunk"}
    if only_rag_data:
        where["is_rag_data"] = True
        where["data_source"] = "rag_pipeline"
    if rag_namespace:
        where["rag_namespace"] = rag_namespace

    # 收集所有扩展查询的结果
    agg: Dict[str, Dict] = {}
    for q in expansions:
        qv = embed_query(q)
        hits = store.search_similar(
            query_vector=qv,
            limit=per,
            score_threshold=score_threshold,
            where=where
        )
        for h in hits:
            mid = h.get("metadata", {}).get("memory_id", h.get("id"))
            s = float(h.get("score", 0.0))
            if mid not in agg or s > float(agg[mid].get("score", 0.0)):
                agg[mid] = h

    # 按分数排序返回
    merged = list(agg.values())
    merged.sort(key=lambda x: float(x.get("score", 0.0)), reverse=True)
    return merged[:top_k]
```

实际应用中，这三种策略的组合使用效果最佳。`MQE`擅长处理用词多样性问题，`HyDE`擅长处理语义鸿沟问题，而统一框架则确保了结果的质量和多样性。对于一般查询，建议启用`MQE`；对于专业领域查询，建议同时启用`MQE`和`HyDE`；对于性能敏感场景，可以只使用基础检索或仅启用`MQE`。

当然还有很多有趣的方法，这里只是为大家适当的扩展介绍，在实际的使用场景里也需要去尝试寻找适合问题的解决方案。

## [8.4 构建智能文档问答助手](https://datawhalechina.github.io/hello-agents/#/./chapter8/%E7%AC%AC%E5%85%AB%E7%AB%A0%20%E8%AE%B0%E5%BF%86%E4%B8%8E%E6%A3%80%E7%B4%A2?id=_84-%e6%9e%84%e5%bb%ba%e6%99%ba%e8%83%bd%e6%96%87%e6%a1%a3%e9%97%ae%e7%ad%94%e5%8a%a9%e6%89%8b)

在前面的章节中，我们详细介绍了`HelloAgents`的记忆系统和`RAG`系统的设计与实现。现在，让我们通过一个完整的实战案例，展示如何将这两个系统有机结合，构建一个智能文档问答助手。

### [8.4.1 案例背景与目标](https://datawhalechina.github.io/hello-agents/#/./chapter8/%E7%AC%AC%E5%85%AB%E7%AB%A0%20%E8%AE%B0%E5%BF%86%E4%B8%8E%E6%A3%80%E7%B4%A2?id=_841-%e6%a1%88%e4%be%8b%e8%83%8c%e6%99%af%e4%b8%8e%e7%9b%ae%e6%a0%87)

在实际工作中，我们经常需要处理大量的技术文档、研究论文、产品手册等`PDF`文件。传统的文档阅读方式效率低下，难以快速定位关键信息，更无法建立知识间的关联。

本案例将基于`Datawhale`另外一门动手学大模型[教程](https://github.com/datawhalechina/happy-llm/releases/download/v1.0.1/Happy-LLM-0727.pdf)`Happy-LLM`的公测`PDF`文档`Happy-LLM-0727.pdf`为例，构建一个基于`Gradio`的`Web`应用，展示如何使用`RAGTool`和`MemoryTool`构建完整的交互式学习助手。`PDF`可在这个链接获取。

我们希望实现以下功能：

1. 智能文档处理：使用`MarkItDown`实现`PDF`到`Markdown`的统一转换，基于`Markdown`结构的智能分块策略，高效的向量化和索引构建

1. 高级检索问答：多查询扩展（`MQE`）提升召回率，假设文档嵌入（`HyDE`）改善检索精度，上下文感知的智能问答

1. 多层次记忆管理：工作记忆管理当前学习任务和上下文，情景记忆记录学习事件和查询历史，语义记忆存储概念知识和理解，感知记忆处理文档特征和多模态信息

1. 个性化学习支持：基于学习历史的个性化推荐，记忆整合和选择性遗忘，学习报告生成和进度追踪

为了更清晰地展示整个系统的工作流程，图`8.6`展示了五个步骤之间的关系和数据流动。五个步骤形成了一个完整的闭环：步骤`1`将`PDF`文档处理后的信息记录到记忆系统，步骤`2`的检索结果也会记录到记忆系统，步骤`3`展示记忆系统的完整功能（添加、检索、整合、遗忘），步骤`4`整合`RAG`和`Memory`提供智能路由，步骤`5`收集所有统计信息生成学习报告。

![图 8.6 智能问答助手的五步执行流程](https://raw.githubusercontent.com/datawhalechina/Hello-Agents/main/docs/images/8-figures/8-6.png)

接下来，我们将展示如何实现这个`Web`应用。整个应用分为三个核心部分：

1. 核心助手类（`PDFLearningAssistant`）：封装`RAGTool`和`MemoryTool`的调用逻辑

1. `Gradio` `Web`界面：提供友好的用户交互界面，这个部分可以参考示例代码学习

1. 其他核心功能：笔记记录、学习回顾、统计查看和报告生成

### [8.4.2 核心助手类的实现](https://datawhalechina.github.io/hello-agents/#/./chapter8/%E7%AC%AC%E5%85%AB%E7%AB%A0%20%E8%AE%B0%E5%BF%86%E4%B8%8E%E6%A3%80%E7%B4%A2?id=_842-%e6%a0%b8%e5%bf%83%e5%8a%a9%e6%89%8b%e7%b1%bb%e7%9a%84%e5%ae%9e%e7%8e%b0)

首先，我们实现核心的助手类`PDFLearningAssistant`，它封装了`RAGTool`和`MemoryTool`的调用逻辑。

#### （1）类的初始化

```python
class PDFLearningAssistant:
    """智能文档问答助手"""

    def __init__(self, user_id: str = "default_user"):
        """初始化学习助手

        Args:
            user_id: 用户ID，用于隔离不同用户的数据
        """
        self.user_id = user_id
        self.session_id = f"session_{datetime.now().strftime('%Y%m%d_%H%M%S')}"
        # 初始化工具
        self.memory_tool = MemoryTool(user_id=user_id)
        self.rag_tool = RAGTool(rag_namespace=f"pdf_{user_id}")
        # 学习统计
        self.stats = {
            "session_start": datetime.now(),
            "documents_loaded": 0,
            "questions_asked": 0,
            "concepts_learned": 0,
        }
        # 当前加载的文档
        self.current_document = None
```

在这个初始化过程中，我们做了几个关键的设计决策：

`MemoryTool`的初始化：通过`user_id`参数实现用户级别的记忆隔离。不同用户的学习记忆是完全独立的，每个用户都有自己的工作记忆、情景记忆、语义记忆和感知记忆空间。

`RAGTool`的初始化：通过`rag_namespace`参数实现知识库的命名空间隔离。使用`f"pdf_{user_id}"`作为命名空间，每个用户都有自己独立的`PDF`知识库。

会话管理：`session_id`用于追踪单次学习会话的完整过程，便于后续的学习历程回顾和分析。

统计信息：`stats`字典记录关键的学习指标，用于生成学习报告。

#### （2）加载`PDF`文档

```python
def load_document(self, pdf_path: str) -> Dict[str, Any]:
    """加载`PDF`文档到知识库

    Args:
        pdf_path: `PDF`文件路径

    Returns:
        Dict: 包含`success`和`message`的结果
    """
    if not os.path.exists(pdf_path):
        return {"success": False, "message": f"文件不存在: {pdf_path}"}

    start_time = time.time()
    # 【RAGTool】处理PDF: MarkItDown转换 → 智能分块 → 向量化
    result = self.rag_tool.execute(
        "add_document",
        file_path=pdf_path,
        chunk_size=1000,
        chunk_overlap=200,
    )

    process_time = time.time() - start_time

    if result.get("success", False):
        self.current_document = os.path.basename(pdf_path)
        self.stats["documents_loaded"] += 1
        # 【MemoryTool】记录到学习记忆
        self.memory_tool.execute(
            "add",
            content=f"加载了文档《{self.current_document}》",
            memory_type="episodic",
            importance=0.9,
            event_type="document_loaded",
            session_id=self.session_id,
        )
        return {
            "success": True,
            "message": f"加载成功！(耗时: {process_time:.1f}秒)",
            "document": self.current_document,
        }
    else:
        return {"success": False, "message": f"加载失败: {result.get('error', '未知错误')}"}
```

我们通过一行代码就能完成`PDF`的处理：

```python
result = self.rag_tool.execute(
    "add_document",
    file_path=pdf_path,
    chunk_size=1000,
    chunk_overlap=200,
)
```

这个调用会触发`RAGTool`的完整处理流程（`MarkItDown`转换、增强处理、智能分块、向量化存储），这些内部细节在`8.3`节已经详细介绍过。我们只需要关注：

+ 操作类型：`"add_document"` - 添加文档到知识库

+ 文件路径：`file_path` - `PDF`文件的路径

+ 分块参数：`chunk_size=1000`, `chunk_overlap=200` - 控制文本分块

+ 返回结果：包含处理状态和统计信息的字典

文档加载成功后，我们使用`MemoryTool`记录到情景记忆：

```python
self.memory_tool.execute(
    "add",
    content=f"加载了文档《{self.current_document}》",
    memory_type="episodic",
    importance=0.9,
    event_type="document_loaded",
    session_id=self.session_id,
)
```

为什么用情景记忆？ 因为这是一个具体的、有时间戳的事件，适合用情景记忆记录。`session_id`参数将这个事件关联到当前学习会话，便于后续回顾学习历程。

这个记忆记录为后续的个性化服务奠定了基础：

+ 用户询问"我之前加载过哪些文档？" → 从情景记忆中检索

+ 系统可以追踪用户的学习历程和文档使用情况

### [8.4.3 智能问答功能](https://datawhalechina.github.io/hello-agents/#/./chapter8/%E7%AC%AC%E5%85%AB%E7%AB%A0%20%E8%AE%B0%E5%BF%86%E4%B8%8E%E6%A3%80%E7%B4%A2?id=_843-%e6%99%ba%e8%83%bd%e9%97%ae%e7%ad%94%e5%8a%9f%e8%83%bd)

文档加载完成后，用户就可以向文档提问了。我们实现一个`ask`方法来处理用户的问题：
