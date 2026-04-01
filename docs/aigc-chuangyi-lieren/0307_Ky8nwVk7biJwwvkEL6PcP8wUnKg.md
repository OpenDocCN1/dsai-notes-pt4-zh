# OpenClaw CLI 超级速查表

> 来源：[https://aigcreative.feishu.cn/docx/TkaQdn0KroVS5XxvZFxcUuTFnBe](https://aigcreative.feishu.cn/docx/TkaQdn0KroVS5XxvZFxcUuTFnBe)

## 一、核心CLI命令

### 网关管理

```
openclaw gateway              # 运行WebSocket网关服务器
openclaw gateway start|stop|restart   # 管理网关服务（launchd/systemd）
```

### 频道管理

```
openclaw channels login              # WhatsApp二维码配对（网页流程）
openclaw channels add                # 添加Telegram/Discord/Slack机器人
openclaw channels status --probe     # 检查频道健康状态与连接性
```

### 系统设置

```
openclaw onboard             # 交互式设置向导
openclaw doctor              # 健康检查与快速修复
openclaw config get|set|unset     # 读取/写入配置值
```

### 模型管理

```
openclaw models list|set|status           # 模型管理与认证状态
openclaw models auth setup-token          # 推荐的Anthropic认证流程
openclaw models fallbacks add|remove      # 配置模型回退链
```

### 内存系统

```
openclaw memory status|index|search      # 对MEMORY.md和每日日志进行向量搜索
```

### 代理与会话

```
openclaw agents list|add|delete    # 多代理工作区管理
openclaw sessions --json           # 列出存储的会话
```

### 诊断与日志

```
openclaw logs --follow            # 通过RPC跟踪网关文件日志
openclaw status --all --deep      # 完整诊断（可粘贴分享）
openclaw security audit           # 审计配置中的安全隐患
```

### 浏览器与定时任务

```
openclaw browser start|tabs|screenshot   # 无头浏览器自动化
openclaw cron list|add|run               # 定时任务管理
```

### 设备配对

```
openclaw pairing list|approve    # 批准DM配对请求
openclaw reset --scope           # 重置配置/凭证/会话/全部
```

---

## 二、全局标志

```
--dev                    # 在 ~/.openclaw-dev 下隔离状态，切换端口
--profile &lt;name&gt;         # 在 ~/.openclaw-&lt;name&gt; 下隔离状态
--no-color               # 禁用ANSI颜色（尊重 NO_COLOR=1）
--update                 # openclaw update 的简写（仅源码安装）
-V, --version, -v        # 打印版本并退出
```

---

## 三、高级命令

```
# 插件管理
openclaw plugins list|enable|disable

# 审批策略
openclaw approvals get|set|allowlist

# 沙盒管理
openclaw sandbox list|recreate|explain

# 系统事件
openclaw system event --text &quot;X&quot;     # 立即/心跳时排队系统事件
openclaw system heartbeat enable|disable|last   # 控制心跳运行

# 更新管理
openclaw update status|wizard

# 节点管理
openclaw nodes list|pending|approve

# 设备管理
openclaw devices list|approve|rotate|revoke

# 健康探测
openclaw health --json --verbose

# 控制面板
openclaw dashboard        # 使用当前令牌打开控制UI
openclaw tui --url --token    # 远程安全的终端UI
```

---

## 四、快速开始

### 安装

```
npm install -g openclaw
```

### 引导设置

```
openclaw onboard --install-daemon    # 安装守护进程
openclaw channels login              # 登录频道
openclaw gateway --port 18789        # 在指定端口启动网关
```

### 引导标志

```
--mode local         # 本地网关模式
--mode remote        # 连接到远程网关
--flow quickstart    # 最小化设置
--skip-channels      # 跳过频道设置
```

---

## 五、频道设置

```
openclaw channels login              # WhatsApp登录
openclaw channels add --channel telegram --token $TOKEN   # 添加Telegram
openclaw channels add --channel discord --token $TOKEN   # 添加Discord
openclaw channels add --channel slack                    # 添加Slack
openclaw channels add --channel googlechat               # 添加Google Chat
openclaw channels add --channel signal                   # 添加Signal
openclaw channels add --channel msteams                  # 添加Microsoft Teams
```

### 快速诊断

```
openclaw channels status --probe    # 检查状态
openclaw channels logs --channel [id] # 查看日志
```

---

## 六、工作区文件

| 文件 | 用途 |
| --- | --- |
| BOOT.md | 代理的操作指令 |
| SOUL.md | 个性、语气、边界 |
| USER.md | 用户信息与偏好 |
| AGENT.md | 代理名称、表情符号、主题 |
| MEMORY.md | 精选的长期记忆（仅DM） |
| YYYY-MM-DD.md | 每日追加日志 |
| TOOL.md | 本地工具说明 |
| HEARTBEAT.md | 心跳检查清单 |
| STARTUP.md | 启动检查清单 |

> 根目录：~/.openclaw/workspace

---

## 七、斜杠命令

```
/status              # 会话健康 + 上下文使用 + WhatsApp凭证状态
/context list        # 上下文窗口中的内容（最大贡献者）
/context detail      # 完整的系统提示 + 注入的工作区文件
/model &lt;model&gt;       # 为此会话切换模型
/compact [instructions]  # 总结较早的上下文，释放窗口空间
/new [model]         # 开始新会话（可选：设置模型）
/reset               # /new 的别名
/stop                # 中止当前运行 + 清除排队的后续操作
/send on|off|inherit # 覆盖此会话的传递设置
/tts on|off          # 切换文本转语音
/think|/verbose       # 切换推理/详细模式
/config              # 持久化配置更改
/debug               # 仅运行时配置覆盖
```

---

## 八、浏览器操作

```
openclaw browser start|stop        # 启动/停止无头实例
openclaw browser tabs              # 列出所有打开的页面
openclaw browser open &lt;url&gt;        # 在新标签页打开URL
openclaw browser screenshot        # 捕获活动视图
openclaw browser navigate &lt;url&gt;    # 导航当前标签页
openclaw browser click|type|press  # DOM交互
openclaw browser evaluate &lt;js&gt;     # 在页面中运行JavaScript
openclaw browser pdf               # 将页面导出为PDF
```

---

## 九、定时任务

```
openclaw cron list               # 查看定时任务
openclaw cron add                # 创建新的定时任务
openclaw cron edit &lt;id&gt;          # 编辑现有任务
openclaw cron enable|disable &lt;id&gt; # 切换任务状态
openclaw cron run &lt;id&gt;           # 手动触发
openclaw cron runs               # 查看运行历史
```

---

## 十、技能系统

### 安装与管理

```
clawhub install &lt;slug&gt;     # 从ClawHub安装技能
clawhub update --all       # 更新所有已安装的技能
clawhub sync --all         # 扫描并发布更新
```

### 技能优先级（从高到低）

1.  <workspace>/skills/ - 工作区技能

1.  ~/.openclaw/skills/ - 用户全局技能

1.  捆绑技能 - 内置技能

---

## 十一、语音合成

支持提供商：

+   **ElevenLabs** - 超真实，延迟较高

+   **OpenAI** - 快速，高质量语音

+   **Cartesia** - 无需API密钥，多语言支持

### 启用自动TTS

在配置中设置：

```
messages.tts.auto: &quot;always&quot;
```

---

## 十二、内存命令

```
openclaw memory index --all    # 索引所有内存
openclaw memory search &quot;X&quot;     # 搜索内存
```

### 内存类型

+   **每日日志**：仅追加，会话开始时读取今天+昨天内容

+   **MEMORY.md**：精选事实，仅在主DM会话中加载

+   **语义搜索**：对内存块进行语义搜索（约400令牌）

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>

# 知识库动态