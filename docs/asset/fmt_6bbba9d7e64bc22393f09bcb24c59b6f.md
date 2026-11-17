
# MCP（模型上下文协议）详解

MCP 的优势在于：一是开放标准利于服务商开发 `API`，二是避免开发者重复造轮子，可利用现有 `MCP` 服务增强 `Agent`。

`MCP`（`Model Context Protocol`，模型上下文协议），2024 年 11 月底，由 `Anthropic` 推出的一种开放标准，旨在统一大型语言模型（`LLM`）与外部数据源和工具之间的通信协议。`MCP` 的主要目的在于解决当前 AI 模型因数据孤岛限制而无法充分发挥潜力的难题，`MCP` 使得 AI 应用能够安全地访问和操作本地及远程数据，为 AI 应用提供了连接万物的接口。

举个栗子，在过去，为了让大模型等 AI 应用使用我们的数据，要么复制粘贴，要么上传下载，非常麻烦。即使是最强大模型也会受到数据隔离的限制，形成信息孤岛，要做出更强大的模型，每个新数据源都需要自己重新定制实现，使真正互联的系统难以扩展，存在很多的局限性。现在，`MCP` 可以直接在 AI 与数据（包括本地数据和互联网数据）之间架起一座桥梁，通过 `MCP` 服务器和 `MCP` 客户端，大家只要都遵循这套协议，就能实现“万物互联”。

有了 `MCP`，可以和数据和文件系统、开发工具、`Web` 和浏览器自动化、生产力和通信、各种社区生态能力全部集成，实现强大的协作工作能力，它的价值远不可估量。这两种技术都旨在增强 AI 模型与外部数据的交互能力，但 `MCP` 不止可以增强 AI 模型，还可以是其他的应用系统。这样一个理想的“万物互联”生态系统看着很让人着迷。

但是大家是不是担心通过 `MCP Server` 暴露出来的数据会泄露或被非法访问，这个头疼的问题 `MCP` 也考虑到了。`MCP` 通过标准化的数据访问接口，大大减少了直接接触敏感数据的环节，降低了数据泄露的风险。还有，`MCP` 内置了安全机制，确保只有经过验证的请求才能访问特定资源，相当于在数据安全又加上了一道防线。同时，`MCP` 协议还支持多种加密算法，以确保数据在传输过程中的安全性。例如，`MCP` 服务器自己控制资源，不需要将 `API` 密钥等敏感信息提供给 `LLM` 提供商。这样一来，即使 `LLM` 提供商受到攻击，攻击者也无法获取到这些敏感信息。

不过，`MCP` 这套协议/标准，需要大家一起来共建，这个生态才会繁荣，现在，只是测试阶段，一切才刚刚开始，当然，还会涌现出更多的问题。

## MCP 架构设计

`MCP` 协议采用了一种独特的架构设计，它将 `LLM` 与资源之间的通信划分为三个主要部分：客户端、服务器和资源。客户端负责发送请求给 `MCP` 服务器，服务器则将这些请求转发给相应的资源。这种分层的设计使得 `MCP` 协议能够更好地控制访问权限，确保只有经过授权的用户才能访问特定的资源。

以下是 `MCP` 的基本工作流程：`MCP` 遵循客户端-服务器架构（`client-server`），其中包含以下几个核心概念：

`MCP client` 充当 `LLM` 和 `MCP server` 之间的桥梁，`MCP client` 的工作流程如下：`Claude Desktop` 和 `Cursor` 都支持了 `MCP Server` 接入能力，它们就是作为 `MCP client` 来连接某个 `MCP Server` 感知和实现调用。

`MCP server` 是 `MCP` 架构中的关键组件，它可以提供 3 种主要类型的功能：这些功能使 `MCP server` 能够为 AI 应用提供丰富的上下文信息和操作能力，从而增强 `LLM` 的实用性和灵活性。你可以在 `MCP Servers Repository` 和 `Awesome MCP Servers` 这两个 `repo` 中找到许多由社区实现的 `MCP server`。使用 `TypeScript` 编写的 `MCP server` 可以通过 `npx` 命令来运行，使用 `Python` 编写的 `MCP server` 可以通过 `uvx` 命令来运行。

`MCP` 协议支持两种主要的通信机制：基于标准输入输出的本地通信和基于（ ）的远程通信。这两种机制都使用  格式进行消息传输，确保了通信的标准化和可扩展性。

如果你还没有尝试过如何使用 `MCP` 的话，我们可以考虑用 `Cursor`（本人只尝试过 `Cursor`），`Claude Desktop` 或者 `Cline` 来体验一下。当然，我们并不需要自己开发 `MCP Servers`，`MCP` 的好处就是通用、标准，所以开发者并不需要重复造轮子（但是学习可以重复造轮子）。

## 官方与社区 MCP Server

首先推荐的是官方组织的一些 `Server`：。目前社区的 `MCP Server` 还是比较混乱，有很多缺少教程和文档，很多的代码功能也有问题，我们可以自行尝试一下  的一些例子，具体的配置和实战笔者就不细讲了，大家可以参考官方文档。

`MCP` 通过引入多样化的 `MCP Server` 能力，显著增强了 AI 工具的功能，例如我们常用的 `Cursor` 和 `Claude`。以下是一些官方参考服务器，展示了 `MCP` 的核心功能和 `SDK` 的应用：

### 数据与文件系统

文件系统：提供安全文件操作，带可配置的访问控制。

：提供只读数据库访问，具备架构检查功能。

：支持数据库交互和商业智能功能。

`Google Drive`：实现 `Google Drive` 的文件访问和搜索功能。

### 开发工具

`Git`：工具用于读取、搜索和操作 `Git` 仓库。

`GitHub`：集成仓库管理、文件操作和 `GitHub API`。

：支持项目管理的 `GitLab API` 集成。

`Sentry`：从获取并分析问题。

### 网络与浏览器自动化

`Brave Search`：利用 `Brave` 的搜索 `API` 进行网络和本地搜索。

`Fetch`：为 `LLM` 优化的网络内容获取和转换。

：提供浏览器自动化和网页抓取功能。

### 生产力和通信

：支持频道管理和消息功能。

`Google Maps`：提供位置服务、路线和地点详情。

`Memory`：基于知识图谱的持久记忆系统。

### AI 与专业工具

：使用多种模型进行 AI 图像生成。

`Sequential Thinking`：通过思维序列进行动态问题解决。

：使用 `Bedrock Agent Runtime` 从 `AWS` 知识库检索。

### 官方集成工具

这些 `MCP` 服务器由公司维护，用于其平台：

`Axiom`：使用自然语言查询和分析日志、跟踪和事件数据。

`Browserbase`：云端自动化浏览器交互。

`Cloudflare`：在 `Cloudflare` 开发者平台上部署和管理资源。

`E2B`：在安全的云沙箱中执行代码。

`Neon`：与 `Neon` 无服务器 `Postgres` 平台交互。

`Obsidian Markdown Notes`：读取和搜索 `Obsidian` 知识库中的 `Markdown` 笔记。

`Qdrant`：使用 `Qdrant` 向量搜索引擎实现语义记忆。

`Raygun`：访问崩溃报告和监控数据。

`Search1API`：统一的 `API` 用于搜索、爬虫和网站地图。

`Tinybird`：与 `Tinybird` 无服务器 `ClickHouse` 平台交互。

### 集成工具

`Docker`：管理容器、镜像、卷和网络。

`Kubernetes`：管理 `pod`、部署和服务。

`Linear`：项目管理和问题跟踪。

`Snowflake`：与 `Snowflake` 数据库交互。

`Spotify`：控制 `Spotify` 播放和管理播放列表。

`Todoist`：任务管理集成。

目前支持的部分工具列表（更多见）：

## Claude Desktop 配置 MCP 客户端

以 `Claude Desktop` 为例，配置 `MCP` 客户端的步骤如下：

添加所需的 `MCP` 服务器信息，例如：

这里的 `@modelcontextprotocol/server-filesystem`、`mcp-server-git` 是对应的一些 `MCP Server`，可以是开源找来的，也可以是你自己开发的。

配置完后，在主界面对话题右下角就有个锤子出现了，有几个锤子就是配置几个，然后对话中如果涉及使用该工具的，`claude` 就会自动调用 `Cursor` 工具中集成 `mcp server` 功能对开发增加效率非常明显，配置入口在：文件 -> 首选项 -> `Cursor Settings` -> `Features` -> `MCP Server` -> `Add new MCP Server`

配置完后，你需要画图的地方给它提要求就行了，它会自动感知，按上下文生成 `prompt` 并调用工具生成图片：生成的图片质量还不错，符合开发需要的图片。

## MCP 工作原理

那我们来介绍一下 `MCP` 的工作原理。首先我们看一下。总共分为了下面五个部分：

整个 `MCP` 协议核心的在于 `Server`，因为 `Host` 和 `Client` 相信熟悉计算机网络的都不会陌生，非常好理解，但是 `Server` 如何理解呢？看看 `Cursor` 的 AI Agent 发展过程，我们会发现整个 AI 自动化的过程发展会是从 `Chat` 到 `Composer` 再进化到完整的 AI Agent。

`AI Chat` 只是提供建议，如何将 AI 的 `response` 转化为行为和最终的结果，全部依靠人类，例如手动复制粘贴，或者进行某些修改。

`AI Composer` 是可以自动修改代码，但是需要人类参与和确认，并且无法做到除了修改代码之外的其它操作。

`AI Agent` 是一个完全的自动化程序，未来完全可以做到自动读取 `Figma` 的图片，自动生产代码，自动读取日志，自动调试代码，自动 `push` 代码到 `GitHub`。

而 `MCP Server` 就是为了实现 AI Agent 的自动化而存在的，它是一个中间层，告诉 AI Agent 目前存在哪些服务，哪些 `API`，哪些数据源，AI Agent 可以根据 `Server` 提供的信息来决定是否调用某个服务，然后通过 `Function Calling` 来执行函数。

## GitHub MCP Server 代码示例

我们先来看一个简单的例子，假设我们想让 AI Agent 完成自动搜索 `GitHub Repository`，接着搜索 `Issue`，然后再判断是否是一个已知的 `bug`，最后决定是否需要提交一个新的 `Issue` 的功能。那么我们就需要创建一个 `Github MCP Server`，这个 `Server` 需要提供查找 `Repository`、搜索 `Issues` 和创建 `Issue` 三种能力。

我们直接来看看代码：

```javascript
// 此处省略具体代码实现
```

上面的代码中，我们通过  来告诉 `Client` 端我们提供了哪些能力，通过  字段来描述这个能力的作用，通过  来描述完成这个能力需要的输入参数。

我们再来看看具体的实现代码：

```javascript
// 此处省略具体实现代码
```

可以很清晰的看到，我们最终实现是通过了  的 `API` 来实现和 `Github` 交互的，我们通过  函数来调用 `GitHub` 的 `API`，最后返回结果。

在调用 `Github` 官方的 `API` 之前，`MCP` 的主要工作是描述 `Server` 提供了哪些能力(给 `LLM` 提供)，需要哪些参数(参数具体的功能是什么)，最后返回的结果是什么。所以 `MCP Server` 并不是一个新颖的、高深的东西，它只是一个具有共识的协议。

如果我们想要实现一个更强大的 AI Agent，例如我们想让 AI Agent 自动的根据本地错误日志，自动搜索相关的 `GitHub Repository`，然后搜索 `Issue`，最后将结果发送到 `Slack`。那么我们可能需要创建三个不同的 `MCP Server`，一个是 `Local Log Server`，用来查询本地日志；一个是 `GitHub Server`，用来搜索 `Issue`；还有一个是 `Slack Server`，用来发送消息。

AI Agent 在用户输入  指令后，自行判断需要调用哪些 `MCP Server`，并决定调用顺序，最终根据不同 `MCP Server` 的返回结果来决定是否需要调用下一个 `Server`，以此来完成整个任务。

下面是个人推荐的一些 `MCP` 的资源，大家可以参考一下。

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/files"]
    },
    "git": {
      "command": "uvx",
      "args": ["mcp-server-git", "--repository", "path/to/git/repo"]
    }
  }
}
```

```typescript
const server = new Server(
  {
    name: "github-mcp-server",
    version: VERSION,
  },
  {
    capabilities: {
      tools: {},
    },
  }
);

server.setRequestHandler(ListToolsRequestSchema, async () => {
  return {
    tools: [
      {
        name: "search_repositories",
        description: "Search for GitHub repositories",
        inputSchema: zodToJsonSchema(repository.SearchRepositoriesSchema),
      },
      {
        name: "create_issue",
        description: "Create a new issue in a GitHub repository",
        inputSchema: zodToJsonSchema(issues.CreateIssueSchema),
      },
      {
        name: "search_issues",
        description: "Search for issues and pull requests across GitHub repositories",
        inputSchema: zodToJsonSchema(search.SearchIssuesSchema),
      }
    ],
  };
});

server.setRequestHandler(CallToolRequestSchema, async (request) => {
  try {
    if (!request.params.arguments) {
      throw new Error("Arguments are required");
    }

    switch (request.params.name) {
      case "search_repositories": {
        const args = repository.SearchRepositoriesSchema.parse(request.params.arguments);
        const results = await repository.searchRepositories(
          args.query,
          args.page,
          args.perPage
        );
        return {
          content: [{ type: "text", text: JSON.stringify(results, null, 2) }],
        };
      }

      case "create_issue": {
        const args = issues.CreateIssueSchema.parse(request.params.arguments);
        const { owner, repo, ...options } = args;
        const issue = await issues.createIssue(owner, repo, options);
        return {
          content: [{ type: "text", text: JSON.stringify(issue, null, 2) }],
        };
      }

      case "search_issues": {
        const args = search.SearchIssuesSchema.parse(request.params.arguments);
        const results = await search.searchIssues(args);
        return {
          content: [{ type: "text", text: JSON.stringify(results, null, 2) }],
        };
      }

      default:
        throw new Error(`Unknown tool: ${request.params.name}`);
    }
  } catch (error) {}
});

async function runServer() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error("GitHub MCP Server running on stdio");
}

runServer().catch((error) => {
  console.error("Fatal error in main():", error);
  process.exit(1);
});
```

```typescript
export const SearchOptions = z.object({
  q: z.string(),
  order: z.enum(["asc", "desc"]).optional(),
  page: z.number().min(1).optional(),
  per_page: z.number().min(1).max(100).optional(),
});

export const SearchIssuesOptions = SearchOptions.extend({
  sort: z.enum(["comments", ...]).optional(),
});

export async function searchUsers(params: z.infer<typeof SearchUsersSchema>) {
  return githubRequest(buildUrl("https://api.github.com/search/users", params));
}

export const SearchRepositoriesSchema = z.object({
  query: z.string().describe("Search query (see GitHub search syntax)"),
  page: z.number().optional().describe("Page number for pagination (default: 1)"),
  perPage: z.number().optional().describe("Number of results per page (default: 30, max: 100)"),
});

export async function searchRepositories(
  query: string,
  page: number = 1,
  perPage: number = 30
) {
  const url = new URL("https://api.github.com/search/repositories");
  url.searchParams.append("q", query);
  url.searchParams.append("page", page.toString());
  url.searchParams.append("per_page", perPage.toString());

  const response = await githubRequest(url.toString());
  return GitHubSearchResponseSchema.parse(response);
}
```

## MCP 核心概念与安全

`MCP`（`Model Context Protocol`，模型上下文协议）是一种新的开放标准协议，用来在大模型和数据源之间建立安全双向的链接。对照下面这个图，我们来理解下 `MCP`。

`MCP Host` 是什么？`MCP Host` 是、IDE 或其他 AI 工具，也就是大模型的应用。

`MCP Server` 是什么？实现 `MCP` 协议以供客户端访问提供特定功能或数据资源。

怎么安全了？`MCP` 内置了安全机制，`MCP Server` 自己控制资源，不用把  给 ，安全边界清清楚楚！

什么叫做双向？大模型工具可以读数据也可以写数据。

`MCP` 不仅可以访问本地资源（数据库、文件、服务），还能访问远程资源（例如  、），而且都用同一个协议！

除了数据（文件、文档、数据库），`MCP` 服务器还能提供 `API` 集成。

`MCP` 的出现解决了一个大模型应用开发的难题，即数据获取问题。大模型往往作为“孤岛”运行，难以直接访问实时数据源（如企业内部数据库、实时文档、在线服务等）。开发者通常需要为每个应用场景定制专用的适配器或插件，这既耗时费力，又缺乏可扩展性。`MCP` 的发布正是为了解决这一痛点。

通过定义一个标准化的协议，它允许开发者在无需重复开发的情况下快速连接模型与数据源，极大提升了模型的通用性和落地效率。如果 `MCP` 真解决了数据获取难题，那么将进一步降低大模型应用开发成本，将大模型应用推向新的高潮。

`MCP` 提供了一种统一的协议接口，使得模型可以通过  访问任何实现了  的外部数据源（如 `API`、数据库、文档管理系统）。这种标准化方式降低了模型与多样化数据源之间的连接复杂度，显著节省开发时间。

1）`MCP` 作为开放协议，允许任何开发者为其产品创建 `MCP` 服务器。这意味着整个生态将快速扩展，形成类似 `HTTP` 和 `REST API` 的网络效应，推动模型与应用场景的无缝融合。

2）协议内置了严格的权限控制机制，数据源的所有者始终掌握访问权。模型在获取数据时需要经过明确授权，避免数据泄露和滥用问题。

虽然 `Cursor` 已经能自动生成代码，但我们梦想中的 AI 编程工具，应该是这样的：这听起来像是天方夜谭？不，这就是 `MCP`！

那么问题来了，`MCP` 到底是什么？真的能做到这些吗？别急，我们用一个简单的例子讲清楚 `MCP` 的真正用途！

## 一个例子讲清楚 MCP

我们在 `cursor` 中安装了两个已经完成的 `MCP server`，后面会说具体的安装步骤。

1、`filesytem`：作用是将我们的文件保存到本地目录中

2、`fetch`：提供获取 `url` 的方法，可以通过 `fetch` 方法去获取 `url` 的信息。

当我们直接问 `cursor`，帮我读取一下这个网址的内容。`cursor` 就会自动调用我们刚才添加的 `fetch MCP` 服务。就会对网页的内容进行总结并返回。等返回之后，我们可以直接说将内容保存到本地。这时候就会调用 `filesystem` 的 `MCP server`，直接一键保存到本地。

`mcp` 是什么？模型上下文协议（`MCP`）是 `Anthropic` 推出的开放标准，也就是 `claude` 这个公司。它支持通过同一协议访问本地资源（如数据库、文件）和远程资源（如 `Slack`、`GitHub API`），无需定制集成。

当我们看完上一节 `mcp` 的例子之后，再回头看一下官方对 `mcp` 的描述。官方说 `mcp` 是一种协议，作为我们 AI 应用的 `usb-c` 端口，通过这个端口可以适配各种内容。拥有 `usbc`，你的电脑就可以连接外接硬盘、连接鼠标。当然上面的架构图也讲述的很清楚了。左侧的就是我们的开发工具，可以对接 `n` 个 `MCP server`，而这 `n` 个 `server` 又可以对接 `n` 个应用。相当于给我们的 AI 大模型插上了梦想的翅膀。

## 优质 MCP Server 资源

目前有哪些优质的 `mcp server` 基本上优质的 `mcp server` 网站列表如下：

官方地址：官方地址：

一些下载量高的 `mcp` 如：

## 如何添加 MCP Server

`Mcp server` 怎么添加 `Mcp server` 添加其实很简单，我们以获取 `fetch` 这个 `MCP server` 为例子，来重点讲解具体的添加方法。

第一步：在 `simithery` 中搜索关键字“`fetch`”。

第二步：点击详情，找到 `cursor` 的命令。

第三步：打开 `cursor` 的 `settting` 功能，进行 `MCP Server` 的添加。具体操作可以按照步骤去执行，在第三步的 `name` 自己填写，`type` 选择 “`command`”，点击 `save`。

添加完之后，稍等一会，如果检查成功，就会出现绿色的按钮。

有的小伙伴们说了，那网上都没有适合我的 `mcp server`，能不能自己写一个自己用？当然可以了，我们下一篇文章会详细介绍：如何自定义一个 `mcp server`？

## 写在后面的话

随着 `mcp` 的出现，相当于给大模型又添加了更多的技能，未来真正进入到 AI 编程技术平权的时代。如果你对 `mcp` 还有什么疑问，欢迎评论区留言。

最近，如果你经常使用 AI 编程的话，肯定听到过 `MCP` 这个概念？那到底什么是 `MCP` 呢？我今天试图给大家讲明白。

先从专业角度讲，`MCP` 就是  (`Claude`) 主导发布的一个开放的、通用的、有共识的协议标准。

`MCP` 遵循客户端 - 服务器架构，包含以下几个核心部分：

`MCP` 的工作流程可以简单概括为以下几个步骤：

举个例子，例如我们目前还不能通过某个 AI 应用来做到联网搜索、发送邮件、发布自己的博客等等，这些功能单个实现都不是很难，但是如果要全部集成到一个系统里面，就会变得遥不可及。

如果你还没有具体的感受，我们可以思考一下日常开发中，想象一下在 `IDE` 中，我们可以通过 `IDE` 的 AI 来完成下面这些工作。

举个通俗易懂的例子假设你正在使用一个 AI 编程助手来帮助你写代码。这个 AI 助手就是一个 `MCP` 主机。它需要访问一些外部资源，比如代码库、文档或者调试工具。`MCP` 服务器就像是一个中介，它连接了这些资源和 AI 助手。

## MCP 优势

举个生活化的例子：假设你是一个班长，每天要处理很多班级事务：查班级成绩表（`Excel` 文件存在电脑里），收集同学反馈（微信群里聊天记录），安排值日表（在线文档）。

所以，`MCP` 厉害的地方在于，不用重复造轮子。过去每个软件（比如微信、`Excel`）都要单独给 AI 做接口，现在 `MCP` 统一了标准，就像所有电器都用 `USB-C` 充电口，AI 一个接口就能连接所有工具。而且，数据不用上传到云端，AI 直接在本地处理。比如你的成绩单只存在自己电脑里，AI 通过 `MCP` 读取分析，但数据不会外泄。

`MCP` 会让 AI 更 “懂” 上下文，比如你让 AI “总结上周班会的重点”，它能自动调取会议录音、聊天记录、笔记文档，综合这些信息给你答案，而不是凭空编造。

所以，`MCP` 为 AI 应用提供了一个强大的工具，使其能够更灵活、更安全地与外部世界交互。

我看到一篇解读 `MCP` 非常详细和专业的文章，叫：，感兴趣的同学，可以去看看。博客文章地址：

欢迎大家加入我的社群，一起，一起，一起用 ！

最后，欢迎大家关注我的公众号：每天持续为大家分享 AI、技术、副业和互联网相关的干货，一起突破圈层，实现个体崛起。

原文链接：轻松愉快的 SQL 数据分析，只不过是冰山一角

## Claude 官方教程：SQLite 数据分析实战

`Claude`（`Anthropic`）最近出了个 `MCP`（`Model Context Protocol`，模型上下文协议）协议，搞得我朋友圈里有刷屏之势。你能清晰感受到，大伙儿都非常欣喜。我自己试用之后，决定写下这篇文章，分享给你。

`MCP` 是一种新的开放标准协议，用来在大模型和数据源之间建立安全双向的链接。这是它的官方示意图。这张图展示了使用 `Claude` 或其他 `IDE` 时，通过这种双向沟通协议，模型（目前指 `Claude`）可以与不同的数据服务器进行连接。每个连接的数据源可能千差万别，比如上图里面前两个连接本地数据，第三个则直接通过互联网操作远程文件。

`MCP` 有什么用呢？为什么会让这么多的开发者与用户欢欣鼓舞？`MCP` 是一种统一的集成方式，交互界面完全一致。如果其他大模型也跟进，那么以后连接数据的感觉，就像给不同的电子设备使用 `USB-C` 接口，而不用准备那么多种不同的线缆插头。

更重要的，是 `MCP` 的设计目标——提升。因为以前处理数据时，我们通常采用，很不安全。第一种是将数据上传到大模型的对话中。这会带来两个问题：

另一种方式是让大模型获得本地管理员级别处理权限，帮助我们自动处理本地数据。之前我  就属于这种方式。看起来非常方便、灵活，但 AI 代理在本地以管理员权限进行各种操作，看到所有文件。如果它被植入不安全的代码，控制你的计算机，可能导致隐私和重要数据泄露，后果严重性不言而喻。

为解决上述两种极端数据交互方式带来的问题，`Claude` 提供了 `MCP` 作为一种解决方案。作为协议，它是完全开放的。后续其他主流 AI 企业能否跟进，咱们说不准。但是咱们现在就可以用 `Claude` 来体验一下 `MCP` 带来的数据交互好处。

咱们得实践上手一番。官方的 ，我们先沿着这个指南操作一下。放心，指南非常简洁，步骤清晰。跟着做并不难。

官方教程给出了一个最简单的数据操作样例，是一个 `SQLite` 数据库。`SQLite` 设置非常简单，单文件即可运行。我讲数据库课程超过 10 年，一直用的就是 `SQLite`。学生不用一上来就去学习架设服务器、权限管理，而是直接拿过来就可以学习 `SQL` 查询语句。对文科生来说，这都是一个非常简单的界面。

在上手教程里，我们会操作一个本地 `SQLite` 文件，与 `Claude` 进行交互。我们需要预先安装一些软件，不过很简单，你照着指南里面这个命令拷贝到终端执行就行。下面是在我电脑上执行过程截图。

当然别忘了，你需要 ，这是咱们执行后续操作的前提。之后，你需要建立一个 `SQLite` 的数据库样例文件。咱们先按照官方的设定来操作。你复制页面上的这段代码，直接在终端执行，就能搞定。只要没有报错，你就拥有一个本地的 `SQLite` 样例数据了。它存储在你的用户目录下，叫做 `test.db` 。

下面你需要做的，是本次教程里最为不方便的操作 —— 修改 `Claude` 配置文件。我相信在未来的版本当中，这个操作是能够通过图形化的界面来拖拽完成的。不过现在还是原型系统，你暂且忍耐一下。教程里明确告诉你设定文件的路径，你照着这个来执行就好。你可以用 `Visual Studio Code` 或者类似的编辑器打开指定的配置文件路径。我这里用的是 。

打开该文件后，你需要把教程代码段里的内容填进去。不过这里有一个注意事项——你需要把原先代码中的。这个很重要，不然连不上数据，会耽误你很多宝贵时间查错……别问我怎么知道的。

之后注意，你需要在 `macOS` 上你的 `Claude Desktop` 的 `app`。到此，设定就算完成了。

下面，咱们实际看看 `Claude` 是如何与 `test.db` 这个数据文件交互。官网给出的流程图是这样的：如图所示，`Claude` 先要和我们刚刚搭建的 `SQLite` `MCP` 服务之间建立连接，然后可以执行查询的操作。

首先，我们先用提示词来把这二者连接起来。这里的提问我是直接从人家官方的快速开始教程里面照抄的 —— 「你能不能连接我的 `SQLite` 这个数据库，然后告诉我哪些商品现在可售，以及他们的售价？」

`Claude` 立即就会明白需要和 `SQLite` `MCP` 沟通。然后它就找我们要权限。我选择这一整次对话都可以给它开放权限（`Allow for This Chat`）。注意，这就是我刚刚跟你提到的安全性 —— 大模型要做什么操作，找我们要什么样的权限，权限开放的时间范围多大…… 我们都可以自己来控制。

大模型开始与 `MCP` 通讯，执行一系列的 `SQL` 语句，通过查询返回结果。注意，`Claude` 不像 `SQLite` 简单给你返回一个表格作为结果，而是用自然语言回答你的问题。这个样例中，它把现在可售商品都给你列出来，并且后面都标上价格。这种交互就显得非常自然。

下面我们来继续提出另一个样例问题 —— 「在你的数据库中，商品平均价格是多少？」这次大模型没有找我们再要权限。因为刚刚已经说明，整轮对话，它都可以获得 `MCP` 服务数据的操作权限。执行后，`Claude` 告诉我们，平均值为 82.14 美元。

你会发现我们刚刚一直用英文来提问，这是因为教程是英文的，咱们为了方便拷贝了问题。但对 `Claude` 来说，中文完全不是问题。下面咱们用中文来问：「你能分析价格分布并提出任何定价的优化建议吗？」你用中文来问，`Claude` 就会用中文来答。当然，背后还是连接 `MCP` 服务，调用 `SQL` 进行查询。

当查询遇到问题时，`Claude` 会自动反思，并且重组查询式，依照 `MCP` 服务返回的 `SQLite` 查询表格结果，告诉你不同的价格分布。基于这些分析结果，它会给出优化建议，如价格策略、产品组合、促销策略和定价心理学应用等。

注意这是你单独用 `SQLite` 查询数据库无法直接给出的结果，`SQLite` 只能给出表格。而根据背景知识对查询结果表格进行解读，才是。

既然跑通了官网给出的样例，我们接下来换上我讲数据库课程时常用的样例数据集，叫做 `colleges`。这个数据集来自斯坦福大学的一门 `MOOC`，包含学生申请大学的模拟数据。数据集包括三个表格：`apply`（谁申请了哪个学校的哪个专业，是否被录取）、`colleges`（所有大学的列表）和 `students`（所有学生的信息）。

平时上课时，我在这几个表之间来回操作，教学生如何跨越表格综合信息返回正确的结果。这次，咱们不用任何的 `SQL` 命令撰写，而是直接用自然语言来提问。

首先，你要确保 `MCP` 连接成功。注意你需要修改配置文件里，数据库文件的路径，指向  。对了，之后别忘了重启 `Claude Desktop`。

我的问题为：「你能否连接我的 `SQLite` 数据库，并告诉我里面有什么？」

还是索要了一系列权限后，`Claude` 告诉我们有三个表：`college`、`student`、`apply`。之后，通过进一步查询，`Claude` 为我们介绍 `college` 表中有哪些字段，`student` 和 `apply` 表又分别有哪些字段。至此意味着 `MCP` 数据连接成功。

`Claude` 会给出一些建议，告诉你可以问哪些问题。不过我还是用自己的问题好了：「哪些同学报考了 `Stanford` 并且被录取？」

`Claude` 通过 `MCP` 执行查询，告诉我 `Amy`、`Fay`、`Jay`、`Helen` 这几个学生被斯坦福大学录取，并且说明了他们的 `GPA` 和专业信息。`Claude` 特别指出，「」被录取的学生中，两名被计算机科学专业录取，两名被历史专业录取，大多数学生 `GPA` 都很高，3.7 以上，但也有一位学生 `GPA` 较低，仍被历史专业录取。嗯，2.9 的 `GPA` 也能被斯坦福录取，这确实「很有意思」。

接下来咱们问它第二个问题：「哪些学生没有被任何学校录取，是因为分数太低吗？」

`Claude` 返回了两个学生的信息，并且说明 `Bob` 申请了 `Berkeley` 的生物专业，而 `Craig` 申请了 `MIT` 的计算机科学专业。它总结说，这些没被录取的学生 `GPA` 其实不低，这表明 `GPA` 其实不是唯一的录取标准。

然后 `Claude` 甚至还专门给出了报考大学的方法建议。如果单单使用 `SQL` 查询，你不可能获得这些建议，这也是利用大模型做数据分析的有趣之处。`Claude` 通过 `MCP` 把当前的 `SQL` 查询结果与申请美国大学的背景知识有机地联系起来，厉害不？

但实际上，。我教了十多年数据库课，对这个数据集非常熟悉。这里有一个陷阱 —— 这个数据库里，有的学生没有申请任何一所大学。你不申请大学，当然不可能被任何一所大学录取，对吧？因此，在回答这个问题的时候，你的查询不能只看那些全部申请都被拒的学生。所以我进一步提示它：我只提示到这，并没有说「有的学生没有申请学校」。

但 `Claude` 很聪明，马上反应过来。它依然先找出所有提交过申请但没被录取的学生状况。后来它说，「让我们看看数据库中还有哪些学生是完全没有提交任何申请的」。注意这个查询，是它自己总结出来的。综合分析后，它的答案是：

刚才答案中那两个没有问题，是申请后却被所有申请的学校拒绝的学生；但还有若干完全没有提交申请的学生，分别是 `Doris`、`Amy`、`Gary` 和 `Edward`。它还补充道，「这确实是两种完全不同的情况。谢谢您的纠正」。很懂礼貌嘛，孺子可教。

`Claude MCP` 给我们带来的，绝不只是查询更简单、结果更全面、数据更安全这样的优势。至少，它。

在 `Claude` 对话里，你想上传文件，就会看到限制 —— 最多五个文件，每个文件不得超过 30 兆。我找了一个上课时用到的数据库叫 `movie.db`。这个数据库包含了若干年的电影信息，虽然只有 246.7 兆，但这样的文件想在现在的 `Claude` 对话当中使用，那断然是不可能的。你上传不上去，不仅仅是因为它体积太大，更是由于这种  格式 `Claude` 就不允许上传，你连选择它都没有机会。看，这些文件都是灰色的，不能点选。

但是现在不一样了，我们直接把配置 `MCP` 路径修改成 `movie.db`，然后来连接。`Claude` 找出这里面有三张表，分别包括了电影、演员和他们饰演角色的记录。

我问：「有多少女演员同时出演过《哈利・波特》电影的前两部？」你不要小看这个问题，你首先得知道《哈利・波特》电影的前两部都是啥。`Claude` 查询经过一些波折。但它非常勤恳地重构查询，然后告诉我们，这两部电影分别是《哈利・波特与魔法石》和《哈利・波特与密室》。之后它列出了 8 个同时出现在两部电影中女演员的名单，还介绍了这个系列中的主要角色，如赫敏和麦格教授。我觉得这个回答非常好。

如果你在学习 `SQL`，那么还可以打开它的中间分析过程来查看完整 `SQL` 语句。你可以自己用 `SQLite` 工具来验证查询结果。但更多时候，你兴许能从它的答案中得到参考和借鉴。

我必须说明一点 —— 本文所演示的内容，只是 `MCP` 能力的。`MCP` 现在支持的数据服务，就已包括 `GitHub`、`Google Drive`、`Slack` 等。甚至，你还可以用十几分钟的时间，干脆构建一个自己的 `MCP` 服务。官网分别提供了 `Python` 和 `Typescript` 语言版本的对应教程。

而仅从 `SQLite` 的样例看，`MCP` 目前就可以连接本地数据库，不用像原先那样把整个数据来回上传下载。安全性和控制力比以前显著增强。`Claude` 通过 `MCP` 作为中介，能很好地分析 `SQLite` 的数据集。在咱们展示的例子中，`MCP` 的优点是把大模型和数据有机结合起来 —— 通过对外部世界规律的微妙体悟，在真实任务中有效帮助你充分利用自己的数据。

提示词的清晰度依然很重要。例如刚才提到的「申请了学校但没有被录取」和「完全没有申请学校」这样的问题，有时还需要我们引导一下。试想我们把不同的数据来源综合起来，在一个对话中综合调用，这种感觉像更是一种「化学反应」，想想就让人兴奋。

希望 `MCP` 的出现，能激发你的创意，让你利用多元数据集获得更为深入的洞察。还是那句话，「临渊羡鱼不如退而结网」。与其看个热闹，不如自己动手试一试。哪怕你只是按照 `Claude` 官网的教程走一遍也好，相信也能获得更为直接的感悟。

欢迎你把自己尝试 `Claude` + `MCP` 的结果分享在留言区，我们一起交流讨论。祝 AI 辅助数据利用愉快！

如果你觉得本文有用，请。如果本文可能对你的朋友有帮助，请给他们。欢迎，以便及时收到后续的更新内容。
