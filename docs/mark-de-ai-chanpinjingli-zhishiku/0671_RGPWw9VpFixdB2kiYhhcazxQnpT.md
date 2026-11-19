
# S01：最小智能体 — 一个循环就够了

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Q940dp8ujoFHnPxUFZBc4ZDjnSb](https://qqs7y1hozd1.feishu.cn/docx/Q940dp8ujoFHnPxUFZBc4ZDjnSb)

## The Agent Loop (Agent 循环)

构建一个智能体，核心就是一个循环：让智能体不断调用工具，直到它自己判断不再需要调用为止

这就是整个 Agent，后面 11 个章节都在这个循环上叠加机制

整个过程完全交给智能体来决定：把用户的问题丢给大模型，大模型觉得需要用工具就用，用完把结果喂回去，大模型继续想，想完了要么继续用工具，要么直接给你答案

![](img/b7cb7592ed9973f95eef672bc36a63de.png)

什么时候停？大模型自己决定，不需要你写任何判断逻辑。

跑起来之后我试了一下让它列出目录里的 Python 文件，它先用了 Linux 命令失败了（因为我是 Windows），然后自己换成了 Windows 命令，成功了，整个纠错过程完全是模型自己完成的，我什么都没干

![](img/1f5a0e9a64819e4acf0373908346a1cd.png)

[`s01`] `s02` > `s03` > `s04` > `s05` > `s06` | `s07` > `s08` > `s09` > `s10` > `s11` > `s12`

> "One loop & Bash is all you need" -- 一个工具 + 一个循环 = 一个 Agent。

> Harness 层: 循环 -- 模型与真实世界的第一道连接。

### 问题

语言模型能推理代码, 但碰不到真实世界 -- 不能读文件、跑测试、看报错。没有循环, 每次工具调用你都得手动把结果粘回去。你自己就是那个循环。

### 解决方案

```
+--------+      +-------+      +---------+
|  User  | ---> |  LLM  | ---> |  Tool   |
| prompt |      |       |      | execute |
+--------+      +---+---+      +----+----+
                    ^                |
                    |   tool_result  |
                    +----------------+
                    (loop until stop_reason != "tool_use")
```

一个退出条件控制整个流程。循环持续运行, 直到模型不再调用工具。

### 工作原理

1. 用户 `prompt` 作为第一条消息。

```
messages.append({ "role": "user", "content": query })
```

1. 将消息和工具定义一起发给 LLM。

```
response = client.messages.create(
    model=MODEL, system=SYSTEM, messages=messages,
    tools=TOOLS, max_tokens=8000,
)
```

1. 追加助手响应。检查 `stop_reason` -- 如果模型没有调用工具, 结束。

```
messages.append({ "role": "assistant", "content": response.content })
if response.stop_reason != "tool_use":
    return
```

1. 执行每个工具调用, 收集结果, 作为 `user` 消息追加。回到第 2 步。

```
results = []
for block in response.content:
    if block.type == "tool_use":
        output = run_bash(block.input["command"])
        results.append({
            "type": "tool_result",
            "tool_use_id": block.id,
            "content": output,
        })
messages.append({ "role": "user", "content": results })
```

组装为一个完整函数:

```
def agent_loop(query):
    messages = [{ "role": "user", "content": query }]
    while True:
        response = client.messages.create(
            model=MODEL, system=SYSTEM, messages=messages,
            tools=TOOLS, max_tokens=8000,
        )
        messages.append({ "role": "assistant", "content": response.content })

        if response.stop_reason != "tool_use":
            return

        results = []
        for block in response.content:
            if block.type == "tool_use":
                output = run_bash(block.input["command"])
                results.append({
                    "type": "tool_result",
                    "tool_use_id": block.id,
                    "content": output,
                })
        messages.append({ "role": "user", "content": results })
```

不到 30 行, 这就是整个 Agent。后面 11 个章节都在这个循环上叠加机制 -- 循环本身始终不变。

### 变更内容

### 试一试

`cd learn-claude-code`

`python agents/s01_agent_loop.py`

试试这些 `prompt` (英文 `prompt` 对 LLM 效果更好, 也可以用中文):

1. Create a file called `hello.py` that prints "Hello, World!"

1. List all Python files in this directory

1. What is the current `git` branch?

1. Create a directory called `test_output` and write 3 files in it
