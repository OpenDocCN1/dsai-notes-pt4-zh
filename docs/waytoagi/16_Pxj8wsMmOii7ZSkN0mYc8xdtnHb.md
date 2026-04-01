# 2.0 AI 编程

> 来源：[https://waytoagi.feishu.cn/docx/S44LdsRw7ozKVyxvxcncRvb2nJb](https://waytoagi.feishu.cn/docx/S44LdsRw7ozKVyxvxcncRvb2nJb)

### WaytoAGI AI 编程区

目标：AI 编程让你变得更强大，上手 AI 编程，有手就行

---

#### 🚀 新手入门

[🚀 新手入门](https://waytoagi.feishu.cn/wiki/QYpjwMVVsiiVzbktxyFc4I6ansb)基础？从这里开始。工具选型、入门教程、学习路径一站搞定。

#### 🛠️ 工具专区

深入学习 Claude Code、Cursor、Tra[🛠️ 工具专区](https://waytoagi.feishu.cn/wiki/VPluwEi6niXjMdkEeVfctv4Rnad)、Codex 等主流 AI 编程工具。

#### 💡 实战案例

Vibe Coding 实践、AI+网站、AI+小程[💡 实战案例](https://waytoagi.feishu.cn/wiki/J9cfwBy7Aio0EIk54b2cznEcnTd)、AI+移动端、AI+插件、亲子共学。

#### 📚 进阶与深度

[📚 进阶与深度](https://waytoagi.feishu.cn/wiki/Dwq6wrW6PiY7HHke0TicfQ54n7g)核技术、编程心得、GitHub 开源、行业应用图谱。

#### 🎯 社区与活动

编程活动、共学回放、交流群。[🎯 社区与活动](https://waytoagi.feishu.cn/wiki/Adgowqpz3ixkSpk46SncthatnDv)

---

### AI编程工具对比

| 分类 | 门槛 | **产品/平台名称** | 产品形态及主要特点 | 典型用户场景（目标用户） |
| --- | --- | --- | --- | --- |
| 在线编程工具 | 极低 | [**Lovable**](http://Lovable.dev) | - AI 驱动的全栈/无代码平台，通过自然语言快速生成应用  - 支持 UI、后端、认证、存储、支付集成  - 快速部署，适合快速原型与产品上线 | - 非技术创始人或小团队快速验证商业想法  - 产品经理／创业者制作 MVP  - 零代码用户构建完整应用 |
|  |  | [**秒哒**](https://www.miaoda.cn/) | - 零代码 AI 应用生成平台：一句话描述即可生成前后端完整应用  - 支持多智能体协作、工具 API 调用丰富功能扩展  - 适合非技术人群，无需编程能力即可开发应用 | - 企业业务人员／产品经理快速生成工具  - 初创公司或中小企业构建内部应用  - 零代码开发爱好者 |
|  |  | [**扣子空间**](https://space.coze.cn/) | - AI 协同办公与 Agent 平台，可自动解析任务、拆解、调用工具并输出结果  - 支持“探索”与“规划”双模式，用户可在关键节点干预  - 多格式输出（报告、PPT、网页、音频）、集成丰富插件，覆盖内容创作、分析等场景 | - 企业或个人提升办公效率  - 市场研究、内容创作、业务分析人员  - 需要跨工具协作、自动化执行任务的用户 |
| 本地专业 IDE | 中等 | [**Cursor**](https://cursor.com/en)** ** | - AI 驱动的代码编辑器／IDE，支持自然语言编程、上下文补全、代码解释和重构建议  - 支持多语言，集成终端、Git 操作和安全扫描  - 已在开发者社区中广泛应用 | - 专业开发者提升编码效率与质量  - 团队协作、维护大项目的工程师  - 想用自然语言快速生成及理解代码的用户 |
|  |  | [**Trae - CN**](https://www.trae.cn/) | +    基于 VS Code，几乎无学习成本  | 独立开发者 / 学生 / 副业开发者 |
|  |  | [** Kiro**](https://kiro.dev/) | 强调 spec-driven development，把自然语言先转成需求、设计、任务，再交给 agent 执行；支持 hooks、MCP、自动执行大任务 | 复杂项目开发，需求到实现链路较长，团队希望把 AI 开发流程规范化的场景 |
|  |  | [**codebuddy**](https://www.codebuddy.com/) | 更偏 **编码提效工具**：代码补全、上下文感知、多语言、自然语言交互、毫秒级补全 | 日常写代码、补全、问答、理解项目、在现有 IDE 里提效 |
|  |  | [**qoder**](https://qoder.com/) | 介于助手和 agent 之间。既有补全，也有 Agentic Chat、Autonomous Agent、RepoWiki、Memory/Rules、MCP，强调真实软件开发和代码库理解 | 想让 AI 参与更完整开发流程的人，包括规划、实现、文档化、持续协作 |
| CLI 工具（命令行） | 有一定门槛，上限很高 | [** Claude code**](https://www.anthropic.com/claude-code) | - Anthropic 提供的命令行工具，用于代码生成与实验  - 强调自然语言交互与安全性  - 可辅助开发与研究 | - AI 开发者、研究人员  - 需要代码生成与调试的用户  - 对安全性有要求的企业 |
|  |  | [**Gemini CLI**](https://github.com/google-gemini/gemini-cli) | - Google 推出的命令行工具，将 Gemini AI 带入 CLI  - 支持代码生成、调试、任务管理与内容生成  - 与 Gemini Code Assist 深度结合，跨平台使用 | - 喜欢命令行的开发者  - DevOps 工程师  - 需要在终端集成 AI 的自动化场景 |
|  |  | [**Codex**](https://openai.com/codex/) | - OpenAI 早期代码模型，支持多语言生成  - GitHub Copilot 的底层模型  - 支持 GPT-5 | - 代码自动补全  - 简单脚本生成  - IDE 插件用户 |

### 入门教程

+   [彭骋：AI 编程实践-零基础上手AI编程](https://waytoagi.feishu.cn/wiki/UwjzwJroTiK9fok8YfVcocFonYd)

+   [云舒：我用AI上线一个插件后，整理了一份小白也能用的AI编程指南](https://waytoagi.feishu.cn/wiki/Jf2dwLo4LiwmIBkySx4c7H5Wnjb)

+   [向阳乔木：15分钟从 0 到 1 开发Chrome插件，实现小红书、B站等4个网站搜索对比，附谷歌插件发布指南](https://waytoagi.feishu.cn/wiki/ASWrwvVEXiIht7krSvNcorxVntg)

### 交流群

#### 飞书群

#### 微信群

![](img/ec0197984c7cf8c24e5256ca7d4976df.png)

### 共创者计划招募

> 【📖 WaytoAGI AI 编程区：「AI编程系统教程」共建计划招募】各位开发者👨‍💻👩‍💻，WaytoAGI喊你一起干票大的！💥我们正在启动一个"AI编程系统教程"共建计划📖，要和社区高手一起，搞定这四个方向：🤖 AI + 网站📱 AI + 小程序🍎 AI + iOS🌐 AI + 浏览器插件如果你有相关经验，想在AI时代留下你的作品，结识更多牛人🤝，那就别犹豫！时间有限，就差你了！👉 点击链接，即刻上车！👇 报名通道：https://waytoagi.feishu.cn/share/base/form/shrcn8AES9DBuyY8XB5KixKT1ab

### 子页面目录