# MCP是大模型的 USB x.0 接口

> 来源：[https://v11enp9ok1h.feishu.cn/docx/Op2pdSPgbo3QtZxqyMYc0nV7n6e](https://v11enp9ok1h.feishu.cn/docx/Op2pdSPgbo3QtZxqyMYc0nV7n6e)

# 1. 概念

MCP全称模型上下**文协议（Model** Context Protocol），是由 Anthropic （Claude 模型的主体公司）在 2024 年 11 月 推出并开源的一项创新标准，旨在让大语言模型能够无缝连接至第三方的数据源。

该协议支持对接 **内容存储库、业务工具、开发环境** 等多种外部服务，从而赋能 AI 大模型获取更丰富的上下文信息，生成更加精准、相关且智能的回答。

![](img/1b8629e782bbf2fa5fa7ce7fb1890c07.png)

MCP 就像转接头，统一不同服务供所有人使用

MCP 就像一个“转接头”或“通用插座”，它的核心作用是统一不同外部服务（如 Google Drive、GitHub、Slack、本地文件系统等），通过标准化接口与 AI 模型对接。这样，开发者只需基于 MCP 规范开发一次“接口适配器”（MCP 服务器），就能让所有兼容 MCP 的模型（MCP 客户端）无缝接入，无需针对每个模型单独适配，大幅提升兼容性与开发效率。MCP 里面还包含 SSE（Server-Sent Events），是一种允许服务器向浏览器推送实时更新的技术。

为AI模型量身定制的“USB-C接口”

可以标准化地连接AI系统与各类外部工具和数据源

![](img/7eb98a6caa614810a13aec7b30103057.png)

传统的 API 就像不同的门和钥匙

每扇门都需要自己的钥匙和特定的规则

![](img/080c53b6c58e12b8474a3402b79f5f20.png)

**MCP与传统API关键区别：**

+   单一协议： MCP像一个统一接口，只要一次整合，就能连接多个服务。

+   动态发现： AI模型能自动识别并使用可用的工具，不用提前写死每个接口。

+   双向通信： MCP支持类似WebSockets的实时双向通信，模型不仅能查询数据，还能主动触发操作。

![](img/4fa8861445cf76d5f452670519a245fc.png)

LLM 也不是万能的，它缺失了很多能力，LLM 可以作为智能体的大脑，外部工具就是智能体的手和脚，协助智能体执行决策。一个典型的 Agent 的设计，LLM 充当大脑模块，通过多模态输入，处理信息，然后做出决策和规划行动。

MCP 就是想要通过一个开放的协议，为外部工具（或数据源）提供统一和 LLM 交互的统一集成，MCP 就是手脚连接身体的“关节”。

# 2. 案例

## 2.1 API 集成

通过联网搜索向 AI 提供最新信息

![](img/85570ed2fe66324df68731c78d44c653.png)

常见的 AI 助手采用通过**联网搜索获取实时信息**。**当**用户开启联网搜索时，助手先将用户的请求发送至搜索引擎，再将返回内容与用户输入一起提供给大模型，最终生成回答。搜索引擎在此作为实时信息源，为大语言模型提供额外的上下文。

通过 API 向 AI 提供自有系统数据

![](img/580e2cb2b2614f1c455f562848258914.png)

**如果希望 AI 能提供行业****内**部信息、或者研发的自有系统内的信息**，**AI 联网搜索的效果就很不好，甚至无法实现。用户可以自行搭建 AI 代理，将自有系统的数据通过 API 的形式接入 AI 助手，为大语言模型补充提供丰富的上下文信息。

通过 MCP 服务器向 AI 提供上下文信息

![](img/de2f06b01e4ca21b691bd58900239bba.png)

MCP 协议解决了 AI 大模型**与****数**据源集成碎片化**的**问题，提供统一标准，让开发者无需为每个数据源和 AI 助手单独开发连接器。通过 MCP，数据源和 AI 工具可建立安全双向连接，使 AI 在不同工具和数据集间流畅协作，实现更可持续的架构。

## 2.2 Blender MCP

一句话提示，Claude自动化打开Blender将2D图片转为3D建模。而且还能只用一次提示词，再基于这个场景搭建可以互动的网页。

![](img/79080ae668903acf31f40cbda28fceb8.png)

![](img/993a915c6ed4a6850d46cafed064a826.png)

![](img/a1a436747be27be0dbaad228a233afb7.png)

![](img/20fbf1ea76fe812cfc2f57ac7aa01f91.png)

还有直接通过口喷需求对话的方式快速去构建一个用 blender 渲染好的飞机...

![](img/44aef26c3735619284285a354a8f0aed.png)

[ajIJcAQUofHxwmZ9 (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/SrjWb8fo6oCvrQxHZCQczRI2nep)

## 2.3 Manus

Manus 的底层其实也是用了类似 MCP 的方式进行了一系列的工具能力调用（但是求证后实际上是没有使用 MCP，但是思维模式相似），包括了类似“Browser Use”这种可以直接在终端中操控电脑浏览器的工具，能够跨越纯 API 数据获取的方式，进行指令的下发。

分析亚马逊在线商店（全程录屏效果参考）

![](img/f2de6a0382bd31354c6d9c7b74815794.png)

[录屏2025-03-07 09.47.57 (1).mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Hu3XbsEXboPCHPxtNyZcvYkynwg)

**Read my CV & find ML jobs,** save them to a file

![](img/fd6d419aa86f565dbe8e30870f5f07ec.png)

[389458132-171fb4d6-0355-46f2-863e-edb04a828d04 (1).mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Z1NUbenkVodDcYxxTHrctEc1nEW)

## 2.4 MCP 服务市场

市面上目前也有越来越多的能力被挖掘出来，并被统一、分类，由不同的数据源和工具控制和供给，以便于用户快速的安装调用。

网络爬虫，可以获取页面结构信息

![](img/3f0044d6c87ca43e3d686fd6c8eacc70.png)

让大语言模型能够获取到本机时间

![](img/4913f87a0ab5d19f4601860105c2884f.png)

让大语言模型能够调用发送邮件

![](img/ac2101cf8ffd3b05583893f6bcb446ed.png)

控制电脑本地的文件读取

![](img/da1ea02569e8be8231000c4220494216.png)

方便的管理 Github 项目信息

![](img/f9d67643e4f10187cbf9cfca8cbe2375.png)

获取 Figma 的文件信息

![](img/23cdca5fd174e294c8452d6903dcfa35.png)

...

有非常多比较不错的集成综合工具站可以去看看

![](img/17a4581a1d4feef6960d0df444446418.png)


来自社区好朋友idoubi大佬的杰作

![](img/85c226041a309fdea7769190db1b35ba.png)

# 3.实践

vscode下载地址：https://code.visualstudio.com/

扩展里面搜索：cline

![](img/aa0743bde3a54445cfa110ed2ab32050.png)

## 3.1 将设计稿转成前端代码

Figma to html MCP 实战，仅需贴入一个链接给大模型，就可以将 figma 中的设计稿界面快速的变成在线可以预览的网页。

1.  **安装 MCP **

使用的是 Figma-Context-MCP 框架，在本地启动服务后再进行信息注册。

1.  使用 JSON 结构进行注册，同时获取 Figma 的 API Key

![](img/6827d8832e987b7c5ec3b04f58b667a2.png)

**2.复制 Figma 稿件地址**

![](img/13c96142fcecb09e4152393fb63db4bf.png)

原稿件样例

![](img/16a83476b0520797976285b52d7209ab.png)

贴入 figma 稿件地址后，等待效果生成

![](img/ac6957d4f00d2098f9a165f0b82220b2.png)

**3.生成预览前端代码**

生成的代码预览效果图

![](img/953a176871086bdf4a6134bc5d9d64bd.png)

生成的代码预览效果图

![](img/1aedfafbc027dff2034c697bb2a8b38d.png)

当然，MCP 服务也没有那么“神”，其实是做了一个编辑器的中间转换器，能够支持编辑和读取 figma 内的相关数据。

![](img/2f6b5c5a0daa7511abda712874a13d85.png)

[录屏2025-03-19 09.59.10.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OwnqbttzCoHPR1xeA9wcG6P8nM0)

在这个场景中的 MCP 相当于是对 Figma 的数据上下文结构信息进行**读取，结构化设计能力还是依赖于**底层模型 Claude，还是会出现布局混乱的现象，在实际用途上是更方便的取数了。

Figma 的 MCP 文件读取，可以看到实际上是读取了 Figma 的文件 Dom 结构信息。

![](img/e28c4501b7a5d3b1aa77877a70360df9.png)

使用图片下载的 MCP 能力，对页面结构中的图片进行填充。

![](img/694972ee8de5d8cd1714ff3adff8c479.png)

## 3.2 生成 AI 新闻资讯卡片

主要是实现一句话联网搜索相关信息后，参考相关的样式图进行页面填充，但是看起来也并不稳定，底层还是依赖于 Claude 的能力。

1.  **获取最新的新闻数据**

![](img/ce05afe4df5a6a86b72800c2e5ad20cc.png)

1.  **参考 figma 样式**

![](img/5badd1a69c8df532cad6d4616eafbb91.png)

给予对应的 figma 样式参考链接


1.  **生成最后的效果**

![](img/4b4a4abdae2537e4b1ac6f0a790258be.png)

![](img/6d1c325d09a7813fcc673307ac08843a.png)

![](img/cf206d6ca9d3822c7a410e64b3959071.png)

[录屏2025-03-19 17.56.30.mov](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/DriqbhYmPo1szExhYJsckbpFnad)

# MCP客户端工具

注意，很多模型是不能直接支持mcp服务的，因为模型本身没有办法去识别插件的能力（类似之前的function calling）也需要去对不同的工具去识别，在这里推荐开源的模型就是Qwen-max。

![](img/ab94584ee0bc7f3906175c426e3f4b62.png)

Cherry Studio，支持了SSE（类似https接口形式）和npx的安装方式。

当然必须给他点个赞，还是个开源项目：https://github.com/CherryHQ/cherry-studio

安装完cherry studio后，除了配置基础的模型服务，我们主要在设置中去配置MCP服务器，同时我们应该先把UV/Bun进行一键安装依赖，然后点击添加服务器。

![](img/164d1f68dd642ffb888ddad311d1a5ab.png)

以idoubi大佬的mcp服务市场对接为例：https://mcp.so/


+   SSE注册方法：

直接拿到sse的url即可，就可以在cherry stuido中选择以“sse”的结构导入。

![](img/f102fa40b30cda956e0dfe794ca76814.png)

![](img/09c424de2068ebb242fa8279da92fd99.png)

+   npx注册方法：

点击“编辑JSON”按钮，选择“STDIO”选项的结构，将下面的json结构文件中的正文部分内容进行贴入。

![](img/cd5468b3231a17680bd31df9d6b0c06c.png)

![](img/0ca64c647e6028c50f87c3df5ee8fc0f.png)

```
{
  &quot;mcpServers&quot;: {
    &quot;sequential-thinking&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;@modelcontextprotocol/server-sequential-thinking&quot;
      ]
    }
  }
}
```

配置好后点击“启用”按钮。

![](img/fbe0427141f620654d378fe771f69c4c.png)

需要选择能够支持MCP的模型后，启用MCP服务

![](img/f60edaf437f79cd15a54e824cfbcc1f3.png)

![](img/9305367efb61d569e305094fbbaea065.png)

可以看到他MCP服务已经调用成功了。

# 共学直播课程实践

👍 配合视频食用更佳哦：

+   搜索最新的新闻（serper search） + figma mcp，生成最新新闻数据渲染的界面。figma mcp npx版本目前好像因为简化了不需要启动本地服务，于是图片就没办法下载到本地了，这个估计近期作者会去修复。

![](img/24ea7e23c90ea1fb066fdc3130b3ee49.png)

+   Cline + figma + 高德地图生成的多个页面。

https://mp.weixin.qq.com/s/pFq6WhcOSCin4zl9msPvKg 高德的mcp已经支持sse，可以查看这个文章。

把多个figma的链接放到一个需求文档里面可以直接直接生成：





+   Master go的mcp服务也可以支持生成界面。

![](img/8062cd0ba685301851549dbee9325bea.png)

# 参考

Anthropic : Model Context Protocol (MCP)

[Model Context Protoc](https://docs.anthropic.com/en/docs/agents-and-tools/mcp)ol (MCP) - Anthropic

Get started with the Model Context Protocol (MCP)

[Introduction - Mo](https://modelcontextprotocol.io/)del Context Protocol

Learn how to add and use custom MCP tools within Cursor feature

Cursor – Model Context Protocol