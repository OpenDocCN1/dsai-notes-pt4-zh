
# 09将错误压缩进入上下文

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/X0RhdoHPeoNTIVx49d6cNW6SnXg](https://qqs7y1hozd1.feishu.cn/docx/X0RhdoHPeoNTIVx49d6cNW6SnXg)

> 用简明、结构化的方式将失败信息注入下个 prompt 中，避免无关或冗余错误日志干扰推理。

> 此要素关注 Agent 的自我修复能力。Agent 执行任务时，工具调用失败常见。

> 优秀 LLM 能通过读取错误消息或堆栈跟踪，调整后续工具调用，实现自我纠正。此要素核心是将错误信息以对 LLM 有用的方式注入上下文窗口，以便 LLM 理解学习。

![](img/1bf76201157277c479debe0c38b4ebd2.png)

## 此要素实现通常包括：

> 错误捕获：在工具调用周围使用 `try-except` 块捕获异常。

> 错误格式化：将原始错误信息（如堆栈跟踪）格式化为 LLM 易懂的简洁形式，提取关键错误类型、消息和上下文。

> 错误注入：将格式化错误信息作为新事件类型（如 `error`）添加到 Agent 上下文窗口。

> 重试机制：可实现简单重试计数器，限制对同一工具的连续失败尝试次数，防止 Agent 陷入无限循环。

## 以下是具体实现示例：

> 虽然大多数框架都实现了这一功能，但完全可以仅实现此项功能，而无需考虑其他 11 个因素。

```
thread = {&quot;events&quot;: [initial_message]}

while True:
  next_step = await determine_next_step(thread_to_prompt(thread))
  thread[&quot;events&quot;].append({
    &quot;type&quot;: next_step.intent,
    &quot;data&quot;: next_step,
  })
  try:
    result = await handle_next_step(thread, next_step) # our switch statement
  except Exception as e:
    # if we get an error, we can add it to the context window and try again
    thread[&quot;events&quot;].append({
      &quot;type&quot;: &#x27;error&#x27;,
      &quot;data&quot;: format_error(e),
    })
    # loop, or do whatever else here to try to recover
```

建议针对特定工具调用实现错误计数器（`errorCounter`），将其调用次数限制在约3次尝试或其他符合业务场景的合理逻辑范围内。以下是三种实现方案：

```
consecutive_errors = 0

while True:

  # ... existing code ...

  try:
    result = await handle_next_step(thread, next_step)
    thread[&quot;events&quot;].append({
      &quot;type&quot;: next_step.intent + &#x27;_result&#x27;,
      data: result,
    })
    # success! reset the error counter
    consecutive_errors = 0
  except Exception as e:
    consecutive_errors += 1
    if consecutive_errors &lt; 3:
      # do the loop and try again
      thread[&quot;events&quot;].append({
        &quot;type&quot;: &#x27;error&#x27;,
        &quot;data&quot;: format_error(e),
      })
    else:
      # break the loop, reset parts of the context window, escalate to a human, or whatever else you want to do
      break
  }
}
```

## 此方法优势：

• 自愈能力：LLM 识别并响应错误，提高 Agent 鲁棒性和任务完成能力。

• 持久性：即使单个工具调用失败，Agent 也能继续运行，尝试从错误中恢复。

需注意，过度注入原始错误信息可能导致 LLM 失控，重复相同错误。结合掌控控制流（Factor 8）和掌控上下文窗口（Factor 3），开发者可精细管理错误信息呈现方式，如多次失败后移除旧错误事件，或将问题升级给人工。最重要的是，构建小型、专注的 Agent（Factor 10），可从根本上减少错误发生可能性和复杂性。
