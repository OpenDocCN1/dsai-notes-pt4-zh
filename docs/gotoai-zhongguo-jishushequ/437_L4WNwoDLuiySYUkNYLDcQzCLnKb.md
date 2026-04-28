# MCP技术浪潮中的Agent应用开发新范式

> 来源：[https://gotoaiworld.feishu.cn/docx/HVf0dMFbYo2glTxIL2pcJzo8nfb](https://gotoaiworld.feishu.cn/docx/HVf0dMFbYo2glTxIL2pcJzo8nfb)

> 作者：xisheng

> 当下，随着MCP（模型上下文协议）的迅速发展，AI 开发领域的协作生态正经历重塑，MCP为应用和服务方之间的协同开辟了新途径。太极平台积极拥抱 MCP，搭建起一座连接资源、工具与LLM的坚固桥梁。本文聚焦于 MCP 技术及其生态系统，一同探索太极平台如何借助 MCP，汇聚多方能力，释放大模型的巨大潜能，塑造 Agent 应用开发的全新范式。

# MCP技术

#### 1.1 MCP是什么()

MCP （Model Context Protocol，模型上下文协议）是一个标准化协议，定义了应用程序和 AI 模型之间交换上下文信息的方式。这使得开发者能够以一致的方式将各种数据源、工具和功能连接到 AI 模型（一个中间协议层），就像 USB-C 让不同设备能够通过相同的接口连接一样。MCP 的目标是创建一个通用标准，使 AI 应用程序的开发和集成变得更加简单和统一。

![](img/14933b808995250e93aa27316ac8ba18.png)

#### 1.2 MPC核心组件

如上图所示，在MCP架构体系中，包含MCP Server()、MCP Client和MCP Host几个核心组件，及组件间交互协议。

##### 1.2.1 MCP Server

MCP Server是一个通过MCP协议实现对外提供服务的轻量级应用，它对外提供工具执行、资源访问、预定义Prompt等一系列资源。

○ 资源（Resources）：类似文件的数据，可以被客户端读取，如 API 响应或文件内容 ○ 工具（Tools）：可以被 LLM 调用的函数 ○ 提示（Prompts）：预先编写的模板，帮助用户完成特定任务

下面是一个[**文件系统操作**](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)的MCP Server示例，它提供了对文件、目录的读写操作。

![](img/8e305cc0351155259d162052902d1b3e.png)

##### 1.2.2 MCP Client

在Host应用内部，MCP Client 与 MCP server 保持 1:1 的连接。MCP Client 充当 LLM 和 MCP Server 之间的桥梁。简单理解就是MCP Client SDK，用于实现Host应用使其能够与MCP Server交互。

##### 1.2.3 MCP Host

发起请求的 LLM 应用程序（例如 Claude Desktop、IDE 或 AI 工具）。通常像Claude Desktop 和Cursor()这类智能助手应用和IDE，它们都是MCP Host应用，通过MCP client来连接某个MCP Server，并调用其提供的工具。

另外值得注意的是，**运**行MCP Host应用所在的主机也属于MCP Host的一部分，主机的文件系统、浏览器等都可能成为MCP Server在运行时操作的对象。

#### 1.3 通信协议

MCP采用JSON-RPC来编码消息，在Sever和Client之间通过标准输入输出和Streamable HTTP进行消息交换。

##### 1.3.1 Standard Input/Output (stdio)

采用标准输入/输出流进行传输，适用于在同一台机器上运行的客户端和服务器之间的通信。

**Server**

![](img/7cf991e44a2a39f2545a57256fc23f76.png)

**Client**

![](img/e2794e84808c1355432725616b83f03f.png)

##### 1.3.2 Server-Sent Events (SSE)

远程通信**：**利用 SSE 与 HTTP 结合，实现跨网络的实时数据传输，适用于需要访问远程资源或分布式部署的场景。

**Server**

![](img/366f9088120b0df5f8394cde21ed0224.png)

**Client**

#### 1.4 MCP协议带来的改变

在使用MCP协议之前，开发智能应用的开发者（如Cursor）如果要使用外部服务的能力，每个应用开发需要各自开发对接外部服务的程序来实现能力集成。使用MCP后，服务提供者（如Slack、Google Drive）会以MCP标准协议对外提供MCP Server，服务的工具和资源将统一汇聚到市场上，应用以标准化的接口对接资源和工具，从而提升资源和工具的标准化和复用率。

![](img/78643b6ee118ca405f90b6a2b0d2f9e1.png)

# MCP执行细节

当用户在Host应用中输入一个Query后，Host应用是如何在LLM, MCP Client和MCP Server之间交互的呢？下图描述了Query请求处理的完整流程：

![](img/5effc7f6ddd2762bb82e32509ef9c42b.png)

1.  MCP Client首先从MCP Server获取可用的工具列表；

1.  将用户的Query连同工具描述通过Function Calling一起发送给 LLM；

1.  LLM 决定是否需要使用工具以及使用哪些工具；

1.  如果需要使用工具，MCP Client会通过 MCP Server执行相应的工具调用；

1.  工具调用的结果会被发送回LLM；

1.  LLM基于所有信息生成自然语言响应；

1.  最后将响应展示给用户。

#### 2.1 Host调用LLM

Host将用户Query和Tools一起发给LLM。

![](img/2bbd68e503801d7b60f0f6a579ba6c6b.png)

#### 2.2 Client调用MCP Server

大模型通过Query和提供的工具进行推理，确定完成用户任务需要执行的工具，如果LLM返回结果中如果需要执行tool_calls，则在会话中调用MCP Server。

![](img/5acba0ff6def6edfd3b80298575ca314.png)

#### 2.3 返回最终处理结果

Host将MCP Server执行的结果和Message一并发给LLM，如果不需要再调用其他工具，那么调用LLM做最后润色后返回最终结果。

![](img/9e17f64622f4ea1a167f947e7201e621.png)

# Agent, Function Calling与MCP

#### 3.1 Agent

LLM Agents是专为生成需要推理和执行任务的高级人工智能系统。它们能够通过推理思考、回忆过往对话，并通过调用外部工具，根据上下文和用户要求动态规划和执行任务，最终自动完成用户问题。

![](img/4eab5079c2f27df80802984e91fc77da.png)

形象来讲，LLM是一个强大的文本处理大脑，但其本身不具备执行任务的能力，因此需要借助工具，使其能够调用外部工具和服务，从而代理人工完成复杂任务的规划和执行。

#### 3.2 Function Calling

OpenAI最初通过Function Calling来给大模型装上“手脚”，让其能够调用外部工具和服务，从而代理人工完成复杂任务的规划和执行。

![](img/62bc1275a390eeb24649a9225dfe1168.png)

通过精妙的Prompt设计，可以让模型根据用户Query和可用的Function描述来进行任务分解和推理，从而选择合适的工具进行执行，并通过多次迭代完成用户任务。

#### 3.3 MCP

Function Calling的问题显而易见，对于Function的描述各厂商存在差异，模型迁移成本高；另外Fucntion Call存在效果不稳定、容错性低等问题。而MCP标准化了Agent与外部资源和工具交互的接口、协议及实现。从而为构建更为健壮的Agent应用和推动生态的协作起到了关键作用。

#### 3.4 对比

![](img/3479761662828c7e7001a9fbc60bf2ff.png)

# MCP技术生态

随着[**Manus**](https://manus.im/)的发布，MCP协议得以快速普及，MCP应用、MCP Server服务以及MCP应用市场的蓬勃发展，使得MCP技术生态迅速繁荣起来。

![](img/7e71f573f5d41e976166459771ae0247.png)

#### 4.1 MCP Server Marketplace

几个主要的MCP Server Marketplace上Host的MCP Server数量在短短20天内（2025/3/28 - 2025/4/18）有了大幅的增长。这进一步说明了其技术生态的繁荣程度。

#### 4.2 MCP应用程序

我们可以在[**PulseMCP**](https://www.pulsemcp.com/clients)上看到200多个应用支持了MCP，你可以在上面找到适合自己的MCP应用。

![](img/12e4e977c04d2baf85d95b567a883e9b.png)

#### 4.3 MCP Server

以[**mcp.so**](https://mcp.so/categories)上的MCP Server目录为例，我们看到MCP Server的范围已经涵盖数据、研究、云平台、数据库、Chatbot、文件系统、自动化等多个领域。

![](img/83a283887e023ee064a6d8d53a8cb861.png)

[**除此之外，在开**](https://github.com/jlowin/fastmcp)发[**者支持方面，除了官**](https://docs.spring.io/spring-ai/reference/api/mcp/mcp-overview.html)方[**的SDK以外，Fa**](https://github.com/lastmile-ai/mcp-agent)stMCP、Spring AI、mcp-agent等技术框架也进一步降低了MCP的开发成本。

# 对MCP发展的思考

作为一个新兴的技术规范，我们应该以审慎而积极的态度来看待，以下是个人对MCP可能带来的变化和其局限的分析判断。

#### 5.1 MCP带来的变化

正如WebService和WSDL定义了系统间服务调用的基本协议，推动了SOA架构的广泛发展，从此单应用系统逐渐发展成为多应用系统，企业内部的各个系统实现了数据交换和服务集成。REST轻量级服务风格进一步降低了数据传输中的Schema复杂度，而之后的微服务又使得服务进一步解耦和原子化，结合容器编排技术提升了服务的治理能力。任何一种技术和协议都是在不断迭代中进行发展、升级及迭代的。

在此，我们不妨大胆预测MCP带来的变化：

![](img/e0874d1d5029752d0350aee85f76796a.png)

**MCP OpenAPI化**

MCP将成为软件对外开放和集成的新协议，SaaS、应用服务将会以MCP方式提供对外服务，MCP将成为与OpenAPI类似的对外服务协议。

**应用交互重构**

重构应用形态，智能助手将以“搜索框”的形式成为各个应用的交互入口，支持语音及自然语言交换形式，完成复杂任务的执行。

**业务E2E自动化**

端到端理解用户意图、执行任务、容错处理将大大简化用户业务流程，一句话自动完成下单、售后、流程审批等操作将变得更为常见。

**生态开放协作**

支付、生活服务、内容服务等服务作为基础能力，实现你中有我、我中有你，从而更大限度拓展服务生态的协作开放。例如，一个个人助手可以完成调用获取好友列表、差旅申请、支付流程等操作，实现在一个应用中通过集成多个服务的MCP Server来完成复杂业务，前提是MCP授权及安全有保证，且MCP具有广泛的应用场景。

#### 5.2 技术限制

当然，MCP还处于发展初期，不可避免存在很多问题及缺陷，具体表现在：

**应用范围受限**

MCP Server大部分以本地库方式执行，很大比例的MCP Server依赖于浏览器等外部工具，通常是以Desktop App + MCP方式执行，更适合个人Assistant。企业应用一般在云端执行，这在一定程度限制了MCP的应用。

**行业标准支持**

当前在数据连接和交互领域有众多标准在竞争，MCP协议只是其中之一，要想成为行业通用标准面临很大挑战。目前MCP主要依托Anthropic的Claude生态，缺少核心厂商的支持，但也看到像OpenAI，以及阿里、腾讯也在积极拥抱MCP。

**对协议本质的质疑**

MCP本质上更像是 FunctionCall与Proxy 的组合，在这个角度上来看，MCP只是对于Agent执行外部工具和访问外部资源做了规范，并未在更大范围内实现标准化。许多人质疑其是否能够称之为一个真正的协议。

**安全风险**

MCP Server本地部署，被Host使用后将直接获得Host OS的访问权限，不安全的M[**CP S**](https://km.woa.com/articles/show/625688)erver将可能成为漏洞跳板。此外，不可信的MCP Server通过各种攻击手段也带来了诸多安全挑战。

![](img/ec09ac37a7dbaee997b1b337c3bcc3fd.png)

例如：开发一个 MCP Sever 用来实现加法操作，同时在其中放入恶意的 Prompt，要求 LLM 读取用户当前的工作路径。那么在其执行时就会读取用户工作目录和文件。