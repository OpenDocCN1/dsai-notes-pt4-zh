
# Preference 偏好设置

> 来源：[https://kcn9ut4m7icc.feishu.cn/docx/KxyQdpKjnoaRbsx4UOvc0OBYnVg](https://kcn9ut4m7icc.feishu.cn/docx/KxyQdpKjnoaRbsx4UOvc0OBYnVg)

Learn how to customize Chat, Composer, Terminal, and Editor features in Cursor’s settings menu

学习如何在 Cursor 的设置菜单中自定义 Chat、Composer、Terminal 和 Editor 功能

You can set preferences for certain features from Cursor Settings

您可以从光标设置中设置某些功能的偏好设置

> Preferences are stored locally and are not synchronized across different machines

> 首选项存储在本地，且不会在不同机器之间同步

## Chat & Composer 聊天 & Composer

### Agent Stickiness 代理粘性

If enabled, your selection between normal and Agent mode will persist across new Composer conversations.

如果启用，您在正常模式和代理模式之间的选择将在新的 Composer 对话中保持不变。

![](img/4bca91ec82ab6952228487650170ee7b.png)

### Auto-Scroll to Bottom 自动滚动至底部

Automatically scrolls to the bottom of the Composer pane when a new message is generated.

自动滚动到 Composer 面板底部，当生成新消息时。

#### Auto-Apply to Files Outside Context 自动应用于上下文外的文件

Allows Composer to automatically apply changes to files outside your current context.

允许 Composer 自动将更改应用到当前上下文之外的文件。

#### Collapse Input Box Pills in Pane or Editor 折叠面板或编辑器中的输入框药丸

Collapses pills in the Composer pane or the editor input box to save space.

折叠 Composer 面板或编辑输入框中的药丸以节省空间。

#### Render Pills Instead of Blocks 渲染药丸而非方块

Collapses Composer code blocks into pills instead of rendering them as code blocks.

将 Composer 代码块折叠成药丸形状，而不是以代码块的形式渲染。

#### Iterate on Lints 迭代 Lints

If there are linter errors, Composer will try to fix them iteratively.

如果存在代码检查错误，Composer 将尝试迭代地修复它们。

### Codebase Indexing 代码库索引

#### Index New Files by Default 默认按索引新文件

When enabled, new files will be indexed automatically by default. If disabled, you can still index your folders by pressing the `Compute Index` button.

当启用时，新文件将默认自动索引。如果禁用，您仍然可以通过按 `Compute Index` 按钮来索引您的文件夹。

Indexes are auto-generated only for folders with fewer than 10,000 files.

索引仅自动生成少于 10,000 个文件的文件夹。

#### Git Graph Relationships Git 图关系

When enabled, Cursor will index your `git` history to understand relationships between files.

当启用时，Cursor 将索引您的 Git 历史记录，以了解文件之间的关系。

Code and commit messages are stored locally, while metadata about commits (`SHA` values, number of changes, and obfuscated filenames) is stored in the cloud.

代码和提交信息存储在本地，而关于提交的元数据（`SHA` 值、变更数量和混淆后的文件名）存储在云端。

### Editor 编辑

#### Chat/Edit Tooltip 聊天/编辑提示信息

If enabled, shows a chat/edit tooltip near highlighted code in your editor.

如果启用，将在您的编辑器中突出显示的代码附近显示聊天/编辑提示。

#### Auto Parse Links 自动解析链接

If enabled, Cursor automatically parses links and adds them to the context.

如果启用，Cursor 会自动解析链接并将它们添加到上下文中。

#### Auto-Select for `Ctrl/Cmd + K` 自动选择 `Ctrl/Cmd + K`

If enabled, Cursor automatically selects regions for inline code editing.

如果启用，Cursor 会自动选择区域进行内联代码编辑。

#### Themed Diffs 主题差异

If enabled, Cursor uses themed background colors for inline diffs in your editor.

如果启用，Cursor 将在您的编辑器中使用主题背景颜色显示内联差异。

### Terminal 终端

#### Show Terminal Hover Hint 显示终端悬停提示

If enabled, shows a hover hint in the terminal with the command to `Add to Chat` or `Debug with AI`.

如果启用，将在终端中显示悬停提示，显示“添加到聊天”或“用 AI 调试”的命令。

#### Use Preview Box 使用预览框

If enabled, Cursor uses a preview box in the terminal to show the output of `Ctrl/Cmd + K`. If disabled, responses stream directly into the terminal.

如果启用，Cursor 会在终端中使用预览框来显示 `Ctrl/Cmd + K` 的输出。如果禁用，响应将直接流入终端。

![](img/589735487c1702c023b20a353059622e.png)
