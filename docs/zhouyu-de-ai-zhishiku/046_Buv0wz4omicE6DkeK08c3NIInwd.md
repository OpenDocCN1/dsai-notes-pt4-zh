# Mcp

> 来源：[https://xatc0v8uz5m.feishu.cn/docx/IMzadKZpcoSpnHxap98cjSWun17](https://xatc0v8uz5m.feishu.cn/docx/IMzadKZpcoSpnHxap98cjSWun17)

```
{
  &quot;m**cpServers&quot;: **{
    &quot;GitHub&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;@modelcontextprotocol/server-github&quot;
      ],
      &quot;env&quot;: {
        &quot;GITHUB_PERSONAL_ACCESS_TOKEN&quot;: &quot;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&quot;
      }
    },
        &quot;Memory&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;@modelcontextprotocol/server-memory&quot;
      ],
      &quot;env&quot;: {
        &quot;MEMORY_FILE_PATH&quot;: &quot;memory.json&quot;
      }
    },
        &quot;sequential-thinking&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [
        &quot;-y&quot;,
        &quot;@modelcontextprotocol/server-sequential-thinking&quot;
      ]
    }
  }
}



&quot;chrome-devtools&quot;: {
     &quot;command&quot;: &quot;npx&quot;,
     &quot;args&quot;: [&quot;-y&quot;, &quot;chrome-devtools-mcp@latest&quot;]
   }
   ```

2. **spec-workflow** - 规格工作流管理
   ```json
   &quot;spec-workflow&quot;: {
     &quot;command&quot;: &quot;npx&quot;,
     &quot;args&quot;: [&quot;-y&quot;, &quot;@pimzino/spec-workflow-mcp@latest&quot;, &quot;/f/编程/251011trae测试&quot;, &quot;--AutoStartDashboard&quot;]
   }
   ```

3. **context7** - 高级上下文管理
   ```json
   &quot;context7&quot;: {
     &quot;command&quot;: &quot;npx&quot;,
     &quot;args&quot;: [&quot;-y&quot;, &quot;@upstash/context7-mcp&quot;],
     &quot;env&quot;: {
       &quot;CONTEXT7_API_KEY&quot;: &quot;ctx7sk-4577b8bf-ec69-42ab-be89-2e68d424b3f8&quot;
     }
   }


{
  &quot;mcpServers&quot;: {
    &quot;firecrawl-mcp&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;firecrawl-mcp&quot;],
      &quot;env&quot;: {
        &quot;FIRECRAWL_API_KEY&quot;: &quot;YOUR-API-KEY&quot;
      }
    }
  }
}
```

#### 最值得安装的MCP服务器

以下是20个最值得安装的MCP服务器，基于官方仓库(Model Context Protocol Servers)和社区推荐的“awesome-mcp-servers”(Awesome MCP Servers)，选择功能多样、受欢迎的服务器：

1.  Filesystem Server - 提供安全的文件操作，带可配置的访问控制。

1.  **GitHub Server** - 管理仓库、文件操作，并整合GitHub API。

1.  Google Drive Server - 提供Google Drive的文件访问和搜索功能。

1.  PostgreSQL Server - 提供只读数据库访问，带模式检查。

1.  **Slack Server** - 管理Slack的频道和消息功能。

1.  **Git Server** - 提供读取、搜索和操作Git仓库的工具。

1.  **Memory Server** - 基于知识图谱的持久记忆系统。

1.  Puppeteer Server - 启用浏览器自动化和网页抓取。

1.  **Time Server** - 处理时间和时区转换功能。

1.  Aws KB Retrieval Server - 使用Bedrock Agent Runtime从Aws知识库检索信息。

1.  Brave Search Server - 使用Brave的搜索API进行网络和本地搜索。

1.  **EverArt Server** - 使用各种模型生成AI图像。

1.  **Fetch Server** - 抓取并转换网络内容以高效使用LLM。

1.  Everything Server - 参考/测试服务器，带提示、资源和工具。

1.  Sequential Thinking Server - 通过思维序列促进动态和反思性问题解决。

1.  **Sqlite Server** - 提供数据库交互和商业智能功能。

1.  Cloudflare MCP Server - 整合Cloudflare服务，如Workers、KV、R2、D1。

1.  Browserbase MCP Server - 在云中自动化浏览器交互，如网页导航和数据提取。

1.  Grafana MCP Server - 允许搜索仪表板、调查事件并查询Grafana实例中的数据源。

1.  Meilisearch MCP Server - 与Meilisearch交互和查询，提供全文和语义搜索功能。

![](img/995139ff583599316a091204bfb08cac.png)

# chrome-devtools-mcp

让您的编码代理（如 Gemini、Claude、Cursor 或 Copilot）能够控制和检查实时 Chrome 浏览器。它充当模型-上下文-协议（MCP）服务器，为您的 AI 编码助手提供对 Chrome DevTools 全部功能的访问，以实现可靠的自动化、深入的调试和性能分析。

https://developer.chrome.com/blog/chrome-devtools-mcp?hl=zh-cn

https://github.com/ChromeDevTools/chrome-devtools-mcp/?tab=readme-ov-file#mcp-client-configuration

CC安装

claude mcp add chrome-devtools npx chrome-devtools-mcp@latest

```
{
  &quot;mcpServers&quot;: {
    &quot;chrome-devtools&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;chrome-devtools-mcp@latest&quot;]
    }
  }
}
```

![](img/5c445a4004dd81b495cb9cd8bcd34abc.png)

# Spec Kit

```
{
  &quot;mcpServers&quot;: {
    &quot;spec-workflow&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;@pimzino/spec-workflow-mcp@latest&quot;, &quot;/path/to/your/project&quot;, &quot;--AutoStartDashboard&quot;]
    }
  }
}
```

```
角色与目标 (Role and Goal)
你是一名顶级的 AI 软件工程师，严格遵循“规范驱动开发”方**法论。你的核心任务是确保在实****施任何功能之前，****需求都清晰明确，技术设计都****经过深思熟虑，并且****每一步都获得了用户**的**批准。
全局约束** (Global Cons**traints)**
● 语言: 简体中文
**工作流：规范驱动开****发**** (W**orkflow: Spec-Driven Development)
你必须遵循以下三阶段工作流。所有产出的文档（**.md 文件）都**必须**保存在当前项目根目录下**的 specs/{feature_**name}/ 文件**夹中**。在进入下一阶段前**，必须获得我的明确批准。
阶段 1: 需求与验收标准 (Requirements &amp; A**cceptance Crit**eria)
**1. 启动与澄清**: 当我提出新需求时，你首先要做的不是**编码，而是通过提**问来彻底理解需求的背景、目标和边界。
**2. 生成需求文**档: 在理解需求后，创建 requirements.md 文件。
  ○ 路径:specs/{feature_name}/requirements.md
  ○ 内容结构:
# 需求文档: [功能名称]

## 1. 介绍
[此处填写对功能的1-2句话高级总结]

## 2. 需求与用户故事
### 需求 1: [具体需求点名称]
**用户故事:** As a [角色], I want [功能], so that [收益].

#### 验收标准
***WHEN** [某个事件或触发器], **THEN** the system **SHALL** [系统必须做出的响应].
* **IF** [某个前提条件为真], **THEN** the system **SHALL** [系统必须做出的响应].
---
&lt;!-- 根据需要添加更多需求点 --&gt;
3. 请求批准: 生成文档后，必须暂停并使用标准问句进行确认：
“需求文档（requirements.md）已生成，请审阅。如果内容准确无误，我们将进入技术设计阶段。”
阶段 2: 技术方案设计 (Technical Design)
1. 启动与分析: 获得需求批准后，告知我你将开始技术设计。你必须首先分析项目的现有代码库和架构，确保新设计与之兼容。
2. 生成设计文档: 创建 design.md 文件。
  ○ 路径:specs/{feature_name}/design.md
  ○ 内容结构 (精简版):
# 技术设计: [功能名称]

## 1. 架构概述
[简述新功能如何融入现有系统，必要时使用 Mermaid 图表]

## 2. 数据模型/接口设计
* **数据库:** [定义新的数据表或对现有表的修改]
* **API 端点:** [定义相关的 API 端点 (路径, 方法, 关键请求/响应)]

## 3. 关键组件与测试策略
* **组件分解:** [列出核心的前/后端模块或组件]
* **测试策略:** [简述单元测试和集成测试的重点]
3. 请求批准: 生成文档后，必须暂停并使用标准问句进行确认：
“技术设计文档（design.md）已完成，请审阅。我们是否可以继续进行任务规划？”
阶段 3: 任务拆分与执行 (Task Planning &amp; Execution)
1. 生成任务清单: 获得设计批准后，创建 tasks.md 文件，将技术方案分解为具体、有序的编码步骤。
  ○ 路径:specs/{feature_name}/tasks.md
  ○ 内容结构:
# 实施计划: [功能名称]
- [ ] **任务1:** [描述一个具体、独立的编码任务, 如: 创建数据库迁移文件]
  -_关联需求: #1_
- [ ] **任务2:** [如: 生成 Prisma 客户端并更新类型定义]
  -_关联需求: #1_
- [ ] **任务3:** [如: 创建 POST /api/reviews 端点逻辑]
  -_关联需求: #1_
2. 请求最终批准: 展示任务清单，并请求开始执行。
“实施计划（tasks.md）已准备就绪。请确认，确认后我将开始按计划执行编码任务，并实时更新任务状态。”
3. 执行: 获得最终批准后，开始编码，并严格按照 tasks.md 的顺序执行。 
4. 更新: 执行完一个task以后更新文档,然后开始下一个task执行
MCP 服务使用规则 (MCP Service Usage Rules)
为了最大化效率和准确性，你必须智能地使用可用的 MCP 服务。
● 通用原则: 如果 MCP 服务器存在，优先使用其提供的 工具 (Tool) 和 资源 (Resource)。
● 核心服务使用策略:
  a. 官方文档审查 (Documentation Review):
    ■ 必须 使用 contextx7 MCP 服务查询项目依赖（npm包、框架等）的官方文档，确保生成的代码与当前使用的版本兼容，避免 API 废弃或不一致的问题。
  b. 网页自动化 (Web Automation):
    ■ 所有需要与网页交互的任务（内容抓取、登录、表单提交等），必须 使用 playwright MCP 服务执行，以确保能处理动态渲染的 JavaScript 内容。 . 交互式反馈 (Interactive Feedback):
```

![](img/e8e9e2a8722df64a04bf148037105a86.png)

https://agents.md/#examples

![](img/6b450f28af04f46cfafceead47324166.png)

![](img/138fa1c848d4844f7e22a54159e453ae.png)

1. @modelcontextprotocol/server-filesystem - 文件系统操作

2. @modelcontextprotocol/server-github - GitHub 集成

3. @modelcontextprotocol/server-postgres - PostgreSQL 数据库操作

4. @modelcontextprotocol/server-aws-kb - AWS 知识库（用于 S3 存储）

5. @modelcontextprotocol/server-fetch - 网络请求

6. @modelcontextprotocol/server-shell - Shell 命令执行

[第三期 TRAE 使用技巧](https://lcnziv86vkx6.feishu.cn/wiki/KlniwP8gfiDNtwko7iPcVDoMnMe)

Tavily mcp

https://app.tavily.com/home获取秘钥

是一个搜索工具，可以理解为谷歌搜索，解决trae自带的联网搜索出来的低质量搬运文章

```
{
  &quot;mcpServers&quot;: {
    &quot;tavily-remote-mcp&quot;: {
      &quot;command&quot;: &quot;npx -y mcp-remote https://mcp.tavily.com/mcp/?tavilyApiKey=&lt;your-api-key&gt;&quot;,
      &quot;env&quot;: {}
    }
  }
}
```

context7

解决LLM对于API的理解过时或者瞎猜API的问题

https://github.com/upstash/context7

Sequential Thinking:引导模型通过分步骤、有逻辑地逐步推理来解决复杂问题的方法,旨在提升思考的条理性

准确性和可解释性。

官方地址:servers/src/sequentialthinking at main . modelcontextprotocol/servers

4.Time:解决LLM不知道现在的时间是什么时候的问题

。官方地址:servers/src/time at main.modelcontextprotocol/servers

5.PlaywrightMCP:让TraeAgent可以操作浏览器自动测试前端代码的修改是否符合任务预期(整个操作过程非常像

人,有了这个MCP工具你还可以快速让你的Agent具备Browser操作能力,妥妥的自动化神器)

9月8号微软官方发了一个重大更新,现在可以通过安装Playvwright MCP的Chrome Extension让Playwright

复用我们正常使用的浏览器的登录状态操作浏览器进行自动化测试了!在以前只能创建一个纯净的浏览器来测

试,对于登录状态之类的没办法复用,导致很多前端项目的测试都非?常不方便。

官方地址:GitHub-microsoft/playwright-mcp:Playwright Toolsfor MCP

Chrome Extension下载地址:https://github.com/microsoft/playwright-mcp/releases/tag/v0.0.37

有了Playwright之后强烈建议大家自定义一个Agent然后把Trae自带的预览功能取消勾选,用Playwright调试

非常强大!!!

。案例:用Playwright MCP爬取当前网页的数据

6.Figma MCP:让Agent更语义化的理解Figma的设计稿内容,使号Agent更好的还原Figma的设计稿内容。

Figma官方MCP使用文档:https://help.figma.com/hc/en-us/articles/32132100833559-Guide-to-the-Dev-

Mode-MCP-Server

注意不要使用Trae的MCP市场里面下载的那个Figma MCP,那个是野生MCP,更推荐大家使用Figma官方的

MCP,不但功能更多,还原的效果也越好~(希望Trae的官方人员看到这里可以解决一下这个问题,帮大家引导

到Figma官方的地址去)

二、自定义Agent的SystemPrompt描述指南:

1.首先需要推荐一个神中神github项目:GitHub-x1xhlol/systenm-prompts-and-models-of-ai-tools,这个项目

中逆向了各种AIIDE的Agent的SystemPrompt,大家可以各个都看一下学习一下AI大佬们是用什么框架来写

SystemPrompt的(看不懂英文的,可以翻译为中文之后看内容)

2.其次是在自定义Agent的Prompt中强烈建议大家写清楚自己启用了哪些MCP给这个Agent,并且描述这个MCP是做

什么用的,这样Agent在完成任务时调用MCP才会更准确(如果果还是不够准确建议大家在描述问题时,直接告诉它应

该用哪个MCP进行什么操作,相当于描述它在完成这个任务时应该遵从什么工作流~)

```
# MCP Servers
1. tavily-mcp
用途:网络搜索
2. playwright
用途:控制浏览器来验证前端修改是否符合预期
3.context7
用途:获取API最新的官方文档
Sequential Thinking
用途:引导模型通过分步骤、有逻辑地逐步推理来解决复复杂问题的方法,旨在提升思考的条理性
5. Time
用途:获取当前时间
```

最后就是Trae的Agent我认为应该这么用最爽:

a.首先自定义一个通用任务的CodingAgent,这个CodingAgent的SystemPrompt可以模仿各个AIIDE逆向出来

的SystemPrompt的描述。然后平时完成任务就用这个Agent进行辅助编码。

b.对于常用的特定工作流也自定义一个Agent,然后在Agent的SysstemPrompt中写入这个工作流的流程,第一步

应该怎么做,第二步应该调用什么MCP工具获取什么,然后根据获取的内容去做什么。

C.这样面对不同任务切换不同Agent不但可以减少描述需求的时间,还可以让Agent的操作更符合你的预期。

注意:一个自定义Agent的MCP的总Tools不要超过40个,LLM面临超过40个Tools时调用的精准度极速下降!!!

三、分享一些我正在使用的UserRules:

这个可以因人而异,大家只需要复制你认为符合你的需求的规见则即可。

```
1.Always responsein中文
2.任何时候都不应该猜测api的名字,当无法确定api时应该使用context7获取api的官方文档。
3.使用sequentialthinking来逐步分析用户的问题
4.所有为了测试或者验证效果而创建的,在测试完成之后都应该删删除。
5.网络搜索时应该使用tavily-mcp
6.完成前端修改之后使用playwright进行修改验证
7.优先使用pnpm
8.优先使用uv
```

四、需求描述技巧:

进行需求描述时应该尽可能地把你当前描述的这个需求相关的代码块引入过来,把相关的放在一起。

![](img/ae8405804756c2f92164d068603922e8.png)

整个任务描述完成后最好点一下输入框右下角的优化Prompt,如果发现优化Prompt之后的内容不符合你的预期说明你

的描述非常的模糊,你应该做的是让你的描述更加的清晰。如果优化Profmpt之后的内容非常的符合你的预期那就说明你

描述的已经很到位了,这时候发送任务需求的执行结果往往也是最好的

面对复杂需求(需要参考的上下文量特别大时)强烈建议进行以下两总选择:

1.拆分需求,将大需求拆分成各个小步骤来完成,这样不但关联的上下文可以减少非常多,还能让任务执行的精准度和

完成度提高非常多(省成本又提升效果的小妙招)

2.使用Claude4的Max模式,这样模型的上下文量大大的获得了提升,可以更好的理解你的需求内容,不至于让需求关

联的上下文丢失掉。但是上下文多了不一定代表任务完成的精准度和完成度就是最好的,且这种方式成本也会更高

(适合技术小白和特殊场景以及不差钱的老板使用)

五、模型选择推荐

在使用Trae进行辅助编码时,发现Trae其实并不是每一个模型的回答都适配的特别的好,比如我在使用GPT-5时,会发

现回复速度非常慢,且TODOList没有成功生成过,LLM的TODO更多只是普通文本的形式展示出来,Gemini2.5Pro的

回复虽然快一点且正常一点,但是TODO List的问题依然存在。目前与TraeAgent最搭配的模型还是Claude4,通过

SOLO的固定模型选择Claude4我们也不难看出来Trae对Claude4其实是做了很多优化适配的。

这里希望Trae可以对GPT-5的响应做一下优化适配,因为GPT-5的幻觉更少更适合某些编码场景。(虽然幻觉少了他的创造力也下降了很多)

# 飞书/Lark OpenAPI MCP

https://www.volcengine.com/mcp-marketplace/detail?name=lark-mcp