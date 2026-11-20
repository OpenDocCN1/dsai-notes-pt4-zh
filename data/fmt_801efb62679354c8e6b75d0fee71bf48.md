
# 基于Agent搭建ai论文写作程序

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Lgw6dLBn1oJTpdxKYxBclbZ6nsc](https://qqs7y1hozd1.feishu.cn/docx/Lgw6dLBn1oJTpdxKYxBclbZ6nsc)

本程序能够自动化完成从规划、研究、撰写到修改的整个论文创作流程。基于`LangGraph`和`LangChain`框架构建，利用大型语言模型和搜索工具协同工作，形成了一个完整的论文写作流水线。

## 核心功能分析

### 计划生成功能 (`plan_node`)

这个 `node` 将接收状态，然后创建一个消息列表。其中一个将是 `PLAN_PROMPT`，那将是 `SystemMessage`。然后创建 `HumanMessage`，传入我们要做的 `task`。然后我们将得到一个 `response`，我们将取得这个消息的信息将其设置为 `plan`。该函数负责基于用户输入的任务创建论文大纲。它通过向语言模型发送系统提示和用户任务，获取一个结构化的论文大纲，这作为后续写作的指导框架。

```
def plan_node(state: AgentState):
    """创建论文大纲"""
    messages = [
        SystemMessage(content=PLAN_PROMPT),
        HumanMessage(content=state["task"])
    ]
    response = model.invoke(messages)
    return {"plan": response.content}
```

### 研究规划功能 (`research_plan_node`)

这个 `node` 是接收 `plan` 并进行一些 `research`。首先会生成一些 `query`，这基本上是在说，我们将要调用它的响应将是我们之前调用的 `pydantic` object，其中包含查询列表。所以我们将在消息列表上调用 `invoke`。我们拿到了 `RESEARCH_PLAN_PROMPT`，然后创建 `HumanMessage`，传入我们要做的 `task`。我们将会获取我们当前文档的列表，我们将用他们来撰写这篇 `essay`。然后我们将循环遍历 `query`，并在 `tavily` 中进行搜索，我们取得 `result` 后，会将其附加到 `content` 中。这确保了AI在写作过程中能够基于最新、相关的外部信息进行创作，而不仅仅依赖于模型的内置知识。

```
def research_plan_node(state: AgentState):
    """基于任务生成研究查询并收集信息"""
    queries = model.with_structured_output(Queries).invoke([
        SystemMessage(content=RESEARCH_PLAN_PROMPT),
        HumanMessage(content=state["task"])
    ])
    content = state.get('content', [])
    for q in queries.queries:
        response = tavily.search(query=q, max_results=2)
        for r in response['results']:
            content.append(r['content'])
    return {"content": content}
```

### 内容生成功能 (`generation_node`)

对于生成节点，我们要做的第一件事是准备内容。然后我们将创建 `user_message`，我们将 `plan` 和 `task` 结合起来。然后创建一个消息列表，首先是一个带有写作提示的 `SystemMessage`，我们在其中格式化 `content`。我们把信息传递给消息队列，得到一个响应。然后我们会更新修订号，这可以跟踪我们做了多少次修订，以便系统可以跟踪当前处于写作流程的哪个阶段。

```
def generation_node(state: AgentState):
    """基于计划和研究生成或修改论文草稿"""
    content = "\n\n".join(state['content'] or [])
    user_message = HumanMessage(
        content=f"{state['task']}\n\n Here is my plan:\n\n {state['plan']}"
    )
    messages = [
        SystemMessage(
            content=WRITER_PROMPT.format(content=content)
        ),
        user_message
    ]
    response = model.invoke(messages)
    return {
        "draft": response.content, 
        "revision_number": state.get("revision_number", 1) + 1
    }
```

### 反思评价功能 (`reflection_node`)

生成之后，我们现在需要一个反思节点。反思节点会将反思提示作为系统提示，然后会提取 `draft`。该函数模拟了一个教师角色，对生成的论文草稿进行评价和批评。这种自我反思机制使AI能够识别当前论文的不足之处，并为下一轮修改提供指导。

```
def reflection_node(state: AgentState):
    """为草稿生成批评和建议"""
    messages = [
        SystemMessage(content=REFLECTION_PROMPT),
        HumanMessage(content=state["draft"])
    ]
    response = model.invoke(messages)
    return {"critique": response.content}
```

### 批评研究功能 (`research_critique_node`)

通过 `with_structured_output(Queries)` 方法将模型输出强制转换为预定义的 `Queries` 结构体，确保输出格式规范。然后，系统将批评内容作为输入，让AI分析论文的弱点并生成针对性查询。它包含完善的错误处理和回退机制，确保即使查询生成失败，系统也能继续运行。

```
def research_critique_node(state: AgentState):
    """基于批评进行研究以解决弱点"""
    try:
        # 尝试获取结构化输出
        queries = model.with_structured_output(Queries).invoke([
            SystemMessage(content=RESEARCH_CRITIQUE_PROMPT),
            HumanMessage(content=state["critique"])
        ])
        
        # 检查queries是否有效
        if queries is None or not hasattr(queries, 'queries') or not queries.queries:
            # 创建基于文章主题的默认查询
            print("警告: 结构化输出生成失败，使用默认查询")
            default_queries = [
                f"{state['task']} statistics", 
                f"{state['task']} comparison", 
                f"{state['task']} examples"
            ]
            
            content = state.get('content', [])
            # 使用默认查询
            for q in default_queries:
                response = tavily.search(query=q, max_results=2)
                for r in response['results']:
                    content.append(r['content'])
                    
            return {"content": content}
        
        # 如果queries结构正确，按原计划继续
        content = state.get('content', [])
        for q in queries.queries:
            response = tavily.search(query=q, max_results=2)
            for r in response['results']:
                content.append(r['content'])
                
        return {"content": content}
        
    except Exception as e:
        # 记录错误
        print(f"research_critique_node中出现错误: {e}")
        return {"content": state.get('content', [])}
```

### 决策控制功能 (`should_continue`)

这将查看修订号，如果大于最大修订次数，我们将结束，否则返回 `reflect` 继续反思。注意这是在生成步骤后的操作，我们要么完成，要么进入评论循环。

```
def should_continue(state):
    """决定是否继续修改或结束"""
    if state["revision_number"] > state["max_revisions"]:
        return END
    return "reflect"
```

## LangGraph 的建立

这在之前的文章中有提及，感兴趣的小伙伴可以去拿章看看什么是 `langgraph`

```
builder = StateGraph(AgentState)

builder.add_node("planner", plan_node)
builder.add_node("generate", generation_node)
builder.add_node("reflect", reflection_node)
builder.add_node("research_plan", research_plan_node)
builder.add_node("research_critique", research_critique_node)

builder.set_entry_point("planner")  # 设置一个入口

# 条件边
builder.add_conditional_edges(
    "generate",
    should_continue,
    {END: END, "reflect": "reflect"}
)

builder.add_edge("planner", "research_plan")
builder.add_edge("research_plan", "generate")
builder.add_edge("reflect", "research_critique")
builder.add_edge("research_critique", "generate")
```

工作流构建函数定义了整个论文写作的流程图，通过设置各个功能节点之间的连接关系，形成了一个完整的闭环系统：

1. 从计划节点开始
2. 进行初始研究
3. 生成初稿
4. 对初稿进行反思评价
5. 基于评价进行针对性研究
6. 修改论文
7. 根据修订次数决定是否继续循环或结束

## 完整执行功能

```
thread = {"configurable": {"thread_id": "1"}}

with SqliteSaver.from_conn_string(":memory:") as checkpointer:
    graph = builder.compile(
        checkpointer=checkpointer,
        interrupt_after=['planner', 'generate', 'reflect', 'research_plan', 'research_critique']
    )
    for s in graph.stream({
        'task': "What different between Langchain and langsmith?",
        "max_revisions": 2,
        "revision_number": 1
    }, thread):
        print(s)
```

## 部分测试结果

![](img/bbf5a77233c7df3d2f4c04b70a180ce4.png)
