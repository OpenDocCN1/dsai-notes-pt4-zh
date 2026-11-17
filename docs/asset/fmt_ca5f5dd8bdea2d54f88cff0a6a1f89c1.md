
```python
class SkillMiddleware(AgentMiddleware):
    """
    Skill 中间件 - 实现动态工具过滤

    这是 Claude Skills 的核心组件！

    工作原理：
    1. 在每次模型调用前拦截请求
    2. 从 request.state 中读取 skills_loaded 列表
    3. 根据 skills_loaded 过滤工具列表
    4. 使用 request.override() 替换工具列表
    5. 传递给下一个 handler

    这样，模型在每次调用时只会看到相关的工具！
    """

    def __init__(self, verbose: bool = True):
        """
        初始化 SkillMiddleware

        Args:
            verbose: 是否打印详细日志（用于调试和演示）
        """
        super().__init__()
        self.verbose = verbose
        self.call_count = 0

    def _get_skills_from_state(self, request: ModelRequest) -> List[str]:
        """
        从请求状态中提取 skills_loaded

        注意：AgentState 是 TypedDict，本质上是 dict
        所以我们使用字典方式访问
        """
        skills_loaded = []

        if hasattr(request, 'state') and request.state is not None:
            # TypedDict 本质是 dict，使用 .get() 方法
            if isinstance(request.state, dict):
                skills_loaded = request.state.get("skills_loaded", [])
            else:
                # 兼容其他类型
                skills_loaded = getattr(request.state, "skills_loaded", [])

        return skills_loaded

    def wrap_model_call(
        self,
        request: ModelRequest,
        handler: Callable[[ModelRequest], ModelResponse]
    ) -> ModelResponse:
        """
        【核心方法】拦截模型调用，动态过滤工具

        这是整个 Claude Skills 系统最关键的方法！
        """
        self.call_count += 1

        # Step 1: 从状态中获取已加载的 Skills
        skills_loaded = self._get_skills_from_state(request)

        # Step 2: 获取过滤后的工具
        filtered_tools = get_tools_for_skills(skills_loaded)

        # Step 3: 打印日志
        if self.verbose:
            print(f"\n{'─'*60}")
            print(f"[SkillMiddleware] 第 {self.call_count} 次模型调用")
            print(f"{'─'*60}")
            print(f"skills_loaded: {skills_loaded}")
            print(f"过滤后工具 ({len(filtered_tools)}个): {[t.name for t in filtered_tools]}")

            # 对比原始工具数量
            if hasattr(request, 'tools') and request.tools:
                original_count = len(request.tools)
                print(f"工具数量变化: {original_count} → {len(filtered_tools)}")

        # Step 4: 【关键】使用 request.override() 替换工具列表
        # 这会创建一个新的 ModelRequest，其中 tools 被替换为过滤后的列表
        filtered_request = request.override(tools=filtered_tools)

        if self.verbose:
            print(f"已将过滤后的工具传递给模型")
            print(f"{'─'*60}\n")

        # Step 5: 调用下一个 handler（实际的模型调用）
        return handler(filtered_request)

    async def awrap_model_call(
        self,
        request: ModelRequest,
        handler: Callable[[ModelRequest], ModelResponse]
    ) -> ModelResponse:
        """
        异步版本 - 与同步版本逻辑相同

        LangChain 可能使用异步调用，所以需要同时实现两个版本
        """
        self.call_count += 1

        skills_loaded = self._get_skills_from_state(request)
        filtered_tools = get_tools_for_skills(skills_loaded)

        if self.verbose:
            print(f"\n{'─'*60}")
            print(f"[SkillMiddleware] (async) 第 {self.call_count} 次模型调用")
            print(f"skills_loaded: {skills_loaded}")
            print(f"过滤后工具: {[t.name for t in filtered_tools]}")
            print(f"{'─'*60}\n")

        filtered_request = request.override(tools=filtered_tools)
        return await handler(filtered_request)


print("SkillMiddleware 类已定义")
print("\n关键方法说明:")
print("  • wrap_model_call(): 同步拦截模型调用")
print("  • awrap_model_call(): 异步拦截模型调用")
print("  • request.override(): 创建修改后的请求对象")
```

```
SkillMiddleware 类已定义

关键方法说明:
  • wrap_model_call(): 同步拦截模型调用
  • awrap_model_call(): 异步拦截模型调用
  • request.override(): 创建修改后的请求对象
```

上面的 `SkillMiddleware` 实现了动态工具过滤的核心逻辑。需要重点理解几个关键点：

```
    # 1. 状态读取
    skills_loaded = request.state.get("skills_loaded", [])

    # 2. 工具过滤
    filtered_tools = get_tools_for_skills(skills_loaded)

    # 3. 请求修改，这是最关键的
    filtered_request = request.override(tools=filtered_tools)
```

这是整个系统最关键的一行代码！`request.override()` 方法会创建一个新的 `ModelRequest` 对象，其中 `tools` 参数被替换为我们过滤后的工具列表，而其他参数保持不变。

> **设计模式**：`override()` 方法遵循「不可变性」原则，它不会修改原始请求，而是返回一个新的请求对象。这是函数式编程的最佳实践。

## 3.9 创建 Agent 并测试

现在我们已经准备好了所有组件，接下来使用 LangChain 1.1 的 `create_agent` 函数来创建 `Agent`。这个函数接受以下关键参数：

### create_agent 参数说明

| 参数名 | 类型 | 描述 | 必填 |
| --- | --- | --- | --- |
| `model` | `BaseChatModel` | 语言模型实例 | 是 |
| `tools` | `List[BaseTool]` | 工具列表 | 是 |
| `middleware` | `Tuple[AgentMiddleware]` | 中间件列表 | 否 |
| `state_schema` | `TypedDict` | 状态 Schema | 否 |
| `system_prompt` | `str` | 系统提示 | 否 |

```
# 创建 SkillMiddleware 实例
skill_middleware = SkillMiddleware(verbose=True)

# 定义系统提示
SYSTEM_PROMPT = """
你是一个智能助手，可以使用各种技能来帮助用户完成任务。

## 工作方式

1. 你有两类工具：
   - **Skill Loader**（技能加载器）：用于加载特定技能，名称以 skill_ 开头
   - **功能工具**：执行具体任务的工具

2. 当用户请求某个功能时：
   - 首先检查是否有对应的功能工具
   - 如果没有，调用相应的 Skill Loader 加载技能
   - 加载后，使用新获得的工具完成任务

3. 可用的 Skill Loaders：
   - skill_data_analysis：加载数据分析相关工具
   - skill_text_processing：加载文本处理相关工具

请根据用户的需求，灵活使用工具完成任务。
"""

print("准备创建 Agent...")
print(f"  模型: DeepSeek Reasoner")
print(f"  工具数量: {len(ALL_TOOLS)}")
print(f"  中间件: SkillMiddleware")
print(f"  状态 Schema: SkillState")
```

```
准备创建 Agent...
  模型: DeepSeek Reasoner
  工具数量: 6
  中间件: SkillMiddleware
  状态 Schema: SkillState
```

```
# 创建 Agent
try:
    agent = create_agent(
        model=model,
        tools=ALL_TOOLS,  # 注册所有工具（但 Middleware 会动态过滤）
        middleware=(skill_middleware,),  # 关键：添加 SkillMiddleware
        state_schema=SkillState,  # 使用我们定义的状态 Schema
        system_prompt=SYSTEM_PROMPT,
    )
    print("\nAgent 创建成功！")
    print("\n关键配置:")
    print(f"  • 注册工具总数: {len(ALL_TOOLS)}")
    print(f"  • 初始可见工具: {len(LOADER_TOOLS)} (仅 Loaders)")
    print(f"  • Middleware: SkillMiddleware (动态过滤)")
    
except TypeError as e:
    print(f"创建时遇到参数问题: {e}")
    print("尝试简化版本...")
    agent = create_agent(
        model=model,
        tools=ALL_TOOLS,
        middleware=(skill_middleware,),
    )
    print("Agent 创建成功（简化版本）")
```

```
Agent 创建成功！

关键配置:
  • 注册工具总数: 6
  • 初始可见工具: 2 (仅 Loaders)
  • Middleware: SkillMiddleware (动态过滤)
```

Agent 创建完成！注意几个关键点：

1. `tools` 参数：我们传入了所有 6 个工具，但这不代表模型会看到全部。`Middleware` 会在每次调用前过滤。

2. `middleware` 参数：必须是一个元组（`tuple`），即使只有一个中间件也要写成 `(middleware,)`。

3. `state_schema` 参数：告诉 `Agent` 我们的状态结构，这样它就知道如何管理 `skills_loaded` 字段。

# 4. LangChain Skills Agent运行测试

让我们先测试一个简单的问候，看看初始状态下模型能看到多少工具：

```
print("="*60)
print("测试场景 1：初始状态 - 简单问候")
print("="*60)
print("\n预期行为:")
print("   • skills_loaded: [] (空)")
print("   • 可见工具: 2 个 (仅 Loaders)")
print("\n" + "-"*60)

# 构造输入 - 使用 HumanMessage 对象
from langchain_core.messages import HumanMessage

test_input = {
    "messages": [HumanMessage(content="你好，请告诉我你现在有哪些工具可用？")],
    "skills_loaded": []  # 初始状态：没有加载任何技能
}

# 调用 Agent
result = agent.invoke(test_input)

print("-"*60)
print("\nAI 响应:")
for msg in result.get("messages", []):
    if msg.__class__.__name__ == "AIMessage" and msg.content:
        print(msg.content)
```
