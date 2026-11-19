
# S02：工具箱 — 告诉智能体能用什么、不能碰什么

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/UOTHds4OzoWHlGxcHJQcHJbonYE](https://qqs7y1hozd1.feishu.cn/docx/UOTHds4OzoWHlGxcHJQcHJbonYE)

## Tool Use (工具使用)

光有循环不够，第一课里智能体只有一个 `bash` 命令行，什么都能干，太危险了。

这一课做了两件事：

第一，建了个工具箱，把所有允许它用的工具注册进去（读文件、写文件、编辑文件、跑命令），没注册的它根本看不见、也用不了。

第二，给文件工具加了个围栏，只能操作项目目录里的文件，防止智能体越界访问系统文件。

![](img/3ef54d0a8708a51ccba8b2097a1566f4.png)

最强的是：加新工具完全不用改循环代码，往工具箱里多注册一条就行，循环从头到尾一行没变，后面每一课都在往工具箱里加东西，循环始终不动。

`s01` > `[s02]` `s03` > `s04` > `s05` > `s06` | `s07` > `s08` > `s09` > `s10` > `s11` > `s12`

> "加一个工具, 只加一个 `handler`" -- 循环不动, 新工具进 `dispatch map` 就行。

> Harness 层: 工具分发 -- 扩展模型能触达的边界。

### 问题

只有 `bash` 时, 所有操作都走 shell。`cat` 截断不可预测, `sed` 遇到特殊字符就崩, 每次 `bash` 调用都是不受约束的安全面。专用工具 (`read_file`, `write_file`) 可以在工具层面做路径沙箱。

关键洞察: 加工具不需要改循环。

### 解决方案

```
+--------+      +-------+      +------------------+
|  User  | ---> |  LLM  | ---> | Tool Dispatch    |
| prompt |      |       |      | {                |
+--------+      +---+---+      |   bash: run_bash |
                    ^           |   read: run_read |
                    |           |   write: run_wr  |
                    +-----------+   edit: run_edit |
                    tool_result | }                |
                                +------------------+
```

`dispatch map` 是一个字典：`{tool_name: handler_function}`。一次查找替代任何 `if/elif` 链。

### 工作原理

1. 每个工具有一个处理函数。路径沙箱防止逃逸工作区。

```
def safe_path(p: str) -> Path:
    path = (WORKDIR / p).resolve()
    if not path.is_relative_to(WORKDIR):
        raise ValueError(f"Path escapes workspace: {p}")
    return path

def run_read(path: str, limit: int = None) -> str:
    text = safe_path(path).read_text()
    lines = text.splitlines()
    if limit and limit < len(lines):
        lines = lines[:limit]
    return "\n".join(lines)[:50000]
```

1. `dispatch map` 将工具名映射到处理函数。

```
TOOL_HANDLERS = {
    "bash":       lambda **kw: run_bash(kw["command"]),
    "read_file":  lambda **kw: run_read(kw["path"], kw.get("limit")),
    "write_file": lambda **kw: run_write(kw["path"], kw["content"]),
    "edit_file":  lambda **kw: run_edit(kw["path"], kw["old_text"],
                                        kw["new_text"]),
}
```

1. 循环中按名称查找处理函数。循环体本身与 `s01` 完全一致。

```
for block in response.content:
    if block.type == "tool_use":
        handler = TOOL_HANDLERS.get(block.name)
        output = handler(**block.input) if handler \
            else f"Unknown tool: {block.name}"
        results.append({
            "type": "tool_result",
            "tool_use_id": block.id,
            "content": output,
        })
```

加工具 = 加 `handler` + 加 `schema`。循环永远不变。

### 相对 s01 的变更

### 试一试

```
cd learn-claude-code
python agents/s02_tool_use.py
```

试试这些 prompt (英文 prompt 对 LLM 效果更好, 也可以用中文):

1. Read the file `requirements.txt`

1. Create a file called `greet.py` with a `greet(name)` function

1. Edit `greet.py` to add a docstring to the function

1. Read `greet.py` to verify the edit worked
