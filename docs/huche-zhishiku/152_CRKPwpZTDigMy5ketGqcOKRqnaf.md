# Gemini-CLI：您的命令行AI助手

> 来源：[https://huchegames.feishu.cn/docx/O91adwISroEA9kxhWs2c7PTznMf](https://huchegames.feishu.cn/docx/O91adwISroEA9kxhWs2c7PTznMf)

> 🔗 原文链接： h[ttps://juejin.cn/p](https://juejin.cn/post/7522039703916396586)ost/752203970391...

> ⏰ 2025-07-02

## Gemini-CLI：您的命令行AI助手

Gemini-CLI 是 Google 推出的一款开源命令行工具，它将强大的 Gemini 模型直接集成到您的终端中。对于开发者、系统管理员和任何经常使用命令行的人来说，这是一个功能强大的 AI 伙伴，可以帮助您完成编码、内容创作、问题解答和自动化任务等一系列工作。

本指南将详细介绍如何安装和使用 Gemini-CLI，助您轻松上手。

### 1. 安装

安装 Gemini-CLI 非常简单，您需要确保您的系统中已安装 Node.js 18 或更高版本。

您可以通过以下两种方式安装 Gemini-CLI：

+   **通过 ****npx**** 快速运行 (推荐) **： 这种方式无需全局安装，每次运行时都会使用最新版本。只需在终端中执行以下命令：

```
npx @google/gemini-cli
```

+   **全局安装 **： 如果您希望在任何路径下都能直接使用 gemini 命令，可以选择全局安装。请注意，这可能需要管理员权限（使用 sudo ）：

```
npm install -g @google/gemini-cli
```

+   安装完成后，您可以随时通过输入 gemini 来启动它。

### 2. 认证

首次运行 Gemini-CLI 时，它会引导您完成认证过程。主要有两种认证方式：

+   使用 Google 账户登录 (推荐) ： 这是最简单快捷的方式。选择使用 Google 账户登录后，您的浏览器将打开一个认证页面。登录成功后，您将获得一个慷慨的免费使用额度，足以满足大多数个人开发者的日常需求。

+   **使用 API 密钥 **： 如果您需要更高的请求额度、希望使用特定的模型，或者在无法打开浏览器的环境中使用，您可以选择使用 API 密钥。

1.  在 Google AI Studio 中创建一个 API 密钥。

1.  在您的终端中将密钥设置为环境变量。对于 Linux 和 macOS，请使用：

```
export GEMINI_API_KEY=&quot;YOUR_API_KEY&quot;
```

1.  对于 Windows，请使用：

```
set GEMINI_API_KEY=&quot;YOUR_API_KEY&quot;
```

1.  请将 YOUR_API_KEY 替换为您自己的密钥。

### 3. 使用方法

Gemini-CLI 支持两种主要的交互模式：交互式聊天和非交互式（单次）查询。

#### 3.1 交互式聊天

这是 Gemini-CLI 的核心功能。在终端中输入以下命令即可启动一个可以持续对话的聊天会话：

```
gemini
```

启动后，您可以直接输入您的问题或指令。Gemini-CLI 会记住之前的对话内容，从而进行有上下文的交流。

在交互式聊天中，您还可以使用一些有用的内置命令（以 / 开头）：

+   /help : 显示所有可用的命令和提示。

+   /history : 查看您的对话历史。

+   /clear : 清除当前的聊天会话。

+   /stats : 显示当前会话的统计信息。

+   /file : 允许您引用本地文件。输入 /file 后，您可以选择一个或多个文件，Gemini 将能够理解并基于这些文件的内容进行回答。

**示例：**

```
&gt; 你好，请向我介绍一下你自己。
&gt; 我想写一个 Python 脚本来获取今天的天气，你能帮我吗？
&gt; /file
  (选择您的 `myscript.py` 文件)
&gt; 帮我审查一下我刚刚上传的这个脚本，看看有没有可以改进的地方。
```

#### 3.2 非交互式查询

对于需要快速得到答案或希望将 Gemini-CLI 集成到脚本中的场景，非交互式模式非常有用。您可以通过 -p 或 --prompt 标志直接从命令行传递提示。

**语法：**

```
gemini -p &quot;您的提示内容&quot;
```

**示例：**

+   **基本查询：**

```
gemini -p &quot;用一句话解释什么是 API&quot;
```

+   **结合文件内容提问： **您可以在提示中使用 @ 符号来引用本地文件。

```
gemini -p &quot;请总结一下 @report.txt 这个文件的主要内容&quot;
```

+   您也可以同时引用多个文件：

```
gemini -p &quot;比较 @script_v1.js 和 @script_v2.js 这两个文件的不同之处&quot;
```

+   **通过管道传递内容： **对于更复杂的脚本集成，您可以通过管道将其他命令的输出直接传递给 Gemini-CLI。

```
cat package.json | gemini -p &quot;根据这个 package.json 文件，分析项目的技术栈&quot;
```

### 4. 高级用法和提示

+   **指定模型 **：您可以使用 --model 标志来指定要使用的 Gemini 模型，例如 gemini-1.5-flash 。

+   **上下文为王 **：向 Gemini 提供尽可能多的相关上下文，可以帮助它给出更准确、更有用的回答。在提问时，清晰地描述您的目标和需求。

+   **代码生成与解释 **：您可以直接粘贴代码片段，让 Gemini 解释其功能、查找错误或进行重构。

+   **内容创作 **：无论是写一封邮件、一篇博客文章，还是一个营销口号，Gemini-CLI 都能为您提供帮助。

Gemini-CLI 是一个不断发展的工具，建议您通过 npx @google/gemini-cli@latest 保持更新，以体验最新的功能和改进。祝您使用愉快！

深入阅读：

---

<https://huchegames.feishu.cn/sync/Hzv6dRSGJsmHgOb4eSHcN1BHntg>