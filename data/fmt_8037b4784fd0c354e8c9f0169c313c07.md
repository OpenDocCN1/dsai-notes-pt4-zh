
# 03掌控 Context 窗口

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Qvd2dtRnsoFd4DxPFMPcBIWenwb](https://qqs7y1hozd1.feishu.cn/docx/Qvd2dtRnsoFd4DxPFMPcBIWenwb)

> 使用业务结构化截面替代冗长消息格式，优化 token 消耗、提高系统效率与稳定性。

> 掌控上下文窗口强调开发者应主动管理和构建传递给 LLM 的上下文信息，而非仅依赖标准消息格式。

LLM 是无状态函数，将输入转化为输出。为获最佳输出，需提供最佳输入，即进行精细的“上下文工程”。

上下文窗口不仅是对话历史，它包含 LLM 决策所需的一切信息：

> • 提示词和指令：LLM 接收的初始指令和角色设定。
> • 外部数据：通过检索增强生成（RAG）等方式获取的相关文档或外部数据。
> • 历史状态：过去的工具调用、执行结果、错误信息及其他相关历史记录。
> • 记忆：来自相关但独立历史或对话的过去消息或事件。
> • 结构化输出指令：关于 LLM 应输出何种结构化数据的明确指示。

![](img/6931ac392397e09a52523c6509a073b7.png)

此要素核心在于，开发者可构建自定义上下文格式，最大化 LLM 理解能力和令牌效率。

> 例如，将所有相关信息打包到一个用户消息中，使用 XML 或其他自定义标记组织不同类型数据（如 Slack 消息、Git 标签列表、工具调用结果等）。

## 标准上下文格式与自定义格式对比

多数 LLM 客户端采用如下标准消息格式：

```json
[
  {
    "role": "system",
    "content": "You are a helpful assistant..."
  },
  {
    "role": "user",
    "content": "Can you deploy the backend?"
  },
  {
    "role": "assistant",
    "content": null,
    "tool_calls": [
      {
        "id": "1",
        "name": "list_git_tags",
        "arguments": "{}"
      }
    ]
  },
  {
    "role": "tool",
    "name": "list_git_tags",
    "content": "{\"tags\": [{\"name\": \"v1.2.3\", \"commit\": \"abc123\", \"date\": \"2024-03-15T10:00:00Z\"}, {\"name\": \"v1.2.2\", \"commit\": \"def456\", \"date\": \"2024-03-14T15:30:00Z\"}, {\"name\": \"v1.2.1\", \"commit\": \"abe033d\", \"date\": \"2024-03-13T09:15:00Z\"}]}",
    "tool_call_id": "1"
  }
]
```

尽管标准消息格式能满足多数应用场景，但若想充分释放当前 LLM 的潜力，就必须以最高效的 token 和注意力分配方式传递上下文信息。

作为标准消息格式的替代方案，您可针对具体用例构建定制化上下文格式。例如：通过自定义数据结构，按需将其封装或拆解至 `user`/`system`/`assistant`/`tool` 等任一消息类型中。

典型做法示例：将整个上下文窗口整合为单个 `user` 消息。

```json
[
  {
    "role": "system",
    "content": "You are a helpful assistant..."
  },
  {
    "role": "user",
    "content": |
            Here's everything that happened so far:
        
        <slack_message>
            From: @alex
            Channel: #deployments
            Text: Can you deploy the backend?
        </slack_message>
        
        <list_git_tags>
            intent: "list_git_tags"
        </list_git_tags>
        
        <list_git_tags_result>
            tags:
              - name: "v1.2.3"
                commit: "abc123"
                date: "2024-03-15T10:00:00Z"
              - name: "v1.2.2"
                commit: "def456"
                date: "2024-03-14T15:30:00Z"
              - name: "v1.2.1"
                commit: "ghi789"
                date: "2024-03-13T09:15:00Z"
        </list_git_tags_result>
        
        what's the next step?
    }
]
```

虽然模型可能通过您提供的工具模式推断出您询问的是下一步操作，但将其整合到提示词模板中总是有益无害的。

## 代码示例

我们可以采用类似以下方式构建：

```python
class Thread:
  events: List[Event]

class Event:
  # could just use string, or could be explicit - up to you
  type: Literal["list_git_tags", "deploy_backend", "deploy_frontend", "request_more_information", "done_for_now", "list_git_tags_result", "deploy_backend_result", "deploy_frontend_result", "request_more_information_result", "done_for_now_result", "error"]
  data: ListGitTags | DeployBackend | DeployFrontend | RequestMoreInformation |  
        ListGitTagsResult | DeployBackendResult | DeployFrontendResult | RequestMoreInformationResult | string

def event_to_prompt(event: Event) -> str:
    data = event.data if isinstance(event.data, str) \
           else stringifyToYaml(event.data)

    return f"<{event.type}>\n{data}\n</{event.type}>"


def thread_to_prompt(thread: Thread) -> str:
  return '\n\n'.join(event_to_prompt(event) for event in thread.events)
```

## 示例上下文窗口

采用此方法时，各上下文窗口可能呈现如下形式：

初始 Slack 请求：

```xml
<slack_message>
    From: @alex
    Channel: #deployments
    Text: Can you deploy the latest backend to production?
</slack_message>
```

列出 Git 标签后：

```xml
<slack_message>
    From: @alex
    Channel: #deployments
    Text: Can you deploy the latest backend to production?
    Thread: []
</slack_message>

<list_git_tags>
    intent: "list_git_tags"
</list_git_tags>

<list_git_tags_result>
    tags:
      - name: "v1.2.3"
        commit: "abc123"
        date: "2024-03-15T10:00:00Z"
      - name: "v1.2.2"
        commit: "def456"
        date: "2024-03-14T15:30:00Z"
      - name: "v1.2.1"
        commit: "ghi789"
        date: "2024-03-13T09:15:00Z"
</list_git_tags_result>
```

出现错误并恢复后：

```xml
<slack_message>
    From: @alex
    Channel: #deployments
    Text: Can you deploy the latest backend to production?
    Thread: []
</slack_message>

<deploy_backend>
    intent: "deploy_backend"
    tag: "v1.2.3"
    environment: "production"
</deploy_backend>

<error>
    error running deploy_backend: Failed to connect to deployment service
</error>

<request_more_information>
    intent: "request_more_information_from_human"
    question: "I had trouble connecting to the deployment service, can you provide more details and/or check on the status of the service?"
</request_more_information>

<human_response>
    data:
      response: "I'm not sure what's going on, can you check on the status of the latest workflow?"
</human_response>
```

接下来您可以采取以下操作：

```python
nextStep = await determine_next_step(thread_to_prompt(thread))
```

```json
{
  "intent": "get_workflow_status",
  "workflow_name": "tag_push_prod.yaml",
}
```

XML 格式仅是一个示例——关键在于您可以为应用程序构建符合自身逻辑的格式。通过尝试不同的上下文结构和数据存储/传递策略，您将获得更高质量的输出。

## 自定义上下文窗口的核心优势

信息密度：以最大化 LLM 理解能力的方式结构化信息

错误处理：采用有助于 LLM 恢复的格式记录错误信息，建议在问题解决后从上下文窗口移除相关错误记录

安全性：完全控制传递给 LLM 的信息，过滤敏感数据

灵活性：根据实际使用效果动态调整格式

令牌效率：为上下文格式优化令牌使用效率和 LLM 理解度

上下文窗口包含：提示词、指令、RAG 文档、历史记录、工具调用和记忆模块

请牢记：上下文窗口是您与 LLM 交互的主界面。通过自主设计信息结构和呈现方式，可显著提升智能体性能。

示例说明信息密度：相同信息量下，优化后的格式可减少 30% 令牌消耗（注：此处保留技术指标数据不译）

![](img/4f3c7102a0d169f411be76085d2a9a86.png)

此方式允许：

• 信息密度：以 LLM 最易理解和处理的方式组织信息，提高信息利用率。

• 错误处理：以有助于 LLM 恢复的方式包含错误信息，可选择性隐藏已解决错误。

• 安全性：控制哪些信息传递给 LLM，过滤敏感数据。

• 灵活性：根据用例需求灵活调整上下文格式，通过实验找到最佳实践。

• 令牌效率：优化上下文格式，在有限令牌预算内传递更多有效信息。

通过掌控上下文窗口，开发者能更精细控制 LLM 输入，显著提升 Agent 性能、可靠性和自我修复能力。Agent 能更好理解当前状态、历史事件和外部信息，做出更准确、智能的决策。
