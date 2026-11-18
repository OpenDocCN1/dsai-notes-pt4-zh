
# 10 构建小而专注的 Agent

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/RWyXdf7lQoF6qPxrozFcvWsPnZ5](https://qqs7y1hozd1.feishu.cn/docx/RWyXdf7lQoF6qPxrozFcvWsPnZ5)

> 拆分功能域，将复杂系统拆分为多个职责明确的小 agent，降低复杂性、提升系统可维护性。

> 此要素主张，与其构建包揽一切的庞大 Agent，不如构建专注于特定任务、功能单一的 Agent。

> Agent 应视为更大、主要由确定性系统组成的系统中的构建块。此原则核心洞察在于 LLM 局限性：任务越庞大复杂，所需步骤越多，上下文窗口越长。上下文增长，LLM 越易失焦。

![](img/aba552dc3e8c7d2bf79421d90dcc1e1d.png)

将 Agent 关注点限制在特定领域，工作流控制在 3-10 步（最多 20 步），可保持上下文窗口可管理性，维持 LLM 高性能。

**此方法优势：**

> **可管理上下文：**更小上下文窗口意味着 LLM 性能更佳，因模型不必处理过多信息。

> **清晰职责：**每个 Agent 有明确范围和目的，避免职责不清。

> **更高可靠性：**复杂工作流中迷失可能性更小，提高 Agent 可靠性。

> **更易测试：**单一功能更易测试验证，简化开发维护。

> **改进调试：**问题发生时，更易识别修复，因问题范围限于小而专注的 Agent。

即便大语言模型（LLMs）变得更智能，当前的分阶段设计仍然必要。以下是关键分析：

## 技术演进与架构设计

### 渐进式扩展的必要性

- 当前 LLMs 处理长工作流时仍存在上下文丢失和推理偏差问题
- 模块化设计（如 DAG 分解）可降低系统复杂度，提升容错性
- 类比：重构大型代码库时需保持模块间松耦合

### 动态适应架构示例

```
# 混合模式控制器
def workflow_orchestrator(task_complexity):
    if llm_context_reliability &gt; 0.9:
        return execute_full_dag()  # 未来全自动模式
    else:
        return stepwise_validation()  # 当前分步验证模式
```

## 实施

### 企业级实施要点

### 向后兼容设计

- 通过抽象层隔离核心逻辑（如策略模式）
- 示例接口：

```
class WorkflowEngine:
    @abstractmethod
    def execute(self, dag): pass  # 统一执行入口
```

### 关键监控指标

- 上下文窗口利用率（阈值告警）
- 子工作流中断率（SLO < 0.1%）
- 人工干预频率（自动化成熟度指标）
