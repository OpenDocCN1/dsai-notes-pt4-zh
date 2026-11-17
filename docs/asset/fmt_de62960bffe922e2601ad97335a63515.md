
# Fetch 的 Resources 和 Tools 能力对比

> 来源：[https://qianniucity.feishu.cn/docx/Ow3kdmDhEoV9wkxWSqzcq2UGnGc](https://qianniucity.feishu.cn/docx/Ow3kdmDhEoV9wkxWSqzcq2UGnGc)

> 来自用户的提问：这个能力和 他的 Tool 能力有什么不同，在 Fetch 上增加一个获取页面的工具不也可以

先明确一下：Resources 和 Tools 是 MCP 的两大支柱，Fetch 作为 MCP Server，两者都能用来抓网页，但定位和用法完全不一样。

## Resources：被动的数据“货架”

+ **本质**：Resources 是“喂数据”的接口，Fetch 用它把网页内容暴露给 LLM，像个“只读数据库”。LLM 自己决定啥时候读、怎么用。

+ **Fetch 的实现**：抓网页，转 Markdown，存成资源，等待 LLM 调用。

+ **特点**：

+ 被动性：Fetch 只管提供，LLM 得主动请求。比如 `fetch:///live/cnn-headlines`，LLM 自己去读。

+ 持久性：资源可以订阅更新（比如实时新闻），但本质上是“预备好的数据池”。

+ 广域性：能覆盖全网，只要有 URL，就能抓。

**例子**：

我问：“今天 CNN 头条是什么？” LLM 通过 Resources 读取 `live_news.md`，回答：“SpaceX 发射火箭了。” Fetch 只负责抓和喂，分析靠 LLM。

## Tools：主动的“执行者”

+ **本质**：Tools 是“干活”的接口，Fetch 用它让 LLM 主动触发抓取任务，像个“命令行工具”。LLM 说“干啥”，Fetch 就干啥。

+ **Fetch 的实现**：定义一个工具（比如 `fetch_url`），接收 URL，实时抓取并返回结果。

+ **特点**：

+ 主动性：LLM 控制抓取时机和目标，Fetch 是“听命行事”。

+ 即时性：每次调用都重新抓，不存档，适合一次性任务。

+ 灵活性：可以加参数（比如“只抓正文”），比 Resources 更定制化。

**例子**：

我问：“抓一下 CNN 的头条。” LLM 调用 `fetch_url("https://cnn.com/headlines")`，Fetch 返回 Markdown，LLM 再回答：“SpaceX 发射火箭了。” Fetch 是实时干活，主动性在 LLM 手里。

## 核心区别

直白比喻：

+ Resources 是 Fetch 开的“自助餐馆”，网页内容摆那儿，LLM 自己去拿。

+ Tools 是 Fetch 的“外卖小哥”，LLM 下单，Fetch 现抓现送。

## 在 Fetch 上加一个获取页面的工具可行吗？

Tools 和现有的 Resources 相比，有啥不一样？咱分析一下：

加工具的实现

**场景工具**：`fetch_page`

**MCP 配置（伪代码）**：

```
server.setRequestHandler("tools/list", async () => {
  return {
    tools: [{
      id: "fetch_page",
      name: "Fetch Page Content",
      parameters: [
        { name: "url", type: "string" },
        { name: "only_body", type: "boolean", default: false }
      ],
      description: "Fetch a webpage and return Markdown"
    }]
  };
});
server.setRequestHandler("tools/call", async (request) => {
  if (request.toolId === "fetch_page") {
    const html = await fetchURL(request.params.url);
    const cleanedHtml = request.params.only_body ? extractBody(html) : html;
    const markdown = htmlToMarkdown(cleanedHtml);
    return { output: markdown };
  }
});
```

用法：

我说：“用 `fetch_page` 抓 `https://cnn.com`，只拿正文。” LLM 调用工具，Fetch 返回：

```
SpaceX 发射新火箭，目标火星移民。
```

## 加工具 vs. 现有 Resources

1. 功能重叠：

+ Resources 也能抓网页（比如 `fetch:///live/cnn-headlines`），转 Markdown 喂 LLM。

+ Tools 也能干这活（`fetch_page`），区别在于调用方式。

1. 不同点：

+ 控制权：Resources 是 Fetch 先抓好放那儿，LLM 读；Tools 是 LLM 说抓啥就抓啥，更灵活。

+ 场景：Resources 适合持续喂数据（比如订阅新闻流），Tools 适合临时任务（抓个特定页面）。

+ 参数：Tools 可以加开关（比如 `only_body`），Resources 通常是固定输出。

1. 可行性：

加个 `fetch_page` 工具完全没问题，Fetch 本身就支持这种玩法（`mcp.so` 提到 fetch 工具）。但它不会取代 Resources，而是互补：

+ 用 Resources 做“后台监控”（持续抓取热点网页）。

+ 用 Tools 做“前台操作”（临时抓个页面分析）。

## Fetch 上 Resources 和 Tools 的分工

实际案例：

+ Resources：我让 Fetch 订阅科技新闻页，LLM 每天读 `fetch:///live/tech-news`，自动告诉我“最近 AI 有啥突破”。

+ Tools：我突然想查 SpaceX，调用 `fetch_page("spacex.com")`，LLM 立刻分析最新动态。

分工：

+ Resources 是 Fetch 的“长期内存”，存网页快照或实时流，适合批量、持续任务。

+ Tools 是 Fetch 的“即时反应”，按需抓取，适合单次、定制任务。

## 自由发挥：加工具后的潜力

如果在 Fetch 上加更多工具，能干啥？

1. 链式抓取：加个 `fetch_related_links`，从一个页面抓取所有超链接，再批量转 Markdown，LLM 就能分析整个网站。

1. 智能过滤：加个 `fetch_filtered`，让 LLM 指定关键词（“只抓 AI 相关段落”），Fetch 返回精炼内容。

1. 跨域协作：跟 Google Drive Server 联动，Tools 抓网页后直接存 Drive，Resources 再喂给 LLM。

## 写到最后

加个 `fetch_page` 工具完全可行，但它是 Tools 的活儿，跟 Resources 的“后台喂数据”角色不冲突，反而能让 Fetch 更全能。
