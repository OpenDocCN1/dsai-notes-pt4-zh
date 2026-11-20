
# 国产模型大多可以通过 OpenAI 兼容接口或 OpenRouter 接入

## 通义千问（阿里云 DashScope）— 通过 OpenAI 兼容接口

在 `~/.openclaw/openclaw.json` 中配置：

```json
{
  "models": {
    "providers": {
      "dashscope": {
        "baseUrl": "https://dashscope.aliyuncs.com/compatible-mode/v1"
      }
    }
  },
  "agents": { "defaults": { "model": "dashscope/qwen-max" } }
}
```

API Key 通过环境变量设置（参考 DashScope 文档）

## Moonshot / Kimi — 通过 OpenAI 兼容接口

`baseUrl: "https://api.moonshot.cn/v1"`

## DeepSeek — 通过 OpenAI 兼容接口

`baseUrl: "https://api.deepseek.com/v1"`

## 最简单的方式：通过 OpenRouter 一站式接入所有国产模型

```shell
export OPENROUTER_API_KEY="sk-or-xxxxx"
openclaw config set agents.defaults.model "openrouter/deepseek/deepseek-chat"
```

国产模型的优势：不需要代理、中文能力强、价格便宜。推荐中国用户优先考虑。

### Q33: 怎么配置模型故障转移（Fallback）？

**说明：** 当主模型不可用时，自动切换到备用模型，保证服务不中断。

在 `~/.openclaw/openclaw.json` 中配置 Fallback 链：

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "openai/gpt-5.2",
        "fallbacks": [
          "anthropic/claude-sonnet-4-6",
          "openai/gpt-5.2-mini"
        ]
      }
    }
  }
}
```

当主模型返回 `5xx`、超时或 `429` 限流时，自动按顺序尝试备用模型。

查看 Fallback 状态：

```shell
openclaw models fallbacks list
openclaw models status
```

### Q34: API 费用太高怎么控制成本？

**解决方案：**

**方案一：在模型提供商控制台设置预算上限**

- OpenAI: Settings &gt; Limits 设置月度上限
- Anthropic: Plans &amp; Billing 查看用量
- Google Cloud: 设置 API 配额和预算提醒
- OpenClaw 本身没有内置预算控制配置

**方案二：使用更便宜的模型**

`gpt-5.2-mini` 价格是 `gpt-5.2` 的 1/10

```shell
openclaw config set agents.defaults.model "openai/gpt-5.2-mini"
```

**方案三：精简记忆文件减少 Token 消耗**

`MEMORY.md` 建议控制在 500 行以内
精简 `SOUL.md` 系统提示词

**方案四：查看系统状态**

```shell
openclaw status
```

**方案五：本地模型零成本**

```shell
ollama pull llama3.1:8b
openclaw config set agents.defaults.model "ollama/llama3.1:8b"
```

### Q35: 怎么让不同的对话用不同的模型？

**说明：** 可以为不同的 Agent 配置不同的模型，通过在配置文件中为每个 Agent 指定独立的模型。

**方案一：为不同 Agent 配置不同模型**

在 `~/.openclaw/openclaw.json` 中配置：

```json
{
  "agents": {
    "defaults": {
      "model": "openai/gpt-5.2-mini"
    },
    "list": [
      { "id": "coding", "model": "anthropic/claude-sonnet-4-6" },
      { "id": "casual", "model": "openai/gpt-5.2-mini" }
    ]
  }
}
```

**方案二：使用模型别名快速切换**

```shell
openclaw models aliases add code-review "anthropic/claude-opus-4-6"
openclaw models aliases add quick-chat "openai/gpt-5.2-mini"
```

**方案三：通过 bindings 将不同 Channel 路由到不同 Agent**

每个 Agent 使用各自配置的模型。详见 `08-多Agent协作指南.md`。

---

## 四、技能和工具问题

> 📌 本节包含 10 个问题：
>
> **Q36: 技能不生效 | Q37: 自定义技能报错 | Q38: 工具执行失败 | Q39: 禁用工具 | Q40: 添加新工具 | Q41: 技能中途中断 | Q42: 查看工具调用 | Q43: 内置技能列表 | Q44: 技能冲突 | Q45: 分享技能**

### Q36: 技能（Skill）不生效，AI 不执行

**现象：** 你发消息让 AI 执行某个技能（比如「帮我搜索 xxx」），但 AI 只是用文字回复了一段话，并没有真正调用工具去执行操作。日志中也看不到工具调用记录。

**原因：** 技能没有正确加载，或者触发条件不匹配。

**解决方案：**

**第一步：检查技能是否已加载**

```shell
openclaw skills list
```

确认目标技能在列表中且状态为 `enabled`。

**第二步：检查技能的触发关键词**

```shell
openclaw skills info <skill-name>
```

看 `triggers` 字段，确认你的消息包含触发词。

**第三步：手动触发技能测试**

```shell
openclaw skills check <skill-name> --input "测试消息"
```

**第四步：重新加载技能**

```shell
openclaw skills list
```

### Q37: 自定义技能报错

**现象：** 在 `~/.openclaw/workspace/skills/` 目录下创建了自定义技能的 `.md` 文件，但执行 `openclaw skills list` 时该技能未出现，或者出现 `skill parse error`、`invalid skill format` 等报错信息。

**原因：** 技能文件格式不对，或者引用了不存在的工具。

**解决方案：**

**第一步：检查技能文件格式**

```shell
openclaw skills check ~/.openclaw/workspace/skills/my-skill.md
```

**第二步：确认引用的工具都存在**

```shell
openclaw skills list
```

对照技能文件中引用的工具名称。

技能文件的基本格式：

```shell
cat ~/.openclaw/workspace/skills/my-skill.md
```

正确的技能文件结构：

```yaml
---
name: my-custom-skill
description: 我的自定义技能
triggers:
  - "帮我做xxx"
  - "执行xxx"
tools:
  - file_read
  - file_write
  - web_search
---

## 指令

当用户要求做 xxx 时，按以下步骤执行：

1. 先搜索相关信息
2. 读取必要的文件
3. 生成结果并写入文件
```

### Q38: 工具（Tool）执行失败

**现象：** AI 回复中显示「正在调用 xxx 工具」，但随后报错 `tool execution failed`、`permission denied` 或 `tool not found`，操作没有完成。

**原因：** 工具缺少必要的权限或依赖。

**解决方案：**

**第一步：检查工具状态**

```shell
openclaw doctor
```

诊断安全和配置问题。

**第二步：测试单个工具**

```shell
openclaw skills check <skill-name>
```

**第三步：检查工具依赖**

```shell
openclaw skills info <skill-name>
```

看 `dependencies` 字段，确认依赖都已安装。

**第四步：常见工具问题**

- `web_search` 需要网络连接
- `file_write` 需要目标目录有写权限
- `shell_exec` 需要在配置中启用（默认禁用，出于安全考虑）

### Q39: 怎么禁用某个工具？

**说明：** 出于安全考虑，你可能想禁用某些危险工具。

工具权限通过沙箱模式控制。在 `~/.openclaw/openclaw.json` 中配置沙箱：

```json
{
  "agents": {
    "defaults": {
      "sandbox": { "mode": "all" }
    }
  }
}
```

`sandbox.mode`: `"off"`（默认，不限制）| `"non-main"`（限制非主 Agent）| `"all"`（限制所有 Agent）。沙箱模式会限制文件系统访问和命令执行。

也可以在 `SOUL.md` 中明确告诉 AI 不要使用某些工具。例如：`"永远不要执行 shell 命令，不要删除文件"`。

查看当前工具权限配置：

```shell
openclaw doctor
```

### Q40: 怎么给 AI 添加新工具？

**说明：** OpenClaw 支持通过 MCP（Model Context Protocol）协议接入外部工具。

**方案一：使用内置的 MCP 服务器**

```shell
openclaw plugins
```

**方案二：接入第三方 MCP 服务器**

在 `~/.openclaw/openclaw.json` 中配置 MCP 服务器连接。

**方案三：自己写工具（TypeScript）**

在 `~/.openclaw/workspace/tools/` 下创建工具文件。参考文档：`https://docs.openclaw.ai/tools/custom`。

### Q41: 技能执行到一半中断了

**现象：** AI 在执行一个多步骤技能时（比如「搜索 → 读取文件 → 生成报告」），执行到中间某一步后突然停下来，不再继续后续步骤，也没有给出完成提示。

**原因：** 可能是 Token 限制导致输出被截断，或者某个中间步骤失败了。

**解决方案：**

**第一步：查看会话日志，找到中断点**

使用前台模式运行 Gateway 查看详细日志：

```shell
openclaw gateway --port 18789 --verbose
```

**第二步：Token 限制导致截断的解决方案**

不同模型有不同的最大输出限制（如 `GPT-5.2` 最大 32K tokens）。换用输出限制更大的模型，或精简系统提示词。

**第三步：让 AI 继续**

直接发消息 `"继续"` 或 `"请继续执行"`。

**第四步：如果是工具执行失败导致的中断**

```shell
openclaw skills check <failed-tool-name>
```

修复工具问题后重试。

### Q42: 怎么查看 AI 调用了哪些工具？

**解决方案：**

查看最近会话的工具调用记录：

```shell
openclaw sessions list
```

实时查看工具调用（调试模式）：

```shell
openclaw logs --follow
```

日志中会显示每次工具调用的详细信息。

查看系统状态：

```shell
openclaw status
```

### Q43: 内置技能列表在哪里看？

**解决方案：**

列出所有技能（包括内置技能）：

```shell
openclaw skills list
```

查看某个技能的详细说明：

```shell
openclaw skills info <skill-name>
```

### Q44: 技能之间会冲突吗？

**说明：** 如果多个技能的触发条件重叠，可能会出现冲突。OpenClaw 会按优先级选择。

查看技能列表：

```shell
openclaw skills list
```

查看特定技能的详细信息：

```shell
openclaw skills info <skill-name>
```

### Q45: 怎么分享自定义技能给别人？

**解决方案：**

查看技能文件：

```shell
openclaw skills list
```

技能文件位于 `~/.openclaw/workspace/skills/` 目录。可以直接复制 `.md` 文件来分享技能。

查看社区技能库：`https://github.com/openclaw/openclaw-skills`。

---

## 五、部署和运维问题

> 📌 本节包含 10 个问题：
>
> **Q46: Docker 容器立即退出 | Q47: 容器内存过高 | Q48: 数据卷备份 | Q49: 自动重启 | Q50: 查看日志 | Q51: 监控运行状态 | Q52: 磁盘空间清理 | Q53: 多服务器部署 | Q54: 设置 HTTPS | Q55: 自动更新**

### Q46: Docker 容器启动后立即退出

**现象：** 执行 `docker compose up -d` 后，运行 `docker ps` 看不到 OpenClaw 容器，用 `docker ps -a` 查看发现容器状态为 `Exited (1)` 或其他非零退出码。

**原因：** 配置文件缺失、端口冲突、或者环境变量没设置。

**解决方案：**

**第一步：查看容器退出日志**

```shell
docker compose logs openclaw
```

**第二步：常见原因排查**

- 原因 1：配置文件不存在

  ```shell
  ls ./data/openclaw.json
  ```

  确认挂载的配置文件存在。

- 原因 2：端口被占用

  ```shell
  lsof -i :18789
  ```

  如果被占用，修改 `docker-compose.yml` 中的端口映射。

- 原因 3：环境变量缺失

  确认 `.env` 文件存在且包含必要变量：

  ```shell
  cat .env
  ```

**第三步：用前台模式启动，方便看日志**

```shell
docker compose up
```

不加 `-d`。

### Q47: Docker 容器内存占用过高

**现象：** 运行 `docker stats` 发现 OpenClaw 容器的内存使用量持续增长，从几百 MB 涨到好几 GB，甚至导致宿主机变卡或 OOM（Out of Memory）被系统杀掉。

**原因：** 连接了太多平台、会话数据积累、或者存在内存泄漏。

**解决方案：**

查看容器资源使用：

```shell
docker stats openclaw
```

**方案一：限制容器内存**

在 `docker-compose.yml` 中添加：

```yaml
deploy:
  resources:
    limits:
      memory: 1G
```

**方案二：清理会话数据**

```shell
docker exec openclaw openclaw sessions cleanup
```

**方案三：减少同时连接的平台数量**

**方案四：定期重启容器（临时方案）**

配置 `crontab`：

```shell
0 4 * * * docker compose restart openclaw
```

### Q48: Docker 数据卷怎么备份？

**解决方案：**

**方案一：直接备份挂载目录**

```shell
tar -czf openclaw-backup-$(date +%Y%m%d).tar.gz ./data/
```

**方案二：备份 Docker volume**

```shell
docker run --rm \
  -v openclaw_data:/source \
  -v $(pwd):/backup \
  alpine tar -czf /backup/openclaw-data.tar.gz -C /source .
```

**方案三：自动化备份脚本**

建议配合 `crontab` 每天自动备份：

```shell
0 3 * * * /path/to/backup-script.sh
```

### Q49: Gateway 进程崩溃后怎么自动重启？

**解决方案：**
