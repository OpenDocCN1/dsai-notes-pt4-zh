# Claude Code五件套一篇全解（Plugins/Skills/MCP/SubAgents/Commands）

> 来源：[https://my.feishu.cn/docx/QPOrdSELDoLTtxxploOcGf2xnsc](https://my.feishu.cn/docx/QPOrdSELDoLTtxxploOcGf2xnsc)

## 为什么你的Claude越用越卡？- 老金的血泪教训

先说个血泪教训，很多兄弟都踩过这个坑。

最近帮一位开发者排查问题：他的Claude装了20个插件，响应时间从2秒变成15秒。

打开配置一看：三个搜索工具(DuckDuckGo、Brave、Tavily)功能重复90%，五个文档插件全是Markdown处理器...

我说，老哥，你这不是在用工具，你是在开批发市场。

这是典型的 工具囤积症：看网上推荐就装,从不思考是否真正需要。

老金我刚开始用时，也干过这事。

两周装了30多个扩展，结果:响应越来越慢,经常卡死,最惨的是根本分不清该用哪个。

后来我花整整一天,把所有扩展卸干净,从头梳理逻辑。

发现真正有用的就那么几个,其他都是重复的。

卸掉80%后,效率反而提升了一倍。

后果很直接：

1、每次请求都要遍历所有工具导致响应变慢。

2、工具间功能重叠让Claude不知道该调用哪个。

3、配置文件臃肿让启动加载时间变长。

这就是老金今天要说的: 装对比装多重要10倍。

这5类核心能力就是本文要讲的——

## 五件套全景：它们分别是谁？

很多人分不清这5个东西,老金先用大白话说一遍，让你有个全景图。

1、 **Plugins** (插件): 这是 **打包容器**，用来装其他四个工具的包装盒。

2、 **MCP**** (Mod**el Context Protocol): 这是 外部连接器，让Claude能访问网络、文件、数据库等。

3、 **Skills** (技能): 这是 **自动SOP**，Claude会根据场景自动触发的标准作业流程。

4、 **SubAgents** (子智能体): 这是 **并行工作组**，你一次派出去10个克隆人同时干活。

5、 **斜杠命令**** (Sl**ash Commands): 这是 手动按钮，你输入/release，它就老老实实执行一套固定流程。

### 核心定义对比表

## 你该用哪个？(先看决策树)

在你深入了解每个工具之前，先看这张决策图，对号入座不会错:

**问自己4个问题:**

问题0: 团队需要标准化工具包吗?

如果是 → **Plugins** (打包Skills+Commands+MCP+SubAgents,一键安装分发)

问题1: 这事需要调用外部能力吗?(搜索、文件、浏览器、记忆、数据库)

如果是 → **MCP** (选对应服务器:搜索用exa/brave、文件用filesystem、记忆用memory)

问题2: 这事我会重复做很多次吗?(审查/检查/核对)

如果是 → 自动触发用 **Skills **(Claude检测场景自动执行)

如果是 → 手动触发用** 斜杠命令** (你输入/xxx才执行)

问题3: 这事需要并行处理吗?(多个独立任务同时进行,如性能/安全/探索)

如果是 → **SubAgents** (据说最多10并行,成本N倍,节省50-80%时间)

如果是 → 单任务深度推理用 sequential-thinking MCP (深度思考引擎)

### 从易到难排序

1、 Plugins & Slash Commands (★☆☆☆☆)

学习曲线：最平缓。一个一键安装，一个写Markdown即可。

适合人群：所有人。

2、 Skills (★★☆☆☆)

学习曲线：中等。需要理解YAML和SKILL.md的触发机制。

适合人群：有经验的用户。

3、 MCP (★★★☆☆)

学习曲线：陡峭。涉及JSON配置、服务器安装、环境变量。

适合人群：开发者。

4、 SubAgents (★★★★☆)

学习曲线：最陡峭。涉及并行设计、成本优化、无状态架构。

适合人群：高级开发者。

| **能力** | **Plugins** | **Skills** | **MCP** | **Slash Commands** | **SubAgents** |
| --- | --- | --- | --- | --- | --- |
| 自动化执行 | ✓ | ✓ | ✓ | **✗** | ✓ |
| 文件访问 | ✓ | ✓ | ✓ | ✓ | ✓ |
| 外部API调用 | ✓ | **✗** | ✓ | **✗** | ✓ |
| 并行处理 | ✓ | **✗** | **✗** | **✗** | ✓ |
| 代码执行 | ✓ | ✓ | ✓ | ✓ | ✓ |
| 团队共享 | ✓ | ✓ | ✓ | ✓ | ✓ |
| 状态记忆 | ✓ | **✗** | ✓(Memory) | **✗** | **✗** |

### 快速判断表 (场景速查)

## Part 1: MCP (Model Context Protocol) 深度全解

我们先来讲第一个重量级选手：MCP。

### MCP 是什么？

MCP (Model Context Protocol) 是让Claude突破本地限制、连接外部世界的桥梁。

**本质:**

它是一套标准化的通信协议,让Claude能调用各种外部服务,包括联网搜索(DuckDuckGo、Brave、Exa)、操作浏览器(Playwright自动化)、读写本地文件系统、查询实时文档(Context7、DeepWiki)、访问GitHub仓库、长期记忆存储(Memory)、数据库操作(Postgres)。

还有一点：**本地优先** - 数据不经过第三方,隐私可控。

#### 核心: 解决M×N集成问题

MCP解决的核心问题是 **M×N** 噩梦。

传统方式: 10个AI应用 × 100个工具 = 1000个 集成(每个应用都要单独对接100个工具)

MCP方式: 10个AI应用 + 100个MCP服务器 = 110个 集成(所有应用都说“MCP标准语”)

这就是为什么社区能快速发展到6300+服务器，因为它是一个开放标准。

#### Skills 与 MCP 的关系 (重要澄清)

很多人会混淆这两者，但它们 **不是替代关系**，而是 **正交的**、**可以组合的**。

**MC**P (通信协**议)**: 解决的是Agent与 外部 工具如何 对话 的问题。它是一种标准化的通信接口。

**Skill**s (能力封装): 解决的是Agent如何 思考和行動 的问题。它定义了Agent自身具备的知识、工作流和内部工具（scripts/ 里的脚本就是内部工具）。

组合: 一个Skill里的指令(SKILL.md)可以(也经常)指导Agent如何去调用一个远程的MCP服务器。

老金一句话总结：MCP负责连接，Skills负责驱动。

### 老金精选MCP推荐 (从6300+中筛选)

从6300+社区服务器中筛选出真正高频使用的,都是高星、活跃维护、实战验证的。

#### 必装三件套(90%场景够用)

1、sequential-thinking - 深度思考引擎

npx -y mcp-sequential-thinking

场景: 复杂技术选型(微服务 vs 单体、数据库选型)、架构设计决策(分层、模块边界、扩展性)、疑难bug根因分析(多因素交叉、隐蔽逻辑)、性能优化策略规划。

实战案例: "分析为什么我们的订单服务在高峰期会超时,考虑数据库、缓存、网络、并发锁、GC等所有可能因素"

效果: 会给出完整的推理链:假设→验证→排除→聚焦→验证→结论,而不是直接猜一个答案。

2、context7 - 实时技术文档

npx -y @upstash/context7-mcp

场景: 查最新框架文档(Next.js 15、React 19)、API参数和用法确认、库的最佳实践和陷阱、版本迁移指南。

实战案例: "React 19的use hook怎么用?和Suspense的配合方式?"

效果: 直接返回官方文档相关章节+代码示例,比搜索引擎快10倍。

3、memory - 长期记忆

npx -y @modelcontextprotocol/server-memory

场景: 记住项目约定(命名规范、错误码、配置路径)、记住团队踩过的坑(某个库的bug、某个配置的坑)、记住重要决策(为什么选这个方案、权衡了什么)。

实战案例:

```
你:&quot;记住:我们的错误码统一用E开头+4位数字,数据库表名用snake_case,Redis key用冒号分隔&quot;

(一周后)
你:&quot;帮我设计一个用户登录失败的错误码&quot;
Claude自动调用记忆:&quot;根据你们的规范,应该是E1001这样的格式...&quot;
```

#### 进阶工具(特定场景神器)

playwright - 浏览器自动化

```
npx -y @executeautomation/playwright-mcp-server
```

场景: 竞品监控、批量截图、自动化测试、数据抓取

案例: "打开这5个竞品官网,截图首页和定价页,保存到./analysis/"

filesystem - 本地文件操作

```
npx -y @modelcontextprotocol/server-filesystem/path/to/project
```

场景: 读写配置、批量处理文件、项目初始化脚本

注意: 指定允许访问的路径,避免权限过大。

github - GitHub操作（需要官网key）

需要设置GITHUB_TOKEN环境变量

```
npx -y @modelcontextprotocol/server-github
```

场景: 搜索代码示例、查看issue、分析PR、管理仓库

exa - 智能搜索（需要官网key）

```
npx -y exa-mcp-server
```

场景: 需要搜索最新信息、技术博客、开源项目时,比普通搜索引擎更精准

deepwiki - 深度文档聚合

```
npx -y mcp-deepwiki
```

场景: 聚合某个库的完整文档站点,比context7更全面但速度慢一些

#### 官方及其他稳定版

以下是官方或社区长期维护的稳定服务器：

@modelcontextprotocol/server-brave-search - Brave搜索,查最新资料

@modelcontextprotocol/server-fetch - 抓取网页内容

@modelcontextprotocol/server-postgres - 数据库操作

#### 更多服务器:

官方列表: [https://github.com/modelcon](https://github.com/modelcontextprotocol/servers)textprotocol/servers

社区精选: https://github.com/wong2/awesome-mcp-servers

(500+服务器持续更新,包括Notion、Slack、Jira、数据库等各种集成)

#### 老金的常用组合:

日常开发: filesystem + github + memory

深度工作: sequential-thinking + context7 + memory

自动化任务: playwright + filesystem

学习新技术: context7 + deepwiki + exa (或 brave-search)

### MCP配置实战

老金教你怎么配,拿来就能用。

配置文件位置:

项目根目录.claude/mcp.json:

```
{
  &quot;mcpServers&quot;: {
    &quot;sequential-thinking&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;mcp-sequential-thinking&quot;]
    },
    &quot;context7&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;@upstash/context7-mcp&quot;]
    },
    &quot;memory&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;@modelcontextprotocol/server-memory&quot;]
    },
    &quot;brave-search&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;@modelcontextprotocol/server-brave-search&quot;]
    },
    &quot;playwright&quot;: {
      &quot;command&quot;: &quot;node&quot;,
      &quot;args&quot;: [&quot;/path/to/playwright-mcp-server/index.js&quot;]
    },
    &quot;filesystem&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;@modelcontextprotocol/server-filesystem&quot;, &quot;/path/to/allowed/files&quot;]
    },
    &quot;github&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;@modelcontextprotocol/server-github&quot;],
      &quot;env&quot;: {
        &quot;GITHUB_PERSONAL_ACCESS_TOKEN&quot;: &quot;&lt;YOUR_TOKEN&gt;&quot;
      }
    }
  }
}
```

#### 典型使用场景

场景1: 深度思考

"用sequential-thinking分析:微服务 vs 单体架构,哪个更适合我们20人团队?"

(会给出完整推理过程,考虑团队规模、技术债、维护成本等多个维度。)

场景2: 查最新文档

"用context7查React 19的Server Components最新用法。"

(直接返回官方文档的相关章节和代码示例,比搜索引擎精准。)

场景3: 长期记忆

"记住:我们的数据库是PostgreSQL 15,表命名用snake_case,查询超时设置30秒。"

(下次问数据库相关问题,会自动调用这些记忆。)

场景4: 浏览器自动化

"用playwright打开5个竞品官网,截图首页,保存到./screenshots/"

(自动完成浏览器操作,省去重复手动点击。)

## Part 2: Skills (技能) 深度全解

讲完了外部连接器MCP，我们来讲讲内部的自动SOP——Skills。

### Skills 是什么？

Skill (技能) 是自动触发的能力包,2025年10月发布。

**一句话说清楚：****Skill**是**一个 **标准化的文件夹，用来打包Agent完成特定任务所需的 知识和工具（SOP - 标准作业程序）。

它是一种代码和资源的组织方式，一种Agent能力管理的“设计模式”。

一个Skill文件夹通常包含这几部分：

1、 **SKILL.md** ：**核心文件**。用YAML写元数据（名字、描述），用Markdown写详细的指令，告诉Claude在什么情况下、以及如何使用这个Skill。

2、 **scripts/** ：存放可执行的 **Python、Shell脚本**。

3、 **references/** ：存放 **参考文档**（如API文档、数据库Schema、公司政策等），作为给Claude看的知识库。

4、 **assets/** ：存放 **资源文件**（如PPT模板、Logo、项目脚架等），供Claude在执行任务时直接使用。

**核心机制:**

用YAML frontmatter + Markdown定义,Claude检测到相关场景自动加载。

**技术亮点 (精髓):**

Skills的核心是为上下文窗口减负的 三层分层加载策略 (Progressive loading):

1、 **第一层：元数据** (Name + Description)。这部分信息极简（约100 tokens），会常驻在Claude脑中。当用户提问时，Claude会快速扫描所有Skill的描述，做第一道筛选，成本极低。

2、 **第二层：SKILL.md**** (详细指**令)。当Claude认为某个Skill相关时，它 才会去加载 SKILL.md 里的详细指令，告诉它任务的具体步骤和规则。这步的上下文消耗中等。

3、 **第三层：脚本和参考文档** (scripts/ references/)。只有当 SKILL.md 里的指令明确要求时，Claude才会去读取 scripts/ 里的代码或 references/ 里的文档。这步的上下文消耗是按需的。

这个机制的好处显而易见，**极大地节省了宝贵的上下文窗口**，避免了一次性把所有东西都塞进去。

适合场景:

1、代码提交前自动检查格式和测试

2、写文档时自动应用团队规范

3、检测到依赖变更自动评估风险

4、每次写宣发文案要过品牌规范

**关键:**

Claude自动识别场景并触发,你不用手动调用。

**爽点:**

设置一次,永久受益,错误率能降一半。

### Skills实战进阶: 从入门到治理

老金把Skills的玩法分三个层次,每层都是真实踩过坑后总结的。

#### 第一层: 工程化单点突破 (把说明书变技能包)

把最烦人的重复劳动自动化。少写废话提示词,让Claude自动识别场景。

案例1: 智能代码审查 (不是简单跑Lint)

smart-code-review.md:

```
# 名称
smart-code-review

# 触发
当用户提到&quot;review&quot;&quot;审查&quot;&quot;检查代码&quot;或准备提交PR时

# 执行逻辑
1、分析变更文件类型和影响范围
2、根据文件类型选择检查项:
- API文件 → 检查鉴权、参数校验、错误处理、API文档
- 数据库迁移 → 检查索引、向后兼容、回滚脚本、性能影响
- 前端组件 → 检查无障碍、性能、状态管理、错误边界
- 配置文件 → 检查敏感信息泄露、环境变量、默认值安全性
3、调用对应SubAgent进行专项审查:
- security-auditor → SQL注入、XSS、CSRF等
- performance-engineer → N+1查询、大对象、内存泄漏
- frontend-developer → 重渲染、bundle大小、懒加载
4、汇总问题并按严重程度分级:
- 阻断级(安全漏洞、数据丢失风险)
- 警告级(性能隐患、可维护性问题)
- 建议级(优化空间、最佳实践)

# 输出
分级报告 + 修复建议 + 参考文档链接
```

效果: 以前代码审查靠人工挑毛病,经常遗漏关键问题。现在说"review这个PR",自动按文件类型做专项检查,该找的坑一个不漏。团队线上事故率下降60%。

案例2: 依赖升级风险评估

dependency-upgrade-check.md:

```
# 触发
检测到package.json/requirements.txt/go.mod等变更时

# 执行逻辑
1、识别新增、升级、删除的依赖
2、对每个变更依赖:
- 用github MCP查该库的changelog和breaking changes
- 用context7查新版本的API变化
- 扫描项目代码,找出使用该依赖的位置
- 评估影响面(多少文件、是否核心路径)
3、检查依赖树冲突(peerDependencies、版本兼容性)
4、查询CVE数据库,检查已知漏洞
5、生成升级checklist

# 输出
## 升级风险评估
- 高风险依赖(breaking changes多、使用面广)
- 中风险依赖(小改动、局部影响)
- 低风险依赖(bugfix、无API变化)

## 需要人工验证的点
[列出可能出问题的代码位置]

## 建议升级顺序
[从低风险到高风险,逐步验证]

## 回滚预案
[如果升级出问题,怎么快速回退]
```

效果: 以前升级依赖全靠运气,升了才知道炸不炸。现在每次升级前先跑一遍评估,该注意的点全列出来,升级有底气。

#### 第二层: 编排型工作流 (参数化+链式调用)

一个技能适配多项目,多个技能串成流水线。

案例: 生产事故应急响应

incident-response.md:

```
# 触发
用户说&quot;线上出问题了&quot;&quot;紧急&quot;&quot;事故&quot;等关键词

# 执行流程(自动并行+串行)
## 阶段1:快速止血(并行执行)
- 调用devops-troubleshooter → 分析日志、定位异常
- 调用performance-engineer → 检查资源使用、是否过载
- 调用database-optimizer → 检查慢查询、锁等待

## 阶段2:根因分析(基于阶段1结果)
- 如果是代码问题 → 调用debugger SubAgent
- 如果是配置问题 → 检查最近的配置变更
- 如果是依赖问题 → 检查上游服务状态
- 如果是资源问题 → 检查流量异常、是否被攻击

## 阶段3:止损方案(基于根因)
- 自动生成3个止损方案:
  1、 最快方案(回滚、降级、限流)
  2、 最稳方案(修复根因,重新发布)
  3、 临时方案(绕过问题点,保持可用)
- 对比每个方案的:恢复时间、风险、副作用

## 阶段4:执行确认
- 输出完整的执行命令和回滚命令
- 标注每步的预期结果和验证方式
- 等待人工确认后才执行

# 输出
## 事故摘要
影响范围、严重程度、开始时间、核心指标变化(错误率、延迟、可用性)

## 根因分析
详细分析,附日志/监控截图链接

## 止损方案对比
3个方案的完整对比表

## 执行清单
一步步操作指令,可复制执行

## 事后改进
如何预防类似问题再次发生
```

效果: 以前线上出问题,团队一片慌乱,排查靠经验靠猜。现在说"线上订单服务挂了",5分钟内给出完整分析+3套止损方案。平均故障恢复时间从45分钟降到12分钟。

#### 第三层: 团队智能体 (知识沉淀+自进化)

案例: 技术债追踪助手

tech-debt-tracker.md:

```
# 名称
tech-debt-tracker

# 描述
每周帮助团队识别和追踪最重要的技术债

# 触发
用户提到&quot;技术债&quot;&quot;代码坏味道&quot;&quot;重构计划&quot;时

# 执行逻辑
1、集成SonarQube API,获取现有扫描结果
2、按影响面排序(修改频率高 + bug数多 = 优先级高)
3、生成本周TOP 3待还技术债清单
4、每项包含:位置、问题描述、建议方案、预估工作量

# 输出
Markdown周报:
- TOP 3技术债(本周建议优先处理)
- 已还技术债(上周完成的3项)
- 决策记录(为什么选这些,存入memory)
```

效果: 不再是"想起来就还",而是每周有明确的3个小目标。结合SonarQube等专业工具,Skill负责排优先级和生成报告。

#### 高级: 版本治理+回滚

目标: 把Skills当团队资产,能升级、能回退、可审计。

案例: 技能版本化

```
team-skills/
  code-review/
    v1.0.0/Skill.md
    v1.1.0/Skill.md
    v2.0.0/Skill.md  # 当前版本
```

落地做法:

小范围灰度两版,对比"查出率、耗时、误报率"。

确定没问题再全量,有问题立即回滚。

预期效果: 两周内误报率从18%压到6%。

#### Skills设计的核心洞察:

1、不是写提示词,是 **设计系统**

单个Skill是能力,多个Skill编排是流程,Skill+MCP+SubAgent是智能体

2、让隐性知识 **显性化**

老司机的直觉判断,拆解成可执行的检查项

团队踩过的坑,固化成自动检查规则

3、设计 **自进化机制**

好的Skill会从每次执行中学习

结合memory MCP,形成团队知识图谱

4、追求 **ROI最大化**

不要为了自动化而自动化

5、**AI工程化** 的体现

AI Agent的未来，一半靠模型，另一半靠工程。 Skills提供了一种解耦的、模块化的设计模式来管理Agent能力，这远比维护一个几千行的巨大System Prompt或一个混乱的工具文件要健壮和可扩展。

每个Skill都可以独立版本控制、测试和迭代。

#### Skills是**上下文工程**吗?

老金我在上个文章就讲到，Skills是上下文工程，而且更工程化。

**1、 结构化提示**

把长提示拆成"名称/描述/输入/输出/触发"。

**比裸写清晰很多。**

**2、 渐进式注入**

需要时再加载,避免上下文爆炸。

**不浪费token。**

**3、 参数化与链式**

一套路适配多场景,多套路串流水线。

**复用性强。**

**4、 版本管理 (治理与回滚)**

版本化、灰度、回滚,当资产治理。

这是传统提示词工程做不到的。

**5、 与工具协作**

结合MCP、斜杠命令、SubAgents。

形成完整工作流。

所以Skills就是 上下文工程 的工程化落地。

它让 **怎么说** 变成了 **怎么做**。

## Part 3: SubAgents (子智能体) 深度全解

这是五件套里最科幻的一个，也是成本最高的一个。

### SubAgents 是什么？

SubAgent (子智能体) 是通过**Task工具创建的**独立Claude实例,唯一支持并行的工具。

**本质: **

每个SubAgent是完全独立的Claude副本,你需要:手动指定角色定位(如security-auditor、performance-engineer)、提供必要上下文、明确任务边界和产出。

SubAgent是用户通过Markdown文件(.md)自定义的 系统提示 + 工具配置 组合。

你需要自己定义角色,或复用社区(如 buildwithclaude.com 或 subagents.app)提供的模板。

### 技术特性

1、 无状态: 各SubAgent间不共享内存**,每次都是一次性**执行,需要完整初始指令,不适合迭代式调试 或逐步细化任务。

2、 并行上限: 最多 **10个**** 同时运行 **(第11+个任务将自动排队), 推荐3-5个。

3、 独立Context: 每个SubAgent在隔离的上下文中运行 (推断约100K tokens)。

4、 继承工具: 自动继承主Agent的所有工具 (Task Tool本身除外)。

**5、 **超时: 单个SubAgent最长运行 5分钟。

6、 Token成本: 总Token成本与串行执行N个任务相同 (即 N个Task = N倍单Task成本)。

### 典型场景

```
你:这个支付模块需要全面审查

并行创建5个Task:
1、架构分析SubAgent → 模块设计和接口审查
2、安全审计SubAgent → SQL注入、XSS、CSRF检查
3、性能分析SubAgent → 慢查询和瓶颈定位
4、代码质量SubAgent → 测试覆盖和规范检查
5、数据库SubAgent → 索引和查询优化建议

30分钟返回5份报告(串行需2.5小时)
成本:5倍token,节省80%时间,ROI极高
```

### 重要提示: 成本与ROI

1、 成本模型: 这是一个 **时间换成本** 的策略, 而非 成本优化。线性累加(5个=5倍),但节省时间通常50-80%。

2、 ROI高 (推荐): 3-5倍成本, 节省50-80%时间。

3、 ROI低 (谨慎): 10倍成本, 节省时间有限。

4、 编排方式: 需手动整合各SubAgent产出,不自动汇总。

### 适用与不适用场景

1、 (★★★★★) **极度推荐:** 多角度代码审查、项目规划、文档生成 (任务完全独立)。

2、 (★★★★☆) **推荐:** 文件批处理、API集成测试 (任务独立)。

3、 (★★☆☆☆) **不推荐:** 迭代式功能调试 (因其无状态特性)。

4、 (★☆☆☆☆) **禁止:** 任务间有依赖、敏感数据处理、单一复杂任务 (不需要并行)。

## Part 4: 斜杠命令 (Slash Commands) 深度全解

这是一个简单但极其好用的工具，用于手动标准化。

### 斜杠命令 是什么？

斜杠命令 (Slash Commands) 是手动触发的工作流快捷方式,存在.claude/commands/目录。

**本质:**

纯Markdown定义的多步骤流程,输入/命令名手动触发。

**API注入:**

作为用户消息扩展,而非系统提示工具(这是与Skills的核心区别)。

**命令内部可以:**

调用Skills自动化检查、编排SubAgents并行工作、执行MCP工具链、串联Git操作。

典型场景:

1、 /release - 完整发版流程(测试→构建→打tag→发布→通知)

2、 /security-check - 安全扫描(依赖漏洞+代码审计+配置检查)

3、 /perf-audit - 性能审计(包体积+加载时间+运行时分析)

4、 /refactor-plan - 重构规划(架构分析+影响评估+迁移方案)

5、 /onboard - 新人入职(克隆仓库+环境配置+文档导覽)

6、 /rollback - 紧急回滚

### 与Skills的核心区别

强大之处:

可以在一条命令里编排整个团队最佳实践,新人也能一键执行老司机工作流。

### 斜杠命令实战

老金教你怎么写命令。

第一步: 定义命令

建文件:.claude/commands/release.md

```
# 版本发布流程

1、拉最新代码确认版本号
2、触发Skills检查:代码审查、完整测试、资源检查
3、生成changelog
4、预发布测试
5、人工确认
6、正式发布
7、发通知
```

第二步: 使用

直接输入:/release

第三步: 效果

整个团队按统一流程走,不会漏步骤。版本发布出错率从15%降到0。

## Part 5: Plugins (插件) 深度全解

这是最表层的一个概念，也是最容易被误解的。

### Plugins 是什么？

Plugins (插件) 是应用级打包容器。

重要: Plugins不是第五个独立工**具，而是**用来打包其他四个工具的 打包容器。

一个Plugin可以包含:

1、 5个Skills

2、 10个Slash Commands

3、 3个MCP服务器配置

4、 2个SubAgent定义

5、 若干Hooks

**适合场景:**

1、 团队标准化工具打包、快速分享工作流、一键安装完整套件。

2、 或者, 临时用一次,快速试试某个功能。

**缺点:**

装太多会卡,而且大部分功能都重复。

**老金建议:**

个人用全局配置,团队用Plugins打包分发。别超过3个，够用就行。

## Part 6: 终极实战：五件套组合拳

理论讲完了，我们来看看它们在真实战场上是怎么协同作战的。

### 五件套组合拳实战 (5个真实场景)

老金分享5个真实场景,展示五件套的协同威力。

#### 场景1: 微服务拆分决策(高风险架构决策)

背景: 单体应用用户量暴涨,考虑拆微服务,但怕拆坏了。

组合拳:

```
第1步: 深度思考(sequential-thinking MCP)
&quot;分析我们这个单体应用,是否适合拆微服务? 考虑:团队规模(15人)、技术栈(Spring Boot単体)、QPS(5000)、业务复杂度、运维能力、迁移成本&quot;
-&gt; 返回: 完整推理链, 3个方案对比 (不拆/部分拆/全拆)

第2步: 查技术方案(context7 MCP)
&quot;查Spring Cloud Alibaba的服务拆分最佳实践和Seata分布式事务方案&quot;
-&gt; 返回: 官方文档相关章节, 4种事务方案对比

第3步: 记录决策(memory MCP)
&quot;记住:我们的微服务拆分原则: 1、按业务域拆,不按技术层拆 2、优先拆读多写少的服务...&quot;

第4步: 制定执行计划(SubAgent: backend-architect)
&quot;基于上述决策,制定详细的微服务拆分计划: 1、识别服务边界 2、设计迁移方案(分阶段,可回滚)...&quot;
-&gt; 返回: 6步完整迁移计划
```

结果: 大幅缩短决策周期,从数周降至数天。决策有推理链,方案基于最佳实践,知识沉淀到memory。

成本提示: 适用于重要架构决策,预计消耗1-2万tokens。

#### 场景2: 生产事故快速响应(争分夺秒)

背景: 晚上11点,支付服务突然超时,订单积压。

组合拳:

```
说:&quot;线上支付服务挂了,订单超时率80%&quot;

自动触发Skill: incident-response

阶段1: 并行排查(3个SubAgent同时工作)
- devops-troubleshooter → 扫描日志,发现大量数据库慢查询
- performance-engineer → CPU/内存正常,但数据库连接池打满
- database-optimizer → 发现某个SQL从0.1s变成5s

阶段2: 根因定位(sequential-thinking MCP)
&quot;基于上述数据,分析根因: - SQL为何突然变慢? - 为何今天才出问题?...&quot;
-&gt; 返回: 根因:订单表数据量突破1000万,某个未加索引的查询开始全表扫描

阶段3: 生成止损方案(Skill内部逻辑)
-&gt; 返回: 3个方案对比 (快速/中期/根治)

阶段4: 执行(一键复制命令)
-&gt; 输出可执行命令 (kubectl scale..., ALTER TABLE...)

最后: 记录到memory (memory MCP)
&quot;记住:订单表到1000万行会有性能问题,需提前分库分表&quot;
```

结果: 快速定位根因,生成多套止损方案供选择,知识沉淀。

重要: 所有生产操作命令必须人工审核,禁止直接执行。

#### 场景3: 接手遗留项目(快速上手)

背景: 接手一个2年没人维护的老项目,代码无文档。

组合拳:

```
第1步: 项目全貌扫描(SubAgent: Explore)
&quot;探索这个项目: 1、识别技术栈和依赖 2、找到核心模块和入口...&quot;
-&gt; 返回(10分钟): 项目架构图, 核心模块清单, 风险清单

第2步: 关键逻辑理解(并行调用3个SubAgent)
- backend-architect → &quot;分析用户登录流程的实现&quot;
- database-optimizer → &quot;分析数据库表设计和关系&quot;
- security-auditor → &quot;扫描常见安全漏洞&quot;
-&gt; 返回: 登录流程时序图, ER图, 7个安全风险点

第3步: 技术债评估(Skill: tech-debt-manager)
-&gt; 自动扫描代码,输出: 高优先级技术债(5个), 建议优先偿还的3个

第4步: 依赖升级计划(Skill: dependency-upgrade-check)
-&gt; 检测到: 3个依赖有已知CVE漏洞, 生成分阶段升级计划

第5步: 知识沉淀(memory MCP)
&quot;把这个项目的关键信息记住: - 核心模块和职责 - 数据库表和字段含义...&quot;
```

结果: 1-2天完成初步梳理(以前要1-2周摸索)。输出技术栈、模块、风险清单,并建立项目知识库。

注意: AI生成的架构图需人工验证。

#### 场景4: 新功能完整开发流程(端到端)

背景: 要做一个"用户积分系统"功能。

组合拳:

```
Phase 1: 需求分析(sequential-thinking MCP)
&quot;分析积分系统需求: - 核心功能:赚积分、花积分... - 风险:超发、作弊、性能&quot;
-&gt; 返回: 功能拆解, 技术方案选型(Redis+MySQL双写)

Phase 2: 查技术方案(context7 MCP)
&quot;查Redis分布式锁的最佳实践和积分系统的反作弊方案&quot;
-&gt; 返回: Redisson分布式锁示例, 防御手段

Phase 3: 数据库设计(SubAgent: database-optimizer)
&quot;设计积分系统的数据库表结构...&quot;
-&gt; 返回: 完整DDL, 索引设计

Phase 4: 开发(写代码)
[正常开发过程]

Phase 5: Code Review(Skill: smart-code-review)
-&gt; 自动触发: 检查并发安全, SQL性能, 异常处理...

Phase 6: 提交前检查(斜杠命令: /commit)
-&gt; 自动执行: 运行单测, 检查代码规范, 生成commit message...

Phase 7: 记录(memory MCP)
&quot;记住积分系统的实现: - 用Redis计数器+MySQL持久化...&quot;
```

结果: 完整的端到端流程,技术方案基于最佳实践,代码质量有保障,知识沉淀。

#### 场景5: 技术栈升级(高风险迁移)

背景: Vue 2升级到Vue 3,涉及200+组件。

组合拳:

```
第1步: 影响面评估(SubAgent: frontend-developer)
&quot;分析这个Vue 2项目升级到Vue 3的影响: - 哪些API发生了breaking change...&quot;
-&gt; 返回: 需要改动的文件清单(185个), 不兼容的库(3个), 估算工作量:15人天

第2步: 查迁移指南(context7 MCP)
&quot;查Vue 3官方迁移指南和常见坑&quot;
-&gt; 返回: 官方migration guide, 社区常见问题合集

第3步: 制定迁移计划(sequential-thinking MCP)
&quot;制定分阶段、可回滚的Vue 3迁移计划...&quot;
-&gt; 返回: 5个阶段的迁移计划, 每阶段的回滚方案

第4步: 自动化迁移(Skill: migration-script)
-&gt; 用ast-grep批量替换: this.$xxx → composition API...

第5步: 逐批验证(Skill: migration-validator)
-&gt; 每迁移一批: 运行单测, 运行E2E测试, 检查bundle size...

第6步: 记录(memory MCP)
&quot;记住Vue 2→3迁移的坑: - Element UI要换成Element Plus...&quot;
```

结果: 迁移效率提升30-50%(分阶段验证降低风险),自动化+人工审查减少遗漏,知识沉淀。

重要: 自动化工具需小范围试点后再大规模执行。

## Part 7: 落地指南与答疑

理论和实战都有了，最后是怎么上手和排坑。

### 30分钟快速落地指南 (照着做)

老金写个可复制的落地流程,跟着做就行。

#### 阶段1: 核心三件套(10分钟)

第1步: 装必备MCP(3分钟)

在项目根目录创建.claude/mcp.json:

```
{
  &quot;mcpServers&quot;: {
    &quot;sequential-thinking&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;mcp-sequential-thinking&quot;]
    },
    &quot;context7&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;@upstash/context7-mcp&quot;]
    },
    &quot;memory&quot;: {
      &quot;command&quot;: &quot;npx&quot;,
      &quot;args&quot;: [&quot;-y&quot;, &quot;@modelcontextprotocol/server-memory&quot;]
    }
  }
}
```

重启Claude,验证生效。

第2步: 写第一个Skill(5分钟)

在.claude/skills/目录下创建pre-commit-check.md:

```
# 名称
pre-commit-check

# 描述
代码提交前的标准检查:格式化、Lint、测试

# 触发
当用户提到&quot;提交&quot;&quot;commit&quot;&quot;推送&quot;时自动触发

# 执行逻辑
1、运行代码格式化
2、运行Lint检查
3、运行相关测试
4、生成检查报告

# 输出
分级报告:阻断问题、警告问题、通过项
```

验证: (无需重启)输入:"我准备提交代码了",看是否自动引用并执行。

第3步: 试用SubAgent(5分钟)

创建一个分析任务:

```
请创建一个SubAgent分析当前项目:

角色:项目探索专家
任务:
1、扫描src/目录(排除node_modules和测试文件)
2、识别主要技术栈(框架、语言、主要依赖)
3、列出不超过5个核心模块
4、标注3个明显的技术债或改进点

项目路径: [你的项目绝对路径]
```

验证: 等待2-5分钟,看是否返回结构化报告。

#### 阶段2: 团队定制化(20分钟)

第4步: 记录团队知识(5分钟)

对Claude说:

```
&quot;用memory(MCP)记住我们团队的规范:

代码规范:
- 用ESLint + Prettier
- 命名用驼峰(变量)和帕斯卡(组件)

Git规范:
- commit格式:feat/fix/docs: 描述
- PR必须关联issue

技术栈:
- 前端:React 18 + TypeScript + Vite
- 后端:Node.js + Express + PostgreSQL
&quot;
```

验证: 下次问问题时,看Claude是否会自动调用这些规范。

第5步: 创建团队斜杠命令(10分钟)

在.claude/commands/目录下创建release.md:

```
# 版本发布流程
你是发布助手,帮助用户安全发布版本。

## 执行步骤
1、确认当前Git分支是main
2、拉取最新代码(git pull)
3、触发Skills:
- pre-commit-check(代码质量)
- test-coverage-check(测试覆盖率&gt;80%)
4、询问版本号(major.minor.patch)
5、生成changelog(基于Git commit)
...
## 每步都需要用户确认
不要自动执行,每步输出命令,等用户确认。
```

验证: 输入/release,看是否触发完整流程。

第6步: 优化你的第一个工作流(5分钟)

基于你团队最烦的事,写一个Skill或斜杠命令(代码提交流程、PR创建流程、新人环境配置等)。

#### 验收标准(做到这5点算成功)

1、 说"查React 19最新文档",能调用context7返回结果

2、 说"分析该不该用微服务",能用sequential-thinking深度推理

3、 说"记住我们用PostgreSQL",下次相关问题能自动调用memory

4、 说"准备提交代码",能自动触发pre-commit-check (Skill)

5、 输入/release,能执行完整发布流程 (Slash Command)

做到这5点,你的Claude就从 聊天助手 升级成 智能工作流系统 了。

### 常见问题排查手册

#### MCP相关

Q1: MCP配置后不生效?

1、检查文件路径:.claude/mcp.json

2、验证JSON格式是否正确

3、完全关闭Claude(不是最小化),重新打开

4、输入测试:"列出可用的MCP服务器"

Q2: 提示"command not found"?

检查Node.js是否安装并配置好环境变量(需 >= 18.0.0)

Q3: MCP服务器启动失败?

手动测试: npx -y mcp-sequential-thinking

检查网络代理/防火墙设置,或端口是否被占用

#### Skills相关

Q4: Skill不触发?

1、检查文件在.claude/skills/目录

2、检查文件名是.md后缀

3、检查包含必需字段(名称/描述/触发/执行逻辑/输出)

4、检查触发关键词是否明确

5、调试: "列出当前项目的所有Skills" 或 "执行pre-commit-check Skill"

Q5: Skill触发但报错?

1、检查Markdown格式(如#后有空格)

2、检查依赖的MCP/SubAgent是否可用

#### SubAgent相关

Q6: SubAgent调用失败?

1、检查网络: ping api.anthropic.com

2、降低并发(建议≤3个)

3、给更清晰的指令,明确角色和缩小范围

Q7: SubAgent结果不符合预期?

1、明确角色定位、缩小分析范围、指定输出格式、提供必要上下文

#### Memory相关

Q8: Memory存储后查询不到?

1、重启Claude

2、用明确的分类标签: 记住【代码规范-命名】:xxx

3、验证: 查询memory中所有标签包含"代码规范"的条目

Q9: Memory记忆混乱?

1、清理: "列出memory中的所有条目", "从memory中删除标签为'旧规范'的条目"

#### 性能相关

Q10: Claude响应越来越慢?

1、减少MCP服务器(建议≤5个)

2、清理不常用的Skills

3、清理memory过期记忆

4、重启Claude

Q11: Token消耗过快?

1、精简Skill的执行逻辑

2、SubAgent调用时限定范围

3、Memory存储结构化知识(非长文)

## Part 8: 核心要点总结

最后，老金帮你把所有内容再浓缩一遍。

### 核心要点速记(必读)

花30秒理解这5个关键概念,避免99%的误解:

1、 **Plugins ≠ 第五个工具**

错误理解: Plugins是和Skills、MCP平级的第五个工具

正确理解: Plugins是 **打包容器**,用来装其他四个工具

实际用法: 团队标准化工具包,打包后一键分发

2、 Skills vs 斜杠命令的本质区别

对比: Skills vs Slash Commands

触发: Skills是 **自动的****(Cl**aude识别)；Slash Commands是 手动的(输入/)

适合: Skills适合 **频繁重复任务****；Slas**h Commands适合 标准化流程

**举例: ****Skil**ls是检测到"commit" 自动检查；Slash Commands是输入/release 执行发布

记住: Skills是 自动驾驶,斜杠命令是 定速巡航

3、 **MCP解决的核心问题**

传统方式: 10个AI应用 × 100个工具 = 1000个集成(噩梦)

MCP方式: 10个AI应用 + 100个MCP服务器 = 110个集成

节省: **890个** 集成工作量 (这就是为什么社区能快速发展到6300+服务器)

4、 **SubAgents的成本模型**

时间: 并行节省50-80%(5小时→1小时)

成本: 线性增长(5个SubAgent=5倍token)

ROI: 成本5倍,节省4小时,如果时薪$50,实际赚$200-($50x5倍token成本)

结论: 只要时间值钱,SubAgents就划算

5、 **难度排序(从易到难)**

Plugins(5分钟) → 斜杠命令(30分钟) → Skills(2小时) → MCP(4小时) → SubAgents(1天)

新手建议: 先用前2个,够用了再学后3个

### 速记口诀(帮助记忆):

Plugins打包全家桶,Skills自动命令手。

MCP连接外部界,SubAgents并行最省时。

### 按用户类型推荐 (新)

### 推荐的学习路径 (4周计划)

第1周：基础三件套 (Plugins + Slash Commands)

1、第1-2天：安装3-5个常用Plugins,理解作用域。

2、第3-4天：创建3个常用Slash Commands,学习写Markdown流程。

3、第5-7天：将日常任务(如Git提交)用Slash Commands固化。

第2周：进阶Skills + MCP

1、第1-3天：创建5个Skills,理解自动触发机制和SKILL.md格式。

2、第4-5天：安装MCP核心三件套(sequential-thinking, context7, memory),配置mcp.json。

3、第6-7天：尝试让Skills调用MCP (如Skills检测到"查文档"时,自动调用context7)。

第3周：高级SubAgents

1、第1-2天：理解SubAgent的无状态限制和并行设计模式。

2、第3-4天：创建3-5个SubAgent定义(如security-auditor, performance-engineer)。

3、第5-7天：测试并行执行,并分析成本与ROI。

第4周：整合与优化

1、第1-2天：将团队的Skills/MCPs/Commands打包成一个Plugin,分享给同事。

2、第3-5天：监控Token使用,排查性能瓶颈。

3、第6-7天：建立团队的Skills库和最佳实践文档。

### 写在最后

过去3年,老金见过太多团队在AI工具上走弯路。

有的装了30个插件,结果卡得动不了。

有的买了一堆付费工具,结果都吃灰。

有的花3个月自己搞,结果发现官方早有现成方案。

问题不是工具少,而是不知道什么时候用什么。

这5件套就是老金踩了无数坑后,提炼出的最小必要集:

1、 **Plugins** - 打包容器(整合Skills、Commands、MCP、SubAgents于一体)

2、 **MCP** - 连接外部世界(6300+服务器:搜索、文件、浏览器、记忆)

3、 **Skills** - 自动化重复劳动(检测场景自动触发,progressive loading)

4、 **SubAgents** - 并行处理任务(唯一支持并行,最多10个,成本线性增长)

5、 **斜杠命令** - 标准化团队流程(手动触发,一条命令执行完整工作流)

各司其职,组合威力大于单打独斗。

### 给你的3个建议:

1、不要贪多,先用3件套起步

MCP: 必装memory + context7 + sequential-thinking

Skills: 先写3个(代码检查、文档同步、依赖评估)

SubAgents: 遇到复杂问题再调度,不用提前学

其他的,等这3个用顺了再说。

2、设计自己团队的 工作流剧本

把团队最烦的3件事自动化:

1、 代码提交流程(自动检查+commit)

2、 生产事故响应(并行排查+生成方案)

3、 新人入职流程(自动化环境+知识导入)

这3个做好,投入产出比最高。

3、让知识沉淀,而不只是用一次

用memory MCP记录:

1、 团队踩过的坑

2、 重要的架构决策

3、 经常用的配置和规范

好的工具链会自己长知识。

老人的经验能传承给新人,新人犯的错不会重复。

最后说一句。

当你不再问"这个工具怎么用",

而是问"怎么让工具帮我思考"的时候,

你就真正进入AI协作的新阶段了。

工具的终极形态,不是替代你,而是放大你。

这5件套,就是让你从 **1"个人** 变成 **1个团队** 的杠杆。

用好了,一天顶一周。

用不好,一周不如一天。

差距就在这。

技术更新迭代的速度只会越来越快。

今天你刚学会这5件套,明天可能就出来第六件、第七件。

但底层的逻辑是不变的：

怎么把一个复杂的问题,拆解成标准化的步骤？

怎么让机器(AI)去干重复的活,让人(你)来做决策？

真正拉开差距的,不是你会用几个工具,

而是你能不能把这些工具编排起来,解决一个真实、复杂的问题。

当你不再需要记住每个工具怎么用,而是直接说出你要什么,

工具就能自己组合出解决方案——

这才是AI时代真正的 **效率革命**。

不是做得更快,

而是把重复的交给机器,把创造留给人。

**当一**句话就能触发一段工作,我们要重新定义什么叫 合作。

这套方法论,就是帮你搭建自己工作流的脚手架。

能不能用好,真正把效率提上去,就看你的了。

希望能给你点启发。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>