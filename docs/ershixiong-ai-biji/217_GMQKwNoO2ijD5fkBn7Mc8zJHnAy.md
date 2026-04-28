# 如何通过修改 prompt（提示词）来调整 NLWeb 系统的行为和输出。

> 来源：[https://qianniucity.feishu.cn/docx/B6Zod9qoJoPorhxtwnIcyFChnTd](https://qianniucity.feishu.cn/docx/B6Zod9qoJoPorhxtwnIcyFChnTd)

主要内容总结如下：

+   **Prompt 的作用**：
NLWeb 在处理用户查询时，会针对不同任务（如相关性分析、去上下文化、记忆检测、排序、结果摘要等）多次调用 LLM，每次调用都依赖于特定的 prompt。
这些 prompt 都定义在 site_types.xml 文件中，系统行为可以通过修改这些 prompt 灵活调整。

+   **Prompt 结构与定制**：

+   每个 prompt 由唯一的 ref 属性标识，内容为模板字符串，包含如 {request.query}、{site.itemType} 等变量，实际调用时动态填充。

+   Prompt 通常先建立上下文，再给出具体分析或处理指令，并要求 LLM 以结构化格式输出结果。

+   可以为不同类型的内容（如 Recipe、Movie）设计更具体的 prompt，以提升效果。

+   **示例**：

+   记忆检测 prompt：判断用户是否明确要求记住某事，并提取记忆内容。

+   排序 prompt：让 LLM 根据相关性和其他属性（如评分、营养信息）为条目打分并生成简要描述。

+   可以通过修改 prompt，将如星级评分、营养信息等因素纳入排序和描述。

+   **变量说明**：
支持多种变量（如 request.site、site.itemType、request.rawQuery、request.previousQueries 等），便于 prompt 灵活适配不同场景。

整体上，NLWeb 通过可配置的 prompt 机制，使开发者能够细致控制 LLM 的行为，实现高度定制化的对话和检索体验。