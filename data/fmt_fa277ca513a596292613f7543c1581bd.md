
# Claude Code开始失忆了？两大记忆神器帮你选

> 来源：[https://my.feishu.cn/docx/Hj40dBjzNoNXNGx0uTHcmttCnod](https://my.feishu.cn/docx/Hj40dBjzNoNXNGx0uTHcmttCnod)

用Claude Code写代码爽不爽？

爽。

但有个烦人的问题让老金我每天都想骂街——

`claude-mem`每次开新会话都失忆。

你昨天跟它聊了三小时，把项目架构、代码规范、踩过的坑全讲清楚了。

今天一开新会话？

"你好，请问你需要什么帮助？"

次奥，又要从头来一遍。

老金我用Claude Code半年了，每次开新会话都要重复一遍项目背景。

直到发现了这个神器——`claude-mem`。

Github：https://github.com/thedotmack/claude-mem

---

## 30秒速读版（急着走的看这里）

**核心结论**：Claude Code失忆问题有两个主流解决方案——`claude-mem` 和 `mcp-memory-service`。

**谁该关注**：

天天用Claude Code写代码的开发者。

项目周期超过一天的程序员。

受够了每次重复介绍项目背景的人。

**两个方案怎么选**：

+   `claude-mem`：专门为Claude Code设计，两行命令装完就用，简单省事

+   `mcp-memory-service`：通用MCP服务，支持13+个AI客户端，扩展性更强

**老金建议**：

只用Claude Code → 选`claude-mem`（本篇教你装）；

想跨多个AI工具用 → 选`mcp-memory-service`（下一篇详细讲）

**以下是详细介绍（有时间再看）**

## 方案一：`claude-mem`（轻量级选手）

### `claude-mem`是什么？

简单说，它是给Claude Code装的"记忆芯片"。

没有它，Claude Code就像一条金鱼，7秒钟就忘了你是谁。

有了它，Claude Code就像一头大象，永远记得你跟它说过什么。

技术上讲，`claude-mem`是一个持久化记忆压缩系统。

它会自动捕获你和Claude Code的对话，提取关键信息，存到本地数据库里。

下次开新会话，它自动把相关记忆注入进去。

你不用做任何事情，它全自动工作。

## 怎么安装？

老金我第一次看到安装命令的时候，都不敢相信这么简单。

需要先安装PM2：

```
npm install -g pm2
```

**两行命令搞定**：

```
`/plugin marketplace add thedotmack/claude-mem`
`/plugin install claude-mem`
```

没了。

第一行是把插件添加到你的插件市场。

![](img/0e6e05d83c98c9eaf1cacf1c605e6ea5.png)

第二行是安装它。

![](img/3464bec580544284f2cc60513165e870.png)

安装完成后，它会自动启动一个后台服务。

你什么都不用管，它自己就开始工作了。

![](img/b5e6d6058cd450aaf40ad0e9f9a0267b.png)

然后启动：

```
  #安装依赖并启动 Worker

  # 进入插件目录
  cd ~/.claude/plugins/marketplaces/thedotmack

  # 安装依赖
  npm install

  # 启动 worker
  pm2 start ecosystem.config.cjs

  #验证安装

  # 检查 worker 状态
  pm2 list

  # 检查健康状态
  curl http://127.0.0.1:37777/health

  成功返回：{&quot;status&quot;:&quot;ok&quot;,&quot;timestamp&quot;:...}

 #设置 PM2 开机自启（可选）

  pm2 startup
  pm2 save
```

在 Windows 上，~ 路径等同于 C:\Users\<用户名>\，例如：

```
~/.claude/ = C:\Users\admin\.claude\
~/.claude-mem/ = C:\Users\admin\.claude-mem\
```

## 核心功能

### 1. 自动记忆捕获

每次你和Claude Code对话，`claude-mem`都在后台默默记录。

它不是傻乎乎地全部存下来。

它会用AI提取关键信息——

项目架构？记住了。

代码规范？记住了。

你踩过的坑？记住了。

你骂它的话？......也记住了。

### 2. 跨会话记忆注入

这是最牛的功能。

你今天开新会话，`claude-mem`会自动把相关记忆注入进去。

Claude Code一开口就知道你的项目是什么、用什么技术栈、有哪些坑。

不用你再重复介绍一遍。

### 3. `mem-search`语义搜索

这个功能老金我用得特别爽。

你可以用自然语言查询你的记忆库：

```
mem-search: 我之前怎么解决那个数据库连接超时的问题？
```

它会从记忆库里找到相关内容，直接告诉你。

比你自己翻聊天记录快100倍。

### 4. Web界面查看器

`claude-mem`还提供了一个网页界面。

安装后访问 `http://localhost:37777`，你就能看到所有记忆。

可以搜索、删除、编辑。

对于有隐私顾虑的同学，这个功能很实用。

![](img/e2668dda7e2b72420deac8b52bf665b8.png)

![](img/be26a61108562f14ed164a4b3d3c5a3c.png)

浏览器后台可进行设置：

![](img/0b0f593656b898b9c8aa26c47c8e0a7f.png)

## 技术架构（给程序员看的）

`claude-mem`的架构设计挺优雅的。

它用了5个生命周期Hook，自动在会话开始时注入记忆。

数据存储方面，用SQLite存结构化数据，用Chroma向量数据库支持语义搜索。

后台跑的是PM2服务，保证稳定运行。

最聪明的是它的渐进式上下文设计。它会分层检索记忆，优先加载跟当前对话最相关的内容。

所有数据都存在本地，不会上传到任何服务器。

隐私完全可控。

## 和其他方案对比

有人可能会问：`CLAUDE.md`不也能让AI记住项目信息吗？

能，但不一样。

`CLAUDE.md`：你手动写的静态文档，AI每次都会读。

`claude-mem`：自动捕获的动态记忆，根据上下文智能注入。

`CLAUDE.md`适合写项目架构、代码规范这些不怎么变的东西。

`claude-mem`适合记录你的对话历史、踩过的坑、解决过的问题。

两个搭配使用，效果最好。

## 使用建议

### 建议1：直接安装，不要犹豫

964颗GitHub星，说明这东西真的好用。

安装只要两行命令，试试又不会怀孕。

### 建议2：让它自动工作

安装完就不用管了。

它会在后台默默捕获记忆，你完全感知不到。

等你某天开新会话，突然发现Claude Code"好像记得"你之前说的话。

那就是它在起作用了。

### 建议3：善用mem-search

遇到问题先搜一下记忆库。

很可能你之前已经解决过类似问题了。

### 建议4：定期清理敏感信息

如果你对话中涉及敏感信息（密码、密钥之类的）。

可以去Web界面删掉相关记忆。

## 最后说两句

Claude Code是老金我用过最好的AI编程助手。

但"每次都失忆"这个问题，确实让人抓狂。

今天介绍了两个解决方案：

+   `claude-mem`：专为Claude Code设计，两行命令就能用

+   `mcp-memory-service`：通用方案，支持13+个AI客户端

只用Claude Code的同学，装`claude-mem`就够了。

想要更强扩展性的同学，等我下一篇。

## 下一篇预告

《`mcp-memory-service`完整安装教程》

这个工具比`claude-mem`复杂一点，但功能强太多了。

想看下一篇的，点个"在看"让我知道！

---

**今天的内容记住两个命令就行**：

```
/plugin marketplace add thedotmack/claude-mem
/plugin install claude-mem
```

装上试试，你会回来感谢老金我的。

---

**我觉得有用？点个"在看"，下一篇我**讲`mcp-memory-service`！

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
