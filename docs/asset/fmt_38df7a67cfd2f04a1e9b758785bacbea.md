
```python
# 这是一个简单的内存数据库，用于存储和更新问题

class IssuesDatabase:
    """
    模拟的问题数据库
    在实际项目中，这会是 SQLite/PostgreSQL 等真实数据库
    """
    
    def __init__(self):
        self._issues: Dict[str, Issue] = {}
    
    def add_issue(self, issue: Issue) -> None:
        """添加问题"""
        self._issues[issue.id] = issue
        print(f"问题已添加到数据库: {issue.id}")
    
    def get_issue(self, issue_id: str) -> Optional[Issue]:
        """获取问题"""
        return self._issues.get(issue_id)
    
    def update_issue(self, issue_id: str, update_fields: Dict[str, Any]) -> Issue:
        """
        更新问题
        这是 HITL 要保护的关键操作
        """
        issue = self._issues.get(issue_id)
        if not issue:
            raise ValueError(f"问题不存在: {issue_id}")
        
        # 更新字段
        issue_dict = issue.model_dump()
        issue_dict.update(update_fields)
        
        # 创建更新后的 Issue
        updated_issue = Issue(**issue_dict)
        self._issues[issue_id] = updated_issue
        
        print(f"问题已更新: {issue_id}")
        print(f"   更新的字段: {list(update_fields.keys())}")
        
        return updated_issue
    
    def list_issues(self) -> List[Issue]:
        """列出所有问题"""
        return list(self._issues.values())

# 创建数据库实例并添加示例问题
db = IssuesDatabase()
db.add_issue(sample_issue)

# 保存问题 ID 供后续使用
ISSUE_ID = sample_issue.id

print(f"\n数据库中共有 {len(db.list_issues())} 个问题")
```

```
问题已添加到数据库: 205508d5-346d-459b-824c-52a93d9b47dd

数据库中共有 1 个问题
```

## 3.3 初始化 LLM

接下来，我们初始化用于驱动 Agent 的 LLM。这里使用 DeepSeek-v3.2：

```python
from langchain_openai import ChatOpenAI

# 检查 API Key
if not DEEPSEEK_API_KEY:
    raise ValueError("DEEPSEEK_API_KEY 未配置，请先设置环境变量")

# 初始化 DeepSeek LLM
llm = ChatOpenAI(
    api_key=DEEPSEEK_API_KEY,
    base_url="https://api.deepseek.com",
    model="deepseek-chat",
    temperature=0.2,
)

print("LLM 初始化完成")
```

```
LLM 初始化完成
```

## 2.5 创建 HITL Agent（核心代码）

现在是本课最核心的部分——创建支持 HITL 的 Agent。

我们需要：

1. 定义工具函数：`update_issue` - 更新问题的工具；
2. 配置中间件：`HumanInTheLoopMiddleware` - 在工具执行前触发中断；
3. 配置检查点：`InMemorySaver` - 保存执行状态；
4. 创建 Agent：使用 `create_agent()` 组装所有组件；

```python
import json
from langchain.agents import create_agent
from langchain.agents.middleware import HumanInTheLoopMiddleware
from langchain_core.messages import HumanMessage
from langgraph.checkpoint.memory import InMemorySaver
from langgraph.types import Command

# -----------------------------
# Step 1: 定义工具函数
# -----------------------------
# 这个函数会被 HITL 中间件拦截
# 在执行前会触发中断，等待人工确认

def update_issue(issue_id: str, update_fields: Dict[str, Any]) -> str:
    """
    更新问题（需要人工确认）
    
    Args:
        issue_id: 要更新的问题 ID
        update_fields: 要更新的字段字典
    
    Returns:
        "ok" 表示更新成功
    """
    # 实际执行数据库更新
    db.update_issue(issue_id, update_fields)
    return "ok"

# -----------------------------
# Step 2: 创建 HITL Agent
# -----------------------------

# System Prompt：告诉 AI 它的角色和任务
SYSTEM_PROMPT = """你是一个审阅工作流执行器。
你会收到 issue_id 和 update_fields。
你必须且只能调用一次 `update_issue` 工具，并严格使用提供的参数。
不要自行猜测、不要新增字段、不要修改字段含义。
"""

# 创建支持 HITL 的 Agent
hitl_agent = create_agent(
    model=llm,
    tools=[update_issue],  # 注册工具
    system_prompt=SYSTEM_PROMPT,
    middleware=[
        # 配置 HITL 中间件
        HumanInTheLoopMiddleware(
            interrupt_on={
                "update_issue": True,  # 在调用 update_issue 前触发中断
            },
            description_prefix="需要人工确认的操作",
        ),
    ],
    checkpointer=InMemorySaver(),  # 使用内存检查点保存状态
)

print("="*60)
print("HITL Agent 创建完成")
print("="*60)
print("配置说明:")
print("   - 工具: update_issue (更新问题)")
print("   - 中间件: HumanInTheLoopMiddleware")
print("   - 触发中断: 调用 update_issue 前")
print("   - 状态保存: InMemorySaver (内存检查点)")
print("="*60)
```

```
============================================================
HITL Agent 创建完成
============================================================
配置说明:
   - 工具: update_issue (更新问题)
   - 中间件: HumanInTheLoopMiddleware
   - 触发中断: 调用 update_issue 前
   - 状态保存: InMemorySaver (内存检查点)
============================================================
```

## 4. HITL 运行测试

现在我们来演示完整的 HITL 流程。我们模拟用户想要「采纳」一个问题的场景：首先，我们封装两个核心函数：

+ `start_hitl()`：启动 HITL 流程，触发中断；
+ `resume_hitl()`：根据人工决策恢复执行；

```python
async def start_hitl(
    thread_id: str,
    issue_id: str,
    update_fields: Dict[str, Any]
) -> Dict[str, Any]:
    """
    启动 HITL 流程
    
    Args:
        thread_id: 唯一标识本次 HITL 流程
        issue_id: 要更新的问题 ID
        update_fields: 要更新的字段
    
    Returns:
        中断信息（包含 interrupt_id 和提议的操作）
    """
    # 配置（thread_id 用于检索检查点）
    config = {"configurable": {"thread_id": thread_id}}
    
    # 构建提示词，告诉 Agent 要执行什么操作
    prompt = (
        "请按照提供的参数更新 issue。\n"
        f"issue_id: {issue_id}\n"
        f"update_fields(JSON): {json.dumps(update_fields, ensure_ascii=False)}\n"
        "你必须调用 update_issue。\n"
    )
    
    # 运行 Agent，直到遇到中断
    async for step in hitl_agent.astream(
        {"messages": [HumanMessage(content=prompt)]},
        config,
        stream_mode="values"
    ):
        # 检查是否有中断
        if "__interrupt__" in step:
            interrupt = step["__interrupt__"][0]
            try:
                return {
                    "id": interrupt.id,
                    "value": interrupt.value,
                    "thread_id": thread_id,
                }
            except AttributeError:
                return {
                    "value": interrupt,
                    "thread_id": thread_id,
                }
    
    # 如果没有中断（不应该发生），返回 None
    return None


async def resume_hitl(
    thread_id: str,
    decision: Dict[str, Any],
    interrupt_id: str = None
) -> None:
    """
    根据人工决策恢复 HITL 流程
    
    Args:
        thread_id: HITL 流程的 thread_id
        decision: 人工决策，格式：
            - 批准: {"type": "approve"}
            - 修改: {"type": "edit", "edited_action": {...}}
            - 拒绝: {"type": "reject", "message": "..."}
        interrupt_id: 中断 ID（可选）
    """
    config = {"configurable": {"thread_id": thread_id}}
    
    # 使用 Command 恢复执行
    cmd = Command(resume={"decisions": [decision]})
    
    # 继续执行直到完成
    async for step in hitl_agent.astream(cmd, config, stream_mode="values"):
        # 检查是否有新的中断（不应该发生）
        if "__interrupt__" in step:
            raise RuntimeError("HITL 恢复后产生了新的中断（异常情况）")
    
    print("HITL 流程已完成")


print("="*60)
print("HITL 操作函数已定义")
print("="*60)
print("start_hitl(thread_id, issue_id, update_fields)")
print("   - 启动 HITL 流程，触发中断")
print("   - 返回中断信息（包含 thread_id 用于后续恢复）")
print("")
print("resume_hitl(thread_id, decision, interrupt_id)")
print("   - 根据人工决策恢复执行")
print("   - decision: approve/edit/reject")
print("="*60)
```

```
============================================================
HITL 操作函数已定义
============================================================
start_hitl(thread_id, issue_id, update_fields)
   - 启动 HITL 流程，触发中断
   - 返回中断信息（包含 thread_id 用于后续恢复）

resume_hitl(thread_id, decision, interrupt_id)
   - 根据人工决策恢复执行
   - decision: approve/edit/reject
============================================================
```

### 4.1 人机交互场景 1：批准（Approve）
