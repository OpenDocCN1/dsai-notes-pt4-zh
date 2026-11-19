
# OpenClaw 专题技术库

> 来源：[https://gotoaiworld.feishu.cn/docx/LFVUdT5tAosbfrxEoY0crBpinPu](https://gotoaiworld.feishu.cn/docx/LFVUdT5tAosbfrxEoY0crBpinPu)

`https://github.com/openclaw/openclaw`

![](img/ed8ee3b695bc18596e260fe44749b337.png)

## OpenClaw — Personal AI Assistant

![](img/868525053284a7b17f9f2780bd716327.png)

`https://github.com/openclaw/openclaw`

OpenClaw 是一个开源的个人智能助手项目，旨在让用户在自己的设备上运行私有 AI。它具有极高的多平台兼容性，能够无缝接入 WhatsApp、Telegram、Slack 和 Discord 等主流社交软件。该系统采用本地优先的架构，通过一个集中的控制平面管理各种会话、工具和自动化任务。用户不仅可以通过命令行界面进行深度定制，还能利用其专有的 macOS、iOS 和 Android 客户端实现语音唤醒和视觉交互。此外，项目内置了完善的安全沙箱机制，确保在处理远程指令时保护宿主系统的隐私与安全。

Your own personal AI assistant. Any OS. Any Platform. The lobster way. 🦞

`https://github.com/openclaw/openclaw`

[`openclaw.ai`](https://openclaw.ai/)

EXFOLIATE! EXFOLIATE!

OpenClaw is a personal AI assistant you run on your own devices. It answers you on the channels you already use (WhatsApp, Telegram, Slack, Discord, Google Chat, Signal, iMessage, Microsoft Teams, WebChat), plus extension channels like BlueBubbles, Matrix, Zalo, and Zalo Personal. It can speak and listen on macOS/iOS/Android, and can render a live Canvas you control. The Gateway is just the control plane — the product is the assistant.

If you want a personal, single-user assistant that feels local, fast, and always-on, this is it.

[Website](https://openclaw.ai/) · [Docs](https://docs.openclaw.ai/) · [Vision](https://github.com/openclaw/openclaw/blob/main/VISION.md) · [DeepWiki](https://deepwiki.com/openclaw/openclaw) · [Getting Started](https://docs.openclaw.ai/start/getting-started) · [Updating](https://docs.openclaw.ai/install/updating) · [Showcase](https://docs.openclaw.ai/start/showcase) · FAQ · Wizard · Nix · Docker · Discord

Preferred setup: run the [onboarding wizard (`openclaw onboard`)](https://docs.openclaw.ai/start/getting-started) in your terminal. The wizard guides you step by step through setting up the gateway, workspace, channels, and skills. The CLI wizard is the recommended path and works on macOS, Linux, and Windows (via WSL2; strongly recommended). Works with `npm`, `pnpm`, or `bun`. New install? Start here: Getting started

[Website](https://openclaw.ai/) · [Docs](https://docs.openclaw.ai/) · [Vision](https://github.com/openclaw/openclaw/blob/main/VISION.md) · [DeepWiki](https://deepwiki.com/openclaw/openclaw) · [Getting Started](https://docs.openclaw.ai/start/getting-started) · [Updating](https://docs.openclaw.ai/install/updating) · [Showcase](https://docs.openclaw.ai/start/showcase) · [FAQ](https://docs.openclaw.ai/help/faq) · Wizard · Nix · Docker · Discord

![](img/ea03c412e9da5492ad2489273fbdb9d9.png)

`https://docs.openclaw.ai/`

### Docs

Use these when you’re past the onboarding flow and want the deeper reference.

+ [Start](https://docs.openclaw.ai/) with the docs index for navigation and “what’s where.”
+ [Read](https://docs.openclaw.ai/concepts/architecture) the architecture overview for the gateway + protocol model.
+ [Use the full configuration reference](https://docs.openclaw.ai/gateway/configuration) when you need every key and example.
+ [Run](https://docs.openclaw.ai/gateway) the Gateway by the book with the operational runbook.
+ [Learn how the Control UI/Web surfaces work](https://docs.openclaw.ai/web) and how to expose them safely.
+ [Understand](https://docs.openclaw.ai/gateway/remote) remote access over SSH tunnels or tailnets.
+ [Follow](https://docs.openclaw.ai/start/wizard) the onboarding wizard flow for a guided setup.
+ [Wire external triggers via the webhook surface](https://docs.openclaw.ai/automation/webhook).
+ Set up Gmail Pub/Sub triggers.
+ [Learn the macOS menu bar companion details](https://docs.openclaw.ai/platforms/mac/menu-bar).
+ [Platform guide: Windows (WSL2)](https://docs.openclaw.ai/platforms/windows), [Linux](https://docs.openclaw.ai/platforms/linux), [macOS](https://docs.openclaw.ai/platforms/macos), [iOS](https://docs.openclaw.ai/platforms/ios), [Android](https://docs.openclaw.ai/platforms/android)
+ [Debug](https://docs.openclaw.ai/channels/troubleshooting) common failures with the troubleshooting guide.
+ [Review security guidance before exposing anything](https://docs.openclaw.ai/gateway/security).

### Advanced docs (discovery + control)

+ Discovery + transports
+ [Bonjour/mDNS](https://docs.openclaw.ai/gateway/bonjour)
+ [Gateway pairing](https://docs.openclaw.ai/gateway/pairing)
+ Remote gateway `README`
+ [Control UI](https://docs.openclaw.ai/web/control-ui)
+ [Dashboard](https://docs.openclaw.ai/web/dashboard)

### Operations & troubleshooting

+ [Health checks](https://docs.openclaw.ai/gateway/health)
+ [Gateway lock](https://docs.openclaw.ai/gateway/gateway-lock)
+ Background process
+ Browser troubleshooting (Linux)
+ [Logging](https://docs.openclaw.ai/logging)

### Deep dives

+ [`TypeBox` schemas](https://docs.openclaw.ai/concepts/typebox)
+ [Agent loop](https://docs.openclaw.ai/concepts/agent-loop)
+ [Presence](https://docs.openclaw.ai/concepts/presence)
+ [RPC adapters](https://docs.openclaw.ai/reference/rpc)
+ [Queue](https://docs.openclaw.ai/concepts/queue)

### Workspace & skills

+ [Skills config](https://docs.openclaw.ai/tools/skills-config)
+ [Default `AGENTS`](https://docs.openclaw.ai/reference/AGENTS.default)
+ Templates: `AGENTS`
+ Templates: `BOOTSTRAP`
+ Templates: `IDENTITY`
+ [Templates: `SOUL`](https://docs.openclaw.ai/reference/templates/SOUL)
+ Templates: `TOOLS`
+ [Templates: `USER`](https://docs.openclaw.ai/reference/templates/USER)

### Platform internals

+ [macOS dev setup](https://docs.openclaw.ai/platforms/mac/dev-setup)
+ [macOS menu bar](https://docs.openclaw.ai/platforms/mac/menu-bar)
+ macOS voice wake
+ [iOS node](https://docs.openclaw.ai/platforms/ios)
+ [Android node](https://docs.openclaw.ai/platforms/android)
+ [Windows (WSL2)](https://docs.openclaw.ai/platforms/windows)
+ [Linux app](https://docs.openclaw.ai/platforms/linux)

### Email hooks (Gmail)

`docs.openclaw.ai/gmail-pubsub`

### Molty

OpenClaw was built for Molty, a space lobster AI assistant. 🦞 by Peter Steinberger and the community.

+ [`openclaw.ai`](https://openclaw.ai/)
+ [`soul.md`](https://soul.md/)
+ [`steipete.me`](https://steipete.me/)
+ [@openclaw](https://x.com/openclaw)
