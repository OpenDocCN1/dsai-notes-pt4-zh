
```python
#!/usr/bin/env python3
"""
PostToolUse Hook - 自动备份
编辑重要文件后自动创建.bak备份
"""
import sys
import json
import shutil
from pathlib import Path
from datetime import datetime

# 需要备份的目录
BACKUP_DIRS = ['config', 'src', 'docs', '.claude']

def main():
    try:
        input_data = json.loads(sys.stdin.read())
    except json.JSONDecodeError:
        return

    tool_name = input_data.get('tool_name', '')
    tool_input = input_data.get('tool_input', {})
    file_path = tool_input.get('file_path', '')

    # 只处理Edit工具
    if tool_name != 'Edit':
        return

    # 检查是否在需要备份的目录中
    should_backup = any(dir_name in file_path for dir_name in BACKUP_DIRS)

    if should_backup and Path(file_path).exists():
        # 创建备份
        timestamp = datetime.now().strftime('%Y%m%d_%H%M%S')
        backup_path = f"{file_path}.{timestamp}.bak"

        try:
            shutil.copy2(file_path, backup_path)
            print(f"[Backup] ✅ 已创建备份: {Path(backup_path).name}", file=sys.stderr)
        except Exception as e:
            print(f"[Backup] ⚠️ 备份失败: {e}", file=sys.stderr)


if __name__ == '__main__':
    main()
```

### 3.3 UserPromptSubmit（用户提示词提交）

> **一句话理解**：`UserPromptSubmit`就像"邮件发送前的自动补充"，可以在用户输入发送给Claude之前自动增强或过滤。

#### 触发时机

在用户提交提示词后，Claude处理之前。

#### 输入参数（通过`stdin`的JSON）

> ⚠️ **重要**：`UserPromptSubmit`的输入是JSON格式，不是纯文本！

```json
{
  "session_id": "abc123",
  "transcript_path": "/Users/.../.claude/projects/.../session.jsonl",
  "cwd": "/your/project/path",
  "permission_mode": "default",
  "hook_event_name": "UserPromptSubmit",
  "prompt": "请帮我写一篇关于AI的文章"
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `session_id` | string | 当前会话ID |
| `prompt` | string | 用户输入的原始提示词 |
| `cwd` | string | Claude Code的工作目录 |
| `permission_mode` | string | 权限模式（default/plan/bypassPermissions等） |
| `hook_event_name` | string | 事件类型标识 |

#### 输出格式

Hook的`stdout`输出会被添加到AI上下文（作为系统消息注入）。

**方式1：直接输出文本**（会作为额外上下文添加）

```
## 写作要求
- 字数：1500字
- 风格：老金式接地气风格
- 包含实战案例
```

**方式2：输出JSON格式**（更多控制）

```json
{
  "continue": true,
  "suppressOutput": false
}
```

#### 完整示例：写作规范自动追加

**场景**：检测到写作任务时，自动追加写作规范

**脚本**`.claude/hooks/user-prompt-enhance.py`：

```python
#!/usr/bin/env python3
"""
UserPromptSubmit Hook - 提示词自动增强
检测到特定任务时自动追加相关规范

【重要】：输入是JSON格式，必须先解析！
"""
import sys
import json

# 从stdin读取JSON输入
try:
    input_data = json.loads(sys.stdin.read())
except json.JSONDecodeError:
    # JSON解析失败，直接退出
    sys.exit(0)

# 获取用户输入的提示词
user_input = input_data.get('prompt', '').strip()

# 如果没有prompt字段，直接退出
if not user_input:
    sys.exit(0)

# 过滤简单回复（不需要增强）
simple_responses = ['好的', '是的', '继续', 'ok', 'yes', 'no', '确认', '取消']
if user_input.lower() in simple_responses or len(user_input) < 5:
    # 不需要增强，不输出任何内容
    sys.exit(0)

# 过滤斜杠命令
if user_input.startswith('/'):
    sys.exit(0)

# 检查是否是写作任务
writing_keywords = ['写', '文章', '生成', '创作', 'write', 'article', '内容']
is_writing_task = any(kw in user_input.lower() for kw in writing_keywords)

if is_writing_task:
    # 输出额外上下文（会添加到AI上下文中）
    enhancement = """
---
## 写作规范提醒（Hook自动注入）
1. **风格**：接地气、说人话，避免AI腔
2. **结构**：开头金句 -> 核心要点 -> 实战案例 -> 总结升华
3. **字数**：1500-2000字
4. **检查**：完成后运行 /pre-check 进行质量检查
---
"""
    print(enhancement)
    print(f"[Hook] 已为写作任务注入规范", file=sys.stderr)

sys.exit(0)
```

> 💡 **注意**：

+ 输入是JSON，必须用`json.loads()`解析
+ 用户原始输入在`prompt`字段中
+ 通过`stdout`输出JSON格式的`additionalContext`字段来注入上下文
+ `stderr`仅用于调试，不会显示在Claude Code界面

**配置**：

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python .claude/hooks/user-prompt-enhance.py",
            "timeout": 5
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
2. **结构**：开头金句 -> 核心要点 -> 实战案例 -> 总结升华
3. **字数**：1500-2000字
4. **检查**：完成后运行 /pre-check 进行质量检查
---
```

### 3.4 Notification（通知）

> **一句话理解**：`Notification` Hook就像"消息推送处理器"，在Claude发送通知时触发。

#### 触发时机

当Claude Code需要请求用户权限或提示输入空闲超过60秒时。

#### 输入参数（通过`stdin`的JSON）

```json
{
  "session_id": "abc123",
  "transcript_path": "/Users/.../.claude/projects/.../session.jsonl",
  "cwd": "/your/project/path",
  "hook_event_name": "Notification",
  "message": "Claude is waiting for your input"
}
```

| 字段 | 类型 | 说明 |
| --- | --- | --- |
| `session_id` | string | 当前会话ID |
| `transcript_path` | string | 会话记录文件路径 |
| `cwd` | string | 工作目录 |
| `hook_event_name` | string | 事件类型标识（固定为`"Notification"`） |
| `message` | string | 通知消息内容（`Notification`特有字段） |

> ⚠️ **注意**：根据官方实现，`Notification` Hook没有`title`字段，只有`message`字段。

#### 完整示例：桌面通知Hook

**场景**：将Claude的通知转发到系统桌面通知

**脚本**`.claude/hooks/notification-desktop.py`：
