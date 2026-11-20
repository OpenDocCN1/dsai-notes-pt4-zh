
# Composer  编辑器

> 来源：[https://kcn9ut4m7icc.feishu.cn/docx/UeMed85DEowgvRxZ6EvcumjHnhN](https://kcn9ut4m7icc.feishu.cn/docx/UeMed85DEowgvRxZ6EvcumjHnhN)

Unified AI interface that combines `Ask`, `Edit`, and `Agent` modes to help write, edit, and understand code directly in your editor

统一 AI 界面，结合询问、编辑和代理模式，帮助您在编辑器中直接编写、编辑和理解代码

Cursor’s unified AI interface combines different capabilities in one seamless experience. Use `⌘I` to open it, and `⌘N` to create a new conversation. Switch between modes using the `mode picker` in the input box.

光标统一 AI 界面将不同功能整合在一个无缝体验中。使用 `⌘I` 打开它，使用 `⌘N` 创建新的对话。使用输入框中的 `mode picker` 切换模式。

![](img/06a489687d704d922b5a5d02af21015b.png)

## Modes  模式

The interface offers three modes that you can select from the `mode picker`:

界面提供三种模式，您可以从 `mode picker` 中进行选择：

![](img/2de5b0c3eae1e8254fa3f828d2170a7c.png)

You can switch between modes during a conversation using the `mode picker` or `⌘.` shortcut. This flexibility lets you adapt to your current needs - from asking questions to making changes to using advanced tools.

您可以在对话中使用 `mode picker` 或 `⌘.` 快捷键在模式之间切换。这种灵活性让您能够根据当前需求进行调整——从提问到修改，再到使用高级工具。

## Context  上下文

You can use [`@-symbols`](https://docs.cursor.com/context/@-symbols/basic) to include relevant context in your prompts. The interface will automatically suggest relevant context based on your query.

您可以使用 `@` 符号在提示中包含相关上下文。界面将根据您的查询自动建议相关上下文。

### Autocontext (Beta)  自动上下文（Beta）

Cursor can automatically include relevant code in your conversations using embeddings and a custom model. Instead of manually selecting context with `@-symbols`, it analyzes your prompt and includes the most relevant code from your codebase. Enable this feature in `Settings > Features > Autocontext`.

光标可以自动使用嵌入和自定义模型将相关代码包含到您的对话中。无需手动使用 `@` 符号选择上下文，它将分析您的提示并包含来自您的代码库中最相关的代码。在 `Settings > Features > Autocontext` 中启用此功能。

![](img/a03c0cde76ab6b1631fa27279a38e405.png)

## Generating & Applying Changes 生成并应用更改

When code changes are suggested:

当建议代码更改时：

+   Review them in the `diff view`

查看 `diff view` 中的它们

+   Accept or reject changes with the buttons provided

接受或拒绝更改，请使用提供的按钮

+   Use `checkpoints` to undo if needed

使用 `checkpoints` 在需要时撤销

## Checkpoints  检查点

For every iteration a checkpoint is created. You can return to any previous version by clicking on `checkout` near that checkpoint. This is handy if you don’t like the current changes and want to revert to an earlier state.

每次迭代都会创建一个检查点。您可以通过点击该检查点附近的 `checkout` 返回到任何之前的版本。如果您不喜欢当前的更改并希望恢复到早期状态，这会很有用。

![](img/f36809e723708553196a76815897311f.png)

## History  历史

Access previous conversations through the history. Open it from the history icon to the right of `Cursor Tab`. You’ll see a list of past conversations which you can revisit, rename, or remove.

通过历史记录访问之前的对话。从“`Cursor Tab`”右侧的历史图标打开。您将看到一个过去对话的列表，您可以重新访问、重命名或删除。

Open with `⌘+⌥+L` or `Ctrl+Alt+L` when the interface is focused.

打开时使用 `⌘+⌥+L` 或 `Ctrl+Alt+L` ，当界面聚焦时。

![](img/cecf21dfbc89f6b08863f13cbea27d68.png)

## Layout  布局

+   `Pane`: A sidebar with the interface on the left and your code editor on the right.

`Pane`：左侧是界面，右侧是您的代码编辑器。

+   `Editor`: A single editor window, similar to viewing code normally. You can move it around, split it, or even place it in a separate window.

`Editor`：单个编辑窗口，类似于正常查看代码。您可以移动它，拆分它，甚至将其放置在单独的窗口中。

+   `Floating`: A draggable window that you can position where you like

`Floating`：一个可拖动的窗口，您可以将其放置在您喜欢的位置

You can change this from the menu > Open as `layout`

您可以从菜单 > 以 `layout` 打开进行更改

## Iterate on lints  迭代 lint

The interface attempts to fix `linting` issues in generated code for most programming languages. If `lint` errors are detected, it will try to fix them automatically when this feature is enabled. Currently, only one iteration is supported.

界面尝试修复大多数编程语言生成代码中的 `lint` 错误。如果检测到 `lint` 错误，当此功能启用时，它将尝试自动修复它们。目前，仅支持一次迭代。

![](img/722114dffae3af8935e378f2fd9c5ec0.png)

> Some languages (like `Rust`) require files to be saved before `lint` errors appear, which may limit this feature’s effectiveness in all languages.

> 一些语言（如 `Rust`）要求在出现 `lint` 错误之前保存文件，这可能会限制此功能在所有语言中的有效性。

## FAQ  常见问题解答

### What’s the difference between the modes? 模式之间有什么区别？

`Ask` mode helps you understand and explore code. Use it to ask questions, get explanations, and learn about your codebase.

`Ask` 模式帮助您理解和探索代码。使用它来提问、获取解释并了解您的代码库。

`Edit` mode focuses on making single-turn edits to your code. It provides a workspace where you can make precise changes to your files.

`Edit` 模式专注于对您的代码进行单次编辑。它提供了一个工作区，您可以在其中对文件进行精确的修改。

`Agent` mode (default) combines both capabilities with additional tools and reasoning abilities for handling complex tasks.

`Agent` 模式（默认）结合了两种功能，并增加了处理复杂任务的工具和推理能力。

### How are long conversations handled? 如何处理长对话？

For long conversations, Cursor summarizes earlier messages with smaller models like `cursor-small` and `gpt-4o-mini` to keep responses fast and relevant.

Cursor –作曲家 对于长时间对话，Cursor 使用类似 `cursor-small` 和 `gpt-4o-mini` 的小型模型来总结之前的消息，以保持回复快速且相关。

This approach helps ensure that even extended conversations remain responsive and coherent, without losing track of key details from earlier exchanges.

这种方法有助于确保即使是长时间的对话也能保持响应和连贯，不会遗漏之前交流中的关键细节。

### Can I access my conversation history on another computer? 我可以在另一台电脑上访问我的聊天记录吗？

Conversation history is stored locally on your computer and is not stored on Cursor’s servers or tied to your Cursor account.

会话历史存储在您的计算机上，不会存储在 Cursor 的服务器上，也不会与 Cursor 账户关联。

This means if you switch to a different computer, you won’t have access to your previous history. You can only access your history on the computer where it was originally created.

这意味着如果您切换到另一台电脑，您将无法访问您之前的历史记录。您只能在创建历史记录的原电脑上访问历史记录。
