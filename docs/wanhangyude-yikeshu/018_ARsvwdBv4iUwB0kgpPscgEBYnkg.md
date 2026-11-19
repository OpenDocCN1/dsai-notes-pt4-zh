
# Claude Code Skills 技能大全

> 来源：`https://my.feishu.cn/docx/GUsVdvJgIoxd9pxo3QdczjJknsd`

> 版本：2026年2月 | 总计：111+ 技能

本文档汇总了所有已安装的 Claude Code Skills，按功能分类并标注推荐等级。

---

## 📊 技能来源总览

| 来源 | 技能数 | 推荐度 | 说明 |
| --- | --- | --- | --- |
| `Superpowers` | 31 | ⭐⭐⭐⭐⭐ | 核心开发工作流框架 |
| `Anthropic 官方` | 12 | ⭐⭐⭐⭐⭐ | 官方维护的技能 |
| `daymade/claude-code-skills` | 34 | ⭐⭐⭐⭐⭐ | 社区精选技能集 |
| `Context-Engineering` | 13 | ⭐⭐⭐⭐⭐ | 上下文工程专家技能 |
| `awesome-agent-skills` | 5 | ⭐⭐⭐⭐ | Agent 开发示例 |
| `其他社区技能` | 15+ | ⭐⭐⭐⭐ | 各类实用技能 |

---

## 🚀 必装核心技能 (强烈推荐)

### Superpowers 核心工作流

> 来源：`obra/superpowers` | 2k+ Star

完整软件开发工作流，从设计到部署的全流程支持。

| 技能 | 功能描述 | 使用场景 |
| --- | --- | --- |
| `brainstorming` | 苏格拉底式交互设计 | 开始新项目前，将想法转化为设计 |
| `writing-plans` | 创建详细实现计划 | 设计完成后，分解为具体任务 |
| `using-git-worktrees` | 创建隔离工作区 | 开始功能开发前，设置独立工作空间 |
| `subagent-driven-development` | 子驱动开发模式 | 按任务执行计划，每个任务独立子代理 |
| `test-driven-development` | TDD 开发实践 | 编写功能时先写测试 |
| `requesting-code-review` | 代码审查请求 | 完成任务后，自动触发代码审查 |
| `systematic-debugging` | 四阶段调试框架 | 遇到 bug 时，系统性定位根因 |
| `finishing-a-development-branch` | 完成开发分支 | 功能完成后，合并/PR/清理 |

### 官方技能

> 来源：`anthropics/skills`

| 技能 | 功能描述 | 使用场景 |
| --- | --- | --- |
| `docx` | Word 文档处理 | 创建/编辑/分析 `.docx` 文件 |
| `pdf` | PDF 操作工具 | 提取文本/合并/拆分/OCR |
| `pptx` | PowerPoint 处理 | 创建/编辑演示文稿 |
| `xlsx` | Excel 表格处理 | 创建/编辑/分析电子表格 |
| `frontend-design` | 前端界面设计 | 避免通用 AI 美学，打造独特 UI |
| `skill-creator` | 技能创建指南 | 创建新技能时使用 |

---

## 🛠️ 开发工具类

### Git & GitHub

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `github-ops` | GitHub 操作全流程 | ⭐⭐⭐⭐⭐ |
| `github-contributor` | GitHub 开源贡献策略 | ⭐⭐⭐⭐⭐ |
| `git-commit` | 约定式提交消息生成 | ⭐⭐⭐⭐ |
| `git-master` | Git 高级操作 | ⭐⭐⭐⭐ |
| `using-git-worktrees` | Git 并行开发 | ⭐⭐⭐⭐⭐ |

### 代码质量

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `code-review` | 智能代码审查 | ⭐⭐⭐⭐⭐ |
| `api-design-principles` | REST/GraphQL API 设计 | ⭐⭐⭐⭐⭐ |
| `qa-expert` | QA 测试流程建立 | ⭐⭐⭐⭐ |
| `testing-anti-patterns` | 测试反模式检测 | ⭐⭐⭐⭐ |

### 调试 & 排查

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `systematic-debugging` | 四阶段调试框架 | ⭐⭐⭐⭐⭐ |
| `root-cause-tracing` | 根因追溯 | ⭐⭐⭐⭐⭐ |
| `debug-helper` | 调试助手 | ⭐⭐⭐⭐ |
| `verification-before-completion` | 完成前验证 | ⭐⭐⭐⭐ |
| `defense-in-depth` | 多层验证 | ⭐⭐⭐⭐ |

---

## 📝 文档创作类

### 文档生成

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `doc-coauthoring` | 协作文档工作流 | ⭐⭐⭐⭐⭐ |
| `docx` | Word 文档操作 | ⭐⭐⭐⭐⭐ |
| `pdf` | PDF 处理工具 | ⭐⭐⭐⭐⭐ |
| `pdf-creator` | Markdown 转 PDF | ⭐⭐⭐⭐ |
| `pptx` | PowerPoint 处理 | ⭐⭐⭐⭐⭐ |
| `ppt-creator` | 自动生成 PPT | ⭐⭐⭐⭐⭐ |
| `meeting-minutes-taker` | 会议纪要生成 | ⭐⭐⭐⭐⭐ |

### 文档优化

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `docs-cleaner` | 文档去重整合 | ⭐⭐⭐⭐ |
| `fact-checker` | 事实核查工具 | ⭐⭐⭐⭐⭐ |
| `markdown-tools` | Markdown 转换 | ⭐⭐⭐⭐ |
| `humanizer-zh` | 去 AI 味优化 | ⭐⭐⭐⭐⭐ |
| `internal-comms` | 内部沟通文档 | ⭐⭐⭐⭐ |

---

## 🎨 设计创作类

### 视觉设计

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `canvas-design` | 艺术海报设计 | ⭐⭐⭐⭐⭐ |
| `algorithmic-art` | 算法生成艺术 | ⭐⭐⭐⭐ |
| `frontend-design` | 前端界面设计 | ⭐⭐⭐⭐⭐ |
| `ui-designer` | 设计系统提取 | ⭐⭐⭐⭐⭐ |
| `theme-factory` | 主题样式生成 | ⭐⭐⭐⭐ |
| `brand-guidelines` | 品牌视觉规范 | ⭐⭐⭐⭐ |

### 演示 & 视频

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `cli-demo-generator` | CLI 演示生成 | ⭐⭐⭐⭐ |
| `video-comparer` | 视频对比分析 | ⭐⭐⭐⭐ |
| `slack-gif-creator` | Slack GIF 创建 | ⭐⭐⭐ |

---

## 🧠 Agent & 上下文工程

> 来源：`muratcankoylan/Agent-Skills-for-Context-Engineering` | 5k+ Star

### 上下文管理

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `context-fundamentals` | 上下文基础原理 | ⭐⭐⭐⭐⭐ |
| `context-compression` | 上下文压缩技术 | ⭐⭐⭐⭐⭐ |
| `context-optimization` | 上下文优化 | ⭐⭐⭐⭐⭐ |
| `context-degradation` | 上下文降解诊断 | ⭐⭐⭐⭐⭐ |

### Agent 架构

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `multi-agent-patterns` | 多智能体模式 | ⭐⭐⭐⭐⭐ |
| `memory-systems` | 记忆系统实现 | ⭐⭐⭐⭐⭐ |
| `hosted-agents` | 托管 Agent | ⭐⭐⭐⭐ |
| `bdi-mental-states` | BDI 心理状态模型 | ⭐⭐⭐⭐ |
| `tool-design` | 工具设计 | ⭐⭐⭐⭐ |

### 项目开发

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `project-development` | LLM 项目开发 | ⭐⭐⭐⭐⭐ |
| `evaluation` | Agent 评估框架 | ⭐⭐⭐⭐ |
| `advanced-evaluation` | 高级评估技术 | ⭐⭐⭐⭐ |

---

## 🔧 实用工具类

### 技能管理

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `skill-creator` | 技能创建指南 | ⭐⭐⭐⭐⭐ |
| `skill-reviewer` | 技能审查工具 | ⭐⭐⭐⭐ |
| `skills-search` | 技能搜索安装 | ⭐⭐⭐⭐⭐ |
| `statusline-generator` | 状态栏生成 | ⭐⭐⭐⭐ |

### 文件操作

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `repomix-safe-mixer` | 安全打包代码 | ⭐⭐⭐⭐⭐ |
| `repomix-unmixer` | 解包恢复代码 | ⭐⭐⭐⭐ |
| `claude-code-history-files-finder` | 历史文件恢复 | ⭐⭐⭐⭐ |

### 其他实用技能

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `deep-research` | 深度研究报告 | ⭐⭐⭐⭐⭐ |
| `prompt-optimizer` | 提示词优化 | ⭐⭐⭐⭐⭐ |
| `youtube-downloader` | 视频下载 | ⭐⭐⭐⭐ |
| `transcript-fixer` | 转录修正 | ⭐⭐⭐⭐ |
| `fact-checker` | 事实核查 | ⭐⭐⭐⭐⭐ |

---

## 🌐 特定领域技能

### 前端开发

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `frontend-design` | 前端界面设计 | ⭐⭐⭐⭐⭐ |
| `tailwind-design-system` | Tailwind 设计系统 | ⭐⭐⭐⭐⭐ |
| `webapp-testing` | Web 应用测试 | ⭐⭐⭐⭐ |
| `web-artifacts-builder` | Web 工件构建 | ⭐⭐⭐⭐ |
| `web-design-guidelines` | Web 设计指南 | ⭐⭐⭐⭐ |
| `i18n-expert` | 国际化专家 | ⭐⭐⭐⭐ |

### iOS 开发

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `iOS-APP-developer` | iOS 应用开发 | ⭐⭐⭐⭐ |

### 系统运维

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `cloudflare-troubleshooting` | Cloudflare 故障排查 | ⭐⭐⭐⭐ |
| `macos-cleaner` | macOS 清理工具 | ⭐⭐⭐⭐ |

### 数据处理

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `xlsx` | Excel 表格处理 | ⭐⭐⭐⭐⭐ |
| `mermaid-tools` | Mermaid 图表工具 | ⭐⭐⭐⭐ |
| `json-canvas` | JSON Canvas 处理 | ⭐⭐⭐ |

---

## 💡 问题解决技能

### 思维方法

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `when-stuck` | 卡壳时解决方案 | ⭐⭐⭐⭐⭐ |
| `inversion-exercise` | 反向思维练习 | ⭐⭐⭐⭐ |
| `collision-zone-thinking` | 跨界思维创新 | ⭐⭐⭐⭐ |
| `simplification-cascades` | 简化级联 | ⭐⭐⭐⭐ |
| `scale-game` | 极限测试 | ⭐⭐⭐⭐ |
| `meta-pattern-recognition` | 元模式识别 | ⭐⭐⭐⭐ |

### 协作流程

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `preserving-productive-tensions` | 保留建设性分歧 | ⭐⭐⭐⭐ |
| `receiving-code-review` | 接收代码审查反馈 | ⭐⭐⭐⭐⭐ |
| `remembering-conversations` | 搜索历史对话 | ⭐⭐⭐⭐ |

---

## 📚 学习与参考

| 技能 | 功能描述 | 推荐度 |
| --- | --- | --- |
| `tracing-knowledge-lineages` | 追溯知识演变 | ⭐⭐⭐⭐ |
| `pulling-updates-from-skills-repository` | 同步技能更新 | ⭐⭐⭐⭐ |
| `sharing-skills` | 贡献技能到上游 | ⭐⭐⭐⭐ |
| `gardening-skills-wiki` | 维护技能 Wiki | ⭐⭐⭐⭐ |

---

## 🏷️ 技能推荐等级说明

| 等级 | 说明 |
| --- | --- |
| ⭐⭐⭐⭐⭐ | 强烈推荐，日常工作必备 |
| ⭐⭐⭐⭐ | 非常有用，特定场景必备 |
| ⭐⭐⭐ | 有一定帮助，按需使用 |
| ⭐⭐ | 特殊用途，偶尔使用 |

---

## 🔗 相关资源

### 官方文档

+   Claude Skills 官方文档
+   Anthropic Skills 仓库

### 推荐仓库

| 仓库 | Star | 说明 |
| --- | --- | --- |
| `obra/superpowers` | 2k+ | 核心开发工作流 |
| `muratcankoylan/Agent-Skills-for-Context-Engineering` | 5k+ | 上下文工程 |
| `daymade/claude-code-skills` | - | 社区精选 |
| `travisvn/awesome-claude-skills` | 1k+ | 技能列表 |

---

## 📝 使用技巧

### 快速触发

技能会根据描述自动触发，你也可以直接使用 `/技能名` 调用：

```
/brainstorming          # 开始头脑风暴
/writing-plans          # 创建实现计划
/test-driven-development # 开始 TDD 开发
/fact-checker           # 事实核查
/deep-research          # 深度研究
```

### 组合使用

```
1. /brainstorming → 设计想法
2. /writing-plans → 创建计划
3. /test-driven-development → 执行开发
4. /requesting-code-review → 代码审查
```

---

**更新日期**: 2026-02-07

**技能总数**: 111+

**文档版本**: v1.0
