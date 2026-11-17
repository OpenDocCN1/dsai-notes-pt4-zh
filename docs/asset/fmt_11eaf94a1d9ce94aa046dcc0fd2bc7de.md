
欢迎使用 HR 政策智能体！输入 `exit` 或 `quit` 退出。

用户: 你好，请问公司如何请病假

---------------------------------------------------------------------------

```
欢迎使用 HR 政策智能体！输入 exit 或 quit 退出。

用户: 你好，请问公司如何请病假

---------------------------------------------------------------------------

AttributeError                            Traceback (most recent call last)

Cell In[46], line 1
----> 1 run_cli()


Cell In[45], line 13, in run_cli()
     11 history.append({"role": "user", "content": user_input})
     12 # Invoke graph
---> 13 state_out = hr_bot.invoke({"messages": history})
     14 answer = state_out["answer"]
     15 print("助理:", answer)


File ~/anaconda3/lib/python3.12/site-packages/langgraph/pregel/__init__.py:2719, in Pregel.invoke(self, input, config, stream_mode, output_keys, interrupt_before, interrupt_after, checkpoint_during, debug, **kwargs)
   2716 chunks: list[Union[dict[str, Any], Any]] = []
   2717 interrupts: list[Interrupt] = []
-> 2719 for chunk in self.stream(
   2720     input,
   2721     config,
   2722     stream_mode=stream_mode,
   2723     output_keys=output_keys,
   2724     interrupt_before=interrupt_before,
   2725     interrupt_after=interrupt_after,
   2726     checkpoint_during=checkpoint_during,
   2727     debug=debug,
   2728     **kwargs,
   2729 ):
   2730     if stream_mode == "values":
   2731         if (
   2732             isinstance(chunk, dict)
   2733             and (ints := chunk.get(INTERRUPT)) is not None
   2734         ):


File ~/anaconda3/lib/python3.12/site-packages/langgraph/pregel/__init__.py:2436, in Pregel.stream(self, input, config, stream_mode, output_keys, interrupt_before, interrupt_after, checkpoint_during, debug, subgraphs)
   2434         for task in loop.match_cached_writes():
   2435             loop.output_writes(task.id, task.writes, cached=True)
-> 2436         for _ in runner.tick(
   2437             [t for t in loop.tasks.values() if not t.writes],
   2438     timeout=self.step_timeout,
   2439     get_waiter=get_waiter,
   2440     schedule_task=loop.accept_push,
   2441         ):
   2442             # emit output
   2443             yield from output()
   2444 # emit output


Cell In[43], line 152, in node_classify(state)
    147 guardrail_input = guardrail_prompt.format(
    148     chat_history=history_text,
    149     question=state["question"],
    150 )
    151 response = model.invoke(guardrail_input)
--> 152 is_relevant = "是" in response.lower()
    153 state.update({"is_relevant": is_relevant})
    154 return state


File ~/anaconda3/lib/python3.12/site-packages/pydantic/main.py:991, in BaseModel.__getattr__(self, item)
    988     return super().__getattribute__(item)  # Raises AttributeError if appropriate
    989 else:
    990     # this is the current error
--> 991     raise AttributeError(f'{type(self).__name__!r} object has no attribute {item!r}')


AttributeError: 'AIMessage' object has no attribute 'lower'

During task with name 'classify' and id '4892b539-7819-b7e2-7687-790ed0542e4d'
```

```
hr_bot.invoke({"messages": [{"role": "user", "content": "请问今天天气如何？"}]})
```

```
{'messages': [{'role': 'user', 'content': '请问今天天气如何？'}],
 'question': '请问今天天气如何？',
 'is_relevant': False,
 'answer': '我不能回答与 HR 政策无关的问题。'}
```

完整结构如下：

【开始】 START

↓

【节点】Extract（提取问题 + 截取最近50条对话）

↓

【节点】Classify（使用护栏Prompt判断问题是否属于HR范围）

↓

【分支判断】

├─ 如果【是HR相关】：

│

│ ↓

│ 【节点】Retrieve（用FAISS向量检索获取相关文档）

│ ↓

│ 【节点】ComposePrompt（把上下文和对话拼接成完整Prompt）

│ ↓

│ 【节点】AnswerGenerate（调用模型生成回答）

│ ↓

│ 【结束】END

│

└─ 如果【不是HR相关】：

↓

【节点】Deny（返回“我不能回答与HR政策无关的问题。”）

↓

【结束】END

完整代码解释如下：

### 一、文件头部与依赖导入

```
from __future__ import annotations
import os
from typing import List, TypedDict, Optional
```

+   `__future__` import annotations：允许 Python 3.7+ 推迟类型注解的求值，避免循环依赖问题。
+   `os`：用于获取环境变量。
+   `typing`：定义类型提示。

```
from langgraph.graph import StateGraph, START, END
from langchain.chat_models import init_chat_model
from langchain_openai import OpenAIEmbeddings
from langchain.prompts import PromptTemplate
from langchain_community.vectorstores import FAISS
from langchain.docstore.document import Document
```

这些是 `LangChain` 和 `LangGraph` 的核心依赖：

+   `StateGraph`：`LangGraph` 的状态图，用来定义对话流程。
+   `init_chat_model`：初始化聊天模型（这里用 DeepSeek）。
+   `OpenAIEmbeddings`：用于生成文本 Embedding。
+   `FAISS`：用于向量检索。
+   `PromptTemplate`：用于定义 Prompt 模板。

### 二、模型与检索器初始化

#### 1）初始化聊天模型

```
model = init_chat_model(model="deepseek-chat", model_provider="deepseek")
```

这里创建了一个 DeepSeek Chat 模型，用于后续生成回复。

#### 2）Embedding 模型

```
OPENAI_EMBEDDING_API_KEY = os.getenv("OPENAI_API_KEY")
OPENAI_EMBEDDING_BASE_URL = "https://ai.devtool.tech/proxy/v1"
embed = OpenAIEmbeddings(
    api_key=OPENAI_EMBEDDING_API_KEY,
    base_url=OPENAI_EMBEDDING_BASE_URL,
    model="text-embedding-3-small",
)
```

+   从环境变量中获取 API Key。
+   指定了自定义代理（`base_url`）。
+   使用 `text-embedding-3-small` 模型生成 Embedding 向量。

#### 3）加载 FAISS 检索库

```
vector_store = FAISS.load_local(
    folder_path="staff_handbook_db",
    embeddings=embed,
    allow_dangerous_deserialization=True,
)
retriever = vector_store.as_retriever(search_kwargs={"k": 3})
```

+   加载之前已创建的 FAISS 数据库（存有员工手册向量）。
+   `as_retriever` 把它变成检索器，查询返回最相关的 3 条内容。

### 三、Prompt 模板

#### 1）护栏（分类）Prompt

```
GUARDRAIL_TMPL = """
...
""".strip()
```

这是一个 `分类Prompt`，目的是判断问题是否属于 HR 范围，回答“是”或“否”。

对应的封装：

```
guardrail_prompt = PromptTemplate(
    input_variables=["chat_history", "question"],
    template=GUARDRAIL_TMPL,
)
```

#### 2）答案生成 Prompt

```
ANSWER_TMPL = """
...
""".strip()
```

这是回答问题的 Prompt，要求：

+   不重复问题。
+   不说“这是答案”。
+   不提上下文。

封装：

```
answer_prompt = PromptTemplate(
    input_variables=["chat_history", "context", "question"],
    template=ANSWER_TMPL,
)
```

### 四、辅助函数

1.  把消息列表变成字符串

```
def _msg_list_to_text(messages: List[dict]) -> str:
    return "\n".join(f"{m['role']}: {m['content']}" for m in messages)
```

作用：把对话历史格式化成字符串。

1.  把文档列表变成字符串

```
def _docs_to_text(docs: List[Document]) -> str:
    return "\n\n".join(d.page_content for d in docs)
```

作用：拼接检索到的文档文本。

1.  确保返回纯文本

```
def _to_str(llm_output):
    return llm_output.content if hasattr(llm_output, "content") else str(llm_output)
```

作用：兼容 LLM 返回的不同类型（`AIMessage` 或 `str`）。

### 五、状态定义

```
class HRState(TypedDict, total=False):
    messages: List[dict]
    question: str
    is_relevant: bool
    context_docs: Optional[List[Document]]
    answer_prompt: str
    answer: str
```

这是 `LangGraph` 的对话状态，用于在流程里保存：

+   当前对话消息
+   用户问题
+   是否相关
+   检索文档
+   生成用的 Prompt
+   最终答案

### 六、流程节点

每个“节点”是对话中的一个逻辑步骤。

---

#### 1）提取问题

```
def node_extract(state: HRState) -> HRState:
    msgs = state["messages"]
    if len(msgs) > 50:
        msgs = msgs[-50:]  # 只保留最新50条消息
    state["messages"] = msgs
    state["question"] = msgs[-1]["content"] if msgs else ""
    return state
```

作用：提取用户问题，并保留最近对话。

#### 2）分类

```
def node_classify(state: HRState) -> HRState:
    history_text = _msg_list_to_text(state["messages"][:-1])
    prompt = guardrail_prompt.format(chat_history=history_text, question=state["question"])
    resp = model.invoke(prompt)
    is_relevant = "是" in _to_str(resp).lower()
    state["is_relevant"] = is_relevant
    return state
```

作用：

+   用分类 Prompt 判断问题是否属于 HR 范围。
+   把“是/否”结果写入状态。

#### 3）检索
