# 第六章：Hermes Agent的Skills System

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/EpMMdewpdoFEYxxAa2Ict4aVnac](https://qqs7y1hozd1.feishu.cn/docx/EpMMdewpdoFEYxxAa2Ict4aVnac)

**Skill**[s是按需提供的知识文档，代理](https://agentskills.io/specification)可以在需要时加载。它们遵循渐进式披露模式，以最大限度地减少令牌使用，并与agentskills.io开放标准兼容。

所有Skills都存放在~/.hermes/skills/ —— 这是主要目录和事实来源。全新安装时，捆绑的技能会从仓库复制过来。通过中心安装和代理创建的技能也会存放在这里。代理可以修改或删除任何技能。

您还可以将 Hermes 指向**外部技能目录**[ —— 与本](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills#external-skill-directories)地目录一起扫描的其他文件夹。请参阅外部技能目录（如下）。

另见：

+   [捆绑技能目录](https://hermes-agent.nousresearch.com/docs/reference/skills-catalog)

+   [官方可选技能目录](https://hermes-agent.nousresearch.com/docs/reference/optional-skills-catalog)

## 使用技能

每个已安装的技能都会自动作为斜杠命令可用：

```
# In the CLI or any messaging platform:
/gif-search funny cats
/axolotl help me fine-tune Llama 3 on my dataset
/github-pr-workflow create a PR for the auth refactor
/plan design a rollout for migrating our auth provider

# Just the skill name loads it and lets the agent ask what you need:
/excalidraw
```

捆绑技能是具有自定义行为的技能支持斜杠命令的一个很好的例子。运行该命令会告诉Hermes在必要时检查上下文，编写一个Markdown实现计划而不是执行任务，并将结果保存到相对于活动工作区/后端工作目录的位置。plan/plan [request].hermes/plans/

您还可以通过自然对话与技能进行交互：

```
hermes chat --toolsets skills -q &quot;What skills do you have?&quot;
hermes chat --toolsets skills -q &quot;Show me the axolotl skill&quot;
```

## 渐进式披露

技能采用了一种高效利用令牌的加载模式：

```
Level 0: skills_list()           → [{name, description, category}, ...]   (~3k tokens)
Level 1: skill_view(name)        → Full content + metadata       (varies)
Level 2: skill_view(name, path)  → Specific reference file       (varies)
```

代理仅在实际需要时才加载完整的技能内容。

## SKILL.md格式

```
---
name: my-skill
description: Brief description of what this skill does
version: 1.0.0
platforms: [macos, linux]     # Optional — restrict to specific OS platforms
metadata:
  hermes:
    tags: [python, automation]
    category: devops
    fallback_for_toolsets: [web]    # Optional — conditional activation (see below)
    requires_toolsets: [terminal]   # Optional — conditional activation (see below)
    config:                          # Optional — config.yaml settings
      - key: my.setting
        description: &quot;What this controls&quot;
        default: &quot;value&quot;
        prompt: &quot;Prompt for setup&quot;
---

# Skill Title

## When to Use
Trigger conditions for this skill.

## Procedure
1. Step one
2. Step two

## Pitfalls
- Known failure modes and fixes

## Verification
How to confirm it worked.
```

### 特定平台技能

技能可以使用以下字段将自身限制在特定的操作系统上：平台

```
platforms: [macos]            # macOS only (e.g., iMessage, Apple Reminders, FindMy)
platforms: [macos, linux]     # macOS and Linux
```

设置后，该技能会自动在系统提示、不兼容平台的斜杠命令中隐藏。如果省略，则该技能会在所有平台上加载。技能列表()

### 条件激活（备用技能）

**技能可以**根据当前会话中可用的工具自动显示或隐藏。这对于备用技能最为有用，备用技能是免费或本地的替代方案，仅在高级工具不可用时才会出现。

```
metadata:
  hermes:
    fallback_for_toolsets: [web]      # Show ONLY when these toolsets are unavailable
    requires_toolsets: [terminal]     # Show ONLY when these toolsets are available
    fallback_for_tools: [web_search]  # Show ONLY when these specific tools are unavailable
    requires_tools: [terminal]        # Show ONLY when these specific tools are available
```

**示例：**内置技能使用。当你设置好后，网络工具集可用，代理使用——DuckDuckGo技能保持隐藏。如果API密钥缺失，网络工具集不可用，DuckDuckGo技能会自动作为备用技能出现。duckduckgo-searchfallback_for_toolsets: [web]FIRECRAWL_API_KEYweb_search

没有任何条件字段的技能的行为与之前完全相同——它们始终显示。

## 加载时安全设置

技能可以声明所需的环境变量，而不会在发现过程中消失：

```
required_environment_variables:
  - name: TENOR_API_KEY
    prompt: Tenor API key
    help: Get a key from https://developers.google.com/tenor
    required_for: full functionality
```

当遇到缺失值时，Hermes仅在技能实际加载到本地CLI中时才会安全地请求该值。您可以跳过设置并继续使用该技能。消息界面在聊天中从不询问机密信息，而是提示您使用或在本地使用。hermes setup~/.hermes/.env

一旦设置，声明的环境变量将**自动传递**到沙箱中，技能脚本可以直接使用。对于非[技能环境变量](https://hermes-agent.nousresearch.com/docs/user-guide/security#environment-variable-passthrough)，请使用配置选项。详情请参阅环境变量传递。execute_codeterminal$TENOR_API_KEYterminal.env_passthrough

### 技能配置设置

技能还可以声明存储在以下位置的非机密配置设置（路径、首选项）：config.yaml

```
metadata:
  hermes:
    config:
      - key: wiki.path
        description: Path to the wiki directory
        default: &quot;~/wiki&quot;
        prompt: Wiki directory path
```

设置存储在您的config.yaml中。提示未配置的设置，并显示它们。当技能加载时，其解析的配置值会被注入到上下文中，以便代理自动知道已配置的值。技能配置hermes配置迁移hermes配置显示

详情请参阅[技能设置](https://hermes-agent.nousresearch.com/docs/user-guide/configuration#skill-settings)和[创建技能 - 配置设置](https://hermes-agent.nousresearch.com/docs/developer-guide/creating-skills#config-settings-configyaml)。

## 技能目录结构

```
~/.hermes/skills/                  # Single source of truth
├── mlops/                         # Category directory
│   ├── axolotl/
│   │   ├── SKILL.md               # Main instructions (required)
│   │   ├── references/            # Additional docs
│   │   ├── templates/             # Output formats
│   │   ├── scripts/               # Helper scripts callable from the skill
│   │   └── assets/                # Supplementary files
│   └── vllm/
│       └── SKILL.md
├── devops/
│   └── deploy-k8s/                # Agent-created skill
│       ├── SKILL.md
│       └── references/
├── .hub/                          # Skills Hub state
│   ├── lock.json
│   ├── quarantine/
│   └── audit.log
└── .bundled_manifest              # Tracks seeded bundled skills
```

## 外部技能目录

如果你在Hermes之外维护技能（例如，多个AI工具使用的共享目录），你也可以告诉Hermes扫描这些目录。~/.agents/skills/

在以下部分添加：external_dirsskills~/.hermes/config.yaml

```
skills:
  external_dirs:
    - ~/.agents/skills
    - /home/shared/team-skills
    - ${SKILLS_REPO}/skills
```

路径支持扩展和环境变量替换。~${VAR}

### 工作原理

+   **只读**：仅扫描外部目录以发现技能。当代理创建或编辑技能时，它总是写入~/.hermes/skills/

+   **本地优先级**：如果同一技能名称在本地目录和外部目录中都存在，则本地版本优先。

+   **完全集成**：外部技能会出现在系统提示索引中，并且以斜杠命令的形式呈现，与本地技能并无二致。技能列表技能视图/技能名称

+   **不存在的路径会被默默跳过**：如果配置的目录不存在，Hermes会忽略它而不报错。这对于可能并非每台机器上都存在的可选共享目录很有用。

### 示例

```
~/.hermes/skills/               # Local (primary, read-write)
├── devops/deploy-k8s/
│   └── SKILL.md
└── mlops/axolotl/
    └── SKILL.md

~/.agents/skills/               # External (read-only, shared)
├── my-custom-workflow/
│   └── SKILL.md
└── team-conventions/
    └── SKILL.md
```

所有四项技能都出现在您的技能索引中。如果您创建一个名为本地的新技能，它将覆盖外部版本。我的自定义工作流

## 代理管理技能（skill_manage工具）

**智能体可以**通过工具创建、更新和删除自己的技能。这就是智能体的程序性记忆——当它想出一个非平凡的工作流程时，它会将该方法保存为一项技能，以备将来复用。skill_manage

### 当代理创建技能时

+   成功完成一项复杂任务（5次以上工具调用）后

+   当遇到错误或死路时，找到可行的路径

+   当用户纠正其方法时

+   当它发现一个非平凡的工作流时

### 操作

提示

此操作更适合更新——它比其他操作更节省令牌，因为只有更改的文本会出现在工具调用中。patchedit

## 技能中心

浏览、搜索、安装和管理来自在线注册表、直接知名技能端点和官方可选技能的技能。skills.sh

### 常用命令

```
hermes skills browse                              # Browse all hub skills (official first)
hermes skills browse --source official            # Browse only official optional skills
hermes skills search kubernetes                   # Search all sources
hermes skills search react --source skills-sh     # Search the skills.sh directory
hermes skills search https://mintlify.com/docs --source well-known
hermes skills inspect openai/skills/k8s           # Preview before installing
hermes skills install openai/skills/k8s           # Install with security scan
hermes skills install official/security/1password
hermes skills install skills-sh/vercel-labs/json-render/json-render-react --force
hermes skills install well-known:https://mintlify.com/docs/.well-known/skills/mintlify
hermes skills list --source hub                   # List hub-installed skills
hermes skills check                               # Check installed hub skills for upstream updates
hermes skills update                              # Reinstall hub skills with upstream changes when needed
hermes skills audit                               # Re-scan all hub skills for security
hermes skills uninstall k8s                       # Remove a hub skill
hermes skills publish skills/my-skill --to github --repo owner/repo
hermes skills snapshot export setup.json          # Export skill config
hermes skills tap add myorg/skills-repo           # Add a custom GitHub source
```

### 支持的中心源

### 集成枢纽和注册中心

爱马仕目前与这些技能生态系统和发现源集成：

#### 官方可选技能（）官方

这些在Hermes仓库中维护，并以内置信任方式安装。

+   目录：[官方可选技能目录](https://hermes-agent.nousresearch.com/docs/reference/optional-skills-catalog)

+   仓库中的源文件：optional-skills/

+   示例：

```
hermes skills browse --source official
hermes skills install official/security/1password
```

#### skills.sh ()skills-sh

这是Vercel的公共技能目录。Hermes可以直接搜索它，查看技能详情页面，解析别名式的URL段，并从底层源仓库进行安装。

+   目录：[skills.sh](https://skills.sh/)

+   CLI/工具库：vercel-labs/skills

+   官方 Vercel 技能仓库：vercel-labs/agent-skills

+   示例：

```
hermes skills search react --source skills-sh
hermes skills inspect skills-sh/vercel-labs/json-render/json-render-react
hermes skills install skills-sh/vercel-labs/json-render/json-render-react --force
```

#### 知名技能端点 ()知名

这是基于 URL 的发现机制，来自发布内容的网站。它不是单一的集中式中心——而是一种网络发现约定。/.well-known/skills/index.json

+   示例实时端点：[ Mintlify文档技能索引](https://mintlify.com/docs/.well-known/skills/index.json)

+   参考服务器实现：vercel-labs/skills-handler

+   示例：

```
hermes skills search https://mintlify.com/docs --source well-known
hermes skills inspect well-known:https://mintlify.com/docs/.well-known/skills/mintlify
hermes skills install well-known:https://mintlify.com/docs/.well-known/skills/mintlify
```

#### 直接GitHub技能()github

Hermes 可以直接从 GitHub 仓库和基于 GitHub 的 taps 进行安装。当你已经知道仓库/路径，或者想要添加自己的自定义源仓库时，这非常有用。

默认标签（无需任何设置即可浏览）：

+   [openai/技能](https://github.com/openai/skills)

+   [anthropics/技能](https://github.com/anthropics/skills)

+   VoltAgent/出色的代理技能

+   [garrytan/gstack](https://github.com/garrytan/gstack)

+   示例：

```
hermes skills install openai/skills/k8s
hermes skills tap add myorg/skills-repo
```

#### ClawHub ()clawhub

作为社区资源集成的第三方技能市场。

+   站点：[clawhub.ai](https://clawhub.ai/)

+   Hermes源ID： clawhub

#### Claude市场风格仓库()claude-marketplace

Hermes支持发布Claude兼容插件/市场清单的市场仓库。

已知的集成源包括：

+   [anthropics/技能](https://github.com/anthropics/skills)

+   [aiskillstore/市场](https://github.com/aiskillstore/marketplace)

Hermes源ID：  claude-marketplace

#### LobeHub ()lobehub

Hermes可以搜索LobeHub公共目录中的代理条目，并将其转换为可安装的Hermes技能。

+   站点：[LobeHub](https://lobehub.com/)

+   公共代理索引：chat-agents.lobehub.com

+   支持仓库：lobehub/lobe-chat-agents

+   Hermes源ID：  lobehub

### 安全扫描和--force

所有通过中心安装的技能都会经过**安全扫描器**的检查，该扫描器会检查数据泄露、提示注入、破坏性命令、供应链信号和其他威胁。

hermes技能检查...现在在可用时也会显示上游元数据：

+   仓库URL

+   skills.sh详情页URL

+   安装命令

+   每周安装量

+   上游安全审计状态

+   知名索引/端点URL

当你已审核第三方技能并希望覆盖非危险策略阻止时使用：--force

```
hermes skills install skills-sh/anthropics/skills/pdf --force
```

重要行为：

+   --force 可以覆盖针对警告/警示类结果的策略阻止。

+   --force不会**覆盖**扫描结果。危险

+   官方可选技能（）被视为内置信任，不会显示第三方警告面板。官方/...

### 信任级别

### 更新生命周期

现在，中心能够追踪足够的来源信息，以便重新检查已安装技能的上游副本：

```
hermes skills check          # Report which installed hub skills changed upstream
hermes skills update         # Reinstall only the skills with updates available
hermes skills update react   # Update one specific installed hub skill
```

此方法使用存储的源标识符加上当前上游捆绑包内容哈希来检测偏差。

GitHub速率限制

技能中心操作使用 GitHub API，该 API 对未认证用户的速率限制为每小时 60 个请求。如果在安装或搜索期间遇到速率限制错误，请在 <a i=1>GITHUB_TOKEN.env 文件中设置，将限制提高到每小时 5000 个请求。发生这种情况时，错误消息会包含可操作的提示。</a>

### 斜杠命令（在聊天中）

所有相同的命令都适用于：/技能

```
/skills browse
/skills search react --source skills-sh
/skills search https://mintlify.com/docs --source well-known
/skills inspect skills-sh/vercel-labs/json-render/json-render-react
/skills install openai/skills/skill-creator --force
/skills check
/skills update
/skills list
```

官方可选技能仍使用类似和的标识符。官方/安全/1password官方/迁移/openclaw迁移