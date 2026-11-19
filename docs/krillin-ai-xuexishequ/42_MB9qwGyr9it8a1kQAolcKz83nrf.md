
# Overview  概览

> 来源：[https://kcn9ut4m7icc.feishu.cn/docx/EiHSdeA8CofXBjxFaTycZOsknFh](https://kcn9ut4m7icc.feishu.cn/docx/EiHSdeA8CofXBjxFaTycZOsknFh)

Chat feature that uses AI to answer code questions with smart context and file references in your editor

聊天功能，使用 AI 在编辑器中智能回答代码问题，并参考文件引用

Chat lets you ask questions or solve problems in your codebase with the most capable language models, all in your editor.

Chat 允许您在代码库中使用最强大的语言模型提问或解决问题，所有操作都在您的编辑器中完成。

![](img/29144c41b7047012424c0ce473743d98.png)

For language models to give good answers, they need to know specific things that are relevant to your codebase — context.

为了语言模型能够给出好的答案，它们需要了解与您的代码库相关的一些具体信息——上下文。

Cursor has several built-in features to provide context in chat, such as automatically including context across your entire codebase, searching the web, indexing documentation, and user-specified references to code blocks. These features are built to eliminate the tedious copy-pasting otherwise necessary for working with language models on code.

光标具有多个内置功能，以在聊天中提供上下文，例如自动包括整个代码库的上下文、搜索网络、索引文档以及用户指定的代码块引用。这些功能旨在消除与代码语言模型工作相关的繁琐复制粘贴操作。

By default, Chat is in the AI pane, which is on the opposite side of your primary sidebar. You can toggle the AI pane by pressing `Ctrl/⌘ + L`, which focuses onto the chat when opened. To submit your query, press `Enter`.

默认情况下，聊天位于 AI 面板中，该面板位于您主侧边栏的对面。您可以通过按 `Ctrl/⌘ + L` 来切换 AI 面板，打开时将焦点置于聊天上。要提交您的查询，请按 `Enter`。

## User and AI Messages  用户和 AI 消息

User messages contain the text you type, along with the context you’ve referenced. You can go back to any previous user messages to edit and rerun your queries. This will overwrite any messages after that and regenerate new ones.

用户消息包含您输入的文本以及您引用的上下文。您可以返回到任何之前的用户消息进行编辑和重新运行查询。这将覆盖该消息之后的所有消息并重新生成新的消息。

[AI messages](https://docs.cursor.com/chat/apply) are the responses generated from the AI model you’ve picked. They are paired with the user message before them. AI messages may contain parsed code blocks which can be added to your codebase with instant apply.

AI 消息是从您选择的 AI 模型生成的响应。它们与之前的用户消息配对。AI 消息可能包含可即时应用到您的代码库中的解析代码块。

All user/AI messages together in the same thread are called a chat thread, and each chat thread is saved in your chat history.

所有用户/人工智能的消息都在同一个线程中，这种线程被称为聊天线程，每个聊天线程都会保存在您的聊天历史中。

## Chat History  聊天记录

By pressing the “Previous Chats” button on the top right of the AI pane, or by pressing `Ctrl/⌘ + Alt/Option + L`, you can see the chat history. You can click on any chat thread to go back and see the messages that make up that thread, and you can also modify the title of the thread by clicking the pen icon, or delete the thread by clicking the garbage can icon upon hovering over the thread in the history.

通过点击 AI 面板右上角的“上一轮对话”按钮，或按 `Ctrl/⌘ + Alt/Option + L` ，您可以查看聊天历史。您可以点击任何聊天线程返回并查看构成该线程的消息，您还可以通过点击笔图标修改线程标题，或者通过在历史中悬停线程并点击垃圾箱图标来删除线程。

The title of a Cursor thread is just the first few words of the first user message.

光标线程的标题只是第一条用户消息的前几个词。

## Default Context  默认上下文

By default, Chat includes the current file as context. You can submit a query without including any context by removing the current file pill from the message. As you type, you can see what will be included in context in the pills above the input box.

默认情况下，聊天包括当前文件作为上下文。您可以通过从消息中移除当前文件药丸来提交不包含任何上下文的查询。当您键入时，您可以看到将在输入框上方的药丸中包含哪些上下文。

## Adding Context  添加上下文

By default, user messages will contain the text you type, along with the context you’ve referenced. You can add more custom context to each bubble with `@` symbols, and by default, the current viewing file will be used as context as well in the user message.

默认情况下，用户消息将包含您输入的文本以及您引用的上下文。您可以通过使用`@`符号为每个气泡添加更多自定义上下文，并且默认情况下，当前查看的文件也将作为上下文用于用户消息。

See the [`@` symbols](https://docs.cursor.com/context/@-symbols/@-files) pages for more information.

查看 `@` 符号页面以获取更多信息。

## AI Fix in Chat  AI 修复聊天

A convenient feature to fix linter errors in your codebase is to use the AI fix in chat. To do this, hover over the error in the editor, and click the blue AI fix button that shows up.

方便的功能，用于修复代码库中的 linter 错误，是在聊天中使用 AI 修复。为此，在编辑器中悬停在错误上，并点击出现的蓝色 AI 修复按钮。

The keyboard shortcut for this would be to do `Ctrl/⌘ + Shift + E`.

键盘快捷键为此将是按 `Ctrl/⌘ + Shift + E` 。

![](img/a8934b6234777c4c10a2c9240849aa9e.png)
