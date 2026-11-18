
# Claude Code 中没用过 Hooks？那你可真是丢了个神器

> 来源：[https://my.feishu.cn/docx/K6zudghxhonYP9xyukGcebBCnbf](https://my.feishu.cn/docx/K6zudghxhonYP9xyukGcebBCnbf)

用 Claude Code Hooks 快一年了，上周在群里随口问了一句：

"你们用 Hooks 吗？"

20 多个人，17 个回：啥是 Hooks？

剩下 3 个说听说过，不知道咋配。

我当时就懵了。

这就好比你买了辆车，天天当板车推，从来不知道能打火开。

我接着问："那你们平时用 Claude Code 干啥？"

基本都是：让它改改代码，问问 bug 在哪儿。

我用 Hooks 用了大半年了，自动 `git commit`、自动格式化、保护敏感文件，这些都是日常操作。

周五凌晨刷 GitHub，看到 disler 的 `claude-code-hooks-mastery`，1.9k 星。

点开一看，把 8 种 Hook 全实现了，代码写得比我自己的好太多。

这个项目值得推荐，比我自己摸索的那套完善多了。

## 我这大半年用的 Hook

先说说我日常在用的几个，都是被坑出来的。

### 去年 11 月的坑：代码丢了 3 次

去年 11 月开始用 Claude Code，有次让它重构一个 300 行的文件。

改了半天，突然断网，再打开，全没了。

那天改的逻辑，全都要重新想。

那时候就配了 `PostToolUse` Hook，Claude 动了文件就自动 `git commit`。

大半年过来，再没丢过代码。

### 去年 12 月的坑：`.env` 被删了

有次让 Claude "清理一下项目里的无用文件"。

它把 `.env` 直接删了。

数据库密钥、API key，全没了。

找了半天备份才恢复。

从那以后配了 `PreToolUse` Hook，碰到 `.env`、`.git` 这些文件，直接拦住。

### 今年开始的坑：格式化手动跑

以前写完代码，得手动 `npx prettier --write xxx`。

一天下来得手动跑 50 次。

后来 `PostToolUse` Hook 配上，改完 `JS/TS` 文件，自动格式化。

这三个 Hook，我每天都在用，已经成习惯了。

看了 disler 的 `claude-code-hooks-mastery`，发现人家把 8 种 Hook 都实现了。

我自己这大半年，其实只用了 3 种：`PostToolUse`、`PreToolUse`、偶尔用用 `Stop`。

其他 5 种，听说过但没深入用。

`UserPromptSubmit` 是按回车时触发的，`Notification` 是通知时用的，`SubagentStop` 是子任务结束用的，`PreCompact` 是压缩对话前触发，`SessionStart` 是启动时加载。

disler 项目里，这 8 种 Hook 都有完整实现，代码质量比我自己写的好太多。

特别是 `Stop` Hook 那块，他用了 `stop_hook_active` 标志防无限循环，我之前没想到这个细节，吃过亏。

还有个新东西我没玩过：Prompt-based Hooks。

不用写脚本，直接写提示词让 LLM 判断，这个思路挺骚。

所以这次专门写篇文章推荐，顺便把我自己这大半年踩的坑也说说。

关键是，这 8 个 Hook 里，有 4 个能"拦住" Claude，4 个不能。

能拦住的是 `UserPromptSubmit`（拦提示词）、`PreToolUse`（拦工具执行）、`Stop`（拦停止）、`SubagentStop`（拦子任务停止）。

不能拦住的是 `PostToolUse`（事后检查，已经晚了）、`Notification`（纯提醒）、`PreCompact`（压缩前备份）、`SessionStart`（启动时加载）。

这个区别很重要。

比如你想防止 Claude 删 `.env`，必须用 `PreToolUse`，在它动手之前拦住。

`PostToolUse` 不行，文件已经删了，来不及了。

## 最实用的 3 个配置

说了这么多理论，直接给配置。

### 配置 1：自动 git commit

这个救过我无数次。

在项目根目录创建 `.claude/settings.json`：

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Edit|Write",
      "hooks": [{
        "type": "command",
        "command": "git add -A && git commit -m 'auto: claude changes'"
      }]
    }]
  }
}
```

只要 Claude 改了文件，自动 commit。

再也不怕代码丢。

### 配置 2：保护敏感文件

这个防删库跑路。

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Edit|Write",
      "hooks": [{
        "type": "command",
        "command": "python3 -c \"import json,sys;d=json.load(sys.stdin);p=d.get('tool_input',{}).get('file_path','');sys.exit(2 if '.env' in p or '.git/' in p else 0)\""
      }]
    }]
  }
}
```

这个 Python 脚本的意思：

检查文件路径，如果是 `.env` 或 `.git`，退出码返回 2。

退出码 `2` 在 Claude Code 里是"阻止操作"的意思。

Claude 想删 `.env`，被 Hook 拦下来，删不了。

### 配置 3：自动格式化

强迫症必备。

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Edit .*\\.(js|ts|jsx|tsx)$",
      "hooks": [{
        "type": "command",
        "command": "npx prettier --write $FILE_PATH"
      }]
    }]
  }
}
```

改完 `JS/TS` 文件，自动 `prettier`。

代码永远美美的。

### 把这 3 个配置合在一起

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [{
          "type": "command",
          "command": "git add -A && git commit -m 'auto: claude changes'"
        }]
      },
      {
        "matcher": "Edit .*\\.(js|ts|jsx|tsx)$",
        "hooks": [{
          "type": "command",
          "command": "npx prettier --write $FILE_PATH"
        }]
      }
    ],
    "PreToolUse": [{
      "matcher": "Edit|Write",
      "hooks": [{
        "type": "command",
        "command": "python3 -c \"import json,sys;d=json.load(sys.stdin);p=d.get('tool_input',{}).get('file_path','');sys.exit(2 if '.env' in p or '.git/' in p else 0)\""
      }]
    }]
  }
}
```

复制到 `.claude/settings.json`，重启 Claude Code，就生效了。

## Exit Code 这个东西

这里有个关键点，Exit Code。

你的 Hook 脚本退出的时候，返回的数字：`0` 是成功继续，`2` 是阻止操作把错误信息喂给 Claude，其他数字是报错但继续。

所以保护文件那个脚本，用的 `sys.exit(2)`。

如果检测到 `.env`，退出码 `2`，Claude 就停下来，不会继续删文件。

这个机制，`PreToolUse`、`UserPromptSubmit`、`Stop`、`SubagentStop` 都支持。

`PostToolUse` 不支持，因为工具已经执行完了。

## 进阶玩法：Stop Hook 强制继续

有时候 Claude 干完活想停，但你觉得还没完成。

比如跑测试，有 3 个失败了，Claude 说"我完成了"。

你想让它继续修到全部通过。

用 `Stop` Hook：

```json
{
  "hooks": {
    "Stop": [{
      "hooks": [{
        "type": "command",
        "command": "python ~/.claude/hooks/check_tests.py"
      }]
    }]
  }
}
```

`check_tests.py`：

```python
import json, sys

# 检查测试是否全部通过
# 如果有失败,返回exit code 2

data = json.load(sys.stdin)

# 检查stop_hook_active,防止无限循环
if data.get("stop_hook_active"):
    sys.exit(0)  # 已经在继续了,让它停

# 这里写你的测试检查逻辑
# 如果测试失败:
print("还有3个测试失败,继续修!", file=sys.stderr)
sys.exit(2)  # 阻止停止
```

注意那个 `stop_hook_active` 检查，很重要。

不然会无限循环，Claude 永远停不下来。

## 高级控制：JSON 输出

除了 Exit Code，Hook 还能返回 JSON 控制。

比如 `PreToolUse` 可以返回：

```json
{
  "decision": "approve",
  "reason": "这个文件可以改"
}
```

或者：

```json
{
  "decision": "block",
  "reason": "不许碰.env文件"
}
```

`decision` 有 3 种：`approve` 是批准绕过权限检查，`block` 是阻止把 `reason` 喂给 Claude，`undefined` 是正常流程。

`Stop` Hook 也能用：

```json
{
  "decision": "block",
  "reason": "测试还没全通过,继续修"
}
```

这个 JSON 输出，比 Exit Code 更灵活，可以给 Claude 更详细的反馈。

## 新特性：Prompt-based Hooks

从最近版本开始，`Stop` 和 `SubagentStop` 支持一种新模式。

不用写脚本，直接写个提示词，让 LLM 判断：

```json
{
  "hooks": {
    "Stop": [{
      "hooks": [{
        "type": "prompt",
        "prompt": "Check if all tests passed. If any failed, block and tell Claude to fix them."
      }]
    }]
  }
}
```

Claude Code 会把 Hook 的输入和你的提示词，发给 Haiku（快速 LLM）。

Haiku 返回决策，告诉 Claude Code 该咋办。

这个比写脚本灵活多了，但要消耗 API。

## 真实数据

用了一个月，统计了一下：

**自动 commit：**

以前每天手动 `commit` 15-20 次。

现在自动，省 30 分钟。

**自动格式化：**

以前手动 `prettier` 一天 50 次。

现在自动，省 8 分钟。

**误删事故：**

以前误删过 3 次敏感文件。

配了保护 Hook，0 次。

Anthropic 官方研究说，Hooks 能让开发效率提升 50-100%。

TELUS 给 57000 员工部署后，PR 周转快了 30%。

IG Group 的分析团队，每周省 70 小时。

## 也有坑

Hook 写错了，会影响 Claude 的正常工作。

比如你的 Hook 超时（默认 60 秒），会拖慢 Claude。

调试也不方便，Hook 报错信息有时候看不到。

建议先在测试项目试，Hook 里加日志方便调试，执行时间控制在 1 秒内。

我的做法是在 Hook 里加这一行：

```bash
echo "$(date) Hook执行: $TOOL_NAME" >> ~/.claude/hook-debug.log
```

出问题了，去这个日志文件里找。

## 现在该你了

如果你经常让 Claude 改代码，手动 `commit` 太麻烦，担心 Claude 误删重要文件，代码格式化要手动跑，那你应该用 Hook。

如果你只是偶尔问问题，不让 Claude 动文件，项目没有 git，那 Hook 对你用处不大。

GitHub 上那个教程，1.9k 星了：

https://github.com/disler/claude-code-hooks-mastery

里面有完整的 8 种 Hook 实现，代码都是现成的。

我给的 3 个配置，复制到 `.claude/settings.json`，重启就能用。

用完你会发现，Claude Code 不只是个聊天工具。

配上 Hook，它是个可以深度定制的自动化平台。

以前我们和 Claude 对话，是一问一答。

现在配上 Hook，它改代码、自动检查、自动提交，一气呵成。

这才是 Claude Code 的正确打开方式。

## 参考来源

**核心项目：**

+   `claude-code-hooks-mastery` - disler 完整教程，1.9k 星

**官方文档：**

+   Hooks Reference - Claude Code Docs

+   Hooks Reference - Claude Docs

+   Claude Code Hook Control Flow

**社区资源：**

+   `awesome-claude-code`

+   ClaudeLog - Hooks Mechanics

+   GitButler - Claude Code Hooks

**深度教程：**

+   The Ultimate Claude Code Guide

+   Guide to Claude Code Hooks

+   Automate AI Workflows

**研究数据：**

+   Anthropic Productivity Research

+   Claude Code Best Practices

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
