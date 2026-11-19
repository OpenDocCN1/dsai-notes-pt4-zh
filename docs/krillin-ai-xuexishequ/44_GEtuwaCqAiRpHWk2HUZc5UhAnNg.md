
# With Codebase 与代码库

> 来源：[https://kcn9ut4m7icc.feishu.cn/docx/BAo1d0Covo0vfTxVXWXcN0gjnqg](https://kcn9ut4m7icc.feishu.cn/docx/BAo1d0Covo0vfTxVXWXcN0gjnqg)

Learn how Chat searches codebases using default search, embeddings, and advanced `@Codebase` queries.

了解如何使用默认搜索、嵌入和高级 `@Codebase` 查询在代码库中进行搜索。

## Default Codebase Chat 默认代码库聊天

[If a co](https://docs.cursor.com/context/codebase-indexing)debase isn’t indexed, Chat will first attempt to compute a few search queries to be used to search across your codebase. For better accuracy, it’s recommended to use embeddings search.

如果代码库未索引，Chat 将首先尝试计算一些搜索查询，用于在您的代码库中进行搜索。为了提高准确性，建议使用嵌入搜索。

## Embeddings Search 嵌入搜索

With codebase indexing, Chat can accurately generate responses based on your codebase.

使用代码库索引，Chat 可以根据您的代码库准确生成响应。

By pressing `Ctrl/⌘ + Enter` after typing a message, Chat scans through your indexed codebase to find pieces of relevant code. This is generally good for quickly including code snippets to be taken into the context of the conversation. For more control over the codebase search and better accuracy, you can use `@codebase`.

通过在输入消息后按下 `Ctrl/⌘ + Enter` ，Chat 会扫描您的索引代码库以查找相关代码片段。这通常有助于快速将代码片段包含到对话的上下文中。为了更好地控制代码库搜索并提高准确性，您可以使用 `@codebase` 。

![](img/db16b6ff60142840ffbb2a9e51b2d460.png)

## Advanced Codebase Search 高级代码库搜索

Cursor codebase chat goes through a more detailed search when `@Codebase` is used.

光标代码库聊天在使用 `@Codebase` 时进行更详细的搜索。

See more about `@Codebase` [here](https://docs.cursor.com/context/@-symbols/@-codebase).

查看关于 `@Codebase` 的更多信息。
