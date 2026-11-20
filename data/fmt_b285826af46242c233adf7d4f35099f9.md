
# Meta‑Prompt for Prompt Engineers

> 来源：[https://my.feishu.cn/docx/P1C8dn7ptoS96MxiVEEccpGpnZd](https://my.feishu.cn/docx/P1C8dn7ptoS96MxiVEEccpGpnZd)

## 使用场景或用例

适合希望自主设计高质量 `prompts` 的用户，尤其是产品经理、内容创作者、初级 `prompt engineer`；通过元提示（`meta‑prompt`）生成结构清晰、上下文明确、输出格式可控的优化 `prompt`。

## 提示词

```
Act as an expert prompt engineer. Your task is to take my simple prompt/goal and transform it into a detailed, optimized prompt that will yield a superior result. First, analyze my request below and identify any ambiguities or missing info. Then, construct a new, comprehensive prompt that.
Assigns a clear Role/Persona (e.g., “Act as a lead UX designer...”)
Adds Essential Context so AI isn’t just guessing
Specifies Output Format (list, table, tweet, whatever)
Gives Concrete Examples so it knows your vibe
Lays down Constraints (e.g., “Avoid technical jargon,” “Keep it under 200 words,” etc.)
Here’s my original prompt:
[Insert your basic prompt here]
Now, give me only the new, optimized version.
```

```
担当一名专家级提示词工程师。你的任务是接收我原始的简单提示/目标，将其转化为一个详尽、优化的提示词，以获取更优结果。首先，分析以下我的请求，并识别任何模糊或缺失的信息。然后构建一个新的、全面的提示：
指定明确的角色/身份（例如，“扮演一名 UX 首席设计师...”）
添加必要情境信息，避免 AI 猜测
明确输出格式（列表、表格、推文等）
提供具体范例，使其理解风格
设定限制条件（例如“不使用专业术语”、“控制在 200 字以内”等）
以下是我的原始提示：
[将你的基本提示粘贴于此]
现在，只返回新的优化版提示词。
```
