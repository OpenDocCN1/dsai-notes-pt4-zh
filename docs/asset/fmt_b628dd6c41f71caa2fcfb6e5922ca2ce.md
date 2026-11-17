
# RAG 架构设计文档

## 第二层：文件处理与 Agent 编排

+   **向量化**：调用 `Embedding` 模型生成向量。
+   **兼容性保障**：处理 Windows 路径问题，生成 `index.faiss` 和 `metadata.json`。
+   `agent_service.py` (智能编排)
+   **中间件逻辑**：在运行时读取 `metadata.json`，动态生成 `System Prompt`。
+   **动态工具绑定**：利用闭包（`Closure`）在函数内部动态定义 `retrieve_context` 工具，并绑定当前的向量库。
+   **执行与解析**：运行 `Agent`，并解析复杂的 `content_and_artifact` 结构，分离出答案和引用来源。

### 第三层：核心配置层 (Core Layer)

+   **文件**：`app/core/config.py`
+   **职责**：管理全局单例。确保 `DeepSeek LLM` 和 `OpenAI Embedding` 模型只被初始化一次，避免重复开销。

### 第四层：数据契约层 (Schemas Layer)

+   **文件**：`app/schemas/api_schemas.py`
+   **职责**：定义“共同语言”。使用 `Pydantic` 严格定义了前端传什么（如 `kb_name`）、后端回什么（如 `DocSource`），确保前后端联调顺畅。

## 3. 数据流转全生命周期 (Data Flow)

当用户发起一次“关联知识库的对话”请求时，数据是如何流转的？

1.  **`Request`**: 前端发送 `POST /api/chat`，携带 `{ "query": "病假", "kb_name": "staff_kb" }`。
1.  **`Routing`**: `main.py` 接收请求，转发给 `endpoints.py`。
1.  **`Validation`**: `ChatRequest` Schema 验证数据格式是否合法。
1.  Service Invocation: 接口层调用 `AgentService.chat_with_agent()`。
1.  Middleware Execution (`AgentService`):
    +   加载 `staff_kb/metadata.json`，发现主题包含“休假制度”。
    +   生成 `System Prompt`：“你是一个基于‘休假制度’知识库的助手...”。
    +   加载 `FAISS` 索引，动态创建 `retrieve_context` 工具。
1.  **`Agent Reasoning`**: `DeepSeek` 模型思考 -> 决定调用工具 -> 获得检索结果（含 `Score`）。
1.  Response Construction: `AgentService` 解析执行结果，分离出 `Answer` 和 `Artifacts`。
1.  **`Return`**: 接口层将结果封装为 `JSON` 返回前端。

## 4. 架构设计总结

+   模块化 (`Modularity`)：文件处理与对话逻辑完全分离，修改向量库逻辑不会影响 `Agent` 对话逻辑。
+   可扩展性 (`Extensibility`)：如果未来要换成 `ChromaDB` 或其他 `LLM`，只需修改 `file_service.py` 或 `config.py`，无需改动上层业务。
+   标准化 (`Standardization`)：严格遵循 `FastAPI` 和 `LangChain` 的最佳实践，代码清晰、易读、易维护。

这套架构不仅仅是教学演示，更是一个轻量级企业级 `RAG` 应用的标准模板。

### 核心代码解释

```python
from typing import List, Optional, Tuple
from langchain.tools import tool
from langchain.agents import create_agent
from langchain_core.messages import AIMessage, ToolMessage
from langchain_core.documents import Document

from app.core.config import get_llm
from app.services.file_service import FileService
from app.schemas.api_schemas import DocSource

class AgentService:
    
    @staticmethod
    def chat_with_agent(query: str, kb_name: Optional[str], top_k: int) -> Tuple[str, List[DocSource]]:
        """
        Agentic RAG 主流程：
        1. 动态加载 Metadata 
        2. 动态生成 System Prompt
        3. 动态绑定 VectorStore Tool
        """
        llm = get_llm()
        tools = []
        
        # 默认 System Prompt
        system_context = "你是一名乐于助人的AI助手，请直接回答用户的问题。用户可以上传文档，你会基于用户上传的文档知识进行回答。"

        # === 中间件逻辑：如果有知识库，则注入上下文 ===
        if kb_name:
            vector_store = FileService.load_vector_store(kb_name)
            if vector_store:
                
                # 1. 读取元数据，构建动态 Prompt
                metadata = FileService.load_kb_metadata(kb_name)
                topics = metadata.get("topics", [])
                topics_str = "、".join(topics) if topics else "通用文档"
                
                system_context = (
                    f"你是一名基于知识库【{kb_name}】的智能助手。\n"
                    f"该知识库主要包含以下主题内容：**{topics_str}**。\n"
                    "当用户的问题涉及到上述内容或细节时，请务必调用 retrieve_context 工具检索信息来回答。\n"
                    "如果问题与知识库无关（例如闲聊），请用你的通用知识回答，并简要告知用户该问题超出了当前知识库范围。"
                )

                # 2. 定义绑定了当前 vector_store 的工具
                @tool(response_format="content_and_artifact")
                def retrieve_context(search_query: str):
                    """Retrieve information to help answer a query."""
                    # 使用 with_score 是为了给前端提供置信度，虽然 LLM 主要看 content
                    docs_and_scores = vector_store.similarity_search_with_score(search_query, k=top_k)
                    
                    # 序列化给 LLM 看 (仅文本)
                    serialized = "\n\n".join(
                        (f"Source: {doc.metadata}\nContent: {doc.page_content}")
                        for doc, score in docs_and_scores
                    )
                    
                    # 构造 Artifact (包含分数，给前端用)
                    artifacts = []
                    for doc, score in docs_and_scores:
                        # 兼容处理：确保 artifact 里存的是易于解析的对象或原始 Document
                        # 这里我们存原始 Document 对象，稍后在外部解析
                        # 为了携带 score，我们动态给 doc 加个属性，或者封装一下
                        doc.metadata["score"] = float(score) # 将分数注入 metadata 方便携带
                        artifacts.append(doc)
                    
                    return serialized, artifacts
                
                tools = [retrieve_context]

        # === 创建 Agent ===
        # 使用 create_agent (LangChain 1.1 标准)
        agent = create_agent(llm, tools, system_prompt=system_context)

        # === 执行 Agent ===
        messages = [{"role": "user", "content": query}]
        response = agent.invoke({"messages": messages})
        
        # === 解析结果 ===
        # 从 response['messages'] 中提取最终回答和 Artifact
        final_answer = ""
        sources = []

        if "messages" in response:
            msg_list = response["messages"]
            
            # 1. 获取最后一条 AI 回复
            last_msg = msg_list[-1]
            if isinstance(last_msg, AIMessage):
                final_answer = last_msg.content

            # 2. 遍历获取 ToolMessage 中的 Artifact
            for msg in msg_list:
                if isinstance(msg, ToolMessage) and msg.artifact:
                    for doc in msg.artifact:
                        if isinstance(doc, Document):
                            # 从 metadata 中取出我们刚才塞进去的 score
                            score = doc.metadata.get("score", 0.0)
                            
                            sources.append(DocSource(
                                content=doc.page_content,
                                metadata=doc.metadata,
                                score=score
                            ))
        
        return final_answer, sources

    @staticmethod
    def recall_test(kb_name: str, query: str, top_k: int) -> List[DocSource]:
        """
        召回测试 (不走 Agent，直接查向量库)
        """
        vector_store = FileService.load_vector_store(kb_name)
        if not vector_store:
            raise ValueError(f"Knowledge base '{kb_name}' not found.")
            
        docs_and_scores = vector_store.similarity_search_with_score(query, k=top_k)
        
        results = []
        for doc, score in docs_and_scores:
            results.append(DocSource(
                content=doc.page_content,
                metadata=doc.metadata,
                score=float(score)
            ))
        return results
```

### 模块一：动态上下文注入 (Dynamic Context Injection)

> **核心概念**：拒绝千篇一律的 `System Prompt`，根据知识库内容实时调整 AI 人设。

在 `chat_with_agent` 函数的开头，我们并没有使用写死的 `Prompt`，而是引入了一个“中间件”逻辑：

```python
# === 中间件逻辑 ===
if kb_name:
    # 1. 实时读取磁盘上的元数据 (metadata.json)
    metadata = FileService.load_kb_metadata(kb_name)
    topics = metadata.get("topics", [])
    
    # 2. 动态组装 System Prompt
    system_context = (
        f"你是一名基于知识库【{kb_name}】的智能助手。\n"
        f"该知识库主要包含以下主题内容：**{topics_str}**。\n"
        "..."
    )
```

+   **技术解读**：
    +   按需加载：只有当用户指定了 `kb_name` 时，系统才会去加载对应的向量库和元数据。
    +   `Prompt` 模板化：利用 Python 的 `f-string`，将提取到的 topics（如“薪资管理”、“API接口”）嵌入到系统提示词中。
    +   价值：这让 `Agent` 具备了**元认知能力**。它知道自己“懂什么”，从而在回答问题时更自信，或者在遇到无关问题时能准确拒绝。

### 模块二：运行时工具绑定 (Runtime Tool Binding)

> **核心概念**：利用 Python 的 `闭包 (Closure)` 特性，为每一次对话创建专属的检索工具。

请注意 `retrieve_context` 函数定义的位置——它是在 `chat_with_agent` 函数内部定义的，而不是全局定义的。

```python
# 在函数内部定义 Tool
@tool(response_format="content_and_artifact")
def retrieve_context(search_query: str):
    # 这里直接使用了外部作用域的 vector_store 变量
    docs_and_scores = vector_store.similarity_search_with_score(search_query, k=top_k)
    # ...
```

+   **技术解读**：
    +   闭包机制：这个 `Tool` 捕获了当前请求上下文中的 `vector_store` 和 `top_k` 参数。
