
# 1. 卸载旧版本

```
npm uninstall -g @anthropic-ai/claude-code
```

```
nvm use 22
```

```
npm uninstall -g @anthropic-ai/claude-code
```

# 2. 然后运行原生安装

```
claude install
```

# 3. 清理nvm shims

```
rm ~/.asdf/shims/claude  # 如果你用asdf
```

## C. 推荐学习资源

Claude Code官方文档：

+   英文：`https://docs.anthropic.com/en/docs/claude-code`
+   中文：`https://code.claude.com/docs/en/`

Anthropic API文档：

+   `https://docs.anthropic.com/`

### 终端使用教程：

+   Windows Terminal：`https://learn.microsoft.com/zh-cn/windows/terminal/`
+   macOS终端：`https://support.apple.com/zh-cn/guide/terminal/`

### 社区支持：

+   Claude Discord：`https://discord.gg/anthropic`
+   GitHub Discussions：`https://github.com/anthropics/claude-code/discussions`

---

课程制作：老金

最后更新：2026年2月（原生安装版本）

版本：`V3.0`（原生安装重大更新版）

许可：本课程采用`CC BY-NC-SA 4.0`许可

---

## 版本更新日志

### V3.0 重大更新（2026-02-02）

| 更新项 | 变化说明 |
| --- | --- |
| 安装方式 | 从`npm`安装切换到原生安装 |
| `Node.js` | 不再需要`Node.js`和`npm` |
| 安装时间 | 从40分钟缩短到5分钟 |
| 自动更新 | 新增后台自动更新功能 |
| 系统要求 | 简化系统要求检查清单 |
| FAQ | 新增8个原生安装相关常见问题 |
| 迁移指南 | 新增从`npm`迁移到原生的完整指南 |

### V2.1 修正内容（2025-12-23）

| 修正项 | 问题 | 修正后 |
| --- | --- | --- |
| API测试模型名称 | `claude-sonnet-4-20250514` 格式和日期错误 | 修正为 `claude-sonnet-4-6` |

> 💡 审查方法：基于官方Claude API文档验证模型名称准确性。
