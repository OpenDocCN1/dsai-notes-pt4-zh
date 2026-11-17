
# Azure AI 多智能体日志处理系统案例分析

这段代码实现了一个基于 Azure AI 的多智能体（Multi-Agent）日志处理系统，主要功能是通过两个专业 AI Agent 协作分析和处理系统日志。以下是核心功能解析：

## 双 Agent 架构

- `INCIDENT_MANAGER`：负责分析日志内容，诊断问题并给出处理建议
- `DEVOPS_ASSISTANT`：执行具体的运维操作，如服务重启、事务回滚等

## 典型工作流程

- 用户提供日志文件路径
- `INCIDENT_MANAGER` 分析日志并推荐处理方案
- `DEVOPS_ASSISTANT` 执行具体操作并记录结果
- 循环交互直到问题解决或确认无需操作

## 核心功能模块

- 日志处理：通过 `LogFilePlugin` 读取/追加日志
- 运维操作：`DevopsPlugin` 提供 5 种标准操作：
  - 重启服务（`restart_service`）
  - 回滚事务（`rollback_transaction`）
  - 重新部署资源（`redeploy_resource`）
  - 增加配额（`increase_quota`）
  - 问题升级（`escalate_issue`）
- 决策控制：通过自定义策略管理 Agent 交互顺序和终止条件

## 技术特点

- 使用 Azure AI 服务作为 Agent 底层
- 异步（`asyncio`）编程模型
- 插件式架构（通过`@kernel_function` 装饰器）
- 自动化的对话流程控制（`GroupChat` 管理）
- 内置错误处理和速率限制规避

## 输入输出

- 输入：日志文件目录（自动从 `sample_logs` 复制到 `logs`）
- 输出：控制台显示处理过程和结果，同时更新原始日志文件

这个系统典型应用于 IT 运维场景，可自动完成从日志分析到问题处理的完整闭环，显著提高故障处理效率。通过 Agent 分工协作，既保证了诊断准确性，又能安全执行具体运维操作。

```
Getting log files...

Ready to process log file: log2.log

INCIDENT_MANAGER > /home/zunhua/ai-agents/Labfiles/05-agent-orchestration/Python/logs/log2.log | Rollback transaction
DEVOPS_ASSISTANT > Transaction rolled back successfully.
INCIDENT_MANAGER > /home/zunhua/ai-agents/Labfiles/05-agent-orchestration/Python/logs/log2.log | No action needed.

Ready to process log file: log1.log

INCIDENT_MANAGER > /home/zunhua/ai-agents/Labfiles/05-agent-orchestration/Python/logs/log1.log | Restart service ServiceX
DEVOPS_ASSISTANT > Service ServiceX restarted successfully.
INCIDENT_MANAGER > /home/zunhua/ai-agents/Labfiles/05-agent-orchestration/Python/logs/log1.log | No action needed.

Ready to process log file: log4.log

INCIDENT_MANAGER > /home/zunhua/ai-agents/Labfiles/05-agent-orchestration/Python/logs/log4.log | Redeploy resource ResourceX
DEVOPS_ASSISTANT > Resource 'ResourceX' redeployed successfully.
INCIDENT_MANAGER > /home/zunhua/ai-agents/Labfiles/05-agent-orchestration/Python/logs/log4.log | No action needed.

Ready to process log file: log3.log

INCIDENT_MANAGER > /home/zunhua/ai-agents/Labfiles/05-agent-orchestration/Python/logs/log3.log | Increase quota
DEVOPS_ASSISTANT > Successfully increased quota.
INCIDENT_MANAGER > /home/zunhua/ai-agents/Labfiles/05-agent-orchestration/Python/logs/log3.log | No action needed.
```

这个输出展示了一个自动化事件管理系统的处理流程，涉及多个日志文件的处理。以下是详细分析：

## 处理流程模式

- 每个日志文件都遵循相同的处理模式：
  1. 显示`Ready to process log file: [文件名]`
  2. `INCIDENT_MANAGER` 读取日志并决定采取的动作
  3. `DEVOPS_ASSISTANT` 执行具体操作并反馈结果
  4. `INCIDENT_MANAGER` 确认"无后续操作需要"

## 处理的日志文件及对应操作

- `log2.log`：
  - 动作：回滚事务（Rollback transaction）
  - 结果：事务成功回滚
- `log1.log`：
  - 动作：重启服务 `ServiceX`（Restart service `ServiceX`）
  - 结果：`ServiceX` 成功重启
- `log4.log`：
  - 动作：重新部署资源 `ResourceX`（Redeploy resource `ResourceX`）
  - 结果：`ResourceX` 成功重新部署
- `log3.log`：
  - 动作：增加配额（Increase quota）
  - 结果：配额成功增加

## 系统特点

- 职责分离：`INCIDENT_MANAGER` 负责决策，`DEVOPS_ASSISTANT` 负责执行
- 闭环处理：每个操作都有明确的成功反馈和最终确认
- 标准化流程：所有日志文件采用相同处理流程
- 自动化程度高：从日志分析到操作执行完全自动化

## 可能的应用场景

- 自动化运维系统
- 云资源管理系统
- 服务监控和自愈系统

所有操作都成功完成，没有出现需要人工干预的情况，表明系统在这个案例中运行良好。

以下是使用 Mermaid 描述的 `INCIDENT_MANAGER` 和 `DEVOPS_ASSISTANT` 之间的协作流程，基于此前的日志输出分析：

```
sequenceDiagram
    autonumber
    participant LogFile as Log File
    participant IM as INCIDENT_MANAGER
    participant DA as DEVOPS_ASSISTANT

    loop For each log file
        LogFile->>IM: Ready to process log file: [filename]
        IM->>IM: Analyze log content
        alt If action needed
            IM->>DA: Command (e.g., "Rollback transaction")
            DA->>DA: Execute operation
            DA-->>IM: Success/Failure response
            IM->>IM: Verify resolution
        else No action needed
            IM->>IM: Skip processing
        end
        IM-->>LogFile: No action needed (completion)
    end
```

![](img/beda0216a90a86392ba0860cd174f412.png)

## 流程说明

1. **日志文件触发**：系统逐个处理日志文件（`log1.log`，`log2.log`，...）。

2. `INCIDENT_MANAGER`（`IM`）：
   - 读取日志内容并分析是否需要操作。
   - 如果需操作，向 `DEVOPS_ASSISTANT` 发送具体指令（如重启服务、回滚事务等）。

3. `DEVOPS_ASSISTANT`（`DA`）：
   - 执行指令并返回成功/失败结果（所有案例中均返回成功）。

4. **闭环验证**：
   - `INCIDENT_MANAGER` 确认问题已解决，最终标记为 "No action needed"。

## 关键协作特点

- **单向指令流**：`IM` 是决策者，`DA` 是执行者（无反向触发）。
- **原子性操作**：每个日志文件处理是独立的事务。
- **成功驱动**：仅当 `DA` 返回成功时，流程才结束（未体现错误处理分支）。

如果需要扩展（如错误处理或更复杂逻辑），可以进一步细化 `alt` 分支。

# 使用 Azure AI Foundry Agent Service 开发多智能体解决方案

https://learn.microsoft.com/en-us/training/modules/develop-multi-agent-azure-ai-foundry/

## 什么是互联代理，它们在 Azure AI Foundry Agent Service 中扮演什么角色？

互联代理是 Azure AI Foundry Agent Service 的一项功能，允许您将复杂任务分解为更小、更专业的角色。这种方法避免了构建自定义协调器或硬编码路由逻辑的需求。在一个互联代理系统中，有一个主代理负责解释用户输入并将任务委派给互联的子代理。每个子代理都被设计用于执行特定功能，例如总结文档、验证策略或从知识源检索数据。这种分工有助于简化复杂的工作流程、提高代理性能和准确性，并使系统更易于维护和扩展。

## 使用互联代理而非单一代理处理所有任务的主要优势是什么？

使用互联代理而非单一代理处理所有任务的主要优势在于其模块化、可维护性和可扩展性。单一代理在范围扩大时会变得难以管理，而互联代理则能提供：

- 简化复杂工作流程：通过将大任务分解为小任务，使系统更易于理解和管理。
- 提高性能和准确性：每个代理专注于特定功能，从而提高了其在该领域内的表现。
- 易于维护和扩展：模块化设计使得调试、添加或替换代理变得更加容易，而无需修改整个系统。
- 无需自定义协调：主代理使用自然语言路由任务，无需硬编码逻辑。
- 更高的可靠性和可追溯性：职责明确分离，使得问题更容易调试，因为代理可以单独测试。
- 灵活和可扩展：可以添加或替换代理，而无需重做整个系统或修改主代理。

## 主代理和互联代理在多代理解决方案中的具体职责是什么？

在多代理解决方案中，主代理和互联代理有明确的职责划分：

主代理职责：

- 充当协调器。
- 解释用户输入背后的意图。
- 确定哪个互联代理最适合处理请求。
- 选择适当的互联代理。
- 转发相关上下文和指令。
- 聚合或总结结果。
- 只有主代理的响应对最终用户可见。

互联代理职责：

- 专注于单一的责任领域。
- 根据明确的提示完成特定操作。
- （如果需要）使用工具完成其任务。
- 将结果返回给主代理。
- 设计时应具有单一职责，以提高系统调试、扩展和重用性。

## 如何在 Azure AI Foundry Agent Service 中设置一个互联代理解决方案？

在 Azure AI Foundry Agent Service 中设置一个互联代理解决方案涉及以下步骤：

1. 初始化代理客户端：首先创建一个连接到 Azure AI Foundry 项目的客户端。
1. 创建要连接到主代理的代理：使用 `AgentsClient` 对象的 `create_agent` 方法定义要连接到主代理的代理。为代理提供清晰的指令以定义其目的（例如，检索股票价格、总结文档或验证合规性）。
1. 初始化互联代理工具：使用代理定义创建一个 `ConnectedAgentTool`。为其分配名称和描述，以便主代理知道何时以及如何使用它。
1. 创建主代理：使用 `create_agent` 方法创建主代理。使用 `tools` 属性添加互联代理，并将 `ConnectedAgentTool` 定义分配给主代理。
1. 创建线程并发送消息：创建用于管理对话上下文的代理线程。然后在该线程上创建包含您希望代理完成的请求的消息。
1. 运行代理工作流：消息添加后，创建一个运行以处理请求。主代理使用其工具按需委派任务并为用户编译最终响应。
1. 处理结果：运行完成后，可以查看主代理的响应。最终输出可能包含来自一个或多个互联代理的见解。

## 互联代理如何帮助工程团队处理支持工单？

互联代理可以极大地帮助工程团队处理支持工单，通过自动化和简化工单分类及响应流程。例如，可以构建一个工单分流助手：

- 主代理：接收新的支持工单，解释其内容并确定任务。
- 互联子代理：
  - 分类代理：将工单分类为错误、功能请求或基础设施问题。
  - 优先级设置代理：根据预定义规则设置工单的优先级。
  - 团队建议代理：根据工单类型和优先级建议合适的团队来处理工作。

通过这种方式，这些专业代理可以协同工作，从而简化工单处理过程，加快团队响应速度，减少人工审查和排序的时间。

## 在构建互联代理系统时，设计互联代理的关键原则是什么？

在构建互联代理系统时，设计互联代理的关键原则是单一职责。每个互联代理都应被设计为专注于一个单一的职责领域。这使得您的系统更易于：

- 调试：由于代理是独立的，可以单独测试和定位问题。
- 扩展：可以轻松添加新功能，而无需修改现有代理。
- 重用：专业化的代理可以在不同的解决方案中重复使用。

清晰的提示和明确定义的目的是确保互联代理能够准确有效地完成其特定任务。

## 为什么在多代理系统中，主代理的响应是唯一对最终用户可见的？

在多代理系统中，主代理的响应是唯一对最终用户可见的，这主要有以下几个原因：

- 统一的用户体验：主代理作为与用户的唯一接口，负责整合所有子代理的输出，并以一致、连贯的方式呈现最终结果。这避免了用户需要理解或处理多个子代理可能返回的碎片化或技术性的中间结果。
- 上下文和意图管理：主代理负责解释用户的整体意图和管理对话上下文。它会确保所有子代理的工作都围绕这个核心意图展开，并将最终的响应格式化，使其直接回应用户的原始请求。
- 抽象复杂性：多代理系统的内部工作流程（例如任务委派、结果聚合）对用户来说是复杂的。主代理的作用是抽象这种复杂性，只向用户展示一个简洁、有用的最终答案，就好像是一个单一的智能实体在处理请求。
- 错误处理和容错：如果某个子代理出现问题或返回不完整的结果，主代理有责任处理这些情况，并尝试提供最佳的、用户友好的响应，而不是将原始错误信息直接暴露给用户。

## 互联代理解决方案的模块化方法如何促进可扩展性和灵活性？

互联代理解决方案的模块化方法极大地促进了可扩展性和灵活性，体现在以下几个方面：

- 更容易开发和调试：由于解决方案被分解为独立、专注的模块，开发人员可以更容易地构建和测试各个组件，从而加快开发周期并简化调试过程。
- 可重用的专业能力：每个互联代理都封装了特定的专业能力。这些代理可以作为独立组件在不同的解决方案中重复使用，避免了重复开发，提高了效率。
- 与业务逻辑对齐的扩展：您可以根据实际业务需求，按需添加或升级特定的代理，而无需重新设计或修改整个系统。这使得系统能够以与业务增长和变化相适应的方式进行扩展。
- 无需重写系统即可添加或替换代理：如果需要新的功能或更好的性能，可以轻松添加新的互联代理或替换现有代理，而无需修改主代理或其他互联代理的核心逻辑。这种松耦合的设计使得系统更具适应性。
- 简化复杂工作流程：通过将复杂任务分解为由不同代理处理的小块，系统变得更易于管理和理解，即使在规模增长时也能保持清晰的结构。

这种模块化设计为构建协作式 AI 解决方案提供了灵活的基础，使其能够随着需求的变化而扩展。

## 实验：Develop a multi-agent solution

https://github.com/cloudzun/mslearn-ai-agents/blob/main/Instructions/06-build-multi-agent-solution.md
