
# Cursor 教程

> 来源：[https://v11enp9ok1h.feishu.cn/docx/E6qTdVrmSoXEt3xvithclD29nwd](https://v11enp9ok1h.feishu.cn/docx/E6qTdVrmSoXEt3xvithclD29nwd)

![](img/204b9a78aad3f74e9f5475ed89f53e97.png)

Cursor 编辑器是一款专为现代开发者设计的 AI 驱动的代码编辑器。

Cursor 通过集成先进的大型语言模型（如 GPT-4、Claude 3.5）来理解和生成代码。

Cursor 是一款基于 Visual Studio Code (VS Code) 的现代化代码编辑器，它继承了 VS Code 的强大功能和扩展性，并在此基础上加入了 AI 辅助编程功能，旨在提升开发者的效率和代码质量。

## Cursor 特点

Cursor 基于 [VS Code](https://www.runoob.com/vscode/vscode-tutorial.html) 开发，因此它拥有 VS Code 的所有优点，例如：

+ 跨平台支持：支持 Windows、macOS 和 Linux 系统。

+ 丰富的插件生态系统：可以使用 VS Code 的庞大插件库来扩展功能。

+ 强大的代码编辑功能：支持语法高亮、代码补全、代码片段、代码导航等功能。

+ 集成终端：方便开发者直接在编辑器内运行命令。

对于熟悉 VS Code 的用户来说，Cursor 的学习成本非常低，可以快速上手。

Cursor 的最大亮点在于其内置的 AI 辅助编程功能，它可以帮助开发者：

+ 代码补全：根据上下文和代码风格，提供更智能、更准确的代码补全建议。

+ 代码生成：根据自然语言描述或代码片段，自动生成代码，例如生成函数、类、测试用例等。

+ 代码解释：解释代码的功能和逻辑，帮助开发者理解复杂的代码。

+ 代码调试：帮助开发者查找和修复代码中的错误。

这些 AI 功能可以显著提升开发效率，减少重复性工作，让开发者更专注于创造性工作。

## 核心特征

+ AI 原生设计：从底层架构就融入了 AI 能力，而非后期添加的插件

+ 智能代码生成：通过自然语言描述快速生成代码片段

+ 上下文感知：深度理解项目结构和代码关系

+ 实时协助：在编程过程中提供即时的建议和优化

+ 多模型支持：集成了多种先进的大语言模型

### Cursor 与传统 IDE 的区别

#### 核心差异总结

## 设置

如果您之前使用 VS Code，可以轻松将您的扩展、主题、设置和快捷键一键导入到 Cursor 中。

只需进入 `Cursor Settings > General > Account`，选择 `import` 导入配置文件，即可完成迁移。

![](img/469ecfe86060ae1c3acc6681acde68f0.png)

Cursor 会定期同步 VS Code 的最新版本，确保您始终使用最新的功能和优化。

### 为什么 Cursor 是一个独立应用，而不是 VS Code 的插件？

作为独立应用，Cursor 可以更深度地优化编辑器界面，并实现更强大的 AI 集成，例如，Cursor Tab 和 `CMD-K` 等功能，无法通过插件形式在现有编程环境中实现。

### 如何设置 Cursor？

+ 点击右上角的齿轮按钮，或按下快捷键 `Ctrl/⌘ + Shift + J`，即可打开 Cursor 的专属设置面板。

+ 您也可以通过 `Ctrl/⌘ + Shift + P` 输入 `Cursor Settings` 来打开。

![](img/16572b41c344371806019ff979a87112.png)

### 为什么 Cursor 的活动栏是水平的？

默认情况下，Cursor 的活动栏是水平放置的，这是为了节省空间，方便集成聊天功能。

![](img/afbe5d11bc460e06a4e622ccdf0b6f0d.png)

如果您更喜欢传统的垂直活动栏，可以按照以下步骤调整：

+ 打开 Cursor 设置。

+ 搜索并找到 `workbench.activityBar.orientation`，将其值改为 `vertical`。

+ 重启 Cursor 即可生效。

![](img/445447bad9e386e4e7676c7d51489b49.png)

改为 `vertical` 后重启效果：

![](img/e10fbf57f099da72b57f5adef0d23c07.png)
