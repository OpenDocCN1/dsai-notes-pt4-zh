
# 01自然语言绑定工具调用

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Wo7WdFyTXoaQtlxX4S7c3IeSnzd](https://qqs7y1hozd1.feishu.cn/docx/Wo7WdFyTXoaQtlxX4S7c3IeSnzd)

> 将用户输入转换为结构化工具调用，而非在 LLM 内进行多步推理，从而增强可追踪性与错误隔离。

此要素强调 LLM 在 `Agent` 架构中的核心作用：作为意图解析器和行动规划器。

> LLM 理解用户自然语言输入，根据知识和工具定义，生成结构化工具调用。

> 这些调用是 `Agent` 与外部世界交互的接口。

> 通过将自然语言转化为结构化输出，`Agent` 实现与外部系统（如 API、数据库、其他服务）的无缝集成。

这种分离使 LLM 专注于理解和推理，确定性代码负责可靠执行操作，提高 `Agent` 可靠性和可测试性。

![](img/4c7a2cb41f13253b92c36bfecf938ba1.png)

## 关键点

> **意图解析：**LLM 将自然语言请求解析为明确、可执行的意图。

> **结构化输出：**LLM 生成标准化结构化数据（如 JSON），描述要调用的工具及其参数。

> **确定性执行：**结构化输出由确定性代码处理，确保操作准确性和可预测性。

> **解耦：**LLM 推理能力与工具执行逻辑解耦，提高系统模块化和可维护性。

转换为描述 `Stripe` API 调用的结构化对象

```json
{
  "function": {
    "name": "create_payment_link",
    "parameters": {
      "amount": 750,
      "customer": "cust_128934ddasf9",
      "product": "prod_8675309",
      "price": "prc_09874329fds",
      "quantity": 1,
      "memo": "Hey Jeff - see below for the payment link for the february ai tinkerers meetup"
    }
  }
}
```

注：实际 `Stripe` API 的调用更为复杂，一个真正实现此功能的智能体（如视频所示）需要列举客户信息、商品列表、价格详情等数据来构建包含正确 ID 的有效载荷，或者将这些 ID 包含在提示/上下文窗口中（下文将说明这两种方式本质上是相通的！）

随后，确定性代码即可获取该有效载荷并进行后续处理。（关于这一点，我们将在第三部分展开详述）

```python
# The LLM takes natural language and returns a structured object
nextStep = await llm.determineNextStep(
  """
  create a payment link for $750 to Jeff 
  for sponsoring the february AI tinkerers meetup
  """
  )

# Handle the structured output based on its function
if nextStep.function == 'create_payment_link':
    stripe.paymentlinks.create(nextStep.parameters)
    return  # or whatever you want, see below
elif nextStep.function == 'something_else':
    # ... more cases
    pass
else:  # the model didn't call a tool we know about
    # do something else
    pass
```

注：在实际应用中，完整智能体会接收 API 调用结果并据此循环处理，最终返回类似以下内容：

（示例代码/输出位置）

不过在本部分，我们将跳过该步骤，将其留待后续环节处理（是否纳入该功能由您自行决定）
