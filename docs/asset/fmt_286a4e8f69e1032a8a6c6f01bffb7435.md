
# OpenClaw 完全指南

> 来源：[https://my.feishu.cn/docx/K70ud7P9ao0axgxpmXCccMIJnGc](https://my.feishu.cn/docx/K70ud7P9ao0axgxpmXCccMIJnGc)

> 一个能真正帮你干活的 AI 助手，不只是聊天。

---

## 一、OpenClaw 是什么？

普通的 AI（ChatGPT、Claude 网页版）是问答助手——你问，它答，永远停留在"对话"层面。

OpenClaw 不一样：它是一个能动手的 AI Agent，可以：

- 整理你的文件
- 发邮件、查数据、管日程
- 控制智能家居
- 执行 `shell` 命令
- 只要你用电脑能做的事，它基本都能替你做

核心特点：

- 开源免费 - 代码在 GitHub 上，随便看
- 本地部署 - 数据在你自己电脑上，不经过第三方服务器
- 记忆系统 - 能记住你说过的话，不是每次都从零开始
- 多渠道接入 - Discord、Telegram、飞书、WhatsApp 都能用
- 可扩展 - 通过 Skill 系统教它新技能

## 二、安装教程

### 2.1 环境要求

### 2.2 Windows 安装

```
# 1. 安装 Node.js（去 nodejs.org 下载安装包）

# 2. 以管理员身份打开 CMD，运行：
npm i -g openclaw

# 3. 运行配置向导
openclaw onboard --install-daemon
```

### 2.3 Mac 安装

```
# 方式一：一键安装
curl -fsSL https://openclaw.ai/install.sh | bash

# 方式二：NPM 安装（推荐）
npm i -g openclaw

# 运行配置向导
openclaw onboard --install-daemon
```

### 2.4 Linux 安装

```
# 安装 Node.js 22
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install -y nodejs

# 安装 OpenClaw
npm i -g openclaw

# 运行配置向导
openclaw onboard --install-daemon
```

### 2.5 配置 AI 模型

OpenClaw 本身免费，但需要连接大模型才能"思考"。

国内推荐（不需要🪜）：

海外推荐（需要🪜）：

获取 API Key 步骤：

1. 注册对应平台账号
1. 进入 API Keys 页面
1. 创建新密钥，复制保存

⚠️ 注意：不要用 OAuth 登录（用你的 ChatGPT Plus/Claude Pro 账号直接登录），有封号风险。

### 2.6 验证安装

```
# 检查状态
openclaw status

# 打开网页面板
openclaw dashboard
# 或直接访问 http://127.0.0.1:18789/

# 健康检查
openclaw doctor
```

### 2.7 常用命令

## 三、核心功能

### 3.1 记忆系统

OpenClaw 的记忆系统基于 Markdown 文件，存在 `~/.openclaw/workspace/` 目录下。

最低配置：只写 `SOUL.md` + `USER.md`，5 分钟搞定。

`SOUL.md` 示例：

```
# SOUL.md - 你是谁

你是小白，一个直接、高效的 AI 助手。

核心信条：
- 跳过废话，直接解决问题
- 不要"这是一个好问题"这种客套
- 能做的直接做，不确定的再问
- 隐私就是隐私，没得商量
```

`USER.md` 示例：

```
# USER.md

- 名字：你的名字
- 时区：Asia/Shanghai
- 偏好：沟通直接、步骤清晰
- 当前重点：工作/学习方向
```

### 3.2 技能系统（Skills）

Skill 就是一个 Markdown 文件，告诉 AI"遇到什么情况，按什么步骤执行"。不需要编程，任何人都能写。

新手必装技能：

安装方法：

```
# 方式一：聊天安装
# 直接对 OpenClaw 说："安装 self-improving-agent 技能"

# 方式二：命令行安装
npx clawhub@latest install self-improving-agent

# 方式三：手动安装
git clone https://github.com/xxx/skill-name.git ~/.openclaw/skills/skill-name
```

### 3.3 多渠道接入

OpenClaw 支持同时接入多个聊天渠道，消息不会串。

### 3.4 定时任务（Cron）

两种方式：

Cron 配置示例（每天早上 9 点发新闻摘要）：

```
{
  "name": "每日早报",
  "schedule": { "kind": "cron", "expr": "0 9 * * *", "tz": "Asia/Shanghai" },
  "payload": {
    "kind": "agentTurn",
    "message": "搜索今天的科技和 AI 领域新闻热点，整理成 5 条简报",
    "model": "haiku"
  },
  "sessionTarget": "isolated",
  "delivery": { "mode": "announce" }
}
```

## 四、进阶配置

### 4.1 解决 AI "失忆"问题

长对话接近上下文限制时，OpenClaw 会自动压缩旧对话，可能丢失细节。

解决方案：开启 `memoryFlush`，压缩前自动保存关键信息。

在 `~/.openclaw/openclaw.json` 添加：

```
{
  "agents": {
    "defaults": {
      "compaction": {
        "reserveTokensFloor": 20000,
        "memoryFlush": {
          "enabled": true,
          "softThresholdTokens": 4000
        }
      }
    }
  }
}
```

### 4.2 子 Agent 并行任务

让主 AI 派出子进程并行处理任务，完成后自动汇报。

配置模型分级（省 60-70% token）：

```
{
  "models": {
    "provider/claude-opus-4": { "alias": "opus" },
    "provider/claude-sonnet-4": { "alias": "sonnet" },
    "provider/claude-haiku-4": { "alias": "haiku" }
  }
}
```

### 4.3 开发自定义 Skill

每个 Skill 是一个目录，必须包含 `SKILL.md`：

```
skills/
  my-skill/
    SKILL.md
    scripts/    ← 可选
```

`SKILL.md` 写法：

```
---
name: ip-lookup
description: >
  IP 地址查询。触发条件：用户问 IP 地址、IP 归属地、
  "这个 IP 是哪里的"、"帮我查一下 XX.XX.XX.XX"等。
---

# IP 地址查询

## 步骤
1. 从用户消息中提取 IP 地址
2. 调用 http://ip-api.com/json/{IP}?lang=zh-CN
3. 解析 JSON，按格式回复

## 输出格式
🌐 IP 查询结果：{IP}
📍 位置：{country} {regionName} {city}

## 错误处理
- 返回 fail → 告诉用户"可能是内网 IP 或无效地址"
```

要点：`description` 决定触发率，要列出所有可能的触发词。

## 五、使用场景

### 5.1 个人知识管理

- 自动整理笔记、文件
- 日志记录和检索
- 项目状态追踪

### 5.2 信息雷达

- 每日自动推送新闻摘要
- 监控特定关键词
- 竞品动态追踪

### 5.3 内容创作辅助

- 素材收集整理
- 大纲生成
- 文案优化

### 5.4 自动化工作流

- 定时任务提醒
- 服务状态巡检
- 邮件/消息自动处理

### 5.5 编程辅助

- 代码审查
- 调试帮助
- 文档生成

## 六、相关资源

### 官方资源

### 教程文章

[OpenClaw_完整技能手册.md](https://tcntqlwu4es2.feishu.cn/wiki/Uh99wgXEdi4CeBkQp51cX8xwnbb?from=from_copylink)

### 技能资源

### API 提供商

## 七、常见问题

Q: 输入 `openclaw` 提示 `command not found`？

A: 关掉终端重新打开试试。还不行就检查 `npm` 全局路径：

```
echo 'export PATH=$(npm config get prefix)/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

Q: 网页面板打不开？

A:

1. 确认在运行：`openclaw status`
1. 没运行就启动：`openclaw gateway start`
1. 端口被占用就换端口：`openclaw gateway --port 18790`

Q: AI 不回复？

A:

1. 检查 API Key：`openclaw doctor`
1. 检查账户余额
1. 检查网络（海外模型需要🪜）

Q: 聊着聊着 AI "失忆"了？

A: 开启 `memoryFlush`，详见 4.1 节

Q: Discord Bot 在线但不回复？

A: 几乎肯定是 `MESSAGE CONTENT INTENT` 没开。去 Discord Developer Portal → Bot → Privileged Gateway Intents → 开启 `MESSAGE CONTENT INTENT`。

Q: 想重新配置？

A: 直接运行 `openclaw onboard`，会重新进入配置向导。

Q: 完全卸载重装？

```
openclaw gateway stop
npm uninstall -g openclaw
rm -rf ~/.openclaw
```

## 八、快速上手 Checklist

按这个顺序来，1 小时内搞定：

- 安装 Node.js（≥22）
- 安装 OpenClaw：`npm i -g openclaw`
- 运行配置向导：`openclaw onboard --install-daemon`
- 获取 API Key（推荐智谱 GLM-5）
- 验证安装：`openclaw status` + `openclaw dashboard`
- 写 `SOUL.md` 和 `USER.md`
- 装第一个技能：`self-improving-agent`
- 测试对话

---

最后更新：2026年3月9日
