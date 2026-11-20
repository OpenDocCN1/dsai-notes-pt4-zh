
# MCP不香了，Claude Skills全攻略

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/RMZAdsVmooKSRAxnc2PcRR9FnxG](https://qqs7y1hozd1.feishu.cn/docx/RMZAdsVmooKSRAxnc2PcRR9FnxG)

## 前言

最近 Claude 公司又搞出了个新名堂——Skills（技能），官方的解释是，使用 Skills 可以提升执行特定任务的能力。

有了 Skills，Claude 在处理特定任务时会更给力，比如，可以在本地就能调用 Skills 玩转图片、Excel、Word、PDF 等处理操作。

Skills 其实就是一个文件夹，里面包含了 Claude 在需要时可以加载的指令、脚本和资源等，只有当 Claude 认为某个 Skill 和当前任务相关时，它才会启用，即按需加载。

![](img/ff99a10c273ddbc7c43a0d1a38145568.png)

**Skills 的特点：**

+ 可组合性强：各种 Skills 可以像堆积木一样组合起来，Claude 会自动判断需要哪些 Skills，并协调它们的使用。提供了为特定工作流程拓展 Claude 的能力；

+ 可移植性强：Skills 在任何地方都使用相同的格式。一次构建，即可在所有 Claude 应用、Claude Code 和 API 中使用。并且，通过 Git 版本控制就能在团队里进行共享；

+ 效率高：需要啥就加载啥，绝不浪费，也能减少大量重复的提示词；

+ 功能强大：对于那些传统编程比 token 生成更靠谱的任务，Skills 甚至可以包含可执行代码。

说白了，就是个“技能包”，你可以把 Skills 想象成可定制化的 AI 工具包，它能帮你把专业知识打包起来，让 Claude 能够灵活运用。

**Skills 的分类：**

+ Personal Skills：个人技能，你的所有项目都可以用上的 Skills 哦，位于 `~/.claude/skills/` 目录下；

+ Project Skills：项目技能，项目技能仅对项目生效，方便团队共享，位于每个项目中的 `.claude/skills/` 目录下；

+ Plugin Skills：插件技能，插件也能捆绑一些 Skills，安装后就能直接用，用法和个人以及项目 Skills 是一样的。

### Skills 实战体验

> Skills 功能仅对 Claude Pro、Max、Team 和 Enterprise 用户开放。

Skills 其实就是插件的一种形式，我们通过 `anthropics/skills` marketplace 市场里的插件来安装 Skills，Claude 会在需要的时候自动加载。

#### 添加 Skills 插件市场

首先使用下面的指令来添加插件市场：

> `/plugin marketplace add anthropics/skills`

![](img/2e4d0968c450852cc85fd3320a878c60.png)

也可以输入 `/plugin` 按提示添加插件市场：

![](img/d80eaa2f00854b48ed9a74623f0ca86f.png)

然后输入官方的 Github Skills 地址：

> `https://github.com/anthropics/skills`

#### 安装 Skills 插件

添加市场后会进入安装技能插件：

![](img/3f69b45c7e9ca1f131395f2ead962529.png)

也通过下面的命令来快速安装 Skills：

> `/plugin install document-skills@anthropic-agent-skills`

> `/plugin install example-skills@anthropic-agent-skills`

官方提供的两个技能插件用途：

+ document-skills：文档技能包，可以处理 Excel、Word、PPT、PDF 等文档。

+ example-skills：示例技能包，可以处理技能创建、MCP 构建、视觉设计、算法艺术、网页测试、Slack 动图制作、主题样式等。

使用“方向键 + 空格键”选中需要安装的技能插件，最后按「i」安装即可：

![](img/e89aa3cc278de45d22a5052338927218.png)

![](img/f404e4a0263b59523c0b61c8eed8b03a.png)

安装成功，通过 `/plugin` 命令进入 Select marketplace 可以看到已经添加的技能插件及市场：

![](img/38029884da4d7115acec290c14093fe4.png)

通过 `/plugin` 命令进入 Manage plugins 可以对技能插件进行更新、删除等操作：

![](img/880b3bd0c70be992bb3d20c49235a8c3.png)

安装完成后，去 `~/.claude/skills` 目录下可以看到安装的技能插件：

![](img/2a9c6950dbb38263dcc5bb5f7cf65634.png)

技能都保存在目录里，每个目录都包含一个 `SKILL.md` 文件。所以，你也可以手动把 skills 添加到个人的 `~/.claude/skills` 目录。

#### 使用 Skills 插件

上面说了，Claude 会根据你的任务自动调用相关的 skills，根本不用你手动去选。而且，在它工作的过程中，你甚至能看到 Skills 在 Claude 的思考链里是怎么运作的。

比如，我让它总结这个项目的功能然后生成 PDF：

![](img/1a39fbc396e14f22e3ffdce573e5f8bf.png)

如下图所示，它自动加载了 PDF skill：

![](img/50a2e395cd8508f78071dcb6fa2eba98.png)

![](img/bd316006200595c9c6355fd36b150c20.png)

最终在项目根目录下生成了 `overview.pdf` 文件，按需加载技能插件，而不是全部加载到上下文中，也有效减少了占用上下文的长度。

### 有了 MCP，为什么又搞出 Skills？

MCP 本质上是为 AI 大模型提供调用外部工具的能力，MCP Server 就是这个能力的具体实现——你可以通过它，把你已有的 API、脚本、服务包装成 AI 能理解和调用的 MCP 工具。

**使用 MCP 的限制：**

+ 如果只靠 MCP，你虽然可以调用很多工具／数据，但模型每次必须在提示或上下文里夹带大量相关信息，这会消耗大量 token、降低效率。

+ 在很多场景下，问题不是调用 API，而是按公司标准／流程来做事，MCP 可以访问数据或工具，但不会自动知道这个流程的外在规则是什么。

而 Skills 正好解决了这些问题，所以，MCP 是 AI 连接外部的工具，而 Skills 教模型如何使用工具。

MCP + Skills 可以协同工作，在很多复杂系统中，两者往往组合使用，模型先通过 MCP 访问工具／数据，再通过 Skills 引导流程执行。

### Skills 和 Slash Commands 有什么区别？

Skills 是由模型驱动的，Claude 会根据你的任务和 Skill 的描述自动匹配并使用这些 Skills，完全不需要你介入。

Slash Commands（斜杠命令）则是由用户触发的，你需要主动输入 `/command` 才能触发。

Claude Code 的高级用法看这篇：

> 玩转 Claude Code 的 23 个实用小技巧，效率拉满！！

### 总结

最近 Claude 推出的 Skills 功能，说白了，就是给 AI 加了个「技能包」系统，能让它更聪明地完成特定任务。这个设计不但提升了 Claude 的实用性，也让它更接近「可编程 AI 助手」的终极形态。

首先，Skills 的设计非常工程化，Claude 不再傻傻地一口气加载所有技能，而是根据上下文判断该用什么技能，加载对应的脚本或指令，这点在实际使用中能显著节省上下文长度，尤其适合 token 资源有限的模型环境。

其次，Skills 有很好的组合性与移植性。你可以把多个 Skills 像乐高积木一样组合，形成一个完整的任务流程，比如：读 Excel → 数据处理 → 生成图表 → 输出 PDF，全套操作模型自己就能搞定。

更厉害的是，这套技能在 Claude 所有端都通用，构建一次可以全平台复用。

最后再强调一点，Skills 并不是简单的脚本插件，而是 Claude 内部思维链的一部分。你甚至能看到它在思考时是如何挑选、调用技能的，整个过程可视化、可追踪，非常利于调试和优化。
