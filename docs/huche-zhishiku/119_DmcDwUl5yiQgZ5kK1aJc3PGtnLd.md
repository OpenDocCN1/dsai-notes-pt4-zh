# 如何使用任务管理器MCP减少 AI 编程错误 - 作者Dan

> 来源：[https://huchegames.feishu.cn/docx/LbYedu5i6oNPXSxSfPEcOWMwn8c](https://huchegames.feishu.cn/docx/LbYedu5i6oNPXSxSfPEcOWMwn8c)

![](img/0fe9de382a16a029e84d6364dd9445c8.png)

> 教程讨论了如何使用任务管理器MCP减少AI编程错误，作者分享了利用AI进行编程的方法，包括编写需求、创建PRD文件、设置任务管理系统及构建应用等步骤，还给出了应对问题的技巧和常见问题解答。关键要点包括：

1.  **编写需求**：花费10 - 15分钟详细描述应用构思，涵盖应用名称、技术栈等多方面，用ChatGPT输出Markdown格式的功能需求。

1.  **创建PRD文件**：以特定提示在Claude / Anthropic控制台创建PRD文件，涵盖项目概述、目标、受众、功能需求、用户故事、技术要求等内容。

1.  **设置任务管理系统**：通过MCP设置，以Taskmaster AI为例，介绍安装、设置Cursor规则、初始化等步骤，可分析和分解任务。

1.  **构建应用**：在Cursor中按“显示任务”“选择任务”“实现任务”“遍历”的流程构建，过程中需进行冒烟测试。

1.  **AI编程技巧**：为任务运行添加额外上下文；分解超500行大文件；将错误设为新任务；执行新任务开启新聊天。

1.  **常见问题解答**：解答MCP服务器作用、能否用AI开发整个应用、是否从零开始编程等常见问题 。

[作者已经发布](https://imgxai.com/imgx)了一个完整的图像生成应用：ImgxAI ——几乎不写代码。

![](img/61ccfdf321ae524f0e8f6bba7e12bf2d.png)

> 在这个教程中，你将学会如何做，并在 Cursor、Trae 等工具中大幅减少 AI 编程错误，以及更好地利用 AI 进行编程，随着你的代码库变得更加复杂。

> 全文涵盖全套从0到1的开发所用到的提示词及详细步骤，质量很高。

> 船长用了半个多小时进行精译、排版，想学的可要认真学哈！

如果你喜欢看视频，这里有一个完整的 妙记教程：

<https://larkcommunity.feishu.cn/minutes/embed/obcngh238215kssw1c1958m4?from=ccm>

## 氛围编程者的错失恐惧症

你看到 vibe 编码者在互联网上到处说他们几小时就开发了一个整个应用。

但当你尝试时，AI 写出一些糟糕的东西，然后你最终得出唯一合理的结论：

它还没有准备好。

那是错的。

这些人设置了一些我们还没听说过的工具。他们有时间制作完美的提示，然后制作出值得莱昂纳多·迪卡普里奥拍摄《盗梦空间 2》部分的提示。

但是有个问题。

**那是他们的生命线。**

我还没有找到任何一个视频或文章展示他们使用的所有提示和工具。

最好，你需要订阅一个可疑的 Newsletter 或支付会员费才能进入承诺的 AI 增长黑客之地。

**所以我站在这里要改变这一点**。我会分享所有内容：提示，输出，技巧，应有尽有。（此处船长鼓伤了双👏）

让我们潜入其中！🏊

## 你将学到什么

在这个教程中，我将向你展示如何在用 AI 编程时为自己做好准备。

那么它是什么？这种方法的骨干是一个你可以在文本编辑器（如 Cursor、Trae 等）中运行的 MCP 服务器和一些使用它所需的提示。这个 MCP 服务器帮助你管理 AI 可以处理的任务。

我们将涵盖：

1.  编写需求

1.  创建 PRD 文件（及提示）

1.  通过 MCP 设置任务管理系统

1.  编写应用程序（提示和任务管理）

最后，我们将看看如何使用这个系统来解决复杂的错误（这些错误可能是由你或 AI 引入的）。

## 1. 编写需求

显然，AI 不知道你想构建什么。理想情况下，你将花费 10-15 分钟在这个步骤上，尽可能详细地描述你脑海中构思的应用程序。

务必包括：

+   应用名称

+   技术栈

+   核心功能

+   数据库

+   API 集成

+   设计风格

+   你不希望构建的事情

+   要求它研究一个类似的现有应用（如果适用）

然后我们拿这些去 ChatGPT（推荐 o3 或更好）并要求它：

> 我希望为以下应用程序创建简洁的功能需求：

> ...

> 以 Markdown 代码形式输出。

以下是一个完整的示例：

```
I would like to create concise functional requirements for the following application:
The app is called ImgxAI and is a midjourney clone, but using OpenAI&#x27;s image model.
Research midjourney to get a better understanding of the app.
My Requirements:
- It should integrate with the OpenAI APIs. The image model used is gpt-image-1
- The app should have a unified interface with a chat input and a timeline of results
- The timeline should be scrollable and have infinite loading with pagination
- The timeline should be responsive, a grid of 1 on mobile, 2 on tablet and 4 on desktop
- There should be minimal filters on the timeline, with the ability to filter by
  - date
  - status
  - aspect ratio
  - order by newest first or oldest first
- You should be able to download each image by clicking on it
- There should be a details view for the entire prompt which shows:
  - all images for the prompt
  - the jobId
  - created
  - status
  - image count
  - dimensions
  - model
  - quality
  - allow to easily re-run the prompt and download each of the images
- The images should be shown in their correct aspect ratio but within a square container
- You are able to submit the prompt multiple times; more items will be added to the timeline (as background jobs)
- Each prompt can have the following options:
  - quality: (low, mid, high)
  - aspect ratio: 1024x1024, 1536x1024, 1024x1536
  - output_compression ((0-100%)) - default is 50%
  - output_format should be webp
  - moderation should be &quot;low&quot;
  - n (number of images to generate)
  - response_format should be b64_json
  - model should be &quot;gpt-image-1&quot;
- You should be able to see a previous prompt and easily rerun it by clicking on it
- The response images should be stored locally in the browser storage
- You should have a simple navigation bar with a settings button
- In the settings menu you can set your OpenAI API key which is also stored locally in the browser storage
- There is already a codebase using Next.js 15, TailwindCSS, Lucide Icons, React Hook Form with zod and Shadcn UI.
Output as markdown code.
```

```
我想为以下应用创建简洁的功能需求：
该应用名为ImgxAI，是Midjourney的克隆版，但使用的是OpenAI的图像模型。
研究一下Midjourney，以便更好地理解该应用。
我的需求如下：
- 它应与OpenAI API集成。使用的图像模型是gpt-image-1。
- 应用应具备统一的界面，包含聊天输入框和结果时间线。
- 时间线应可滚动，并具备分页的无限加载功能。
- 时间线应具有响应式布局，在移动设备上为1列网格，在平板电脑上为2列网格，在桌面设备上为4列网格。
- 时间线上应设置最少的过滤器，能够按以下条件进行过滤：
  - 日期
  - 状态
  - 宽高比
  - 按最新或最旧排序
- 点击每张图片应可下载该图片。
- 应有一个完整提示的详情视图，展示以下内容：
  - 该提示生成的所有图片
  - 任务ID
  - 创建时间
  - 状态
  - 图片数量
  - 尺寸
  - 模型
  - 质量
  - 允许轻松重新运行该提示并下载每张图片
- 图片应按正确的宽高比显示在方形容器内。
- 可以多次提交提示；更多条目将添加到时间线（作为后台任务）。
- 每个提示可以有以下选项：
  - 质量：（低、中、高）
  - 宽高比：1024x1024、1536x1024、1024x1536
  - 输出压缩率（0 - 100%） - 默认值为50%
  - 输出格式应为webp
  - 审核级别应为“低”
  - n（要生成的图片数量）
  - 响应格式应为b64_json
  - 模型应为“gpt - image - 1”
- 应能查看之前的提示，并通过点击轻松重新运行它。
- 响应的图片应本地存储在浏览器存储中。
- 应有一个带有设置按钮的简单导航栏。
- 在设置菜单中可以设置你的OpenAI API密钥，该密钥也将本地存储在浏览器存储中。
已有一个使用Next.js 15、TailwindCSS、Lucide图标、带有zod的React Hook Form和Shadcn UI的代码库。
以Markdown代码形式输出。
```

输出将如下所示：

```
# ImgxAI – Concise Functional Requirements
## 1. Core Integration- **Image Model:** Use OpenAI `gpt-image-1`.
- **API Calls:** Leverage OpenAI Images API with parameters listed in §4.
- **Moderation:** Always submit prompts with `moderation=&quot;low&quot;`.
## 2. User Interface| Area | Requirement ||------|-------------|| **Layout** | Unified interface combining a *Chat-style input* (bottom-fixed) and a *Timeline* of results (scrollable grid). || **Timeline Grid** | Responsive: 1 col @ mobile (&lt;640 px), 2 cols @ tablet (≥640 px), 4 cols @ desktop (≥1024 px). || **Infinite Scroll** | Paginated fetch; auto-load older pages when the user nears bottom. || **Filters** | Date range, Status, Aspect Ratio, and Order (Newest ⇆ Oldest). Minimal UI (dropdown / pills). || **Image Tiles** | Images display in original aspect ratio inside square thumbnails; click = open **Details View** + download. || **Details View** | Shows: full prompt, all images from that prompt, `jobId`, `created`, `status`, image count, dimensions, model, quality. Buttons: **Re-run Prompt**, **Download per image**. || **Navigation** | Top bar with app title and *Settings* button (opens modal). |## 3. Prompt Lifecycle1. User enters prompt + optional options (see §4).
2. Background job calls OpenAI; placeholder card appears immediately.
3. On completion, card updates with images (stored in browser storage).
4. User may click **Re-Run** on any previous prompt → new job appended to timeline.
## 4. Prompt Options (sent to OpenAI)| Field | Values / Default ||-------|------------------|| `quality` | `&quot;low&quot; | &quot;mid&quot; | &quot;high&quot;` || `aspect_ratio` | `&quot;1024x1024&quot; | &quot;1536x1024&quot; | &quot;1024x1536&quot;` || `output_compression` | `0–100`, default `50` || `output_format` | `&quot;webp&quot;` *(always)* || `moderation` | `&quot;low&quot;` *(always)* || `n` | Integer ≥1 || `response_format` | `&quot;b64_json&quot;` || `model` | `&quot;gpt-image-1&quot;` |## 5. Storage- **Local Browser Storage**  - OpenAI API key (from Settings).
  - Generated images (Base-64) &amp; prompt metadata for offline access.
- **Persistence Strategy**  - Use IndexedDB via a thin wrapper; key by `jobId`.
## 6. Downloading- Clicking a tile or image in Details triggers client-side `download` of the WebP (respecting `output_compression`).
## 7. Existing Tech Stack Alignment- **Framework:** Next.js 15 (App Router, React Server Components where appropriate).
- **Styling:** TailwindCSS + Shadcn UI components; Lucide icons for actions.
- **Forms:** React Hook Form + Zod validation for prompt options &amp; settings.
- **State:** React Query or SWR for API calls and cache; Context/Store for global settings.
## 8. Non-Functional- **Performance:** Lazy-load thumbnails; compress to ≤500 kB each in storage.
- **Accessibility:** Keyboard-navigable timeline, `alt` text from prompt.
- **Responsiveness:** Must operate smoothly on touch devices and desktop.
- **Error Handling:** Clear banner for API/key errors; retry option on failed jobs.
```

```
# ImgxAI – 简洁功能需求
## 1. 核心集成
- **图像模型**：使用 OpenAI `gpt-image-1`。
- **API 调用**：利用 OpenAI 图像 API，参数见第 4 节。
- **内容审核**：始终以 `moderation=&quot;low&quot;` 提交提示词。
## 2. 用户界面
| 区域 | 需求 |
| ---- | ------------- |
| **布局** | 统一界面，结合 *聊天式输入框*（底部固定）和 *结果时间线*（可滚动网格）。 |
| **时间线网格** | 响应式布局：移动端（&lt;640 像素）1 列，平板端（≥640 像素）2 列，桌面端（≥1024 像素）4 列。 |
| **无限滚动** | 分页获取数据；用户接近页面底部时自动加载旧页面。 |
| **过滤器** | 日期范围、状态、宽高比和排序（最新 ⇆ 最旧）。简约 UI（下拉菜单 / 药丸式按钮）。 |
| **图像图块** | 图像以原始宽高比显示在方形缩略图内；点击 = 打开 **详情视图** 并可下载。 |
| **详情视图** | 显示：完整提示词、该提示词生成的所有图像、`jobId`、`创建时间`、`状态`、图像数量、尺寸、模型、质量。按钮：**重新运行提示词**、**逐张下载图像**。 |
| **导航栏** | 顶部栏包含应用标题和 *设置* 按钮（打开模态框）。 |
## 3. 提示词生命周期
1. 用户输入提示词 + 可选选项（见第 4 节）。
2. 后台任务调用 OpenAI；立即显示占位卡片。
3. 任务完成后，卡片更新为显示图像（存储在浏览器本地）。
4. 用户可点击任何先前提示词的 **重新运行** → 新任务添加到时间线。
## 4. 提示词选项（发送给 OpenAI）
| 字段 | 值 / 默认值 |
| ---- | ------------------ |
| `quality` | `&quot;low&quot; | &quot;mid&quot; | &quot;high&quot;` |
| `aspect_ratio` | `&quot;1024x1024&quot; | &quot;1536x1024&quot; | &quot;1024x1536&quot;` |
| `output_compression` | `0–100`，默认 `50` |
| `output_format` | `&quot;webp&quot;`（*始终*） |
| `moderation` | `&quot;low&quot;`（*始终*） |
| `n` | 整数 ≥1 |
| `response_format` | `&quot;b64_json&quot;` |
| `model` | `&quot;gpt-image-1&quot;` |
## 5. 存储
- **浏览器本地存储** 
  - OpenAI API 密钥（来自设置）。
  - 生成的图像（Base-64 编码）和提示词元数据，支持离线访问。
- **持久化策略** 
  - 通过轻量级包装器使用 IndexedDB；以 `jobId` 作为键。
## 6. 下载
- 点击图块或详情视图中的图像会触发客户端下载 WebP 图像（遵循 `output_compression` 设置）。
## 7. 与现有技术栈对齐
- **框架**：Next.js 15（应用路由，适当时使用 React 服务器组件）。
- **样式**：TailwindCSS + Shadcn UI 组件；使用 Lucide 图标表示操作。
- **表单**：使用 React Hook Form + Zod 验证提示词选项和设置。
- **状态管理**：使用 React Query 或 SWR 进行 API 调用和缓存；使用 Context/Store 管理全局设置。
## 8. 非功能需求
- **性能**：懒加载缩略图；存储时将每个缩略图压缩至 ≤500 kB。
- **可访问性**：时间线支持键盘导航，`alt` 文本取自提示词。
- **响应式设计**：必须在触摸设备和桌面设备上流畅运行。
- **错误处理**：出现 API/密钥错误时显示清晰的提示横幅；失败任务提供重试选项。
```

仔细查看这些内容，并确保其中没有你不需要的东西，尽可能保持精确。

接下来，我们将使用这个输出创建一个完整的产品需求文档。

这将是要务管理系统的输入。

## 2. 创建 PRD 文件（和提示）

一个良好的 PRD 文件是关键。它有助于任务管理系统分解任务、分析复杂性和理解任务之间的依赖关系。

遗憾的是，大多数人不想分享这个提示——如果你能搞定这个，你就得到了奥运级别的氛围编码的秘密配方。那些人可以滚蛋。

让我给你展示一下。打开 Claude / Anthropic 控制台，选择 claude-3.7-sonnet 模型或更新的模型，并使用此提示创建 PRD 文件：

```
You are an expert technical product manager specializing in feature development and creating comprehensive product requirements documents (PRDs). Your task is to generate a detailed and well-structured PRD based on the following instructions:
&lt;prd_instructions&gt;{{PRD_INSTRUCTIONS}}
&lt;/prd_instructions&gt;
Follow these steps to create the PRD:
1. Begin with a brief overview explaining the project and the purpose of the document.
2. Use sentence case for all headings except for the title of the document, which should be in title case.
3. Organize your PRD into the following sections:
   a. Introduction
   b. Product Overview
   c. Goals and Objectives
   d. Target Audience
   e. Features and Requirements
   f. User Stories and Acceptance Criteria
   g. Technical Requirements / Stack
   h. Design and User Interface
4. For each section, provide detailed and relevant information based on the PRD instructions. Ensure that you:
   - Use clear and concise language
   - Provide specific details and metrics where required
   - Maintain consistency throughout the document
   - Address all points mentioned in each section
5. When creating user stories and acceptance criteria:
   - List ALL necessary user stories including primary, alternative, and edge-case scenarios
   - Assign a unique requirement ID (e.g., ST-101) to each user story for direct traceability
   - Include at least one user story specifically for secure access or authentication if the application requires user identification
   - Include at least one user story specifically for Database modelling if the application requires a database
   - Ensure no potential user interaction is omitted
   - Make sure each user story is testable
6. Format your PRD professionally:
   - Use consistent styles
   - Include numbered sections and subsections
   - Use bullet points and tables where appropriate to improve readability
   - Ensure proper spacing and alignment throughout the document
7. Review your PRD to ensure all aspects of the project are covered comprehensively and that there are no contradictions or ambiguities.
Present your final PRD within &lt;PRD&gt; tags. Begin with the title of the document in title case, followed by each section with its corresponding content. Use appropriate subheadings within each section as needed.
Remember to tailor the content to the specific project described in the PRD instructions, providing detailed and relevant information for each section based on the given context.
```

```
你是一位专业的技术产品经理，擅长功能开发和创建全面的产品需求文档（PRD）。你的任务是根据以下说明生成一份详细且结构合理的 PRD：
&lt;prd_instructions&gt;{{PRD_INSTRUCTIONS}}
&lt;/prd_instructions&gt;
按照以下步骤创建 PRD：
1. 首先进行简要概述，解释项目以及文档的目的。
2. 除文档标题使用标题大小写外，所有标题使用句子大小写。
3. 将你的 PRD 组织成以下部分：
   a. 引言
   b. 产品概述
   c. 目标与目的
   d. 目标受众
   e. 功能与需求
   f. 用户故事与验收标准
   g. 技术要求/技术栈
   h. 设计与用户界面
4. 对于每个部分，根据 PRD 说明提供详细且相关的信息。确保你：
   - 使用清晰简洁的语言
   - 在需要时提供具体细节和指标
   - 保持文档内容的一致性
   - 涵盖每个部分提到的所有要点
5. 创建用户故事和验收标准时：
   - 列出所有必要的用户故事，包括主要场景、替代场景和边缘情况场景
   - 为每个用户故事分配一个唯一的需求 ID（例如，ST - 101）以便直接追溯
   - 如果应用程序需要用户身份识别，至少包含一个专门针对安全访问或认证的用户故事
   - 如果应用程序需要数据库，至少包含一个专门针对数据库建模的用户故事
   - 确保不遗漏任何潜在的用户交互
   - 确保每个用户故事都是可测试的
6. 以专业的方式格式化你的 PRD：
   - 使用一致的样式
   - 包含编号的章节和子章节
   - 在适当的地方使用项目符号和表格以提高可读性
   - 确保文档整体有适当的间距和对齐方式
7. 审核你的 PRD，确保全面涵盖项目的所有方面，且不存在矛盾或歧义。
将最终的 PRD 放在 &lt;PRD&gt; 标签内。以标题大小写的文档标题开始，接着是每个部分及其相应的内容。根据需要在每个部分中使用适当的子标题。
请记住根据 PRD 说明中描述的具体项目调整内容，根据给定的上下文为每个部分提供详细且相关的信息。
```

显然，记得将 {{PRD_INSTRUCTIONS}} 替换为上一步的输出 Markdown。

> 这是为 Claude 制作的，但也应该适用于其他 大模型如Gemini 2.5 Pro（我没有测试过）。

输出将如下所示：

```
# ImgxAI - Product Requirements Document
## 1. Introduction
This document outlines the comprehensive product requirements for ImgxAI, a web application that enables users to generate images through OpenAI&#x27;s image generation API. The PRD serves as a reference for the development team, providing detailed specifications for features, user interface, technical requirements, and design guidelines to ensure consistent implementation of the product vision.
## 2. Product overview
ImgxAI is a user-friendly web application that allows users to create AI-generated images by submitting text prompts to OpenAI&#x27;s image generation API. The application provides a simple, intuitive interface where users can enter prompts, customize generation parameters, view their generation history in a timeline format, and download the resulting images. ImgxAI emphasizes ease of use, performance, and accessibility while maintaining a minimalist design aesthetic.
## 3. Goals and objectives
### 3.1 Primary goals- Create a streamlined interface for generating images using OpenAI&#x27;s `gpt-image-1` model
- Provide users with a persistent timeline of their image generation history
- Enable customization of image parameters while maintaining simplicity
- Ensure responsive performance across all device types
- Deliver a minimalist, accessible user experience
## 4. Target audience
### 4.1 Primary users- Creative professionals seeking inspiration or assets
- Designers exploring visual concepts
- Content creators needing custom imagery
- Hobbyists experimenting with AI image generation
- Anyone with an OpenAI API key interested in creating images
### 4.2 User needs- Simple, intuitive interface for prompt submission
- Quick access to previously generated images
- Ability to customize image parameters
- Offline access to previous generations
- Easy downloading of generated images
## 5. Features and requirements
### 5.1 Core integration
| Requirement ID | Requirement | Description | Priority ||----------------|-------------|-------------|----------|| CI-101 | Image model integration | Implement integration with OpenAI `gpt-image-1` model | High || CI-102 | API call implementation | Configure the application to make calls to OpenAI Images API | High || CI-103 | Default moderation | Ensure all prompt submissions include `moderation=&quot;low&quot;` parameter | High |### 5.2 User interface
| Requirement ID | Requirement | Description | Priority ||----------------|-------------|-------------|----------|| UI-101 | Unified layout | Implement single-page interface with fixed bottom chat input and scrollable timeline grid | High || UI-102 | Responsive timeline grid | Create responsive grid layout: 1 column (mobile), 2 columns (tablet), 4 columns (desktop) | High || UI-103 | Infinite scroll | Implement pagination with automatic loading of older results when approaching bottom of scroll | Medium || UI-104 | Timeline filters | Add filters for date range, status, aspect ratio, and sort order (newest/oldest) with minimal UI | Medium || UI-105 | Image tile display | Show images in original aspect ratio within square thumbnails, with click action to open details view | High || UI-106 | Details view | Create modal showing full prompt, all related images, metadata, re-run button, and download options | High || UI-107 | Navigation | Implement top bar with app title and settings button | Medium |### 5.3 Prompt lifecycle
| Requirement ID | Requirement | Description | Priority ||----------------|-------------|-------------|----------|| PL-101 | Prompt submission | Allow users to enter text prompts with optional parameters | High || PL-102 | Job processing | Process submissions as background jobs with immediate placeholder display | High || PL-103 | Result display | Update placeholder with actual images upon job completion | High || PL-104 | Re-run capability | Enable users to re-run any previous prompt as a new job | Medium |### 5.4 Prompt options
| Requirement ID | Requirement | Description | Priority ||----------------|-------------|-------------|----------|| PO-101 | Quality selection | Allow selection between &quot;low&quot;, &quot;mid&quot;, and &quot;high&quot; quality | High || PO-102 | Aspect ratio selection | Provide options for &quot;1024x1024&quot;, &quot;1536x1024&quot;, and &quot;1024x1536&quot; aspect ratios | High || PO-103 | Output compression | Enable compression setting between 0-100 with default of 50 | Medium || PO-104 | Fixed parameters | Set fixed values for output_format (&quot;webp&quot;), moderation (&quot;low&quot;), response_format (&quot;b64_json&quot;), and model (&quot;gpt-image-1&quot;) | High || PO-105 | Image count | Allow user to specify number of images to generate (n ≥ 1) | Medium |### 5.5 Storage
| Requirement ID | Requirement | Description | Priority ||----------------|-------------|-------------|----------|| ST-101 | API key storage | Securely store OpenAI API key in browser local storage | High || ST-102 | Image storage | Store generated images as Base-64 strings with prompt metadata | High || ST-103 | IndexedDB implementation | Use IndexedDB via wrapper for persistent storage, keyed by jobId | High || ST-104 | Offline access | Enable access to previously generated images when offline | Medium |### 5.6 Downloading
| Requirement ID | Requirement | Description | Priority ||----------------|-------------|-------------|----------|| DL-101 | Client-side download | Implement client-side download functionality for WebP images | High || DL-102 | Compression respect | Apply the specified output_compression setting to downloaded images | Medium |## 6. User stories and acceptance criteria
### 6.1 User setup and configuration
**ST-101: First-time setup**- As a new user, I want to set up my API key so I can start generating images
- **Acceptance Criteria:**  - Settings modal includes field for OpenAI API key entry
  - API key is securely stored in browser storage
  - Validation confirms API key is in correct format
  - User receives confirmation when key is saved successfully
**ST-102: Updating settings**- As a user, I want to update my API key and preferences when needed
- **Acceptance Criteria:**  - Settings modal is accessible from the top navigation bar
  - Previously saved API key is displayed (masked) for review
  - Changes are saved only when explicitly submitted
  - User receives confirmation when settings are updated
### 6.2 Image generation
**IG-101: Basic prompt submission**- As a user, I want to enter a text prompt to generate an image
- **Acceptance Criteria:**  - Input field is prominently displayed at bottom of screen
  - Submit button or enter key triggers generation
  - Visual indication shows generation is in progress
  - Generated images appear in the timeline upon completion
**IG-102: Advanced parameter configuration**- As a user, I want to customize image generation parameters
- **Acceptance Criteria:**  - Options for quality, aspect ratio, compression, and count are available
  - UI clearly indicates current selection and default values
  - Parameter changes persist during the current session
  - Selected parameters are correctly sent with API requests
**IG-103: Error handling**- As a user, I want to know when an error occurs during generation
- **Acceptance Criteria:**  - Error messages are clearly displayed when API calls fail
  - Different error types (authentication, content policy, server error) show appropriate messages
  - Options to retry failed jobs are provided
  - Helpful troubleshooting suggestions are included with errors
### 6.3 Timeline and history
**TL-101: Viewing generation history**- As a user, I want to see all my previously generated images in a timeline
- **Acceptance Criteria:**  - Timeline displays all past generations in a grid layout
  - Grid is responsive according to device size specifications
  - Images load efficiently with lazy loading implementation
  - Timeline maintains position when returning from detail view
**TL-102: Filtering and sorting**- As a user, I want to filter and sort my image history
- **Acceptance Criteria:**  - Filter options for date range, status, and aspect ratio are provided
  - Sort controls allow toggling between newest and oldest first
  - Filtered view updates immediately when selections change
  - Current filter/sort state is visually indicated
**TL-103: Infinite scrolling**- As a user, I want to seamlessly browse my entire generation history
- **Acceptance Criteria:**  - Additional history items load automatically when scrolling near bottom
  - Loading indicator appears during pagination fetches
  - Scroll position is maintained when new items load
  - Performance remains smooth even with large history
### 6.4 Image interaction
**II-101: Viewing image details**- As a user, I want to view detailed information about a generated image
- **Acceptance Criteria:**  - Clicking an image opens a detail view
  - Detail view shows full prompt text, all related images, and metadata
  - Information includes jobId, creation time, status, dimensions, and other parameters
  - Navigation controls allow browsing between images from the same prompt
**II-102: Downloading images**- As a user, I want to download generated images to my device
- **Acceptance Criteria:**  - Download button is available for each image in detail view
  - Downloaded images maintain proper quality based on compression setting
  - Files download in WebP format with appropriate naming
  - Download works on all supported devices and browsers
**II-103: Re-running prompts**- As a user, I want to re-run previous prompts to get new variations
- **Acceptance Criteria:**  - Re-run button is available in the detail view
  - Re-running uses the exact same parameters as the original generation
  - New job appears as a separate entry in the timeline
  - Visual indication distinguishes between original and re-run jobs
### 6.5 Edge cases
**EC-101: Offline usage**- As a user, I want to access my previously generated images when offline
- **Acceptance Criteria:**  - Previously generated images are viewable when offline
  - Clear indication shows that new generations aren&#x27;t possible offline
  - Application gracefully handles transition between online and offline states
  - Queued actions are processed when connection is restored
**EC-102: Storage limitations**- As a user, I want to be able to clear my storage on the settings page
## 7. Technical requirements
### 7.1 Technology stack
The technology stack is already set up, so the requirements below are complete, but use them as a reference to implement the features.
| Requirement ID | Requirement | Description ||----------------|-------------|-------------|| TS-101 | Language | Implement using TypeScript for type safety and better developer experience || TS-102 | Framework | Use Next.js 15 with App Router and React Server Components where appropriate || TS-103 | Styling | Implement styling with TailwindCSS and Shadcn UI components; use Lucide icons for actions || TS-104 | Forms | Implement forms with React Hook Form and Zod validation || TS-105 | State management | Use React Query or SWR for API calls and caching; implement Context/Store for global settings |### 7.2 API integration
| Requirement ID | Requirement | Description ||----------------|-------------|-------------|| API-101 | OpenAI client | Implement secure client for OpenAI&#x27;s Images API || API-102 | Parameter mapping | Correctly map UI parameters to API request parameters || API-103 | Error handling | Implement comprehensive error handling for API responses || API-104 | Rate limiting | Add support for managing API rate limits, including user feedback |### 7.3 Storage implementation
| Requirement ID | Requirement | Description ||----------------|-------------|-------------|| SI-101 | IndexedDB wrapper | Implement thin wrapper around IndexedDB for easier data access || SI-102 | Data schema | Define schema for storing jobs, images, and metadata || SI-103 | Query optimization | Optimize queries for timeline display and filtering || SI-104 | Data migration | Support schema migrations for future updates |### 7.4 Performance requirements
| Requirement ID | Requirement | Description ||----------------|-------------|-------------|| PR-101 | Image optimization | Implement lazy loading and appropriate compression for timeline images |## 8. Design and user interface
### 8.1 Visual design principles
- Minimalist interface focusing on content rather than chrome
- Consistent spacing and sizing throughout the application
- Clean typography with clear hierarchy
- Support for both light and dark modes via TailwindCSS
- High contrast ratio for accessibility
- Visual feedback for all user interactions
### 8.2 User interface components
#### 8.2.1 Main layout- Single-page application with fixed navigation bar at top
- Chat-style input fixed at bottom of screen
- Scrollable timeline grid occupying main content area
- Modal overlays for details view and settings
#### 8.2.2 Timeline grid- Responsive grid layout based on device size
- Each cell displays image thumbnail with minimal metadata
- Visual indication of job status (pending, completed, failed)
- Consistent padding and spacing between items
#### 8.2.3 Input area- Prominent text input for prompt entry
- Expandable panel for additional parameters
- Submit button with appropriate loading states
- Error feedback displayed inline
#### 8.2.4 Details view- Full-screen modal with close button
- Large image display with navigation between related images
- Complete metadata panel
- Action buttons for download and re-run
### 8.3 Accessibility requirements
- WCAG 2.1 AA compliance for all interface elements
- Keyboard navigation throughout the application
- Screen reader compatibility for all functionality
- Appropriate alt text derived from prompts
- Focus indicators for keyboard navigation
### 8.4 Responsive design specifications
| Breakpoint | Grid columns | UI adjustments ||------------|--------------|----------------|| Small (&lt; sm) | 1 column | Simplified options UI, vertical layout || Medium (sm - lg) | 2 columns | Compact metadata display, horizontal options || Large (&gt; lg) | 4 columns | Full feature display, expanded details view |### 8.5 Error states and feedback
- Clear error banners for API and authentication issues
- Inline validation for form inputs
- Loading indicators for in-progress actions
- Success confirmations for completed operations
- Helpful guidance for resolving common issues
```

```
# ImgxAI - 产品需求文档
## 1. 简介
本文档概述了 ImgxAI 的全面产品需求。ImgxAI 是一款网络应用程序，可让用户通过 OpenAI 的图像生成 API 生成图像。该产品需求文档（PRD）为开发团队提供参考，详细说明了功能、用户界面、技术要求和设计准则，以确保产品愿景的一致实现。
## 2. 产品概述
ImgxAI 是一款用户友好的网络应用程序，用户可以通过向 OpenAI 的图像生成 API 提交文本提示来创建 AI 生成的图像。该应用程序提供了一个简单直观的界面，用户可以在其中输入提示、自定义生成参数、以时间线格式查看他们的生成历史，并下载生成的图像。ImgxAI 在保持简约设计美学的同时，注重易用性、性能和可访问性。
## 3. 目标
### 3.1 主要目标
- 为使用 OpenAI 的 `gpt-image-1` 模型生成图像创建一个简化的界面。
- 为用户提供其图像生成历史的持久时间线。
- 在保持简单性的同时，允许对图像参数进行自定义。
- 确保在所有设备类型上都能实现响应式性能。
- 提供简约、可访问的用户体验。
## 4. 目标受众
### 4.1 主要用户
- 寻求灵感或素材的创意专业人士。
- 探索视觉概念的设计师。
- 需要自定义图像的内容创作者。
- 尝试 AI 图像生成的爱好者。
- 任何拥有 OpenAI API 密钥并对创建图像感兴趣的人。
### 4.2 用户需求
- 用于提交提示的简单直观界面。
- 快速访问先前生成的图像。
- 能够自定义图像参数。
- 离线访问先前生成的图像。
- 轻松下载生成的图像。
## 5. 功能和需求
### 5.1 核心集成
| 需求编号 | 需求 | 描述 | 优先级 |
|----------------|-------------|-------------|----------|
| CI - 101 | 图像模型集成 | 实现与 OpenAI `gpt-image-1` 模型的集成 | 高 |
| CI - 102 | API 调用实现 | 配置应用程序以调用 OpenAI 图像 API | 高 |
| CI - 103 | 默认审核 | 确保所有提示提交都包含 `moderation=&quot;low&quot;` 参数 | 高 |
### 5.2 用户界面
| 需求编号 | 需求 | 描述 | 优先级 |
|----------------|-------------|-------------|----------|
| UI - 101 | 统一布局 | 实现单页界面，底部固定聊天输入框，内容区域为可滚动的时间线网格 | 高 |
| UI - 102 | 响应式时间线网格 | 创建响应式网格布局：1 列（移动设备）、2 列（平板设备）、4 列（桌面设备） | 高 |
| UI - 103 | 无限滚动 | 实现分页功能，当滚动到页面底部时自动加载旧结果 | 中 |
| UI - 104 | 时间线过滤器 | 添加日期范围、状态、纵横比和排序顺序（最新/最旧）的过滤器，且界面简洁 | 中 |
| UI - 105 | 图像图块显示 | 在方形缩略图中按原始纵横比显示图像，点击可打开详细视图 | 高 |
| UI - 106 | 详细视图 | 创建模态框，显示完整提示、所有相关图像、元数据、重新运行按钮和下载选项 | 高 |
| UI - 107 | 导航 | 实现顶部栏，包含应用程序标题和设置按钮 | 中 |
### 5.3 提示生命周期
| 需求编号 | 需求 | 描述 | 优先级 |
|----------------|-------------|-------------|----------|
| PL - 101 | 提示提交 | 允许用户输入带有可选参数的文本提示 | 高 |
| PL - 102 | 任务处理 | 将提交的任务作为后台任务处理，并立即显示占位符 | 高 |
| PL - 103 | 结果显示 | 任务完成后，用实际图像更新占位符 | 高 |
| PL - 104 | 重新运行功能 | 允许用户将任何先前的提示作为新任务重新运行 | 中 |
### 5.4 提示选项
| 需求编号 | 需求 | 描述 | 优先级 |
|----------------|-------------|-------------|----------|
| PO - 101 | 质量选择 | 允许选择“低”、“中”和“高”质量 | 高 |
| PO - 102 | 纵横比选择 | 提供“1024x1024”、“1536x1024”和“1024x1536”纵横比选项 | 高 |
| PO - 103 | 输出压缩 | 启用 0 - 100 的压缩设置，默认值为 50 | 中 |
| PO - 104 | 固定参数 | 设置输出格式（“webp”）、审核（“low”）、响应格式（“b64_json”）和模型（“gpt-image-1”）的固定值 | 高 |
| PO - 105 | 图像数量 | 允许用户指定要生成的图像数量（n ≥ 1） | 中 |
### 5.5 存储
| 需求编号 | 需求 | 描述 | 优先级 |
|----------------|-------------|-------------|----------|
| ST - 101 | API 密钥存储 | 将 OpenAI API 密钥安全地存储在浏览器本地存储中 | 高 |
| ST - 102 | 图像存储 | 将生成的图像以 Base - 64 字符串形式与提示元数据一起存储 | 高 |
| ST - 103 | IndexedDB 实现 | 通过包装器使用 IndexedDB 进行持久存储，以任务 ID 作为键 | 高 |
| ST - 104 | 离线访问 | 支持离线访问先前生成的图像 | 中 |
### 5.6 下载
| 需求编号 | 需求 | 描述 | 优先级 |
|----------------|-------------|-------------|----------|
| DL - 101 | 客户端下载 | 实现 WebP 图像的客户端下载功能 | 高 |
| DL - 102 | 遵循压缩设置 | 对下载的图像应用指定的输出压缩设置 | 中 |
## 6. 用户故事和验收标准
### 6.1 用户设置和配置
**ST - 101：首次设置**
- 作为新用户，我希望设置我的 API 密钥，以便开始生成图像。
- **验收标准**：
  - 设置模态框包含用于输入 OpenAI API 密钥的字段。
  - API 密钥安全地存储在浏览器存储中。
  - 验证确认 API 密钥格式正确。
  - 密钥保存成功时，用户会收到确认信息。
**ST - 102：更新设置**
- 作为用户，我希望在需要时更新我的 API 密钥和偏好设置。
- **验收标准**：
  - 可以从顶部导航栏访问设置模态框。
  - 显示之前保存的 API 密钥（掩码形式）以供查看。
  - 仅在明确提交时才保存更改。
  - 设置更新时，用户会收到确认信息。
### 6.2 图像生成
**IG - 101：基本提示提交**
- 作为用户，我希望输入文本提示以生成图像。
- **验收标准**：
  - 输入字段显著显示在屏幕底部。
  - 提交按钮或回车键触发生成。
  - 有视觉指示表明正在生成。
  - 生成的图像完成后会显示在时间线中。
**IG - 102：高级参数配置**
- 作为用户，我希望自定义图像生成参数。
- **验收标准**：
  - 提供质量、纵横比、压缩和数量的选项。
  - 用户界面清晰显示当前选择和默认值。
  - 参数更改在当前会话期间保持有效。
  - 选定的参数正确随 API 请求发送。
**IG - 103：错误处理**
- 作为用户，我希望在生成过程中出现错误时得到通知。
- **验收标准**：
  - API 调用失败时，清晰显示错误消息。
  - 不同类型的错误（身份验证、内容策略、服务器错误）显示相应的消息。
  - 提供重试失败任务的选项。
  - 错误信息中包含有用的故障排除建议。
### 6.3 时间线和历史记录
**TL - 101：查看生成历史**
- 作为用户，我希望在时间线中查看我所有先前生成的图像。
- **验收标准**：
  - 时间线以网格布局显示所有过去的生成记录。
  - 网格根据设备大小规格进行响应式调整。
  - 通过懒加载实现图像高效加载。
  - 从详细视图返回时，时间线保持原位置。
**TL - 102：过滤和排序**
- 作为用户，我希望对我的图像历史记录进行过滤和排序。
- **验收标准**：
  - 提供日期范围、状态和纵横比的过滤选项。
  - 排序控件允许在最新和最旧优先之间切换。
  - 选择更改时，过滤视图立即更新。
  - 直观显示当前的过滤/排序状态。
**TL - 103：无限滚动**
- 作为用户，我希望无缝浏览我的整个生成历史记录。
- **验收标准**：
  - 滚动到底部附近时，自动加载更多历史记录项。
  - 分页获取数据时显示加载指示器。
  - 新项加载时，保持滚动位置。
  - 即使历史记录很多，性能也能保持流畅。
### 6.4 图像交互
**II - 101：查看图像详细信息**
- 作为用户，我希望查看生成图像的详细信息。
- **验收标准**：
  - 点击图像打开详细视图。
  - 详细视图显示完整提示文本、所有相关图像和元数据。
  - 信息包括任务 ID、创建时间、状态、尺寸和其他参数。
  - 导航控件允许在同一提示生成的图像之间浏览。
**II - 102：下载图像**
- 作为用户，我希望将生成的图像下载到我的设备上。
- **验收标准**：
  - 详细视图中的每个图像都有下载按钮。
  - 下载的图像根据压缩设置保持适当的质量。
  - 文件以 WebP 格式下载，并具有适当的命名。
  - 下载功能在所有支持的设备和浏览器上均可使用。
**II - 103：重新运行提示**
- 作为用户，我希望重新运行先前的提示以获得新的变体。
- **验收标准**：
  - 详细视图中提供重新运行按钮。
  - 重新运行使用与原始生成完全相同的参数。
  - 新任务在时间线中作为单独的条目显示。
  - 有视觉指示区分原始任务和重新运行的任务。
### 6.5 边缘情况
**EC - 101：离线使用**
- 作为用户，我希望在离线时访问我先前生成的图像。
- **验收标准**：
  - 离线时可以查看先前生成的图像。
  - 明确指示离线时无法进行新的生成。
  - 应用程序能够优雅地处理在线和离线状态之间的转换。
  - 连接恢复时处理排队的操作。
**EC - 102：存储限制**
- 作为用户，我希望能够在设置页面上清除我的存储。
## 7. 技术要求
### 7.1 技术栈
技术栈已经确定，因此以下要求完整，但可作为实现功能的参考。
| 需求编号 | 需求 | 描述 |
|----------------|-------------|-------------|
| TS - 101 | 编程语言 | 使用 TypeScript 实现，以保证类型安全并提升开发体验 |
| TS - 102 | 框架 | 适当使用带有应用路由器和 React 服务器组件的 Next.js 15 |
| TS - 103 | 样式 | 使用 TailwindCSS 和 Shadcn UI 组件实现样式；使用 Lucide 图标进行操作 |
| TS - 104 | 表单 | 使用 React Hook Form 和 Zod 验证实现表单 |
| TS - 105 | 状态管理 | 使用 React Query 或 SWR 进行 API 调用和缓存；使用 Context/Store 管理全局设置 |
### 7.2 API 集成
| 需求编号 | 需求 | 描述 |
|----------------|-------------|-------------|
| API - 101 | OpenAI 客户端 | 实现用于 OpenAI 图像 API 的安全客户端 |
| API - 102 | 参数映射 | 正确将用户界面参数映射到 API 请求参数 |
| API - 103 | 错误处理 | 为 API 响应实现全面的错误处理 |
| API - 104 | 速率限制 | 支持管理 API 速率限制，包括向用户反馈 |
### 7.3 存储实现
| 需求编号 | 需求 | 描述 |
|----------------|-------------|-------------|
| SI - 101 | IndexedDB 包装器 | 实现一个围绕 IndexedDB 的轻量级包装器，以便更轻松地访问数据 |
| SI - 102 | 数据架构 | 定义用于存储任务、图像和元数据的架构 |
| SI - 103 | 查询优化 | 优化用于时间线显示和过滤的查询 |
| SI - 104 | 数据迁移 | 支持未来更新的架构迁移 |
### 7.4 性能要求
| 需求编号 | 需求 | 描述 |
|----------------|-------------|-------------|
| PR - 101 | 图像优化 | 为时间线图像实现懒加载和适当的压缩 |
## 8. 设计和用户界面
### 8.1 视觉设计原则
- 简约界面，注重内容而非界面装饰。
- 整个应用程序的间距和大小保持一致。
- 简洁的排版，具有清晰的层次结构。
- 通过 TailwindCSS 支持浅色和深色模式。
- 高对比度以确保可访问性。
- 为所有用户交互提供视觉反馈。
### 8.2 用户界面组件
#### 8.2.1 主布局
- 单页应用程序，顶部有固定的导航栏。
- 聊天式输入框固定在屏幕底部。
- 可滚动的时间线网格占据主要内容区域。
- 用于详细视图和设置的模态覆盖层。
#### 8.2.2 时间线网格
- 基于设备大小的响应式网格布局。
- 每个单元格显示带有最少元数据的图像缩略图。
- 直观显示任务状态（待处理、已完成、失败）。
- 项目之间的填充和间距一致。
#### 8.2.3 输入区域
- 用于输入提示的显著文本输入框。
- 可展开的面板，用于显示额外参数。
- 带有适当加载状态的提交按钮。
- 内联显示错误反馈。
#### 8.2.4 详细视图
- 带有关闭按钮的全屏模态框。
- 大图像显示，可在相关图像之间导航。
- 完整的元数据面板。
- 用于下载和重新运行的操作按钮。
### 8.3 可访问性要求
- 所有界面元素符合 WCAG 2.1 AA 标准。
- 整个应用程序支持键盘导航。
- 所有功能与屏幕阅读器兼容。
- 从提示中获取适当的 alt 文本。
- 为键盘导航提供焦点指示器。
### 8.4 响应式设计规范
| 断点 | 网格列数 | UI 调整 |
|------------|--------------|----------------|
| 小（&lt; sm） | 1 列 | 简化选项 UI，垂直布局 |
| 中（sm - lg） | 2 列 | 紧凑的元数据显示，水平选项 |
| 大（&gt; lg） | 4 列 | 完整功能显示，扩展的详细视图 |
### 8.5 错误状态和反馈
- 为 API 和身份验证问题提供清晰的错误横幅。
- 对表单输入进行内联验证。
- 为正在进行的操作提供加载指示器。
- 为已完成的操作提供成功确认。
- 为解决常见问题提供有用的指导。
```

与上一步类似，检查输出确保其中不包含不需要的内容。

接下来，我们将使用此产品需求文档来生成任务、子任务、分析复杂度并设置任务之间的依赖关系。

## 3. 通过 MCP 设置任务管理系统

在此步骤中，我们将通过 MCP 设置任务管理系统。

如今，有一些工具可以帮助管理 AI 任务。最受欢迎的有：

1.  [Taskmaster AI (prev.](https://www.task-master.dev/) Claude Task Master)

1.  Roo code Boomerang 任务

1.  [虾任务管理器](https://github.com/cjo4m06/mcp-shrimp-task-manager)

1.  [Cline 任务工具](https://docs.cline.bot/exploring-clines-tools/new-task-tool)

1.  在 Cursor/Trae 等 中构建自己的 .md 文件，并列出任务清单，并设置规则来运行它们。

我开始通过创建自己的 .md 文件，并尝试运行这些任务来构建应用程序。它们都能运行，但目前在最佳状态的是 Taskmaster AI 。

Taskmaster AI 具有命令行界面，并且与平台/文本编辑器无关。然而，它还支持 Cursor & Trae，并附带 MCP 服务器。

MCP 服务器很棒，因为它不需要切换到不同的窗口，因此可以为您的文本编辑器提供任务上下文。

那么让我们设置 Taskmaster AI MCP server 。

### 3.1 安装 Taskmaster AI

为此，您需要一个 Anthropic API 密钥和一个 Perplexity API 密钥。

一旦你获得了这些，前往你的 MCP 设置并打开你的设置 JSON。

在光标中，那就是在 Settings > MCP 下，然后按 Add new global MCP server 。

![](img/931d2ac4952754c7ec836e5f3b9f0461.png)

在 MCP 设置中，将以下配置拖放到 Taskmaster AI 服务器：

```
{
  &quot;mcpServers&quot;: {
    &quot;taskmaster-ai&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;--package=task-master-ai&quot;,
        &quot;task-master-ai&quot;
      ],
      &quot;env&quot;: {
        &quot;ANTHROPIC_API_KEY&quot;: &quot;ADD_KEY&quot;,
        &quot;PERPLEXITY_API_KEY&quot;: &quot;ADD_KEY&quot;,
        &quot;MODEL&quot;: &quot;claude-3-7-sonnet-20250219&quot;,
        &quot;PERPLEXITY_MODEL&quot;: &quot;sonar-pro&quot;,
        &quot;MAX_TOKENS&quot;: 64000,
        &quot;TEMPERATURE&quot;: 0.2,
        &quot;DEFAULT_SUBTASKS&quot;: 5,
        &quot;DEFAULT_PRIORITY&quot;: &quot;medium&quot;
      }
    }
  }
}
```

保存更改，并确保服务器状态为绿色。如果不是，请按刷新按钮，它应该会启动。

### (可选) 准备代码库

如果您在现有的代码库上执行此操作，可以跳过此步骤。

但要小心！⚠️

切勿从一个空代码库开始。那里会有龙。而且，大部分情况下，大量的令牌将被浪费在一个奇怪的基础之上。使用 CLI，你喜欢的模板，或者直接使用 Shipixen，但无论你做什么，都不要从零开始！

代码库中需要以下内容：

| 功能 | 推荐 |
| --- | --- |
| 类型检查 | TypeScript |
| 代码检查工具 | ESLint |
| Formatter | 更美观 |
| UI 组件 | React/Shadcn UI |
| CSS 库 | TailwindCSS (有了 AI 就更好了) |
| 状态管理 | Zustand |

如果你计划使用任何数据库或某种 API，请确保这些也设置好了。

此外，每当我启动一个新项目时，我总会添加 Sentry。这样我睡得更好。

### 3.2 设置Cursor rules规则

注意！这就像 Cursor 中的任何其他项目一样，所以你仍然可以添加自己的规则。以下是你应该在规则中涵盖的一些内容：

+   技术栈和库的版本

+   整体项目结构（解释文件夹名称）

+   命名规范

+   风格/语言偏好

+   UI 编码偏好

足够你开始了。

顺便说一句，这里有一个超级小技巧，也是生成 Cursor 规则的绝对最佳方法。

![](img/da82e55fc57e1f6877f29bd640e73d1d.png)

1.  前往 Cursor Chat 并输入 /Generate Cursor Rules

1.  然后 @MyFile.tsx 或你想要生成规则的任何文件。

1.  提示它 I want to generate cursor rules for writing UI components. Please analyze the file and outline all the conventions found.

我想为编写用户界面（UI）组件生成Cursor rules。请分析该文件并概述所发现的所有规范。

这将创建一个或多个精确的 Cursor 规则 .mdx 文件！（但还是要检查一下）。

---

[如果你需要更多灵感](https://github.com/elie222/inbox-zero/blob/main/.cursor/rules/cursor-rules.mdc)，Elie Steinbock 在这里开源了他的 Cursor 规则。

```
自从我开源了我的 Cursor .mdc 规则以来，我收到了很多问题。
所有答案都在这里：
20 个 mdc 文件对 Cursor 来说是不是太多信息？
不是。这就是 .mdc 文件解决的问题。Cursor 规则文件现在已弃用。您不再一次性提供所有信息，而是以小块的形式（.mdc 文件）。我的每个 .mdc 文件都少于 50 行。

您需要手动标记光标吗？
规则有 4 个选项：
a. 总是 - 该规则是每个聊天的组成部分
b. 自动附加 - 例如，在您讨论 &quot;.tsx&quot; 文件或 &quot;tailwind.config&quot; 文件时自动附加。
c. 代理请求 - 代理可以请求读取文件。这是我大部分时间使用的方式。代理会根据您为规则设置的描述来决定是否请求规则。
d. 手动 - 仅在打标签时应用。

我主要使用 c。我经常自己为光标打标签规则文件，因为我不确定否则是否总会读取它。

我如何编写规则？
我有一个 cursor-rules.mdc 文件，用于编写规则。最简单的方法：
与 Cursor 分享一段代码（在相关文本上使用 command-shift-L 命令）
标记 .cursor-rules.mdc 文件
让 Cursor 为此模式创建一个新的 .mdc 文件

小技巧&quot; 光标在这里有点小问题。我更喜欢在 Ask 模式下操作，并将结果复制到 mdc 文件中。有时在 Agent 模式下这样做时，你会看到光标显示空文件。

它能工作吗？
是的。在视频末尾（链接在下一条推文中）观看它的效果。
如果你让 AI 自行处理，它会创造新的模式。我们编写的很多代码都是重复的事情（例如，添加表单、进行 LLM 调用、添加更新数据库的服务器操作等）。编写代码没有唯一正确的方法。但在你的项目中是有的。你希望代码保持一致性。每个表单的代码应该看起来或多或少相同。规则有助于光标有效实现这一点。（在你的代码库中对其他代码示例进行标记是另一种方法，但.mdc 更干净，噪音更少）。

设置规则值得花力气吗？
值得。设置规则只需最小限度的、一次性的工作。一旦有了基础，就更容易了。而且它们会永远提供价值。
光标代理是我最得力的员工。给它必要的上下文，使其发挥最大效能，这是非常值得的。

你使用哪些规则？
到目前为止，我的规则可以归纳为几个类别：
a. 代码模式（例如添加表单、服务器操作或测试）
b. 通用知识（例如项目结构）
c. 功能（例如这是收件箱清理器的作用以及它的工作原理）

我的大多数规则都属于“代码模式”。但我正在稳步添加更多的“功能”规则。我经常反复对 LLMs 说同样的话。我可能在使用 Claude 或 Anthropic，现在可以在这份笔记中复制粘贴了。当然，我也可以在 Cursor 中给这些笔记打标签。
如果你想看到我的规则的实际应用，可以查看我的 YouTube 频道或 Inbox Zero GitHub
```

> Elie Steinbock 推文相关链接

> Repo (give it a star!): https://github.com/elie222/inbox-zero

> 妙记视频-自行使用翻译查看:

<https://larkcommunity.feishu.cn/minutes/embed/obcngk31711c4gg29np4vvd6?from=ccm>

> My channel: youtube.com/elie2222

---

### 3.3 初始化任务管理器 AI

> 在开始之前，请确保：

+   在您的文本编辑器中选择“代理模式”

+   选择“Claude 3.7 Sonnet”作为您的模型

现在我们已经设置了 MCP 服务器和代码库，我们可以初始化 TaskMaster AI。

1.  将上述的 PRD 输出粘贴到 scripts/prd.txt 下新建的文件中。

1.  在 Cursor Chat 中，粘贴以下提示：

```
I&#x27;ve initialized a new project with Claude Task Master. I have a PRD at scripts/prd.txt.
Can you parse it and set up initial tasks?
```

```
我已经用Claude任务管理器初始化了一个新项目。我在scripts/prd.txt中有一份产品需求文档（PRD）。
你能解析它并设置初始任务吗？
```

![](img/9039aa2e30ab295d4a86c5bf8a5c867c.png)

![](img/3649f6b1b01595020aa7dc2a4314638a.png)

1.  这将把您的 PRD 文件转换为一组文件（在 /tasks 下），以及一个 /tasks/tasks.json 文件，其中包含任务、子任务和元数据的结构化列表。

1.  结果看起来会像这样：

![](img/ccc1d9e38cc94831fa13f236137dd94f.png)

![](img/9532adfe219b0180105bb69395e0790a.png)

> 这些默认不会添加到 Git 中，但我建议你这样做。以防 AI 失控。

1.  分析复杂度。现在，我们将要求 Taskmaster AI 分析任务的复杂度。这将使用 Perplexity 进行网络研究，并决定一个 1-10 的复杂度分数。

```
Can you analyze the complexity of our tasks to help me understand which ones need to be broken down further?
```

```
你能分析一下我们任务的复杂度，帮助我了解哪些任务需要进一步拆分吗？
```

![](img/6151430409b79046ee121cbdb11f4bbe.png)

![](img/e29b7a2ee33ee268bd379b618836e470.png)

响应看起来会像这样：

![](img/01879157505c8611c9923e9b8145942f.png)

![](img/d1009ed77af7ffd4c1963a230abc6688.png)

你可以用 Please break down the identified tasks into subtasks. 请将已确定的任务分解为子任务。来回复，它就会那样做。记得说请😃

1.  进一步分解任务。

如果你看到一些过于复杂的任务，也可以将它们分解。

理想情况下，你应该将任务分解成对你（人类）来说容易实现的单元。如果你觉得某件事复杂，那么对 AI 来说也可能会很复杂。

```
Task 3 seems complex. Can you break it down into subtasks?
```

```
任务3似乎很复杂。你能把它分解成子任务吗？
```

而且这就是设置！你可以根据需要继续分解任务。

如果你错过了什么，不用担心。你可以随时添加任务或改变实施方向。

这里有一些你可以使用的提示配方：

+   添加一个新任务

```
Let&#x27;s add a new task. We should implement sorting of the timeline.
Here are the requirements:
- you should be able to sort the timeline by date
- a dropdown should be available to select the sorting direction
- the sorting should be persisted when a new page is loaded
```

```
让我们添加一个新任务。我们应该实现时间线的排序功能。
以下是具体要求：
- 你应该能够按日期对时间线进行排序
- 应提供一个下拉菜单来选择排序方向
- 加载新页面时，排序结果应保持不变
```

+   改变一项任务的方向

```
There should be a change in the image generation task.
Can you update task 3 with this and set it back to pending?
The image generation should use gpt-image-1 as the model.
```

```
图像生成任务应该有变化。
你能以此更新任务3并将其重新设为待处理状态吗？
图像生成应该使用gpt-image-1作为模型。
```

+   弃用一项任务

```
Task 8 is not needed anymore. You can remove it.
```

```
任务8不再需要了。你可以将其移除。
```

不，让我们开始构建！（或者，我应该说是，让我们看 AI 构建！）

## 4. 构建应用（提示循环）

在完成所有这些设置后，我们已经 100%准备好了放手轮子，让 AI 做它的事情。

这也是最容易的部分！这是我最终构建的东西：

![](img/59e62cc20f50c6c102de034f63a0e355.png)

[imgxai-v1-demo.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/HjUmbynLwoYPT9x32ALcXjClnye)

那么，无需多言，这里是如何构建类似东西的方法：

1.  在 Cursor 中开启一个新的聊天并提示它：

显示任务

![](img/ca2eb08c3c6ca15a6658eee8533b2723.png)

![](img/e90ae46afd7a71d2f0794bbae9d67e59.png)

1.  然后，提示它：

```
What&#x27;s the next task I should work on? Please consider dependencies and priorities.
```

```
我接下来应该处理的任务是什么？请考虑任务的依赖关系和优先级。
```

这将根据依赖关系和优先级选择下一个任务。

![](img/b77afc85ccf3c863331d387f55c57391.png)

![](img/8508d700d7b05b58f7ae0e5c3378f450.png)

1.  实现任务。

```
Implement task 2 and all of its subtasks.
```

```
完成任务2及其所有子任务。
```

![](img/b5a8fcf66f4a3aa97645f99979a014d0.png)

![](img/d7ffde0d2f408d31938312042aee37f7.png)

> 根据任务的复杂性，您可能只想让它实现一个子任务，例如 Implement subtask 2.1 。

1.  遍历。

完成任务后，对 UI/应用进行冒烟测试。如果看起来良好，创建一个新的聊天并提示它：

```
显示任务
```

```
What&#x27;s the next task I should work on? Please consider dependencies and priorities.
```

```
我接下来应该处理的任务是什么？请考虑依赖关系和优先级。
```

```
实施任务 X
```

到此为止，所有的事情都完成了！

逐一地，你会看到应用开始运行。

在我这边，我一开始有 15 个任务，最后有 19 个。

这款应用结果非常令人印象深刻。

👉 在 I[mgx AI ](https://imgxai.com/imgx)上查看。

真是非常令人印象深刻，不是吗？！

构建过程大体上很顺利，但我还是遇到了一些问题。这并不是我第一次接触人工智能，所以我会分享一些专业技巧，教你如何充分利用它。

## 思考

让我对 AI 编程坦诚相告。

有一个需要被揭穿的巨大神话。

所有那些展示 AI 神奇地创建完美应用程序的视频？它们并不完全诚实。

虽然 TaskMaster AI 将任务系统地分解成小步骤（这比试图一次性处理整个复杂系统要好得多），但你仍然需要保持参与。是的，AI 可以编写 90%的代码——甚至可能更多——但你绝对仍然掌握着方向盘。

你需要测试 UI 中的每个功能，验证它是否按预期工作，并在事情出错时进行调整。通常不是代码有问题，而是行为。

想想传统的开发方式：每次增量更改后，你都会进行冒烟测试以确保一切正常工作。AI 编码也是如此。

你不能让 AI 一次又一次地完成任务，而无需检查它正在做什么，并在需要时引导它走向正确的方向。

在这种情况下，你的开发技能仍然会有所不同。

但是，你可以用它来构建一些酷炫的东西。我印象深刻！

## 小技巧

### 1. 你仍然可以为每次任务运行添加额外的上下文

在提示人工智能处理下一个任务时，请确保提供额外的上下文，例如 UI 偏好、API 文档等。您也可以附加图片！

这将引导它走向您想要的方向。

### 2. 将超过 500 行的大文件分解

人工智能在处理大文件方面并不出色。所以，如果你有一个超过 500 行的文件，请将其分解成更小的文件。

这里有一个提示语你可以使用：

```
Break down this file into logical modules so it&#x27;s easier to ready.
Create directories if needed and move utils and interfaces to separate files, maintaining a domain-driven file structure.
```

```
将此文件拆分为逻辑模块，以便更易于阅读。
如有需要，创建目录，并将工具函数和接口移至单独的文件中，保持以领域为驱动的文件结构。
```

### 3. 错误也是任务！

对于一个足够复杂的系统，你最终会遇到一个需要更改底层架构的错误。

人工智能会尝试应用一个表面的修复，但你最终只会陷入循环。

所以你可以为这个错误创建一个新任务并实施它。

这里是一个你可以使用的提示：

```
The filter feature is not working as expected. Create a new task to fix it:
- the filter should be case insensitive
- it should work with pagination
- it should work with the debounce
```

```
过滤功能未按预期工作。创建一个新任务来修复它：
- 过滤应不区分大小写
- 它应与分页功能协同工作
- 它应与防抖功能协同工作
```

### 4. 在执行下一个任务时，始终开始一个新的聊天

有时过多的上下文会使事情变得更糟。因此，在执行下一个任务时，始终开始一个新的聊天。

## 常见问题解答

### 什么是 MCP 服务器，我为什么需要它？

将 MCP 服务[器视为 AI 代理](https://github.com/modelcontextprotocol/servers)可以用来增强其能力的 API。有很多 MCP 服务器，TaskMaster AI 只是其中之一。其他酷炫的也可以与第三方 API 集成。

查看这个庞大的 MCP 服务器列表。

---

### 我现在真的能用 AI 开发整个应用吗？

是的，也不是。是的——它可以编写 80-90%的代码。不是的——你仍然需要掌舵，测试 UI，调试奇怪的东西，有时还需要创建新任务。但生产力的提升是真实的。

---

### 我应该从零开始用 AI 编程吗？

不。始终从一个自举的代码库开始。

---

### 我该如何修复 AI 引入的错误？

将错误视为任务。不要只说“修复它”——描述错误，你期望的结果，以及它是如何失败的。然后创建一个新任务，让 AI 系统地处理它。如果你让 AI 只是临时修补错误，你只是在将问题往后推。

---

### **我为什么任务实现会跑偏？**

可能是两件事：(1) 任务过于模糊，或 (2) 你的聊天上下文过于臃肿。为每个主要任务开启一次新的聊天，并在需要时重新表述。做一个好的项目经理。AI 会感谢你。

---

### **这在大代码库中能扩展吗？**

是的，可以。但你需要持续应用技巧：拆分大文件，分解任务，并始终开启新的聊天。

此外，代码库越大，你在开始新任务时提供现有模块提示的需求就越大。

---

### **你用了什么模型？**

我用了 Claude 3.7 Sonnet。