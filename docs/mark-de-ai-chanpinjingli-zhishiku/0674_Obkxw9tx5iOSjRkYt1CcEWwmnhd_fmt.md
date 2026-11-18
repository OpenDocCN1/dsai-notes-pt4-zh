
# S04：子智能体 — 上下文隔离与分工

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/O3dqdZNbiomqnQxMknacKZjvnye](https://qqs7y1hozd1.feishu.cn/docx/O3dqdZNbiomqnQxMknacKZjvnye)

## Subagents (Subagent)

有了 `Todo` 不跑偏了，但新问题来了：所有工作产生的中间内容（读了多少文件、跑了多少命令）都堆在同一个上下文里，越来越臃肿

解决方案是在工具箱里增加一个 `task` 工具。父 Agent 可以通过它创建子 Agent，把子任务交出去

> 子 Agent 拥有全新的、干净的上下文，在自己的循环里独立工作，执行过程中可能经历了十几轮甚至三十几轮的工具调用，产生了很长的上下文，但最终传回给父 Agent 的只是一段简短的摘要

父 Agent 只需要接收结果，就可以保持自己的上下文非常干净，子 Agent 的所有中间过程直接丢弃

为了避免无限套娃，子 Agent 的工具箱里没有 `task` 工具：只有父 Agent 才能派生子 Agent

![](img/b76ec7d294cd1753246969eab3d3df53.png)

`s01` > `s02` > `s03` > [`s04`] `s05` > `s06` | `s07` > `s08` > `s09` > `s10` > `s11` > `s12`

> "大任务拆小, 每个小任务干净的上下文" -- Subagent 用独立 `messages[]`，不污染主对话。

> Harness 层: 上下文隔离 -- 守护模型的思维清晰度。

### 问题

Agent 工作越久, `messages` 数组越臃肿。每次读文件、跑命令的输出都永久留在上下文里。"这个项目用什么测试框架?" 可能要读 5 个文件, 但父 Agent 只需要一个词: `pytest`。

### 解决方案

```
Parent agent                     Subagent
+------------------+             +------------------+
| messages=[...]   |             | messages=[]      | &lt;-- fresh
|                  |  dispatch   |                  |
| tool: task       | ----------&gt; | while tool_use:  |
|   prompt="..."   |             |   call tools     |
|                  |  summary    |   append results |
|   result = "..." | &lt;---------- | return last text |
+------------------+             +------------------+

Parent context stays clean. Subagent context is discarded.
```

### 工作原理

1. 父 Agent 有一个 `task` 工具。Subagent 拥有除 `task` 外的所有基础工具 (禁止递归生成)。

```
PARENT_TOOLS = CHILD_TOOLS + [
    {"name": "task",
     "description": "Spawn a subagent with fresh context.",
     "input_schema": {
         "type": "object",
         "properties": {"prompt": {"type": "string"}},
         "required": ["prompt"],
     }},
]
```

1. Subagent 以 `messages=[]` 启动, 运行自己的循环。只有最终文本返回给父 Agent。

```
def run_subagent(prompt: str) -> str:
    sub_messages = [{"role": "user", "content": prompt}]
    for _ in range(30):  # safety limit
        response = client.messages.create(
            model=MODEL, system=SUBAGENT_SYSTEM,
            messages=sub_messages,
            tools=CHILD_TOOLS, max_tokens=8000,
        )
        sub_messages.append({"role": "assistant",
                             "content": response.content})
        if response.stop_reason != "tool_use":
            break
        results = []
        for block in response.content:
            if block.type == "tool_use":
                handler = TOOL_HANDLERS.get(block.name)
                output = handler(**block.input)
                results.append({"type": "tool_result",
                    "tool_use_id": block.id,
                    "content": str(output)[:50000]})
        sub_messages.append({"role": "user", "content": results})
    return "".join(
        b.text for b in response.content if hasattr(b, "text")
    ) or "(no summary)"
```

Subagent 可能跑了 30+ 次工具调用, 但整个消息历史直接丢弃。父 Agent 收到的只是一段摘要文本, 作为普通 `tool_result` 返回。

### 相对 s03 的变更

### 试一试

```
cd learn-claude-code
python agents/s04_subagent.py
```

试试这些 `prompt` (英文 `prompt` 对 LLM 效果更好, 也可以用中文):

1. Use a subtask to find what testing framework this project uses

1. Delegate: read all `.py` files and summarize what each one does

1. Use a `task` to create a new module, then verify it from here
