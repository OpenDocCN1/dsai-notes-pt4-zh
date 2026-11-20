
# 一、Cursor 安装与初始设置

## 1. 下载与安装

1. 下载对应版本的 `cursor`：其中 Windows 点击主页右上角会自动下载 windows 的安装版本，mac 系统和 linux 系统的同学可根据系统对应下载。

![](img/b759c9094d57eec89955784017a97925.png)

1. 出现安装提示不必担心，点击确认即可

![](img/81b221818823752c7455775ad1cc5fc3.png)

1. 选择附加任务，请全选，减少环境配置的工作等。

![](img/091436eee42cff4d468cf0e729faa1da.png)

1. 初始化时，`keyboard` 选择 `VS code`，第二个 `Language for AI` 选择中文，AI 交流时则会选择中文回复，第三个点击开启后点击 `continue`。

1. 使用 `VScode` 的朋友可以导入 `VScode` 的插件，点击 `Use` ，没有就选择

## 2. 注册账号

若你没有 `Cursor` 账号，在启动界面会看到 `Sign up` 按钮，点击进入注册页面。你可以使用邮箱进行注册，按照提示输入邮箱地址、设置密码，再次确认密码后，点击 `Create account` 完成注册。

![](img/5d00a6b53da63c107892cb9f9f2fc76b.png)

## 3. 登录账号

如果你已有账号，在启动界面点击 `Log in`，输入注册时使用的邮箱和密码，点击 `Log in` 即可登录。登录成功后，你将进入 `Cursor` 的主界面。

![](img/630723003a78f191b2b3523ef13eb865.png)

## 4. 语言汉化

1. 安装插件，点击左上角 4 个方块的图标，下方输入框中输入 `chinese`。在插件列表找到 `Chinese(Simplified)`，点击【`Install`】按钮进行安装

![](img/aa747fbed4c360117f65cee96bf14c3d.png)

1. 观察左侧弹出的界面，点击正方形物体，并在搜索栏搜索 `Chinese`，点击中文简体的 `install`。点击 `Trust`

![](img/f06854b563adf7bba05330289129b1cb.png)

![](img/39d767bad5696d1d7b5839b6ca5c6b49.png)

![](img/bf2589c6b7021390f621a1ebd2249298.png)

## 5. 主界面介绍

1. 菜单栏：位于窗口的顶部，包含文件、编辑、视图、运行、工具和帮助等菜单项。

1. 侧边栏：位于窗口的左侧，通常显示项目文件结构、搜索、版本控制等功能。

1. 编辑区：位于窗口的中央，是编写和编辑代码的主要区域。

![](img/5a82ed9a70464b214574917df9095a93.png)

## 6. 打开目录

![](img/836e79389732e8e29dfdf64be3f7ae18.png)

除此之外，也可以在项目文件夹处点击右键，点击使用 `cursor` 打开。

# 二、Cursor 的核心功能

## 1. Cursor 快捷键及功能

1. `CTRL+ L`：打开对话框，可以看到右侧的 `chat` 栏，可以和 AI 实时沟通。

![](img/428a644db4be4d7c815b1840b698097f.png)

1. `CTRL+ K`：打开生成窗口，可以在光标上方打开生成窗口，这个时候你可以输入你的指令，让 AI 帮你修改或者生成一些代码，实现自然语言编程。

![](img/ad342aa984fe3332c7847522d2e4f7bd.png)

1. `CTRL+ENTER`/`CTRL+DEL`：快速的接受和拒绝 `cursor` 的修改，`accept/reject` 的快捷键

1. `CTRL+B`：左侧面板隐藏或显示

1. `CTRL+ALT+J`：书签

## 2. Cursor Tab

`Cursor Tab` 是我们本地的自动补全功能。它是一个更强大的 `Copilot`，能够建议整个差异，并具有特别好的记忆。在光标周围建议编辑，而不仅仅是插入额外的代码。同时修改多行。根据您最近的更改和 `linter` 错误提出建议。

1. 代码/注释自动补全

首先我们使用 `Ctrl+K` 给 `Ai agent` 命令，让其生成一段可以测试 `Tab` 按键的 `python` 代码，当我将首行注释时，`python` 代码由于无法识别 `tk` 是什么而产生报错（可以看到第一行文字颜色稍浅，是 `tab` 功能的提示输入），让我们尝试将第一段删除，并敲击键盘上的 `tab` 键进行使用。同时，在编写程序时，`Cursor` 有时候也会帮我们自动注释，这个时候敲击 `tab` 同样可以注释。

![](img/70233ba200aa824d200a7c9ef7fb188a.png)

1. `Tab` 自动补全后，先点击右上角三角小箭头，是启动功能，然后会生成我们代码的结果小窗口（测试 `Tab` 功能的程序，并能够进行直接测试）。

![](img/182ea92ef35367b8274ada5801c28f45.png)

1. `Tab` 键的深度使用：

`Tab`：按 `Tab` 键接受整个建议。

`Esc`：按 `Esc` 键拒绝整个建议。

`Ctrl+→`：逐字接受建议。

1. 文件跳转

`Tab` 会预测开发者接下来可能需要编辑的位置，并在接受当前修改后自动建议跳转到下一个相关位置。只需再次按下 `Tab` 键即可快速跳转，极大减少了鼠标操作和手动寻找的时间。在某些项目中，当我们有多个文件时，`Tab` 也会帮我们预测下个文件功能的位置，敲击 `Tab` 进行文件的跳转。

1. 从 `GitHub Copilot` 迁移

由于 `Cursor` 默认与 `GitHub Copilot` 一起使用，当同时安装了 `GitHub Copilot` 和 `Cursor`。我们建议在使用 `Cursor` 时关闭 `GitHub Copilot`。默认情况下，`Cursor` 优先于 `GitHub Copilot`。如果您想使用 `GitHub Copilot`，可以在设置中禁用 `Cursor`。

## 3. Cursor 模型设置

1. 打开设置，点击右上角的小齿轮，然后点击 `Models`

![](img/67c3947568465f875d1d85914072109d.png)

![](img/9ab1bebe87548adb2365897221a177d9.png)

1. 以派欧的 `api` 为例，引入自己的大模型 `api`，可以使用其他平台的 `api` 都是没问题的，这里只是引用一个可以免费试用一定额度的平台作为举例。派欧注册页面

1. 点击选择模型，以 `deepseek/deepseek-r1-0528` 举例

![](img/96fa3c360a77e99e14d07e60def46e7f.png)

1. 点击模型打开，其中 `Base URL` 和 `Model ID` 是后续配置需要使用的东西

![](img/280742180b00a6f2420b950b4e7b1097.png)

1. 创建自己的秘银：右上角账户→`Api` 密钥管理→新建（记得一定要妥善保存，这个平台为了安全问题，只会出现一次密码。）

![](img/4989fce9d50fa3c945f77311f0ef49cd.png)

1. 在 `Cursor` 中配置

![](img/d701e5f46648be71696470f5c95edcfb.png)

![](img/42bf066bf307a641309a7b28951fa622.png)

## 4. Cursor Chat

注：最新版的 `Cursor Composer` 已被合并，因此本文没有 `Composer` 的资料

`Cursor Chat` 让您在代码库中提问或解决问题，使用最强大的语言模型，全部在您的编辑器中进行。为了使语言模型能够给出良好的答案，它们需要了解与您的代码库相关的特定信息——上下文。

`Cursor` 具有多种内置功能，以在聊天中提供上下文，例如自动包含整个代码库的上下文、搜索网络、索引文档以及用户指定的代码块引用。它们旨在消除与语言模型在代码上工作时所需的繁琐复制粘贴。

1. 如何使用：键盘上按动 `Ctrl+L` 或者点击右上角小按钮

![](img/3580a6f2fd1a7ceaf0ff64351e44b42e.png)

1. 用户和 AI 消息

用户消息包含您输入的文本，以及您引用的上下文。您可以返回任何先前的用户消息进行编辑并重新运行查询。这将覆盖之后的任何消息并生成新的消息。

AI 消息是您选择的 AI 模型生成的响应。它们与之前的用户消息配对。AI 消息可能包含解析的代码块，可以通过 `即时应用` 添加到您的代码库中。

所有用户/AI 消息在同一线程中称为聊天线程，每个聊天线程都保存在您的聊天历史中。

![](img/42bf150208fddedafe3ee1af43ccd663.png)

1. 和 AI 更好的对话

默认情况下，`Cursor Chat` 包含当前文件作为上下文。通过 `@` 符号向每个气泡添加更多自定义上下文，默认情况下，当前查看的文件也将作为上下文在用户消息中使用。

## 5. Cursor+ MCP

1. `MCP` 是什么

`MCP` 的全称是 `Model Context Protocol`，是一个开放协议，它使 `LLM` 应用与外部数据源和工具之间的无缝集成成为可能。无论你是构建 `AI` 驱动的 `IDE`、改善 `chat` 交互，还是构建自定义的 `AI` 工作流，`MCP` 提供了一种标准化的方式，将 `LLM` 与它们所需的上下文连接起来。

1. 为什么选择 `MCP`

`MCP` 可帮助您在 `LLM` 之上构建代理和复杂的工作流。`LLM` 通常需要与数据和工具集成，而 `MCP` 可提供以下功能：

+ 越来越多的预建集成可供您的 `LLM` 直接插入

+ 在 `LLM` 提供商和供应商之间切换的灵活性

+ 保护基础架构内数据的最佳实践

1. 总体架构

`MCP` 的核心遵循客户端-服务器架构，其中主机应用程序可以连接到多个服务器：

![](img/96795d7737c08e4858415bf11e40a2ab.png)

**`MCP 主机`**：像 `Claude Desktop`、`IDE` 或 `AI` 工具这样的程序，需要通过 `MCP` 访问数据

**`MCP 客户端`**：与服务器保持 1:1 连接的协议客户端

**`MCP 服务器`**：轻量级程序，每个程序都通过标准化模型上下文协议公开特定功能

**本地数据源**：`MCP` 服务器可以安全访问的您计算机上的文件、数据库和服务

**远程服务**：`MCP` 服务器可以通过互联网（例如通过 `API`）连接到的外部系统

**扩展导读**：[`MCP` 官方文档](https://modelcontextprotocol.io/introduction)、[`Github` 官网](https://github.com/modelcontextprotocol)、`MCP Servers` `Github` 地址

1. `MCP` 与 `Cursor` 集成服务

[官网地址](https://cursor.directory/mcp)

![](img/8229eef12b0687a37d6f42ac9651bd1c.png)

1. 正式使用

+ 前置安装：需要更新 `Cursor` 到最新版本，其次需要下载 `Node.js`，因为 `MCP` 的底层需要可参考资料：https://blog.csdn.net/m0_74825699/article/details/146028679

+ `Cursor` 中配置 `MCP` 的位置：操作路径：`Cursor Setting` > `Tools & Integrations` > `MCP Tools`

![](img/47379564493d54956374bdeb15a41e7e.png)

+ 打开 `MCP` 集成网站→搜索 `Sequential Thinking`→点击 `Cursor`→点击 `One-Click install`→ 打开 `cursor`，点击 `install`

（以这个 `mcp` 举例：将复杂的问题分解为可管理的步骤）`Smithery.ai` 是一个模型上下文协议（`MCP`）服务器的注册和管理平台。简单来说，它是一个中心枢纽，用于发现和管理各种可以为大型语言模型提供实时上下文信息的服务器。

![](img/27c943a7f250f4da9f8f01133dff2fb7.png)

![](img/e478438b459be121a395129dc09742ef.png)

![](img/e46e1a080bb9d128fd7480e24abfd979.png)

+ 打开 `Cursor` 对 `MCP` 的白名单：举例：`Sequential Thinking` 并按回车键

![](img/3f9b59bf821fc608591e27ffb7c58077.png)

+ 检验是否配置成功的标志：

方案一：可以看到，`MCP` 管理中所有工具都是绿色小点、且显示可使用 `tools` 的数量

![](img/368489732be1634095a6db4b8cee7c6f.png)

方案二：进行 `AI` 测试，在右侧聊天框，输入 `MCP` 使用工具名称，比如 `Sequntial Thinking` 中的工具 `sequentialthinking`，并进行对话，并能看到 `Called` 对应的 `sequentialthinking`，并出现第一步...，即成功调用 `MCP`

![](img/6f8c6ada4338d09b3d259b6ed278508d.png)

![](img/7a39e66714feac9a3c1f67c8771778b5.png)

+ 常见的配置失败的原因：

1、忘记下载 `Node.js`

2、`smithery.ai` 这类网站通常是转存或者收集各类 `MCP`，在配置 `MCP` 时有一次网站的跳转，在有些 `MCP` 时，可以选择删除进行配置，更为保险的方案是：在转存的原始网站，比如 `github` 上去搜索此 `MCP` 并寻找其配置文件，通常可以成功配置。

![](img/f0980272e462c23f8dc8b72f11c65fcc.png)

3、没有设置白名单

## 6. 代码库索引

1. 代码库索引功能可以提高使用 `AI` 时的代码库答案的准确性。在后台，`Cursor` 为您代码库中的每个文件计算嵌入，并将使用这些嵌入来提高代码库答案的准确性。点击 `Add Context` 可以看到出现了多种类别。

[相关原理的深度解析](https://zhuanlan.zhihu.com/p/1921345802057548652)

![](img/44a2bf5c0590ba06000b1eeae99cb3c6.png)

1. `@Files`：在 `Cursor Chat` 和 `Cmd+K` [`等 AI`](https://cursordocs.com/docs/context/@-symbols/@-code) 输入框中，您可以通过使用 `@Files` 来引用整个文件。此外，如果您在 `@` 后继续输入，将在 `@Code` 策略后看到文件搜索结果。其中为了确保您引用的文件是正确的，`Cursor` 将显示文件路径的预览。当您在不同文件夹中有多个同名文件时，这尤其有用。如 `fronted.py` 来源于文件夹 `AutoComplete_App`，敲击最下方的 `fronted.py` 会跳转到对应的文件。

![](img/6f21e450312d49ba07f3b4414309aeff.png)

1. `@Folders`：您还可以将整个文件夹作为上下文引用。`@Folders` 在需要提供大量上下文时特别有用，例如在长上下文聊天中。

![](img/3bbb3a55ae00d28661bb638eca385cb8.png)

1. `@Code`：类似于 `[@Files](https://cursordocs.com/docs/context/@-symbols/@-code)` 符号，`Cursor` 将显示代码内容的预览，以便您验证所引用的代码是否正确。另一种将代码片段作为上下文添加的方法是选择您想要引用的代码，然后点击 `添加到聊天`（`Ctrl/⌘ Shift L`）或 `添加到编辑`（`Ctrl/⌘ Shift K`）。这些操作将把选定的代码片段添加到聊天输入框或当前活动的 `Cmd K` 提示栏中。要将选定的代码添加到新聊天中，您可以按 `Ctrl/⌘ L`。

![](img/dcb410073d7a780a000aa2b20d8ee624.png)

1. `@Docs`：`Cursor` 附带了一套爬取、索引并准备好作为上下文使用的第三方文档。您可以通过使用 `@Docs` 符号访问它们。如果您想爬取和索引尚未提供的自定义文档，可以通过 `@Docs > 添加新文档` 来实现。`Cursor` 将索引并学习该文档，您将能够像使用其他文档一样将其用作上下文。

![](img/b9774c8d94fbc1692f988863c3897be0.png)

1. `@Git`：目前，`@Git` 仅在 `Cursor` 聊天中支持。在 `Cursor` 的聊天中，您可以使用 `@Git` 将 `git` 提交、差异或拉取请求添加到您的提示中。

![](img/4befa0f2ba4529b813adadf10da318b9.png)

1. `@Past chats`：引用过去对话中的信息，可以让模型了解以前 `chat` 记录的核心内容。

![](img/7629a2ee1f3fbd6bdf378ffb68fa0873.png)
