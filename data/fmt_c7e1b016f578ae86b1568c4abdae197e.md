
# Claude官方开源16个技能库，老金帮你挑出5个必装神器

> 来源：[https://my.feishu.cn/docx/PmJ3dLa8too1qWxWzTOcv2PXnag](https://my.feishu.cn/docx/PmJ3dLa8too1qWxWzTOcv2PXnag)

周五晚上捣鼓Claude Code，发现Anthropic官方开源的库，老金安装后忘了讲给各位小伙伴了。

挺通用的，建议大家都安装了。

近29k星，2.6k fork。

16个官方出品，文档处理到创意设计全覆盖。

关键是——**免费的，装上就能用。**

今天把最值得装的5个分享出来。

## 先说这个库是什么

官方仓库地址：`github.com/anthropics/skills`

**简单说**：Anthropic把内部用的Claude插件打包开源了。

分两个包：

1、`document-skills`（4个）

处理文档的，Word、PDF、PPT、Excel。

这4个是Claude.ai处理文档的底层能力，现在开源了。

2、`example-skills`（12个）

创意和开发类，做GIF、设计图、测试网页、创建`MCP`服务器等。

两个包加起来16个。

挨个测了一遍，下面说说哪些值得装。

## 怎么装？30秒搞定

打开Claude Code，输入两条命令：

```
/plugin marketplace add anthropics/skills
```

![](img/a7f1bf1ad848f1c3995b394f0b3bbe31.png)

然后安装想要的包：

```
/plugin install document-skills@anthropic-agent-skills
/plugin install example-skills@anthropic-agent-skills
```

装完重启Claude Code就能用了。

第一次装还担心麻烦，结果真的就两条命令，30秒搞定。

## 怎么验证装成功了？

**方法1：查看插件列表**

输入这条命令，Tab切换切页。：

```
/plugin list
```

看到`document-skills`和`example-skills`在列表里就是成功了。

![](img/dbb7aa217cc0ad3156894b6e3cda40fe.png)

**方法2：直接问Claude**

在Claude Code里输入：

> "什么skill能用?"

Claude会列出所有已安装的Skills。

看到`pdf`、`skill-creator`、`webapp-testing`这些名字出现，就是装好了。

## 装好了怎么用？

重点来了：Skills是自动激活的，不用手动选！

这是很多人困惑的地方。

Skills不是像菜单一样让你点选的。

你只需要正常和Claude对话，描述你想做的事，Claude会自动判断用哪个Skill。

**举个例子**：

你想处理PDF，不用说"用PDF Skill"。

直接说：

> "@文件 帮我提取这个里面的说明"

然后把PDF文件拖进去。

Claude自动识别到你在处理PDF，就会调用`pdf`这个Skill。

你会看到它开始提取文字、分析内容。

![](img/821eb14d537b149c35159eb8b7ec8151.png)

**再举个例子**：

你想创建自己的插件，直接说：

> "帮我创建一个自动生成周报的插件"

Claude识别到你要创建Skill，自动调用`skill-creator`。

然后一步步问你需求，帮你生成`SKILL.md`文件。

**简单说**：别管什么Skill不Skill，正常说话就行。

Claude比你聪明，它知道该用哪个。

![](img/8c15cecb21865f10c8c34e2647e7c667.png)

## 精选：5个必装

**先说清楚**：这5个不能单独装，装了上面两个包就全有了。

PDF在`document-skills`包里，其他4个都在`example-skills`包里。

执行那两条`/plugin install`命令，5个就都装好了。

---

### 第1个：PDF（必装指数：⭐⭐⭐⭐⭐）

**所在包**：`document-skills`

**能干什么**：

提取PDF里的文字、表格、表单字段。

还能合并、拆分PDF，填写表单。

**我怎么用的**：

上周客户发来一份30页合同，让我帮他看看有没有坑。

以前得一页页翻，现在直接说：

> "提取这份合同里所有涉及付款的条款"

唰一下，Claude自动找出来了。

3分钟搞定，以前得翻半小时。

**适合谁**：经常处理PDF合同、报告、表单的朋友。

### 第2个：`skill-creator`（必装指数：⭐⭐⭐⭐⭐）

**所在包**：`example-skills`

**能干什么**：

教你创建自己的Skills。

"用来创建插件的插件"，有点套娃，但很实用。

**我怎么用的**：

想做一个"五金店报价"功能。

自动根据产品型号生成报价单。

以前不知道怎么写，现在直接说：

> "帮我创建一个五金产品报价插件"

它会一步步问你：

想让它干什么？

需要什么输入？

输出格式是什么？

问完自动生成`SKILL.md`文件，稍微改改就能用了。

**适合谁**：想自定义Claude能力的朋友。

### 第3个：`webapp-testing`（必装指数：⭐⭐⭐⭐）

**所在包**：`example-skills`

**能干什么**：

用`Playwright`测试本地网页。

能截图、查看日志、验证UI。

**我怎么用的**：

写了个小工具，想看看手机端显示正不正常。

直接说：

> "在800x600分辨率下截个图"

Claude自动打开浏览器、调整分辨率、截图。

比自己开浏览器调试快多了。

**适合谁**：写前端、做网页的朋友。

### 第4个：`mcp-builder`（必装指数：⭐⭐⭐⭐）

**所在包**：`example-skills`

**能干什么**：

教你创建`MCP`服务器。

想让Claude连接外部API？这个很有用。

**我怎么用的**：

想让Claude直接查五金店的库存系统。

直接说：

> "帮我创建一个库存查询的`MCP`服务器"

它会生成`Python`代码，复制粘贴就能跑。

以前看`MCP`文档头大。

现在10分钟就能搭一个。

**适合谁**：想让Claude连接自己系统的朋友。

### 第5个：`brand-guidelines`（必装指数：⭐⭐⭐）

**所在包**：`example-skills`

**能干什么**：

自动应用品牌配色和字体。

这个是Anthropic自己用的，现在开源了。

**我怎么用的**：

做PPT的时候，总是配色配得丑。

装了这个后，说：

> "帮我用专业的配色方案调整这个设计"

Claude自动按设计规范调整颜色。

不是设计师，但效果比自己配的好看多了。

**适合谁**：做汇报、做PPT、需要设计感的朋友。

## 其他11个简单说

剩下的也列一下，按需安装：

**文档类（3个）**：

`docx`（Word处理）、`pptx`（PPT处理）、`xlsx`（Excel处理）

**创意类（4个）**：

`algorithmic-art`（代码艺术图）、`canvas-design`（视觉设计）、`slack-gif-creator`（Slack动图表情）、`theme-factory`（主题样式）

**开发类（3个）**：

`artifacts-builder`（`React`复杂组件）、`frontend-design`（前端设计）、`template-skill`（技能模板）

**办公类（1个）**：

`internal-comms`（内部邮件、周报、FAQ）

文档类的3个值得一起装，处理文档是刚需。

创意和开发类看个人需求。

## 几个注意事项

**1、开源协议**

`example-skills`是`Apache 2.0`，完全开源。

`document-skills`是`source-available`，可以看代码但不能商用。

自己用没问题，别拿去卖。

**2、需要付费版**

Skills功能需要Pro、Max、Team或Enterprise版本。（中转站不用管，都能用）

免费版用不了，别白折腾。

**3、有些需要额外配置**

比如`webapp-testing`需要装`Playwright` `MCP`服务器。

`brand-guidelines`需要配置品牌色。

装之前看一下`SKILL.md`里的Requirements。

## 总结

Anthropic这波开源挺良心的。

用了一周，确实提升了不少效率。

特别是PDF和`skill-creator`，现在天天用。

如果你用Claude Code，这16个建议都装上试试。

反正免费，装了不好用再卸载就行。

有什么问题欢迎留言，咱们一起交流。

## 参考来源

Anthropic Skills官方仓库：`github.com/anthropics/skills`

Claude Code Skills文档：`code.claude.com/docs/en/skills`

Agent Skills标准：`agentskills.io`

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
