
# 智谱 GLM 5：Agent 融入日常生活

> 来源：[https://chatmemo.feishu.cn/docx/CgyRdcBsSoF0zCxZGTAcCL9ansf](https://chatmemo.feishu.cn/docx/CgyRdcBsSoF0zCxZGTAcCL9ansf)

如果让你来协调六七个人的聚会选点，你会怎么做？

每个人从不同的地方出发，有人坐地铁、有人开车，你还得考虑通勤时间的公平性。不能让某个人跑一小时，其他人只走十分钟。

光在群里收集完所有人的出发地，打开地图一个个查，你大概就已经想说……算了，随便选个中间的点吧。

![](img/7798f58d0293898e61f30a8ea5af594f.png)

我把所有人的出发地发给了 Agent ⬇️

![](img/0e124eb5c837a2ba4856f19db1627968.png)

在无人干预的 4 分钟内，它自主调用 40 次地图工具，交错思考 25 轮后，一次性给出了综合各方通勤距离的选点方案，附带每个人的出行建议。

![](img/7042c12260cb80be1113ecf1da348e97.png)

还做出了完整清晰、带有路线方案与用时的可视化路线图：

![](img/a48a4f33ed9667e9971c850b0fa2870b.png)

👉 完成这个任务的是 GLM-5。

智谱过年前最新发布的开源模型，专为多阶段、长步骤复杂 Agentic 任务设计。侧重拆解复杂任务、调用外部工具、多步自主执行。也是 Opus 4.6 等海外模型正在走的方向。

这篇文章，我会分享有关 GLM 5 的一切：

1. 速览 GLM 5 规格参数、使用途径
2. Agent 效果实验与 `Skill` 方法：聚会选点、资讯日报自动生成……
3. 我对 GLM 5 的使用感受，以及 2026 年 Agent 变化的小想法

抛开 Coding 视角，Agent 现在能帮我们什么，也在这个过程中感受 GLM 5 的新提升。

---

## 👉 速览 GLM-5，你所需知道的一切

GLM 5 正式发布前，有个小插曲。

一个叫「Pony Alpha」的匿名模型，悄悄出现在 OpenRouter 上，占据模型趋势榜前列。

![](img/054fbc53682b1b50ce73de7cc0481da4.png)

现在身份揭晓了，就是 GLM 5 的测试版。

GLM 5 已在 Z.ai、智谱清言、BigModel 上线，可以在上述网站直接使用。

![](img/050859a47795a360f6ef02903dcaf7dd.png)

得益于 `Agent` 能力提升，Z.ai 直接在官网推出了 `Agent` 模式，能够自行使用多种工具、技能，交付复杂任务结果，当前可免费体验。

另外，技术方面，GLM 5 新模型的变化相当明显：

+ 参数规模提升明显，`GLM-5` 总参数从 4.7 的 355B（激活 32B），拓展至 744B（激活 40B），预训练数据从 23T 提升至 28.5T，通过更大规模的预训练算力，提升模型通用智能水平
+ 采用全新的异步强化学习框架 `Slime`，提升强化学习后训练流程效率
+ 并且采用了 DeepSeek Sparse Attention 机制，在维持长文本效果无损的同时，大幅降低模型部署成本

——说人话就是：**多、快、好、省。**（不客气，我是翻译侠）

在业内多个公认的主流基准测试中，Coding 和 Agent 跑分均到了开源模型前列。

![](img/e2045e9a2850b0cf6f72d3470c7a3186.png)

> 在多个榜单表现中，基本对齐 Claude Opus 4.5

价格方面，输入 4-6 元/百万 tokens，输出 18-22 元/百万 tokens，是国产模型的主流定价区间。

![](img/96cf011e59e2fcc2db2512b0f3ff1e3a.png)

依然全系开源，遵循 MIT 协议。

且已纳入 `GLM Coding Plan` 计划，兼容 `Claude Code`、`Opencode` 等主流 Agent 工具，也支持在 `OpenClaw` 中接入 GLM 5。是日常在 Agent 中爽用 GLM 模型的好选择。`Coding Plan` 订阅地址：https://bigmodel.cn/glm-coding

本次还发布了专门的 `Z Code` 工具，用户可以用手机远程指挥桌面端 Agent，解决代码、生活办公等复杂任务：

![](img/d1bcd62622129d6e16f921a3789e3621.png)

[Case-ZCode-0211.m4v](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/OLW7bCMFVouLAwxGGncc8vxPnGg)

---

## 1️⃣ 日常生活中的复杂 Agent 任务：多人聚会路线规划

最近新模型们的 Coding 能力越来越强，讲真挺难想出好办法，让非技术的读者们能在短篇幅内，感受到模型的能力提升。

灵机一动，与其强行讲解重构、Debug 效果，让 Agent 融入日常，解决生活中的复杂任务，也是一种有意思的新测试角度。

相信大部分人都经历过聚会选点的难题：约了几个朋友周末聚聚，地址选在哪更为方便？

![](img/3b19e13c4441af0557a052bc7ce1ef17.png)

为了解决这个问题，我们不得不多次在地图 APP 内选点、规划路线、估算时间。人数越多、距离跨度越远，任务也越复杂。

不过，这刚好是个典型「拆解任务、调用工具、多步执行」的复杂任务，简直是用来测试模型 Agentic 能力再好不过的 `Benchmark`。

整个配置只要 1 个高德地图 `MCP`。

> Agent 配置教程附在本节末尾

只要把每个人的出发地和交通偏好列出来，Agent 就能自动找出一个对所有人通勤最公平的聚会地点（最优策略 = 让路程最远的人耗时最短）。

---

### 1）路线规划：GLM 5 效果如何？

为了能有更好的测试效果，我设计了一个完整的 `benchmark` `prompt` ⬇️

也可以用于你的日常使用，对参与者信息稍作调整后，发给配好 MCP 的 GLM 5 `Claude Code` 就行。

![](img/bed56a906e445900f844b228de84e731.png)

![](img/9f683d9d9b006731aa3b4d46d149d010.png)

`prompt` 示例

然后就不用自己操心了。

GLM 5 会自己拆任务，逐一将出发地解析为经纬坐标

![](img/2429d6bf9789dc502c61efe40245f035.png)

搜索地理中心区域的候选聚会点

![](img/58890a4606ae73dc9f28c786aabefd94.png)

可以看到，在执行过程中，GLM 5 充分发挥了 Agentic 模型的自主性和复杂任务适应能力，多次、主动查询了大量不同点位、不同交通的各方耗时情况 ⬇️

![](img/dd91cf34fb4f6be0c021bff0eb543676.png)

并用一次性路径规划接口，交叉计算每个人到各候选点的通勤方案。

最终输出了各人出行方案，包含每个人从哪出发、走哪条线路、坐什么交通、预计多久到达。

![](img/53936fe2744086c0abf711acf6555517.png)

总计整个过程仅需 4 分钟，无人干预，自主推进。

![](img/fe404e50c012f9924f02d5441c2db1e1.png)

相较于人类用户自行翻查地图 APP，分次输入每个人的起点、目标点，Agent 能一次性查询目标地区的多家咖啡店，并考虑不同交通方案，大幅提升此类决策的效率与准确度，整个流程快了太多。

---

### 2）制作可视化路线图，验证可信度

为了能够更好的看看路线规划的效果，我继而要求 GLM 5 做了份可视化路线图：

```
给我做一个浏览器能看的 `HTML` 地图，用地图可视化标点体现出来他们的起点和最终的终点以及大致的路线。用于让参会者一眼明白自己的路线建议方案。
```

它生成的第一份 Google Map 风格路线图是这样的：

![](img/45786b1d7925419e31e006e015a14ccd.png)

非常清晰，标注了各方所在的起点和目的地，还有各自的出行方式、大致路线、时间的完整指引。

如果不是我说，你肯定想不到这是 Agent 生成的临时地图，在聚会时能省下太多决策与寻路精力。（甚至在高德 APP，我们没法做出同时查询多人路线）

刚才的地图偏向 Google Map 风格，我还试着让其一键编程为 Apple Map 风格，这是对应的结果 ⬇️

![](img/9e9f94ea2447c1351affe3b4477b2890.png)

前端 Coding 审美过关，GLM 5 在 Coding 能力上也依然保持了一直以来的优秀水准。

最终，手动与高德地图 APP 的实际导航建议比对，验证方案真实性，基本出行方案与用时结论也都与 APP 近乎一致 ✅

![](img/604c539a065644f607f9a5e54883078c.png)

路线规划是个典型的多轮工具调用的复杂 Agent 任务，甚至通过增加参会人数、距离跨度，能持续提升任务难度。

而测试下来，GLM 5 不负其榜单表现，`Cover` 了该任务所需的 AI 能力。

---

### 附：如何尝试多人聚会选点 Agent 任务？

整个任务只要配置一个 `MCP`：高德地图。

到高德开放平台申请一个 `Key`，把 `MCP` 配置到你的 Coding Agent 环境里（我是 `Claude Code`），就可以开始了。

大致流程如下：

1）安装 `Claude Code`：

如果你还没有 `Claude Code`，可参考我的历史文章 `Agent Skills` 终极指南 。你可在「第二部分：`Skill` 完全教程」中学会如何安装，顺便习得当下最值得学习的 Agent `Skill` 用法。

2）获取地图 MCP `Key`：

高德给了个人开发者每月 15w 次的地图服务额度。

参考 https://lbs.amap.com/api/mcp-server/create-project-and-key ，在 https://console.amap.com/dev/key/app 注册开发者身份并创建应用，即可获取高德 MCP `Key`。这是 GLM 5 Agent 查询地点、路线的关键工具。

![](img/6b041523e0e34abba755877c91022dae.png)

3）让 `Claude Code` 替你配置地图 MCP

在 CC 对话界面中发送以下 `Prompt`：

```
添加 MCP：
{
  &quot;mcpServers&quot;: {
    &quot;amap-maps-streamableHTTP&quot;: {
      &quot;url&quot;: &quot;https://mcp.amap.com/mcp?key=【此处替换为你的 MCP Key】&quot;
    }
  }
}
```

![](img/7da71979ea28071e893df2e8d8f0dd03.png)

Agent 就会自动帮你完成 MCP 的剩余配置，完成后重启 CC。

![](img/958d6800e93d434fccd572c7c784dba2.png)

接着只要按上文示例发送任务消息，AI 就能自行批量查询地点，规划出行路线。

接下来看另一个办公、生活常见的需求：多信源信息聚合。

---

## 2️⃣ 每日资讯日报：多信源信息聚合

日报生成是一个典型的 Agent 任务：多信源、多工具、流水线处理。

有朋友戏称其为：AI 时代的 `Hello World`（程序员梗：学编程的第一步就是让程序输出 `Hello World`）

我用 `Claude Code` 做了一个 AI 资讯日报的 Agent `Skill`：

和 AI 说明想关注的信源，Agent 就能自动抓取、筛选、摘要，生成一份结构化的每日资讯，并把每日的资讯自动部署为好看的网页。

![](img/f2ca5bb81529805a375999759c6c64c3.png)

这是它的整个Agent 设计流程介绍，全程尽可能依靠 Agent 自行处理爬取过程中不同信源的处理问题。

![](img/766915e8052476b4c5ad4a5b4f7d0b54.png)

对于基座模型来说，想要一次性无错完成，也是不小的挑战。

在测试 GLM 5 的过程中，只需要我发送一条 `Skill` 调用指令：

![](img/ef5c383af33d10bb332c35caaf772f83.png)

GLM 5 就作为 Agent 的基座模型，一口气处理了最近 3 天的多个信源的资讯，涵盖 OpenAI、Anthropic 官网 News，X 上的特定关注者、以及部分海外科技资讯网站。

有意思的是 Agent 在这个任务里的工具调用方式。

不同类型的信源有不同的自适应获取策略：

+ `Level 1`：`RSS` 源直接解析 `feed`
+ `Level 2`：普通网页用 `WebFetch` 抓取
+ `Level 3`：需要登录或 `JS` 渲染的页面则调用 `Browser MCP` 操作浏览器，避开反爬机制。

![](img/d738f2aa65c150f856c9bee21b3a7c42.png)

![](img/1d4207717250588e43eaff2af0db8b25.png)

> 更加智能的是，当你要求添加信源时，Agent 能根据需要，自行判断每个信源该用哪种方式，逐一执行，再统一去重、摘要、排版。

这类「重复、多步、有明确规则」的信息聚合任务，特别适合交给 Agent。人来做很累，每天打开十几个网站逐个翻阅；Agent 做就是一条指令的事。

最终，GLM 5 一次性完成了日报的信源抓取、入库、分条摘要、合并日报、网站更新的全过程。相当顺滑、实用。

![](img/907233b7813f0b0732717d7f51d3d85e.png)

![](img/d13605bb8cfbacd8152cd97afaa81bc3.png)

这个 `Skill` 目前已经放在了 GitHub 上供体验：

> 你可以在 `eze-skills` 获取我全部公开的 `Skill`：https://github.com/eze-is/eze-skills

`Daily-news` 也还在迭代中（主要想优化多信源的并行抓取策略），主流程已经可用，感兴趣可以试试。

---

## 🎐 写在最后：当 Agent 融入日常生活

智谱在发布 GLM 5 时，给它定的主题是 Agentic Engineering 时代。

![](img/3a6564d79e0b5f2bf4326e4a8c93bc6c.png)

从实测体感来看，GLM 5 的 Agent 任务执行力确实到位。多步拆解、工具调用、自主推进，完成度又有了质的提升。

社区也生产了一批复杂 Coding 的 Case，你可以在这个视频里，看看 GLM 5 的其他 Coding 效果。

![](img/7426a1e20fc907fc9828628cab7d85b7.png)

[Case-全-0211-1.m4v](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/BOavblLLMobGR6xpnpgcJfb2nRf)

距离上午官宣发布到写文的这段时间内，由于大量用户的涌入，GLM 5 的 API 速率出现了短期波动，`GLM Coding Plan` 也已经在官网售罄。

![](img/07ba8ce4a79d72fc24d5f48964efbf70.png)

只不过 GLM 5 是纯文本模型，多模态版本需要等待后续更新，“贴张参考图让 AI 照着做”这类视觉提示效果不会太好（官方采用 4.6v 联用的方法进行兼容），前端风格迁移设计场景受限。

但正如文章展示的，即使在生活类复杂任务中，不依赖视觉的 Agent 任务空间，也已经相当具备想象力 ⬇️

![](img/a300c20454f5c8ece91c23cc3efcfd1a.png)

![](img/288886a216575368ab88cc59d87c851a.png)

---

这篇文章故意没聊 coding benchmark 测试。

因为时至今日，如果未经精心设计高难 `Benchmark`，你越来越难单凭简单 Case，体感区分国产和海外模型的能力上限。更多模型差异来自于提示习惯与模型自身思考风格的偏好。

然而，从 `Claude Cowork`、`OpenClaw`，再到国内的各类办公 Agent，

今年将有一个趋势值得关注：构成 Personal Agent 的关键拼图，已然初具雏形

![](img/2fa32d8828fa8e7aba3d0684cb37c121.png)

+ `GLM 5` 等 Agentic 模型越来越强，国产模型已足以应对多阶段、长步骤的复杂任务
+ `MCP` 连接现实服务，寻路、抓取、浏览网页、操作 office 文件，Agent 的"手"越伸越长
+ `Skill` 定义场景化的处理流程，让 Agent 按 `SOP` 自主执行
+ `Memory` 沉淀人与 Agent 之间的交互记忆，让 AI 越来越懂你的语境与偏好

2026，Agent 开始走出 IDE，作为每个人的口袋 Agent，处理人人都有的日常复杂需求。

每个人的 Personal Agent，可能比我们想的更快到来。

![](img/c7182674e3c411bf5bdeb446af75acc5.png)
