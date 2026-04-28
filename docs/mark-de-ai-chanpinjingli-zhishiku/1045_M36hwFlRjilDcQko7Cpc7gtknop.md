# Hermes Agent 官方入门指南

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/XKOTdDPCMoemoxxpqNkcSW5nnoZ](https://qqs7y1hozd1.feishu.cn/docx/XKOTdDPCMoemoxxpqNkcSW5nnoZ)

![](img/08f4d85a484d42a1f57e9712c09eabb1.png)

# Quickstart 快速入门

This guide walks you through installing Hermes Agent, setting up a provider, and having your first conversation. By the end, you'll know the key features and how to explore further.

这份指南将带你完成**Hermes Agent**的安装、服务提供商配置，并开启你的首次对话。读完后，你将了解其核心功能以及后续探索方法。

## Install Hermes Agent

Run the one-line installer:

运行一键安装命令：

```
# Linux / macOS / WSL2 / Android (Termux)
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

> Android / Termux

> If you're installing on a phone[, see the de](https://hermes-agent.nousresearch.com/docs/getting-started/termux)dicated Termux guide for the tested manual path, supported extras, and current Android-specific limitations.

> 如果你是在手机上进行安装，请参阅专门的Termux指南，其中包含经过测试的手动安装方法、支持的附加功能以及当前针对Android系统的特定限制。

> Windows Users（Windows 用户）

> Install [WSL2](https://learn.microsoft.com/en-us/windows/wsl/install) first, then run the command above inside your WSL2 terminal.

> 请先安装 WSL2，然后在 WSL2 终端内运行上述命令。

After it finishes, reload your shell:

安装完成后，重新加载你的命令行环境：

```
source ~/.bashrc   # or source ~/.zshrc
```

## Set Up a Provider 配置服务提供商

The installer configures your LLM provider automatically. To change it later, use one of these commands:

安装程序会**自动配置**你的大语言模型（LLM）服务提供商。如需后续更改，可使用以下任一命令：

```
hermes model       # Choose your LLM provider and model
hermes tools       # Configure which tools are enabled
hermes setup       # Or configure everything at once
```

hermes model walks you through selecting an inference provider:

hermes model 引导你选择推理服务提供商：

> **Minimum context: 64K tokens（**最小上下文窗口: 64K tokens）

> Minimum context: 64K tokens

> Hermes Agent requires a **model with at** least 64,000 tokens of context. Models with smaller windows cannot maintain enough working memory for multi-step tool-calling workflows and will be rejected at startup. Most hosted models (Claude, GPT, Gemini, Qwen, DeepSeek) meet this easily. If you're running a local model, set its context size to at least 64K (e.g. for llama.cpp or for Ollama).--ctx-size 65536-c 65536

> **H**ermes Agent要求使用上下文窗口至少为64K tokens的模型。上下文窗口更小的模型无法为多步骤工具调用工作流保留足够的运行内存，启动时会被系统拒绝。大多数云端托管模型（Claude、GPT、Gemini、通义千问、深度求索）均可轻松满足该要求。如果你运行本地模型，请将其上下文大小设置为至少 64K（例如在 llama.cpp 或 Ollama 中，使用参数：--ctx-size 65536 / -c 65536）

> **Tip（提示）**

> 你可以随时切换服务提供商，**无需修改代码，也不会被绑定锁定**。配置自定义接口时，Hermes 会提示你输入上下文窗口大小，并在可行时自动检测。详情请参阅「上下文长度检测」。

## Start Chatting 始对话

That's it! You'll see a welcome banner with your model, available tools, and skills. Type a message and press Enter.

搞定！你会看到一个欢迎界面，显示当前使用的模型、可用工具和技能。输入消息并按回车即可开始。

The agent has access to tools for web search, file operations, terminal commands, and more — all out of the box.

可直接使用**网页搜索、文件操作、终端命令**等各类工具，开箱即用。

## Try Key Features 试用核心功能

### Ask it to use the terminal 让它调用终端

```
❯ What&#x27;s my disk usage? Show the top 5 largest directories.
```

The agent will run terminal commands on your behalf and show you the results.

该代理将代表你执行终端命令，并向你展示执行结果。

### Use slash commands 使用斜杠命令

Type to see an autocomplete dropdown of all commands/

使用斜杠命令/

### Multi-line input 多行输入

Press or to add a new line. Great for pasting code or writing detailed prompts.Alt+EnterCtrl+J

按“or”键可添加新行，非常适合粘贴代码或编写详细提示词。Alt+EnterCtrl+J

### Interrupt the agent 中断代理

If the agent is taking too long, just type a new message and press Enter — it interrupts the current task and switches to your new instructions. also works.Ctrl+C

如果Agent运行耗时过长，只需输入新消息并按下回车键——这将中断当前任务，并切换执行你的新指令。该操作同样有效。Ctrl+C

### Resume a session 恢复会话

When you exit, hermes prints a resume command:

当你退出时，hermes会打印一条恢复命令：

```
hermes --continue    # Resume the most recent session
hermes -c            # Short form
```

## Explore Further 深入探索

Here are some things to try next:

接下来你可以尝试做这些事：

### Set up a sandboxed terminal 搭建一个沙箱终端

For safety, run the agent in a Docker container or on a remote server:

为确保安全，请在Docker容器或远程服务器上运行该代理：

```
hermes config set terminal.backend docker    # Docker isolation
hermes config set terminal.backend ssh       # Remote server
```

### Connect messaging platforms 连接消息平台

Chat with Hermes from your phone or other surfaces via Telegram, Discord, Slack, WhatsApp, Signal, Email, or Home Assistant:

通过 Telegram、Discord、Slack、WhatsApp、Signal、电子邮件或家庭助手，在手机或其他设备上与 Hermes 聊天：

```
hermes gateway setup    # Interactive platform configuration
```

### Add voice mode 添加语音模式

Want microphone input in the CLI or spoken replies in messaging?

想要在命令行中使用麦克风输入，或在消息应用中获得语音回复？

```
pip install &quot;hermes-agent[voice]&quot;
# Includes faster-whisper for free local speech-to-text
```

Then start Hermes and enable it inside the CLI:

然后启动 Hermes 并在命令行内启用该功能：

```
/voice on
```

P[ress to re](https://hermes-agent.nousresearch.com/docs/user-guide/features/voice-mode)cord, or use to have Hermes speak its replies. See Voice Mode for the full setup across CLI, Telegram, Discord, and Discord voice channels.Ctrl+B/voice tts

按下（对应按键）即可录音，或让 Hermes 语音回复。有关在命令行、Telegram、Discord 及 Discord 语音频道中的完整设置，请参阅语音模式说明。

### Schedule automated tasks 安排自动化任务

```
❯ Every morning at 9am, check Hacker News for AI news and send me a summary on Telegram.
```

The agent will set up a cron job that runs automatically via the gateway.

代理会创建一个 cron 任务，通过网关自动运行。

### Browse and install skills 浏览并安装技能

```
hermes skills search kubernetes
hermes skills search react --source skills-sh
hermes skills search https://mintlify.com/docs --source well-known
hermes skills install openai/skills/k8s
hermes skills install official/security/1password
hermes skills install skills-sh/vercel-labs/json-render/json-render-react --force
```

Tips:

+   Use to search the public directory.--source skills-shskills.sh

+   使用该功能搜索公共目录。

+   Use with a docs/site URL to discover skills from .--source well-known/.well-known/skills/index.json

+   搭配文档/网站链接使用，即可从中发现并获取相关技能。

+   仅在审核过第三方技能后再使用。它可以覆盖非危险的策略模块，但无法更改扫描判定结果。--forcedangerous

Or use the slash command inside chat./skills

或者在聊天中使用斜杠命令。

### Use Hermes inside an editor via ACP

通过 ACP 在编辑器内使用 Hermes

Hermes can also run as an ACP server for ACP-compatible editors like VS Code, Zed, and JetBrains:

Hermes 还可作为 ACP 服务器运行，适配 VS Code、Zed、JetBrains 等支持 ACP 的编辑器。

```
pip install -e &#x27;.[acp]&#x27;
hermes acp
```

See ACP Editor Integration for setup details.

有关安装详情，请参阅 **ACP 编辑器集成** 文档。

### Try MCP servers（尝试使用 MCP 服务器）

Connect to external tools via the Model Context Protocol（通过模型上下文协议连接外部工具）:

```
# Add to ~/.hermes/config.yaml
mcp_servers:
  github:
    command: npx
    args: [&quot;-y&quot;, &quot;@modelcontextprotocol/server-github&quot;]
    env:
      GITHUB_PERSONAL_ACCESS_TOKEN: &quot;ghp_xxx&quot;
```

---

## Quick Reference（快速参考）

## Next Steps

+   [**CLI Guide**](https://hermes-agent.nousresearch.com/docs/user-guide/cli) — Master the terminal interface

+   [**Configuration**](https://hermes-agent.nousresearch.com/docs/user-guide/configuration) — Customize your setup

+   Messaging Gateway — Connect Telegram, Discord, Slack, WhatsApp, Signal, Email, or Home Assistant

+   Tools & Toolsets — Explore available capabilities

**后续步骤**

+   命令行指南 — 熟练掌握终端界面操作

+   配置项 — 自定义你的运行环境

+   消息网关 — 连接 Telegram、Discord、Slack、WhatsApp、Signal、电子邮件或家庭助手

+   工具与工具集 — 探索可使用的各项功能