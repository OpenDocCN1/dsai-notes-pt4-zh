
## LangGraph 工作流构建

```
import pprint

from langgraph.graph import END, StateGraph

workflow = StateGraph(GraphState)

# Define the nodes
workflow.add_node("web_search", web_search) # web search
workflow.add_node("retrieve", retrieve) # retrieve
workflow.add_node("grade_documents", grade_documents) # grade documents
workflow.add_node("generate", generate) # rag
workflow.add_node("llm_fallback", llm_fallback) # llm

# Build graph
workflow.set_conditional_entry_point(
    route_question,
    {
        "web_search": "web_search",
        "vectorstore": "retrieve",
        "llm_fallback": "llm_fallback",
    },
)
workflow.add_edge("web_search", "generate")
workflow.add_edge("retrieve", "grade_documents")
workflow.add_conditional_edges(
    "grade_documents",
    decide_to_generate,
    {
        "web_search": "web_search",
        "generate": "generate",
    },
)
workflow.add_conditional_edges(
    "generate",
    grade_generation_v_documents_and_question,
    {
        "not supported": "generate", # Hallucinations: re-generate 
        "not useful": "web_search", # Fails to answer question: fall-back to web-search 
        "useful": END,
    },
)
workflow.add_edge("llm_fallback", END)

# Compile
app = workflow.compile()
```

## 测试

分别测试三种复杂度`query`检索的情况。

### 网页检索

```
# Run
inputs = {"question": "What player are the Bears expected to draft first in the 2024 NFL draft?"}
for output in app.stream(inputs):
    for key, value in output.items():
        # Node
        pprint.pprint(f"Node '{key}':")
        # Optional: print full state at each node
    pprint.pprint("\n---\n")

# Final generation
pprint.pprint(value["generation"])
```

打印结果

```
---ROUTE QUESTION---
---ROUTE QUESTION TO WEB SEARCH---
---WEB SEARCH---
"Node 'web_search':"
'\n---\n'
---GENERATE---
---CHECK HALLUCINATIONS---
---DECISION: GENERATION IS GROUNDED IN DOCUMENTS---
---GRADE GENERATION vs QUESTION---
---DECISION: GENERATION ADDRESSES QUESTION---
"Node 'generate':"
'\n---\n'
'The Bears are expected to draft Caleb Williams with their first pick.'
```

### RAG

```
# Run
inputs = {"question": "What are the types of agent memory?"}
for output in app.stream(inputs):
    for key, value in output.items():
        # Node
        pprint.pprint(f"Node '{key}':")
        # Optional: print full state at each node
        # pprint.pprint(value["keys"], indent=2, width=80, depth=None)
    pprint.pprint("\n---\n")

# Final generation
pprint.pprint(value["generation"])
```

打印结果

```
---ROUTE QUESTION---
---ROUTE QUESTION TO RAG---
---RETRIEVE---
"Node 'retrieve':"
'\n---\n'
---CHECK DOCUMENT RELEVANCE TO QUESTION---
---GRADE: DOCUMENT RELEVANT---
---GRADE: DOCUMENT RELEVANT---
---GRADE: DOCUMENT RELEVANT---
---GRADE: DOCUMENT RELEVANT---
---ASSESS GRADED DOCUMENTS---
---DECISION: GENERATE---
"Node 'grade_documents':"
'\n---\n'
---GENERATE---
---CHECK HALLUCINATIONS---
---DECISION: GENERATION IS GROUNDED IN DOCUMENTS---
---GRADE GENERATION vs QUESTION---
---DECISION: GENERATION ADDRESSES QUESTION---
"Node 'generate':"
'\n---\n'
'Sensory, short-term, and long-term memory.'
```

### LLM直接回答

```
# Run
inputs = {"question": "Hello, how are you today?"}
for output in app.stream(inputs):
    for key, value in output.items():
        # Node
        pprint.pprint(f"Node '{key}':")
        # Optional: print full state at each node
        # pprint.pprint(value["keys"], indent=2, width=80, depth=None)
    pprint.pprint("\n---\n")

# Final generation
pprint.pprint(value["generation"])
```

打印结果

```
---ROUTE QUESTION---
---ROUTE QUESTION TO LLM---
---LLM Fallback---
"Node 'llm_fallback':"
'\n---\n'
("I don't have feelings as an AI assistant, but I'm here to help you with your "
 'queries. How can I assist you today?')
```

---

## 基于LlamaIndex的代码实现

这里将展示与`Adaptive-RAG`类似的方法实现，使用`MistralAI`的`RouterQueryEngine`和`FunctionCalling`功能，根据查询的复杂性调用不同的工具或索引。

+ 复杂查询：利用多个工具，这些工具需要来自多个文档的上下文。

+ 简单查询：使用需要单文档上下文的单个工具，或者直接用`LLM`来提供答案。

方法按以下步骤实现：

+ 下载数据

+ 加载数据

+ 为3个文档创建索引

+ 用文档和`LLM`创建查询引擎

+ 为复杂查询初始化`FunctionCallingAgentWorker`。

+ 创建工具

+ 创建`RouterQueryEngine`-根据查询复杂性路由

+ 查询

### 准备环境

```
pip install llama-index
pip install llama-index-llms-mistralai
pip install llama-index-embeddings-mistralai
```

#### 设置api key

```
import os
os.environ['MISTRAL_API_KEY'] = '<YOUR MISTRAL API KEY>'
```

### 配置LLM和embedding模型

```
import nest_asyncio
nest_asyncio.apply()


from llama_index.core import SimpleDirectoryReader, VectorStoreIndex
from llama_index.llms.mistralai import MistralAI
from llama_index.embeddings.mistralai import MistralAIEmbedding
from llama_index.core import Settings

from llama_index.core.tools import QueryEngineTool, ToolMetadata
from llama_index.core.query_engine.router_query_engine import RouterQueryEngine
from llama_index.core.selectors.llm_selectors import LLMSingleSelector

# Note: Only `mistral-large-latest` supports function calling
llm = MistralAI(model='mistral-large-latest') 
embed_model = MistralAIEmbedding()

Settings.llm = llm
Settings.embed_model = embed_model
```

### 日志配置

```
# NOTE: This is ONLY necessary in jupyter notebook.
# Details: Jupyter runs an event-loop behind the scenes.
#          This results in nested event-loops when we start an event-loop to make async queries.
#          This is normally not allowed, we use nest_asyncio to allow it for convenience.
import nest_asyncio

nest_asyncio.apply()

import logging
import sys

# Set up the root logger
logger = logging.getLogger()
logger.setLevel(logging.INFO)  # Set logger level to INFO

# Clear out any existing handlers
logger.handlers = []

# Set up the StreamHandler to output to sys.stdout (Colab's output)
handler = logging.StreamHandler(sys.stdout)
handler.setLevel(logging.INFO)  # Set handler level to INFO

# Add the handler to the logger
logger.addHandler(handler)

from IPython.display import display, HTML
```

### 下载数据

下载Lyft在2020年、2021年和2022年向美国证券交易委员会提交的1万份文件。

```
!wget "https://www.dropbox.com/scl/fi/ywc29qvt66s8i97h1taci/lyft-10k-2020.pdf?rlkey=d7bru2jno7398imeirn09fey5&dl=0" -q -O ./lyft_10k_2020.pdf
!wget "https://www.dropbox.com/scl/fi/lpmmki7a9a14s1l5ef7ep/lyft-10k-2021.pdf?rlkey=ud5cwlfotrii6r5jjag1o3hvm&dl=0" -q -O ./lyft_10k_2021.pdf
!wget "https://www.dropbox.com/scl/fi/iffbbnbw9h7shqnnot5es/lyft-10k-2022.pdf?rlkey=grkdgxcrib60oegtp4jn8hpl8&dl=0" -q -O ./lyft_10k_2022.pdf
```

### 加载数据

```
# Lyft 2020 docs
lyft_2020_docs = SimpleDirectoryReader(input_files=["./lyft_10k_2020.pdf"]).load_data()

# Lyft 2021 docs
lyft_2021_docs = SimpleDirectoryReader(input_files=["./lyft_10k_2021.pdf"]).load_data()

# Lyft 2022 docs
lyft_2022_docs = SimpleDirectoryReader(input_files=["./lyft_10k_2022.pdf"]).load_data()
```

### 创建索引

```
# Index on Lyft 2020 Document
lyft_2020_index = VectorStoreIndex.from_documents(lyft_2020_docs)

# Index on Lyft 2021 Document
lyft_2021_index = VectorStoreIndex.from_documents(lyft_2021_docs)

# Index on Lyft 2022 Document
lyft_2022_index = VectorStoreIndex.from_documents(lyft_2022_docs)
```

### 创建query引擎
