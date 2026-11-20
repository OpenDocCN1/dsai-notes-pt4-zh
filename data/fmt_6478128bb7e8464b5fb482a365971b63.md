
# 04工具应为结构化输出

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/XIzDdygyxoPmzCxBjwxcyjeWnef](https://qqs7y1hozd1.feishu.cn/docx/XIzDdygyxoPmzCxBjwxcyjeWnef)

LLM 只负责生成结构化 JSON 表示意图；由 deterministic 代码处理实际操作，从而避免 LLM 执行直接跳变。

此要素揭示 Agent 中工具调用的本质：它们是 LLM 生成的结构化数据，触发确定性代码执行。

LLM 核心任务是决定“做什么”，实际“如何做”由确定性代码控制。

![](img/219339a6b3e943dc2c40991173c03880.png)

## 此模式带来清晰分离

1. LLM 输出结构化 JSON：LLM 根据自然语言理解和上下文，生成描述下一步操作的结构化数据。

2. 确定性代码执行：应用程序代码接收并解析结构化输出，执行实际确定性操作，如调用外部 API、更新数据库或触发其他服务。

3. 结果反馈：操作结果被捕获并反馈回 LLM 上下文，供 LLM 后续推理。

## 此设计理念优势

- 解耦：LLM 专注于决策和意图表达，执行细节由可靠确定性代码处理，降低 LLM 出错风险。

- 灵活性：开发者可根据业务需求，自由定义工具结构和行为，不受 LLM 框架预设限制。

- 可控性：即使 LLM“调用”工具，开发者仍可在确定性代码层面进行额外验证、审批或自定义逻辑，确保高风险操作安全。

- 可测试性：工具执行逻辑确定，易于单元测试和集成测试。

## 示例

假设您提供了两个工具：`CreateIssue`（创建问题）和`SearchIssues`（搜索问题）。当要求 LLM"从多个工具中选择一个使用时"，本质上只需让它输出可解析为工具对象的 JSON 数据即可。

```
class Issue:
  title: str
  description: str
  team_id: str
  assignee_id: str

class CreateIssue:
  intent: "create_issue"
  issue: Issue

class SearchIssues:
  intent: "search_issues"
  query: str
  what_youre_looking_for: str
```

## 该模式简洁高效

1. LLM 输出结构化 JSON

2. 确定性代码执行对应操作（如调用外部 API）

3. 结果反馈至上下文窗口

## 这种设计实现了 LLM 决策层与应用程序执行层的清晰解耦

- LLM 负责决策：确定需要执行的操作

- 代码控制执行：决定具体实现方式

```
if nextStep.intent == 'create_payment_link':
    stripe.paymentlinks.create(nextStep.parameters)
    return # or whatever you want, see below
elif nextStep.intent == 'wait_for_a_while': 
    # do something monadic idk
else: #... the model didn't call a tool we know about
    # do something else
```

## 注

关于“纯文本提示词”（plain prompting）、“工具调用”（tool calling）和“JSON模式”（JSON mode）的优劣比较及其性能权衡已有大量讨论。相关资源链接将在后续提供，但本文暂不展开。可参考以下资料：

- 《Prompting vs JSON Mode vs Function Calling vs Constrained Generation vs SAP》
- 《何时使用函数调用、结构化输出或 JSON 模式？》
- 《OpenAI JSON 模式与函数调用对比分析》

“下一步操作”可能不仅限于“执行纯函数并返回结果”。当您将“工具调用”视为 LLM 输出 JSON 描述确定性代码应执行的操作时，便能释放出极大的灵活性。结合 factor 8 对控制流的自主掌控，可实现更强大的系统能力。
