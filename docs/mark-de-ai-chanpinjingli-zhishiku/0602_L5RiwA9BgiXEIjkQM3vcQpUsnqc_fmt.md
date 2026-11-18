
# 07将人类交互视为工具调用

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/T48PdM4dyoQ0iHxpQy7cH9dLnDg](https://qqs7y1hozd1.feishu.cn/docx/T48PdM4dyoQ0iHxpQy7cH9dLnDg)

> 在高风险步骤中自动触发人类反馈工具，而非异常或旁路，使人工干预成为系统一部分。

传统 LLM API 在返回纯文本和结构化数据间存在高风险抉择。

> 此要素提出更可靠方法：LLM 始终输出结构化数据，并通过工具调用明确与人工交互。

> 即使 Agent 需联系人工，也通过特定工具调用（如 `request_human_input`）表达意图，而非直接自然语言提问。此工具调用包含人工输入问题、上下文及可能选项（如紧急程度、响应格式）。

Agent 发出此调用后，执行流可暂停，等待人工响应。

![](img/86d5f6dfdef038b57e2d00a134467980.png)

## 此方法优势

> 清晰指令：结构化工具调用使 LLM 更精确指示人工介入类型和目的。

> 内外循环分离：Agent 支持外部循环工作流，由非人工事件触发，关键时主动联系人工寻求帮助、反馈或批准。与传统人工-Agent 交互模式（内部循环）对比。

> 多人工访问：结构化事件轻松跟踪协调不同人工输入。

> 多 Agent 协作：简单抽象可扩展支持 Agent 间请求和响应。

> 持久性：结合启动/暂停/恢复（Factor 6），构建持久、可靠、可内省多人协作工作流。

将人工交互视为特殊工具调用，Agent 控制流更可预测、可管理。Agent 可处理高风险操作，执行前强制人工审批。为构建更智能、协作 Agent 系统奠定基础，使其更好融入人工工作流程。

建议尝试让 LLM 始终以 JSON 格式输出，并通过自然语言标记（如 `request_human_input` 或 `done_for_now`）明确其意图——相比传统的工具调用（如 `check_weather_in_city`），这种方式可能带来更好的效果。

```
class Options:
  urgency: Literal["low", "medium", "high"]
  format: Literal["free_text", "yes_no", "multiple_choice"]
  choices: List[str]

# Tool definition for human interaction
class RequestHumanInput:
  intent: "request_human_input"
  question: str
  context: str
  options: Options

# Example usage in the agent loop
if nextStep.intent == 'request_human_input':
  thread.events.append({
    type: 'human_input_requested',
    data: nextStep
  })
  thread_id = await save_state(thread)
  await notify_human(nextStep, thread_id)
  return # Break loop and wait for response to come back with thread ID
else:
  # ... other cases
```

稍后，您可能会收到来自 Slack、邮件、短信或其他事件处理系统的 Webhook 回调。

```
@app.post('/webhook')
def webhook(req: Request):
  thread_id = req.body.threadId
  thread = await load_state(thread_id)
  thread.events.push({
    type: 'response_from_human',
    data: req.body
  })
  # ... simplified for brevity, you likely don't want to block the web worker here
  next_step = await determine_next_step(thread_to_prompt(thread))
  thread.events.append(next_step)
  result = await handle_next_step(thread, next_step)
  # todo - loop or break or whatever you want

  return {"status": "ok"}
```

如果我们采用因子3中提到的XML式结构化上下文管理方案，经过多轮对话后，上下文窗口可能会呈现如下形态：

```
(snipped for brevity)

<slack_message>
    From: @alex
    Channel: #deployments
    Text: Can you deploy backend v1.2.3 to production?
    Thread: []
</slack_message>

<request_human_input>
    intent: "request_human_input"
    question: "Would you like to proceed with deploying v1.2.3 to production?"
    context: "This is a production deployment that will affect live users."
    options: {
        urgency: "high"
        format: "yes_no"
    }
</request_human_input>

<human_response>
    response: "yes please proceed"
    approved: true
    timestamp: "2024-03-15T10:30:00Z"
    user: "alex@company.com"
</human_response>

<deploy_backend>
    intent: "deploy_backend"
    tag: "v1.2.3"
    environment: "production"
</deploy_backend>

<deploy_backend_result>
    status: "success"
    message: "Deployment v1.2.3 to production completed successfully."
    timestamp: "2024-03-15T10:30:00Z"
</deploy_backend_result>
```

![](img/1017c8f29367590ff57a75b58ea932f7.png)
