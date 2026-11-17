
# CUA 集成流程

要在应用中集成计算机使用代理（CUA），需遵循以下步骤：

## 🌟 1. 发送请求

你需要调用 `computer-use-preview` 模型，并启用 `computer_use_preview` 工具，指定：

- 屏幕尺寸（`display_width`, `display_height`）
- 操作环境（`environment`：如 `browser`、`windows`、`ubuntu`）
- 目标任务（输入 `input` 指令）

### 🌟 示例：让 CUA 在 Bing 搜索 OpenAI 新闻

```python
from openai import OpenAI

client = OpenAI()

response = client.responses.create(
    model="computer-use-preview",
    tools=[
        {
            "type": "computer_use_preview",
            "display_width": 1024,
            "display_height": 768,
            "environment": "browser"  # 其他选项："mac", "windows", "ubuntu"
        }
    ],
    input=[
        {
            "role": "user",
            "content": "Check the latest OpenAI news on bing.com."
        }
        # 可选：包含初始界面的截图
        # {
        #     "type": "input_image",
        #     "image_url": f"data:image/png;base64,{screenshot_base64}"
        # }
    ],
    truncation="auto"  # 允许截断
)

print(response.output)
```

### 📌 效果

- 该 API 请求会启动计算机代理，让它在 Bing 上搜索 OpenAI 新闻。
- 可选：传入界面截图（Base64 编码）以帮助模型理解当前环境。

------

## 🌟 2. 接收建议操作

模型的 API 响应可能包含：

- 文本输出
- 计算机操作（`computer_call`）
- 其他工具调用

### 📌 示例：模型返回点击操作

```json
"output": [
    {
        "type": "reasoning",
        "id": "rs_67cc...",
        "content": []
    },
    {
        "type": "computer_call",
        "id": "cu_67cc...",
        "call_id": "call_zw3...",
        "action": {
            "type": "click",
            "button": "left",
            "x": 156,
            "y": 50
        },
        "pending_safety_checks": [],
        "status": "completed"
    }
]
```

### 📌 解析

- `computer_call`：告诉应用程序执行点击操作（在 `x=156, y=50`）。
- `reasoning`：提供推理过程（如果有）。
- `pending_safety_checks`：如果存在安全检查，应用程序需要处理。

------

# 启用 Web 搜索

你可以在 API 请求中，将 `web_search_preview` 添加到 `tools` 数组 以启用 Web 搜索。模型在处理请求时，可以选择是否使用搜索工具。

## 🌟 示例：启用 Web 搜索

```python
from openai import OpenAI

client = OpenAI()

response = client.responses.create(
    model="gpt-4o",
    tools=[{"type": "web_search_preview"}],  # 启用 Web 搜索工具
    input="What was a positive news story from today?"
)

print(response.output_text)
```

### 📌 效果

- 该 API 请求会调用 `web_search_preview`，允许模型在回答前搜索最新的新闻。
- 但模型可以自行决定是否使用该工具。

------

## 2. 强制使用 Web 搜索

如果希望确保模型一定使用 Web 搜索（避免它仅使用内部知识回答），可以设置 `tool_choice` 参数：

```python
tool_choice={"type": "web_search_preview"}
```

### 📌 作用

- 让 Web 搜索始终执行，而不是让模型决定是否使用搜索工具。
- 提升一致性，但可能会增加查询时间。

------

## 3. 输出格式与引用

如果模型调用了 Web 搜索，API 响应将包含两部分：

1. Web 搜索调用的 ID
2. 模型的回答，并带有网页来源的引用信息

### 📌 示例输出

```json
[
  {
    "type": "web_search_call",
    "id": "ws_67c9fa0502748190b7dd390736892e100be649c1a5ff9609",
    "status": "completed"
  },
  {
    "id": "msg_67c9fa077e288190af08fdffda2e34f20be649c1a5ff9609",
    "type": "message",
    "status": "completed",
    "role": "assistant",
    "content": [
      {
        "type": "output_text",
        "text": "On March 6, 2025, several news...",
        "annotations": [
          {
            "type": "url_citation",
            "start_index": 2606,
            "end_index": 2758,
            "url": "https://...",
            "title": "Title..."
          }
        ]
      }
    ]
  }
]
```

### 📌 解析

- `web_search_call`：存储搜索请求的 ID 和状态（已完成）。
- `message`：
  - `output_text`：模型生成的文本。
  - `annotations`：
    - `url_citation`：提供引用来源，包括 URL、标题、文本在回答中的位置。

### 📌 前端要求

- 当向用户展示搜索结果时，必须提供清晰的引用，并确保可点击。
