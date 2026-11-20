
# 从构建Agent转向Skills，打造可迁移的大模型应用新范式！

> 来源：[https://qqs7y1hozd1.feishu.cn/docx/JfDAdz82RoU1aExAqwmctdF7nsd](https://qqs7y1hozd1.feishu.cn/docx/JfDAdz82RoU1aExAqwmctdF7nsd)

## 从 Agent 到 Skills 的新范式

Anthropic提出从构建Agent转向构建Skills的新理念。Skills是包含任务说明、工具、专业知识和素材的文件夹，简单易管理且可组合，解决了Agent领域专业度不足的问题。Skills与MCP互补，前者提供专业知识，后者连接外部世界。未来将把Skills作为软件管理，实现知识可迁移。类比而言，Skills就像应用层，让开发者能编码自己的专业和观点，是大模型应用的关键。

Anthropic 分享了一些最新的insight。一个暴论：别再造 Agent 了，开始造 Skills 吧。

![](img/7a14995257de6219e12ae62d8700ea97.png)

早期的时候，他们发现，不同领域的 Agent 底层其实很通用，其实没必要做这么多Agent，财务，数据分析…，全都能用代码搞定。

![](img/3d0bcd4ada93c1250c617dfb8817e08c.png)

Coding Agent is All you Need，代码Agent 就是数字世界的通用接口。

![](img/09634ceb2cc21f56a74d1b0242637c86.png)

但很快遇到了新问题：领域专业度。比如说报税，你是要一个能拿IMO金牌的数学天才，还是一个老会计？

![](img/eed1f99492f3813df860b52e911a891a.png)

答案显而易见。

现在的 Agent 就像天才：

聪明，但缺经验。你投入精力引导能做成事，但它没法很好地吸收你的专业知识，也不会持续学习。

所以他们后来做了 Skills。

## Skills 的本质与设计

Skills 是什么？本质就是文件夹。

![](img/5cd617c744564a75156b8a67097ed947.png)

一个 Skill = 任务说明 `skill.md` + 脚本工具 + 专业知识 + 素材资源。

这种简单是刻意的。能用 Git 管理，能丢云盘共享，能打包发团队。文件作为原语用了几十年，没必要搞复杂。

![](img/39da9a3a56b972f98da5a85f914f3f12.png)

精髓在于上下文窗口管理。运行时只暴露元数据告诉模型有这个能力，真正要用的时候再加载具体内容。这样一个 Agent 能挂上百个 Skills，真正实现可组合。

发布后几周，生态已经冒出几千个 Skills，最有意思的是，非技术岗位也在做 Skills。财务、法务、招聘，都能用这套东西让 Agent 更懂自己的日常工作。

## Skills 与 MCP 的关系

Skills 和 MCP 是什么关系？不是替代，是互补。

![](img/ce1c1fa88a034584e5194a90f2e24236.png)

MCP 负责连接外部世界，Skills 负责提供专业知识。开发者已经开始用 Skills 编排多个 MCP 工具的复杂工作流了。

## 未来方向：将 Skills 作为软件管理

未来方向很清晰：把 Skills 当软件来管理。测试评估、版本控制、依赖声明，这些软件工程的玩法都要搬过来。

![](img/a38f02fb8e31b97b4cbd78dcad6dfd09.png)

当 Claude 自己开始创建 Skills，学习就真正可迁移了。你写下的东西，未来版本能直接用。目标是和 Claude 共事 30 天后，效果远超第 1 天。

![](img/01c12e5a89e37bdbc7f38858fd226d56.png)

最后一个类比收尾：模型像处理器，Agent 运行时像操作系统，Skills 就是应用层。少数公司造处理器和 OS，但几百万开发者在应用层编码自己的专业和观点。

![](img/adfb4870fc017962c435b63a9bea30f7.png)

Skills 就是要打开这一层。往文件夹里放东西，就能让 Agent 更强。

## 总结

所以，别再造 Agent 了，开始造 Skills 吧。
