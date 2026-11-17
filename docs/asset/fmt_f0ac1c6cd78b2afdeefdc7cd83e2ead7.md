
# 5) 解析 tool 的 <tool_response> JSON 内容为字符串

```python
_TOOLRESP_RE = re.compile(r"<tool_response>\s*(\{.*?\})\s*</tool_response>", re.DOTALL)

def parse_tool_response_value(value: str) -> str:
    """
    输入 Hermes 的 tool.value，输出一个 JSON 字符串（尽量规范化）。
    """
    if not value:
        return ""
    m = _TOOLRESP_RE.search(value)
    raw = (m.group(1).strip() if m else value.strip())
    # 尝试 JSON
    try:
        obj = json.loads(raw)
        return json.dumps(obj, ensure_ascii=False)
    except Exception:
        pass
    # 尝试 Python 字面量
    try:
        obj = ast.literal_eval(raw)
        return json.dumps(obj, ensure_ascii=False)
    except Exception:
        # 留原文（让模板做 tojson），至少是字符串
        return raw
```

## 5.1 开发者文本噪音清洗

```python
DEV_NOISE_PATTERNS = [
    r"Here are the available tools:.*",
    r"Use the following pydantic model.*",
    r"Each function call should be enclosed.*",
    r"You are a function calling AI model\..*",
    r"within\s*<tools>\s*</tools>\s*XML tags.*",
]
DEV_NOISE_RE = re.compile("|".join(DEV_NOISE_PATTERNS), re.DOTALL)

def clean_developer_text(s: str) -> str:
    s = _TOOLS_BLOCK_RE.sub("", s or "")  # 先去掉 <tools>…</tools>
    s = DEV_NOISE_RE.sub("", s).strip()
    s = re.sub(r"\n{3,}", "\n\n", s)
    return s
```

## 5.2 开发者文本最终清洗

```python
import re

# 原有噪音模式
DEV_NOISE_PATTERNS = [
    r"Here are the available tools:.*",
    r"Use the following pydantic model.*",
    r"Each function call should be enclosed.*",
    r"You are a function calling AI model\..*",
    r"within\s*<tools>\s*</tools>\s*XML tags.*",
    # 新增：清理 <think> 相关说明
    r"You should enclose.*?<think>.*?</think>.*?(solution|response).*",
    r"enclose your thoughts.*?<think>.*?</think>.*",
    r"internal monologue.*?<think>.*?</think>.*",
]
DEV_NOISE_RE = re.compile("|".join(DEV_NOISE_PATTERNS), re.DOTALL | re.IGNORECASE)

def final_clean_developer_text(s: str) -> str:
    """
    清理 Hermes system 提示，去掉工具声明、pydantic schema、<think> 标签说明，
    只保留干净的通用 developer 提示。
    """
    if not s:
        return ""
    # 先去掉 <tools>...</tools> 块
    s = _TOOLS_BLOCK_RE.sub("", s)
    # 去掉定义的噪音段落
    s = DEV_NOISE_RE.sub("", s).strip()
    # 压缩多余空行
    s = re.sub(r"\n{3,}", "\n\n", s)
    return s
```

## 5.3 消息清洗与 think 标签处理

```python
import re, json

_UNPAIRED_THINK_OPEN  = re.compile(r"<think>\s*", re.IGNORECASE)
_UNPAIRED_THINK_CLOSE = re.compile(r"\s*</think>", re.IGNORECASE)

def _strip_unpaired_think_tags(text: str) -> str:
    if not isinstance(text, str):
        return "" if text is None else str(text)
    # 去掉单独出现（或未配对）的 <think> / </think> 标签
    text = _UNPAIRED_THINK_OPEN.sub("", text)
    text = _UNPAIRED_THINK_CLOSE.sub("", text)
    return text

def _drop_none_fields(msg: dict) -> dict:
    # 删掉值为 None 的键
    return {k: v for k, v in msg.items() if v is not None}

def sanitize_messages_for_gptoss(messages: list) -> list:
    cleaned = []
    for m in messages:
        m = dict(m)  # 复制一份
        role = m.get("role")

        # 1) 去掉 thinking=None
        if "thinking" in m and (m["thinking"] is None or m["thinking"] == ""):
            m.pop("thinking", None)

        # 2) 对 content 做兜底清洗（移除残留 think 标签）
        if "content" in m:
            m["content"] = _strip_unpaired_think_tags(m["content"])

        # 3) tool 消息的 content 必须是字符串
        if role == "tool":
            c = m.get("content", "")
            if not isinstance(c, str):
                try:
                    m["content"] = json.dumps(c, ensure_ascii=False)
                except Exception:
                    m["content"] = str(c)

        # 4) assistant+tool_calls 情况：不能带 content
        if role == "assistant" and "tool_calls" in m and m.get("tool_calls"):
            m.pop("content", None)  # 保守起见清掉

        # 5) 删除 None 字段
        m = _drop_none_fields(m)

        # 6) 丢弃完全空的消息（极少数脏数据）
        if role and (m.get("content") or m.get("tool_calls") or m.get("thinking") or role == "tool"):
            cleaned.append(m)

    return cleaned
```

# 6) 单条样本转换：Hermes → gpt-oss (messages + tools)

```python
def hermes_to_gptoss_example(ex: dict):
    conv = ex.get("conversations") or []
    top_tools_raw = ex.get("tools")  # 可能是 str / list[str] / list[dict] / dict / None

    messages = []
    idx = 0
    sys_text = ""

    if len(conv) and conv[0].get("from") == "system":
        sys_text = conv[0].get("value", "") or ""
        # 若顶层无 tools，从 system 的 <tools>... 抽
        if not top_tools_raw:
            extracted = extract_tools_from_system(sys_text)
            if extracted:
                top_tools_raw = extracted

        dev_text = final_clean_developer_text(sys_text)
        if dev_text.strip():
            messages.append({"role": "developer", "content": dev_text.strip()})
        idx = 1

    # 1) 工具归一化（你已有）
    top_tools_norm = parse_tools_any(top_tools_raw)
    gptoss_tools = hermes_tools_to_gptoss_tools(top_tools_norm or [])

    # 2) 主循环（加一点点健壮性）
    for turn in conv[idx:]:
        who = (turn.get("from") or "").lower()
        val = turn.get("value", "")
        if not isinstance(val, str):  # 防御：确保是字符串
            val = str(val)

        if who in ("human", "user"):
            if val.strip():
                messages.append({"role": "user", "content": val})

        elif who in ("gpt", "assistant"):
            thinking, calls = parse_assistant_content(val)
            if calls:
                for j, call in enumerate(calls):
                    msg = {"role": "assistant", "tool_calls": [call]}
                    if j == 0 and thinking:   # 只有第一条带 thinking
                        msg["thinking"] = thinking
                    messages.append(msg)
            else:
                # 非工具调用回合
                content = val
                if thinking is not None:
                    content = _THINK_RE.sub("", val).strip()
                messages.append(
                    {"role": "assistant", **({"thinking": thinking} if thinking else {}), "content": content}
                )

        elif who == "tool":
            tool_payload = parse_tool_response_value(val)  # -> JSON 字符串
            messages.append({"role": "tool", "content": tool_payload})

    # 3) 收尾清理（你已有）
    messages = sanitize_messages_for_gptoss(messages)
    return {"messages": messages, "tools": gptoss_tools}
```
