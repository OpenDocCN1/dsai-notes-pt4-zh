# Hooks系统完整指南：自动化工作流的终极武器

> 来源：[https://my.feishu.cn/docx/Cf5jdJNl9o9LP3x1HTvcT2GQnUb](https://my.feishu.cn/docx/Cf5jdJNl9o9LP3x1HTvcT2GQnUb)

# Hooks系统完整指南：自动化工作流的终极武器

> **课程信息**

+   **作者**：老金

+   **预计学时**：4-6小时

+   **难度等级**：⭐⭐ 入门级（有Claude Code基础即可）

+   **更新日期**：2026年3月

+   **适用版本**：Claude Code v2.1+（验证于2026-03-18）

+   **前置要求**：已完成Claude Code安装和基础使用

---

## 本课学习目标

完成本课学习后，你将能够：

1.  **理解Hooks的核心价值**：掌握Hooks与传统提示词的本质区别

1.  **配置第一个Hook**：5分钟内完成最简单的Hook配置并看到效果

1.  **掌握20种Hook类型**：PreToolUse、PostToolUse、UserPromptSubmit、SubagentStart、SubagentStop、StopFailure、PostCompact、InstructionsLoaded、Elicitation等全部类型

1.  **实现自动化工作流**：Git提交检查、代码格式化、文件保护等实战场景

1.  **排查Hook故障**：独立解决90%的常见配置和执行问题

1.  **安全使用Hooks**：理解安全风险并正确配置权限

---

## 学习路径导航（先看这里！）

> **根据你的情况选择学习路径**：这是一篇3000+行的长教程，不用全看！根据你的目标选择路径。

### 路径A：快速上手（30分钟）

**适合人群**：急着体验Hooks，想快速配一个看效果

**只看这些章节**（其他跳过）：

```
✅ 术语表（5分钟） - 快速了解Hook核心概念
✅ 第一部分1.1-1.2：Hooks简介（5分钟） - 理解Hook是什么
✅ 第二部分：5分钟快速开始（15分钟） - 配置第一个Hook
✅ 第三部分3.1：PreToolUse基础（5分钟） - 最常用的Hook类型
```

**30分钟后你能达到**：成功配置第一个Hook，Claude Code能自动执行你的脚本

---

### 路径B：完整学习（4-6小时）

**适合人群**：想深入理解Hooks，掌握所有类型和高级用法

**学习顺序**：从头到尾所有章节

**建议分段学习**：

+   第1天（2小时）：第1-3部分（理解+15种类型）

+   第2天（2小时）：第4-5部分（实战场景+故障排查）

+   第3天（1小时）：第6-7部分（FAQ+附录）

---

### 路径C：问题排查（10分钟）

**适合人群**：Hook配置出问题，需要快速解决

**直接跳到这些章节**：

```
🔧 第五部分：故障排查 - 按错误类型查找解决方案
🔧 第六部分：FAQ - 20个常见问题解答
```

**使用方法**：

1.  按 Ctrl + F 搜索你的错误信息关键词

1.  找到对应的Q&A

1.  按步骤解决

---

### 路径D：专项学习（30-60分钟/主题）

**适合人群**：已经会配置Hook，想学习特定功能

| 想学什么 | 看哪几节 | 预计时间 |
| --- | --- | --- |
| **Git自动化** | 第四部分4.1节 | 45分钟 |
| **代码格式化** | 第四部分4.2节 | 30分钟 |
| **文件保护** | 第三部分3.1节 | 20分钟 |
| **提示词优化** | 第三部分3.3节 | 30分钟 |
| **安全最佳实践** | 第一部分1.4节 + 第五部分 | 40分钟 |

---

## 术语表（小白必读）

**在开始之前，先了解**这些关键术语。用生活类比帮助理解：

| 术语 | 英文全称 | 通俗解释 | 生活类比 |
| --- | --- | --- | --- |
| **Hook** | - | 在特定事件发生时自动执行的脚本 | 汽车传感器（检测到碰撞自动弹安全气囊） |
| **PreToolUse** | Pre Tool Use | 工具调用**前**触发的Hook | 机场安检门（登机前检查） |
| **PostToolUse** | Post Tool Use | 工具调用**后**触发的Hook | 快递签收后的自动通知 |
| UserPromptSubmit | User Prompt Submit | 用户输入提交时触发的Hook | 邮件发送前的拼写检查 |
| **Notification** | - | 通知事件触发的Hook | 手机APP推送通知 |
| **SessionStart** | Session Start | 会话开始时触发的Hook | 开机自动启动程序 |
| **SessionEnd** | Session End | 会话结束时触发的Hook | 关机前自动保存 |
| **Stop** | - | AI停止响应时触发的Hook | 紧急刹车后的状态保存 |
| **WorktreeCreate** 🆕 | Worktree Create | 工作树**创建**时触发的Hook | 新开一个平行工作台时的初始化 |
| **WorktreeRemove** 🆕 | Worktree Remove | 工作树**删除**时触发的Hook | 关闭平行工作台时的清理 |
| **Matcher** | - | 匹配规则，决定Hook对哪些工具生效 | 筛选器（只检查特定行李） |
| **Decision** | - | PreToolUse Hook的返回决策 | 安检结果（放行/拦截/询问） |
| **stdin** | Standard Input | 标准输入，Hook接收数据的方式 | 传送带送入检查口 |
| **stdout** | Standard Output | 标准输出，Hook返回结果的方式 | 检查结果显示屏 |
| **stderr** | Standard Error | 标准错误输出，仅用于调试日志（不会显示在Claude Code界面） | 后台监控日志（用户看不到） |
| **timeout** | - | 超时时间，Hook最长运行时间 | 限时检查（超时自动放行） |
| **JSON** | JavaScript Object Notation | 一种通用的数据格式，用花括号{}组织数据，settings.json配置文件就是JSON格式 | 标准化的表格模板 |
| **~****（波浪号）** | Home Directory | 用户的&quot;家目录&quot;，macOS是/Users/用户名，Linux是/home/用户名，Windows对应C:\Users\用户名 | 你电脑上&quot;我的文档&quot;的上级目录 |

---

## 第一部分：Hooks简介（5分钟理解）

### 1.1 Hooks是什么

> **一句话理解**：Hooks是Claude Code的"自动化传感器"，在特定事件发生时自动执行你的脚本，实现100%可靠的自动化。

#### 为什么需要Hooks？

没有Hooks之前（靠AI"记住"）：

```
问题：AI有时会&quot;忘记&quot;你的要求

你：每次写代码后帮我运行格式化
Claude：好的！（这次记住了）

...10分钟后...

Claude：代码写好了！
你：等等，你忘了格式化！
Claude：抱歉，我忘了...
```

有了Hooks之后（100%自动执行）：

```
解决方案：不依赖AI记忆，配置Hook后自动执行

配置PostToolUse Hook → 监听Write工具 → 自动运行格式化脚本

Claude：代码写好了！
[Hook自动触发：运行 prettier --write xxx.js]
结果：代码已自动格式化，100%不会忘记
```

> **生活类比**：

+   **没有Hooks**：靠人记住每次开车前检查轮胎（经常忘）

+   **有Hooks后**：汽车传感器自动检测胎压，异常自动报警（100%可靠）

#### Hooks的核心价值

| 对比维度 | 提示词方式 | Hooks方式 |
| --- | --- | --- |
| **可靠性** | 不确定（AI可能忘记） | 100%执行（确定性） |
| **一致性** | 每次可能不同 | 每次完全相同 |
| **自动化** | 需要AI主动执行 | 事件触发自动执行 |
| **团队协作** | 每人都要提醒AI | 配置一次，全员生效 |
| **适用场景** | 灵活建议 | 强制规则 |

### 1.2 Hooks能做什么（6个实际案例）

案例1：文件保护（PreToolUse）

```
场景：禁止Claude修改production目录下的文件

Hook触发：Claude尝试Write(file_path=&quot;production/config.js&quot;)
Hook检查：路径包含&quot;production/&quot;
Hook决策：deny（拒绝）
结果：Claude收到错误提示，文件未被修改
```

案例2：代码格式化（PostToolUse）

```
场景：每次保存代码后自动格式化

Hook触发：Claude成功执行Write(file_path=&quot;src/app.js&quot;)
Hook执行：运行 prettier --write src/app.js
结果：代码自动格式化，无需手动操作
```

案例3：提示词优化（UserPromptSubmit）

```
场景：自动在写作任务后追加写作规范

用户输入：&quot;帮我写一篇关于AI的文章&quot;
Hook检测：包含&quot;写&quot;和&quot;文章&quot;关键词
Hook追加：&quot;\n\n## 写作规范\n1. 风格：接地气\n2. 字数：1500字&quot;
Claude收到：原始输入 + 写作规范
```

案例4：Git提交检查（PreToolUse + Bash）

```
场景：提交前自动检查代码质量

Hook触发：Claude执行Bash(command=&quot;git commit -m xxx&quot;)
Hook执行：运行lint检查、测试、敏感信息扫描
Hook决策：全部通过 → allow；有问题 → deny
结果：低质量代码无法提交
```

案例5：会话初始化（SessionStart）

```
场景：启动Claude Code时自动加载项目配置

Hook触发：Claude Code启动
Hook执行：检查Python依赖是否安装
结果：缺少依赖时自动提示安装命令
```

案例6：桌面通知（Notification）

```
场景：Claude需要用户确认时发送桌面通知

Hook触发：Claude发送通知请求用户确认
Hook执行：调用系统通知API
结果：用户收到桌面弹窗，不会错过重要确认
```

### 1.3 Hooks执行流程

**完整生命周期图**：

```
用户输入
    ↓
[UserPromptSubmit Hook] ← 可以修改/增强提示词
    ↓
Claude处理提示词
    ↓
决定调用工具（如Write）
    ↓
[PreToolUse Hook] ← 可以允许/拒绝/询问
    ↓
执行工具（如Write）
    ↓
[PostToolUse Hook] ← 可以执行后处理
    ↓
返回结果给用户
```

**20种Hook类型触发时机**：

| Hook类型 | 触发时机 | 典型用途 | 可否阻止后续操作 |
| --- | --- | --- | --- |
| UserPromptSubmit | 用户输入提交后 | 提示词优化、敏感词过滤 | ✅ 是 |
| **PreToolUse** | 工具调用前 | 权限校验、参数验证 | ✅ 是 |
| **PostToolUse** | 工具调用后 | 格式修复、自动测试 | ❌ 否 |
| **Notification** | 通知发送时 | 日志记录、桌面通知 | ❌ 否 |
| **SessionStart** | 会话开始时 | 环境初始化 | ❌ 否 |
| **SessionEnd** | 会话结束时 | 清理临时文件 | ❌ 否 |
| **Stop** | AI停止响应时 | 保存状态 | ❌ 否 |
| **SubagentStart** 🆕 | 子代理启动时 | 子代理环境初始化、日志记录 | ❌ 否 |
| **SubagentStop** 🆕 | 子代理停止时 | 子代理结果收集、资源清理 | ❌ 否 |
| PermissionRequest 🆕 | 权限请求时 | 自动审批策略、权限审计 | ✅ 是 |
| **PreCompact** 🆕 | 上下文压缩前 | 保存关键上下文、日志记录 | ❌ 否 |
| **ConfigChange** 🆕 | 配置变更时 | 配置审计、自动同步 | ❌ 否 |
| **TeammateIdle** 🆕 | 队友空闲时 | 任务分配、状态通知 | ❌ 否 |
| **WorktreeCreate** | 工作树创建时 | 初始化工作树配置 | ❌ 否 |
| **WorktreeRemove** | 工作树删除时 | 清理工作树资源 | ❌ 否 |
| **StopFailure** 🆕 | API异常停止时 | 错误告警、日志记录 | ❌ 否 |
| **PostCompact** 🆕 | 上下文压缩后 | 记录token变化 | ❌ 否 |
| InstructionsLoaded 🆕 | 指令文件加载时 | 验证指令完整性 | ❌ 否 |
| **Elicitation** 🆕 | MCP请求输入时 | 记录交互日志 | ❌ 否 |
| ElicitationResult 🆕 | MCP输入完成时 | 验证输入数据 | ❌ 否 |

### 1.4 安全警告（重要！）

> ⚠️ **严重警告**：Hooks可以执行**任意Shell命令**，这意味着配置不当可能导致：

+   文件被删除或修改

+   敏感信息泄露

+   系统被恶意脚本攻击

**安全最佳实践**：

| 风险 | 防护措施 |
| --- | --- |
| **恶意脚本** | 只运行你信任的脚本，不要从不明来源复制配置 |
| **权限过大** | 脚本只请求必要的权限，避免使用sudo |
| **敏感信息** | 不要在脚本中硬编码密码/Token |
| **无限循环** | 设置合理的timeout，避免脚本卡死 |
| **团队配置** | 代码审查.claude/settings.json变更 |

**配置检查清单**：

```
□ 脚本来源可信吗？（自己写的/官方示例/信任的开源）
□ 脚本权限最小化了吗？（不需要sudo就不用）
□ 敏感信息用环境变量了吗？（不硬编码）
□ 设置了合理的timeout吗？（防止卡死）
□ 团队成员都知道这个Hook吗？（透明度）
```

---

## 第二部分：5分钟快速开始（立即见效）

> **本节目的**：用最快速度配置第一个Hook，让你立即看到效果！

> ⏱️ **预计时间**：5-10分钟

### 2.1 配置第一个Hook（最简单版本）

**为什么选这个示例？**

+   ✅ 最简单，只需要3个文件

+   ✅ 效果直观，立即看到输出

+   ✅ 无依赖，不需要安装任何东西

#### 步骤1：创建Hook脚本目录

**这一步要做什么**：在项目根目录创建 .claude/hooks/ 目录

Windows系统（PowerShell）：

```
# 进入你的项目目录
cd C:\你的项目路径

# 创建hooks目录
New-Item -ItemType Directory -Path &quot;.claude\hooks&quot; -Force
```

**macOS/Linux系统：**

```
# 进入你的项目目录
cd ~/你的项目路径

# 创建hooks目录
mkdir -p .claude/hooks
```

**验证是否成功：**

```
# 检查目录是否存在
ls .claude/hooks
# 应该显示空目录（暂时没有文件）
```

#### 步骤2：创建最简单的Hook脚本

**这一步要做什么**：创建一个Python脚本，在每次Write工具执行后打印提示

**创建文件 **.claude/hooks/post-write-hello.py：

> 💡 你**有两种选择：**

> 选择A：在Claude Code对话框里说人话（推荐新手）

```
帮我创建.claude/hooks/post-write-hello.py文件，内容是一个PostToolUse Hook脚本，在Write工具执行后打印提示信息
```

> （换行用 Shift + Enter，最后按 Enter 发送）

> Claude Code会自动帮你创建正确格式的文件！

> 选择B：在终端里用命令行（熟悉命令行的用户）

> 见下方PowerShell/Bash代码

Windows（PowerShell）：

```
@&#x27;
#!/usr/bin/env python3
&quot;&quot;&quot;
最简单的PostToolUse Hook示例
每次Write工具执行后记录日志
&quot;&quot;&quot;
import sys
import json
from pathlib import Path
from datetime import datetime

# 从stdin读取工具执行信息
try:
    input_data = json.loads(sys.stdin.read())
except:
    sys.exit(0)

# 获取工具名称和文件路径
tool_name = input_data.get(&#x27;tool_name&#x27;, &#x27;&#x27;)
tool_input = input_data.get(&#x27;tool_input&#x27;, {})
file_path = tool_input.get(&#x27;file_path&#x27;, &#x27;&#x27;)

# 只处理Write工具
if tool_name == &#x27;Write&#x27;:
    # PostToolUse Hook执行后处理任务
    # 注意：PostToolUse Hook无法向用户输出信息
    # Claude Code只会显示&quot;Hook执行成功&quot;
    # 如果需要调试，可以写入日志文件
    log_file = Path.home() / &#x27;.claude&#x27; / &#x27;hooks&#x27; / &#x27;post-write.log&#x27;
    log_file.parent.mkdir(parents=True, exist_ok=True)
    with open(log_file, &#x27;a&#x27;, encoding=&#x27;utf-8&#x27;) as f:
        timestamp = datetime.now().strftime(&#x27;%Y-%m-%d %H:%M:%S&#x27;)
        f.write(f&quot;[{timestamp}] ✅ 文件已保存: {file_path}\n&quot;)

sys.exit(0)
&#x27;@ | Out-File -FilePath &quot;.claude\hooks\post-write-hello.py&quot; -Encoding utf8
```

**macOS/Linux：**

```
cat &gt; .claude/hooks/post-write-hello.py &lt;&lt; &#x27;EOF&#x27;
#!/usr/bin/env python3
&quot;&quot;&quot;
最简单的PostToolUse Hook示例
每次Write工具执行后记录日志
&quot;&quot;&quot;
import sys
import json
from pathlib import Path
from datetime import datetime

# 从stdin读取工具执行信息
try:
    input_data = json.loads(sys.stdin.read())
except:
    sys.exit(0)

# 获取工具名称和文件路径
tool_name = input_data.get(&#x27;tool_name&#x27;, &#x27;&#x27;)
tool_input = input_data.get(&#x27;tool_input&#x27;, {})
file_path = tool_input.get(&#x27;file_path&#x27;, &#x27;&#x27;)

# 只处理Write工具
if tool_name == &#x27;Write&#x27;:
    # PostToolUse Hook执行后处理任务
    # 注意：PostToolUse Hook无法向用户输出信息
    # Claude Code只会显示&quot;Hook执行成功&quot;
    # 如果需要调试，可以写入日志文件
    log_file = Path.home() / &#x27;.claude&#x27; / &#x27;hooks&#x27; / &#x27;post-write.log&#x27;
    log_file.parent.mkdir(parents=True, exist_ok=True)
    with open(log_file, &#x27;a&#x27;, encoding=&#x27;utf-8&#x27;) as f:
        timestamp = datetime.now().strftime(&#x27;%Y-%m-%d %H:%M:%S&#x27;)
        f.write(f&quot;[{timestamp}] ✅ 文件已保存: {file_path}\n&quot;)

sys.exit(0)

sys.exit(0)
EOF

# 添加执行权限（macOS/Linux必须）
chmod +x .claude/hooks/post-write-hello.py
```

**验证脚本创建成功：**

```
# 查看文件内容
cat .claude/hooks/post-write-hello.py
# 应该显示你刚才写入的Python代码
```

#### 步骤3：配置settings.json

**这一步要做什么**：告诉Claude Code在PostToolUse时运行你的脚本

**创建或编辑 **.claude/settings.json：

> 💡 你**有两种选择：**

> 选择A：在Claude Code对话框里说人话（推荐新手）

```
帮我创建.claude/settings.json配置文件，配置PostToolUse Hook监听Write工具并运行post-write-hello.py脚本
```

> （换行用 Shift + Enter，最后按 Enter 发送）

> Claude Code会自动帮你创建正确格式的配置文件！

> 选择B：在终端里用命令行（熟悉命令行的用户）

> 见下方PowerShell/Bash代码

Windows（PowerShell）：

```
@&#x27;
{
  &quot;hooks&quot;: {
    &quot;PostToolUse&quot;: [
      {
        &quot;matcher&quot;: &quot;Write&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/post-write-hello.py&quot;,
            &quot;timeout&quot;: 10
          }
        ]
      }
    ]
  }
}
&#x27;@ | Out-File -FilePath &quot;.claude\settings.json&quot; -Encoding utf8
```

**macOS/Linux：**

```
cat &gt; .claude/settings.json &lt;&lt; &#x27;EOF&#x27;
{
  &quot;hooks&quot;: {
    &quot;PostToolUse&quot;: [
      {
        &quot;matcher&quot;: &quot;Write&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/post-write-hello.py&quot;,
            &quot;timeout&quot;: 10
          }
        ]
      }
    ]
  }
}
EOF
```

> 💡 配**置说明：**

+   "PostToolUse"：Hook类型，工具执行后触发

+   "matcher": "Write"：只匹配Write工具

+   "command"：要执行的脚本命令

+   "timeout": 10：超时10秒

#### 步骤4：启动Claude Code并测试

**这一步要做什么**：重启Claude Code，让它读取新配置

```
# 在项目目录启动Claude Code
claude
```

**测试命令**：

```
你：帮我创建一个test.txt文件，内容是&quot;Hello Hooks!&quot;
```

**预期结果**：

```
Claude：我来帮你创建test.txt文件。

[Write工具执行]

✅ Hook执行成功

文件已创建成功！
```

> 💡 重**要说明：**

+   PostToolUse Hook执行后，Claude Code只会显示"Hook执行成功"

+   Hook的日志已写入~/.claude/hooks/post-write.log文件

+   你可以查看日志文件确认Hook确实执行了：

> cat ~/.claude/hooks/post-write.log

# 应该看到类似：[2026-03-05 16:30:00] ✅ 文件已保存: /path/to/test.txt

> ✅ **关键确认**：看到 Hook触发成功！ 说明Hook配置正确并成功执行！

### 2.2 验证Hook工作正常

**完整验证清单**：

**如果没有看到Hook输出**：

1.  **检查JSON格式**：

```
# 验证JSON格式是否正确
python -c &quot;import json; json.load(open(&#x27;.claude/settings.json&#x27;))&quot;
# 如果没报错说明格式正确
```

1.  **检查Python是否可用**：

```
python --version
# 应该显示Python 3.x
```

1.  **手动测试脚本**：

```
echo &#x27;{&quot;tool_name&quot;: &quot;Write&quot;, &quot;tool_input&quot;: {&quot;file_path&quot;: &quot;test.txt&quot;}}&#x27; | python .claude/hooks/post-write-hello.py
# 应该显示Hook触发成功的消息
```

### 2.3 恭喜完成第一个Hook！

**你刚才完成了什么？**

1.  ✅ 创建了Hook脚本目录

1.  ✅ 编写了第一个Hook脚本

1.  ✅ 配置了settings.json

1.  ✅ 验证了Hook正常工作

**接下来可以**：

+   继续学习15种Hook类型（第三部分）

+   学习实战应用场景（第四部分）

+   遇到问题查看故障排查（第五部分）

---

## 第三部分：15种Hook类型详解

> **本节目的**：掌握所有Hook类型的用法

> ⏱️ **预计时间**：1.5-2小时

### 3.1 PreToolUse（工具调用前）

> **一句话理解**：PreToolUse就像"安检门"，在工具执行前检查是否允许通过。

#### 触发时机

**在Clau**de准备调用工具（如Write、Edit、Bash）时，但尚未执行。

#### 输入参数（通过stdin的JSON）

```
{
  &quot;tool_name&quot;: &quot;Write&quot;,
  &quot;tool_input&quot;: {
    &quot;file_path&quot;: &quot;C:/project/src/app.js&quot;,
    &quot;content&quot;: &quot;console.log(&#x27;Hello World&#x27;);&quot;
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| tool_name | string | 工具名称（Write, Edit, Bash, Read等） |
| tool_input | object | 工具的输入参数 |

#### 决策输出（通过stdout的JSON）

**PreT**oolUse Hook可以返回决策指令控制工具是否执行。

> **新旧API并存**：v2.1.49+ 推荐使用新版 hookSpecificOutput.permissionDecision 字段，旧版 decision 字段仍然支持但已废弃。

**新版API（推荐）** — 通过 hookSpecificOutput.permissionDecision 字段：

```
{
  &quot;hookSpecificOutput&quot;: {
    &quot;permissionDecision&quot;: &quot;deny&quot;
  },
  &quot;message&quot;: &quot;禁止修改production目录下的文件&quot;
}
```

**旧版API（已废弃但仍支持）** — 通过 decision 字段：

```
{
  &quot;decision&quot;: &quot;deny&quot;,
  &quot;message&quot;: &quot;禁止修改production目录下的文件&quot;
}
```

**新版决策值**（hookSpecificOutput.permissionDecision）：

| 决策值 | 说明 | 工具是否执行 |
| --- | --- | --- |
| &quot;allow&quot; | 允许执行，绕过权限系统 | ✅ 是 |
| &quot;deny&quot; | 拒绝执行，原因会反馈给Claude | ❌ 否 |
| &quot;ask&quot; | 暂停，询问用户决定 | 🤔 等待用户决定 |
| 无输出 | 默认允许 | ✅ 是 |

**旧版决策值**（decision，已废弃）：

| 旧版值 | 等同于新版 | 说明 |
| --- | --- | --- |
| &quot;approve&quot; | &quot;allow&quot; | 允许执行 |
| &quot;block&quot; | &quot;deny&quot; | 拒绝执行 |

> PostToolUse 和 UserPromptSubmit 的决策值：这两个Hook类型使用 "block" 来阻止/提供反馈，无输出则默认允许。

#### 完整示例1：文件保护Hook

**场景**：禁止修改production/目录下的文件

**脚本 ****.claude/hooks/pre-p**rotect-production.py：

```
#!/usr/bin/env python3
&quot;&quot;&quot;
PreToolUse Hook - 保护production目录
禁止Write/Edit工具修改production目录下的文件
&quot;&quot;&quot;
import sys
import json

# 读取stdin的JSON输入
try:
    input_data = json.loads(sys.stdin.read())
except json.JSONDecodeError:
    sys.exit(0)

tool_name = input_data.get(&#x27;tool_name&#x27;, &#x27;&#x27;)
tool_input = input_data.get(&#x27;tool_input&#x27;, {})
file_path = tool_input.get(&#x27;file_path&#x27;, &#x27;&#x27;)

# 只检查Write和Edit工具
if tool_name not in [&#x27;Write&#x27;, &#x27;Edit&#x27;]:
    sys.exit(0)

# 规范化路径（处理Windows和Unix路径）
file_path_normalized = file_path.replace(&#x27;\\&#x27;, &#x27;/&#x27;)

# 检查是否是保护目录
protected_dirs = [&#x27;production/&#x27;, &#x27;prod/&#x27;, &#x27;.env&#x27;]
for protected in protected_dirs:
    if protected in file_path_normalized:
        # 拒绝执行
        decision = {
            &quot;decision&quot;: &quot;deny&quot;,
            &quot;message&quot;: f&quot;❌ 禁止修改受保护的路径！\n路径: {file_path}\n原因: 包含受保护目录 &#x27;{protected}&#x27;\n\n请先切换到dev环境或手动操作。&quot;
        }
        print(json.dumps(decision, ensure_ascii=False))
        sys.exit(0)

# 允许执行（无输出=默认allow）
sys.exit(0)
```

**配置 **.claude/settings.json：

```
{
  &quot;hooks&quot;: {
    &quot;PreToolUse&quot;: [
      {
        &quot;matcher&quot;: &quot;Write|Edit&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/pre-protect-production.py&quot;,
            &quot;timeout&quot;: 5
          }
        ]
      }
    ]
  }
}
```

**运行效果**：

当Claude尝试修改production/config.json时：

```
Claude：我来修改production/config.json...

❌ 禁止修改受保护的路径！
路径: C:/project/production/config.json
原因: 包含受保护目录 &#x27;production/&#x27;

请先切换到dev环境或手动操作。
```

#### 完整示例2：危险命令拦截Hook

**场景**：拦截危险的Bash命令（如rm -rf）

**脚本 ****.claude/hooks/pre-bl**ock-dangerous-cmd.py：

```
#!/usr/bin/env python3
&quot;&quot;&quot;
PreToolUse Hook - 拦截危险命令
阻止执行可能造成破坏的Shell命令
&quot;&quot;&quot;
import sys
import json
import re

# 危险命令模式
DANGEROUS_PATTERNS = [
    r&#x27;rm\s+-rf\s+/&#x27;,           # rm -rf /
    r&#x27;rm\s+-rf\s+~&#x27;,           # rm -rf ~
    r&#x27;rm\s+-rf\s+\*&#x27;,          # rm -rf *
    r&#x27;rm\s+-rf\s+\.\.&#x27;,        # rm -rf ..
    r&#x27;:()\s*{\s*:\|:&amp;\s*};:&#x27;,  # Fork炸弹
    r&#x27;mkfs\.&#x27;,                  # 格式化磁盘
    r&#x27;dd\s+if=.+of=/dev/&#x27;,     # 覆盖磁盘
    r&#x27;&gt;\s*/dev/sda&#x27;,           # 覆盖磁盘
    r&#x27;chmod\s+-R\s+777\s+/&#x27;,   # 危险权限
]

# 读取输入
try:
    input_data = json.loads(sys.stdin.read())
except json.JSONDecodeError:
    sys.exit(0)

tool_name = input_data.get(&#x27;tool_name&#x27;, &#x27;&#x27;)
tool_input = input_data.get(&#x27;tool_input&#x27;, {})
command = tool_input.get(&#x27;command&#x27;, &#x27;&#x27;)

# 只检查Bash工具
if tool_name != &#x27;Bash&#x27;:
    sys.exit(0)

# 检查危险模式
for pattern in DANGEROUS_PATTERNS:
    if re.search(pattern, command, re.IGNORECASE):
        decision = {
            &quot;decision&quot;: &quot;deny&quot;,
            &quot;message&quot;: f&quot;🚨 危险命令已拦截！\n\n命令: {command}\n\n匹配的危险模式: {pattern}\n\n如果确实需要执行，请在终端手动运行。&quot;
        }
        print(json.dumps(decision, ensure_ascii=False))
        sys.exit(0)

sys.exit(0)
```

**配置**：

```
{
  &quot;hooks&quot;: {
    &quot;PreToolUse&quot;: [
      {
        &quot;matcher&quot;: &quot;Bash&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/pre-block-dangerous-cmd.py&quot;,
            &quot;timeout&quot;: 5
          }
        ]
      }
    ]
  }
}
```

### 3.2 PostToolUse（工具调用后）

> **一句话理解**：PostToolUse就像"快递签收通知"，在工具成功执行后自动触发后续操作。

#### 触发时机

在工具**成功执行后**立即触发，可以处理工具的输出结果。

#### 输入参数（通过stdin的JSON）

```
{
  &quot;tool_name&quot;: &quot;Write&quot;,
  &quot;tool_input&quot;: {
    &quot;file_path&quot;: &quot;C:/project/src/app.js&quot;,
    &quot;content&quot;: &quot;console.log(&#x27;Hello&#x27;);&quot;
  },
  &quot;tool_output&quot;: {
    &quot;success&quot;: true,
    &quot;message&quot;: &quot;File written successfully&quot;
  }
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| tool_name | string | 工具名称 |
| tool_input | object | 工具的输入参数 |
| tool_output | object | 工具的输出结果 |

#### 输出格式

> ⚠️ 重要：PostToolUse Hook的输出机制

> **PostToolU**se Hook无法向用户输出信息！

+   ✅ **可以做**：执行后处理任务（格式化、备份、测试、写日志文件）

+   ❌ **不能做**：向用户显示信息（Claude Code只会显示"Hook执行成功/失败"）

+   ❌ **常见误区**：print(..., file=sys.stderr)不会显示在界面，只在终端可见

> 如果需要向用户输出信息，请使用UserPromptSubmit Hook的additionalContext机制。

**PostT**oolUse Hook不返回决策（工具已经执行完了），只能：

+   执行后处理任务（格式化、备份、测试）

+   写入日志文件（用于调试）

#### 完整示例1：自动代码格式化

**场景**：在Write工具保存.js/.ts文件后，自动运行Prettier格式化

**脚本 **.claude/hooks/post-auto-format.py：

```
#!/usr/bin/env python3
&quot;&quot;&quot;
PostToolUse Hook - 自动代码格式化
保存代码文件后自动运行对应的格式化工具
&quot;&quot;&quot;
import sys
import json
import subprocess
from pathlib import Path

# 格式化工具配置
FORMATTERS = {
    &#x27;.js&#x27;: &#x27;npx prettier --write &quot;{file}&quot;&#x27;,
    &#x27;.ts&#x27;: &#x27;npx prettier --write &quot;{file}&quot;&#x27;,
    &#x27;.jsx&#x27;: &#x27;npx prettier --write &quot;{file}&quot;&#x27;,
    &#x27;.tsx&#x27;: &#x27;npx prettier --write &quot;{file}&quot;&#x27;,
    &#x27;.json&#x27;: &#x27;npx prettier --write &quot;{file}&quot;&#x27;,
    &#x27;.css&#x27;: &#x27;npx prettier --write &quot;{file}&quot;&#x27;,
    &#x27;.py&#x27;: &#x27;black &quot;{file}&quot;&#x27;,
    &#x27;.go&#x27;: &#x27;gofmt -w &quot;{file}&quot;&#x27;,
}

# 排除的目录
EXCLUDED_DIRS = {&#x27;node_modules&#x27;, &#x27;venv&#x27;, &#x27;.venv&#x27;, &#x27;__pycache__&#x27;, &#x27;dist&#x27;, &#x27;build&#x27;, &#x27;.git&#x27;}

def should_format(file_path: str) -&gt; bool:
    &quot;&quot;&quot;检查是否应该格式化该文件&quot;&quot;&quot;
    path = Path(file_path)

    # 检查是否在排除目录中
    for part in path.parts:
        if part in EXCLUDED_DIRS:
            return False

    # 检查文件扩展名
    return path.suffix in FORMATTERS

def run_formatter(file_path: str) -&gt; str:
    &quot;&quot;&quot;运行格式化工具&quot;&quot;&quot;
    path = Path(file_path)
    suffix = path.suffix

    if suffix not in FORMATTERS:
        return None

    cmd = FORMATTERS[suffix].format(file=file_path)

    try:
        result = subprocess.run(
            cmd,
            shell=True,
            capture_output=True,
            text=True,
            timeout=30
        )
        if result.returncode == 0:
            return f&quot;✅ 格式化成功&quot;
        else:
            return f&quot;⚠️ 格式化失败: {result.stderr[:100]}&quot;
    except subprocess.TimeoutExpired:
        return &quot;⚠️ 格式化超时&quot;
    except FileNotFoundError:
        return &quot;⚠️ 格式化工具未安装&quot;
    except Exception as e:
        return f&quot;⚠️ 格式化错误: {str(e)}&quot;

def main():
    # 读取输入
    try:
        input_data = json.loads(sys.stdin.read())
    except json.JSONDecodeError:
        return

    tool_name = input_data.get(&#x27;tool_name&#x27;, &#x27;&#x27;)
    tool_input = input_data.get(&#x27;tool_input&#x27;, {})
    file_path = tool_input.get(&#x27;file_path&#x27;, &#x27;&#x27;)

    # 只处理Write工具
    if tool_name != &#x27;Write&#x27;:
        return

    # 检查是否需要格式化
    if not file_path or not should_format(file_path):
        return

    # 运行格式化
    result = run_formatter(file_path)
    if result:
        print(f&quot;\n[AutoFormat] {Path(file_path).name}: {result}&quot;, file=sys.stderr)

if __name__ == &#x27;__main__&#x27;:
    main()
```

**配置**：

```
{
  &quot;hooks&quot;: {
    &quot;PostToolUse&quot;: [
      {
        &quot;matcher&quot;: &quot;Write&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/post-auto-format.py&quot;,
            &quot;timeout&quot;: 30
          }
        ]
      }
    ]
  }
}
```

**运行效果**：

```
Claude：我来创建app.js文件...

[Write工具执行成功]

[AutoFormat] app.js: ✅ 格式化成功
```

#### 完整示例2：自动备份Hook

**场景**：在Edit工具修改重要文件后，自动创建备份

**脚本 **.claude/hooks/post-auto-backup.py：

```
#!/usr/bin/env python3
&quot;&quot;&quot;
PostToolUse Hook - 自动备份
编辑重要文件后自动创建.bak备份
&quot;&quot;&quot;
import sys
import json
import shutil
from pathlib import Path
from datetime import datetime

# 需要备份的目录
BACKUP_DIRS = [&#x27;config&#x27;, &#x27;src&#x27;, &#x27;docs&#x27;, &#x27;.claude&#x27;]

def main():
    try:
        input_data = json.loads(sys.stdin.read())
    except json.JSONDecodeError:
        return

    tool_name = input_data.get(&#x27;tool_name&#x27;, &#x27;&#x27;)
    tool_input = input_data.get(&#x27;tool_input&#x27;, {})
    file_path = tool_input.get(&#x27;file_path&#x27;, &#x27;&#x27;)

    # 只处理Edit工具
    if tool_name != &#x27;Edit&#x27;:
        return

    # 检查是否在需要备份的目录中
    should_backup = any(dir_name in file_path for dir_name in BACKUP_DIRS)

    if should_backup and Path(file_path).exists():
        # 创建备份
        timestamp = datetime.now().strftime(&#x27;%Y%m%d_%H%M%S&#x27;)
        backup_path = f&quot;{file_path}.{timestamp}.bak&quot;

        try:
            shutil.copy2(file_path, backup_path)
            print(f&quot;[Backup] ✅ 已创建备份: {Path(backup_path).name}&quot;, file=sys.stderr)
        except Exception as e:
            print(f&quot;[Backup] ⚠️ 备份失败: {e}&quot;, file=sys.stderr)


if __name__ == &#x27;__main__&#x27;:
    main()
```

### 3.3 UserPromptSubmit（用户提示词提交）

> **一句话理解**：UserPromptSubmit就像"邮件发送前的自动补充"，可以在用户输入发送给Claude之前自动增强或过滤。

#### 触发时机

在用户提交提示词后，**Claude处理之前**。

#### 输入参数（通过stdin的JSON）

> ⚠️ **重要****：UserP**romptSubmit的输入是JSON格式，不是纯文本！

```
{
  &quot;session_id&quot;: &quot;abc123&quot;,
  &quot;transcript_path&quot;: &quot;/Users/.../.claude/projects/.../session.jsonl&quot;,
  &quot;cwd&quot;: &quot;/your/project/path&quot;,
  &quot;permission_mode&quot;: &quot;default&quot;,
  &quot;hook_event_name&quot;: &quot;UserPromptSubmit&quot;,
  &quot;prompt&quot;: &quot;请帮我写一篇关于AI的文章&quot;
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| session_id | string | 当前会话ID |
| prompt | string | 用户输入的原始提示词 |
| cwd | string | Claude Code的工作目录 |
| permission_mode | string | 权限模式（default/plan/bypassPermissions等） |
| hook_event_name | string | 事件类型标识 |

#### 输出格式

Hook的stdout输出会被**添加到AI上下文**（作为系统消息注入）。

**方式1：直接输出文本**（会作为额外上下文添加）

```
## 写作要求
- 字数：1500字
- 风格：老金式接地气风格
- 包含实战案例
```

**方式2：输出JSON格式**（更多控制）

```
{
  &quot;continue&quot;: true,
  &quot;suppressOutput&quot;: false
}
```

#### 完整示例：写作规范自动追加

**场景**：检测到写作任务时，自动追加写作规范

**脚本 ****.claude/hooks/us**er-prompt-enhance.py：

```
#!/usr/bin/env python3
&quot;&quot;&quot;
UserPromptSubmit Hook - 提示词自动增强
检测到特定任务时自动追加相关规范

【重要】：输入是JSON格式，必须先解析！
&quot;&quot;&quot;
import sys
import json

# 从stdin读取JSON输入
try:
    input_data = json.loads(sys.stdin.read())
except json.JSONDecodeError:
    # JSON解析失败，直接退出
    sys.exit(0)

# 获取用户输入的提示词
user_input = input_data.get(&#x27;prompt&#x27;, &#x27;&#x27;).strip()

# 如果没有prompt字段，直接退出
if not user_input:
    sys.exit(0)

# 过滤简单回复（不需要增强）
simple_responses = [&#x27;好的&#x27;, &#x27;是的&#x27;, &#x27;继续&#x27;, &#x27;ok&#x27;, &#x27;yes&#x27;, &#x27;no&#x27;, &#x27;确认&#x27;, &#x27;取消&#x27;]
if user_input.lower() in simple_responses or len(user_input) &lt; 5:
    # 不需要增强，不输出任何内容
    sys.exit(0)

# 过滤斜杠命令
if user_input.startswith(&#x27;/&#x27;):
    sys.exit(0)

# 检查是否是写作任务
writing_keywords = [&#x27;写&#x27;, &#x27;文章&#x27;, &#x27;生成&#x27;, &#x27;创作&#x27;, &#x27;write&#x27;, &#x27;article&#x27;, &#x27;内容&#x27;]
is_writing_task = any(kw in user_input.lower() for kw in writing_keywords)

if is_writing_task:
    # 输出额外上下文（会添加到AI上下文中）
    enhancement = &quot;&quot;&quot;
---
## 写作规范提醒（Hook自动注入）
1. **风格**：接地气、说人话，避免AI腔
2. **结构**：开头金句 -&gt; 核心要点 -&gt; 实战案例 -&gt; 总结升华
3. **字数**：1500-2000字
4. **检查**：完成后运行 /pre-check 进行质量检查
---&quot;&quot;&quot;
    print(enhancement)
    print(f&quot;[Hook] 已为写作任务注入规范&quot;, file=sys.stderr)

sys.exit(0)
```

> 💡 注**意：**

+   输入是JSON，必须用json.loads()解析

+   用户原始输入在prompt字段中

+   通过stdout输出JSON格式的additionalContext字段来注入上下文

+   stderr仅用于调试，不会显示在Claude Code界面

**配置**：

```
{
  &quot;hooks&quot;: {
    &quot;UserPromptSubmit&quot;: [
      {
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/user-prompt-enhance.py&quot;,
            &quot;timeout&quot;: 5
          }
        ]
      }
    ]
  }
}
```

**运行效果**：

用户输入：

```
帮我写一篇关于Claude Code的介绍文章
```

Claude实际收到：

```
帮我写一篇关于Claude Code的介绍文章

---
## 写作规范提醒（自动追加）
1. **风格**：接地气、说人话，避免AI腔
2. **结构**：开头金句 -&gt; 核心要点 -&gt; 实战案例 -&gt; 总结升华
3. **字数**：1500-2000字
4. **检查**：完成后运行 /pre-check 进行质量检查
---
```

### 3.4 Notification（通知）

> **一句话理解**：Notification Hook就像"消息推送处理器"，在Claude发送通知时触发。

#### 触发时机

当Claude Code需要请求用户权限或提示输入空闲超过60秒时。

#### 输入参数（通过stdin的JSON）

```
{
  &quot;session_id&quot;: &quot;abc123&quot;,
  &quot;transcript_path&quot;: &quot;/Users/.../.claude/projects/.../session.jsonl&quot;,
  &quot;cwd&quot;: &quot;/your/project/path&quot;,
  &quot;hook_event_name&quot;: &quot;Notification&quot;,
  &quot;message&quot;: &quot;Claude is waiting for your input&quot;
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| session_id | string | 当前会话ID |
| transcript_path | string | 会话记录文件路径 |
| cwd | string | 工作目录 |
| hook_event_name | string | 事件类型标识（固定为&quot;Notification&quot;） |
| message | string | 通知消息内容（Notification特有字段） |

> ⚠️ **注意****：根****据官方实现****，N**otification Hook没有title字段，只有message字段。

#### 完整示例：桌面通知Hook

**场景**：将Claude的通知转发到系统桌面通知

**脚本 ****.claude/hooks/not**ification-desktop.py：

```
#!/usr/bin/env python3
&quot;&quot;&quot;
Notification Hook - 桌面通知
将Claude的通知转发到系统桌面通知
&quot;&quot;&quot;
import sys
import json
import subprocess
import platform

def send_notification(title: str, message: str):
    &quot;&quot;&quot;发送系统桌面通知&quot;&quot;&quot;
    system = platform.system()

    try:
        if system == &#x27;Darwin&#x27;:  # macOS
            subprocess.run([
                &#x27;osascript&#x27;, &#x27;-e&#x27;,
                f&#x27;display notification &quot;{message}&quot; with title &quot;{title}&quot;&#x27;
            ])
        elif system == &#x27;Linux&#x27;:
            subprocess.run([&#x27;notify-send&#x27;, title, message])
        elif system == &#x27;Windows&#x27;:
            # Windows Toast通知（推荐方式）
            # 需要先安装：Install-Module -Name BurntToast -Scope CurrentUser
            try:
                # 优先使用BurntToast（更可靠）
                ps_cmd = f&#x27;New-BurntToastNotification -Text &quot;{title}&quot;, &quot;{message}&quot;&#x27;
                result = subprocess.run([&#x27;powershell&#x27;, &#x27;-Command&#x27;, ps_cmd], capture_output=True)
                if result.returncode != 0:
                    raise Exception(&quot;BurntToast not available&quot;)
            except:
                # 回退方案：使用Windows原生Toast
                ps_cmd = f&#x27;&#x27;&#x27;
                [Windows.UI.Notifications.ToastNotificationManager, Windows.UI.Notifications, ContentType = WindowsRuntime] | Out-Null
                $template = [Windows.UI.Notifications.ToastTemplateType]::ToastText02
                $xml = [Windows.UI.Notifications.ToastNotificationManager]::GetTemplateContent($template)
                $text = $xml.GetElementsByTagName(&quot;text&quot;)
                $text[0].AppendChild($xml.CreateTextNode(&quot;{title}&quot;)) | Out-Null
                $text[1].AppendChild($xml.CreateTextNode(&quot;{message}&quot;)) | Out-Null
                $toast = [Windows.UI.Notifications.ToastNotification]::new($xml)
                [Windows.UI.Notifications.ToastNotificationManager]::CreateToastNotifier(&quot;Claude Code&quot;).Show($toast)
                &#x27;&#x27;&#x27;
                subprocess.run([&#x27;powershell&#x27;, &#x27;-Command&#x27;, ps_cmd], capture_output=True)
    except Exception as e:
        print(f&quot;通知发送失败: {e}&quot;, file=sys.stderr)

def main():
    try:
        input_data = json.loads(sys.stdin.read())
    except json.JSONDecodeError:
        return

    # 获取消息内容（注意：没有notification_type字段）
    message = input_data.get(&#x27;message&#x27;, &#x27;&#x27;)
    session_id = input_data.get(&#x27;session_id&#x27;, &#x27;&#x27;)

    if not message:
        return

    # 构建通知标题
    title = &quot;Claude Code&quot;

    # 发送桌面通知
    send_notification(title, message)
    print(f&quot;[Notification] 已发送桌面通知: {message[:50]}...&quot;, file=sys.stderr)

if __name__ == &#x27;__main__&#x27;:
    main()
```

### 3.5 SessionStart（会话开始）

> **一句话理解**：SessionStart就像"开机自启动程序"，在Claude Code启动时自动执行初始化任务。

#### 触发时机

Claude Code**启动时**触发。

#### 用途

+   初始化环境

+   检查依赖

+   加载配置

#### 完整示例：环境检查Hook

**脚本 ****.claude/hooks/se**ssion-start-check.py：

```
#!/usr/bin/env python3
&quot;&quot;&quot;
SessionStart Hook - 环境检查
启动时检查必需的工具和依赖是否已安装
&quot;&quot;&quot;
import sys
import shutil

# 检查必需的工具
required_tools = {
    &#x27;node&#x27;: &#x27;Node.js (npm install)&#x27;,
    &#x27;python&#x27;: &#x27;Python 3.x&#x27;,
    &#x27;git&#x27;: &#x27;Git版本控制&#x27;,
}

# 可选但推荐的工具
optional_tools = {
    &#x27;prettier&#x27;: &#x27;Prettier代码格式化 (npm install -g prettier)&#x27;,
    &#x27;black&#x27;: &#x27;Black Python格式化 (pip install black)&#x27;,
}

missing_required = []
missing_optional = []

# 检查必需工具
for tool, desc in required_tools.items():
    if not shutil.which(tool):
        missing_required.append(f&quot;  X {tool}: {desc}&quot;)

# 检查可选工具
for tool, desc in optional_tools.items():
    if not shutil.which(tool):
        missing_optional.append(f&quot;  ! {tool}: {desc}&quot;)

# 输出检查结果
if missing_required or missing_optional:
    print(&quot;\n&quot; + &quot;=&quot;*50, file=sys.stderr)
    print(&quot;环境检查结果&quot;, file=sys.stderr)
    print(&quot;=&quot;*50, file=sys.stderr)

    if missing_required:
        print(&quot;\nX 缺少必需工具（请安装）：&quot;, file=sys.stderr)
        for item in missing_required:
            print(item, file=sys.stderr)

    if missing_optional:
        print(&quot;\n! 缺少可选工具（建议安装）：&quot;, file=sys.stderr)
        for item in missing_optional:
            print(item, file=sys.stderr)

    print(&quot;\n&quot; + &quot;=&quot;*50 + &quot;\n&quot;, file=sys.stderr)
else:
    print(&quot;V 环境检查通过，所有工具已就绪&quot;, file=sys.stderr)

sys.exit(0)
```

**配置**：

```
{
  &quot;hooks&quot;: {
    &quot;SessionStart&quot;: [
      {
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/session-start-check.py&quot;,
            &quot;timeout&quot;: 10
          }
        ]
      }
    ]
  }
}
```

### 3.6 SessionEnd 和 Stop

#### SessionEnd（会话结束）

**触发时机**：Claude Code正常退出时

**用途**：

+   清理临时文件

+   保存会话状态

+   备份日志

**示例脚本 ****.claude/hooks/se**ssion-end-cleanup.sh：

```
#!/bin/bash
# SessionEnd Hook - 清理临时文件

temp_dir=&quot;$HOME/.claude/temp&quot;
if [ -d &quot;$temp_dir&quot; ]; then
    rm -rf &quot;$temp_dir&quot;/*
    echo &quot;V 临时文件已清理&quot; &gt;&amp;2
fi

exit 0
```

#### Stop（AI停止响应）

**触发时机**：当Claude Code代理完成响应时

输入参数（通过stdin的JSON）：

```
{
  &quot;session_id&quot;: &quot;abc123&quot;,
  &quot;transcript_path&quot;: &quot;/Users/.../.claude/projects/.../session.jsonl&quot;,
  &quot;cwd&quot;: &quot;/your/project/path&quot;,
  &quot;hook_event_name&quot;: &quot;Stop&quot;
}
```

> ⚠️ **注意**：Stop Hook**没有****reason****字段**，只有标准字段。

**用途**：

+   保存当前状态

+   记录会话信息

+   可以通过输出{"continue": false}强制停止

**示例脚本 **.claude/hooks/stop-save-state.py：

```
#!/usr/bin/env python3
&quot;&quot;&quot;
Stop Hook - 保存状态
AI停止时保存当前会话状态
&quot;&quot;&quot;
import sys
import json
from datetime import datetime
from pathlib import Path

try:
    input_data = json.loads(sys.stdin.read())
except json.JSONDecodeError:
    input_data = {}

# 获取标准字段（注意：没有reason字段）
session_id = input_data.get(&#x27;session_id&#x27;, &#x27;unknown&#x27;)
cwd = input_data.get(&#x27;cwd&#x27;, str(Path.cwd()))

# 保存状态
state_dir = Path.home() / &#x27;.claude&#x27; / &#x27;state&#x27;
state_dir.mkdir(parents=True, exist_ok=True)
state_file = state_dir / &#x27;last-session-state.json&#x27;

state = {
    &quot;stopped_at&quot;: datetime.now().isoformat(),
    &quot;session_id&quot;: session_id,
    &quot;project_dir&quot;: cwd
}

with open(state_file, &#x27;w&#x27;, encoding=&#x27;utf-8&#x27;) as f:
    json.dump(state, f, indent=2, ensure_ascii=False)

print(f&quot;V 会话状态已保存到: {state_file}&quot;, file=sys.stderr)
sys.exit(0)
```

---

### 3.7 WorktreeCreate 和 WorktreeRemove（工作树管理）🆕

> **v2.1.49+ 新增**：这两个Hook类型配合 Claude Code 内置的 Git Worktree 功能使用，用于 Git Worktree（工作树）的生命周期管理。

#### 什么是 Git Worktree？

> 💡 生**活类比：**Git Worktree 就像在同一个项目里开了多个"平行工作台"。你可以在工作台A修bug，同时在工作台B开发新功能，互不干扰。Claude Code 使用 Worktree 来实现并行任务隔离。

#### WorktreeCreate（工作树创建时触发）

**触发时机**：Claude Code 创建新的 Git Worktree 时自动触发

**典型用途**：

+   初始化工作树特定的环境配置

+   安装工作树所需的依赖

+   设置工作树专属的环境变量

+   记录工作树创建日志

**配置示例**：

```
{
  &quot;hooks&quot;: {
    &quot;WorktreeCreate&quot;: [
      {
        &quot;command&quot;: &quot;echo \&quot;新工作树已创建: $(date)\&quot; &gt;&gt; ~/.claude/worktree.log&quot;,
        &quot;timeout&quot;: 10000
      }
    ]

  }
}
```

**输入数据**（通过 stdin 接收 JSON）：

```
{
  &quot;worktree_path&quot;: &quot;/path/to/worktree&quot;,
  &quot;branch&quot;: &quot;feature/new-feature&quot;
}
```

#### WorktreeRemove（工作树删除时触发）

**触发时机**：Claude Code 删除 Git Worktree 时自动触发

**典型用途**：

+   清理工作树相关的临时文件

+   释放工作树占用的资源

+   记录工作树删除日志

+   归档工作树的工作成果

**配置示例**：

```
{
  &quot;hooks&quot;: {
    &quot;WorktreeRemove&quot;: [
      {
        &quot;command&quot;: &quot;echo \&quot;工作树已删除: $(date)\&quot; &gt;&gt; ~/.claude/worktree.log&quot;,
        &quot;timeout&quot;: 10000
      }
    ]
  }
}
```

**实战场景：工作树生命周期管理**

```
{
  &quot;hooks&quot;: {
    &quot;WorktreeCreate&quot;: [
      {
        &quot;command&quot;: &quot;.claude/hooks/worktree-init.sh&quot;,
        &quot;timeout&quot;: 30000
      }
    ],
    &quot;WorktreeRemove&quot;: [
      {
        &quot;command&quot;: &quot;.claude/hooks/worktree-cleanup.sh&quot;,
        &quot;timeout&quot;: 15000
      }
    ]
  }
}
```

> ⚠️ **注意****：Wor**ktreeCreate 和 WorktreeRemove 都是不可阻止的Hook，它们只用于执行附加操作，不能阻止工作树的创建或删除。

#### 与 --worktree 启动参数的关系

> 🔥 重**要：**2**026年2月**（v2.1.49），Claude Code 正式内置了 Git Worktree 支持，这是一个核心功能级别的更新，不仅仅是 Hook。

**--worktree****（****-w****）启动参数**：

```
# 在独立工作树中启动 Claude Code
claude --worktree
# 或简写
claude -w
```

**工作原理**：

```
你的项目仓库（主工作目录）
├── .git/                    # 共享的 Git 历史
├── .claude/worktrees/       # 工作树存放目录（加到 .gitignore）
│   ├── worktree-abc123/     # Agent A 的独立工作目录
│   └── worktree-def456/     # Agent B 的独立工作目录
├── src/                     # 主工作目录的文件
└── ...
```

**使用场景**：

| 场景 | 说明 |
| --- | --- |
| 并行开发 | 终端1: claude -w 开发新功能 / 终端2: claude -w 修复 bug |
| 代码审查 | 主工作目录继续开发,工作树中运行审查 Agent |
| 实验性修改 | 在工作树中试验方案,不影响主目录 |

**配置步骤**：

```
# 1. 将工作树目录加到 .gitignore
echo &quot;.claude/worktrees/&quot; &gt;&gt; .gitignore

# 2. 启动第一个 Agent（在工作树中）
claude -w

# 3. 打开另一个终端，启动第二个 Agent（在另一个工作树中）
claude -w
```

**Hook 的角色**：

+   **Git 用户**：直接使用 claude -w 即可，Hook 是可选的增强（如自动安装依赖）

+   **非 Git 用户**（SVN/Perforce/Mercurial）：通过 WorktreeCreate/WorktreeRemove Hook 自定义工作树的创建和清理逻辑，替代默认的 Git 行为

---

### 3.8 SubagentStart 和 SubagentStop（子代理生命周期）🆕

> **v2.1.49+ 新增**：这两个Hook类型用于监控和管理子代理（Subagent/Task）的生命周期。

#### SubagentStart（子代理启动时触发）

**触发时机**：Claude Code 启动子代理（通过 Task 工具）时自动触发

**典型用途**：

+   记录子代理启动日志

+   初始化子代理专属的环境配置

+   监控并发子代理数量

**配置示例**：

```
{
  &quot;hooks&quot;: {
    &quot;SubagentStart&quot;: [
      {
        &quot;command&quot;: &quot;echo \&quot;子代理已启动: $(date)\&quot; &gt;&gt; ~/.claude/subagent.log&quot;,
        &quot;timeout&quot;: 5000
      }
    ]
  }
}
```

#### SubagentStop（子代理停止时触发）

**触发时机**：子代理完成任务或被终止时自动触发

**典型用途**：

+   收集子代理执行结果

+   清理子代理使用的临时资源

+   记录子代理执行耗时

---

### 3.9 PermissionRequest（权限请求）🆕

> **v2.1.49+ 新增**：在权限请求时触发，可用于实现自动审批策略。

**触发时机**：Claude Code 请求执行需要用户授权的操作时

**典型用途**：

+   根据规则自动批准/拒绝权限请求

+   记录权限请求审计日志

+   实现基于项目的权限策略

**配置示例**：

```
{
  &quot;hooks&quot;: {
    &quot;PermissionRequest&quot;: [
      {
        &quot;command&quot;: &quot;python .claude/hooks/permission-policy.py&quot;,
        &quot;timeout&quot;: 5000
      }
    ]
  }
}
```

---

### 3.10 PreCompact（上下文压缩前）🆕

> **v2.1.49+ 新增**：在 Claude Code 执行上下文压缩（Compact）前触发。

**触发时机**：当对话上下文即将被压缩时

**典型用途**：

+   在压缩前保存关键上下文信息

+   记录压缩事件日志

+   导出当前对话状态

---

### 3.11 ConfigChange（配置变更）🆕

> **v2.1.49+ 新增**：在 Claude Code 配置发生变更时触发。

**触发时机**：settings.json 或其他配置文件被修改时

**典型用途**：

+   配置变更审计和日志记录

+   自动同步配置到其他环境

+   配置变更通知

---

### 3.12 TeammateIdle（队友空闲）🆕

> **v2.1.49+ 新增**：在多代理协作场景中，当队友代理进入空闲状态时触发。

**触发时机**：协作中的队友代理完成当前任务、进入空闲状态时

**典型用途**：

+   自动分配待处理任务给空闲队友

+   发送状态通知

+   协调多代理工作流

---

### 3.13 新增Hook事件类型 🆕

> **v2.1+ 新增**：以下Hook类型进一步扩展了Claude Code的事件覆盖范围，让你能捕捉到更多关键时刻。

> 🎯 生**活类比：**如果之前的Hook是"门窗报警器"，这些新Hook就是"烟雾报警器、水浸传感器、燃气检测器"——覆盖了之前监控不到的异常场景。

#### StopFailure（API异常停止时触发）

**触发时机**：当API错误（429限流、401认证失败、500服务器错误等）导致会话异常停止时触发

> ⚡ **与Stop的区别**：Stop 是正常结束（用户主动退出、任务完成），就像下班正常关灯锁门；StopFailure 是异常中断，就像突然停电——你需要知道发生了什么并采取措施。

**典型用途**：

+   发送告警通知（Slack/邮件/钉钉）

+   记录错误日志用于后续分析

+   触发自动重试或降级逻辑

**配置示例**：

```
{
  &quot;hooks&quot;: {
    &quot;StopFailure&quot;: [
      {
        &quot;command&quot;: &quot;python .claude/hooks/alert-on-failure.py&quot;,
        &quot;timeout&quot;: 10000
      }
    ]
  }
}
```

> 📝 输**入数据：**StopFailure的stdin JSON中包含 error 字段，携带具体的错误类型和消息，可用于区分限流、认证失败等不同场景。

---

#### PostCompact（上下文压缩后触发）

**触发时机**：执行 /compact 命令或上下文自动压缩完成后触发

> 🎯 生**活类比：**就像搬家后清点物品——压缩完成后，你想知道"丢掉了多少东西、还剩多少空间"。

**典型用途**：

+   记录压缩前后的token数量变化

+   触发上下文恢复操作（如重新加载关键文件）

+   日志记录用于监控上下文使用趋势

**配置示例**：

```
{
  &quot;hooks&quot;: {
    &quot;PostCompact&quot;: [
      {
        &quot;command&quot;: &quot;echo \&quot;[$(date)] 上下文已压缩\&quot; &gt;&gt; ~/.claude/compact.log&quot;,
        &quot;timeout&quot;: 5000
      }
    ]
  }
}
```

> ⚠️ **注意**：PostCompact 是**不可阻止**的Hook，压缩已经完成，你只能执行后续操作。与 PreCompact（压缩前）配合使用效果更佳。

---

#### InstructionsLoaded（指令文件加载时触发）

**触发时机**：CLAUDE.md 等指令文件加载到上下文时触发

> 🎯 生**活类比：**就像新员工入职时检查培训手册是否齐全——确保AI读到了正确且完整的指令。

**典型用途**：

+   验证指令文件的完整性和正确性

+   记录哪些指令文件被加载（便于调试）

+   触发项目特定的初始化操作

**配置示例**：

```
{
  &quot;hooks&quot;: {
    &quot;InstructionsLoaded&quot;: [
      {
        &quot;command&quot;: &quot;python .claude/hooks/verify-instructions.py&quot;,
        &quot;timeout&quot;: 5000
      }
    ]
  }
}
```

> 📝 输**入数据：**stdin JSON中包含已加载指令文件的路径列表，可用于检查关键指令文件是否缺失。

---

#### Elicitation 和 ElicitationResult（MCP交互输入）🆕

> **v2.1+ 新增**：这两个Hook配合MCP Elicitation功能使用，让你能监控和管理MCP服务器与用户之间的交互输入。

> 🎯 生**活类比：**Elicitation 就像客服系统弹出问卷调查——MCP服务器需要向用户询问信息；ElicitationResult 就像用户填完问卷提交——你可以记录和验证填写的内容。

Elicitation — MCP请求输入时触发

**触发时机**：MCP服务器通过Elicitation API向用户发起交互输入请求时

**典型用途**：

+   记录MCP交互请求日志

+   自动填充常用值（如默认配置）

ElicitationResult — 用户完成输入后触发

**触发时机**：用户完成MCP交互输入并提交后

**典型用途**：

+   验证用户输入数据的合法性

+   记录交互结果用于审计

+   触发后续自动化流程

**配置示例**：

```
{
  &quot;hooks&quot;: {
    &quot;Elicitation&quot;: [
      {
        &quot;command&quot;: &quot;echo \&quot;MCP请求输入: $(date)\&quot; &gt;&gt; ~/.claude/elicitation.log&quot;,
        &quot;timeout&quot;: 5000
      }
    ],
    &quot;ElicitationResult&quot;: [
      {
        &quot;command&quot;: &quot;python .claude/hooks/validate-elicitation.py&quot;,
        &quot;timeout&quot;: 5000
      }
    ]
  }
}
```

---

### 3.14 HTTP Hooks（远程Webhook集成）🆕

> **v2.1+ 新增**：除了传统的Shell命令Hook，现在可以直接将Hook事件POST到远程URL，无需编写本地脚本。

> 🎯 生**活类比：**之前的Hook像是"自家装的门铃"——得自己接线、自己写响铃逻辑；HTTP Hook像是"接入物业监控中心"——事件发生时自动通知远程服务，你只管接收处理。

**配置格式**：

```
{
  &quot;hooks&quot;: {
    &quot;PostToolUse&quot;: [
      {
        &quot;type&quot;: &quot;http&quot;,
        &quot;url&quot;: &quot;https://your-webhook.example.com/hook&quot;,
        &quot;timeout&quot;: 5000
      }
    ]
  }
}
```

**工作原理**：

+   **当Hoo**k事件触发时，Claude Code 自动向配置的URL发送 POST 请求

+   请求体为JSON格式，自动包含该Hook的标准输入数据（与Shell Hook的stdin内容相同）

+   支持设置超时时间，防止远程服务无响应时阻塞工作流

Shell Hook vs HTTP Hook 对比：

| 特性 | Shell Hook | HTTP Hook 🆕 |
| --- | --- | --- |
| 执行方式 | 运行本地脚本/命令 | POST JSON到远程URL |
| 需要本地脚本 | ✅ 是 | ❌ 否 |
| 跨平台一致性 | ❌ 需处理OS差异 | ✅ 任何平台行为一致 |
| 适合场景 | 本地文件操作、代码检查 | 远程通知、日志收集、CI/CD |
| 网络依赖 | ❌ 无 | ✅ 需要网络连接 |
| 配置复杂度 | 中（需写脚本） | 低（只需URL） |

**适用场景**：

+   **Slack/Discord通知**：代码提交、构建完成时自动发送消息

+   **CI/CD触发**：Hook事件触发远程构建管道

+   **日志收集服务**：将所有Hook事件发送到Datadog/ELK等平台

+   **团队协作**：多人开发时自动同步状态到共享服务

> ⚠️ **安全提示**：HTTP Hook会将Hook数据发送到外部服务，确保目标URL可信且使用HTTPS。避免在Hook数据中包含敏感信息（API密钥、密码等）。

---

## 第四部分：实战应用场景

> **本节目的**：学习真实项目中的Hook应用

> ⏱️ **预计时间**：1-1.5小时

### 4.1 Git自动化工作流

#### 场景1：提交前自动检查

**需求**：在git commit前自动运行代码检查，包括：

+   代码风格检查（lint）

+   敏感信息检查（API Key等）

+   分支保护（禁止直接提交main）

**完整脚本 ****.claude/hooks/git-p**re-commit-checker.py：

```
#!/usr/bin/env python3
&quot;&quot;&quot;
Git提交前检查系统
在执行git commit前自动运行多项检查
&quot;&quot;&quot;
import sys
import json
import subprocess
import re
from pathlib import Path
from concurrent.futures import ThreadPoolExecutor, as_completed

# 配置
CONFIG = {
    &#x27;protected_branches&#x27;: [&#x27;main&#x27;, &#x27;master&#x27;, &#x27;production&#x27;],
    &#x27;secret_patterns&#x27;: [
        r&#x27;(?i)(api[_-]?key|apikey)\s*[=:]\s*[&quot;\&#x27;]?[\w-]{20,}&#x27;,
        r&#x27;(?i)(secret|password|passwd|pwd)\s*[=:]\s*[&quot;\&#x27;]?[\w-]{8,}&#x27;,
        r&#x27;(?i)(access[_-]?token|auth[_-]?token)\s*[=:]\s*[&quot;\&#x27;]?[\w-]{20,}&#x27;,
        r&#x27;sk-[a-zA-Z0-9]{20,}&#x27;,  # OpenAI API Key
        r&#x27;ghp_[a-zA-Z0-9]{36,}&#x27;,  # GitHub Token
    ],
}

def run_command(cmd, timeout=60):
    &quot;&quot;&quot;运行命令并返回结果&quot;&quot;&quot;
    try:
        result = subprocess.run(cmd, shell=True, capture_output=True, text=True, timeout=timeout)
        return result.returncode, result.stdout, result.stderr
    except subprocess.TimeoutExpired:
        return -1, &#x27;&#x27;, &#x27;Command timed out&#x27;
    except Exception as e:
        return -1, &#x27;&#x27;, str(e)

def check_branch():
    &quot;&quot;&quot;检查分支规则&quot;&quot;&quot;
    code, stdout, _ = run_command(&#x27;git rev-parse --abbrev-ref HEAD&#x27;)
    if code != 0:
        return True, &quot;无法获取当前分支&quot;

    branch = stdout.strip()
    if branch in CONFIG[&#x27;protected_branches&#x27;]:
        return False, f&quot;X 禁止直接提交到受保护分支: {branch}\n请使用Pull Request&quot;

    return True, f&quot;当前分支: {branch}&quot;

def check_secrets():
    &quot;&quot;&quot;检查敏感信息&quot;&quot;&quot;
    code, stdout, _ = run_command(&#x27;git diff --cached&#x27;)
    if code != 0:
        return True, &quot;无法获取diff&quot;

    findings = []
    for pattern in CONFIG[&#x27;secret_patterns&#x27;]:
        if re.search(pattern, stdout):
            findings.append(f&quot;发现可疑模式: {pattern[:40]}...&quot;)

    if findings:
        return False, &quot;X 发现可能的敏感信息:\n&quot; + &#x27;\n&#x27;.join(findings)
    return True, &quot;敏感信息检查通过&quot;

def check_lint():
    &quot;&quot;&quot;代码风格检查&quot;&quot;&quot;
    code, stdout, _ = run_command(&#x27;git diff --cached --name-only --diff-filter=ACMR&#x27;)
    if code != 0:
        return True, &quot;无法获取变更文件列表&quot;

    files = [f for f in stdout.strip().split(&#x27;\n&#x27;) if f]
    py_files = [f for f in files if f.endswith(&#x27;.py&#x27;)]
    js_files = [f for f in files if f.endswith((&#x27;.js&#x27;, &#x27;.ts&#x27;, &#x27;.jsx&#x27;, &#x27;.tsx&#x27;))]

    errors = []

    # Python文件检查
    if py_files:
        code, stdout, stderr = run_command(f&#x27;ruff check {&quot; &quot;.join(py_files)}&#x27;)
        if code != 0:
            errors.append(f&quot;Python代码问题:\n{stdout or stderr}&quot;)

    # JavaScript/TypeScript文件检查
    if js_files:
        code, stdout, stderr = run_command(f&#x27;npx eslint {&quot; &quot;.join(js_files)} --quiet&#x27;)
        if code != 0:
            errors.append(f&quot;JS/TS代码问题:\n{stdout or stderr}&quot;)

    if errors:
        return False, &#x27;\n&#x27;.join(errors)
    return True, &quot;代码风格检查通过&quot;

def main():
    # 读取输入
    try:
        input_data = json.loads(sys.stdin.read())
    except json.JSONDecodeError:
        return

    tool_name = input_data.get(&#x27;tool_name&#x27;, &#x27;&#x27;)
    command = input_data.get(&#x27;tool_input&#x27;, {}).get(&#x27;command&#x27;, &#x27;&#x27;)

    # 只处理git commit命令
    if tool_name != &#x27;Bash&#x27; or &#x27;git commit&#x27; not in command:
        return

    # 运行检查
    checks = [
        (&#x27;分支检查&#x27;, check_branch),
        (&#x27;敏感信息&#x27;, check_secrets),
        (&#x27;代码风格&#x27;, check_lint),
    ]

    results = []
    all_passed = True

    # 并行执行检查
    with ThreadPoolExecutor(max_workers=3) as executor:
        future_to_check = {executor.submit(check[1]): check[0] for check in checks}
        for future in as_completed(future_to_check):
            check_name = future_to_check[future]
            try:
                passed, message = future.result()
                results.append((check_name, passed, message))
                if not passed:
                    all_passed = False
            except Exception as e:
                results.append((check_name, False, f&quot;检查异常: {str(e)}&quot;))
                all_passed = False

    # 输出报告
    print(&quot;\n&quot; + &quot;=&quot;*60, file=sys.stderr)
    print(&quot;Git提交前检查报告&quot;, file=sys.stderr)
    print(&quot;=&quot;*60, file=sys.stderr)


    for name, passed, message in results:
        status = &quot;V PASS&quot; if passed else &quot;X FAIL&quot;
        print(f&quot;\n{status} {name}&quot;, file=sys.stderr)
        print(f&quot;   {message}&quot;, file=sys.stderr)

    print(&quot;\n&quot; + &quot;=&quot;*60, file=sys.stderr)

    # 输出决策
    if not all_passed:
        decision = {
            &quot;decision&quot;: &quot;ask&quot;,
            &quot;message&quot;: &quot;检查未通过，是否仍要继续提交？&quot;
        }
        print(json.dumps(decision, ensure_ascii=False))
    else:
        print(&quot;所有检查通过，允许提交&quot;, file=sys.stderr)

if __name__ == &#x27;__main__&#x27;:
    main()
```

**配置**：

```
{
  &quot;hooks&quot;: {
    &quot;PreToolUse&quot;: [
      {
        &quot;matcher&quot;: &quot;Bash&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/git-pre-commit-checker.py&quot;,
            &quot;timeout&quot;: 120
          }
        ]
      }
    ]
  }
}
```

### 4.2 代码质量保障

#### 场景：Markdown文章质量检查

**需求**：保存Markdown文章后自动检查：

+   字数统计

+   标题检查

+   段落数量

**脚本 ****.claude/hooks/pos**t-article-quality.py：

```
#!/usr/bin/env python3
&quot;&quot;&quot;
PostToolUse Hook - 文章质量检查
保存Markdown文件后自动进行质量检查
&quot;&quot;&quot;
import sys
import json
from pathlib import Path

def check_article_quality(file_path):
    &quot;&quot;&quot;检查文章质量&quot;&quot;&quot;
    try:
        content = Path(file_path).read_text(encoding=&#x27;utf-8&#x27;)
    except Exception as e:
        return f&quot;无法读取文件: {e}&quot;

    # 统计指标
    char_count = len(content)
    word_count = len(content.split())
    has_title = content.strip().startswith(&#x27;#&#x27;)
    paragraphs = [p for p in content.split(&#x27;\n\n&#x27;) if p.strip()]
    paragraph_count = len(paragraphs)

    # 生成报告
    report = []
    report.append(&quot;\n&quot; + &quot;=&quot;*50)
    report.append(&quot;文章质量检查报告&quot;)
    report.append(&quot;=&quot;*50)
    report.append(f&quot;\n字符数: {char_count} {&#x27;V&#x27; if char_count &gt; 500 else &#x27;! 偏短&#x27;}&quot;)
    report.append(f&quot;词数: {word_count}&quot;)
    report.append(f&quot;标题: {&#x27;V 有&#x27; if has_title else &#x27;X 缺少一级标题&#x27;}&quot;)
    report.append(f&quot;段落数: {paragraph_count} {&#x27;V&#x27; if paragraph_count &gt; 3 else &#x27;! 偏少&#x27;}&quot;)

    # 建议
    suggestions = []
    if char_count &lt; 500:
        suggestions.append(&quot;- 建议增加内容，至少500字&quot;)
    if not has_title:
        suggestions.append(&quot;- 建议添加一级标题（# 标题）&quot;)
    if paragraph_count &lt; 3:
        suggestions.append(&quot;- 建议增加段落，改善可读性&quot;)

    if suggestions:
        report.append(&quot;\n改进建议:&quot;)
        report.extend(suggestions)
    else:
        report.append(&quot;\nV 文章质量良好！&quot;)

    report.append(&quot;=&quot;*50 + &quot;\n&quot;)

    return &#x27;\n&#x27;.join(report)

def main():
    try:
        input_data = json.loads(sys.stdin.read())
    except json.JSONDecodeError:
        return

    tool_name = input_data.get(&#x27;tool_name&#x27;, &#x27;&#x27;)
    tool_input = input_data.get(&#x27;tool_input&#x27;, {})
    file_path = tool_input.get(&#x27;file_path&#x27;, &#x27;&#x27;)

    # 只处理Write工具和.md文件
    if tool_name != &#x27;Write&#x27; or not file_path.endswith(&#x27;.md&#x27;):
        return

    # 检查质量
    report = check_article_quality(file_path)
    print(report, file=sys.stderr)

if __name__ == &#x27;__main__&#x27;:
    main()
```

### 4.3 完整配置示例

**综合配置 **.claude/settings.json：

```
{
  &quot;hooks&quot;: {
    &quot;UserPromptSubmit&quot;: [
      {
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/user-prompt-enhance.py&quot;,
            &quot;timeout&quot;: 5
          }
        ]
      }
    ],
    &quot;PreToolUse&quot;: [
      {
        &quot;matcher&quot;: &quot;Write|Edit&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/pre-protect-production.py&quot;,
            &quot;timeout&quot;: 5
          }
        ]
      },
      {
        &quot;matcher&quot;: &quot;Bash&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/pre-block-dangerous-cmd.py&quot;,
            &quot;timeout&quot;: 5
          },
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/git-pre-commit-checker.py&quot;,
            &quot;timeout&quot;: 120
          }
        ]
      }
    ],
    &quot;PostToolUse&quot;: [
      {
        &quot;matcher&quot;: &quot;Write&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/post-auto-format.py&quot;,
            &quot;timeout&quot;: 30
          },
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/post-article-quality.py&quot;,
            &quot;timeout&quot;: 10
          }
        ]
      },
      {
        &quot;matcher&quot;: &quot;Edit&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/post-auto-backup.py&quot;,
            &quot;timeout&quot;: 10
          }
        ]
      }
    ],
    &quot;SessionStart&quot;: [
      {
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/session-start-check.py&quot;,
            &quot;timeout&quot;: 10
          }
        ]
      }
    ],
    &quot;Notification&quot;: [
      {
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;python .claude/hooks/notification-desktop.py&quot;,
            &quot;timeout&quot;: 5
          }
        ]
      }
    ]
  }
}
```

---

## 第五部分：故障排查

> **本节目的**：快速解决Hook配置和执行问题

> ⏱️ **预计时间**：按需查阅

### 5.1 Hook不执行

**症状**：配置了Hook但完全没有触发

**排查步骤**：

1.  **检查配置文件路径**

```
# 确认settings.json在正确位置
ls -la .claude/settings.json
# 应该存在且有内容
```

1.  **检查JSON格式**

```
# 验证JSON格式
python -c &quot;import json; json.load(open(&#x27;.claude/settings.json&#x27;))&quot;
# 没有报错说明格式正确
```

1.  **检查Matcher是否匹配**

```
// 错误：matcher拼写错误
&quot;matcher&quot;: &quot;write&quot;  // X 应该是大写W

// 正确
&quot;matcher&quot;: &quot;Write&quot;  // V
```

1.  **检查脚本路径**

```
# 确认脚本存在
ls -la .claude/hooks/your-hook.py

# macOS/Linux检查执行权限
chmod +x .claude/hooks/your-hook.py
```

1.  **重启Claude Code**

```
# 退出当前会话
exit

# 重新启动
claude
```

### 5.2 Hook执行报错

**症状**：Hook触发了但报错退出

**排查步骤**：

1.  **手动测试脚本**

```
# 模拟输入测试
echo &#x27;{&quot;tool_name&quot;: &quot;Write&quot;, &quot;tool_input&quot;: {&quot;file_path&quot;: &quot;test.txt&quot;}}&#x27; | python .claude/hooks/your-hook.py

# 查看输出和错误
```

1.  **检查Python版本**

```
python --version
# 应该是Python 3.x
```

1.  **检查依赖是否安装**

```
# 如果脚本import了第三方库
pip install 缺少的库
```

1.  **查看stderr输出**

```
# 在脚本中添加调试输出
import sys
print(&quot;DEBUG: 脚本开始执行&quot;, file=sys.stderr)
print(f&quot;DEBUG: 收到输入: {input_data}&quot;, file=sys.stderr)
```

### 5.3 Hook超时

**症状**：Hook执行时间过长被强制终止

**解决方案**：

1.  **增加timeout配置**

```
{
  &quot;type&quot;: &quot;command&quot;,
  &quot;command&quot;: &quot;python .claude/hooks/slow-hook.py&quot;,
  &quot;timeout&quot;: 120  // 增加到120秒
}
```

1.  **优化脚本性能**

```
# 避免不必要的文件扫描
# 使用增量检查而不是全量检查
# 并行执行多个检查任务
```

1.  **异步处理**

```
# 对于不需要阻塞的任务，可以后台执行
import subprocess
subprocess.Popen([&#x27;python&#x27;, &#x27;background-task.py&#x27;],
                 stdout=subprocess.DEVNULL,
                 stderr=subprocess.DEVNULL)
```

### 5.4 Windows特有问题

**问题1：中文乱码**

**症状**：Hook输出中文显示为乱码

**解决方案**：

```
# 在脚本开头设置编码
import sys
import io
sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding=&#x27;utf-8&#x27;)
sys.stderr = io.TextIOWrapper(sys.stderr.buffer, encoding=&#x27;utf-8&#x27;)
```

**问题2：路径分隔符**

**症状**：Windows路径包含反斜杠导致问题

**解决方案**：

```
# 统一处理路径
file_path = file_path.replace(&#x27;\\&#x27;, &#x27;/&#x27;)
```

**问题3：Batch脚本编码**

**症状**：.bat文件中文乱码

**解决方案**：

```
@echo off
chcp 65001 &gt;nul
REM 脚本内容...
```

### 5.5 调试技巧

**技巧1：日志文件**

```
# 写入调试日志
from pathlib import Path
from datetime import datetime

log_file = Path.home() / &#x27;.claude&#x27; / &#x27;hooks-debug.log&#x27;
log_file.parent.mkdir(parents=True, exist_ok=True)

with open(log_file, &#x27;a&#x27;, encoding=&#x27;utf-8&#x27;) as f:
    f.write(f&quot;[{datetime.now()}] {message}\n&quot;)
```

**技巧2：条件调试**

```
import os

# 通过环境变量控制调试模式
DEBUG = os.getenv(&#x27;CLAUDE_HOOK_DEBUG&#x27;, &#x27;&#x27;).lower() == &#x27;true&#x27;

if DEBUG:
    print(f&quot;DEBUG: {data}&quot;, file=sys.stderr)
```

**技巧3：逐步排除法**

```
// 先只保留一个Hook测试
{
  &quot;hooks&quot;: {
    &quot;PostToolUse&quot;: [
      {
        &quot;matcher&quot;: &quot;Write&quot;,
        &quot;hooks&quot;: [
          {
            &quot;type&quot;: &quot;command&quot;,
            &quot;command&quot;: &quot;echo &#x27;Hook triggered!&#x27; &gt;&amp;2&quot;
          }
        ]
      }
    ]
  }
}
```

---

## 第六部分：FAQ（20个常见问题）

### 基础问题

Q1: Hook和CLAUDE.md有什么区别？

| 对比 | Hook | CLAUDE.md |
| --- | --- | --- |
| **执行方式** | 自动执行Shell命令 | Claude解读后决定是否遵循 |
| **可靠性** | 100%执行 | 不确定（AI可能忘记） |
| **用途** | 强制规则、自动化 | 提供上下文、建议 |

Q2: Hook可以用什么语言写？

任何可执行程序都可以：

+   Python（推荐，跨平台）

+   Bash/Shell（macOS/Linux）

+   Batch（Windows）

+   Node.js

+   Go、Rust等（需要编译）

Q3: Hook的timeout默认是多少？

默认60秒。建议根据任务复杂度设置：

+   简单检查：5-10秒

+   代码格式化：30秒

+   完整测试：120秒

Q4: 一个事件可以配置多个Hook吗？

可以！多个Hook会按顺序执行：

```
{
  &quot;matcher&quot;: &quot;Write&quot;,
  &quot;hooks&quot;: [
    {&quot;type&quot;: &quot;command&quot;, &quot;command&quot;: &quot;python hook1.py&quot;},
    {&quot;type&quot;: &quot;command&quot;, &quot;command&quot;: &quot;python hook2.py&quot;}
  ]
}
```

Q5: PreToolUse的decision有哪些值？

PreToolUse 有新旧两套API（新旧并存）：

**新版（推荐）** — 通过 hookSpecificOutput.permissionDecision 字段：

| 值 | 效果 |
| --- | --- |
| &quot;allow&quot; | 允许执行，绕过权限系统 |
| &quot;deny&quot; | 拒绝执行，原因会反馈给Claude |
| &quot;ask&quot; | 暂停，询问用户决定 |
| 无输出 | 默认允许 |

**旧版（已废弃但仍支持）** — 通过 decision 字段：

| 旧版值 | 等同于新版 |
| --- | --- |
| &quot;approve&quot; | &quot;allow&quot; |
| &quot;block&quot; | &quot;deny&quot; |

PostToolUse 和 UserPromptSubmit 使用 "block" 来阻止/提供反馈，无输出则默认允许。

### 配置问题

Q6: settings.json应该放在哪里？

项目根目录的 .claude/settings.json

Q7: Matcher支持正则表达式吗？

支持！例如：

+   "Write" - 精确匹配

+   "Write|Edit" - 匹配Write或Edit

+   ".*" - 匹配所有工具（慎用）

Q8: 如何让Hook只对特定目录的文件生效？

在脚本中检查文件路径：

```
if &#x27;/articles/&#x27; not in file_path:
    sys.exit(0)  # 不在目标目录，跳过
```

Q9: 环境变量怎么传递给Hook脚本？

Claude Code自动传递的环境变量：

+   CLAUDE_PROJECT_DIR - 项目根目录的绝对路径（Claude Code启动目录）

> ⚠️ **注意**：session_id不是环境变量，而是通过stdin的JSON输入传递！

使用方法：

```
import os
import json
import sys

# 从环境变量获取项目目录
project_dir = os.getenv(&#x27;CLAUDE_PROJECT_DIR&#x27;, os.getcwd())

# 从stdin的JSON获取session_id（不是环境变量！）
input_data = json.loads(sys.stdin.read())
session_id = input_data.get(&#x27;session_id&#x27;, &#x27;&#x27;)
```

Q10: 如何临时禁用Hook？

方法1：重命名配置文件

```
mv .claude/settings.json .claude/settings.json.bak
```

方法2：注释掉Hook配置（JSON不支持注释，需要删除）

### 脚本问题

Q11: 如何读取Hook的输入？

```
import sys
import json

# stdin读取JSON
input_data = json.loads(sys.stdin.read())
tool_name = input_data.get(&#x27;tool_name&#x27;)
```

Q12: 如何在Hook中向用户输出信息？

**重要**：不同Hook类型的输出机制不同！

PostToolUse Hook：无法直接输出给用户

+   Claude Code只会显示"Hook执行成功/失败"

+   如需调试，写入日志文件：

```
from pathlib import Path
log_file = Path.home() / &#x27;.claude&#x27; / &#x27;hooks&#x27; / &#x27;debug.log&#x27;

with open(log_file, &#x27;a&#x27;) as f:
    f.write(f&quot;调试信息\n&quot;)
```

UserPromptSubmit Hook：通过stdout JSON的additionalContext字段

```
import json
output = {
    &quot;hookSpecificOutput&quot;: {
        &quot;hookEventName&quot;: &quot;UserPromptSubmit&quot;,
        &quot;additionalContext&quot;: &quot;要显示给用户的内容&quot;
    }
}
print(json.dumps(output))
```

**PreToolUse Hook**：通过stdout JSON返回决策

```
import json
decision = {
    &quot;hookSpecificOutput&quot;: {
        &quot;hookEventName&quot;: &quot;PreToolUse&quot;,
        &quot;permissionDecision&quot;: {
            &quot;decision&quot;: &quot;deny&quot;,
            &quot;message&quot;: &quot;拒绝原因（用户可见）&quot;
        }
    }
}
print(json.dumps(decision))
```

**stderr输出**：仅用于调试，不会显示在Claude Code界面

```
print(&quot;调试信息&quot;, file=sys.stderr)  # 只在终端可见，用户看不到
```

Q13: 如何返回决策（PreToolUse）？

使用stdout输出JSON：

```
print(json.dumps({&quot;decision&quot;: &quot;deny&quot;, &quot;message&quot;: &quot;原因&quot;}))
```

Q14: 脚本报错会影响Claude Code吗？

不会！Hook脚本出错不会阻止Claude Code运行，只是该Hook功能失效。

Q15: 如何处理Windows/macOS/Linux兼容性？

```
import platform
system = platform.system()

if system == &#x27;Windows&#x27;:
    # Windows特定代码
elif system == &#x27;Darwin&#x27;:  # macOS
    # macOS特定代码
else:  # Linux
    # Linux特定代码
```

### 高级问题

Q16: Hook可以修改Claude的输出吗？

不能直接修改。但可以：

+   PostToolUse后修改文件内容

+   UserPromptSubmit修改用户输入

Q17: 多个Hook的执行顺序是什么？

按配置文件中的顺序依次执行。

Q18: Hook可以调用Claude API吗？

可以，但要注意：

+   会消耗额外Token

+   可能导致无限循环

+   建议设置调用限制

Q19: 如何在团队中共享Hook配置？

将 .claude/ 目录加入Git版本控制：

```
git add .claude/settings.json
git add .claude/hooks/
git commit -m &quot;Add Claude Code hooks&quot;
```

Q20: Hook有性能影响吗？

有一定影响：

+   每次工具调用都会触发Hook

+   复杂脚本会增加延迟

+   建议优化脚本性能，设置合理timeout

---

## 附录A：配置速查表

### Hook类型速查

| Hook类型 | 触发时机 | 输入格式 | 输出格式 | 可阻止 | 重要 |
| --- | --- | --- | --- | --- | --- |
| UserPromptSubmit | 用户输入后 | JSON | 文本（注入上下文） | V |  |
| PreToolUse | 工具调用前 | JSON | JSON决策 | V | ⭐ |
| PostToolUse | 工具调用后 | JSON | 无 | X | ⭐ |
| Notification | 通知发送时 | JSON | 无 | X |  |
| SessionStart | 会话开始 | 无 | 无 | X |  |
| SessionEnd | 会话结束 | 无 | 无 | X |  |
| Stop | AI停止 | JSON | 无 | X |  |
| SubagentStart 🆕 | 子代理启动 | JSON | 无 | X |  |
| SubagentStop 🆕 | 子代理停止 | JSON | 无 | X |  |
| PermissionRequest 🆕 | 权限请求 | JSON | JSON决策 | V |  |
| PreCompact 🆕 | 上下文压缩前 | JSON | 无 | X |  |
| ConfigChange 🆕 | 配置变更 | JSON | 无 | X |  |
| TeammateIdle 🆕 | 队友空闲 | JSON | 无 | X |  |
| WorktreeCreate | 工作树创建 | JSON | 无 | X |  |
| WorktreeRemove | 工作树删除 | JSON | 无 | X |  |
| StopFailure 🆕 | API异常停止 | JSON | 无 | X |  |
| PostCompact 🆕 | 上下文压缩后 | JSON | 无 | X |  |
| InstructionsLoaded 🆕 | 指令文件加载 | JSON | 无 | X |  |
| Elicitation 🆕 | MCP请求输入 | JSON | 无 | X |  |
| ElicitationResult 🆕 | MCP输入完成 | JSON | 无 | X |  |

### 常用工具名速查

| 工具名 | 功能 | 常用Hook | 重要 |
| --- | --- | --- | --- |
| Write | 写入文件 | PostToolUse格式化 | ⭐ |
| Edit | 编辑文件 | PostToolUse备份 |  |
| Read | 读取文件 | PreToolUse权限控制 |  |
| Bash | 执行命令 | PreToolUse危险命令拦截 | ⭐ |
| Glob | 文件搜索 | - |  |
| Grep | 内容搜索 | - |  |
| WebSearch | 网络搜索 | - |  |

### Decision值速查

PreToolUse 新版API（hookSpecificOutput.permissionDecision，推荐）：

| 值 | 含义 | 工具执行 | 重要 |
| --- | --- | --- | --- |
| &quot;allow&quot; | 允许，绕过权限系统 | V | ⭐ |
| &quot;deny&quot; | 拒绝，原因反馈给Claude | X | ⭐ |
| &quot;ask&quot; | 暂停，询问用户 | ? |  |
| 无输出 | 默认允许 | V |  |

PreToolUse 旧版API（decision，已废弃但仍支持）：

| 旧版值 | 等同于新版 |
| --- | --- |
| &quot;approve&quot; | &quot;allow&quot; |
| &quot;block&quot; | &quot;deny&quot; |

PostToolUse / UserPromptSubmit：

| 值 | 含义 |
| --- | --- |
| &quot;block&quot; | 阻止/提供反馈 |
| 无输出 | 默认允许 |

---

## 附录B：完整脚本模板

### Python脚本模板

```
#!/usr/bin/env python3
&quot;&quot;&quot;
Hook名称 - 功能描述
&quot;&quot;&quot;
import sys
import json

def main():
    # 读取输入
    try:
        input_data = json.loads(sys.stdin.read())
    except json.JSONDecodeError:
        sys.exit(0)

    tool_name = input_data.get(&#x27;tool_name&#x27;, &#x27;&#x27;)
    tool_input = input_data.get(&#x27;tool_input&#x27;, {})

    # 你的逻辑
    # ...

    # 输出到Claude Code界面
    print(&quot;信息&quot;, file=sys.stderr)

    # 如果是PreToolUse，输出决策
    # print(json.dumps({&quot;decision&quot;: &quot;allow&quot;}))

if __name__ == &#x27;__main__&#x27;:
    main()
```

### Bash脚本模板

```
#!/bin/bash
# Hook名称 - 功能描述

# 读取stdin
input_json=$(cat)

# 使用Python解析JSON
tool_name=$(echo &quot;$input_json&quot; | python3 -c &quot;import sys,json; print(json.load(sys.stdin).get(&#x27;tool_name&#x27;,&#x27;&#x27;))&quot;)

# 你的逻辑
# ...

# 输出日志
echo &quot;信息&quot; &gt;&amp;2

exit 0
```

---

## 附录C：参考资源

### 官方文档

+   Claude Code Hooks官方文档

+   [Claude Code官方指南](https://docs.anthropic.com/en/docs/claude-code)

### 社区资源

+   ClaudeLog Hooks教程

+   GitButler Claude Code Hooks集成

+   Awesome Claude Code

### 相关课程

+   第3部分：Commands系统 - Slash命令开发

+   第4部分：MCP集成 - 外部工具连接

+   第6部分：Skills定制 - 技能包开发

---

## 学习总结

通过本课学习，你已经掌握：

1.  **Hooks核心概念**：理解Hook是什么、为什么需要、能做什么

1.  **15种Hook类型**：PreToolUse、PostToolUse、UserPromptSubmit、Notification、Stop、SessionStart、SessionEnd、SubagentStart、SubagentStop、PermissionRequest、PreCompact、ConfigChange、TeammateIdle、WorktreeCreate、WorktreeRemove全部类型

1.  **配置方法**：settings.json配置格式、Matcher语法、timeout设置

1.  **实战场景**：Git自动化、代码格式化、文件保护、质量检查

1.  **故障排查**：常见问题诊断和解决方法

1.  **安全意识**：Hook安全风险和最佳实践

**下一步建议**：

1.  从第二部分的简单示例开始实践

1.  根据你的项目需求，选择合适的Hook类型

1.  参考第四部分的实战案例，逐步构建自己的自动化工作流

1.  遇到问题查阅第五部分和第六部分

**记住**：Hooks是自动化的终极武器，合理使用可以让你的开发效率翻倍！

---

**文档版本****：v1.****1（**Critical Thinking审查修正版）

最后更新：2026年2月25日

作者：老金

### v1.1 修正内容（2025-12-23）

| 修正项 | 问题 | 修正后 |
| --- | --- | --- |
| UserPromptSubmit输入格式 | 错误描述为&quot;纯文本&quot; | 修正为JSON格式 |
| UserPromptSubmit示例脚本 | 直接读文本导致脚本无法工作 | 使用json.loads解析 |
| 速查表UserPromptSubmit行 | 输入格式列写&quot;纯文本&quot;与正文矛盾 | 修正为JSON，输出列修正为&quot;文本（注入上下文）&quot; |
| Notification输入格式 | 字段不完整 | 添加完整字段说明 |
| Stop Hook输入格式 | 错误声称有reason字段 | 删除不存在的字段 |
| 环境变量说明 | 错误声称有CLAUDE_SESSION_ID | 明确session_id在JSON中 |
| Windows通知脚本 | NotifyIcon不稳定 | 改用BurntToast/原生Toast |