
![](img/8f4da13bb49598023a5af00d3802976c.png)

### 文件职责说明

| 文件/组件 | 职责 | 说明 |
| --- | --- | --- |
| `.claude/settings.json` | Hooks配置 | 定义钩子触发条件和执行命令 |
| `preToolUse` | 触发时机 | 在工具使用前触发 |
| `Bash(git commit)` | 匹配规则 | 匹配特定工具调用 |
| `hook command` | 执行脚本 | 调用Gemini审查代码 |

### 实际案例：Git提交前自动质检

每次commit前，自动调用Gemini审查所有改动。

### 配置 `.claude/settings.json`

```
{
  "hooks": {
    "preToolUse": {
      "Bash(git commit)": [{
        "command": "bash",
        "args": [
          "-c",
          "gemini -p \"请审查以下代码改动：$(git diff --cached)\" > .review.md && cat .review.md"
        ]
      }]
    }
  }
}
```

### 使用效果

```
老金：git commit -m "feat: 添加用户登录"

[Hook自动触发]
正在调用Gemini审查代码改动...

Gemini报告：
代码质量：良好
发现1个安全问题：密码传输未加密
建议：使用HTTPS或加密传输

Claude：老金，Gemini发现了安全问题，是否继续提交？
```

**优点**：全自动，防止低质量代码提交

**缺点**：调试困难，容易误触发

---

## 5种方式对比总结

| 方式 | 上手难度 | 灵活性 | 成本 | 适合场景 | 不适合场景 |
| --- | --- | --- | --- | --- | --- |
| `Command` | ⭐ 最简单 | ⭐⭐ 低 | 低 | 新手、简单任务 | 复杂流程 |
| `Skill` | ⭐⭐⭐ 中等 | ⭐⭐⭐⭐⭐ 极高 | 低 | 复杂工作流、需复用 | 一次性任务 |
| `MCP` | ⭐⭐⭐⭐ 较难 | ⭐⭐⭐ 中 | 中 | 长期协作、上下文共享 | 一次性任务 |
| `Subagent` | ⭐⭐ 简单 | ⭐⭐⭐⭐ 高 | 高 | 并行任务、预算充足 | 串行任务、预算紧张 |
| `Hooks` | ⭐⭐⭐ 中等 | ⭐⭐ 低 | 低 | 自动化、强制质检 | 需要人工判断 |

### 老金的建议

1. 新手从`Command`开始，熟悉流程
2. 进阶用`Skill`，配置一次长期受益
3. 需要上下文共享用`MCP`，但要会写MCP Server
4. 并行任务用`Subagent`，速度优先
5. 质检流程用`Hooks`，自动化防错

**可以组合使用**！比如老金我现在的配置：日常开发用`Skill`（可复用）；大型重构用`MCP`（上下文打通）；Git提交用`Hooks`自动质检；紧急任务用`Command`快速搞定。

---

## 踩坑记录

### 坑1：Codex CLI需要订阅

**问题**：安装了Codex但提示"需要ChatGPT Plus订阅"

**解决**：Codex CLI需要ChatGPT Plus/Pro/Business订阅才能使用，如果没有订阅，可以订阅ChatGPT Plus（$20/月）或者用其他代码生成工具替代。

### 坑2：Gemini配额限制

**问题**：Gemini CLI安装后提示"超过免费配额"

**解决**：使用网页授权方式获取1000次/天的配额：

1. 终端运行 `gemini auth login`
2. 浏览器自动打开，用Google账号登录授权
3. 授权完成后自动配置

或者用API Key方式（只有100次/天）：

1. 访问 https://aistudio.google.com/apikey
2. 创建API Key并配置到环境变量

老金我实测，1000次/天对个人开发足够用了

### 坑3：MCP Server不会写

**问题**：不懂怎么写MCP Server包装本地CLI

**解决**：参考官方文档（https://code.claude.com/docs/en/mcp）或者先用Command/Skill方式，MCP可以以后再学。

### 坑4：Subagent token消耗太高

**问题**：启动3个Subagent，token消耗翻了3倍

**解决**：

1. 简单任务别用Subagent
2. 用haiku模型（便宜）
3. 只在真正需要并行时才用

### 坑5：Hooks触发太频繁

**问题**：每次`Write`都触发Gemini，太慢了

**解决**：加条件判断

```
[ ${lines_changed} -gt 50 ] && gemini -p "请审查：$(cat ${file})"
```

只有改动超过50行才触发

### 坑6：Gemini CLI在IDE环境启动慢

**问题**：Gemini CLI启动后卡在"Connecting to Gemini..."很久

**解决**：设置环境变量禁用IDE集成

```
export GEMINI_IDE_INTEGRATION=false
```

或在 `mcp-config.json` 中配置：

```
{
  "gemini": {
    "environment": {
      "GEMINI_IDE_INTEGRATION": "false"
    }
  }
}
```

### 坑7：Codex sandbox权限不足

**问题**：Codex默认是read-only模式，无法写入文件

**解决**：添加 `--sandbox workspace-write` 参数

```
codex exec --sandbox workspace-write "你的提示词"
```

本项目的MCP Server已自动配置此参数。

### 坑8：Codex CLI认证过期

**问题**：提示 Your access token could not be refreshed because your refresh token was already used

**解决**：重新登录

```
codex auth logout
codex auth login
```

### 坑9：Windows下HOME目录问题

**问题**：MCP Server在Windows下找不到配置文件

**解决**：强制使用`USERPROFILE`作为`HOME`

```
if (process.platform === 'win32' && process.env.USERPROFILE) {
  process.env.HOME = process.env.USERPROFILE;
}
```

本项目的MCP Server已自动处理此问题。

---

## 总结：从工具到思维方式

配完这套方案，老金我最大的感受不是"效率提升40%"这么简单。

真正的改变在于：我对AI协作的理解从"工具"层面上升到了"思维方式"层面。

**以前的思维**：AI是工具、我是操作者；我手动协调多个AI，像个项目经理；效率瓶颈在"人工切换"。

**现在的思维**：**AI可以编排AI**、我是最终决策者；Claude Code是指挥官、其他AI是执行者；效率提升在"信息打通"。

**更深层的启发**：

这不只是技术方案，而是一种**协作模式的演进**：

1. **单兵作战** → 一个AI干所有事（效率低）
2. **人工协调** → 人当中间人协调多个AI（累）
3. **AI编排AI** → 一个AI指挥其他AI（现在）
4. **未来？** → 可能是AI团队自主协作，人只管目标

**老金我突然想明白：**

这不就是从"单核CPU"到"多核CPU"再到"分布式计算"的演进吗？

**单核是一个AI干所有事；**

**多核是人手动切换多个AI；**

**分布式是AI自动调度AI。**

**而Claude Code的5种编排方式，就像提供了5种不同的"任务调度算法"：**

`Command`是简单的串行调度；

`Skill`是可配置的流程编排；

`MCP`是带状态的分布式通信；

`Subagent`是真正的并行计算；

`Hooks`是事件驱动的响应式调度。

**这让我想到了一个更大的问题：**

> 当AI可以编排AI的时候，人的价值在哪里？

老金我的答案是：人的价值在于"定义目标"和"最终决策"。

就像这套方案，老金我只需要：

1. 定义目标："我要开发一个用户认证模块"
2. 选择方式："用Skill方式，因为需要保存中间结果"
3. 最终决策："Gemini说有安全问题，改完再提交"

中间的"分析需求、生成代码、审查质量"这些执行层面的事，交给AI团队自己配合完成。

这就是未来的工作方式：人定义"What"，AI团队负责"How"。

周末两天测下来，开发效率确实翻倍了。但更重要的是，我从"做事"中解放出来，有更多时间"想事"。

想架构、想方案、想怎么做得更好。

这才是AI协作的终极意义：让人回归到人最擅长的事——思考和决策。

### 配置文件参考

+ Command的配置在 `.claude/commands/*.md`
+ Skill的配置在 `.claude/skills/*/SKILL.md`（新格式）或 `skill.yaml`（旧格式兼容）
+ MCP Server配置在 `.mcp.json`（Claude Code标准）
+ 统一配置（代理、权限等）在 `mcp-config.json`
+ Hooks的配置在 `.claude/settings.json`
+ MCP Server实现在 `mcp-servers/` 目录

### 项目文档

+ 详细安装指南：`docs/installation.md`
+ 使用说明：`docs/usage.md`
+ 5种方式对比：`docs/comparison.md`
+ 踩坑记录：`docs/troubleshooting.md`
+ Claude Code规范：`docs/claude-code-specs.md`
+ 兼容性检查报告：`docs/compatibility-report.md`
+ 测试报告：`docs/test-report.md`

### 参考资料

1. Claude Code官方文档：https://code.claude.com/docs
2. MCP协议规范：https://modelcontextprotocol.io/
3. Gemini API文档：https://ai.google.dev/docs

### Sources

1. Top 10 Essential MCP Servers for Claude Code (2025 Developer Version)：https://apidog.com/blog/top-10-mcp-servers-for-claude-code/
2. Connect Claude Code to tools via MCP：https://code.claude.com/docs/en/mcp
3. GitHub - modelcontextprotocol/servers：https://github.com/modelcontextprotocol/servers

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
