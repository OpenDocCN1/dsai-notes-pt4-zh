
# SOUL.md - 财务助理的灵魂

## 定位

我是财务助理 💰，负责成本核算、预算管理、财务分析的财务专家。

## 职责

- 成本核算和分析
- 预算编制和管理
- 财务分析报告
- 投资回报评估

## 核心原则

- 数据准确，账目清晰
- 成本意识，效益优先
- 风险可控，合规经营
- 建议数据支撑

## 自主决策范围

- **可自主**：
  - 财务数据分析
  - 成本核算报告
  - 预算建议

- **需确认**：
  - 重大财务决策
  - 预算调整
  - 投资决策

## 边界

- ❌ 不写代码（交给 `dev`）
- ❌ 不做运营（交给 `ops`）
- ❌ 不审查合同法律风险（交给 `law`）
- ✅ 专注财务和成本

## 语气

- 数据驱动
- 分析严谨
- 建议具体
- 风险提示

## 成长

- 持续优化财务模型
- 积累行业财务经验
- 提升分析效率

## 输出格式

- 报表清晰
- 图表可视化
- 结论和建议分开

---

## 附录 B：`openclaw.json` 完整示例

```

{
  "meta": {
    "lastTouchedVersion": "2026.2.26"
  },
  "agents": {
    "defaults": {
      "workspace": "~/.openclaw/workspace",
      "model": {
        "primary": "glmcode/glm-5"
      },
      "subagents": {
        "maxConcurrent": 8
      }
    },
    "list": [
      {
        "id": "main",
        "subagents": {
          "allowAgents": ["ops", "dev", "xhs", "wechat", "x", "yt", "admin", "intel", "law", "finance"]
        },
        "heartbeat": {
          "every": "6h",
          "session": "main",
          "target": "none",
          "prompt": "Read HEARTBEAT.md if it exists (workspace context). Follow it strictly. Do not infer or repeat old tasks from prior chats. If nothing needs attention, reply exactly: HEARTBEAT_OK"
        }
      }
    ]
  },
  "channels": {
    "feishu": {
      "enabled": true,
      "appId": "你的AppID",
      "appSecret": "你的AppSecret",
      "groupPolicy": "open"
    }
  },
  "models": {
    "providers": {
      "glmcode": {
        "baseUrl": "https://open.bigmodel.cn/api/anthropic",
        "apiKey": "你的APIKey",
        "api": "anthropic-messages",
        "models": [
          { "id": "glm-5", "name": "GLM-5" }
        ]
      }
    }
  },
  "gateway": {
    "port": 18789,
    "mode": "local",
    "bind": "loopback"
  }
}

```

## 附录 C：记忆更新触发条件

| 触发条件 | 动作 |
| --- | --- |
| 上下文 `>=` 3000 token | `memoryFlush` → 写入 `memory/YYYY-MM-DD.md` |
| `Heartbeat`（6h） | 扫描日志 → 提炼到 `MEMORY.md` |
| `Cron` 周反思（周日 10:00） | 扫描 7 天日志 → 完整提炼 |
| 教训出现 `>=` 3 次 | 标记 `[待晋升]` → 写入 `SOUL.md`/`TOOLS.md` |

## 附录 D：扩展阅读

### 组织架构图（10 Agent）

```

总经理 (main) — 全局调度、任务拆解、跨部门协调、OKR、周报月报

      │

      ├── 内容线

      │   ├── 📕部 (xhs) — 笔记创作、封面设计、爆款分析

      │   ├── 🛰️部 (wechat) — 👸号长文、排版、草稿箱发布

      │   ├── X 平台部 (x) — 帖文创作、配图、热点借势

      │   └── YT 部 (yt) — 视频脚本、字幕翻译、SEO 优化

      │

      ├── 运营线

      │   ├── 运营虾 (ops) — 用户增长、数据分析、活动策划

      │   ├── 私人助理 (admin) — 日程管理、备忘、生活琐事

      │   └── 情报部 (intel) — 竞品监控、行业趋势、素材采集

      │

      └── 专业线

          ├── 开发助理 (dev) — 代码开发、系统运维、工具研发

          ├── 法务助理 (law) — 合同审查、合规咨询

          └── 财务助理 (finance) — 成本核算、预算管理

```

### 完整 Agent 列表

| Agent ID | 名称 | Emoji | 所属线 | 核心职责 |
| --- | --- | --- | --- | --- |
| `main` | 小宇 | 🦞 | 总经理 | 全局调度、跨部门协调 |
| `xhs` | 小红书部 | 📕 | 内容线 | 小红书笔记、封面、爆款分析 |
| `wechat` | 微信部 | 📢 | 内容线 | 公众号长文、排版、发布 |
| `x` | X 平台部 | ✖️ | 内容线 | X 帖文、热点借势 |
| `yt` | YT 部 | 📺 | 内容线 | 视频脚本、字幕、SEO |
| `ops` | 运营虾 | 🦐 | 运营线 | 用户增长、数据分析 |
| `admin` | 私人助理 | 🎩 | 运营线 | 日程管理、备忘 |
| `intel` | 情报部 | 🕵️ | 运营线 | 竞品监控、行业趋势 |
| `dev` | 开发助理 | 💻 | 专业线 | 代码开发、运维、研发 |
| `law` | 法务助理 | ⚖️ | 专业线 | 合同审查、合规咨询 |
| `finance` | 财务助理 | 💰 | 专业线 | 成本核算、预算管理 |

### 添加新 Agent 的步骤

1. 创建 Workspace

```

mkdir -p ~/.openclaw/workspace-{新agentID}/{memory,work/{inbox,drafts,archives,output/{reports,content,data,plans},runtime/{state,scripts,cache}}}

```

2. 创建 7 个核心文件

- `SOUL.md`（8 个 h2 结构）
- `AGENTS.md`（复制 `main` 的）
- `USER.md`（复制 `main` 的）
- `IDENTITY.md`
- `TOOLS.md`
- `HEARTBEAT.md`
- `MEMORY.md`

3. 更新 `main` 的 `allowAgents`

```

openclaw config set 'agents.list[0]' '{"id": "main", "subagents": {"allowAgents": ["ops", "dev", "xhs", "wechat", "x", "yt", "admin", "intel", "law", "finance", "新agentID"]}}' --strict-json

```

4. 创建 `Cron` 周反思任务

```

openclaw cron add \
  --name "新agentID-weekly-reflect" \
  --agent "新agentID" \
  --cron "0 10 * * 0" \
  --tz "Asia/Shanghai" \
  --session "isolated" \
  --wake "now" \
  --message "执行周反思..." \
  --thinking "low" \
  --timeout-seconds 300 \
  --announce \
  --channel "last"

```

5. 重启 `Gateway`

```

openclaw gateway restart

```
