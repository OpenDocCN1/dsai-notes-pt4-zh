# 从多智能体协作到 AutoGPT 核心复现

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/InXedT8eIof1FnxLa5DcAz0Mnhh](https://qqs7y1hozd1.feishu.cn/docx/InXedT8eIof1FnxLa5DcAz0Mnhh)

### 前言：为什么要深入 AI Agent 的 “内核”？

当你还在惊叹于 ChatGPT 能流畅对话时，AI 领域已经悄然迈入 “Agent 时代”—— 这些能自主规划、调用工具、甚至反思迭代的智能体，正在重新定义人机协作的边界。

但真正的工业级 Agent 系统绝非简单调用 API 那么简单：如何让多个 Agent 分工协作完成复杂任务？如何让 Agent 拥有 “长期记忆” 而非 “鱼的记忆”？如何避免 Agent 陷入无限循环或产生天价账单？

本文将带你直击这些痛点，从 LangGraph 的状态机架构到 AutoGPT 的核心逻辑，通过两个实战项目（多 Agent 研究助手 + 简化版 AutoGPT），掌握构建生产级 AI Agent 系统的关键技术。

### 一、LangGraph 进阶：构建多 Agent 协作的复杂工作流

LangChain 解决了 Agent 的 “从** 0 **到 1”，而 LangGraph 则实现了 “从 1 到 N”—— 它基于状态机设计，让开发者能精确控制 Agent 的每一步决策，尤其适合构建多 Agent 协作系统。

#### 1.1 核心概念：状态（State）与节点（Node）的舞蹈

LangGraph 的本质是 “可控的状态流转”，核心组件包括：

+   **State**：全局状态容器，存储所有 Agent 共享的数据（如任务进度、历史记录、工具结果）

+   **Node**：执行具体逻辑的单元（如 “研究员 Agent 分析问题”“审查员 Agent 检查结果”）

+   **Edge**：节点间的连接规则（如 “若结果通过审查则输出，否则返回修改”）

+   Conditional Edge：条件分支，根据状态决定下一个节点（类似 “if-else”）

**多 Agent 系统状态流转图**

![](img/4339574a02b36d3bb6c5778a12dfd671.png)

#### 1.2 实战：多 Agent 论文分析系统

**场景**：让 “研究员” Agent 分析论文摘要并生成解读，“审查员” Agent 检查解读的准确性，若不通过则返回修改。

##### 步骤 1：定义全局状态

```
from langgraph.graph import StateGraph, START, END
from pydantic import BaseModel, Field
from typing import List, Optional

# 全局状态：存储任务数据和流程状态
class PaperAnalysisState(BaseModel):
    paper_title: str = Field(description=&quot;论文标题&quot;)
    abstract: str = Field(description=&quot;论文摘要&quot;)
    analysis: Optional[str] = Field(None, description=&quot;研究员生成的分析&quot;)
    review: Optional[str] = Field(None, description=&quot;审查员的反馈&quot;)
    is_approved: bool = Field(False, description=&quot;分析是否通过审查&quot;)

```

##### 步骤 2：实现节点逻辑（多 Agent 角色）

```
**from l**angchain**_openai** import ChatOpenAI

# 初始化大模型
l**lm = Ch**atOpenAI(model_name=**&quot;gpt-4**&quot;, temperatur**e=0.3)
****
# **研**究员****Agen**t：分析**论文摘要
d**ef researcher_node(state: PaperAnalysisState) -&gt; PaperAnalysisState:
    prompt = f&quot;&quot;&quot;
    请分析以下论文摘要，总结核心贡献和创新点：
    标题：{state.paper_title}
    摘要：{state.abstract}
    &quot;&quot;&quot;
    state.analysis = llm.predict(prompt)return state

# 审查员Agent：检查分析准确性
def reviewer_node(state: PaperAnalysisState) -&gt; PaperAnalysisState:
    prompt = f&quot;&quot;&quot;
    请检查以下论文分析是否准确反映了摘要内容，若有遗漏或错误请指出：
    摘要：{state.abstract}
    分析：{state.analysis}
    反馈格式：先回答是否通过（是/否），再说明理由。
    &quot;&quot;&quot;
    review_result = llm.predict(prompt)
    state.review = review_result# 解析是否通过（实际项目需更严谨的判断）
    state.is_approved = &quot;是&quot; in review_result.split(&quot;\n&quot;)[0]return state
```

##### 步骤 3：构建状态机并运行

```
1. 初始化工作流
workflow** = StateGrap**h(Paper**AnalysisSt****ate)

# 2. 添**加节点
workflo**w.add_node(&quot;**re**searcher&quot;,** researcher_node)  **# 研究员**节点
work**flow.**add_node(&quot;reviewer&quot;, revie**wer_node)   **  ** # 审查员节点# **3. 定义流转规则
workflow.add_edge(STA**RT, &quot;researcher&quot;)**  # 开始→研究员
wor**kflow.a**dd_edg**e(&quot;rese**archer&quot;, &quot;reviewer&quot;)  # 研究员→审查员# 4. 条件分支：审查通过则结束，否则返回研究员修改
def should_approve(state: PaperAnalysisState) -&gt; str:
    if state.is_approved:
        return END  # 审查通过，流程结束
    else:
        return &quot;researcher&quot;  # 审查不通过，返回研究员重新分析
workflow.add_conditional_edges(&quot;reviewer&quot;,
    should_approve  # 条件判断函数
)

# 5. 编译工作流
app = workflow.compile()

# 6. 运行：分析一篇AI论文
initial_state = PaperAnalysisState(
    paper_title=&quot;Attention Is All You Need&quot;,
    abstract=&quot;本文提出了Transformer架构，完全基于注意力机制...（省略）&quot;
)
result = app.invoke(initial_state)

# 输出最终结果
print(&quot;分析结果：&quot;, result.analysis)
print(&quot;审查反馈：&quot;, result.review)
```

**关键优势**：相比单 Agent，多 Agent 系统通过分工协作提升结果质量，且状态全程可追溯，便于调试和优化。

### 二、实战项目：拥有长期记忆的 AI 研究助手

#### 2.1 项目架构：让 Agent 既有 “脑子” 又有 “笔记”

这个研究助手需要具备：

+   **短期记忆**：记住当前对话上下文

+   **长期记忆**：通过向量数据库存储知识库（如论文、书籍）

+   **联网能力**：查询最新研究进展（如 arXiv 新论文）

+   **工具调用**：自主决定何时检索记忆、何时联网搜索

**系统架构图**

![](img/1264dea5c57ebd45ed4554ced17272c8.png)

#### 2.2 分步实现

##### 步骤 1：环境准备

pip **install** langchain langchain-openai chromadb langchain-community python-dotenv

##### 步骤 2：初始化核心组件

```
**impo****rt o****s
fr****om d****oten**v import** load_dote**n**v
from langchai**n_**openai impo****rt** **Cha****tOpenAI, OpenAI**Embeddings
from langchain.vecto**rstores im**port C**hroma
from la**ngchain.text_spli**tter impo**rt Recur**siveChara**cterTextSpli**tter**
from langchain.document_loaders import TextLoader

# 加载环境变量
load_dotenv()  # 从.env文件读取OPENAI_API_KEY

# 1. 初始化LLM（核心大脑）
llm = ChatOpenAI(model_name=&quot;gpt-3.5-turbo&quot;, temperature=0)

# 2. 初始化长期记忆（向量数据库）
embeddings = OpenAIEmbeddings()
persist_directory = &quot;./research_assistant_memory&quot;# 若记忆库不存在则初始化，存在则直接加载if not os.path.exists(persist_directory):
    # 加载初始知识库（示例：AI论文摘要集合）
    loader = TextLoader(&quot;ai_papers.txt&quot;)  # 每行一条论文摘要
    documents = loader.load()
    # 分割文本（避免过长）
    text_splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
    splits = text_splitter.split_documents(documents)
    # 创建向量库
    vectordb = Chroma.from_documents(documents=splits,embedding=embeddings,persist_directory=persist_directory
    )
    vectordb.persist()
else:
    # 加载已有记忆库
    vectordb = Chroma(persist_directory=persist_directory,embedding_function=embeddings
    )
```

##### 步骤 3：定义工具（记忆检索 + 联网搜索）

```
**from**** langc****hain****.tools** import Tool
**fro**m langchain.utilities import SerpAPIWrapper

# 1. 长期**记忆检索工具**d**ef r**etrieve**_memo****r**y**(qu**ery: **st**r) -&gt; str:&quot;&quot;&quot;从知识库中检索相关论文或资料，用于回答历史问题&quot;&quot;&quot;
    docs = vecto**rdb.similar**ity_search(query, k=3)  # 取最相关的3条return &quot;\n&quot;.join([f&quot;资料：{doc.page_content}&quot; for doc in docs])

# 2. 联网搜索工具（获取最新信息）
search = SerpAPIWrapper(serpapi_api_key=os.getenv(&quot;SERPAPI_API_KEY&quot;))

# 3. 工具列表（描述需清晰，帮助Agent判断何时调用）
tools = [
    Tool(
        name=&quot;MemoryRetrieval&quot;,
        func=retrieve_memory,
        description=&quot;当需要回答历史研究、经典理论等已有知识时使用&quot;
    ),
    Tool(
        name=&quot;WebSearch&quot;,
        func=search.run,
        description=&quot;当需要最新研究进展、2023年后的论文或动态信息时使用&quot;
    )
]
```

##### 步骤 4：构建带记忆的 Agent

```
**from**** lan**gchain.agents i**mport init**i**alize_agent, A**g**entType
from la**n**gcha**in.memory import C**onver**s**ationB**ufferMem**ory

# **初**始化短期**记忆**（对话历**史）
memory = ConversationBufferMemory(memory_key=&quot;chat_history&quot;,return_messages=True  # 以消息列表形式返回
)

# 初始化Agent
research_agent = initialize_agent(
    tools,
    llm,agent=AgentType.CHAT_CONVERSATIONAL_REACT_DESCRIPTION,  # 带对话记忆的React模式memory=memory,verbose=True,handle_parsing_errors=True  # 处理解析错误
)
```

##### 步骤 5：测试研究助手

```
测试1：查询历史知识（应调用MemoryRetrieval）print(research_agent.run(&quot;Transformer架构有哪些创新点？&quot;))

# 测试2：查询最新进展（应调用WebSearch）print(research_agent.run(&quot;2024年大语言模型领域有哪些重要突破？&quot;))

# 测试3：结合上下文（短期记忆）print(research_agent.run(&quot;刚才提到的那个2024年的突破，有论文链接吗？&quot;))
```

**关键技术点**：

+   向量数据库通过语义相似度检索实现 “长期记忆”

+   ConversationBufferMemory 保存对话历史实现 “短期记忆”

+   Agent 根据问题类型自动选择工具（历史知识→记忆，最新信息→搜索）

### 三、AutoGPT / AgentGPT 深度解析与复现

AutoGPT 作为 “AI 自主代理” 的代表，其核心魅力在于无需人工干预，能自主分解目标、执行计划、反思优化。但它的实现逻辑并不神秘，我们可以拆解为 “目标循环 + 自我批判” 两大模块。

#### 3.1 核心架构：AutoGPT 的 “思考 - 行动” 循环

AutoGPT 的工作流程可简化为：

1.  **目标输入**：用户设定初始目标（如 “制定 Python 三天学习计划”）

1.  **计划分解**：将大目标拆分为可执行的子任务

1.  **行动执行**：调用工具（搜索、文件操作等）完成子任务

1.  **结果反思**：评估当前结果是否接近目标，生成下一步计划

1.  **循环迭代**：重复 “行动 - 反思”，直到目标完成或达到限制条件

**AutoGPT 核心循环图**

![](img/c36f1131aee10f1f2902e2cb97dea4c6.png)

#### 3.2 关键技能：控制 Agent 循环的 “安全闸”

**    **    AutoGPT **最容易出**现的问题是无限循环（如反复搜索相同内容）和成本失控（API 调用量激增）。解决办法是设置多重限制：

1.  **最大迭代次数**：强制终止循环（如最多执行 10 步）

1.  **成本上限**：监控 token 消耗，超过阈值则停止

1.  **超时机制**：单个任务执行超时后终止

1.  **自我检测**：让 Agent 定期判断 “是否在做无用功”

```
循环控制示例代码**class** A**gen**tLoopController:def init(self, max_iterations=10, max_cost=0.1):
       ** se**lf.max_iterations = max_iterations  # 最大迭代次数
       ** s**elf.max_cost = max_cost  # 最大成本**（美元）
       **** self.i**t**eratio**n**_coun**t = 0
      **  **self.t**otal_cost =** 0.0  # 累计成本def** can_con**tinu**e**(**self, **c**urrent_step**_**cost**=0.0) -&gt; bool:&quot;&quot;&quot;判断是否可以继续循环&quot;&quot;&quot;
        self.iteration_count += 1
        self.total_cost += current_step_cost
        # 检查是否超过最大次数if self.iteration_count &gt;= self.max_iterations:print(f&quot;已达到最大迭代次数（{self.max_iterations}），终止循环&quot;)return False# 检查是否超过成本上限if self.total_cost &gt;= self.max_cost:print(f&quot;已超过成本上限（${self.max_cost}），当前花费：${self.total_cost:.4f}&quot;)return Falsereturn True
```

#### 3.3 实战：简化版 AutoGPT 复现（制定 Python 三天学习计划）

我们来复现 AutoGPT 的核心逻辑，实现一个能自主制定学习计划的 Agent。

##### 步骤 1：定义循环核心组件

```
**from**** langc****hain****_opena**i import** ChatOpenAI
fro**m langchain.prompts import ChatPromptTemplate

# 初始化LLM
**llm**** = **ChatOpenAI(model_name=&quot;**gp**t-3.5-tur**bo&quot;,** temper**ature=0.7)

**# 1. 计**划**分**解提示词
p**l**an_prompt **= Chat**Prom**ptTemplate.fr**om_template(**&quot;&quot;&quot;
你需**要**帮**用户制定一个**&quot;{goal}&quot;的计划。
**当前进度：{pr**ogress**}**
已完成步骤：{history}
请分解出下一步具体要做的任务（用一句话描述），并说**明需要**调用什么工具（搜索/思考）。
输出格式：
任务：[具体任务]
工具：[搜索/思考]
&quot;&quot;&quot;)

# 2. 行动执行函数（调用工具）def execute_action(task: str, tool: str) -&gt; str:if tool == &quot;搜索&quot;:# 模拟搜索（实际项目接入SerpAPI）print(f&quot;[搜索] 正在查询：{task}&quot;)return f&quot;搜索结果：关于&#x27;{task}&#x27;的信息已获取，包括基础语法、实战案例等&quot;else:# 直接思考print(f&quot;[思考] 正在处理：{task}&quot;)return llm.predict(f&quot;请完成任务：{task}&quot;)

# 3. 反思评估提示词
reflect_prompt = ChatPromptTemplate.from_template(&quot;&quot;&quot;
目标：{goal}
当前结果：{current_result}
请评估是否已接近目标。若未完成，说明下一步改进方向；若已完成，整理最终结果。
输出格式：
状态：[未完成/已完成]
反馈：[具体反馈或最终结果]
&quot;&quot;&quot;)
```

##### 步骤 2：实现 AutoGPT 循环逻辑

```
**def** simplified_autogpt(goal: str):# 初始化循**环控制器（**最多8步，成本上限**$0****.05）
** **   c**ontroller =** A**g**ent**LoopController(max_iterations=8, max_cost=0.05**)
    progr****ess = &quot;**未开始&quot;
    h**i****story** = []  # 记录已完成步骤
    **final_**result **= &quot;&quot;while **True:# 检查是否**继续循环if no**t contro**ller.**can_continu**e(cu**rre**nt_st**ep_cost=0.0**05):**  # 假设每步成本$**0.00**5
            fi**n**al_res**ult** = f**&quot;任务未**完成，已执行{controller.iteration_count}步**，结果如下：**{progress}&quot;break# 1. **计划**分**解：确定下**一**步任****务
   **     plan_c**hain** = plan_prompt | llm
        pl**an_re**sult** = pl**an_chain.i**nvoke(**{&quot;goal**&quot;: goal**,&quot;progress&quot;: progress,&quot;history&quot;: history
        }).content
        # 解析任务和工具（实际项目需更严谨的解析）
        task = plan_result.split(&quot;任务：&quot;)[1].split(&quot;\n&quot;)[0]
        tool = plan_result.split(&quot;工具：&quot;)[1].split(&quot;\n&quot;)[0]
        # 2. 行动执行：完成当前任务
        action_result = execute_action(task, tool)
        history.append(f&quot;步骤{len(history)+1}：{task} → {action_result[:30]}...&quot;)
        # 3. 反思评估：判断是否完成
        reflect_chain = reflect_prompt | llm
        reflect_result = reflect_chain.invoke({&quot;goal&quot;: goal,&quot;current_result&quot;: action_result
        }).content
        # 解析状态if &quot;已完成&quot; in reflect_result.split(&quot;状态：&quot;)[1].split(&quot;\n&quot;)[0]:
            final_result = reflect_result.split(&quot;反馈：&quot;)[1]break# 更新进度
        progress = action_result
    return final_result

# 测试：制定Python三天学习计划
result = simplified_autogpt(goal=&quot;为我制定一个学习Python的三天计划，适合零基础入门&quot;)
print(&quot;最终计划：&quot;, result)
```

**输出示例**（简化版）：

**[思考]**** 正在处****理：确定**三天学习的核心模块，包括基础语法、简单项目

[搜索] 正在查询：Python零基础三天学习大纲

[思考] 正在处理：根据搜索结果细化每天的学习内容和时间分配

最终计划：

第一天：Python环境搭建（1小时）、变量与数据类型（2小时）、条件语句（1小时）

第二天：循环结构（2小时）、函数定义（2小时）、列表与字典（1小时）

第三天：文件操作（1小时）、简易爬虫实战（3小时）、总结与答疑（1小时）

**核心逻辑提炼**：通过 “计划 - 行动 - 反思”