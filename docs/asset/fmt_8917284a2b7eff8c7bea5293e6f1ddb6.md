
# Agentic RAG

> 来源：[https://v11enp9ok1h.feishu.cn/docx/KaPrdeLbFod2zCxnnr3cZ1pznDc](https://v11enp9ok1h.feishu.cn/docx/KaPrdeLbFod2zCxnnr3cZ1pznDc)

构建具有智能体能力的RAG管道，能够从不同来源如向量数据库和互联网动态获取上下文。

![](img/bb77ab6840a758952107d5aa862a9c66.png)

## 设置`LLM`（大语言模型）

`CrewAI`与所有主流`LLM`和提供商无缝集成。以下是通过`Ollama`设置本地`Qwen 3`的方法：

![](img/950a2c7647c1200e7744a8c0d325bcca.png)

```python
from crewai import Crew, Agent, Task, LLM
import litserve as ls
from tools import FirecrawlWebSearchTool, VectorDBSearchTool

class AgenticRAGAPI(ls.LitAPI):
    def setup(self, device):
        llm = LLM(model="ollama/qwen3")...
```

### 定义研究智能体和任务

该智能体接受用户查询，并使用向量数据库工具和由`Firecrawl`提供支持的网络搜索工具检索相关上下文。

再次，将其放入`LitServe`的`setup()`方法中：

```python
from crewai import Crew, Agent, Task, LLM
import litserve as ls
from tools import FirecrawlWebSearchTool, VectorDBSearchTool

class AgenticRAGAPI(ls.LitAPI):
    def setup(self, device):
        llm = LLM(model="ollama/qwen3")
        
        researcher_agent = Agent(role="Senior Researcher",
                                 goal="Research about the user query...",
                                 backstory="You are a skilled researcher...",
                                 tools=[FirecrawlWebSearchTool(), VectorDBSearchTool],
                                 llm=llm)
        
        researcher_task = Task(description="Research about: {query}",
                               agent=researcher_agent)...
```
