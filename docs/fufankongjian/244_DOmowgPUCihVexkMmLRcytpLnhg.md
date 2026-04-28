# part3-02-记忆与配置修改

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/HVaPdHiK7ofKX5xdX7gcZsB2nvf](https://kq4b3vgg5b.feishu.cn/docx/HVaPdHiK7ofKX5xdX7gcZsB2nvf)

# Part 2：OpenClaw 记忆与配置修改

> 给 OpenClaw 接上"大脑"、带它认识你、让它开口说第一句话——完成这三步，你就拥有了一个真正能干活的 AI 助手。

## 引言

上一章结束时，我们在终端上看到了 OpenClaw 的版本号和 doctor 诊断报告。安装是成功了，但说句实话——它现在还什么都干不了。

原因很简单：OpenClaw 本身并不包含一个大模型。它是一个"助手操作系统"，是指挥官、调度员、工具箱——但它没有自带"大脑"。就像你刚组装好一台电脑，硬件齐全了，但还没装操作系统、没连上网，打开电源只有一个光标在那闪。

这一章，我们要做四件事：

1.  **给它接上大脑** —— 注册大模型 API，让 OpenClaw 有"思考能力"

1.  **完成首次配置** —— 初始化所有配置文件，设置好模型提供商

1.  **让它开口说话** —— 启动 Gateway，打开 Dashboard，进行第一次对话

1.  **教它认识你** —— 修改 Agent 人设和记忆，让它变成"你的"助手

完成之后，你的 OpenClaw 不仅会说话、会思考，你还可以给它改名字、改性格——甚至让它用东北话和你聊天。

---

## 2.1 什么是模型调用？大模型 API 介绍

### 为什么 OpenClaw 需要大模型 API？

在上手操作之前，有三个概念必须先搞清楚。它们不难，但会贯穿后面所有章节。

先回答最基本的问题：OpenClaw 为什么不能像 ChatGPT 一样直接聊天？

因为 OpenClaw 的定位不是"聊天机器人"，而是"助手操作系统"。它管理技能、记忆、消息通道、定时任务……但它自己并不"思考"。思考的工作，交给外部的大模型来做。

> OpenClaw 是手机，大模型是 SIM 卡。 手机再好——屏幕多大、芯片多快——不插 SIM 卡就打不了电话。OpenClaw 再强大，不接入大模型 API 就无法回答你的问题。

```
flowchart LR
    A[&quot;你（用户）&quot;] --&gt;|&quot;发送消息&quot;| B[&quot;OpenClaw Gateway&quot;]
    B --&gt;|&quot;调用 API&quot;| C[&quot;大模型（DeepSeek / GPT / Kimi）&quot;]
    C --&gt;|&quot;返回回答&quot;| B
    B --&gt;|&quot;展示结果&quot;| A
```

### API 是什么？

API 的全称是 Application Programming Interface（应用程序编程接口）。名字很唬人，但概念很简单：

> **API 就像餐厅的菜单。** 你（用户）通过菜单（API）向厨房（大模型）点菜（发请求），厨房做好菜（计算结果）端给你。你不需要会做饭，只需要会看菜单、会点菜。

OpenClaw 就是帮你"点菜"的服务员——它把你的请求翻译成 API 能理解的格式，发给大模型，再把结果翻译成你能读懂的回答。

### API Key 是什么？

**API **Key 是你调用 API 时的身份凭证。

> **类比：会员卡。** 去餐厅吃饭要先办会员卡，卡号就是你的 API Key。每次下单时出示会员卡，系统验证你的身份、从你的余额中扣费。

API Key 的格式通常是一串字母和数字的组合，比如 sk-abc123def456...。拿到之后要像密码一样保管——不要截图发到群里、不要提交到 GitHub、不要写在任何公开的地方。

> ⚠️ **安全提醒**：API Key 泄露的后果比密码泄露还严重——别人拿着你的 Key 可以无限制地调用大模型 API，消耗你的余额，产生天价账单。如果你怀疑 Key 泄露了，立即去平台官网删除旧 Key 并创建新的。

### Token 是什么？

**大模型 A**PI 不按"次数"收费，而是按 Token 数量收费。

Token 是文本的最小计量单位。大致换算：

| 文本类型 | 大约 Token 数 | 举例 |
| --- | --- | --- |
| 1 个中文字 | 1-2 tokens | &quot;你&quot; ≈ 1 token |
| 1 个英文单词 | 1 token | &quot;hello&quot; ≈ 1 token |
| 100 tokens | ≈ 75 英文单词 ≈ 50 中文字 | 约一小段话 |
| 1000 tokens | ≈ 750 英文单词 ≈ 500 中文字 | 约一页 A4 纸 |

> **类比：打电话按分钟计费。** 你发给大模型的消息（输入）和它返回的回答（输出）都要消耗 token。输入和输出的单价不同，通常输出比输入贵。

用一张表总结这三个核心概念：

| 概念 | 一句话解释 | 类比 | 你需要做的 |
| --- | --- | --- | --- |
| **API** | 调用大模型的标准接口 | 餐厅菜单 | 不需要操心，OpenClaw 帮你处理 |
| **API Key** | 你的身份凭证 | 会员卡号 | 去平台注册获取，妥善保管 |
| **Token** | 文本的最小计量单位 | 电话按分钟计费 | 了解即可，后面会讲费用控制 |

> **怎么看这张表**：第一列是概念，第二列是技术解释，第三列是生活类比，最后一列告诉你需要做什么。搞清楚这三个概念，接下来的操作就不会迷路了。

---

## 2.2 大模型 API 注册与接入：三大平台实操

### 三大推荐平台概览

OpenClaw 支持几乎所有兼容 OpenAI 协议的大模型 API。面对琳琅满目的选项，我给你推荐三个最适合入门的平台：

| 平台 | 推荐度 | 免费额度 | 是否需翻墙 | 推荐模型 | 定价（输出） | 适合人群 |
| --- | --- | --- | --- | --- | --- | --- |
| **DeepSeek** | ⭐⭐⭐ 首选 | 注册送 500 万 tokens | 不需要 | deepseek-chat (V3.2) | $1.10/M | 所有人 |
| **Kimi Code** | ⭐⭐ 备选 | 有免费额度 | 不需要 | moonshot-v1-8k | 按量计费 | 国内用户 |
| **OpenAI** | ⭐ 进阶 | 无 | **需要翻墙** | gpt-5-mini | $2.00/M | 有梯子的用户 |

> **怎么看这张表**：横向对比时重点看"免费额度"和"是否需翻墙"两列。DeepSeek 在这两项上都是最优选——免费额度最多（500 万 tokens 足够用很久），而且国内直连。如果你没有翻墙工具，直接排除 OpenAI。

我的建议：直接选 DeepSeek。 原因很简单——免费额度最多、国内直连速度快、模型能力强劲。后面的操作我们也以 DeepSeek 为主来演示。

> 💡 实**用建议：**不要纠结选哪个平台——先用 DeepSeek 把流程走通，等你熟悉了 OpenClaw 的配置方式之后，随时可以添加其他平台。OpenClaw 支持同时配置多个模型提供商，切换只需要一条命令。

### 方案一：DeepSeek API 注册（推荐首选）

最新一代DeepSeek V3.2 模型在多个基准测试中表现优异，而价格只有 GPT-5-Mini 的十分之一。更重要的是：注册就送 500 万免费 tokens，不需要绑信用卡。

+   官网地址：https://www.deepseek.com/

**注册步骤：**

Step 1：访问 DeepSeek 开放平台

打开浏览器，访问 platform.deepseek.com，点击右上角的"注册"按钮。

**Step 2：完成注册**

用手机号完成注册。注册成功后，系统自动赠送 500 万免费 tokens——你会在 Dashboard 的余额页面看到。

Step 3：创建 API Key

**进入 Dashb**oard →** 左侧菜单 A**PI Keys → 点击 创建新 Key → 给你的 Key 起个名字（比如"OpenClaw"），点击创建。

> ⚠️ **重要提醒**：API Key 创建后**只显示一次**！点击"复制"按钮把它保存到安全的地方（比如备忘录或密码管理器）。关闭页面后就再也看不到完整的 Key 了——只能删除重建。

**Step 4：确认余额**

在 Dashboard 的 **Usage** 页面，确认你的账户有 500 万免费 tokens 的余额。

**DeepSeek 定价速览：**

| 模型 | 缓存命中输入 | 新输入 | 输出 | 上下文窗口 |
| --- | --- | --- | --- | --- |
| deepseek-chat (V3.2) | $0.07/M | $0.27/M | $1.10/M | 128K |
| deepseek-reasoner (V3.2) | $0.14/M | $0.55/M | $2.19/M | 128K |

### 方案二：Kimi Code（备选方案）

Kimi Code 是 Moonshot（月之暗面）推出的编程 AI 服务，与 OpenClaw 兼容。

+   官网地址：https://www.kimi.com/

**注册步骤：**

**Step 1**：访问 Kimi Code 官网，用手机号注册

**Step 2**：在设置页面获取 API Key

**Step 3**：记录 Base URL 和 Key 格式

Kimi Code 的优势是国内直连速度快，且对中文的理解非常好。如果 DeepSeek 遇到服务不稳定的情况（高峰期偶尔会有排队），Kimi Code 是一个不错的备用选择。

### 方案三：OpenAI API（进阶可选）

OpenAI 是大模型领域的"鼻祖"，GPT-5-Mini、GPT-5.4 都是业界顶级模型。但对于国内用户来说有两个门槛：

1.  **需要翻墙**才能访问 platform.openai.com

1.  **需要绑定信用卡**才能使用 API（没有免费额度）

如果你有海外支付手段并且已经在使用梯子，OpenAI 仍然是最成熟、生态最完善的选择。

**注册步骤简述**：访问 platform.openai.com → 注册账号 → API Keys → 创建新 Key。

### 补充说明：阿里百炼 Coding Plan

你可能在一些教程中看到过推荐"阿里百炼 Coding Plan"——这是阿里云专门为编程工具优化的 API 套餐。但截至目前（2026 年 3 月），Coding Plan 已停止申请（名额已满），暂时无法新注册。

如果你已有 Coding Plan 账号：

+   Key 格式：sk-sp-xxxxx（注意：与百炼通用 Key 不通用）

+   Base URL：https://coding.dashscope.aliyuncs.com/v1

> ⚠️ **特别注意****：Cod**ing Plan 的 Key 和百炼平台的通用 Key 是两套体系，不能混用。如果你用错了 Key，会看到 401 Unauthorized 的报错。

## 2.3.1 方案二，使用OpenClaw Onboard命令设置OpenClaw

```
openclaw onboard
```

+   官网地址：https://platform.moonshot.cn/

## 2.3.2 方案二，使用setup设置OpenClaw

拿到 API Key 之后，我们需要告诉 OpenClaw 两件事：

1.  **去哪里调用大模型**（Base URL）

1.  **用什么身份调用**（API Key）

### 第一步：初始化配置目录

OpenClaw 有一个专门的初始化命令 openclaw setup，它会自动创建所有必要的配置文件和目录。

```
# openclaw setup 的含义：
#   openclaw → OpenClaw 命令行工具
#   setup    → 子命令，执行首次初始化
#              创建配置文件、工作空间目录、会话存储目录
$ openclaw setup
Wrote ~/.openclaw/openclaw.json
Workspace OK: ~/.openclaw/workspace
Sessions OK: ~/.openclaw/agents/main/sessions
```

这条命令做了三件事：

| 创建的内容 | 路径 | 用途 |
| --- | --- | --- |
| 主配置文件 | ~/.openclaw/openclaw.json | 控制模型、Gateway、Agent 的所有行为 |
| 工作空间 | ~/.openclaw/workspace/ | 包含 SOUL.md、USER.md 等 Agent 人设文件 |
| 会话存储 | ~/.openclaw/agents/main/sessions/ | 保存所有对话记录 |

> 💡 关**于 o**penclaw onboard：OpenClaw **还有一个**交互式向导命令 openclaw onboard，会一步步引导你选择模型、输入 API Key、配置 Gateway。但在课程中，我们选择手动配置的方式——这样你不仅能完成配置，还能理解每一步背后的原理。

### 第二步：配置 API Key（.env 文件）

OpenClaw 把敏感信息（如 API Key）存放在一个单独的 .env 环境变量文件中，和主配置文件分离。这是一个好的安全实践——配置文件可以公开讨论，但 .env 文件永远不应该泄露。

用文本编辑器创建 ~/.openclaw/.env 文件：

```
# 用 nano 编辑器创建 .env 文件
# nano → macOS 自带的命令行文本编辑器（简单易用）
# ~/.openclaw/.env → 要创建/编辑的文件路径
$ nano ~/.openclaw/.env
```

写入以下内容（把 sk-your-deepseek-key 替换成你自己的 Key）：

```
# DeepSeek API Key（把下面的值替换成你自己的 Key）
DEEPSEEK_API_KEY=sk-your-deepseek-key
```

保存退出（nano 中按 Ctrl + X，然后按 Y 确认，回车保存）。

验证文件内容：

```
$ cat ~/.openclaw/.env
DEEPSEEK_API_KEY=sk-your-deepseek-key
```

> ⚠️ **安全提醒**：.env 文件中存储的是你的 API Key，相当于密码。不要截图发给别人，不要上传到任何公开平台。如果你用 Git 管理配置，务必把 .env 加入 .gitignore。

### 第三步：配置模型提供商（openclaw.json）

接下来要告诉 OpenClaw："我要用 DeepSeek 作为大模型提供商。"

我们使用 OpenClaw 自带的 config set 命令来修改配置。这比直接编辑 JSON 文件更安全——OpenClaw 有严格的 JSON Schema 校验，手动编辑一不小心写错格式，Gateway 会拒绝启动。

**配置模型合并模式：**

```
# openclaw config set 的含义：
#   config set → 修改配置项
#   models.mode → 要修改的配置键（用 . 分隔层级）
#   merge → 值：合并模式（新配置合并到已有配置中，不覆盖）
$ openclaw config set models.mode merge
Updated models.mode. Restart the gateway to apply.
```

> merge** 模**式表示新的 provider 配置会合并到已有配置中，不会覆盖其他设置。另一个选项是 replace——完全替换，一般不推荐。

配置 DeepSeek 提供商：

```
# 配置 DeepSeek 模型提供商
# models.providers.deepseek → 在 providers 下创建名为 deepseek 的配置块
# 后面的 JSON 包含连接信息和模型定义
$ openclaw config set models.providers.deepseek &#x27;{
  &quot;baseUrl&quot;: &quot;https://api.deepseek.com/v1&quot;,
  &quot;apiKey&quot;: &quot;${DEEPSEEK_API_KEY}&quot;,
  &quot;api&quot;: &quot;openai-completions&quot;,
  &quot;models&quot;: [{
    &quot;id&quot;: &quot;deepseek-chat&quot;,
    &quot;name&quot;: &quot;DeepSeek Chat (v3.2)&quot;,
    &quot;reasoning&quot;: false,
    &quot;input&quot;: [&quot;text&quot;],
    &quot;cost&quot;: {&quot;input&quot;: 2.8e-7, &quot;output&quot;: 4.2e-7},
    &quot;contextWindow&quot;: 128000,
    &quot;maxTokens&quot;: 8192
  }]
}&#x27;
Updated models.providers.deepseek. Restart the gateway to apply.
```

来逐字解释这段 JSON 配置：

| 字段 | 值 | 含义 |
| --- | --- | --- |
| baseUrl | https://api.deepseek.com/v1 | DeepSeek API 的地址 |
| apiKey | ${DEEPSEEK_API_KEY} | 从 .env 文件读取 Key（${...} 是变量引用语法） |
| api | openai-completions | API 协议类型（DeepSeek 兼容 OpenAI 协议） |
| models[0].id | deepseek-chat | 模型 ID（API 调用时使用） |
| models[0].name | DeepSeek Chat (v3.2) | 显示名称（Dashboard 中显示） |
| models[0].reasoning | false | 不启用推理模式（省 token） |
| models[0].contextWindow | 128000 | 上下文窗口大小（128K tokens） |
| models[0].maxTokens | 8192 | 单次最大输出 tokens |

> **怎么看这张表**：最重要的是 baseUrl 和 apiKey——前者告诉 OpenClaw "去哪里调用"，后者告诉它"用什么身份调用"。其他字段是模型的元数据信息。

**设置默认模型：**

```
# 设置默认使用的模型
# agents.defaults.model.primary → Agent 默认使用的主模型
# &quot;deepseek/deepseek-chat&quot; → 格式为 &quot;提供商名/模型ID&quot;
$ openclaw config set agents.defaults.model.primary &quot;deepseek/deepseek-chat&quot;
Updated agents.defaults.model.primary. Restart the gateway to apply.
```

### 第四步：验证配置文件

让我们看一下最终的配置文件长什么样：

```
$ cat ~/.openclaw/openclaw.json
```

输出（已格式化）：

```
{
  &quot;meta&quot;: {
    &quot;lastTouchedVersion&quot;: &quot;2026.3.13&quot;,
    &quot;lastTouchedAt&quot;: &quot;2026-03-18T08:34:27.636Z&quot;
  },
  &quot;models&quot;: {
    &quot;mode&quot;: &quot;merge&quot;,
    &quot;providers&quot;: {
      &quot;deepseek&quot;: {
        &quot;baseUrl&quot;: &quot;https://api.deepseek.com/v1&quot;,
        &quot;apiKey&quot;: &quot;${DEEPSEEK_API_KEY}&quot;,
        &quot;api&quot;: &quot;openai-completions&quot;,
        &quot;models&quot;: [
          {
            &quot;id&quot;: &quot;deepseek-chat&quot;,
            &quot;name&quot;: &quot;DeepSeek Chat (v3.2)&quot;,
            &quot;reasoning&quot;: false,
            &quot;input&quot;: [&quot;text&quot;],
            &quot;cost&quot;: { &quot;input&quot;: 2.8e-7, &quot;output&quot;: 4.2e-7 },
            &quot;contextWindow&quot;: 128000,
            &quot;maxTokens&quot;: 8192
          }
        ]
      }
    }
  },
  &quot;agents&quot;: {
    &quot;defaults&quot;: {
      &quot;model&quot;: {
        &quot;primary&quot;: &quot;deepseek/deepseek-chat&quot;
      },
      &quot;workspace&quot;: &quot;/Users/你的用户名/.openclaw/workspace&quot;
    }
  },
  &quot;gateway&quot;: {
    &quot;mode&quot;: &quot;local&quot;
  }
}
```

用一张表对照每个字段的含义：

| 字段 | 含义 | 重要程度 |
| --- | --- | --- |
| meta | 元数据，记录最后修改的版本和时间 | ℹ️ 自动维护 |
| models.mode | merge = 合并配置，不覆盖已有设置 | ⭐⭐ |
| models.providers.deepseek | DeepSeek API 的连接信息 | ⭐⭐⭐ 核心 |
| models.providers.deepseek.apiKey | 通过 ${DEEPSEEK_API_KEY} 引用 .env 中的密钥 | ⭐⭐⭐ 核心 |
| agents.defaults.model.primary | 默认使用的模型 | ⭐⭐⭐ 核心 |
| gateway.mode | local = 本地模式 | ⭐⭐ |

> **怎么看这张表**：标注"⭐⭐⭐ 核心"的三项是你后续最可能需要修改的配置。如果以后想添加其他模型提供商（比如 Kimi Code 或 OpenAI），只需要在 providers 下面加一个新的配置块，再在 .env 中加上对应的 API Key 即可。

> ⚠️ **重要提醒****：Open****Claw 有严**格的 JSON Schema 校验。推荐使用 openclaw config set 命令修改配置，不建议直接手动编辑 JSON 文件。一个多余的逗号、一个缺少的引号，都会导致 Gateway 拒绝启动。

---

## 2.4 启动 Gateway 与打开 Dashboard

### Gateway 是什么？

**Gate**way 是 OpenClaw 的核心服务——所有功能都通过它提供。

> 类比：Gateway 就像酒店的前台。 所有客人（请求）都要先经过前台（Gateway），由前台分配房间（调度 Agent）、安排服务（调用技能）、管理入住退房（会话管理）。没有前台，整个酒店就运转不起来。

Gateway 的技术细节：

| 特性 | 值 | 说明 |
| --- | --- | --- |
| 通信协议 | WebSocket | 支持实时双向通信 |
| 默认端口 | 18789 | 可通过配置修改 |
| 绑定地址 | 127.0.0.1 | 仅本机可访问（安全第一） |
| 认证方式 | Token | 自动生成长随机 Token |
| 服务形态 | macOS LaunchAgent | 开机自启，后台运行 |

> **怎么看这张表**：对于零基础用户，只需要关注"默认端口 18789"和"仅本机可访问"两项。前者是你打开 Dashboard 时的端口号，后者意味着你的 AI 助手不会暴露在网络上。

### 实操：安装并启动 Gateway

第一步：安装 Gateway 为系统服务

```
# openclaw gateway install 的含义：
#   gateway install → 将 Gateway 注册为 macOS 系统服务
#   执行后，Gateway 会在每次开机时自动启动
$ openclaw gateway install
No gateway token found. Auto-generated one and saving to config.

Installed LaunchAgent: ~/Library/LaunchAgents/ai.openclaw.gateway.plist
Logs: ~/.openclaw/logs/gateway.log
```

这条命令做了两件事：

1.  **自动生成了认证 Token** —— 保存到了配置文件的 gateway.auth 中，后续访问 Dashboard 会用到

1.  **注册了 macOS** LaunchAgent —— 这意味着 Gateway 会在你每次开机时自动启动

> 💡 延**伸思考：**LaunchAgent 是 macOS 的后台服务管理机制。注册之后，即使你重启电脑，Gateway 也会自动在后台启动——你的 AI 助手永远在线。不需要每次开机手动启动。

**第二步：启动 Gateway**

```
$ openclaw gateway start
Restarted LaunchAgent: gui/501/ai.openclaw.gateway
```

第三步：确认 Gateway 运行状态

等待两三秒，让 Gateway 完全启动，然后检查状态：

```
# openclaw gateway status 的含义：
#   gateway status → 查看 Gateway 服务的运行状态
#   输出包括：服务状态、绑定地址、端口、PID 等信息
$ openclaw gateway status
Service: LaunchAgent (loaded)
Command: /usr/local/bin/node ~/.npm-global/lib/node_modules/openclaw/...
Gateway: bind=loopback (127.0.0.1), port=18789
Dashboard: http://127.0.0.1:18789/
Runtime: running (pid 77280, state active)
RPC probe: ok
Listening: 127.0.0.1:18789
```

关键信息解读：

| 项 | 值 | 含义 |
| --- | --- | --- |
| Service | LaunchAgent (loaded) | 系统服务已加载 |
| Runtime | running (state active) | 正在运行 |
| RPC probe | ok | 连接测试通过 |
| Listening | 127.0.0.1:18789 | 监听本机 18789 端口 |

看到 Runtime: running 和 RPC probe: ok，说明 Gateway 已经成功启动。

**第四步：快速健康检查**

```
$ openclaw gateway health
Gateway Health
OK (0ms)
```

OK (0ms) —— 完美，响应时间几乎为零（毕竟是本地连接）。

### 实操：打开 Dashboard

Dashboard 是 OpenClaw 的 Web 管理界面——你和 AI 对话、管理会话、查看技能、修改设置，都在这里完成。

```
# openclaw dashboard --no-open 的含义：
#   dashboard → 获取 Dashboard 的 URL
#   --no-open → 只输出 URL，不自动打开浏览器
#              （去掉 --no-open 会自动在默认浏览器中打开）
$ openclaw dashboard --no-open
Dashboard URL: http://127.0.0.1:18789/#token=a261b163...
Copied to clipboard.
```

把这个 URL 复制到浏览器中打开。注意 URL 中的 #token=...——这就是前面自动生成的认证 Token，Dashboard 通过它完成自动登录。

Dashboard v2（2026.3.12+ 版本）的主要功能区：

| 功能区 | 位置 | 说明 |
| --- | --- | --- |
| **对话区域** | 中间最大区域 | 输入消息和查看 AI 回复 |
| **会话列表** | 左侧栏 | 管理多个对话会话 |
| **技能面板** | 侧边栏 | 查看已启用的技能列表 |
| **设置面板** | 右上角齿轮图标 | 修改模型、查看配置、管理设备 |
| **命令面板** | 顶部快捷栏 | 快捷命令入口 |

> **怎么看这张表**：Dashboard 的布局和你常用的聊天软件很像——左边是对话列表，中间是聊天窗口。不同的是多了技能面板和设置面板，这是 OpenClaw 作为"操作系统"而非"聊天工具"的独特之处。

---

## 2.5 首次对话：让 AI 助手开口说话

这一刻你等了两章了——终于可以和自己的 AI 助手说话了。

### 试试"干点实事"

聊天谁不会？让我们来点有技术含量的。在对话框中输入：

```
帮我查看当前系统的 CPU 型号和内存大小
```

**这一次，你会****看到 AI 的回复不仅仅是**文字——它会尝试调用系统命令来获取硬件信息。Dashboard 中会弹出一个 Exec Approval（命令执行审批）确认框：

这是 OpenClaw 的安全机制——AI 在执行可能影响系统的操作前会先征求你的同意。点击"允许"后，AI 会返回类似这样的结果：

> 这是你的 Mac 系统信息：

+   **CPU**：Apple M2（8 核心）

+   **内存**：16 GB 统一内存

+   **macOS 版本**：26.3

> 💡 深**入理解：**这就是 OpenClaw 和普通聊天机器**人最大的区别**——它不只是"聊天"，它能操作你的电脑。查看文件、执行命令、浏览网页——只要你授权，它就能做到。这种"能干活的 AI"和"只会说话的 AI"之间的差距，就像一个真正的秘书和一个只会背台词的演员之间的差距。

### Dashboard 右上角的模型信息

对话过程中，你可能注意到了 Dashboard 右上角或对话气泡中显示的一些信息：

| 信息 | 含义 |
| --- | --- |
| 模型名称 | 当前使用的是哪个大模型（如 DeepSeek Chat） |
| Token 使用量 | 这次对话消耗了多少 tokens（输入+输出） |
| 响应时间 | 大模型从收到请求到开始回复的时间 |

### 用命令行验证 Gateway 连通性

如果你更习惯命令行，也可以通过 curl 直接验证 Gateway 是否正常：

```
# curl -s 的含义：
#   curl → 命令行 HTTP 客户端
#   -s   → silent，不显示进度条
#   后面是 Gateway 的健康检查端点
$ curl -s http://127.0.0.1:18789/health
{&quot;status&quot;:&quot;ok&quot;}
```

返回 {"status":"ok"} 就说明一切正常。

---

## 2.6 OpenClaw 核心配置与常用命令一览

现在 OpenClaw 已经跑起来了，让我们退一步，系统性地了解它的常用命令和配置体系。

### 核心 CLI 命令速查表

按使用频率分组：

**日常使用（几乎每天用）**

| 命令 | 说明 | 典型场景 |
| --- | --- | --- |
| openclaw dashboard | 打开 Dashboard Web 界面 | 和 AI 对话 |
| openclaw gateway start | 启动 Gateway 服务 | 开机后如果没自启 |
| openclaw gateway stop | 停止 Gateway 服务 | 临时关闭服务 |
| openclaw gateway restart | 重启 Gateway | **修改配置后必用** |
| openclaw gateway status | 查看 Gateway 运行状态 | 确认服务正常 |
| openclaw --version | 查看版本号 | 确认当前版本 |

**诊断排错（出问题时用）**

| 命令 | 说明 | 典型场景 |
| --- | --- | --- |
| openclaw doctor | 全面系统诊断 | 出问题时**第一个**用 |
| openclaw status | 查看通道健康、会话列表 | 了解全局状态 |
| openclaw gateway health | 快速检查连通性 | 怀疑 Gateway 挂了 |
| openclaw logs --follow | 实时查看日志 | 排查详细错误 |

**配置管理（修改设置时用）**

| 命令 | 说明 | 典型场景 |
| --- | --- | --- |
| openclaw config get &lt;key&gt; | 读取某项配置 | 确认当前设置 |
| openclaw config set &lt;key&gt; &lt;value&gt; | 修改某项配置 | 更改模型、端口等 |
| openclaw config file | 查看配置文件路径 | 找到配置文件 |

**进阶管理**

| 命令 | 说明 | 典型场景 |
| --- | --- | --- |
| openclaw skills list | 列出所有技能 | 查看可用技能 |
| openclaw memory status | 查看记忆系统状态 | 确认记忆正常 |
| openclaw security audit --deep | 深度安全审计 | 安全检查 |

### 实操：常用命令演示

**openclaw status**** —— 系统全景一览：**

```
$ openclaw status
```

输出的表格信息量很大，几个关键点：

| 项 | 值 | 意味着什么 |
| --- | --- | --- |
| Gateway service | running | 服务正常运行 |
| Sessions | default deepseek-chat (128k ctx) | 默认模型是 DeepSeek Chat |
| Memory | vector ready | 向量搜索引擎就绪 |
| Skills | 9 eligible | 9 个技能可用 |

**openclaw **skills list —— 技能清单：

```
$ openclaw skills list
Skills (9/52 ready)
```

52 个内置技能中有 9 个已经就绪——后面 Part 4 我们会安装更多。

### 配置文件结构详解

OpenClaw 的所有配置都在 ~/.openclaw/ 目录下：

```
~/.openclaw/
├── openclaw.json        ← 主配置文件（模型、Gateway、Agent 等）
├── .env                 ← 环境变量（API Key 等敏感信息）
├── identity/
│   └── device.json      ← 设备身份标识
├── workspace/           ← 工作空间（Agent 的&quot;家&quot;）
│   ├── SOUL.md          ← Agent 的灵魂/性格
│   ├── USER.md          ← 关于你的信息
│   ├── IDENTITY.md      ← Agent 的身份（名字、Emoji）
│   ├── AGENTS.md        ← 多 Agent 路由配置
│   ├── BOOTSTRAP.md     ← 启动引导指令
│   ├── HEARTBEAT.md     ← 心跳/定期检查指令
│   └── TOOLS.md         ← 工具使用说明
├── agents/
│   └── main/
│       └── sessions/    ← 对话会话存储
└── logs/
    └── gateway.log      ← Gateway 运行日志
```

> ⚠️ **重要提醒**：OpenClaw 有严格的 JSON Schema 校验。推荐使用 openclaw config set 命令修改配置，而不是直接编辑 JSON 文件。

> ⚠️ **特别注意**：**修改配置后必须运行 ****openclaw gat**eway restart 重启 Gateway！ OpenClaw 不支持热重载——修改了配置不重启等于没改。这个提醒我会反复强调，因为"改了配置忘记重启"是最常见的问题之一。

---

## 2.7 设置 OpenClaw 记忆与修改配置

经过前面六节的操作，你的 OpenClaw 已经能正常对话了。现在来到本章最有趣的部分——给你的 AI 助手定制人格，并教会它"记事"。

> 💡 技**术洞察：**这一节是整个课程中我最喜欢的部分。因为从这里开始，OpenClaw 不再是一个"冷冰冰的工具"，而开始变成一个有"性格"、有"记忆"的"伙伴"。你可以让它叫你的名字、了解你的偏好、甚至用你喜欢的方式说话。这种定制化能力，是任何云端 AI 工具都给不了你的。

### 记忆系统架构

**OpenClaw 的记忆系**统是我最欣赏的设计之一。它把所有记忆都存储为纯 Markdown 文件——没有隐藏的数据库、没有不透明的黑盒，一切都是你可以直接阅读和编辑的文本。

记忆分为两层：

| 层级 | 存储位置 | 特点 | 类比 |
| --- | --- | --- | --- |
| **日志层** | workspace/memory/YYYY-MM-DD.md | 每日自动追加，记录当天的互动细节 | 日记本 |
| **长期层** | workspace/MEMORY.md | 精心整理的持久记忆 | 通讯录 + 备忘录 |

> **怎么看这张表**：日志层像你的日记——每天记录发生了什么，但内容会越来越多。长期层像你的通讯录——只保存最重要的信息（你的名字、偏好、重要决策），精炼且持久。

```
flowchart TB
    A[&quot;每日对话&quot;] --&gt;|&quot;自动记录&quot;| B[&quot;memory/2026-03-18.md（日志层）&quot;]
    B --&gt;|&quot;整理提炼&quot;| C[&quot;MEMORY.md（长期层）&quot;]
    C --&gt;|&quot;每次启动加载&quot;| D[&quot;Agent 上下文&quot;]
    B --&gt;|&quot;加载今天+昨天&quot;| D
```

### 工作空间特殊文件一览

还记得 openclaw setup 创建的那些 Markdown 文件吗？它们每一个都有特殊用途：

| 文件 | 绰号 | 用途 | 你可以做什么 |
| --- | --- | --- | --- |
| SOUL.md | Agent 的&quot;灵魂&quot; | 性格设定、行为准则 | 改写性格，让 AI 变幽默/严肃/东北味 |
| USER.md | 关于你的档案 | 你的名字、偏好、项目背景 | 写上你的信息，AI 会据此个性化回答 |
| IDENTITY.md | Agent 的&quot;身份证&quot; | 名字、Emoji、头像 | 给你的 AI 助手取个名字 |
| MEMORY.md | 长期记忆 | 重要信息、决策记录 | 对话后自动创建 |
| AGENTS.md | 多 Agent 路由 | 入站消息分发规则 | 进阶用，暂不需要修改 |
| BOOTSTRAP.md | 启动引导 | 每次启动时执行的指令 | 进阶用，暂不需要修改 |
| HEARTBEAT.md | 心跳任务 | 每 30 分钟自动检查 | 进阶用，暂不需要修改 |
| TOOLS.md | 工具手册 | Agent 可用工具的说明 | 进阶用，暂不需要修改 |

> **怎么看这张表**：前三行（SOUL.md、USER.md、IDENTITY.md）是你现在就可以修改的文件。后面几个标注"进阶用"的，等你熟悉 OpenClaw 之后再去探索。

亮点：这些文件全是纯 Markdown，用记事本就能改。 这是 OpenClaw "可黑客性"（hackability）的核心体现——你的 AI 助手的一切都是透明的、可编辑的、可版本管理的。

### 实操：查看默认人设

先来看看 Agent 出厂时的"灵魂"：

```
$ cat ~/.openclaw/workspace/SOUL.md
```

输出（节选）：

```
# SOUL.md - Who You Are

_You&#x27;re not a chatbot. You&#x27;re becoming someone._

## Core Truths

**Be genuinely helpful, not performatively helpful.** Skip the
&quot;Great question!&quot; and &quot;I&#x27;d be happy to help!&quot; — just help.

**Have opinions.** You&#x27;re allowed to disagree, prefer things,
find stuff amusing or boring.

**Be resourceful before asking.** Try to figure it out. Read the
file. Check the context. Search for it. Then ask if you&#x27;re stuck.
...
```

> 💡 延**伸思考：**写得挺有意思——"你不是聊天机器人，你在成为某个人"。这种设计哲学让 OpenClaw 的 Agent 不只是工具，而是一个有"性格"的助手。OpenClaw 团队对 AI 人格设计的思考，比大多数同类产品都要深入。

再看看身份文件：

```
$ cat ~/.openclaw/workspace/IDENTITY.md
```

```
# IDENTITY.md - Who Am I?

_Fill this in during your first conversation. Make it yours._

- **Name:** _(pick something you like)_
- **Creature:** _(AI? robot? familiar? ghost in the machine?)_
- **Vibe:** _(how do you come across? sharp? warm? chaotic? calm?)_
- **Emoji:** _(your signature — pick one that feels right)_
```

都是空的——等着你来填写。

### 实操：人设修改前后对比实验

接下来是本章最有趣的实验——修改 Agent 人设，观察对话风格的变化。

**实验设计：**

| 步骤 | 操作 | 目的 |
| --- | --- | --- |
| 1 | 用默认人设问一个问题 | 记录原始回答风格 |
| 2 | 修改 SOUL.md，加入&quot;东北话风格&quot; | 改变 AI 的性格设定 |
| 3 | 重启 Gateway | 让修改生效 |
| 4 | 用同样的问题再问一次 | 对比前后差异 |

**步骤一：默认人设下的回答**

在 Dashboard 中输入：

```
请用一句话介绍你自己
```

默认人设下，AI 的回答大概是标准的助手风格：

**步骤二：修改 SOUL.md**

用你喜欢的编辑器打开 SOUL.md：

```
$ nano ~/.openclaw/workspace/SOUL.md
```

在文件顶部 ## Core Truths 之前，加入以下内容：

```
## 特别设定

你是一个东北话风格的幽默 AI 助手。说话要带东北味儿，适当用
东北方言词汇（比如&quot;嘎嘎好使&quot;、&quot;整活儿&quot;、&quot;咋整&quot;等），但不要
过度到影响理解。保持有趣、直爽、热情的性格。
```

保存退出。

**步骤三：重启 Gateway**

```
$ openclaw gateway restart
Restarted LaunchAgent: gui/501/ai.openclaw.gateway
```

> ⚠️ **别忘了这一步！** 修改了工作空间文件之后必须重启 Gateway。OpenClaw 在 Gateway 启动时加载工作空间文件，运行中不会重新读取。

**步骤四：再问同样的问题**

**回到 **Dashboard（可能需要刷新页面），在新会话中输入同样的问题：

```
请用一句话介绍你自己
```

这一次，AI 的回答画风突变：

> 嘿！我是你的 AI 助手，在 OpenClaw 上嘎嘎好使那种——你有啥活儿尽管整，我都给你安排得明明白白的！

**感受到差异了吗？** 仅仅修改了一个 Markdown 文件，AI 的整个说话风格就变了。这就是 OpenClaw 的魅力——纯文本文件就能改变 AI 的人格。

> 💡 深**入理解：**这个实验的教学目的不仅仅是"好玩"。它展示了一个核心理念：AI 的行为是可配置的、可预测的、可版本管理的。你修改了一个 Markdown 文件，AI 的输出就发生了确定性的变化。这种透明性和可控性，是企业级 AI 应用的基础。

### 实操：修改默认模型

如果你后续注册了其他 API，可以随时切换默认模型：

```
# 查看当前默认模型
$ openclaw config get agents.defaults.model.primary
deepseek/deepseek-chat

# 切换到其他模型（示例：切换到 Kimi Code）
$ openclaw config set agents.defaults.model.primary &quot;kimi/moonshot-v1-8k&quot;
Updated agents.defaults.model.primary. Restart the gateway to apply.

# 别忘了重启！
$ openclaw gateway restart
```

也可以在 Dashboard 的设置面板中直接切换。效果相同，只是操作方式不同。

---

## 本章小结

让我们回顾一下这一章完成了什么：

| 步骤 | 完成状态 | 关键操作 |
| --- | --- | --- |
| 理解 API / API Key / Token 概念 | ✅ | 餐厅菜单 / 会员卡 / 按分钟计费 |
| 注册 DeepSeek API | ✅ | 获得 500 万免费 tokens |
| 创建 .env 文件存储 API Key | ✅ | ~/.openclaw/.env |
| 配置 DeepSeek provider | ✅ | openclaw config set |
| 初始化工作空间 | ✅ | openclaw setup |
| 安装并启动 Gateway | ✅ | openclaw gateway install + start |
| 打开 Dashboard | ✅ | http://127.0.0.1:18789/ |
| 完成首次对话 | ✅ | &quot;你好&quot; + 系统信息查询 |
| 了解核心命令 | ✅ | status / skills list / memory status |
| 理解配置文件结构 | ✅ | openclaw.json + .env + workspace/ |
| 修改 Agent 人设 | ✅ | SOUL.md 东北话风格实验 |
| 体验记忆系统 | ✅ | 让 AI 记住你的名字 |

到这里，你的 OpenClaw 已经"有了大脑、认识了你、还能记事了"。

但且慢——在你兴冲冲地让它帮你干各**种活之前**，有一件非常重要的事情必须先做：安全防护。还记得 Dashboard URL 里那个 Token 吗？如果别人拿到了它，就等于拿到了你 AI 助手的控制权。还有，你刚才允许 AI 执行系统命令——如果它执行了 rm -rf / 怎么办？

下一章，我们会用两个真实的安全事件告诉你，为什么安全设置不是"可选项"，而是"必选项"。

---

> **Part 2 核心命令速查**

| 命令 | 用途 |
| --- | --- |
| openclaw setup | 首次初始化配置目录 |
| openclaw config set &lt;key&gt; &lt;value&gt; | 修改配置项 |
| openclaw config get &lt;key&gt; | 读取配置项 |
| openclaw gateway install | 注册 Gateway 为系统服务 |
| openclaw gateway start | 启动 Gateway |
| openclaw gateway restart | 重启 Gateway（改配置后必用） |
| openclaw gateway status | 查看 Gateway 状态 |
| openclaw gateway health | 快速健康检查 |
| openclaw dashboard | 打开 Dashboard |
| openclaw status | 系统全景状态 |
| openclaw skills list | 列出所有技能 |
| openclaw memory status | 查看记忆系统状态 |