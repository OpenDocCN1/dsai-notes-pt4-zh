
# Google Antigravity 快速入门

> 来源：[https://aigcreative.feishu.cn/docx/TybEdLqA8oCooLxxA3QceC72ndd](https://aigcreative.feishu.cn/docx/TybEdLqA8oCooLxxA3QceC72ndd)

> 原文链接：Google Antigravity 快速入门

> 来源：👸号：北京GDG

![](img/577e70b6d0cb144967cd9072ef1d2860.png)

## 前言

上个月，Google 发布 `Gemini 3` 的同时，还发布一款全新的 AI IDE - `Antigravity` 。这是一个基于 `VS Code` 的 `Agent First` IDE 。它的核心理念是 AI 不仅是编写代码的工具，而且是一个能够在最小人为干预下，完成规划、执行、验证和迭代复杂任务的自主参与者。传统的编辑器或 IDE 都是由人来编写代码，IDE 提供代码完成等各种辅助功能。而 `Agent First` 的典型用法则是你来下达工作指令，agent（也就是 AI）自己来规划任务、实现目标，在需要权限的时候向你请求许可，最后验证通过后告诉你任务完成，中途还会产生一些中间产物用于和你确认。

本文会对 `Antigravity` 做一个整体介绍，重点是 Agent 的使用方面。当然，传统的 IDE 功能 `Antigravity` 也都有。

注：文中 AI 指底层大模型提供的核心能力，Agent 指利用大模型为人提供特定功能的程序。从用户角度有时候意思差不多，无需特意区分。

## 安装

安装本身很简单，从官网 `https://antigravity.google/` [1] 下载安装即可。然后启动 `Antigravity` ，首先是一些引导设置，然后需要登录 Google 账号，完成。

因为要需要使用 AI 服务，登录账号就是必需的，后续的 AI 功能也是要联网的。所以对国内用户而言真正的难点就是：账号问题和联网问题。

### 账号问题

你的账号需要是受支持的国家地区且年满18周岁，具体支持哪些国家见 `https://antigravity.google/docs/faq` [2] 。必要时需要修改你的账号所属地区和验证年龄。

### 联网问题

世界上有两种软件，代理友好的和代理不友好的。`Antigravity` 目前不会跟随系统或环境的代理设置，也不支持用户自己设置，所以属于代理不友好的软件。虽然 `Editor` 设置里有一个 `proxy` 设置，但是目前它没有覆盖到 `Antigravity` 的所有部分，所以还是有问题。

#### 解决方法

这里只说原理，不介绍具体方法：

1. 全局代理模式：强行让电脑上的所有程序都走代理。

2. 局部代理模式：有些代理工具可以指定特定的程序或进程走代理，这样你只需要找出 `Antigravity` 相关的进程就可以了。

用系统自带的进程管理器或者 `Antigravity` `Help 菜单 - Open Process Explore` 都行。主要是 `Antigravity` 、 `Antigravity Helper` 、 `language_server_xxx` (如 `language_server_macos_arm` ) 。其中主进程负责账号登录，`language_server` 进程负责 agent 的相关功能。

## Editor 窗口

![](img/069281452301fdca6cfb090e3e2c1d4c.png)

和传统 IDE 类似，一个 Editor 窗口对应一个项目，也就是一个 `Workspace` 。一个 workspace 一般就是本地的一个文件夹。

窗口的各部分功能 ：

• 顶部：有一些功能按钮：如 `Open Agent Manager` 等

• 左侧：导航栏

• 中间：编辑器

• 中间下方：`Terminal` 等

• 右侧：Agent 面板

上下左右各部分都可以使用快捷键或点击窗口右上角的图标展开或收缩。

## Agent Manager 窗口

![](img/ac4be6f67cdf16bf7cf1f845ca86651b.png)

点击 Editor 窗口右上角的 `Open Agent Manager` 或使用快捷键 `Cmd/Ctrl + E` 即可打开 Agent Manager 窗口。

Agent Manager 是所有 Agent 的集中管理窗口，这里你可以管理所有 workspace 的对话（`conversation`）。这是传统 IDE 没有的。在 agent 使用模式下，用户和 agent 是通过对话进行互动的。

他们的关系如下：

• `Antigravity` 可以有多个 workspace （所以就有多个 workspace 的 Editor 窗口）和一个 Agent Manager 窗口。 • 一个 workspace 中可以有一个或多个对话。

• Editor 窗口右侧的 agent 面板和 Agent Manager 窗口中相应的对话是一样的，并且 UI 是同步的。你在哪边用都可以。

• Agent Manager 可以同时发起和管理多个对话，Editor 窗口右侧的 agent 面板一次只能显示一个对话（点击顶部的 + 可以创建新的对话，但只能显示一个）。

Agent Manager 中还有一个特殊的 `Playground` ，它不是 workspace ，但是也可以进行对话，方便你尝试各种功能。你可以把它理解为一个临时的 workspace 。另外，通过 Agent Manager 窗口顶部的 `Move` 按钮，可以把 `Playground` 中的某个对话移动到你指定的文件夹中保存。

窗口的各部分功能：

• `Inbox`：所有最近的对话

• `Start Conversation` 或 + 号：新建对话

• `Workspace`：各个 workspace 下的对话

• `Playground`：临时的功能试验区

• 右侧主面板：显示某个具体的对话。注意，点击对话中的某些信息，会在它的右侧再展开一个面板用于显示相关的信息，如 `Task List` 、浏览器截图 等。

• `Knowledge`：自动生成的知识库，需要使用一段时间才可能有

• `Browser`：启动浏览器

• `Provide Feedback`：提交 bug 或 feature request

另外顶部的 `Review Changes` 和它旁边的那个小按钮也很有用。

## 新建对话

![](img/c48f0f3e79e6211cd7f8d5ce1533215c.png)

新建对话时，你可以选择：

• 对话是在哪个 workspace 下运行，或在 `Playground` 中运行

• `Fast` 还是 `Planning` 模式：前者适合简单任务，后者适合复杂的任务，比如从头开始创建一个应用时就要用 `Planning` 模式。

模型选择：不同的模型能力不同、速度不同、token 消耗不同，目前支持：

• `Gemini 3 Pro (high)`

• `Gemini 3 Pro (low)`

• `Claude Sonnet 4.5`

• `Claude Sonnet 4.5 (thinking)`

• `Claude Opus 4.5 (thinking)`

• `GPT-OSS`

价格：目前都是免费的，但是有使用额度限制。`Google AI Ultra` 会员给的额度最高，其次是 `Google AI Pro` 会员，使用额度每 5 小时刷新一次。免费用户额度最小，每周刷新一次，也就是说短时间内用用问题不大。

## 对话执行

![](img/e97565a194ae9f076737a2231e1673fe.png)

通过对话下达任务，每一个对话在执行的时候就是一个 agent 实例。开发者可以同时让最多 5 个 agent 实例并行运行。执行的时候，UI 会不断更新显示 agent 的运行状态。很多任务需要数分钟的时间才能完成，所以你也不需要一直盯着它，需要请求权限、`review` 或任务完成时，它会发出系统通知提醒你。

## 上下文引用

![](img/89df00db4b13aebf234a5032548b3ce1.png)

在对话输入框中，你可以使用 `@` 符号引用文件、目录、`MCP` 等，这样 agent 好知道相关的目标对象。

此外，还可以使用 `/` 符号引用自定义工作流（`workflow`）执行。

## Artifacts

![](img/5004407c6f12a14d9bd6332b77b0a7d0.png)

`Planning` 模式的对话，在 agent 在执行的过程中，会产生一些中间产物，这些中间产物被称为 `Artifacts` 。比如新建一个应用的时候，它会生成 `Task Lists`、`Implementation Plan` 和 `Walkthrough`，也就是任务列表、实现方案和工作总结。前两个是在生成代码之前的产出，`Walkthrough` 是任务完成后的产出。你可以通过添加评论的方式和这些产物交互，然后 agent 会做相应的变更。此外，`Artifacts` 还包括 `code diff`、屏幕截图、屏幕录像等。

### 在 Agent Manager 窗口中查看 Artifacts

点击对话顶部的 `Review Changes` 按钮，以及它旁边的另一个按钮。

### 在 Editor 窗口中查看 Artifacts

agent 对话输入框上方有一个图标按钮也可以查看 `Artifacts` 。

注意：删除对话会同时删除该对话产生的 `Artifacts` 。

## 编辑器和命令行中的 AI 功能

除了在对话窗口中让 agent 执行任务，还可以在 Editor 窗口的编辑器和 `Terminal` 中通过对话生成代码。

![](img/596714a953efdbea5b51eb2f810cb5e5.png)

比如在 `Terminal` 中，按 `Cmd/Ctrl + I` 弹出一个小窗口，输入提示词“统计 `src` 目录下所有 `.js` 文件的行数”，它会生成需要执行 shell 命令。

在代码编辑器中，可以选中几行代码然后按下 `Cmd/Ctrl + I` 弹出输入框输入你的指令。也可以按 `Cmd + L` 激活对话窗口，它会自动使用 `@` 符号引用这几行代码。当然，也可以不选中代码直接使用该功能，比如生成新的函数。

## 内置浏览器 agent

![](img/360fdd54b36a4edbfd1eaa264a4f2e03.png)

`Antigravity` 内置了 `Chrome` 浏览器工具。它可以打开网站、查看网页内容、滚动页面、输入内容、截图、录像等。比如你对 agent 说 “打开 google.com ，搜索“北京天气”，把截图发给我”，或者 “访问 news.google.com ，告诉我今日要闻”。总之，非常适合自动化任务。

agent 在自动执行的时候，为了告诉你这是内置的浏览器在自动工作，别和你自己的浏览器页面混淆，它会在页面的四周显示蓝色边框。另外，需要授权的时候会在 agent 窗口向你请求权限，比如打开页面、执行 `JS` 等工作。

手工打开内置浏览器：在 Editor 窗口点击顶部右上角的浏览器图标，或者在 Agent Manager 窗口点击左下角的浏览器图标。

第一次打开浏览器，页面会提示你需要安装一个 `Chrome` 扩展，点击安装即可。因为这个内置的浏览器是由 `Antigravity` 的 `Browser Subagent` 控制的，目前它应该是基于 `Chrome` 扩展实现的功能。其实，`Chrome` 团队在今年 9 月也推出了自己的 `DevTools MCP` ，可以实现类似的功能，甚至更强大。`Antigravity` 和 `Chrome` 是两个不同的团队，所以目前有各自的实现，以后说不定会统一。不过由于 `Antigravity` 支持 `MCP` ，所以你也可以自己在 `Antigravity` 中添加 `DevTools MCP` 。

## MCP Servers

![](img/cc6515a6aa04db0347ac91f331c64810.png)

点击 Editor 窗口 Agent 面板的右上角，选择 `MCP Servers` ，就会显示 `MCP` 的管理界面。这里列出了很多内置支持的 `MCP Servers` ，选择安装就行了。

![](img/e75f06d9a48b5d06d22b8dcf77d7a47c.png)

不在列表里的我们也可以自定义安装。点击界面右侧的 `Manage MCP Servers` 按钮，会在编辑器窗口中显示已安装的 `MCP Server` ，可以控制启用哪些功能。

![](img/895513826e17b4567d5fcc504d9c6a6b.png)

在这个界面里，点击 `View raw config` 按钮就可以直接手工编辑 `Antigravity` 的 `mcp_config.json` 文件了。比如，我把 `Chrome DevTools MCP` 添加进去：

```
"mcpServers": {    "chrome-devtools": {      "command": "chrome-devtools-mcp",      "args": [        "--browser-url=http://127.0.0.1:9222",        "-y"      ]    }  }
```

## Rules 和 Workflows

![](img/bb8bcec49fe9680406c1a4234076f73d.png)

在 Editor 窗口中点击 agent 面板右上角，点击 `Customizations` 菜单即可显示 `Rules` 和 `Workflows` 定制界面。

### Rule（规则）

`Rule`（规则）：用来定义 agent 的行为。比如让代码遵循特定 `code style` ，让所有方法都有文档注释。定义了 `Rule` 之后，agent 在执行的时候始终会注意这些要求，而不用每次对话都嘱咐它。所以 `Rule` 相当于系统指令。

### Workflows（工作流）

`Workflows`（工作流）：是保存起来的自定义提示词。和 `Rule` 不同，它不会主动触发，而是需要手工触发。在对话窗口中，使用 `/` 符号引用要执行的 `workflow` 即可。相当于给经常要重复执行的命令做了个快捷键。

`Rule` 和 `Workflow` 的作用域都有全局和 workspace 之分。全局就是影响所有的 workspace ；workspace 就是只对当前 workspace 生效。他们的保存路径是：

• Global rule: `~/.gemini/GEMINI.md`

• Global workflow: `~/.gemini/antigravity/global_workflows/` 目录

• Workspace rule: `your-workspace/.agent/rules/` 目录

• Workspace workflow: `your-workspace/.agent/workflows/` 目录

## Nano Banana 生成图片

目前，`Antigravity` 已经支持 `Nano Banana` ，在对话窗口中告诉它生成图片就行。比如根据代码生成一张图解释类之间的关系，或者为网站设计一个 logo。

## 安全

让 agent 在你的电脑本地执行各种命令可能有安全风险。目前 `Antigravity` 通过以下这些设置加以控制（`Antigravity - Settings`）：

• `Secure Mode`：新增的功能，启用后会增强安全，同时也会影响下面这些其它设置功能。

• `Terminal` 中是否自动执行命令设置，以及允许执行的 `Allow List` 和不允许执行的 `Deny List`

• `Browser` 中是否自动执行 Javascript 的设置，以及 `Browser URL Allowlist`

• `File Access`：`Workspace` 隔离设置（是否可以访问 workspace 之外的文件）和是否允许访问 `.gitignore` 文件。

• `Artifact Review Policy` 设置

## 参考资料

官网文档 [3]

Getting Started with Google Antigravity [4]

Tutorial : Getting Started with Google Antigravity [5]

Customize Google Antigravity with rules and workflows [6]

Chrome DevTools MCP [7]

### References

[1] : https://antigravity.google/

[2] : https://antigravity.google/docs/faq

[3] 官网文档: https://antigravity.google/docs

[4] Getting Started with Google Antigravity: https://codelabs.developers.google.com/getting-started-google-antigravity

[5] Tutorial : Getting Started with Google Antigravity: https://medium.com/google-cloud/tutorial-getting-started-with-google-antigravity-b5cc74c103c2

[6] Customize Google Antigravity with rules and workflows: https://medium.com/google-cloud/customize-google-antigravity-with-rules-and-workflows-b75398469302

[7] Chrome DevTools MCP: https://github.com/ChromeDevTools/chrome-devtools-mcp

#AISprint

<https://aigcreative.feishu.cn/sync/K4Gvd9vUQsy9Oubwqaeclu0Fnbg>

# 知识库动态
