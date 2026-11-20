
```
DEEPSEEK_API_KEY=sk-xxx
DASHSCOPE_API_KEY=xxx
```

---

### 🔐 2. 加载 API 密钥与设置环境变量

```python
DeepSeek_API_KEY = os.getenv("DEEPSEEK_API_KEY")
dashscope_api_key = os.getenv("dashscope_api_key")
os.environ["KMP_DUPLICATE_LIB_OK"] = "TRUE"
```

- 从环境变量中读取 DashScope 和 DeepSeek API。
- 设置 `KMP_DUPLICATE_LIB_OK` 避免某些 MKL 多线程报错。

### 🧠 3. 初始化向量 Embedding 模型

```python
embeddings = DashScopeEmbeddings(
    model="text-embedding-v1", dashscope_api_key=dashscope_api_key
)
```

- 用阿里云 DashScope 提供的 `text-embedding-v1` 将文本转为向量表示，用于相似度搜索。

### 📄 4. 处理 PDF 文本与向量化逻辑

```python
def pdf_read(pdf_doc):
    ...
def get_chunks(text):
    ...
def vector_store(text_chunks):
    ...
```

- `pdf_read`：逐页读取 PDF 内容并拼接。
- `get_chunks`：将长文本切片为多个段落（`chunk`），每段 1000 字，重叠 200 字。
- `vector_store`：用 `FAISS` 建立向量索引，并保存到本地 `faiss_db/`。

### 🔁 5. Agent对话链 + 工具调用（核心 RAG）

```python
def get_conversational_chain(tools, ques):
    llm = init_chat_model("deepseek-chat", model_provider="deepseek")
    ...
    agent_executor = AgentExecutor(...)
    response = agent_executor.invoke({"input": ques})
    ...
```

- 初始化 DeepSeek 模型为 `Agent`。
- 使用 `LangChain` 的 `create_tool_calling_agent` 构造 `Agent`，输入：

- `prompt`（你设定的系统角色）

- 工具（`retriever` 工具）

- `AgentExecutor.invoke`：`LangChain` 自动判断是否调用工具，完成“读取上下文 → 查询 → 回答”流程。

### 🔍 6. 用户提问逻辑（调用 FAISS）

```python
def user_input(user_question):
    ...
    new_db = FAISS.load_local("faiss_db", embeddings, ...)
    retriever = new_db.as_retriever()
    retrieval_chain = create_retriever_tool(retriever, "pdf_extractor", ...)
    get_conversational_chain(retrieval_chain, user_question)
```

- 加载本地 `FAISS` 向量库；
- 将其转为 `LangChain` 的检索工具；
- 交由 `Agent` 调用完成回答。

### 🧠 7. 检查数据库是否存在

```python
def check_database_exists():
    return os.path.exists("faiss_db") and os.path.exists("faiss_db/index.faiss")
```

简单检查本地是否已有向量化数据。

### 🌐 8. 主界面逻辑（Streamlit）

```python
def main():
    st.set_page_config(...)
    ...
```

- 页面标题与界面配置。
- `st.columns` 分栏：左边显示提示，右边放置“清空数据库”按钮。
- 主输入框：`st.text_input("请输入问题")`
- 只有当数据库存在时才能提问。
- 侧边栏：
- PDF 上传器；
- 提交按钮（处理上传的 PDF → 分片 → 向量化 → 存储）。

### 🎯 9. 提交 PDF 后执行的逻辑

```python
if process_button:
    raw_text = pdf_read(pdf_doc)
    ...
    text_chunks = get_chunks(raw_text)
    vector_store(text_chunks)
```

- 当点击“提交并处理”后：

1. 读取上传的 PDF；
1. 切片文本；
1. 向量化入库；
1. 弹出气球提示，并 `st.rerun()` 刷新页面状态。

### 📎 项目结构总结

| 模块 | 说明 |
| --- | --- |
| 🧾 PDF解析 | 读取用户上传的 PDF |
| ✂️ 文本切片 | 按段落分割内容 |
| 📊 向量化 | `DashScope Embedding` + `FAISS` 建库 |
| 🔁 查询接口 | 用户输入 → 召回相关 `chunk` |
| 🤖 `DeepSeek Agent` | 调用检索工具并给出回答 |
| 💻 UI层 | `Streamlit` 实现全部交互 |

其中 `LangChain` RAG 核心功能相关代码如下：

- Step 1：PDF 文件上传与文本提取

使用 `st.file_uploader()` 组件支持多文件上传，并通过 `PyPDF2.PdfReader` 对每页内容进行提取，组合为整体文本。

```python
def pdf_read(pdf_doc):
    text = ""
    for pdf in pdf_doc:
        pdf_reader = PdfReader(pdf)
        for page in pdf_reader.pages:
            text += page.extract_text()
    return text
```

- Step 2：文本分块与向量数据库构建

使用 `RecursiveCharacterTextSplitter` 将长文档切割为固定长度（1000 字）+ 重叠（200 字）的小块，将文本块通过 `DashScopeEmbeddings` 嵌入为向量，使用 `FAISS` 本地存储向量数据库。

```python
chunks = text_splitter.split_text(text)
vector_store = FAISS.from_texts(chunks, embedding=embeddings)
vector_store.save_local("faiss_db")
```

- Step 3：用户提问与语义检索

通过 `Streamlit` 获取用户输入问题，如果向量数据库存在，则加载 `FAISS` 检索器，使用 `create_retriever_tool()` 构建 `LangChain` 工具，交由 `AgentExecutor` 执行，自动调用检索器并生成答案。

```python
retrieval_chain = create_retriever_tool(retriever, ...)
agent = create_tool_calling_agent(llm, [retrieval_chain], prompt)
response = agent_executor.invoke({"input": ques})
```

完整的代码已经上传至百度网盘中的 `langchain_rag.py` 文件中，大家可以扫描下方二维码免费领取

![](img/955aedfbb0e9c13b66c1c6d268684a2b.png)

![](img/78d719d74f431b00034289b215ee4565.png)

项目运行效果如下所示：

```python
from IPython.display import Video
```

```python
Video("https://ml2022.oss-cn-hangzhou.aliyuncs.com/LangChain%20RAG.mp4", width=800, height=400)
```

Your browser does not support the video element.
