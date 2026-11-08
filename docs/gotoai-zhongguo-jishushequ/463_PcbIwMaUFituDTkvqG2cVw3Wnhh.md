# OpenClaw 系统架构分析

> 来源：[https://gotoaiworld.feishu.cn/docx/VUWwdQ6FRoHGZExDWkccOtD3nNd](https://gotoaiworld.feishu.cn/docx/VUWwdQ6FRoHGZExDWkccOtD3nNd)

## 架构概述

[OpenClaw 采用插](https://zhida.zhihu.com/search?content_id=269849406&content_type=Article&match_order=1&q=Gateway+%E6%8E%A7%E5%88%B6%E5%B9%B3%E9%9D%A2&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIyODIwNTAsInEiOiJHYXRld2F5IOaOp-WItuW5s-mdoiIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI2OTg0OTQwNiwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.voQin_NN0C3oJCWBOFQrAZEwGA2TsVIMrOkFosEvd70&zhida_source=entity)件化的 G[ateway ](https://zhida.zhihu.com/search?content_id=269849406&content_type=Article&match_order=1&q=%E5%A4%9A%E6%B8%A0%E9%81%93%E6%B6%88%E6%81%AF%E7%B3%BB%E7%BB%9F&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIyODIwNTAsInEiOiLlpJrmuKDpgZPmtojmga_ns7vnu58iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjk4NDk0MDYsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.e9wf_0hTh0f_DLee2KKHUARqoXkaFkLBZbF4rdUS3Z8&zhida_source=entity)控制平面架构，结合多渠道消息系统和跨平台客户端应用，构建了一个完整的个人 AI 助手生态系统。

### 核心架构特征

![](img/fd89e4ba2be1aec56e3c07f0c4a6a5eb.png)

1.  Gateway 控制平面: 单一 WebSocket 服务器管理所有会话、渠道和事件

1.  多渠道消息系统: 统一抽象层支持 15+ 消息平台

1.  插件化扩展: [Monorepo](https://zhida.zhihu.com/search?content_id=269849406&content_type=Article&match_order=1&q=Monorepo&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIyODIwNTAsInEiOiJNb25vcmVwbyIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI2OTg0OTQwNiwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.MC-n9rLGPXDHmQFzgUIacQtuz-QrkWvFL6GvEjvDLOU&zhida_source=entity) 架构下的独立插件包

1.  跨平台客户端: CLI + macOS App + iOS/Android 节点

1.  [AI 代理引擎](https://zhida.zhihu.com/search?content_id=269849406&content_type=Article&match_order=1&q=AI+%E4%BB%A3%E7%90%86%E5%BC%95%E6%93%8E&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIyODIwNTAsInEiOiJBSSDku6PnkIblvJXmk44iLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjk4NDk0MDYsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.HJCzXYv2stMtNBsyyJRAEh5hfLWIkLtPcA8akj_55j4&zhida_source=entity): 基于 Pi Agent 的 RPC 模式代理

1.  本地优先设计: 数据和会话本地存储，隐私可控

---

## 整体架构模式

### 2.1 架构分类

OpenClaw 结合了多种架构模式：

### 2.2 架构层次

![](img/90e1050c68a57d5fd0c691e3181abe30.png)

---

## 核心组件

### 3.1 Gateway 控制平面

![](img/08870edf2cf852f5932d8a1feca37051.png)

职责: 整个系统的核心控制器

功能:

+   WebSocket 服务器 (ws://localhost:18789)

+   [JSON-RPC 2.0](https://zhida.zhihu.com/search?content_id=269849406&content_type=Article&match_order=1&q=JSON-RPC+2.0&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIyODIwNTAsInEiOiJKU09OLVJ🐧yAyLjAiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNjk4NDk0MDYsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.j4IySdL49j-DuLJJoSPhv9jsdZu5uxcDcBLOWJZgy8k&zhida_source=entity) 方法调度 (70+ 方法，8 个命名空间)

+   多层认证系统 (off/local-only/token/tailscale/tailscale-local)

+   作用域授权 (admin/read/write/approvals/pairing)

+   会话生命周期管理

+   多代理路由和隔离

+   实时事件广播和去重

+   Canvas 宿主服务

+   [Cron 定时任务调度](https://zhida.zhihu.com/search?content_id=269849406&content_type=Article&match_order=1&q=Cron+%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1%E8%B0%83%E5%BA%A6&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIyODIwNTAsInEiOiJDcm9uIOWumuaXtuS7u-WKoeiwg-W6piIsInpoaWRhX3NvdXJjZSI6ImVudGl0eSIsImNvbnRlbnRfaWQiOjI2OTg0OTQwNiwiY29udGVudF90eXBlIjoiQXJ0aWNsZSIsIm1hdGNoX29yZGVyIjoxLCJ6ZF90b2tlbiI6bnVsbH0.pEi03VmI3tv07pDBhOFWTxpxPnZXxo3PFcRZxdqbyYg&zhida_source=entity)

+   配置热重载

技术实现:

+   语言: TypeScript

+   框架: [Fastify](https://zhida.zhihu.com/search?content_id=269849406&content_type=Article&match_order=1&q=Fastify&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIyODIwNTAsInEiOiJGYXN0aWZ5IiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjY5ODQ5NDA2LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.I6ERBJBThw59_dRMZfwbE9PjYXObH9H7N4GVgQ9rmw4&zhida_source=entity) + ws

+   协议: JSON-RPC 2.0

+   位置: src/gateway/

关键文件:

+   src/gateway/server.impl.ts - Gateway 主入口和 WebSocket 服务器

+   src/gateway/server-methods.ts - 方法调度和授权系统

+   src/gateway/server-chat.ts - Chat 会话管理

+   src/gateway/server-broadcast.ts - 事件广播系统

+   src/gateway/auth.ts - 多层认证系统

+   src/gateway/server-methods/ - 32+ 方法处理器模块

### 3.2 渠道系统

职责: 连接各个消息平台，提供统一消息接口

核心渠道 (src/ 目录):

+   src/channels/ - 渠道通用逻辑

+   内置频道 (9个): telegram, discord, slack, signal, whatsapp, imessage, web, line, acp

扩展渠道 (extensions/ 目录):

+   extensions/telegram/ - Telegram 扩展

+   extensions/discord/ - Discord 扩展

+   extensions/slack/ - Slack 扩展

+   extensions/whatsapp/ - WhatsApp 扩展

+   extensions/msteams/ - Microsoft Teams

+   extensions/matrix/ - Matrix

+   extensions/googlechat/ - Google Chat

+   extensions/bluebubbles/ - BlueBubbles

+   extensions/zalo/ - Zalo

+   extensions/zalouser/ - Zalo Personal

+   extensions/line/ - LINE

+   extensions/tlon/ - Tlon

+   extensions/twitch/ - Twitch

+   extensions/nostr/ - Nostr

+   extensions/nextcloud-talk/ - Nextcloud Talk

+   extensions/mattermost/ - Mattermost

统一接口:

```
interface Channel {
  start(): Promise&lt;void&gt;
  stop(): Promise&lt;void&gt;
  sendMessage(target: string, content: string): Promise&lt;void&gt;
  onMessage(handler: MessageHandler): void
  getStatus(): ChannelStatus
}
```

### 3.3 消息路由系统

![](img/10a550f35d253263331ef6987f3320da.png)

职责: 将收到的消息路由到正确的代理和会话

功能:

+   渠道识别 (WhatsApp/Telegram/etc.)

+   对等方识别 (用户 ID/群组 ID)

+   代理路由 (多代理隔离)

+   6 级路由优先级: peer > guild > team > account > channel > default

+   7 级配置匹配: 精确 > 空间 > 账户通配符 > 账户 > 频道通配符 > 频道 > 全局

+   会话激活 (peer/guild/team/account/channel/global)

+   多层访问控制:

+   Allowlist 白名单过滤

+   Mention Gating (群组提及门控)

+   Command Gating (控制命令授权)

+   群组逻辑 (提及门控/回复标签)

技术实现:

+   位置: src/routing/

+   关键文件:

+   src/routing/resolve-route.ts - 主路由器 (resolveAgentRoute)

+   src/channels/channel-config.ts - 7级配置匹配

+   src/channels/command-gating.ts - 控制命令授权

+   src/channels/mention-gating.ts - 群组消息过滤

### 3.4 安全与配对系统

职责: 保护系统免受未授权访问

功能:

+   DM 配对模式 (默认拒绝)

+   配对码生成和验证

+   白名单管理

+   访问控制列表 (ACL)

+   配置安全检查 (Doctor 工具)

技术实现:

+   位置: src/pairing/, src/security/

+   存储: ~/.clawdbot/pairing/ (JSON 文件)

配对流程:

```
1. 未知用户发送消息
2. 系统生成短配对码 (例: ABC123)
3. 回复包含配对码
4. 管理员运行: openclaw pairing approve &lt;channel&gt; ABC123
5. 用户添加到白名单
6. 后续消息正常处理
```

### 3.5 AI 代理引擎

![](img/ad88aed8a576dcf178e91af01a8e745e.png)

职责: 处理用户消息，生成 AI 响应

功能:

+   多模型支持 (Anthropic/OpenAI/Bedrock/Ollama)

+   嵌入式运行模式 (runEmbeddedPiAgent)

+   双车道排队系统:

+   sessionLane: 单会话串行执行

+   globalLane: 全局并发控制

+   Auth Profile 轮转: 30 分钟冷却，自动切换认证配置

+   Thinking Level 降级: high → low → off 故障转移

+   上下文溢出处理: 自动压缩和修剪

+   工具调用 (browser/canvas/cron/etc.)

+   块流式输出

+   会话上下文管理

+   多重尝试和错误恢复

技术实现:

+   核心: @mariozechner/pi-agent-core

+   位置: src/agents/pi-embedded-runner/

+   关键文件:

+   run.ts - 核心编排逻辑 (runEmbeddedPiAgent)

+   run/attempt.ts - 单次执行尝试

+   pi-embedded-subscribe.ts - 流式事件订阅

+   pi-tools.ts - 工具创建 (createOpenClawCodingTools)

支持的工具:

+   Browser (playwright)

+   Canvas ([A2UI](https://zhida.zhihu.com/search?content_id=269849406&content_type=Article&match_order=1&q=A2UI&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIyODIwNTAsInEiOiJBMlVJIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjY5ODQ5NDA2LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.nNdpBnagxqHAt6NiviuNFKCLMzILh6pNaWBH0mqVokI&zhida_source=entity))

+   Cron (定时任务)

+   System (macOS 命令)

+   Media (图像/音频/视频)

+   Memory (向量存储)

### 3.6 会话管理系统

职责: 管理多代理、多渠道的会话状态

功能:

+   会话创建和销毁

+   上下文持久化 (JSONL 格式)

+   会话隔离 (main/group/)

+   会话激活模式

+   会话队列模式

技术实现:

+   位置: src/sessions/

+   存储: ~/.clawdbot/sessions/ 或 ~/.clawdbot/agents/<id>/sessions/

+   格式: JSON (完整对象，包含 key + meta + history + version)

+   权限: 0o600 (仅所有者可读写)

会话作用域 (SessionScope):

+   peer: 对等方会话 (1对1 直接对话)

+   guild: 服务器/群组会话

+   team: 团队会话

+   account: 账户会话

+   channel: 频道默认会话

+   global: 全局会话

+   custom: 自定义作用域 (任意字符串)

### 3.7 媒体处理管道

职责: 处理图像、音频、视频、文档

功能:

+   图像:

+   压缩和转换 (Sharp)

+   尺寸限制

+   格式转换 (PNG/JPEG/WebP)

+   音频:

+   转码

+   语音转文字 (转录钩子)

+   TTS 合成

+   视频:

+   转码

+   时长和尺寸限制

+   PDF:

+   提取文本

+   渲染页面

+   文档:

+   DOCX 预览

+   文本提取

技术实现:

+   位置: src/media/, src/media-understanding/

+   依赖: sharp, pdfjs-dist, node-edge-tts

### 3.8 浏览器工具

职责: 提供无头浏览器自动化能力

功能:

+   网页导航

+   截图

+   PDF 生成

+   DOM 操作

+   JavaScript 执行

技术实现:

+   引擎: [Playwright](https://zhida.zhihu.com/search?content_id=269849406&content_type=Article&match_order=1&q=Playwright&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIyODIwNTAsInEiOiJQbGF5d3JpZ2h0IiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjY5ODQ5NDA2LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.uLGeMkbjPu7_sjzkXmmzjI0kYXk7Tx_6rqXh27wtZa4&zhida_source=entity) (Chromium)

+   位置: src/browser/

+   沙箱: multi-agent-sandbox-tools

### 3.9 Canvas 系统

职责: 提供代理驱动的可视化界面

功能:

+   A2UI 协议 (Agent-to-UI)

+   实时 WebSocket 更新

+   多平台支持 (macOS/iOS/Android)

+   组件渲染

+   代理控制

技术实现:

+   宿主: src/canvas-host/

+   A2UI: src/canvas-host/a2ui/

+   打包: scripts/bundle-a2ui.sh

### 3.10 语音交互系统

职责: 支持语音输入和输出

功能:

+   Voice Wake:

+   语音唤醒 (macOS/iOS/Android)

+   自定义唤醒词

+   持续监听

+   Talk Mode:

+   语音对话

+   推送通话 (PTT)

+   覆盖层 UI

+   TTS:

+   [ElevenLabs](https://zhida.zhihu.com/search?content_id=269849406&content_type=Article&match_order=1&q=ElevenLabs&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3NzIyODIwNTAsInEiOiJFbGV2ZW5MYWJzIiwiemhpZGFfc291cmNlIjoiZW50aXR5IiwiY29udGVudF9pZCI6MjY5ODQ5NDA2LCJjb250ZW50X3R5cGUiOiJBcnRpY2xlIiwibWF0Y2hfb3JkZXIiOjEsInpkX3Rva2VuIjpudWxsfQ.YN2-36XG_szAmXKGh8-ALBUyE5tWvyWbD4pSNeESsKk&zhida_source=entity) 集成

+   多语言支持

技术实现:

+   TTS: src/tts/

+   依赖: node-edge-tts

+   客户端: apps/macos, apps/ios, apps/android

### 3.11 CLI 工具

职责: 提供命令行操作界面

命令:

+   openclaw gateway - 启动/停止 Gateway

+   openclaw agent - 与 AI 代理交互

+   openclaw message send - 发送消息

+   openclaw channels status - 渠道状态

+   openclaw config - 配置管理

+   openclaw onboard - 引导向导

+   openclaw doctor - 诊断工具

+   openclaw pairing - 配对管理

技术实现:

+   框架: Commander.js

+   位置: src/cli/, src/commands/

+   入口: openclaw.mjs

### 3.12 插件系统

职责: 支持渠道和功能的动态扩展

插件类型:

+   渠道插件: 扩展消息平台支持

+   工具插件: 扩展 AI 代理工具

+   内存插件: 扩展向量存储

+   诊断插件: 扩展监控能力

技术实现:

+   SDK: src/plugin-sdk/

+   加载: src/plugins/

+   位置: extensions/

插件接口:

```
interface Plugin {
  name: string
  version: string
  init(context: PluginContext): Promise&lt;void&gt;
  destroy(): Promise&lt;void&gt;
}
```

### 3.13 配置管理

职责: 管理系统和用户配置

配置层级:

1.  默认配置 (内置)

1.  全局配置 (~/.clawdbot/config.yaml)

1.  项目配置 (openclaw.config.yaml)

1.  环境变量 (覆盖)

技术实现:

+   位置: src/config/

+   格式: JSON5 (支持注释和尾部逗号)

+   文件: ~/.openclaw/config.json

+   验证: Zod Schema

+   权限: 0o600 (仅所有者可读写)

+   工具: openclaw config

配置系统特性:

+   $include 指令: 支持多文件合并

+   环境变量替换: ${VAR} 自动替换

+   原子性写入: 临时文件 + 重命名

+   备份轮转: 自动保留 3 个备份

+   遗留迁移: 自动迁移旧版配置

+   运行时覆盖: 临时修改不持久化

+   配置缓存: 可配置的内存缓存

关键配置:

```
{
  &quot;gateway&quot;: {
    &quot;mode&quot;: &quot;local&quot;,
    &quot;port&quot;: 18789,
    &quot;auth&quot;: {
      &quot;mode&quot;: &quot;token&quot;,
      &quot;token&quot;: &quot;${GATEWAY_TOKEN}&quot;
    }
  },
  &quot;agents&quot;: {
    &quot;defaults&quot;: {
      &quot;model&quot;: &quot;claude-sonnet-4-20250514&quot;,
      &quot;thinking&quot;: &quot;high&quot;,
      &quot;maxConcurrent&quot;: 3
    }
  },
  &quot;channels&quot;: {
    &quot;defaults&quot;: {
      &quot;dmPolicy&quot;: &quot;allow&quot;,
      &quot;groupPolicy&quot;: &quot;allow-mentions&quot;
    },
    &quot;telegram&quot;: {
      &quot;botToken&quot;: &quot;${TELEGRAM_BOT_TOKEN}&quot;
    }
  }
}
```

### 3.14 跨平台应用

### macOS 应用

职责: 菜单栏控制中心

功能:

+   Gateway 启动/停止

+   渠道状态显示

+   Voice Wake 控制

+   Talk Mode 界面

+   WebChat 嵌入

+   Canvas 显示

技术:

+   语言: Swift

+   框架: SwiftUI

+   位置: apps/macos/

### iOS 应用

职责: 移动节点和 Canvas 客户端

功能:

+   Canvas 渲染

+   Voice Wake

+   Talk Mode

+   相机工具

+   屏幕录制

+   Bonjour 配对

技术:

+   语言: Swift

+   框架: SwiftUI

+   位置: apps/ios/

### Android 应用

职责: 移动节点和 Canvas 客户端

功能:

+   Canvas 渲染

+   Talk Mode

+   相机工具

+   屏幕录制

+   可选 SMS 集成

技术:

+   语言: Kotlin

+   框架: Jetpack Compose

+   位置: apps/android/

---

## 数据流

### 4.1 消息接收流程

![](img/ae8a8c372b609500ea54e4bbfecc6193.png)

详细说明：

```
外部消息 
  → 渠道层 (WhatsApp/Telegram/etc.)
    → 频道元数据解析 (channelId, peerId, guildId)
      → 路由解析 (resolveAgentRoute)
        → 6 级路由优先级匹配
        → 7 级配置匹配
          → 多层访问控制检查
            → Allowlist 白名单
            → Mention Gating (群组)
            → Command Gating (控制命令)
              → 会话激活/创建
                → SessionKey 解析 (agentId + scope + peerId)
                  → 消息入队 (双车道)
                    → sessionLane: 单会话串行
                    → globalLane: 全局并发控制
                      → Agent 执行 (runEmbeddedPiAgent)
                        → Auth Profile 选择/轮转
                        → Thinking Level 应用
                        → 上下文溢出检查
                        → 工具调用 (如果需要)
                        → 流式响应生成
                          → 响应处理
                            → 类型转换 (适配频道)
                            → 分块发送 (长消息)
                            → 重试机制
                              → 渠道发送
                                → 会话持久化
                                  → 用户收到回复
```

### 4.2 命令执行流程

```
CLI 命令 
  → Commander 解析
    → 命令处理器 (commands/)
      → Gateway API (WebSocket/HTTP)
        → Gateway 处理
          → 执行操作
            → 返回结果
              → CLI 显示
```

### 4.3 Canvas 更新流程

```
AI 代理 
  → Canvas 工具调用
    → Canvas 宿主 (gateway)
      → WebSocket 推送
        → macOS/iOS/Android 客户端
          → UI 渲染更新
```

### 4.4 语音交互流程

```
用户语音 
  → Voice Wake 检测 (macOS/iOS/Android)
    → STT (语音转文字)
      → Gateway 消息
        → AI 代理处理
          → 响应生成
            → TTS (文字转语音)
              → 用户听到回复
```

---

## 组件依赖关系

### 5.1 核心依赖

```
Gateway 控制平面
  ├── WebSocket 服务器 (ws)
  ├── HTTP 服务器 (Fastify)
  ├── JSON-RPC 调度器
  │   ├── 认证系统 (5种模式)
  │   ├── 授权系统 (5个作用域)
  │   └── 70+ 方法处理器 (8个命名空间)
  ├── 渠道系统
  │   ├── 9 个内置频道
  │   └── 15+ 扩展频道
  ├── 路由系统
  │   ├── 6 级路由优先级
  │   ├── 7 级配置匹配
  │   └── 多层访问控制
  ├── Chat Run Registry
  ├── AI 代理引擎
  │   ├── 双车道排队 (sessionLane + globalLane)
  │   ├── Pi Agent Core
  │   ├── Auth Profile 轮转
  │   ├── 模型提供者
  │   └── 工具注册表
  ├── 广播管理器
  ├── Canvas 宿主
  └── Cron 调度
```

### 5.2 客户端依赖

```
macOS/iOS/Android 应用
  └── Gateway WebSocket
      ├── 连接管理
      ├── 消息同步
      ├── Canvas 更新
      └── 状态订阅
```

### 5.3 插件依赖

```
插件包 (extensions/)
  └── Plugin SDK
      ├── 插件接口
      ├── 上下文访问
      └── 生命周期钩子
```

---

## 外部集成

![](img/d02b0ba6d0ee24495f0027618a5c8a05.png)

### 6.1 AI 模型提供者

+   Anthropic: Claude (官方 API + OAuth)

+   OpenAI: ChatGPT (官方 API + OAuth)

+   AWS Bedrock: 多模型支持

+   Ollama: 本地模型运行

+   node-llama-cpp: 本地 LLM (可选)

### 6.2 消息平台

+   WhatsApp: Baileys Web 协议

+   Telegram: Bot API

+   Slack: Bolt SDK

+   Discord: Discord.js

+   Google Chat: Chat API

+   Signal: signal-cli

+   iMessage: imsg/BlueBubbles

+   Microsoft Teams: Bot Framework

+   Matrix: Matrix SDK

+   Zalo: Zalo API

### 6.3 语音服务

+   ElevenLabs: TTS 合成 (node-edge-tts)

+   本地 STT: macOS/iOS/Android 系统 API

### 6.4 浏览器

+   Chromium: Playwright 驱动

+   BiDi: Chromium BiDi 协议

### 6.5 网络服务

+   Bonjour: mDNS 服务发现 (@homebridge/ciao)

+   Tailscale: (可选) 远程访问

+   [http://Fly.io](https://link.zhihu.com/?target=http%3A//Fly.io): (可选) 远程部署

---

## 部署架构

### 7.1 本地部署 (推荐)

```
┌─────────────────────────────────────┐
│  用户设备 (macOS/Linux/Windows)      │
│                                     │
│  ┌───────────────────────────────┐ │
│  │  Gateway 守护进程              │ │
│  │  (systemd/launchd)            │ │
│  └───────────────────────────────┘ │
│                                     │
│  ┌───────────────────────────────┐ │
│  │  macOS 菜单栏应用 (可选)       │ │
│  └───────────────────────────────┘ │
│                                     │
│  ┌───────────────────────────────┐ │
│  │  Web 控制界面                  │ │
│  │  http://localhost:18789       │ │
│  └───────────────────────────────┘ │
└─────────────────────────────────────┘
```

### 7.2 远程部署

```
┌─────────────────────────────────────┐
│  云服务器 (VPS/Fly.io)               │
│                                     │
│  ┌───────────────────────────────┐ │
│  │  Docker 容器                   │ │
│  │  └── Gateway 服务              │ │
│  └───────────────────────────────┘ │
│                                     │
│  ┌───────────────────────────────┐ │
│  │  Nginx 反向代理                │ │
│  │  (HTTPS/WSS)                  │ │
│  └───────────────────────────────┘ │
└─────────────────────────────────────┘
          ↓ WebSocket/HTTPS
┌─────────────────────────────────────┐
│  用户设备                            │
│  - iOS 节点                         │
│  - Android 节点                     │
│  - macOS 应用                       │
└─────────────────────────────────────┘
```

### 7.3 Docker 部署

```
docker-compose.yml
  ├── gateway 服务
  ├── 卷挂载 (配置/会话)
  └── 端口映射 (18789)
```

---

## 扩展性设计

### 8.1 水平扩展

+   插件系统: 独立 npm 包，按需安装

+   渠道扩展: 新渠道作为插件添加

+   工具扩展: 新工具通过 SDK 注册

### 8.2 垂直扩展

+   多代理: 每个代理独立进程

+   会话隔离: 多租户会话管理

+   资源限制: 可配置的并发和队列

### 8.3 未来扩展方向

+   分布式 Gateway (多节点)

+   集群会话同步

+   云端会话备份

+   企业级权限管理

---

## 安全架构

### 9.1 认证和授权

+   DM 配对: 默认拒绝未知 DM

+   白名单: 细粒度访问控制

+   OAuth: AI 模型认证

+   API Key: 备用认证方式

### 9.2 数据安全

+   本地存储: 会话和配置本地文件

+   加密传输: WebSocket + HTTPS

+   临时文件: 自动清理媒体缓存

+   隔离会话: 代理间完全隔离

### 9.3 风险防护

+   提示注入: 模型层防护

+   速率限制: 渠道节流 (throttler)

+   Doctor 工具: 配置安全检查

+   沙箱隔离: 浏览器工具沙箱

---

## 性能优化

### 10.1 缓存策略

+   媒体缓存: 临时文件系统

+   会话缓存: 内存 + 持久化

+   配置缓存: 热重载机制

### 10.2 并发控制

+   渠道节流: @grammyjs/transformer-throttler

+   队列模式: 会话级别排队

+   工具池: 浏览器实例复用

### 10.3 资源管理

+   自动清理: 临时文件生命周期

+   内存限制: 可配置的缓存大小

+   连接池: 数据库和 HTTP 连接

---

## 监控和诊断

### 11.1 日志系统

+   分级日志: tslog (trace/debug/info/warn/error)

+   结构化日志: JSON 格式

+   日志轮转: 按日期归档

### 11.2 诊断工具

+   Doctor 命令: openclaw doctor

+   配置检查

+   安全审计

+   版本检查

+   依赖验证

+   状态命令: openclaw channels status --deep

+   渠道探测

+   连接测试

+   API 可达性

### 11.3 可观测性 (可选)

+   OpenTelemetry: diagnostics-otel 插件

+   指标收集: 自定义 metrics

+   追踪: 分布式追踪 (可选)

---

## 总结

OpenClaw 的架构设计体现了以下核心理念：

1.  本地优先: 数据和隐私完全掌控

1.  插件化: 易于扩展和定制

1.  多渠道: 统一抽象，广泛兼容

1.  安全默认: 配对模式和白名单

1.  跨平台: CLI + 原生应用全覆盖

1.  AI 驱动: 强大的代理引擎和工具生态

这种架构既保证了系统的灵活性和可扩展性，又确保了个人助手应有的隐私性和安全性，是一个成熟的、生产级的个人 AI 助手平台。