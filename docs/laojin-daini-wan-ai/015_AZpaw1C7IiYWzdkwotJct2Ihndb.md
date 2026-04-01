# Claude Code 综合实战：企业级最佳实践指南

> 来源：[https://my.feishu.cn/docx/WfdmdkadcoBy6lxyDZucQbgWnnf](https://my.feishu.cn/docx/WfdmdkadcoBy6lxyDZucQbgWnnf)

# Claude Code 综合实战：企业级最佳实践指南

> **课程信息**

+   **作者**：老金

+   **预计学时**：2-3小时

+   **难度等级**：⭐⭐⭐ 进阶

+   **更新日期**：2026年3月

+   **适用版本**：Claude Code v2.1+（验证于2026-03-18）

---

## 📚 本课学习目标

完成本课学习后，你将能够：

1.  **建立团队协作规范**：标准化项目结构、CLAUDE.md规范、代码审查流程

1.  **配置CI/CD集成**：GitHub Actions配置、安全审查、自动化流水线

1.  **实施安全与合规**：权限系统、白名单配置、审计日志、合规检查

1.  **优化性能与成本**：上下文管理、调试技巧、成本控制

---

## 术语表（小白必读）

| 术语 | 英文全称 | 通俗解释 |
| --- | --- | --- |
| **CI/CD** | Continuous Integration/Continuous Deployment | 持续集成/持续部署，自动化代码测试和发布的流程 |
| **GitHub Actions** | - | GitHub提供的自动化工作流服务 |
| **PR** | Pull Request | 代码合并请求，用于代码审查 |
| **MCP** | Model Context Protocol | 模型上下文协议，扩展AI能力的接口标准 |
| **白名单** | Whitelist/Allowlist | 明确允许执行的工具或命令列表 |
| **审计日志** | Audit Log | 记录所有操作的日志，用于安全追踪 |
| **Token** | - | AI处理文字的计费单位 |
| **上下文窗口** | Context Window | AI单次对话能处理的最大信息量 |
| **Skill** | Skill | Claude Code的可复用能力模块（通过SKILL.md定义） |
| **Forked Context** | - | Skills的独立上下文模式，通过context: fork启用，在子代理中运行不影响主会话 |
| **Hot Reload** | - | Skills修改后自动重新加载，无需重启Claude Code |

---

## 目录

1.  团队协作规范

1.  CI/CD集成

1.  安全与合规

1.  性能优化

1.  综合练习

---

## 1. 团队协作规范

### 1.1 为什么需要团队规范

当Claude Code从个人工具演变为团队基础设施时，缺乏统一规范会导致严重问题：

**典型混乱场景**：

+   开发者A的CLAUDE.md有500行自定义规则，开发者B完全没有

+   代码审查时AI生成的代码风格与团队标准完全不同

+   敏感API密钥被AI意外提交到代码仓库

+   不同项目的MCP配置互相冲突导致工具失效

这些问题在3人以下团队可能还能容忍，但团队规模一旦超过5人，没有规范就是灾难的开始。

### 1.2 项目结构标准化

#### 1.2.1 推荐的目录结构

企业级项目应该采用统一的目录结构，让团队成员和AI助手都能快速定位文件：

```
project-root/
├── .claude/                      # Claude Code专用配置
│   ├── settings.json             # 权限和工具配置
│   ├── settings.local.json       # 本地覆盖（不入库）
│   ├── commands/                 # 自定义Slash命令
│   │   ├── 01-dev.md            # 开发相关命令
│   │   ├── 02-test.md           # 测试相关命令
│   │   └── 03-deploy.md         # 部署相关命令
│   ├── hooks/                    # 生命周期钩子
│   │   ├── pre-commit.sh        # 提交前检查
│   │   └── post-review.sh       # 审查后处理
│   └── skills/                   # 技能包
│       └── project-specific/     # 项目特定技能
│           └── SKILL.md          # 技能定义（Markdown格式）
├── .github/                      # GitHub集成
│   ├── workflows/               # CI/CD工作流
│   │   └── claude-review.yml   # Claude自动审查
│   └── CODEOWNERS               # 代码所有者
├── docs/                         # 项目文档
│   └── ai-context/              # AI上下文文档
│       ├── project-structure.md # 项目结构说明
│       ├── coding-standards.md  # 编码规范
│       └── architecture.md      # 架构设计
├── src/                          # 源代码
├── tests/                        # 测试代码
├── CLAUDE.md                     # 主配置文件
├── .mcp.json                     # MCP服务器配置
├── .gitignore                    # Git忽略规则
└── README.md                     # 项目说明
```

#### 1.2.2 目录职责划分

**.claude/**** 目录**：Claude Code的"控制中心"

| 子目录/文件 | 职责 | 入库策略 |
| --- | --- | --- |
| settings.json | 团队统一配置 | ✅ 必须入库 |
| settings.local.json | 个人本地配置 | ❌ 禁止入库 |
| commands/ | 团队共享命令 | ✅ 必须入库 |
| hooks/ | 自动化钩子 | ✅ 必须入库 |
| skills/ | 项目技能包 | ✅ 必须入库 |

**doc**s/ai-context/ 目录：AI理解项目的"说明书"

这个目录专门存放帮助AI理解项目的文档，不是给人看的README，而是给AI看的上下文：

```
# docs/ai-context/project-structure.md 示例

## 项目技术栈
- 前端：React 18 + TypeScript 5.0 + Vite 5
- 后端：Node.js 20 + Fastify 4
- 数据库：PostgreSQL 15 + Prisma ORM
- 缓存：Redis 7
- 部署：Docker + Kubernetes

## 核心模块
### 用户模块 (src/modules/user/)
- 负责用户注册、登录、权限管理
- 依赖：JWT认证、bcrypt加密

### 订单模块 (src/modules/order/)
- 负责订单创建、支付、状态管理
- 依赖：用户模块、支付网关

## 代码生成约定
- 所有API响应使用统一格式：{ data, error, meta }
- 数据库操作必须使用Prisma Client
- 所有日期时间使用UTC时区
```

#### 1.2.3 命名规范

**命令文件命名**：{序号}-{功能域}.md

序号规则：

+   00-09：基础设施命令（help、setup）

+   10-19：开发命令（dev、build）

+   20-29：测试命令（test、lint）

+   30-39：部署命令（deploy、release）

+   40-49：数据命令（migrate、seed）

+   90-99：工具命令（debug、monitor）

**技能包命名**：{项目名}-{功能}

示例：

+   ecommerce-checkout：电商结算流程

+   cms-content-workflow：CMS内容工作流

+   analytics-report-generator：分析报告生成器

### 1.3 CLAUDE.md规范

#### 1.3.1 CLAUDE.md层级结构

Claude Code支持三层配置，优先级从低到高：

1.  **全局配置** (~/.claude/CLAUDE.md)

+   适用于所有项目

+   存放个人偏好、通用规则

1.  **项目配置** (项目根目录/CLAUDE.md)

+   团队共享，入库管理

+   存放项目特定规则、技术栈说明

1.  **子目录配置** (子目录/CLAUDE.md)

+   模块级别的特殊规则

+   例如：src/legacy/CLAUDE.md 存放遗留代码的特殊处理规则

#### 1.3.2 项目CLAUDE.md模板

```
# [项目名称] - Claude Code配置

## 1. 项目概览
- **项目描述**：[一句话描述项目用途]
- **技术栈**：[主要技术栈列表]
- **当前阶段**：[开发/测试/生产]

## 2. 代码规范
### 通用规则
- 所有代码必须有类型注解
- 函数不超过50行，类不超过300行
- 禁止使用any类型（特殊情况需注释说明）

### 命名约定
- 文件名：kebab-case（如 user-service.ts）
- 类名：PascalCase（如 UserService）
- 函数/变量：camelCase（如 getUserById）
- 常量：UPPER_SNAKE_CASE（如 MAX_RETRY_COUNT）

### 文档要求
- 所有公共API必须有JSDoc/TSDoc注释
- 复杂业务逻辑必须有流程说明
- 使用中文注释，代码用英文

## 3. 安全规则
### 禁止行为
- 禁止在代码中硬编码敏感信息
- 禁止提交.env文件到仓库
- 禁止在日志中输出用户隐私数据

### 必须行为
- 所有输入必须验证和消毒
- 数据库查询必须使用参数化
- API必须有速率限制

## 4. 测试要求
- 新功能必须有单元测试
- 核心逻辑测试覆盖率&gt;80%
- 集成测试必须覆盖主要用户流程

## 5. Git规范
### 分支命名
- feature/xxx：新功能
- fix/xxx：bug修复
- refactor/xxx：重构
- docs/xxx：文档更新

### 提交信息
格式：`&lt;type&gt;(&lt;scope&gt;): &lt;description&gt;`

类型：feat、fix、docs、style、refactor、test、chore

## 6. 项目特殊说明
[项目特有的规则和注意事项]
```

#### 1.3.3 全局CLAUDE.md模板

```
# 全局Claude Code配置

## 个人偏好
- 使用中文回复
- 代码注释使用中文
- 偏好简洁的代码风格

## 通用安全规则
- 永远不要在代码中包含真实的API密钥
- 敏感操作需要二次确认
- 不自动执行rm -rf或DROP TABLE等危险命令

## 工具偏好
- Git操作：优先使用命令行而非GUI
- 代码格式化：保存时自动格式化
- 测试：修改代码后自动运行相关测试
```

### 1.4 代码审查流程

#### 1.4.1 AI辅助代码审查流程

```
┌─────────────────────────────────────────────────────────────┐
│                    AI辅助代码审查流程                         │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  开发者提交PR                                                │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────┐                                           │
│  │ CI触发自动   │                                           │
│  │ Claude审查   │                                           │
│  └─────────────┘                                           │
│       │                                                     │
│       ▼                                                     │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Claude Code审查内容：                                │   │
│  │ 1. 代码风格是否符合CLAUDE.md规范                     │   │
│  │ 2. 是否有潜在的安全漏洞                              │   │
│  │ 3. 是否有性能问题                                    │   │
│  │ 4. 测试覆盖是否充分                                  │   │
│  │ 5. 文档是否完整                                      │   │
│  └─────────────────────────────────────────────────────┘   │
│       │                                                     │
│       ▼                                                     │
│  自动添加审查评论到PR                                        │
│       │                                                     │
│       ▼                                                     │
│  人工审查员复核                                              │
│       │                                                     │
│       ▼                                                     │
│  合并或请求修改                                              │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### 1.4.2 代码审查Slash命令

创建 .claude/commands/code-review.md：

```
name: code-review
description: AI代码审查命令

# 代码审查

请对以下代码变更进行审查：

## 审查维度
### 1. 代码质量
- 代码是否清晰可读
- 命名是否表意
- 是否有重复代码
- 函数/类是否过长

### 2. 安全性
- 输入验证是否充分
- 是否有SQL注入风险
- 是否有XSS风险
- 敏感数据处理是否安全

### 3. 性能
- 是否有N+1查询问题
- 循环内是否有不必要的计算
- 是否使用了适当的数据结构

### 4. 测试
- 是否有对应的测试
- 测试覆盖是否充分
- 边界情况是否考虑

### 5. 文档
- 公共API是否有文档
- 复杂逻辑是否有注释
- README是否需要更新

## 输出格式
## 审查结果
### 必须修改 (Blocking)
- [问题描述]
  - 位置：[文件:行号]
  - 建议：[修改建议]

### 建议修改 (Suggestion)
- [问题描述]
  - 位置：[文件:行号]
  - 建议：[修改建议]

### 表扬 (Praise)
- [做得好的地方]

请开始审查...
```

#### 1.4.3 审查清单

| 检查项 | 通过条件 | 优先级 |
| --- | --- | --- |
| 代码风格 | 符合CLAUDE.md规范 | P0 |
| 安全检查 | 无高危漏洞 | P0 |
| 测试覆盖 | 新代码有测试 | P1 |
| 文档完整 | API有注释 | P1 |
| 性能考量 | 无明显性能问题 | P2 |

### 1.5 团队协作最佳实践

#### 1.5.1 配置同步策略

**入库配置（团队共享）**：

```
# .gitignore 中不要忽略这些
!.claude/
!.claude/settings.json
!.claude/commands/
!.claude/hooks/
!.claude/skills/
!CLAUDE.md
!.mcp.json
```

**不入库配置（个人本地）**：

```
# .gitignore 中要忽略这些
.claude/settings.local.json
.claude/*.local.*
.env
.env.local
```

#### 1.5.2 配置冲突解决

当团队配置与个人偏好冲突时，使用覆盖机制：

```
// .claude/settings.local.json（不入库，个人偏好覆盖）
{
  &quot;permissions&quot;: {
    &quot;allow&quot;: [
      &quot;Bash(npm run dev)&quot;,
      &quot;Bash(npm run test)&quot;
    ]
  }
}
```

#### 1.5.3 新成员入职流程

```
## Claude Code新成员入职清单

### 第1步：环境准备
- [ ] 安装Claude Code CLI
- [ ] 配置全局CLAUDE.md
- [ ] 获取API密钥

### 第2步：项目配置
- [ ] 克隆项目仓库
- [ ] 运行 `claude` 初始化
- [ ] 检查MCP服务器是否正常

### 第3步：熟悉规范
- [ ] 阅读项目CLAUDE.md
- [ ] 运行 `/help` 查看可用命令
- [ ] 尝试运行一次代码审查

### 第4步：验证配置
- [ ] 运行测试命令确认配置正确
- [ ] 提交一个测试PR验证CI流程
```

---

## 2. CI/CD集成

### 2.1 GitHub Actions集成概述

Claude Code可以深度集成到GitHub Actions中，实现：

+   自动代码审查

+   PR评论交互

+   安全扫描

+   文档生成

#### 2.1.1 官方Action介绍

Anthropic提供了官方的GitHub Action：anthropics/claude-code-action

**主要功能**：

+   在PR上自动运行Claude Code审查

+   响应Issue评论中的指令

+   执行自定义命令

### 2.2 GitHub Actions配置详解

#### 2.2.1 基础配置

创建 .github/workflows/claude-review.yml：

```
name: Claude Code Review

on:
  pull_request:
    types: [opened, synchronize, reopened]
  issue_comment:
    types: [created]

# 权限配置：授予必要的GitHub权限
permissions:
  contents: read
  pull-requests: write
  issues: write

jobs:
  claude-review:
    # 条件：PR事件 或 Issue评论中包含@claude
    if: |
      github.event_name == &#x27;pull_request&#x27; ||
      (github.event_name == &#x27;issue_comment&#x27; &amp;&amp;
       contains(github.event.comment.body, &#x27;@claude&#x27;))

    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        with:
          fetch-depth: 0  # 获取完整历史，用于diff比较

      - name: Run Claude Code Review
        uses: anthropics/claude-code-action@v1
        with:
          # API密钥（必须在仓库Secrets中配置）
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}

          # 可选：指定模型
          model: &quot;claude-sonnet-4-6&quot;

          # 可选：最大token数
          max_tokens: 4096

          # 可选：超时时间（秒）
          timeout: 300
```

#### 2.2.2 高级配置：多场景工作流

```
name: Claude Code CI/CD Integration

on:
  pull_request:
    types: [opened, synchronize, reopened]
  issue_comment:
    types: [created]
  push:
    branches: [main, develop]

permissions:
  contents: read
  pull-requests: write
  issues: write
  actions: read

env:
  # 共享环境变量
  CLAUDE_MODEL: claude-sonnet-4-6

jobs:
  # ========== PR代码审查 ==========
  review:
    name: Code Review
    if: github.event_name == &#x27;pull_request&#x27;
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Get changed files
        id: changed-files
        uses: tj-actions/changed-files@v40

      - name: Claude Code Review
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          model: ${{ env.CLAUDE_MODEL }}
          prompt: |
            请审查以下代码变更：

            变更文件：${{ steps.changed-files.outputs.all_changed_files }}

            审查要点：
            1. 代码质量和可维护性
            2. 潜在的安全问题
            3. 性能考量
            4. 测试覆盖建议

            请使用中文回复，格式化输出审查结果。

  # ========== 安全扫描 ==========
  security-scan:
    name: Security Scan
    if: github.event_name == &#x27;pull_request&#x27;
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Claude Security Scan
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          model: ${{ env.CLAUDE_MODEL }}
          prompt: |
            请对本次PR进行安全扫描，检查：

            1. 硬编码的敏感信息（API密钥、密码等）
            2. SQL注入漏洞
            3. XSS漏洞
            4. 不安全的依赖
            5. 权限配置问题

            如果发现问题，请标记为 [SECURITY] 并说明风险等级。

  # ========== 交互式命令处理 ==========
  interactive:
    name: Interactive Commands
    if: |
      github.event_name == &#x27;issue_comment&#x27; &amp;&amp;
      contains(github.event.comment.body, &#x27;@claude&#x27;)
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Process Claude Command
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          model: ${{ env.CLAUDE_MODEL }}
          # 从评论中提取命令
          prompt: ${{ github.event.comment.body }}

  # ========== 文档生成（push到main时） ==========
  docs:
    name: Generate Documentation
    if: github.event_name == &#x27;push&#x27; &amp;&amp; github.ref == &#x27;refs/heads/main&#x27;
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Generate API Docs
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          model: ${{ env.CLAUDE_MODEL }}
          prompt: |
            请扫描src/目录下的代码，生成API文档。

            文档格式要求：
            1. Markdown格式
            2. 包含函数签名、参数说明、返回值
            3. 包含使用示例

            输出到docs/api/目录。
```

#### 2.2.3 配置Secrets

在GitHub仓库设置中添加以下Secrets：

| Secret名称 | 说明 | 获取方式 |
| --- | --- | --- |
| ANTHROPIC_API_KEY | Anthropic API密钥 | console.anthropic.com |
| GITHUB_TOKEN | GitHub Token | 自动提供（默认） |

配置路径：Settings > Secrets and variables > Actions > New repository secret

### 2.3 /security-review命令

#### 2.3.1 创建安全审查命令

创建 .claude/commands/security-review.md：

```
name: security-review
description: 执行安全代码审查

# 安全代码审查

## 审查范围
对指定文件或整个项目进行安全审查。

## 检查清单
### 1. 认证与授权
- [ ] 密码存储是否使用安全哈希（bcrypt/argon2）
- [ ] JWT密钥是否足够复杂
- [ ] 会话管理是否安全
- [ ] 权限检查是否完整

### 2. 输入验证
- [ ] 所有用户输入是否验证
- [ ] 是否防止SQL注入
- [ ] 是否防止XSS攻击
- [ ] 是否防止命令注入

### 3. 敏感数据
- [ ] API密钥是否硬编码
- [ ] 数据库凭据是否安全存储
- [ ] 日志是否泄露敏感信息
- [ ] 错误信息是否泄露内部细节

### 4. 配置安全
- [ ] HTTPS是否强制
- [ ] CORS是否正确配置
- [ ] 安全头是否设置
- [ ] Cookie是否安全配置

### 5. 依赖安全
- [ ] 是否有已知漏洞的依赖
- [ ] 依赖版本是否及时更新
- [ ] 是否使用可信的包源

## 输出格式
# 安全审查报告

## 概要
- 审查时间：[时间]
- 审查范围：[范围]
- 风险等级：[高/中/低]

## 发现的问题
### 高危 (Critical)
**问题**：...
**位置**：...
**描述**：...
**修复建议**：...

### 中危 (Medium)
**问题**：...

### 低危 (Low)
**问题**：...

## 最佳实践建议
[改进建议]

## 执行
请开始安全审查...
```

#### 2.3.2 在CI中集成安全审查

```
# .github/workflows/security.yml
name: Security Review

on:
  pull_request:
    paths:
      - &#x27;src/**&#x27;
      - &#x27;package.json&#x27;
      - &#x27;package-lock.json&#x27;

jobs:
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Security Review
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          command: &quot;/security-review&quot;

      - name: Check for Critical Issues
        run: |
          # 解析审查结果，如有高危问题则失败
          if grep -q &quot;Critical&quot; claude-review-output.md; then
            echo &quot;::error::发现高危安全问题，请修复后重新提交&quot;
            exit 1
          fi
```

### 2.4 /install-github-app命令

#### 2.4.1 GitHub App安装指南

Claude Code可以作为GitHub App安装到组织或仓库，提供更深度的集成：

```
## GitHub App安装步骤

### 步骤1：访问安装页面
运行命令：/install-github-app

或直接访问：https://github.com/apps/claude-code

### 步骤2：选择安装范围
- 组织级别：应用于组织下所有仓库
- 仓库级别：只应用于选定的仓库

### 步骤3：配置权限
推荐权限配置：
- Contents: Read
- Pull requests: Read &amp; Write
- Issues: Read &amp; Write
- Metadata: Read

### 步骤4：配置Webhook（可选）
- Webhook URL: 你的服务器地址
- Events: Pull request, Issue comment, Push
```

#### 2.4.2 App vs Action对比

| 特性 | GitHub Action | GitHub App |
| --- | --- | --- |
| 安装复杂度 | 低（配置文件） | 中（OAuth流程） |
| 实时响应 | 否（需要触发） | 是（Webhook） |
| 跨仓库 | 否 | 是 |
| 持久化状态 | 否 | 是 |
| 适用场景 | CI/CD集成 | 深度平台集成 |

### 2.5 完整CI/CD流水线示例

#### 2.5.1 多阶段流水线

```
# .github/workflows/full-pipeline.yml
name: Full CI/CD Pipeline with Claude

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  NODE_VERSION: &#x27;20&#x27;
  CLAUDE_MODEL: claude-sonnet-4-6

jobs:
  # ===== 阶段1：代码检查 =====
  lint:
    name: Lint &amp; Format
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
      - run: npm ci
      - run: npm run lint
      - run: npm run format:check

  # ===== 阶段2：单元测试 =====
  test:
    name: Unit Tests
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
      - run: npm ci
      - run: npm test -- --coverage
      - uses: codecov/codecov-action@v3

  # ===== 阶段3：Claude代码审查（仅PR） =====
  claude-review:
    name: Claude Code Review
    if: github.event_name == &#x27;pull_request&#x27;
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Get PR Diff
        id: diff
        run: |
          git diff origin/main...HEAD &gt; pr_diff.txt
          echo &quot;diff_size=$(wc -l &lt; pr_diff.txt)&quot; &gt;&gt; $GITHUB_OUTPUT

      - name: Claude Review
        if: steps.diff.outputs.diff_size &gt; 0
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          model: ${{ env.CLAUDE_MODEL }}
          prompt: |
            请审查这个PR的代码变更：

            1. 代码质量评估
            2. 潜在bug分析
            3. 性能建议
            4. 安全检查

            请给出具体的改进建议。

  # ===== 阶段4：安全扫描 =====
  security:
    name: Security Scan
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4

      - name: Run npm audit
        run: npm audit --audit-level=high
        continue-on-error: true

      - name: Claude Security Scan
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          command: &quot;/security-review&quot;

  # ===== 阶段5：构建 =====
  build:
    name: Build
    runs-on: ubuntu-latest
    needs: [test, security]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/

  # ===== 阶段6：部署到Staging（仅develop分支） =====
  deploy-staging:
    name: Deploy to Staging
    if: github.ref == &#x27;refs/heads/develop&#x27;
    runs-on: ubuntu-latest
    needs: build
    environment: staging
    steps:
      - name: Download build
        uses: actions/download-artifact@v4
        with:
          name: build-output
      - name: Deploy to Staging
        run: |
          echo &quot;Deploying to staging...&quot;
          # 部署脚本

  # ===== 阶段7：部署到Production（仅main分支） =====
  deploy-production:
    name: Deploy to Production
    if: github.ref == &#x27;refs/heads/main&#x27;
    runs-on: ubuntu-latest
    needs: build
    environment: production
    steps:
      - name: Download build
        uses: actions/download-artifact@v4
        with:
          name: build-output
      - name: Deploy to Production
        run: |
          echo &quot;Deploying to production...&quot;
          # 部署脚本

      - name: Claude Deployment Report
        uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            部署已完成，请生成部署报告：
            - 版本：${{ github.sha }}
            - 分支：${{ github.ref }}
            - 触发者：${{ github.actor }}
```

---

## 3. 安全与合规

### 3.1 权限系统详解

Claude Code采用三级权限模型：Allow（允许）、Ask（询问）、Deny（拒绝）

#### 3.1.1 权限级别说明

**Allow（允许）**：

+   工具可以直接执行，无需确认

+   适用于低风险、频繁使用的操作

+   示例：读取文件、搜索代码

**Ask（询问）**：

+   执行前需要用户确认

+   适用于有一定风险的操作

+   示例：修改文件、执行命令

**Deny（拒绝）**：

+   完全禁止执行

+   适用于高风险或违反政策的操作

+   示例：访问敏感目录、执行危险命令

#### 3.1.2 权限配置文件

```
// .claude/settings.json
{
  &quot;permissions&quot;: {
    // 允许的工具和命令（无需确认直接执行）
    &quot;allow&quot;: [
      &quot;Read&quot;,                    // 读取文件：直接允许
      &quot;Glob&quot;,                    // 文件搜索：直接允许
      &quot;Grep&quot;,                    // 内容搜索：直接允许
      &quot;WebFetch&quot;,                // 网络请求：直接允许
      &quot;WebSearch&quot;,               // 网络搜索：直接允许
      &quot;Bash(npm test *)&quot;,        // 允许运行测试
      &quot;Bash(git diff *)&quot;,        // 允许查看差异
      &quot;Bash(git log *)&quot;          // 允许查看日志
    ],

    // 拒绝的工具和命令（完全禁止执行）
    &quot;deny&quot;: [
      &quot;Bash(rm -rf /)&quot;,          // 禁止删除根目录
      &quot;Bash(rm -rf ~)&quot;,          // 禁止删除用户目录
      &quot;Read(./.env)&quot;,            // 禁止读取环境变量文件
      &quot;Read(./secrets/**)&quot;,      // 禁止读取敏感目录
      &quot;Bash(git push --force)&quot;,  // 禁止强制推送
      &quot;Bash(git reset --hard)&quot;,  // 禁止硬重置
      &quot;Bash(sudo *)&quot;,            // 禁止sudo命令
      &quot;Bash(curl * | bash)&quot;     // 禁止远程脚本执行
    ]
  }
}
```

> **说明**：permissions 采用扁平的 allow / deny 列表结构。未出现在任一列表中的工具和命令默认需要用户确认（ask）。deny 列表中的条目支持通配符匹配，可精确控制危险操作。

#### 3.1.3 运行时权限覆盖

```
# 临时放宽权限（单次会话）
claude --dangerously-skip-permissions

# 临时使用计划模式（需审批后才执行）
claude --permission-mode plan

# 查看当前配置（包括权限）
claude config list
# 或在REPL中使用 /permissions 查看权限详情
```

#### 3.1.4 Sandbox沙箱配置详解

Claude Code支持通过/sandbox命令启用OS级沙箱隔离，将AI的文件系统访问限制在安全范围内。沙箱启用后，可在settings.json中配置详细的文件系统权限：

```
// .claude/settings.json
{
  &quot;sandbox&quot;: {
    &quot;filesystem&quot;: {
      &quot;allowWrite&quot;: [&quot;/tmp&quot;, &quot;./src&quot;],
      &quot;allowRead&quot;: [&quot;./&quot;, &quot;/usr/lib&quot;],
      &quot;denyRead&quot;: [&quot;.env&quot;, &quot;credentials.json&quot;]
    }
  }
}
```

**权限字段说明**：

| 字段 | 作用 | 示例 |
| --- | --- | --- |
| allowWrite | 允许写入的路径列表 | [&quot;/tmp&quot;, &quot;./src&quot;, &quot;./tests&quot;] |
| allowRead | 允许读取的路径列表 | [&quot;./&quot;, &quot;/usr/lib&quot;] |
| denyRead | 明确禁止读取的路径（**优先级最高**） | [&quot;.env&quot;, &quot;credentials.json&quot;, &quot;./secrets/**&quot;] |

> **注意**：denyRead的优先级高于allowRead。即使allowRead中包含了项目根目录"./"，denyRead中列出的文件仍会被禁止读取。建议将所有敏感配置文件（如.env、密钥文件）加入denyRead列表。

### 3.2 allowedTools白名单

#### 3.2.1 白名单配置

```
// .claude/settings.json
{
  &quot;allowedTools&quot;: [
    // 基础工具
    &quot;Read&quot;,
    &quot;Glob&quot;,
    &quot;Grep&quot;,

    // 编辑工具（带路径限制）
    &quot;Edit(src/**)&quot;,
    &quot;Write(src/**)&quot;,
    &quot;Write(tests/**)&quot;,
    &quot;Write(docs/**)&quot;,

    // Bash命令（带命令限制）
    &quot;Bash(npm *)&quot;,
    &quot;Bash(node *)&quot;,
    &quot;Bash(git status)&quot;,
    &quot;Bash(git diff *)&quot;,
    &quot;Bash(git add *)&quot;,
    &quot;Bash(git commit *)&quot;,
    &quot;Bash(npx *)&quot;,

    // 通配符权限支持
    &quot;Bash(*-h*)&quot;,        // 允许所有-h帮助命令
    &quot;Bash(*--help)&quot;,     // 允许所有--help命令

    // MCP工具
    &quot;mcp__context7__*&quot;,
    &quot;mcp__exa__*&quot;
  ],

  // 明确禁止的工具
  &quot;disallowedTools&quot;: [
    &quot;Bash(rm -rf *)&quot;,
    &quot;Bash(sudo *)&quot;,
    &quot;Bash(curl * | bash)&quot;,
    &quot;Write(.env*)&quot;,
    &quot;Write(**/secrets/*)&quot;
  ]
}
```

#### 3.2.2 白名单模式详解

**精确匹配**：

```
&quot;Read&quot;  // 只允许Read工具
```

**通配符匹配**：

```
&quot;Bash(npm *)&quot;  // 允许所有npm命令
&quot;Edit(src/**)&quot;  // 允许编辑src目录下所有文件
```

**MCP工具匹配**：

```
&quot;mcp__context7__*&quot;  // 允许context7的所有功能
&quot;mcp__exa__web_search_exa&quot;  // 只允许exa的搜索功能
```

#### 3.2.3 分环境配置

Claude Code使用三级配置层次，而非环境特定文件：

```
// 1. 全局配置（宽松，个人开发使用）
// ~/.claude/settings.json
{
  &quot;permissions&quot;: {
    &quot;allow&quot;: [
      &quot;Read&quot;,
      &quot;Write&quot;,
      &quot;Edit&quot;,
      &quot;Bash(*)&quot;
    ],
    &quot;deny&quot;: [
      &quot;Bash(rm -rf /)&quot;,
      &quot;Bash(sudo *)&quot;
    ]
  }
}

// 2. 项目配置（团队共享，提交到Git）
// .claude/settings.json
{
  &quot;permissions&quot;: {
    &quot;allow&quot;: [
      &quot;Read&quot;,
      &quot;Glob&quot;,
      &quot;Grep&quot;,
      &quot;Bash(npm test)&quot;,
      &quot;Bash(npm run lint)&quot;
    ]
  }
}

// 3. 本地配置（个人覆盖，不提交到Git）
// .claude/settings.local.json
{
  &quot;permissions&quot;: {
    &quot;allow&quot;: [
      &quot;Bash(npm run dev)&quot;
    ]
  }
}
```

> **注意**：不存在settings.development.json或settings.production.json等环境特定文件。通过.claude/settings.json（共享）和.claude/settings.local.json（个人）的组合实现配置差异化。

### 3.3 MCP服务器安全管理

#### 3.3.1 MCP配置规范

MCP服务器在.mcp.json中配置，标准字段只有command、args和env：

```
// .mcp.json — 标准MCP配置格式
{
  &quot;mcpServers&quot;: {
    // 内部API服务器
    &quot;internal-api&quot;: {
      &quot;command&quot;: &quot;node&quot;,
      &quot;args&quot;: [&quot;./mcp-servers/internal-api/index.js&quot;],
      &quot;env&quot;: {
        &quot;API_KEY&quot;: &quot;${INTERNAL_API_KEY}&quot;
      }
    },

    // 第三方文档服务器
    &quot;context7&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;@context7/mcp-server&quot;]
    },

    // 外部工具
    &quot;external-tool&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;external-mcp-tool&quot;],
      &quot;env&quot;: {
        &quot;TIMEOUT&quot;: &quot;30000&quot;
      }
    }
  }
}
```

> **注意**：MCP服务器的信任管理通过Claude Code的权限系统（settings.json的permissions）控制，而非MCP配置本身。

#### 3.3.2 通过权限系统管理MCP安全

使用settings.json的权限规则控制MCP工具的使用范围：

```
// .claude/settings.json
{
  &quot;permissions&quot;: {
    &quot;allow&quot;: [
      // 只允许特定MCP工具
      &quot;mcp__context7__get_library_docs&quot;,
      &quot;mcp__context7__resolve_library_id&quot;
    ],
    &quot;deny&quot;: [
      // 禁止危险的MCP操作
      &quot;mcp__*__delete_*&quot;,
      &quot;mcp__*__drop_*&quot;
    ]
  }
}
```

#### 3.3.3 MCP安全最佳实践

**环境变量隔离**：

```
{
  &quot;mcpServers&quot;: {
    &quot;my-server&quot;: {
      &quot;command&quot;: &quot;node&quot;,
      &quot;args&quot;: [&quot;server.js&quot;],
      &quot;env&quot;: {
        // 使用环境变量引用，不硬编码
        &quot;API_KEY&quot;: &quot;${MCP_API_KEY}&quot;,
        &quot;DB_URL&quot;: &quot;${MCP_DB_URL}&quot;
      }
    }
  }
}
```

**通过Hooks监控MCP调用**：

MCP服务器本身没有内置的网络隔离或资源限制配置。但可以通过Hooks系统监控和控制MCP工具调用：

```
// .claude/settings.json
{
  &quot;hooks&quot;: {
    &quot;PreToolUse&quot;: [
      {
        &quot;matcher&quot;: &quot;mcp__*&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;echo \&quot;[MCP调用] $(date): $CLAUDE_TOOL_NAME\&quot; &gt;&gt; ./logs/mcp-calls.log&quot;
          }
        ]
      }
    ]
  }
}
```

**安全建议**：

+   定期审查.mcp.json中配置的服务器列表

+   使用permissions.deny禁止MCP服务器中的危险操作

+   通过环境变量传递所有敏感凭证，不要硬编码

+   使用Hooks记录MCP工具调用日志

### 3.4 审计日志

#### 3.4.1 通过Hooks实现审计日志

Claude Code没有内置的审计日志配置块，但可以通过Hooks系统实现操作审计：

```
// .claude/settings.json
{
  &quot;hooks&quot;: {
    &quot;PostToolUse&quot;: [
      {
        &quot;matcher&quot;: &quot;Bash&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;echo &#x27;{\&quot;timestamp\&quot;:\&quot;&#x27;$(date -Iseconds)&#x27;\&quot;,\&quot;tool\&quot;:\&quot;Bash\&quot;,\&quot;exit_code\&quot;:\&quot;&#x27;$CLAUDE_TOOL_EXIT_CODE&#x27;\&quot;}&#x27; &gt;&gt; ./logs/claude-audit.jsonl&quot;
          }
        ]
      },
      {
        &quot;matcher&quot;: &quot;Write&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;echo &#x27;{\&quot;timestamp\&quot;:\&quot;&#x27;$(date -Iseconds)&#x27;\&quot;,\&quot;tool\&quot;:\&quot;Write\&quot;,\&quot;file\&quot;:\&quot;&#x27;$CLAUDE_FILE_PATHS&#x27;\&quot;}&#x27; &gt;&gt; ./logs/claude-audit.jsonl&quot;
          }
        ]
      }
    ]
  }
}
```

> **说明**：Hooks的环境变量（如$CLAUDE_TOOL_NAME、$CLAUDE_FILE_PATHS等）由Claude Code自动注入，可用于记录操作上下文。

#### 3.4.2 审计日志格式

```
// logs/claude-audit/2025-01-15.json
{
  &quot;entries&quot;: [
    {
      &quot;timestamp&quot;: &quot;2025-01-15T10:30:45.123Z&quot;,
      &quot;sessionId&quot;: &quot;sess_abc123&quot;,
      &quot;userId&quot;: &quot;user@company.com&quot;,
      &quot;event&quot;: &quot;tool_use&quot;,
      &quot;tool&quot;: &quot;Bash&quot;,
      &quot;command&quot;: &quot;npm test&quot;,
      &quot;status&quot;: &quot;success&quot;,
      &quot;duration&quot;: 5432,
      &quot;context&quot;: {
        &quot;workingDir&quot;: &quot;/project&quot;,
        &quot;branch&quot;: &quot;feature/auth&quot;
      }
    },
    {
      &quot;timestamp&quot;: &quot;2025-01-15T10:31:00.456Z&quot;,
      &quot;sessionId&quot;: &quot;sess_abc123&quot;,
      &quot;userId&quot;: &quot;user@company.com&quot;,
      &quot;event&quot;: &quot;file_access&quot;,
      &quot;tool&quot;: &quot;Edit&quot;,
      &quot;path&quot;: &quot;src/auth/login.ts&quot;,
      &quot;action&quot;: &quot;modify&quot;,
      &quot;status&quot;: &quot;success&quot;,
      &quot;diff&quot;: {
        &quot;linesAdded&quot;: 15,
        &quot;linesRemoved&quot;: 3
      }
    },
    {
      &quot;timestamp&quot;: &quot;2025-01-15T10:32:00.789Z&quot;,
      &quot;sessionId&quot;: &quot;sess_abc123&quot;,
      &quot;userId&quot;: &quot;user@company.com&quot;,
      &quot;event&quot;: &quot;permission_denied&quot;,
      &quot;tool&quot;: &quot;Read&quot;,
      &quot;path&quot;: &quot;.env.production&quot;,
      &quot;reason&quot;: &quot;Path in denied list&quot;,
      &quot;status&quot;: &quot;blocked&quot;
    }
  ]
}
```

#### 3.4.3 审计日志分析

创建审计分析脚本：

```
#!/usr/bin/env python3
# scripts/analyze-audit.py

import json
from pathlib import Path
from collections import Counter
from datetime import datetime, timedelta

def analyze_audit_logs(log_dir: str, days: int = 7):
    &quot;&quot;&quot;分析最近N天的审计日志&quot;&quot;&quot;

    log_path = Path(log_dir)
    cutoff = datetime.now() - timedelta(days=days)

    stats = {
        &quot;total_events&quot;: 0,
        &quot;tool_usage&quot;: Counter(),
        &quot;denied_access&quot;: [],
        &quot;errors&quot;: [],
        &quot;users&quot;: Counter(),
        &quot;sensitive_access&quot;: []
    }

    for log_file in log_path.glob(&quot;*.json&quot;):
        try:
            file_date = datetime.strptime(log_file.stem, &quot;%Y-%m-%d&quot;)
            if file_date &lt; cutoff:
                continue

            with open(log_file) as f:
                data = json.load(f)

            for entry in data.get(&quot;entries&quot;, []):
                stats[&quot;total_events&quot;] += 1
                stats[&quot;tool_usage&quot;][entry.get(&quot;tool&quot;, &quot;unknown&quot;)] += 1
                stats[&quot;users&quot;][entry.get(&quot;userId&quot;, &quot;unknown&quot;)] += 1

                if entry.get(&quot;status&quot;) == &quot;blocked&quot;:
                    stats[&quot;denied_access&quot;].append(entry)

                if entry.get(&quot;event&quot;) == &quot;error&quot;:
                    stats[&quot;errors&quot;].append(entry)

        except Exception as e:
            print(f&quot;Error processing {log_file}: {e}&quot;)

    return stats

def generate_report(stats: dict) -&gt; str:
    &quot;&quot;&quot;生成审计报告&quot;&quot;&quot;

    report = []
    report.append(&quot;=&quot; * 60)
    report.append(&quot;Claude Code 审计报告&quot;)
    report.append(&quot;=&quot; * 60)
    report.append(&quot;&quot;)

    report.append(f&quot;总事件数：{stats[&#x27;total_events&#x27;]}&quot;)
    report.append(&quot;&quot;)

    report.append(&quot;工具使用统计：&quot;)
    for tool, count in stats[&quot;tool_usage&quot;].most_common(10):
        report.append(f&quot;  - {tool}: {count}次&quot;)
    report.append(&quot;&quot;)

    report.append(&quot;用户活动统计：&quot;)
    for user, count in stats[&quot;users&quot;].most_common(10):
        report.append(f&quot;  - {user}: {count}次&quot;)
    report.append(&quot;&quot;)

    if stats[&quot;denied_access&quot;]:
        report.append(f&quot;权限拒绝事件：{len(stats[&#x27;denied_access&#x27;])}次&quot;)
        for entry in stats[&quot;denied_access&quot;][:5]:
            report.append(f&quot;  - {entry[&#x27;timestamp&#x27;]}: {entry.get(&#x27;path&#x27;, &#x27;N/A&#x27;)}&quot;)
    report.append(&quot;&quot;)

    if stats[&quot;errors&quot;]:
        report.append(f&quot;错误事件：{len(stats[&#x27;errors&#x27;])}次&quot;)
        for entry in stats[&quot;errors&quot;][:5]:
            report.append(f&quot;  - {entry[&#x27;timestamp&#x27;]}: {entry.get(&#x27;message&#x27;, &#x27;N/A&#x27;)}&quot;)

    return &quot;\n&quot;.join(report)

if __name__ == &quot;__main__&quot;:
    import sys
    log_dir = sys.argv[1] if len(sys.argv) &gt; 1 else &quot;./logs/claude-audit/&quot;
    stats = analyze_audit_logs(log_dir)
    print(generate_report(stats))
```

### 3.5 合规性检查清单

#### 3.5.1 数据保护合规

```
## GDPR/个人信息保护合规检查

### 数据收集
- [ ] Claude Code不收集用户个人数据
- [ ] 会话数据不持久化到第三方服务器
- [ ] API密钥通过环境变量传递，不硬编码

### 数据处理
- [ ] 代码审查不上传源代码到公网
- [ ] 敏感文件在deny列表中
- [ ] 审计日志中脱敏处理

### 数据存储
- [ ] 本地日志有访问控制
- [ ] 日志保留期限符合政策
- [ ] 定期清理过期数据
```

#### 3.5.2 访问控制合规

```
## 访问控制检查清单

### 身份认证
- [ ] API密钥安全存储
- [ ] 密钥定期轮换
- [ ] 不同环境使用不同密钥

### 权限管理
- [ ] 最小权限原则
- [ ] 权限配置入库管理
- [ ] 权限变更有审批流程

### 审计追踪
- [ ] 所有敏感操作有日志
- [ ] 日志防篡改
- [ ] 异常行为有告警
```

---

## 4. 性能优化

### 4.1 上下文管理

#### 4.1.1 上下文窗口理解

Claude Code使用的模型有上下文窗口限制，理解并优化上下文使用是提升性能的关键：

**上下文组成**：

```
┌─────────────────────────────────────────────────────────────┐
│                    上下文窗口构成                            │
├─────────────────────────────────────────────────────────────┤

│                                                             │
│  ┌─────────────────┐                                       │
│  │  系统提示词      │  ~2,000 tokens                       │
│  │  (System Prompt) │                                       │
│  └─────────────────┘                                       │
│           │                                                 │
│  ┌─────────────────┐                                       │
│  │  CLAUDE.md      │  ~1,000-5,000 tokens                  │
│  │  项目配置        │                                       │
│  └─────────────────┘                                       │
│           │                                                 │
│  ┌─────────────────┐                                       │
│  │  对话历史       │  动态增长                               │
│  │  (History)      │                                       │
│  └─────────────────┘                                       │
│           │                                                 │
│  ┌─────────────────┐                                       │
│  │  工具结果       │  动态增长                               │
│  │  (Tool Results) │                                       │
│  └─────────────────┘                                       │
│           │                                                 │
│  ┌─────────────────┐                                       │
│  │  当前消息       │  用户输入                               │
│  │  (Current)      │                                       │
│  └─────────────────┘                                       │
│                                                             │
│  总计：最大 1,000,000 tokens（Claude Sonnet 4.6）           │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### 4.1.2 上下文优化策略

**策略1：精简CLAUDE.md**

```
# 优化前（冗长）
## 代码规范
我们的团队使用以下代码规范。首先，所有代码必须使用TypeScript编写。
其次，我们要求所有函数都有JSDoc注释。另外，变量命名必须遵循
camelCase规范。还有，类名必须使用PascalCase...
（继续500字）

# 优化后（精简）
## 代码规范
- 语言：TypeScript
- 注释：JSDoc必需
- 命名：变量camelCase，类PascalCase
- 行数：函数&lt;50行，类&lt;300行
```

**策略2：使用文件引用而非内联**

```
# 不推荐：在CLAUDE.md中内联大量内容
## API文档
{大量JSON内容...}

# 推荐：引用外部文件
## API文档
详见 `docs/api-reference.md`
```

**策略3：分层配置**

```
项目根目录/CLAUDE.md        # 核心规则（&lt;1000 tokens）
├── src/CLAUDE.md          # 源码规则（&lt;500 tokens）
├── tests/CLAUDE.md        # 测试规则（&lt;500 tokens）
└── docs/CLAUDE.md         # 文档规则（&lt;300 tokens）
```

#### 4.1.3 对话管理

**定期清理对话**：

```
# 当对话过长时，使用/clear清理
/clear

# 或创建新会话
claude --new-session

# 跨设备继续会话
/teleport  # 在CLI和claude.ai/code之间传送会话（也可用/tp）
```

**分解大任务**：

```
# 不推荐：一次性大任务
&quot;重构整个项目的所有模块，包括用户模块、订单模块、支付模块...&quot;

# 推荐：分步骤小任务
1. &quot;先分析用户模块的当前结构&quot;
2. &quot;基于分析结果，提出重构方案&quot;
3. &quot;执行用户模块重构&quot;
4. &quot;验证用户模块重构结果&quot;
5. &quot;继续处理订单模块...&quot;
```

💡 Shift+Enter换行

+   在对话框中按 Shift+Enter 可以换行而不发送消息

+   零配置，开箱即用

+   适合输入多行指令或复杂提示词

### 4.2 --verbose调试

#### 4.2.1 启用详细日志

```
# 启用verbose模式（CLI标志，不是配置文件选项）
claude --verbose

# 同时启用debug模式获取更详细信息
claude --verbose --debug

# 清理缓存排查问题
claude --clear-cache
```

#### 4.2.2 verbose输出解读

```
[DEBUG] 2025-01-15 10:30:45.123 Session started: sess_abc123
[DEBUG] 2025-01-15 10:30:45.125 Loading CLAUDE.md from /project/CLAUDE.md
[DEBUG] 2025-01-15 10:30:45.130 CLAUDE.md tokens: 1,234
[DEBUG] 2025-01-15 10:30:45.135 Loading .claude/settings.json
[DEBUG] 2025-01-15 10:30:45.140 MCP servers: 3 configured
[DEBUG] 2025-01-15 10:30:45.200 MCP server &#x27;context7&#x27; connected
[DEBUG] 2025-01-15 10:30:45.300 MCP server &#x27;exa&#x27; connected
[DEBUG] 2025-01-15 10:30:45.400 MCP server &#x27;task-master&#x27; connected

[INFO] User message received: &quot;帮我分析src/auth目录的代码结构&quot;
[DEBUG] 2025-01-15 10:30:46.000 Context size: 5,432 tokens
[DEBUG] 2025-01-15 10:30:46.001 Available context: 194,568 tokens
[DEBUG] 2025-01-15 10:30:46.002 Model: claude-sonnet-4-6

[DEBUG] 2025-01-15 10:30:46.100 Tool call: Glob
[DEBUG] 2025-01-15 10:30:46.101   Pattern: src/auth/**/*
[DEBUG] 2025-01-15 10:30:46.150   Result: 12 files found
[DEBUG] 2025-01-15 10:30:46.151   Tokens used: 234

[DEBUG] 2025-01-15 10:30:46.200 Tool call: Read
[DEBUG] 2025-01-15 10:30:46.201   Path: src/auth/index.ts
[DEBUG] 2025-01-15 10:30:46.250   Result: 156 lines, 2,345 tokens

[DEBUG] 2025-01-15 10:30:47.000 API request sent
[DEBUG] 2025-01-15 10:30:47.001   Input tokens: 8,011
[DEBUG] 2025-01-15 10:30:47.002   Max output tokens: 4,096

[DEBUG] 2025-01-15 10:30:52.000 API response received
[DEBUG] 2025-01-15 10:30:52.001   Output tokens: 1,234
[DEBUG] 2025-01-15 10:30:52.002   Latency: 5,000ms
[DEBUG] 2025-01-15 10:30:52.003   Cost: $0.0234
```

#### 4.2.3 性能瓶颈定位

**常见瓶颈及解决方案**：

| 症状 | 可能原因 | 解决方案 |
| --- | --- | --- |
| 响应慢 | 上下文过大 | 清理对话，精简CLAUDE.md |
| 工具调用失败 | MCP服务器问题 | 检查MCP配置和网络 |
| 频繁token超限 | 单次请求过大 | 分解任务 |
| 成本过高 | 无效调用多 | 优化提示词，减少迭代 |

### 4.3 成本控制

#### 4.3.1 成本计算公式

```
单次对话成本 = (输入tokens / 1M * 输入价格) + (输出tokens / 1M * 输出价格)

示例（Claude Sonnet 4.6）：
- 输入：$3/百万tokens
- 输出：$15/百万tokens
- 一次10K输入+2K输出：(10,000/1M * 3) + (2,000/1M * 15) = $0.03 + $0.03 = $0.06
```

> **128K输出token支持**：Opus模型现已支持最大128K输出token（之前所有模型最大约8K-64K）。这意味着可以在单次请求中生成更长的代码和文档，但也可能显著增加单次请求的输出token成本。使用Opus处理大规模生成任务时，请注意监控输出token用量。

#### 4.3.2 成本优化策略

**策略1：选择合适的模型**

| 任务类型 | 推荐模型 | 原因 |
| --- | --- | --- |
| 简单代码生成 | Claude Haiku 4.5 | 成本低，速度快 |
| 代码审查 | Claude Sonnet 4.6 | 平衡质量和成本 |
| 架构设计 | Claude Opus 4.6 | 复杂推理能力强 |

**策略2：批量处理**

```
# 不推荐：逐个文件处理
claude &quot;检查src/a.ts的代码质量&quot;
claude &quot;检查src/b.ts的代码质量&quot;
claude &quot;检查src/c.ts的代码质量&quot;

# 推荐：批量处理
claude &quot;检查src/目录下所有.ts文件的代码质量，生成汇总报告&quot;
```

策略3：利用API层面的Prompt Caching

Claude API支持Prompt Caching，可大幅降低重复上下文的成本：

```
Prompt Caching成本节省（以Sonnet 4.6为例）：
- 正常输入：$3.0/百万tokens
- 缓存写入：$3.75/百万tokens（1.25倍）
- 缓存读取：$0.30/百万tokens（0.1倍）

场景：重复使用5000 tokens的系统提示
- 无缓存：每次 $0.015
- 有缓存：首次 $0.01875，后续每次 $0.0015（节省90%）
```

> **注意**：Prompt Caching在API层面自动管理，Claude Code CLI用户无需手动配置。对于使用Agent SDK构建应用的开发者，可以在API调用中利用此特性。

#### 4.3.3 成本监控

创建成本监控脚本：

```
#!/usr/bin/env python3
# scripts/monitor-cost.py

import json
from pathlib import Path
from datetime import datetime, timedelta

# 价格配置（美元/百万tokens）
PRICING = {
    &quot;claude-sonnet-4-6&quot;: {&quot;input&quot;: 3.0, &quot;output&quot;: 15.0},
    &quot;claude-haiku-4-5-20251001&quot;: {&quot;input&quot;: 1.0, &quot;output&quot;: 5.0},
    &quot;claude-opus-4-6&quot;: {&quot;input&quot;: 5.0, &quot;output&quot;: 25.0}
}

def calculate_daily_cost(log_dir: str, date: str = None):
    &quot;&quot;&quot;计算指定日期的使用成本&quot;&quot;&quot;

    if date is None:
        date = datetime.now().strftime(&quot;%Y-%m-%d&quot;)

    log_file = Path(log_dir) / f&quot;{date}.json&quot;

    if not log_file.exists():
        return {&quot;date&quot;: date, &quot;total_cost&quot;: 0, &quot;details&quot;: []}

    with open(log_file) as f:
        data = json.load(f)

    total_cost = 0
    details = []

    for entry in data.get(&quot;entries&quot;, []):
        if &quot;tokens&quot; not in entry:
            continue

        model = entry.get(&quot;model&quot;, &quot;claude-sonnet-4-6&quot;)
        prices = PRICING.get(model, PRICING[&quot;claude-sonnet-4-6&quot;])

        input_tokens = entry[&quot;tokens&quot;].get(&quot;input&quot;, 0)
        output_tokens = entry[&quot;tokens&quot;].get(&quot;output&quot;, 0)

        cost = (input_tokens / 1_000_000 * prices[&quot;input&quot;]) + \
               (output_tokens / 1_000_000 * prices[&quot;output&quot;])

        total_cost += cost
        details.append({
            &quot;time&quot;: entry.get(&quot;timestamp&quot;),
            &quot;model&quot;: model,
            &quot;input_tokens&quot;: input_tokens,
            &quot;output_tokens&quot;: output_tokens,
            &quot;cost&quot;: cost
        })

    return {
        &quot;date&quot;: date,
        &quot;total_cost&quot;: total_cost,
        &quot;details&quot;: details
    }

if __name__ == &quot;__main__&quot;:
    import sys
    log_dir = sys.argv[1] if len(sys.argv) &gt; 1 else &quot;./logs/claude-audit/&quot;
    today = calculate_daily_cost(log_dir)
    print(f&quot;今日成本：${today[&#x27;total_cost&#x27;]:.2f}&quot;)
```

#### 4.3.4 成本控制实践

Claude Code本身没有内置的成本限制配置，但可以通过以下方式实现成本控制：

**方法1：API层面设置用量限制**

在 Anthropic Console 中设置：

+   每月消费上限（Spend Limits）

+   用量通知阈值

方法2：使用环境变量控制模型选择

```
# 日常开发使用Haiku（成本最低）
export CLAUDE_MODEL=claude-haiku-4-5-20251001

# 代码审查使用Sonnet（平衡质量和成本）
export CLAUDE_MODEL=claude-sonnet-4-6

# 架构设计使用Opus（最强推理）
export CLAUDE_MODEL=claude-opus-4-6
```

**方法3：通过脚本监控使用量**

结合上一节的成本监控脚本，定期检查使用量并设置告警。

### 4.4 性能基准测试

#### 4.4.1 响应时间基准

| 操作类型 | 预期响应时间 | 实际监控阈值 |
| --- | --- | --- |
| 简单问答 | &lt;2秒 | 5秒告警 |
| 代码生成（&lt;100行） | &lt;5秒 | 10秒告警 |
| 代码审查（&lt;500行） | &lt;10秒 | 20秒告警 |
| 大文件分析 | &lt;30秒 | 60秒告警 |

#### 4.4.2 吞吐量基准

| 指标 | 基准值 | 说明 |
| --- | --- | --- |
| 日均请求数 | &lt;500 | 单用户 |
| 日均token消耗 | &lt;100K | 单用户 |
| 并发会话 | &lt;5 | 单用户 |

---

## 5. 综合练习

### 5.1 练习1：配置企业级项目结构

**目标**：为一个新的企业项目配置完整的Claude Code环境

**要求**：

1.  创建标准目录结构

1.  编写项目CLAUDE.md

1.  配置权限白名单

1.  设置MCP服务器

**步骤提示**：

```
# 1. 创建目录结构
mkdir -p .claude/{commands,hooks,skills}
mkdir -p docs/ai-context
touch CLAUDE.md .mcp.json

# 2. 编写配置文件
# 参考本章模板

# 3. 验证配置
claude config list
```

**验收标准**：

### 5.2 练习2：配置CI/CD流水线

**目标**：为GitHub仓库配置Claude Code自动审查

**要求**：

1.  创建GitHub Actions工作流

1.  配置PR自动审查

1.  配置安全扫描

1.  设置评论交互

**步骤提示**：

```
# .github/workflows/claude.yml
name: Claude CI
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      # 参考本章配置示例
```

**验收标准**：

### 5.3 练习3：安全加固

**目标**：为现有项目进行安全加固

**要求**：

1.  审计当前权限配置

1.  识别并修复安全风险

1.  配置审计日志

1.  创建安全检查清单

**步骤提示**：

```
# 1. 检查当前权限配置
cat .claude/settings.json

# 2. 识别风险配置
# 检查allowedTools是否过于宽松
# 检查是否有敏感路径暴露

# 3. 配置审计日志
# 参考本章审计配置

# 4. 运行安全扫描
/security-review
```

**验收标准**：

### 5.4 练习4：性能优化

**目标**：优化现有项目的Claude Code使用性能

**要求**：

1.  分析当前性能瓶颈

1.  优化上下文使用

1.  配置成本监控

1.  建立性能基准

**步骤提示**：

```
# 1. 启用verbose模式分析
claude --verbose

# 2. 检查CLAUDE.md大小
wc -c CLAUDE.md
# 目标：&lt;5000字符

# 3. 检查对话上下文
# 关注context size日志

# 4. 配置成本监控
# 参考本章监控脚本
```

**验收标准**：

### 5.5 综合实战：从零搭建企业级环境

**场景**：你是一家创业公司的技术负责人，需要为团队（10人）搭建Claude Code企业级开发环境。

**任务清单**：

```
## 第一阶段：基础设施（第1周）

### 任务1.1：标准化项目模板
- [ ] 创建项目模板仓库
- [ ] 包含标准目录结构
- [ ] 包含基础CLAUDE.md
- [ ] 包含.claude/配置目录

### 任务1.2：配置管理
- [ ] 建立全局配置规范
- [ ] 建立项目配置模板
- [ ] 配置Git钩子同步配置

## 第二阶段：CI/CD集成（第2周）

### 任务2.1：GitHub Actions
- [ ] 配置自动代码审查
- [ ] 配置安全扫描
- [ ] 配置文档生成

### 任务2.2：工作流优化
- [ ] 创建团队共享命令
- [ ] 配置自动化钩子
- [ ] 建立审查规范

## 第三阶段：安全合规（第3周）

### 任务3.1：权限管理
- [ ] 定义权限策略
- [ ] 配置分环境权限
- [ ] 配置审计日志

### 任务3.2：合规检查
- [ ] 创建合规检查清单
- [ ] 配置自动合规扫描
- [ ] 建立异常告警

## 第四阶段：监控优化（第4周）

### 任务4.1：成本管控
- [ ] 配置成本监控
- [ ] 设置成本预警
- [ ] 建立成本分析报告

### 任务4.2：性能优化
- [ ] 建立性能基准
- [ ] 优化上下文使用
- [ ] 配置性能监控
```

**交付物**：

1.  项目模板仓库

1.  CI/CD配置文件

1.  安全策略文档

1.  监控仪表盘

1.  团队培训材料

---

## 总结

本章介绍了Claude Code在企业环境中的最佳实践，涵盖：

1.  **团队协作规范**：标准化项目结构、CLAUDE.md规范、代码审查流程

1.  **CI/CD集成**：GitHub Actions配置、安全审查、自动化流水线

1.  **安全与合规**：权限系统、白名单配置、审计日志、合规检查

1.  **性能优化**：上下文管理、调试技巧、成本控制

掌握这些内容后，你将能够：

+   为团队建立统一的Claude Code使用规范

+   实现自动化的代码审查和安全扫描

+   确保AI辅助开发的安全性和合规性

+   有效控制使用成本并优化性能

---

**版本历史**：

+   V1.3.0 (2026-03-18)：新增Sandbox沙箱配置详解（filesystem权限控制）、128K输出token说明、更新适用版本至v2.1+

+   V1.2.0 (2026-03-18)：全面审核修正——修复MCP虚构配置字段（trust/network/limits）、移除虚构settings.json字段（audit/cache/costControl/verbose）、修正分环境配置为标准三级层次、修正CLI命令（permission-mode strict→plan）、更新定价信息

+   V1.1.0 (2026-01-19)：更新至Claude Code 2.1.12，新增Skills和Hooks frontmatter说明、通配符权限、Shift+Enter换行等新特性

+   V1.0.0 (2025-12-24)：初始版本，基于企业级最佳实践指南创建

**最后更新**：2026年3月18日 | **版本**：V1.3.0