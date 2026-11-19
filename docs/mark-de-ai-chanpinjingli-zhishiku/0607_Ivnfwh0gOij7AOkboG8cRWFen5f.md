
# 12 让 Agent 成为无状态 Reducer

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/N4LSd46AsonKPLxLbBFc6J0PnSb](https://qqs7y1hozd1.feishu.cn/docx/N4LSd46AsonKPLxLbBFc6J0PnSb)

Agent 逻辑纯粹函数式，所有持久状态外部托管，可水平缩放、断点恢复，避免副作用。

![](img/b3ab137d95f2434101c7972c6ee174c7.png)

此要素概括 Agent 设计重要理念：

将 Agent 视为纯函数，接收状态（上下文窗口），返回新状态（下一步操作）。

此设计借鉴函数式编程 Reducer 概念，强调 Agent 无状态性和确定性。

## 无状态 Reducer Agent

- 纯函数：

  Agent 输出完全由输入决定，不依赖外部可变状态。相同输入，总产生相同输出。

- 状态通过上下文传递：

  Agent 状态不存储在内部变量，而是通过上下文窗口（历史事件和信息）显式传递。Agent 接收上下文，处理后生成下一步操作，将结果作为新事件添加到上下文。

- 可预测性：

  Agent 无状态，行为高度可预测，调试、测试和推理更易。

- 可伸缩性：

  无状态特性使 Agent 易于水平扩展，各实例独立处理请求，无需担心共享状态。

- 持久性：

  Agent 整个历史（上下文）可持久化，需要时重新加载，实现持久化和恢复。
