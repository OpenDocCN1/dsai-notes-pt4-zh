
# 11 自任触发源，面向用户使用路径

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/Vn4vd00QHovcNPxzmKgck91onsg](https://qqs7y1hozd1.feishu.cn/docx/Vn4vd00QHovcNPxzmKgck91onsg)

支持从多种入口触发 agent（CLI、Webhook、Cron、Slack、Email 等），无需依赖专门 chat UI 。

![](img/668b47992b08cc62689579fc49244c6d.png)

此要素强调 Agent 的无处不在性和易用性。

> Agent 应能从各种渠道被触发，并通过用户习惯渠道响应。

> Agent 不应局限于特定界面或平台，而应融入用户日常工作流，无论是 Slack、电子邮件、短信或其他自定义应用。

此要素与通过简单 API 启动/暂停/恢复（Factor 6）和通过工具调用联系人工（Factor 7）紧密相关。

> Agent 若能轻松启动、暂停、恢复，并能通过工具调用与人工交互，则具备在任何地方被触发和响应的基础。

### 此方法带来以下好处：

> • 以用户为中心：
> Agent 出现在用户最常用沟通渠道，提供无缝体验，让用户感觉 Agent 是真正的数字同事。

> • 外部循环 Agent：
> Agent 可由非人工事件（如定时任务、系统告警、数据变化）触发，即使长时间运行，关键时也能主动联系人工寻求帮助、反馈或批准。

> • 高风险工具：
> Agent 若能快速与各种人工沟通协作，可赋予其执行更高风险操作的能力，如发送外部邮件、更新生产数据等。清晰标准和可审计性确保 Agent 执行这些操作时的信心。
