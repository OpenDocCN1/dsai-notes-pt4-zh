
# AI IDE：Trae！智能代码助手革新编程体验

> 来源：[https://agijuejin.feishu.cn/docx/ME7XdMVFVoy56UxEYjyc1yi1nPe](https://agijuejin.feishu.cn/docx/ME7XdMVFVoy56UxEYjyc1yi1nPe)

> 作者：冴羽

> 链接：https://juejin.cn/post/7461825527059611686

> 来源：稀土掘金

> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

> 如有侵权，请联系删除。

## 前言

今日，字节发布了一款 AI Coding 产品 —— Trae，它是一款对标 Cursor 和 Windsurf 的全新 IDE，也是一款真正为中文开发者量身定制的工具，可谓是中文开发者的福音。

其优雅的 UI、丝滑的交互、母语级的支持、更高的 AI 集成度、更然自的交式互对话开发、更精准的 AI 生效成果，都让你感到亲切和惊艳！

它不再是一个工具，而是一个能 “思考” 和 “共创” 的协作者，帮助你更灵活的调用 AI 参与项目，实现更高效率、更好效果的开发体验。

目前 Trae 内置 `Builder` 和 `Chat` 两种模式：

1. `Builder` 模式：只需要告诉 AI 你想要什么样的应用，它会轻松完成从零到一的项目构建

2. `Chat` 模式：AI 将理解当前代码，你可以随时提出问题、寻求建议。此外也支持编辑器内实时提供建议代码

Trae 底层集成了国外主流的大模型 Claude 3.5 和 GPT-4o，目前免费使用中。

无论是专业开发者还是新手开发者，都能体验到 Trae 带来的效率提升。

下载体验地址：[`trae.ai`](https://www.trae.ai/?utm_source=juejin&utm_medium=juejin_trae&utm_campaign=AGI_article)

![](img/61aa8136302313c4bfb619abd094b6b0.png)

接下来我将详细地为大家介绍 Trae 的功能。

## 安装设置

从官网 [`trae.ai`](https://www.trae.ai/?utm_source=juejin&utm_medium=juejin_trae&utm_campaign=AGI_article) 下载安装后，首次打开会进入设置界面：

![](img/81be49496c7368c475b0e47abc3a055c.png)

然后是主题和语言设置。主题支持暗色、亮色、深蓝。语言支持简体中文和英文，这次终于不需要安装汉化插件了。

![](img/0732afce6246af5f2d298197c184ae9c.png)

Trae 支持从 VS Code 或 Cursor 导入配置，它会将插件、设置、按键绑定同步到 Trae，实现 IDE 的无缝切换：

![](img/fd89968a3ce9f1c846c587f7cf8e1b21.png)

Trae 也提供了自己的 `trae` 命令：

![](img/7d746dce40a3e6d0a8572720f74c99a2.png)

安装后可以通过：

+   `trae`：快速启动 Trae

+   `trae my-react-app`：在 Trae 中打开一个项目

最后一步是登录，可使用 GitHub、Google 账号三方登录，也可以使用 Trae 账号登录，这步跳过也行：

![](img/a9e9b2695e14836dd98a6ea04724f879.png)

## 强大的 AI 功能

Trae 包含了基本的 IDE 功能如代码编写、项目管理、插件管理、版本控制等。

这些功能想必大家已经很熟悉了，所以我们重点介绍其强大的 AI 助手功能，包括：

+   AI 问答：在编码过程中，你随时可以与 AI 助手聊天寻求帮助，包括让 AI 助手解释代码、编写代码注释、修复错误等。

+   实时代码建议：AI 助手将理解当前代码并在编辑器内实时提供建议代码。

+   代码片段生成：用自然语言向 AI 助手描述你的需求，它将生成相应的代码片段或自主编写项目级和跨文件的代码。

+   从 0 到 1 项目开发：告诉 AI 助手你想开发什么样的程序，它将提供相应的代码或根据你的描述自动创建相关文件。

具体体现为编辑器右侧的 `Builder` 和 `Chat` 两种模式：

![](img/32e6815001e1e5aafc009cb2ef36452c.png)

其中：

1. `Builder` 模式：只需要告诉 AI 你想要什么样的应用，它会轻松完成从零到一的项目构建

2. `Chat` 模式：AI 将理解当前代码，你可以随时提出问题、寻求建议。此外也支持编辑器内实时提供建议代码

### Builder 模式

Builder 模式可以帮助您从 0 到 1 开发一个完整的项目。

在 Builder 模式下，AI 助手将主动读取当前项目文件的内容，分解任务，并逐步执行它们。包括：

1. 提取相关上下文

2. 创建或修改文件

3. 生成和运行命令

4. 分析命令执行状态

而你只需要在右下角的聊天输入框输入您的需求即可，支持添加上下文，支持输入多种类型的内容，比如设计草稿、参考样式等等。

你只需要和 AI 进行交互即可快速创建项目：

![](img/54b32c2e5828754d24639e20ef1acf5b.png)

根据你的需求，AI 助手将自动生成代码更改，包括创建新文件和编辑现有文件中的代码，你可以接受或者拒绝修改：

![](img/7903373ff8a624e721d72628427871f6.png)

当 AI 助手完成了你的需求，它将提供一个预览按钮。点击此按钮将打开一个 `Webview` 窗口展示结果：

![](img/b39a43de969a6b0cc110395d322add13.png)

### Chat 模式

在日常开发中可以使用 Chat 模式，它可以回答代码问题、解释代码、生成代码片段、修复错误等，只要你提出要求。

让我们看下实际效果：

![](img/41e0f930942bd0f532b443e5fecee5e6.png)

聊天中生成的代码你可以将其应用到项目中：

除了侧边栏，你也可以在编辑器中使用内联的聊天框。使用快捷键 `Command + I`，或者选择任何代码，然后使用快捷键 `Command + I` 或单击浮动菜单中的 `Edit` 按钮：

![](img/a4465aa322294dc670fed523bbf0eede.png)

当有报错提示的时候，也可以进行 AI 修复，实际效果演示：

![](img/d380556b5e120f870d5dad2f97282c4e.png)

Trae 还有更多的细节值得发现，可以参考其官方文档：`docs.trae.ai/docs/what-i…`

## 总结

在 Cursor、Windsurf 等海外 AI IDE 盛行的当下，这款国内开发者开发的 AI IDE 无疑会成为一款更懂中国开发者的工具，所以未来可期。目前看来，它跟 Cursor 几乎一样强大。无论是专业开发者还是新手开发者，都能体验到 Trae 带来的效率提升。

[那就快来体验一](https://www.trae.ai/?utm_source=juejin&utm_medium=juejin_trae&utm_campaign=techcall)下吧。下载体验地址：`trae.ai`
