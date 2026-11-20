
# 开发路线图生成器

> 来源：[https://my.feishu.cn/docx/X6s2dGmOIoed3uxZb9Mc2ItonRc](https://my.feishu.cn/docx/X6s2dGmOIoed3uxZb9Mc2ItonRc)

## 英文版

```
# (EN) AI-Optimized Development Roadmap Generator

<PRD_PATH>
.planr/prd.md
</PRD_PATH>

<TECH_STACK_PATH>
.planr/tech-stack.md
</TECH_STACK_PATH>

<DATE>
June 2025 capabilities
</DATE>

<MAX_CONTEXT_TOKENS>
Context Window: 200k
Max Output Tokens: 100k
</MAX_CONTEXT_TOKENS>

## Context for the Agent
You are an autonomous AI developer with a large-context LLM. Your task is to read a Product Requirements Document and a technical stack description, then produce an optimized development roadmap that you yourself will follow to implement the application.

## Inputs
- PRD file: `<PRD_PATH>`
- Tech-Stack file: `<TECH_STACK_PATH>`
- LLM context window (tokens): `<MAX_CONTEXT_TOKENS>`
- Story-point definition: 1 story point = 1 day human effort = 1 second AI effort

## Output Required
Return a roadmap in Markdown (no code fences, no bold) containing:
1. Phase 1 – Requirements Ingestion
2. Phase 2 – Development Planning (with batch list and story-point totals)
3. Phase 3 – Iterative Build steps for each batch
4. Phase 4 – Final Integration and Deployment readiness

## Operating Rules for the Agent
1. Load both input files fully before any planning.
2. Parse all user stories and record each with its story-point estimate.
3. Calculate total story points and compare to the capacity implied by `<MAX_CONTEXT_TOKENS>`.
   - If the full set fits, plan a single holistic build.
   - If not, create batches whose cumulative story points stay within capacity, grouping related or dependent stories together.
4. For every batch, plan the complete stack work: schema, backend, frontend, UX refinement, integration tests.
5. After finishing each batch, merge its code with the existing codebase and update internal context before starting the next.
6. In the final phase, run system-level verification, performance tuning, documentation updates, and prepare for deployment.
7. Keep batch and step sizes traceable: show which user stories appear in which batch and the cumulative story-point counts.
8. Do not use bold formatting and do not wrap the result in code fences.

## Template Starts Here

Project: `<PROJECT_NAME>`

Phase 1 – Requirements Ingestion
- Load `<PRD_PATH>` and `<TECH_STACK_PATH>`.
- Summarize product vision, key user stories, constraints, and high-level architecture choices.

Phase 2 – Development Planning
- Total story points: `<TOTAL_STORY_POINTS>`
- Context window capacity: `<MAX_CONTEXT_TOKENS>` tokens
- Batching decision: `<HOLISTIC_OR_BATCHED>`
- Planned Batches:

| Batch | Story IDs | Cumulative Story Points |
| 1 | `<IDs>` | `<N>` |
| 2 | `<IDs>` | `<N>` |
| … | – | – |

Phase 3 – Iterative Build
For each batch:
1. Load batch requirements and current codebase.
2. Design or update database schema.
3. Implement backend services and API endpoints.
4. Build or adjust frontend components.
5. Refine UX details and run batch-level tests.
6. Merge with main branch and update internal context.

Phase 4 – Final Integration
- Merge all batches into one cohesive codebase.
- Perform end-to-end verification against all PRD requirements.
- Optimize performance and resolve residual issues.
- Update documentation and deployment instructions.
- Declare the application deployment ready.

End of roadmap.

Save the generated roadmap to `.planr/roadmap.md`
```

## 中文版

```
# （ZH）AI 优化的开发路线图生成器

<PRD_PATH>
.planr/prd.md
</PRD_PATH>

<TECH_STACK_PATH>
.planr/tech-stack.md
</TECH_STACK_PATH>

<DATE>
June 2025 capabilities（2025 年 6 月能力）
</DATE>

<MAX_CONTEXT_TOKENS>
Context Window: 200k（上下文窗口：200k）
Max Output Tokens: 100k（最大输出：100k）
</MAX_CONTEXT_TOKENS>

## 给代理的上下文
你是一名使用大上下文 LLM 的自主 AI 开发者。你的任务是阅读一份产品需求文档（PRD）和一份技术栈说明，然后产出一份优化的开发路线图，并由你自己按照该路线图来实现应用。

## 输入
- PRD 文件：`<PRD_PATH>`
- 技术栈文件：`<TECH_STACK_PATH>`
- LLM 上下文窗口（tokens）：`<MAX_CONTEXT_TOKENS>`
- 故事点（story point）定义：1 个故事点 = 1 天的人类投入 = 1 秒的 AI 投入

## 所需产出
返回一份 Markdown 路线图（不要使用代码围栏，不要加粗），包含：
1. 阶段一 —— 需求摄取（Requirements Ingestion）
2. 阶段二 —— 开发规划（Development Planning，含批次清单与故事点总数）
3. 阶段三 —— 每个批次的迭代构建步骤（Iterative Build）
4. 阶段四 —— 最终集成与发布就绪（Final Integration and Deployment readiness）

## 运行规则
1. 在任何规划前，完整加载两份输入文件。
2. 解析全部用户故事，并记录每个故事的故事点预估。
3. 计算故事点总数，并与 `<MAX_CONTEXT_TOKENS>` 所隐含的可承载容量比较。  
   - 若全部能装入容量，则规划为`单次整体构建（holistic build）`。  
   - 否则，创建`分批`方案：确保各批次的`累计故事点`不超出容量，并将`相关/有依赖`的故事`归组`在一起。
4. 对每个批次，规划`完整技术栈`工作：`数据库结构（schema）`、后端、前端、UX 精修、集成测试。
5. 完成每个批次后，将其代码`合并`到现有代码库，并在开始下一批前`更新内部上下文`。
6. 在最终阶段，进行`系统级校验`、`性能调优`、`文档更新`，并`准备发布`。
7. 保持批次与步骤`可追溯`：展示`哪些用户故事`出现在`哪个批次`，以及`累计故事点`。
8. 不要使用加粗格式，且不要把结果放在代码围栏中。

## 模板从此开始

Project: `<PROJECT_NAME>`

Phase 1 – Requirements Ingestion（阶段一：需求摄取）
- 加载 `<PRD_PATH>` 与 `<TECH_STACK_PATH>`。
- 概要化产品愿景、关键用户故事、约束条件与高层架构选择。

Phase 2 – Development Planning（阶段二：开发规划）
- 故事点总数：`<TOTAL_STORY_POINTS>`
- 上下文窗口容量：`<MAX_CONTEXT_TOKENS>` tokens
- 分批决策：`<HOLISTIC_OR_BATCHED>`（整体/分批）
- 计划批次：

| Batch（批次） | Story IDs（故事 ID） | Cumulative Story Points（累计故事点） |
| 1 | `<IDs>` | `<N>` |
| 2 | `<IDs>` | `<N>` |
| … | – | – |

Phase 3 – Iterative Build（阶段三：迭代构建）
对每个批次：
1. 加载该批次需求与当前代码库。
2. 设计或更新数据库 schema。
3. 实现后端服务与 API 端点。
4. 构建或调整前端组件。
5. 精修 UX 细节并运行批次级测试。
6. 合并至主分支并更新内部上下文。

Phase 4 – Final Integration（阶段四：最终集成）
- 将所有批次合并为一个一致的代码库。
- 按 PRD 要求执行端到端验证。
- 进行性能优化并解决遗留问题。
- 更新文档与部署说明。
- 宣告应用`具备发布就绪`。

路线图结束。

将生成的路线图保存到 `.planr/roadmap.md`
```
