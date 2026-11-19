
# Antigravity更新了Skill，Skill将是全网即将统一的核心功能！

> 来源：[https://my.feishu.cn/docx/YVShdf9huo5PWexx16EcXU6GnBd](https://my.feishu.cn/docx/YVShdf9huo5PWexx16EcXU6GnBd)

Antigravity 更新 Skill几天了，老金刚有空写。

老金可以趁机再给大家说一遍Skill是什么，它为什么受到全网推崇。

Claude Code在25年的10月就发布了Agent Skills，并在2026年的1月，确立了“全 Skill 化”的架构，并开源其他任意平台可接入使用。

**它把扩展 AI 能力这件事，回归到了最本质、最极客的方式：文件即技能。**

## Skills 是怎么个事儿？

别被名字唬住了。

**Skill 仅仅就是一个文件夹**。

对，你没听错。

+   你想让 Agent 会写 Python？你就往项目里丢个 `python_expert` 文件夹。

+   你想让它会自动部署？丢个 `auto_deploy` 文件夹。

+   你想让它懂你们公司的代码规范？丢个 `team_guidelines` 文件夹。

Antigravity 会自动扫描这些文件夹，读取里面的`SKILL.md`，然后瞬间"学会"这些技能。

这就是 **降维打击**。以前我们要去某个"插件市场"搜半天，现在直接 `Ctrl+C Ctrl+V` 文件夹，技能就装好了。

> **背景说明**：Skills 这个概念其实在 Claude Code 里已经存在很久了。Antigravity 这次做的，是把这套机制推向了极致——完全文件化、零配置、透明可控。

## 核心解密：完整的 Skill 文件夹结构

一个标准的 Skill 文件夹，结构清晰得让人感动。老金带你拆解一下：

```
📂 `my-custom-skill/`
├── 📄 `SKILL.md`             # 必选：Skill 的“灵魂”。定义功能描述、触发场景及 Claude 的思考逻辑。
├── 📄 `config.json`          # 必选：安全性与元数据。定义权限边界（联网、读写）和版本信息。
├── 📄 `requirements.txt`     # 可选：Python 依赖定义（若有脚本，推荐包含以实现自动隔离安装）。
├── 📄 `package.json`         # 可选：Node.js 依赖定义（替代 `requirements.txt`）。
├── 📄 `reference.md`         # 可选：长文档。存储 API 字典或复杂规范，通过“渐进式披露”节省上下文。
├── 📄 `examples.md`          # 可选：少样本学习 (Few-shot)。提供最佳实践案例供 Claude 参考。
├── 📂 `scripts/`             # 可选：可执行逻辑。存放 Python/Bash/JS 脚本，由 Claude 调度执行。
│   └── 🔧 `main_logic.py`
├── 📂 `assets/`              # 可选：静态资源。如预设的代码模板 (.txt)、提示词片段或测试数据。
├── 📂 `tests/`               # 可选：质量保证。存放针对 `scripts` 的单元测试，确保 Skill 更新后依然稳健。
└── 📄 `.clauderules`         # 可选：局部约束。定义在此 Skill 激活时 Claude 必须遵守的特定行为准则。
```

### 路径

项目专属技能 (Workspace Skills)

如果你只想让技能在**当前项目**中生效，请将其建立在项目根目录下的隐藏文件夹内：

+   **路径**：`<项目根目录>/.agent/skills/<你的技能文件夹>/`

+   **示例**：若你的项目文件夹为 `my-app`，技能名为 `deploy-tool`，则路径为 `my-app/.agent/skills/deploy-tool/`。

全局通用技能 (Global Skills)

如果希望在**所有项目**（整机通用）中都能调用该技能，请将其建立在用户家目录下：

+   **路径**：`~/.gemini/antigravity/skills/<你的技能文件夹>/`

+   **提示**：在 Mac 系统中，可通过访达的“前往文件夹”功能直接输入该路径进入。

### 1. 核心大脑：`SKILL.md` (必选)

这是灵魂。只要这个文件存在，Agent 就知道自己会了啥。里面分两部分：

**YAML 头（元数据）**：

```
---
name: "code-security-audit"
description: "用于扫描当前目录下的代码漏洞，建议在提交 PR 前使用"
# 2026 规范：工具名建议使用小写 snake_case
allowed-tools: ["ls", "grep", "read_file", "view_code_definitions"] 
# 独立上下文运行，保护主对话的 Token 额度
context: "fork"
# 定义此 Skill 归属于哪类 Agent（如编码、运维、测试）
agent: "software-engineer"
# 允许用户在终端输入 /code-security-audit 快速启动
user-invocable: true
# 新增：定义此 Skill 的核心唤醒词
keywords: ["security", "audit", "vulnerability"]
---
```

**Markdown 正文（指令）**：

+   用人话写清楚怎么干

+   给具体示例和最佳实践

+   可以引用其他文件（通过链接）

### 2. 详细文档：`reference.md` (可选)

把复杂的API说明、参数详解放这里。

`SKILL.md` 里引用：

```
## 详细说明
完整的API文档见 [reference.md](reference.md)
```

### 3. 使用示例：`examples.md` (可选)

放几个真实例子，让 Agent 照着抄。

### 4. 工具脚本：`scripts/` (可选)

**关键特性**：脚本不会加载到上下文，直接执行，只消耗输出结果的 token。

适合放：

+   复杂的验证逻辑

+   数据处理脚本

+   需要保持一致性的操作

在 `SKILL.md` 里告诉 Claude 执行脚本：

```bash
python scripts/validate.py input.txt
```

## 手把手：3分钟搓一个"自动发布"技能

**别光看，动手。**假设我们要搞一个 一键发版 的技能。

### 第一步：建房

在项目根目录（或者你习惯放文档的地方），建个文件夹 `publish_skill`。

### 第二步：写说明书 (`SKILL.md`)

直接新建 `publish_skill/SKILL.md`，把下面这段粘进去：

```
---

name: auto-publish

description: 当用户说"发布"、"上线"或"publish"时，使用此技能执行自动发布流程

allowed-tools: Bash, Read

---

# 自动发布技能指南

看到用户想要发布，别废话，按这个流程走。

## 1. 安全红线检查 (Critical)

在干活前，必须先检查 git 状态，确保没有未提交的更改。

## 2. 执行发布脚本

环境干净的话，直接运行发布脚本。

## 3. 汇报结果

把脚本输出的最后一行发给用户。
```

### 第三步：给它工具 (`scripts/`)

在 `publish_skill/scripts/` 里放个 `do_publish.sh`：

```bash
#!/bin/bash

set -e  # 遇到错误立即退出

echo "🔄 正在打包..."

# npm run build

# npm run test

echo "✅ 发布成功！版本号 v1.0.1"
```

记得给脚本加执行权限：

```bash
chmod +x scripts/do_publish.sh
```

**完事了。**

现在你回到对话框，跟 Antigravity 说："帮我发布一下"。

它会立刻读取那个 Markdown，先检查 Git，然后跑脚本。

这一套行云流水，全是你定义的标准动作。

## 为什么说这招"太狠了"？

### 1. 一键复用，秒杀配置

当你把团队的代码规范、发布流程、数据库查询方式，写成几个 Skill 文件夹。

新人入职？

**直接把文件夹发给他**。

"把这个丢你项目里"。

啪，他的 Agent 瞬间拥有了和你一样的经验和权限。

这省的不是配置时间，是沟通成本啊！

### 2. 透明，完全透明

没有任何黑盒逻辑。Agent 为什么这么做？

去读 `SKILL.md`。Agent 这里做错了？去改 `SKILL.md`。

你在调试 Agent，就像在调试代码一样可控。

### 3. 社区即仓库

虽然官方没搞什么 App Store，但 GitHub 才是最大的 Store。

以后肯定各种大佬开源自己的 Skill 文件夹：

"老金的 React 调优技能包"

"某大厂的 Go 语言最佳实践包"

下载下来，丢进去，你的 AI 就升级了。

---

⚠️ 老金的特别提醒：安全！安全！还是安全！

虽然“Ctrl+C”装技能很爽，但老金必须多句嘴：技能文件夹里的 `scripts/` 是有执行权限的。

如果你从网上随便下个“大神包”，里面可能藏着删除数据库或者上传密钥的恶意脚本。

在运行任何不明来源的 Skill 前，记得先扫一眼 `config.json` 里的权限声明，这是咱极客的基本操守。

## 老金的最后建议

既然 Antigravity 给了这把"瑞士军刀"，咱们就别把它当普通水果刀用了。

**建议大家现在就做一件事**：把你手头最重复、最繁琐的工作流，写成一个 `SKILL.md`。

哪怕只是一个简单的 "Git 提交规范检查"，你会发现，**让 AI 严格按文档干活的感觉，真的太爽了。**

**话不多说，建文件夹去了！**

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
