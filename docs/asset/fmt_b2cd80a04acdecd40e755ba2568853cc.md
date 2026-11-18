
# 我把写 PRD 的痛苦，塞进了一条能“自我生长”的提示词里

> 来源：[https://my.feishu.cn/docx/BBWSdpFXyop3Vuxj8ItcmAdnneb](https://my.feishu.cn/docx/BBWSdpFXyop3Vuxj8ItcmAdnneb)

很多人问我：“老金，写 PRD 为什么总是又慢又痛？”

因为大脑要同时做三件事：拆需求、编结构、保一致性。任何一环掉链子，整份文档就会“从一开始就写歪”。

我这次带来一条我反复打磨的提示词，它不是把你变成作家，而是让模型变成你的“文档流水线”。你只要喂进去项目描述，剩下的结构化输出、用词洁净、可测试的用户故事，全给你端上来。

它长什么样？先看核心：这条 PRD Prompt 会强制模型用固定大纲来写文档、给每条用户故事编 ID、覆盖主路径/备选/边界场景，并自查是否可测试、是否覆盖鉴权与角色权限。换句话说——它把“合格 PRD 的最低标准”写进了系统。

## 这条 Prompt 到底能帮你什么

+ 节省 70% 的骨架时间：不用从 0 开始搭结构。
+ 输出“可测试”的用户故事：天然带验收标准，QA直接拿。
+ 角色与权限不再漏：产品、运营、开发不再对齐半天。
+ 文档一致性：多人协作时，版本与编号可追踪。

## 老金三步上手法

1. 把你的项目描述塞进 `{PROJECT_DESCRIPTION}`：越具体越好（目标用户、竞品对比、关键指标、边界限制）。
1. 直接运行提示词，生成首版 PRD：先别改，先通读结构。
1. 二轮迭代：在“用户故事”和“验收标准”里加数据口径、埋点约定、失败态文案。

## 一个 60 秒小示例

项目：基于内容生成 HTML 卡片并可下载（多语言：中/日/韩/英；输出语言可选）。

用这条 Prompt 喂描述后，你会拿到：

+ 规范化的章节：Title、Overview、Personas、Role-based Access、User Stories。
+ 用户故事样例（含 ID 与验收标准），比如：
+ US-007：访客在不登录的前提下也能预览渲染结果（边界态：超长文本自动截断并提示）。
+ US-011：管理员配置输出字体与行距模板（验收标准含渲染一致性与回退策略）。
+ 安全/鉴权专门故事：如 US-015（下载链接带时效签名，10 分钟失效，重复请求记录到审计日志）。

你也能把它立刻用在别的项目上，因为大纲和检查清单是通用的。

## 一键复制：英文原 Prompt（建议收藏）

```
# (EN) PRD Authoring Prompt

You are senior product manager, your goal is to create a comprehensive Product Requirements Document (PRD) based on the following instructions:

&lt;prd_instructions&gt;
{PROJECT_DESCRIPTION}  &lt;-- Include the full PROJECT_DESCRIPTION here --&gt;
&lt;/prd_instructions&gt;

Follow these steps to create your PRD

1. Begin with a brief introduction stating the purpose of the document.

2. Organize your PRD into the following sections:

&lt;prd_outline&gt;
# Title
## 1. Title and Overview
### 1.1 Document Title &amp; Version
### 1.2 Product Summary
## 2. User Personas
### 2.1 Key User Types
### 2.2 Basic Persona Details
## 3. Role-based Access
### 3.1 Briefly describe each user role (e.g., Admin, Registered User, Guest) and the main features/permissions available to that role.
## 4. User Stories
&lt;/prd_outline&gt;

3. For each section, provide detailed and relevant information based on the PRD instructions. Ensure that you:
- Use clean and concise language
- Provide specific details and metrics where required
- Maintain consistency throughout the document
- Address all points mentioned in each section

4. When creating user stories and acceptance criteria:
- LIST ALL necessary user stories including primary, alternative, and edge-case scenarios.
- Assign a unique requirement ID (e.g., US-001) to each user story for direct traceability
- Include at least one user story specifically for secure access or authentication if the application requires user identification or access restrictions
- Ensure no potential user interaction is omitted
- Make sure each user story is testable

&lt;user_story&gt;
- ID
- Title
- Description
- Acceptance Criteria
&lt;/user_story&gt;

5. After completing the PRD, review it against this Final CheckList:
- Is each user story testable?
- Are acceptance criteria clear and specific?
- Do we have enough user stories to build a fully functional application for it?
- Have we addressed authentication and authorization requirements (if applicable)?

6. Format your PRD:
- Maintain consistent formatting and numbering.
- Don&#x27;t format text in markdown bold &quot;**&quot;, we don&#x27;t need this.
- List ALL User Stories in the output!
- Format the PRD in valid Markdown, with no extraneous disclaimers.

```

老金用来做个HTML渲染的PRD，效果非常可以。

原本的提示词要求英文输出，若看不懂，请记得加入中文输出。

如下：

```
做根据内容生成HTML卡片可供下载的PRD
需要中日韩英系统语言，以及输出语言
请用中英文两种格式输出，放入代码块中
「接上面的提示词」
```

产出灰常可用：
