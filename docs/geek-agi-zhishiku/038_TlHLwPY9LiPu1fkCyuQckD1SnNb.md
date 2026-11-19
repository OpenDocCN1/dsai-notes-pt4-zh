
# 银海：`MCP` 是大模型的 `USB x.0` 接口

> 来源：[https://geekbang.feishu.cn/docx/XahqdulHdoPkLaxdoC4cDQTxnOd](https://geekbang.feishu.cn/docx/XahqdulHdoPkLaxdoC4cDQTxnOd)

![](img/45e3bfa8088f59249e8020f705b3b717.png)

## 1. 概念

MCP 全称模型上下文协议（`Model Context Protocol`），是由 `Anthropic`（`Claude` 模型的主体公司）在 2024 年 11 月 推出并开源的一项创新标准，旨在让大语言模型能够无缝连接至第三方的数据源。

该协议支持对接 内容存储库、业务工具、开发环境 等多种外部服务，从而赋能 AI 大模型获取更丰富的上下文信息，生成更加精准、相关且智能的回答。

![](img/8e74d9cb0d90213a87cb272c4499dd32.png)

MCP 就像转接头，统一不同服务供所有人使用

MCP 就像一个“转接头”或“通用插座”，它的核心作用是统一不同外部服务（如 `Google Drive`、`GitHub`、`Slack`、本地文件系统等），通过标准化接口与 AI 模型对接。这样，开发者只需基于 `MCP` 规范开发一次“接口适配器”（`MCP` 服务器），就能让所有兼容 `MCP` 的模型（`MCP` 客户端）无缝接入，无需针对每个模型单独适配，大幅提升兼容性与开发效率。`MCP` 里面还包含 `SSE`（`Server-Sent Events`），是一种允许服务器向浏览器推送实时更新的技术。

为 AI 模型量身定制的 “USB-C 接口”

可以标准化地连接 AI 系统与各类外部工具和数据源

![](img/5b5e1e220d804c3ca377920ba0d05d1d.png)

传统的 `API` 就像不同的门和钥匙

每扇门都需要自己的钥匙和特定的规则

![](img/47814199c3b1d4c665081d9cf50c5416.png)

### MCP 与传统 API 关键区别

- 单一协议：`MCP` 像一个统一接口，只要一次整合，就能连接多个服务。
- 动态发现：AI 模型能自动识别并使用可用的工具，不用提前写死每个接口。
- 双向通信：`MCP` 支持类似 `WebSockets` 的实时双向通信，模型不仅能查询数据，还能主动触发操作。

![](img/065566a481df2c5ff4123f875bc535a3.png)

`LLM` 也不是万能的，它缺失了很多能力，`LLM` 可以作为智能体的大脑，外部工具就是智能体的手和脚，协助智能体执行决策。一个典型的 `Agent` 的设计，`LLM` 充当大脑模块，通过多模态输入，处理信息，然后做出决策和规划行动。

`MCP` 就是想要通过一个开放的协议，为外部工具（或数据源）提供统一和 `LLM` 交互的统一集成，`MCP` 就是手脚连接身体的 “关节”。

## 2. 案例

### 2.1 API 集成

通过联网搜索向 AI 提供最新信息

![](img/ec929d71cc7e603f36677db6ec740dc6.png)

常见的 AI 助手采用通过联网搜索获取实时信息。当用户开启联网搜索时，助手先将用户的请求发送至搜索引擎，再将返回内容与用户输入一起提供给大模型，最终生成回答。搜索引擎在此作为实时信息源，为大语言模型提供额外的上下文。

通过 `API` 向 AI 提供自有系统数据

![](img/5ec48fd4774306de15c2fa42a1724eb8.png)

如果希望 AI 能提供行业内部信息、或者研发的自有系统内的信息，AI 联网搜索的效果就很不好，甚至无法实现。用户可以自行搭建 AI 代理，将自有系统的数据通过 `API` 的形式接入 AI 助手，为大语言模型补充提供丰富的上下文信息。

通过 `MCP` 服务器向 AI 提供上下文信息

![](img/3c61f7af6d31b52a3cd25dfe889b14b7.png)

`MCP` 协议解决了 AI 大模型与数据源集成碎片化的问题，提供统一标准，让开发者无需为每个数据源和 AI 助手单独开发连接器。通过 `MCP`，数据源和 AI 工具可建立安全双向连接，使 AI 在不同工具和数据集间流畅协作，实现更可持续的架构。

### 2.2 Blender MCP

一句话提示，`Claude` 自动化打开 `Blender` 将 2D 图片转为 3D 建模。而且还能只用一次提示词，再基于这个场景搭建可以互动的网页。

![](img/fb309d60e272e7e56a02fb648b8a98d2.png)

![](img/2c91df4ca34b7068c9251b2d98156c5c.png)

![](img/0aa2eaed33aa2f018dce4425bdc78fe3.png)

![](img/cbd6ef7b2954025ab525bb10fd393e04.png)

还有直接通过口喷需求对话的方式快速去构建一个用 `blender` 渲染好的飞机...

![](img/ea974295bc33db12402114384ec8af09.png)

[ajIJcAQUofHxwmZ9 (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Y5gCbfvJzod5vkxjHkGcsWePnge)

### 2.3 Manus

`Manus` 的底层其实也是用了类似 `MCP` 的方式进行了一系列的工具能力调用（但是求证后实际上是没有使用 `MCP`，但是思维模式相似），包括了类似 “`Browser Use`” 这种可以直接在终端中操控电脑浏览器的工具，能够跨越纯 `API` 数据获取的方式，进行指令的下发。

分析亚马逊在线商店（全程录屏效果参考）

![](img/d6d6953916f74698c1c79c8d235ef7b6.png)

[录屏2025-03-07 09.47.57 (1).mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/FMz2bq0ZUoedEbxdypRcToM4nYb)

Read my CV & find ML jobs, save them to a file

![](img/7630ef50bf97a0045f93ed3b61884013.png)

[389458132-171fb4d6-0355-46f2-863e-edb04a828d04 (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Dk4AbmXvxoiFcQxHcjfcMNHkntb)

### 2.4 MCP 服务市场

市面上目前也有越来越多的能力被挖掘出来，并被统一、分类，由不同的数据源和工具控制和供给，以便于用户快速的安装调用。

网络爬虫，可以获取页面结构信息

![](img/48ba0b3fec01d1dadea5b70e8292f35b.png)

让大语言模型能够获取到本机时间

![](img/42313a4e287a36318936f9ec65b0e9d6.png)

让大语言模型能够调用发送邮件

![](img/ca6ec5dcc0ddc0d432833f410a807822.png)

控制电脑本地的文件读取

![](img/04efa5e4386cb1fc9ce67dfdfb6a063c.png)

方便的管理 `Github` 项目信息

![](img/17b75dc8e487fadafbef3f31fab25f79.png)

获取 `Figma` 的文件信息

![](img/3784b5dc56be87daeb03005f9798f737.png)

...

有非常多比较不错的集成综合工具站可以去看看

![](img/7e51d7fc994b8801f5d242bfac4e935f.png)

来自社区好朋友 idoubi 大佬的杰作

![](img/1712c1988400dce1342125c21e2325fa.png)

## 3. 实践

`vscode` 下载地址：`https://code.visualstudio.com/`

扩展里面搜索：`cline`

![](img/62fb19e0b9cdabba45175f19667498b9.png)

### 3.1 将设计稿转成前端代码

`Figma` to `html` `MCP` 实战，仅需贴入一个链接给大模型，就可以将 `figma` 中的设计稿界面快速的变成在线可以预览的网页。

1. 安装 `MCP`

   使用的是 `Figma-Context-MCP` 框架，在本地启动服务后再进行信息注册。

   1. 使用 `JSON` 结构进行注册，同时获取 `Figma` 的 `API Key`

   ![](img/8c756ca098fca5c1dc229683767be1c7.png)

   2. 复制 `Figma` 稿件地址

   ![](img/c2ca6f0f15baed06c5b69c92b39406de.png)

   原稿件样例

   ![](img/5cb1eafd47ad2ddef6c83ea4b36b9437.png)

   贴入 `figma` 稿件地址后，等待效果生成

   ![](img/7e8c87b4dde81e38838cabd4d31730a9.png)

   3. 生成预览前端代码

   生成的代码预览效果图

   ![](img/4cf4e3e9750fb825568580205dc7386b.png)

   生成的代码预览效果图

   ![](img/c362c14194a4a6d4b92e200064dbc2a9.png)

当然，`MCP` 服务也没有那么 “神”，其实是做了一个编辑器的中间转换器，能够支持编辑和读取 `figma` 内的相关数据。

![](img/e3f7f58dbe4195368c19d27e1762000a.png)

[录屏2025-03-19 09.59.10.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/EmYHbaALNouFaAxWxwocWA7GnQg)

在这个场景中的 `MCP` 相当于是对 `Figma` 的数据上下文结构信息进行读取，结构化设计能力还是依赖于底层模型 `Claude`，还是会出现布局混乱的现象，在实际用途上是更方便的取数了。

`Figma` 的 `MCP` 文件读取，可以看到实际上是读取了 `Figma` 的文件 `DOM` 结构信息。

![](img/37a2c42ea0b819218f0d8c1fe560f744.png)

使用图片下载的 `MCP` 能力，对页面结构中的图片进行填充。

![](img/2ae5252f82a902164538013bf0a83370.png)

### 3.2 生成 AI 新闻资讯卡片

主要是实现一句话联网搜索相关信息后，参考相关的样式图进行页面填充，但是看起来也并不稳定，底层还是依赖于 `Claude` 的能力。

1. 获取最新的新闻数据

![](img/09b314ca6ca891f5d3923c24b6d15724.png)

2. 参考 `Figma` 样式

![](img/09d4d79668a376e785f4e073e2660998.png)

给予对应的 `figma` 样式参考链接

3. 生成最后的效果

![](img/0d1d34a061d58ef6ab1aa50dcf96af82.png)

![](img/cb486c332f66638146e8ad3098c62885.png)

![](img/413ff425e7db7b8454770bac90f3c8ad.png)

[录屏2025-03-19 17.56.30.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/MKI9bgyTdoidbMxMynZcUuu4nkc)

## MCP 客户端工具

注意，很多模型是不能直接支持 `mcp` 服务的，因为模型本身没有办法去识别插件的能力（类似之前的 `function calling`）也需要去对不同的工具去识别，在这里推荐开源的模型就是 `Qwen-max`。

![](img/d89dbb954e06069359c6b3b03fb5b477.png)

`Cherry Studio`，支持了 `SSE`（类似 `https` 接口形式）和 `npx` 的安装方式。

当然必须给他点个赞，还是个开源项目：`https://github.com/CherryHQ/cherry-studio`

安装完 `cherry studio` 后，除了配置基础的模型服务，我们主要在设置中去配置 `MCP` 服务器，同时我们应该先把 `UV` / `Bun` 进行一键安装依赖，然后点击添加服务器。

![](img/3499b930343a0459b1bdcd84b40a833a.png)

以 idoubi 大佬的 `mcp` 服务市场对接为例：`https://mcp.so/`

- `SSE` 注册方法：

  直接拿到 `sse` 的 `url` 即可，就可以在 `cherry stuido` 中选择以 “`sse`” 的结构导入。

  ![](img/3fab34fa55539341c85dce365e9e472d.png)

  ![](img/6a1b1b58a0ce55fab4728b122e55c1e6.png)

- `npx` 注册方法：

  点击 “编辑 `JSON`” 按钮，选择 “`STDIO`” 选项的结构，将下面的 `json` 结构文件中的正文部分内容进行贴入。

  ![](img/c84f9b3cdb16b2b567e32f3a63f5684f.png)

  ![](img/59c6a6ae736ecbce2429b23d19d86f16.png)

  ```
  {
    "mcpServers": {
      "sequential-thinking": {
        "command": "npx",
        "args": [
          "-y",
          "@modelcontextprotocol/server-sequential-thinking"
        ]
      }
    }
  }
  ```

配置好后点击 “启用” 按钮。

![](img/01f01bc2f8ed082dc66fbe14b008bc2d.png)

需要选择能够支持 `MCP` 的模型后，启用 `MCP` 服务

![](img/751b712255b6a67ce94182387073efdd.png)

![](img/33c08669fe47825e3616423d29536145.png)

可以看到他 `MCP` 服务已经调用成功了。

## 共学直播课程实践

👍 配合视频食用更佳哦：

- 搜索最新的新闻（`serper search`） + `figma mcp`，生成最新新闻数据渲染的界面。`figma mcp npx` 版本目前好像因为简化了不需要启动本地服务，于是图片就没办法下载到本地了，这个估计近期作者会去修复。

![](img/da4aaf4d05d04b177673b4471e7516ce.png)

- `Cline` + `figma` + 高德地图生成的多个页面。

`https://mp.weixin.qq.com/s/pFq6WhcOSCin4zl9msPvKg` 高德的 `mcp` 已经支持 `sse`，可以查看这个文章。

把多个 `figma` 的链接放到一个需求文档里面可以直接直接生成：

- `Master go` 的 `mcp` 服务也可以支持生成界面。

![](img/bf2639e703b4fc65e141207f40c26980.png)

## 参考

Anthropic : `Model Context Protocol (MCP)`

[`Model Context Protocol (MCP)` - Anthropic](https://docs.anthropic.com/en/docs/agents-and-tools/mcp)

Get started with the `Model Context Protocol (MCP)`

[`Introduction - Model Context Protocol`](https://modelcontextprotocol.io/)

Learn how to add and use custom `MCP` tools within `Cursor` feature

`Cursor` – `Model Context Protocol`
