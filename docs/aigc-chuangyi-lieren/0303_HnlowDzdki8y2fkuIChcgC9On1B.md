# OpenClaw 从新手到中级完整教程

> 来源：[https://aigcreative.feishu.cn/docx/IJl4dsBz6onCI1xcvRqcvZImn93](https://aigcreative.feishu.cn/docx/IJl4dsBz6onCI1xcvRqcvZImn93)

> 作者：X@**Nicolechan**

> 链接：https://x.com/stark_nico99/status/2026235176150581282

# 目录

# 什么是 OpenClaw

Op**enCl**aw（曾用名 ClawdBot/Moltbot）是 2026 年最火的开源 AI Agent 项目，GitHub 已获得超过 68,000 星标。它不是普通的聊天机器人，而是一个真正能够执行任务的个人 AI 助理。

**本地执行**

数据存储在你的设备上，无需上传云端

完全掌控隐私和数据安全

**真实执行**

不仅是对话，能实际操作你的电脑

自动化处理邮件、日历、文件管理等任务

**多平台消息**

支持 WhatsApp、Telegram、Discord、Slack 等 10+ 平台

从单一入口管理所有通讯

**持久记忆**

跨会话保存上下文和用户偏好

随着时间推移越来越了解你，持续提升效率

**开源免费**

完全开源，只需自备 API Key

免费，完全自主控制

OpenClaw vs 传统 AI 助手

传统 AI（如 ChatGPT）：

+   你：“帮我整理桌面文件”

+   AI：“我建议你可以这样做：1. 创建文件夹…… 2. 按类型分类……”

**OpenClaw**：

+   你：“帮我整理桌面文件”

+   OpenClaw：直接重命名、分类、移动文件 “已完成！我把 47 个文件按类型整理到 5 个文件夹中。”

这就是“聊天机器人”和“AI Agent”的本质区别。

# 新手阶段：基础入门

## 第一步：环境准备

系统要求

+   **操作系统**： macOS、Linux 或 Windows （需要 WSL）

+   **Node.js**: v18 或更高版本

+   **AI 模型 API Key**: Claude 或 GPT 的 API 密钥

**🔨 实践任务 1：检查环境**

检查 Node.js 版本

```
node --version
```

如果版本低于 v18，需要升级

macOS/Linux 使用 nvm

```
nvm install 18
```

```
nvm use 18
```

Windows 访问 [nodejs.org](https://nodejs.org/) 下载安装

**✅ 完成标准**： 终端显示 Node.js 版本 >= v18.0.0

## 第二步：安装 OpenClaw

方法一：NPM 安装（推荐新手）

```
# 全局安装 OpenClaw
npm install -g openclaw

# 验证安装
openclaw --version
```

方法二：Docker 安装（推荐有 Docker 经验者）

```
# 拉取镜像
docker pull openclaw/openclaw:latest

# 运行容器
docker run -d --name openclaw \
  -v ~/.openclaw:/root/.openclaw \
  openclaw/openclaw:latest
```

方法三：源码安装（开发者）

```
# 克隆项目
git clone https://github.com/openclaw/openclaw.git
cd openclaw

# 安装依赖
npm install

# 启动
npm run start
```

**🔨 实践任务 2：完成安装**

选择一种安装方式，完成 OpenClaw 的安装。

**✅ 完成标准**： 运行 openclaw --version 或 openclaw --help 能正常显示信息

## 第三步：初始化配置

运行向导

```
# 启动初始化向导
openclaw onboard
```

向导会引导你完成：

1.  选择 AI 模型提供商（Anthropic Claude / OpenAI GPT / 本地模型）

1.  输入 API Key

1.  选择消息平台（Telegram / Discord / WhatsApp 等）

1.  配置系统权限（建议先选择沙盒模式）

**配置 API Key**

如果你还没有 API Key，可以通过以下途径获取：

+   An[thropic Claud](http://anthropic.com/)e: anthropic.com（推荐，$20/月 Pro 订阅）

+   **OpenAI GPT**: platform.openai.com

+   **用自己的API 聚合服务**

**🔨 实践任务 3：完成初始化**

运行 openclaw onboard 并完成所有配置步骤。

**✅ 完成标准**：

+   成功配置 API Key

+   选择至少一个消息平台

+   完成权限设置（建议先选沙盒模式）

## 第四步：第一次对话

**启动 OpenClaw**

```
# 启动 OpenClaw
openclaw

# 或者启动 Dashboard（Web 界面）
openclaw dashboard
```

**连接消息平台

Tele**gram 示例：

1.  在 Telegram 中搜索

1.  [@BotFather](https://x.com/@BotFather)

1.  发送 /newbot 创建新机器人

1.  获取 Bot Token

1.  配置到 OpenClaw:

```
openclaw config set channels.telegram.botToken &quot;YOUR_BOT_TOKEN&quot;
openclaw config set channels.telegram.enabled true
```

1.  在 Telegram 中搜索你的机器人并开始对话

**🔨 实践任务 4：第一次对话**

与你的 OpenClaw 助手进行第一次对话，测试以下命令：

```
你好，介绍一下你自己
```

```
你能帮我做什么？
```

```
现在几点了？
```

**✅ 完成标准**： 机器人能正常回复你的消息

## 第五步：理解核心概念

**Gateway（网关）**

Gateway 是 OpenClaw 与外部世界交互的方式：

+   **消息网关**： Telegram、Discord、WhatsApp

+   **API 网关**： HTTP API 接口

+   **CLI 网关**： 命令行交互

**Skills（技能）**

Skills 是 OpenClaw 的能力扩展，类似于“插件”或“应用”：

+   每个 Skill 定义了一组特定任务

+   可以从 Clawhub 安装第三方 Skills

+   也可以自己编写自定义 Skills

**Memory（记忆）**

OpenClaw 会记住：

+   你的偏好和习惯

+   之前的对话上下文

+   重要的信息和任务

**Sandbox（沙盒）**

沙盒模式限制 OpenClaw 的系统访问权限，保护你的电脑安全：

+   **Sandbox Mode**: 限制文件系统、网络、Shell 访问

+   Full Access Mode: 完全权限（需谨慎使用）

🔨 **实践任务 5：探索工作空间**

```
# 查看 OpenClaw 的工作目录
ls ~/.openclaw

# 查看配置文件
openclaw config list

# 查看已安装的 Skills
openclaw skills list

# 运行安全审计
openclaw security audit
```

**✅ 完成标准**： 理解 OpenClaw 的文件结构和基本配置

# 进阶阶段：实用技能

## 第六步：安装和使用 Skills

浏览 Skills 市场

访问 [Clawhub](https://github.com/VoltAgent/awesome-openclaw-skills) 或使用命令：

```
# 搜索 Skills
openclaw skills search email

# 查看 Skill 详情
openclaw skills info @author/skill-name
```

安装常用 Skills

```
# 安装邮件管理 Skill
openclaw skills install @openclaw/email-manager

# 安装日历管理 Skill
openclaw skills install @openclaw/calendar

# 安装文件整理 Skill
openclaw skills install @openclaw/file-organizer

# 安装网页搜索 Skill（使用 Tavily 替代 Brave）
openclaw skills install @openclaw/tavily-search
```

🔨 实践任务 6：安装并测试 Skills

1.  安装至少 3 个 Skills

1.  测试每个 Skill 的功能

1.  记录哪些 Skills 对你最有用

**✅ 完成标准**： 成功安装并使用至少 3 个 Skills

## 第七步：Google Workspace 集成

OpenClaw 可以管理你的 Gmail、Google Calendar、Google Docs 等。

**配置步骤**

**1.** 创建 Google Cloud 项目

访问 console.cloud.google.com 创建新项目

启用 Gmail API、Google Calendar API、Google Drive API

**2.创建服务账号**

在 IAM & Admin → Service Accounts 创建服务账号

下载 JSON 密钥文件

**3.配置 OpenClaw**

```
openclaw config set integrations.google.enabled true
openclaw config set integrations.google.credentialsPath
&quot;/path/to/credentials.json&quot;
```

**4.授权访问**

```
openclaw integrations google authorize
```

🔨 实践任务 7：Google 集成实践

完成以下任务：

**1.日历管理**：让 OpenClaw 为你创建一个会议邀请

```
帮我在明天下午3点创建一个1小时的会议，主题是&quot;项目评审&quot;
```

**2.邮件处理**：让 OpenClaw 检查未读邮件

```
检查我的未读邮件，总结最重要的3封
```

**3.文档编辑**：创建一个 Google Doc

```
创建一个新的 Google Doc，标题是&quot;周报模板&quot;，包含本周工作总结的框架
```

**✅ 完成标准**： 成功完成至少 2 个 Google Workspace 任务

## 第八步：设置定时任务（Cron Jobs）

OpenClaw 可以定期自动执行任务。

**创建每日简报**

```
我想让你每天早上 8 点给我发送一份简报，包含：
1. 今天的天气
2. 我的日历安排
3. 未读邮件数量
4. 一句励志的话
```

OpenClaw 会自动创建一个定时任务（cron job）。

**查看定时任务**

```
# 列出所有定时任务
openclaw cron list

# 查看任务详情
openclaw cron show &lt;task-id&gt;

# 禁用任务
openclaw cron disable &lt;task-id&gt;

# 删除任务
openclaw cron delete &lt;task-id&gt;&lt;/task-id&gt;&lt;/task-id&gt;&lt;/task-id&gt;
```

🔨 实践任务 8：创建定时任务

创建以下定时任务：

1.  **每日简报**：每天早上发送天气和日程

1.  **每周总结**：每周五下午发送本周统计

1.  **定时提醒**：每天下午 6 点提醒你结束工作

**✅ 完成标准**： 至少创建 2 个定时任务并验证它们能正常运行

## 第九步：配置持久记忆

OpenClaw 的记忆系统让它能记住你的偏好。

**训练你的助手**

告诉 OpenClaw 关于你的信息：

```
记住以下关于我的信息：
- 我的名字是 [你的名字]
- 我的工作是 [你的职业]
- 我的工作时间是周一到周五 9:00-18:00
- 我喜欢简洁的回复，不要太啰嗦
- 我使用中文交流
- 我的时区是 GMT+8
```

**查看记忆**

```
# 查看记忆文件
cat ~/.openclaw/memory/long-term.json

# 或在对话中询问
你记得我的哪些信息？
```

🔨 实践任务 9：个性化你的助手

1.  给你的助手起个名字

1.  告诉它你的基本信息和偏好

1.  设置你的工作习惯和时间安排

1.  测试它是否记住了这些信息

**✅ 完成标准**： OpenClaw 能在对话中体现出对你的了解

# 中级阶段：高级应用

## 第十步：编写自定义 Skill

Skills 使用 YAML 或 Markdown 格式定义。

Skill 基本结构

创建文件 ~/.openclaw/skills/my-first-skill.yaml:

name: "每日新闻摘要"

description: "获取并总结今日科技新闻"

version: "1.0.0"

```
triggers:
  - &quot;今日新闻&quot;
  - &quot;科技新闻&quot;

steps:
  - action: web_search
    query: &quot;latest tech news today&quot;
    max_results: 5
    
  - action: summarize
    content: &quot;{{search_results}}&quot;
    style: &quot;bullet_points&quot;
    
  - action: respond
    message: &quot;📰 今日科技新闻摘要：

{{summary}}&quot;
```

**安装自定义 Skill**

```
# 重新加载 Skills
openclaw skills reload

# 测试 Skill
openclaw skills test &quot;每日新闻摘要&quot;
```

🔨 实践任务 10：创建自定义 Skill

创建一个自定义 Skill，实现以下功能之一：

1.  **天气助手**：查询指定城市的天气并格式化输出

1.  **任务管理器**：添加、查看、完成待办事项

1.  **网站监控**：定期检查某个网站是否可访问

1.  **价格追踪**：监控某个商品的价格变化

**✅ 完成标准**： 成功创建并运行一个自定义 Skill

## 第十一步：多 Agent 管理

你可以运行多个 OpenClaw 实例，每个有不同的配置和用途。

**创建多个 Agent**

```
# 创建工作用 Agent
openclaw create-agent work
openclaw config --agent work set ai.model &quot;claude-sonnet-4.6&quot;

# 创建个人用 Agent
openclaw create-agent personal
openclaw config --agent personal set ai.model &quot;gpt-5.3&quot;

# 切换 Agent
openclaw switch-agent work
openclaw switch-agent personal

# 列出所有 Agent
openclaw list-agents
```

**使用场景**

+   **工作 Agent**: 连接公司邮箱、项目管理工具

+   **个人 Agent**: 管理个人日程、家庭事务

+   **实验 Agent**: 测试新功能、新 Skills

🔨 实践任务 11：配置多 Agent

创建至少 2 个不同用途的 Agent，并为它们配置不同的：

+   AI 模型

+   消息平台

+   Skills

+   权限级别

**✅ 完成标准**： 能够在不同 Agent 之间切换并使用

## 第十二步：Docker 沙盒安全实践

Docker 沙盒提供最强的安全隔离。

**配置 Docker 沙盒**

```
# 安装 Docker（如果还没有）
# macOS: 下载 Docker Desktop
# Linux: sudo apt install docker.io

# 配置 OpenClaw 使用 Docker 沙盒
openclaw config set sandbox.mode &quot;docker&quot;
openclaw config set sandbox.docker.image &quot;openclaw/sandbox:latest&quot;

# 测试沙盒
openclaw sandbox test
```

**沙盒限制**

Docker 沙盒会限制：

+   文件系统访问（只能访问指定目录）

+   网络访问（可配置白名单）

+   系统命令执行

+   资源使用（CPU、内存限制）

**🔨 实践任务 12：安全配置**

1.  启用 Docker 沙盒模式

1.  运行安全审计：openclaw security audit --deep

1.  根据审计结果修复安全问题

1.  测试沙盒限制是否生效

**✅ 完成标准**： 安全审计通过，沙盒正常工作

## 第十三步：浏览器控制

OpenClaw 可以控制浏览器执行复杂的网页操作。

**启用浏览器控制**

```
# 安装浏览器控制插件
openclaw plugins install @openclaw/browser-control

# 配置浏览器
openclaw config set browser.enabled true
openclaw config set browser.headless false  # false 表示显示浏览器窗口
```

**使用示例**

```
打开浏览器，访问 GitHub，搜索 &quot;openclaw&quot;，并告诉我前 3 个结果
```

```
帮我在亚马逊上搜索 &quot;机械键盘&quot;，找到评分最高的 3 个产品，记录价格
```

🔨 实践任务 13：浏览器自动化

使用浏览器控制完成以下任务：

1.  **信息收集**：从某个网站抓取数据并整理

1.  **表单填写**：自动填写一个在线表单

1.  **价格比较**：在多个网站比较同一商品的价格

**✅ 完成标准**： 成功完成至少 1 个浏览器自动化任务

## 第十四步：与其他工具集成

**Linear 集成（项目管理）**

```
# 安装 Linear Skill
openclaw skills install @openclaw/linear

# 配置 API Key
openclaw config set integrations.linear.apiKey &quot;YOUR_LINEAR_API_KEY&quot;
```

使用示例：

```
在 Linear 中创建一个新任务：&quot;修复登录页面的 bug&quot;，优先级高
```

**Obsidian 集成（笔记）**

```
# 安装 Obsidian Skill
openclaw skills install @openclaw/obsidian

# 配置 Obsidian vault 路径
openclaw config set integrations.obsidian.vaultPath &quot;/path/to/obsidian/vault&quot;
```

使用示例：

```
在 Obsidian 中创建一篇新笔记，标题是&quot;OpenClaw 学习笔记&quot;
```

🔨 实践任务 14：集成第三方工具

选择至少 2 个你常用的工具，将它们与 OpenClaw 集成：

+   **项目管理**： Linear, Jira, Trello

+   **笔记**： Obsidian, Notion, Evernote

+   **代码**： GitHub, GitLab

+   **通讯**： Slack, Discord, Teams

+   **其他**： 根据你的需求

**✅ 完成标准**： 成功集成并使用至少 2 个第三方工具

## 第十五步：高级配置优化

**性能优化**

```
# 配置缓存
openclaw config set cache.enabled true
openclaw config set cache.ttl 3600

# 配置并发请求
openclaw config set ai.maxConcurrentRequests 3

# 配置超时
openclaw config set ai.timeout 30000
```

**成本控制**

```
# 设置每日 API 调用限制
openclaw config set ai.dailyLimit 1000

# 设置每月预算（美元）
openclaw config set ai.monthlyBudget 50

# 查看使用统计
openclaw stats usage
openclaw stats cost
```

**日志和调试**

```
# 启用详细日志
openclaw config set logging.level &quot;debug&quot;

# 查看日志
openclaw logs

# 查看特定时间的日志
openclaw logs --since &quot;2026-02-20&quot;

# 导出日志
openclaw logs --export logs.txt
```

**🔨 实践任务 15：优化配置**

1.  根据你的使用情况优化性能配置

1.  设置成本控制，避免意外高额费用

1.  配置日志，方便调试问题

1.  监控一周的使用情况和成本

**✅ 完成标准**：

+   配置了成本控制

+   能查看使用统计

+   理解日志系统

## 实践任务清单

新手阶段（必做）

+   **任务 1**: 检查并安装 Node.js v18+

+   **任务 2**: 完成 OpenClaw 安装（选择一种方式）

+   **任务 3**: 运行初始化向导，配置 API Key

+   **任务 4**: 与 OpenClaw 完成第一次对话

+   **任务 5**: 探索工作空间和配置文件

进阶阶段（推荐）

+   **任务 6**: 安装并测试至少 3 个 Skills

+   **任务 7**: 完成 Google Workspace 集成（至少 2 个功能）

+   **任务 8**: 创建至少 2 个定时任务

+   **任务 9**: 个性化你的助手，配置记忆系统

中级阶段（进阶）

+   **任务 10**: 创建一个自定义 Skill

+   **任务 11**: 配置多个 Agent 用于不同场景

+   **任务 12**: 启用 Docker 沙盒并通过安全审计

+   **任务 13**: 完成一个浏览器自动化任务

+   **任务 14**: 集成至少 2 个第三方工具

+   **任务 15**: 优化配置并设置成本控制

实战项目（挑战）

+   **项目 1**: 构建一个自动化工作流，每天早上发送个性化简报

+   **项目 2**: 创建一个邮件自动分类和回复系统

+   **项目 3**: 搭建一个多渠道消息聚合中心

+   **项目 4**: 开发一个价格监控和提醒系统

+   **项目 5**: 构建一个自动化内容发布系统（博客/社交媒体）

# 常见问题解答

## Q1: OpenClaw 的技术要求是什么？

OpenClaw 主要面向有一定技术背景的用户。你需要：

+   会使用命令行

+   理解环境变量配置

+   了解 API Key 的概念

但安装过程已经大大简化，只要你能运行 npm 命令，就能使用 OpenClaw。

## Q2: 如何管理安全风险？

**关键安全建议**：

1.  **使用沙盒模式**：初期测试时使用沙盒模式

1.  **不要存储敏感密码**：不要在配置文件中存储明文密码

1.  **定期审查**：定期检查自动化规则

1.  **理解权限**：只启用你需要的权限

1.  **专用设备**：建议在专用设备上运行（如 Mac Mini、旧笔记本）

1.  **独立账号**：给 OpenClaw 创建独立的邮箱和账号

## Q3: API 成本大概是多少？

取决于使用强度：

+   **轻度使用**（每天 10-20 次对话）：约 $5-10/月

+   **中度使用**（每天 50-100 次对话 + 定时任务）：约 $20-30/月

+   **重度使用**（大量自动化 + 浏览器控制）：约 $50-100/月

**省钱技巧**：

+   使用 API 聚合服务获取更优惠的价格

+   设置每日/每月使用限制

+   优先使用较便宜的模型

+   新用户通常有免费额度

## Q4: 可以使用本地模型吗？

可以！OpenClaw 支持通过 Ollama 连接本地模型：

```
# 安装 Ollama
curl -fsSL https://ollama.ai/install.sh | sh

# 下载模型
ollama pull llama3.2

# 配置 OpenClaw
openclaw config set ai.provider &quot;ollama&quot;
openclaw config set ai.model &quot;llama3.2&quot;
openclaw config set ai.baseUrl &quot;http://localhost:11434&quot;
```

**注意**：本地模型的能力通常不如云端大模型，复杂任务可能表现不佳。

## Q5: OpenClaw 支持中文吗？

完全支持！OpenClaw 有中文社区维护的汉化版本：

+   GitHub: MaoTouHU/OpenClawChinese

+   提供中文界面和中文文档

+   每小时自动同步官方更新

你也可以使用英文版，它能理解和回复中文。

## Q6: 如何备份我的配置？

```
# 备份整个工作目录
cp -r ~/.openclaw ~/.openclaw-backup

# 或使用 Git 同步（推荐）
cd ~/.openclaw
git init
git add .
git commit -m &quot;Initial backup&quot;
git remote add origin YOUR_GITHUB_REPO
git push -u origin main
```

OpenClaw 支持 GitHub 同步功能，可以自动备份配置。

## Q7: 遇到问题如何调试？

```
# 1. 查看日志
openclaw logs

# 2. 运行健康检查
openclaw doctor

# 3. 运行安全审计
openclaw security audit

# 4. 启用调试模式
openclaw config set logging.level &quot;debug&quot;
openclaw

# 5. 查看配置
openclaw config list
```

Q8: 可以在服务器上运行吗？

可以！很多用户在 VPS 或云服务器上运行 OpenClaw：

**推荐平台**：

+   DigitalOcean（有一键部署）

+   AWS EC2

+   阿里云轻量应用服务器

+   Cloudflare Workers（$5/月方案）

**注意事项**：

+   确保服务器安全配置

+   使用 HTTPS 连接

+   配置防火墙

+   定期更新系统

## 学习资源

官方资源

+   **官方网站**： https://openclaw.ai/

+   **GitHub 仓库**： github.com/openclaw/openclaw

+   **官方文档**： https://docs.openclaw.ai/zh-CN

+   **Skills 市场**： [github.com/VoltAgent/awe](https://github.com/VoltAgent/awesome-openclaw-skills)some-openclaw-skills

视频教程

+   freeCodeC[amp 完整教](https://youtu.be/n1sfrc-RjyM)程 (1 小时）: YouTube

+   **OpenClaw 新手入门** (30 分钟）: [YouTube](https://www.youtube.com/watch?v=BoC5MY_7aDk)

+   30[ 分钟精通 O](https://youtu.be/ji_Sd4si7jo)penClaw: YouTube

+   **OpenClaw 速成课程**： [YouTube](https://www.youtube.com/watch?v=u4ydH-QvPeg)

文字教程

+   freeCodeCamp 新手教程： freecodecamp.org

+   DigitalOcean 部署教程： digitalocean.com

+   **Reddit 详细指南**： reddit.com/r/clawdbot

中文资源

+   **CSDN 汉化版教程**： [blog.csdn.net](https://blog.csdn.net/qq_44866828/article/details/157876493)

+   **OpenClaw 中文使用教程**： [lua.ren](https://lua.ren/openclaw/2026-02-15-openclaw-tutorial/index.html)

+   **阿里云部署教程**： [help.aliyun.com](https://help.aliyun.com/zh/simple-application-server/use-cases/quickly-deploy-and-use-openclaw)

+   **Apifox 安装指南**： [apifox.com](https://apifox.com/apiskills/openclaw-installation-and-usage-guide/)

社区

+   **Reddit**: r/clawdbot, r/AiForSmallBusiness

+   **Discord**: OpenClaw 官方 Discord 服务器

+   GitHub Discussions: 在 GitHub 仓库的 Discussions 区提问

+   **中文社区**： MaoTouHU/OpenClawChinese

## 下一步

完成这个教程后，你应该已经掌握了 OpenClaw 从基础到中级的大部分核心技能。接下来你可以：

1.  **深入某个领域**：选择一个你最感兴趣的功能（如浏览器自动化、邮件管理等）深入研究

1.  **参与社区**：在 GitHub 上贡献代码，或在 Clawhub 上分享你的 Skills

1.  **构建实战项目**：用 OpenClaw 解决你实际工作或生活中的问题

1.  **探索高级功能**：研究多 Agent 协作、自定义插件开发等高级话题

OpenClaw 代表了 AI 助手的下一步：从“会说话的工具”到“会做事的助手”。对于愿意花时间配置的用户来说，它可以成为真正的数字分身。

祝你在 OpenClaw 的探索之旅中收获满满！🦞✨

**最后更新****： ****20**26 年 2 月 24 日

版本： 1.0

作者： YouMind 基于最新网络资源整理

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>

# 知识库动态