
案例 A 让我们见识了 `Agent` 在终端里干活的实力。但终端毕竟是开发者的地盘——普通用户不可能打开命令行跟 `Agent` 对话。案例 B 要解决的问题是：怎么让同一个 `Agent` "活"在手机上？

答案就是上一节讲过的 `Gateway` 架构。现在我们亲手把它跑通——从飞书开放平台创建应用、配置权限、启动 `Gateway`，到最终在手机飞书上跟 `Agent` 对话。整个过程大约 15 分钟，做完之后你的 `Agent` 就真的"住进"手机里了。

## 前置条件

开始之前，确认以下环境已就绪（Lesson 1 应该已经搞定了）：

+   `hermes` v0.8.0 已安装（`hermes --version` 验证）

+   DeepSeek API Key 已配置（Lesson 1 环境变量设置过的 `DEEPSEEK_API_KEY`）

+   飞书账号（个人版即可，不需要企业版）

## 第一步：飞书开放平台 — 创建应用

首先打开[飞书开放平台](https://open.feishu.cn)：

这是飞书开放平台的入口。我们需要在这里创建一个"企业自建应用"，让 `Hermes Agent` 通过它接入飞书。

> 注：如果你没有飞书开发者账号，首次访问时会引导你注册。注册完成后进入"开发者后台"。

### 创建应用

进入开发者后台后，点击"创建企业自建应用"：

1.  应用名称填一个有意义的名字（比如"九天 AI 助手"）

1.  应用描述随便写

1.  点击"确认创建"

创建完成后，你会看到应用的基本信息页面，这里最重要的是两个值：

+   `App ID`：形如 `cli_xxxxxxxxxxxx`

+   `App Secret`：一长串字母数字

把这两个值记下来，后面配置环境变量要用。

### 启用 Bot 能力

刚创建的应用默认只是一个"空壳"，需要手动启用 Bot 能力：

1.  在左侧导航找到"添加应用能力"

1.  选择"机器人"

1.  点击"添加"

### 配置权限

Bot 需要"能读消息"和"能发消息"的权限：

1.  进入"权限管理"页面

1.  搜索并开通以下权限：

+   `im:message`（读取消息）

+   `im:message:send_as_bot`（以机器人身份发消息）

+   `im:resource`（读取消息中的资源文件）

### 配置事件订阅（WebSocket 模式）

这一步决定了飞书怎么把用户消息传给你的 `Agent`。`Hermes Agent` 推荐使用 WebSocket 长连接模式——不需要公网 IP，不需要域名，你的笔记本连着家里 Wi-Fi 就能跑：

![](img/e7304fba6ca5608421b0a0df2a450042.png)

1.  进入"事件订阅"页面

1.  **传输方式**选择"WebSocket"（不是 HTTP Webhook）

1.  添加事件 `im.message.receive_v1`（接收消息事件）

> 注：WebSocket 模式是飞书的一个非常友好的设计——传统的 Webhook 方式需要你有公网可访问的服务器，而 WebSocket 方式由你的程序主动连接飞书的服务器，相当于"我来找你"而不是"你来找我"。这对于本地开发和学习来说简直是福音。

### 发布应用

配置完成后，还需要"创建版本"并"申请发布"。如果是个人开发版，通常会自动审批通过。

## 第二步：配置环境变量

飞书应用配好了，现在回到终端。`hermes gateway` 需要知道飞书应用的身份信息，通过环境变量传入：

```
# 飞书应用凭证（替换为你自己的值）
export FEISHU_APP_ID=cli_xxxxxxxxxxxx
export FEISHU_APP_SECRET=你的AppSecret

# 飞书域名（国内版用 feishu，国际版用 lark）
export FEISHU_DOMAIN=feishu

# 连接模式（推荐 websocket）
export FEISHU_CONNECTION_MODE=websocket

# 确保 DeepSeek API Key 已设置
export DEEPSEEK_API_KEY=sk-你的APIKey
```

一共 5 个环境变量，其中 `FEISHU_APP_ID` 和 `FEISHU_APP_SECRET` 来自刚才创建的飞书应用，其余的按上面填就行。

> **Watch-out**：`FEISHU_DOMAIN` 一定要填对——国内飞书用 `feishu`，国际版 Lark 用 `lark`。填错了 WebSocket 会连接到错误的服务器，导致消息收不到但不报错。

## 第三步：启动 Gateway

万事俱备，一条命令启动：

```
hermes gateway run
```

看到终端输出，重点关注几个信息：

1.  "Hermes Gateway Starting..." — `Gateway` 主进程启动

1.  "Messaging platforms + cron scheduler" — 消息平台适配器和定时任务调度器加载

1.  `[Lark] [INFO] connected to `wss://msg-frontier.feishu.cn/ws/v2/...` — WebSocket 连接成功建立

看到第三行的 `connected to wss://...` 就说明你的 `Agent` 已经成功连接到飞书的消息服务器了。`Gateway` 会保持运行，持续监听飞书发来的消息。

> 注：你可能会看到一条 `Unable to hydrate bot identity` 的警告——这是因为我们没有配置 `admin:app.info:readonly` 权限，`Gateway` 无法自动获取 Bot 的显示名称。不影响功能，Bot 照样能收发消息。

## 常见问题：Gateway 启动成功但收不到消息

这是初学者最容易掉进去的坑。`Gateway` 成功启动了，WebSocket 也连上了，但在飞书给 Bot 发消息，完全没反应。

原因是 `Hermes Gateway` 的默认安全策略——如果没有配置任何用户白名单，所有消息都会被静默拒绝。这是一个安全设计：防止你的 Bot 暴露在公网后被陌生人滥用。

解决方法有两种：

方案 A：开发/学习环境（简单粗暴）

```
export GATEWAY_ALLOW_ALL_USERS=true
hermes gateway run
```

**方案 B：生产环境（推荐）**

```
# 只允许特定用户（飞书用户 ID，可在飞书管理后台查到）
export FEISHU_ALLOWED_USERS=ou_xxxxx,ou_yyyyy
hermes gateway run
```

> **Watch-out**：`GATEWAY_ALLOW_ALL_USERS=true` 只用于学习环境。生产部署时一定要用平台级白名单（`FEISHU_ALLOWED_USERS`），否则任何人都能通过飞书跟你的 `Agent` 对话——而你的 `Agent` 有终端访问权限。

## 第四步：手机飞书 — 首次对话

`Gateway` 跑起来了，现在拿出手机打开飞书。

### 找到你的 Bot

在飞书的搜索框里搜索你刚才创建的应用名称（比如"AI智能助手"），点进去就是跟 Bot 的私聊界面。

### 发送第一条消息

随便说句话试试——比如"你好，你是谁？"。如果一切正常，几秒钟后 `Agent` 就会回复你。这一刻你会意识到：这个在终端里跟你聊天的 `Agent`，现在真的住进手机里了。

回头看终端，你会发现 `Gateway` 打印了一系列日志——这跟案例 A 在终端里看到的 ReAct 循环是一样的。区别在于：案例 A 的输入来自命令行，案例 B 的输入来自飞书消息，但背后是同一个 `Agent`、同一个 ReAct 引擎。

### 设置 Home Channel（重要）

首次跟 Bot 对话时，你很可能会看到这条提示：

> 📬 No home channel is set for Feishu. A home channel is where Hermes delivers cron job results and cross-platform messages. Type `/sethome` to make this chat your home channel, or ignore to skip.

![](img/471e1331dd213e19df21f49ebe420078.png)

这是 Hermes `Agent` 的 Home Channel 机制——它需要知道"把定时任务结果和跨平台消息发到哪个聊天窗口"。直接在飞书对话框里输入：

```
/sethome
```

Bot 会确认当前聊天已设为 Home Channel。设置完成后，后面案例 B 的 `cron` 定时任务推送（Block 3d）和跨平台消息投递都会发到这个聊天窗口。

> **Watch-out**：如果你跳过了 `/sethome`，后面的 `hermes cron` 定时任务虽然能创建成功，但结果只会输出到终端（`Deliver: local`），无法推送到飞书。所以这一步别跳过。

### 查看 Gateway 日志

每当飞书有新消息进来，终端会显示类似这样的日志：

你可以清晰地看到：消息从飞书进来 → `Gateway` 路由到 `Agent` → `Agent` 执行 ReAct 循环 → 结果通过 `Gateway` 发回飞书。整个链路就是 Block 3a 讲过的 `Gateway` 架构在真实运行。

![](img/ba1c0a579fd859241eebe5d37f0cf0ae.png)

> Watch-out：`Gateway` 进程必须保持运行才能接收飞书消息。如果你关掉终端或 `Ctrl+C` 停止了 `Gateway`，飞书上的 Bot 就"掉线"了。生产部署时通常会用 `nohup`、`tmux` 或 `systemd` 等方式让 `Gateway` 在后台持续运行。

## 3.3 SOUL.md 人格配置：给 Agent 一个性格

飞书 Bot 跑通了，但你有没有发现——`Agent` 的回答虽然准确，却像一个没有感情的工具。问它"你是谁"，它回答"我是 `Hermes Agent`，一个由 Nous Research 创建的智能 AI 助手，我可以帮你完成各种任务......"。正确，但无聊。

如果你希望 `Agent` 像一个有个性的助手——比如说话幽默、善用类比、有自己的名字——只需要编辑一个文件：`~/.hermes/SOUL.md`。

### 第一步：看看默认状态

在配置人格之前，我们先看看 `Agent` 的"原始面貌"。如果 `~/.hermes/SOUL.md` 已经存在，先临时移走它，然后问 `Agent` 一个简单的问题：

```
mv ~/.hermes/SOUL.md ~/.hermes/SOUL.md.bak   # 临时移走
hermes chat -q &quot;你好，请用一句话介绍一下你自己&quot; --toolsets &#x27;&#x27;
```

![](img/f3337df846d9dcf48afbcc3242388b32.png)

标准的工具人回答——"我是 `Hermes Agent`，一个由 Nous Research 创建的智能 AI 助手，我可以帮你完成各种任务......"。准确但没有温度。这是 `Agent` 内置的默认身份描述，没有任何个性化定制。

### 第二步：编写 SOUL.md

`SOUL.md` 是 Hermes `Agent` 的人格定义文件，位于 `~/.hermes/SOUL.md`。它的内容会被注入到系统提示词的最前面，直接决定 `Agent` 的说话风格和行为方式。

![](img/6a2333dadbc606384b0c61bb1e681130.png)

把备份恢复回来（或者用编辑器新建一个），写入你想要的人格定义：

```
mv ~/.hermes/SOUL.md.bak ~/.hermes/SOUL.md   # 恢复备份
cat ~/.hermes/SOUL.md                         # 查看内容
```

![](img/face0bbdb0adac79f7018667752ea38b.png)

这个 `SOUL.md` 定义了"九天 AI 助手"的人格，包含三个维度：

+   **身份**：名字叫"九天"，定位是热爱技术的编程助手

+   **性格**：像技术前辈一样说话、善用类比、每次给实用小提示

+   **原则**：准确、简单、尊重用户

你完全可以按自己的喜好来写——想让 `Agent` 说话像严谨的学者、像耐心的老师、甚至有自己的口头禅，都行。`SOUL.md` 就是 `Agent` 的"人设文档"，`Markdown` 格式，想怎么写都可以。

### 第三步：验证人格生效

`SOUL.md` 在**新会话启动时**加载。保存文件后，开一个新的 `chat` 会话，同样的问题再问一次：

```
hermes chat -q &quot;你好，请用一句话介绍一下你自己&quot; --toolsets &#x27;&#x27;
```

![](img/e9272434953ff4a709ecacbeb5767adb.png)

对比一下两次回答：

|  | 配置前（默认人格） | 配置后（自定义人格） |

| --- | --- | --- |

| **自我介绍** | &quot;我是 `Hermes Agent`，一个由 Nous Research 创建的智能 AI 助手&quot; | &quot;我是九天，一个热爱技术的 AI 编程助手，喜欢用通俗易懂的方式帮你解决问题&quot; |

| **说话风格** | 标准客服体 | 有温度，像个真人 |

| **名字** | 无 | &quot;九天&quot; |

同一个 `Agent`、同一个模型、同一个 ReAct 引擎——仅仅改了一个 `Markdown` 文件，对话体验就完全不同了。这就是 `SOUL.md` 的威力：零代码定制 `Agent` 人格。

> **Watch-out**：`SOUL.md` 的加载时机是**会话启动时**。如果你在 `hermes chat` 的交互模式中修改了 `SOUL.md`，当前会话不会生效——需要退出后重新开一个新会话。同理，`Gateway` 模式下修改 `SOUL.md` 后，新的飞书对话会自动使用新人格，已有的对话线程不受影响。

> 实用建议：`SOUL.md` 不仅能定义"性格"，还能定义 `Agent` 的专业领域。比如在 `SOUL.md` 里写"你是一个专注于 `Python` 数据分析的助手"，`Agent` 就会在回答中优先使用 `pandas`、`matplotlib` 等工具——这比每次在对话中重复说明要高效得多。

## Hermes Agent 的完整配置体系

![](img/9098143cc5464ffc4468b4f0180a049e.png)

如果你熟悉 `OpenClaw`（`Claude Code` / `open-interpreter`），可能会好奇——Hermes 只有一个 `SOUL.md` 就够了吗？`OpenClaw` 可是有 `CLAUDE.md`、`.cursorrules`、`tools` 配置等一整套体系。

事实上 Hermes `Agent` 的配置体系同样丰富，只是组织方式不同。`~/.hermes/` 目录下的完整配置文件结构：

| 文件/目录 | 用途 | 谁维护 |

| --- | --- | --- |

| `config.yaml` | 主配置文件：模型选择、Provider 设置、记忆开关、工具集控制、`Gateway` 参数 | 用户手动 / `hermes config set` 命令 |

| `SOUL.md` | `Agent` 人格定义（身份、性格、专业领域、行为准则） | 用户手动编写 |

| `.env` | 环境变量（API Key、平台凭证等） | 用户手动配置 |

| `memories/MEMORY.md` | `Agent` 的工作笔记（环境、项目、经验） | `Agent` 自动维护 |

| `memories/USER.md` | 用户画像（偏好、风格、技术水平） | `Agent` 自动维护 |

| `skills/` | 技能文件目录（内置 77 + 自动生成） | 系统内置 + `Agent` 自动生成 |

| `plugins/` | 插件目录（自定义工具和钩子） | 用户手动安装 |

| `state.db` | 会话历史数据库（`SQLite`） | 系统自动维护 |

| `logs/` | 结构化日志（`agent.log` + `errors.log`，`v0.8.0` 新增） | 系统自动维护 |

和 `OpenClaw` 对比，Hermes 的差异在于：`SOUL.md` 对应 `OpenClaw` 的 `CLAUDE.md`（系统级指令），`config.yaml` 对应 `OpenClaw` 的 `settings.json`（全局配置），而 `memories/` 和 `skills/` 是 Hermes 独有的——`OpenClaw` 没有内建的持久记忆和技能自动生成机制。

不过本节的重点是 `SOUL.md`，其他配置文件会在后续案例中逐步用到（`config.yaml` 在案例 C 配置记忆 Provider 时会详细讲解）。

## hermes cron：让 Agent 定时帮你做事

飞书 Bot 能对话了，`SOUL.md` 也定义了人格。但目前 `Agent` 只能被动等你说话——你不发消息，它就安静地待着。有没有办法让 `Agent` 主动干活，比如每半小时检查一下服务器状态、每天早上汇总一次项目进展？

`hermes cron` 就是干这个的——给 `Agent` 设一个"闹钟"，到时间了自动执行你定义的任务。

### 创建定时任务

创建一个定时任务只需一条命令。我们让 `Agent` 每 30 分钟巡检一次系统负载，重复 3 次后自动停止：

```
hermes cron create &quot;every 30m&quot; &quot;查看系统负载，如果 CPU 使用率超过 80% 请告警&quot; \
    --name &quot;系统巡检&quot; --repeat 3
```

命令返回了任务的关键信息：
