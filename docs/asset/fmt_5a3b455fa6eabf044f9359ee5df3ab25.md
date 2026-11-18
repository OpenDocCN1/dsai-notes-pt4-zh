
1. 社区精选：`obra/superpowers`质量高，可以用
1. 其他第三方：审查代码后再用
1. 最好自己写：针对自己的工作流定制

---

## 2026年最新动态

### Claude Code 2.1的Skills增强（2026年1月）

1月7日发布的Claude Code 2.1对`Skills`做了大幅增强：

Hot Reload：修改`Skill`文件后自动生效，不需要重启。这让迭代开发变得很顺畅。

自动发现：支持从嵌套的 `.claude/skills` 目录自动发现`Skills`。

Hooks支持：可以为`Skills`配置`PreToolUse`、`PostToolUse`、`Stop`等钩子。

进度指示器：执行`Skills`时会显示实时进度。

### 开放标准的影响

2025年12月，Anthropic把`Skills`做成了开放标准（`agentskills.io`）。

已采用的公司：

+ Microsoft
+ OpenAI（`Codex CLI`）
+ Atlassian
+ Figma
+ Cursor
+ GitHub

意义：

+ `Skills`不再是Anthropic独家功能
+ 创建的`Skills`可以跨平台使用
+ 生态系统会加速扩展

### 垂直领域扩展

最近新增了医疗和生命科学领域的`Skills`：

+ `FHIR`开发（医疗数据交换标准）
+ 临床试验协议生成
+ 生物信息学工具集成（`scVI-tools`、`Nextflow`）

这表明`Skills`正在从通用能力向垂直领域深入。
