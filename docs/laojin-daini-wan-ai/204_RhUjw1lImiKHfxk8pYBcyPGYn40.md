
# Claude Code装了LSP后，Token消耗直接降了40%

> 来源：[https://my.feishu.cn/docx/A4DedhNkUocboDxop2ocLraenXc](https://my.feishu.cn/docx/A4DedhNkUocboDxop2ocLraenXc)

Claude Code更新了`2.0.74`。

它的更新日志上是这么写的。

![](img/1afb652a1c3ad1c420047cf4323c1264.png)

本次重点是这个`LSP`。

## LSP是个啥玩意儿

先别急往下翻，很多人看到`LSP`仨字母就懵了。

`LSP = Language Server Protocol`

说人话：让编辑器"长脑子"的技术。

VS Code里按`Ctrl`点函数名唰一下跳到定义，鼠标悬停显示类型——这些骚操作背后都是`LSP`。

**没有`LSP`**：AI把代码当文本，用`Grep`搜字符串，搜出来一堆还得自己判断。

**有了`LSP`**：AI直接问语言服务器要精确位置，一次到位。

在Claude Code里，`LSP`是隐性的。

运行 `/config`看不到"`LSP`: 开/关"这种选项。

### LSP有三种配置方式

1. VS Code集成（最简单，需要VS Code）

1. `cclsp`社区方案（不需要VS Code）

1. 手动配置`.lsp.json`（不需要VS Code）

如果你用VS Code，`/config`里有两个相关配置：

`Diff tool = auto`

"差异工具"设置。

设成`auto`让Claude Code自动检测你用的IDE。

检测到VS Code后，它才知道去调VS Code的`LSP`。

`Auto-install IDE extension = true`

自动安装IDE扩展。

开着的话，Claude Code会自动给VS Code装个扩展。

这个扩展负责让Claude Code和VS Code的`LSP`通信。

![](img/be6815807bdb2190bc40a15b9237aa76.png)

### 不用VS Code？没关系。

方法二和方法三都不依赖IDE，纯终端也能用`LSP`。

## 为什么你该关心这事儿

说真的，老金我用Claude Code快一年了，一直有个困扰：

### Token消耗太离谱。

你让它改个函数，它先`Grep`全局搜一遍。

再让它找引用，又`Grep`一遍。

让它跳转定义，还是`Grep`。

每次搜索都在烧`Token`。

一个简单的重构任务，轻轻松松几万`Token`没了。

`LSP`能解决这个问题。

根据Milvus团队的实测数据：

**用了`LSP`**之后，`Token`消耗能降低40%以上。

**差距就是这么大。**

## LSP能干啥（7个核心操作）

那`LSP`到底能干啥？

Claude Code从`2.0.30`开始实验性支持`LSP`（需要设环境变量）。

`2.0.74`版本正式上线。

**重要：这些操作你不用手动调用！**

配好`LSP`后，Claude Code搜代码时自动优先用`LSP`而不是`Grep`。

你只管正常提问，比如"这个函数在哪定义的"，它自己会调`goToDefinition`。

老金扒了官方文档，支持这些操作：

1. `goToDefinition`（跳转定义）

点符号，直接跳到定义位置。

不用再`Grep`全局搜了。

2. `findReferences`（查找引用）

函数被哪调用了？

一条命令全列出来。

重构前必看。

3. `hover`（悬停信息）

光标停在变量上，自动显示类型和文档。

不用翻来翻去找类型定义。

4. `documentSymbol`（文档符号）

当前文件所有函数、类、变量，一键列出。

相当于一个mini大纲。

5. `workspaceSymbol`（工作区搜索）

整个项目搜符号。

比`Grep`精准，因为只搜真正的代码符号，不搜字符串和注释。

6. `goToImplementation`（跳转实现）

接口定义了方法，想看具体怎么实现的？

一键跳过去。

7. `incomingCalls`/`outgoingCalls`（调用链）

这个老金我超爱。

函数被谁调用、调用了谁，画个调用图出来。

理解复杂代码逻辑特别好使。

## 三种配置方法（按推荐度排序）

说完好处，该上手了。

老金我折腾了三种方法，给你们排个序。

### 方法一：VS Code集成（最简单）

如果你本来就用VS Code，这是最省事的：

```
# 1、在VS Code终端里启动Claude Code
claude

# 2、运行配置命令
/config

# 3、设置差异工具为auto
# 系统会自动检测IDE，安装VS Code扩展（Beta版）
```

先说说`/config`界面那堆东西：

运行 `/config`会弹出一堆配置项，看着吓人？

老金给你挑重点。

**和`LSP`有关的就两个**：

+   **`Diff tool`**：设成`auto`

+   `Auto-install IDE extension`：保持`true`

**其他常见的**：

+   **`Model`**：你用的模型

+   **`Output style`**：输出风格

+   **`MCP servers`**：后面带✔就是连上了

看到警告别慌，"`invalid settings`"、`config mismatch`这俩不影响使用。

![](img/8e33d0195f9e07d65048a6f4f05483ca.png)

配完后，Claude Code就能用VS Code的`LSP`了。

### 怎么验证`LSP`在工作

老实说，目前官方没有明确的指示器。

没有UI显示"`LSP`已连接"或"`LSP`启动成功"。

这也是很多人困惑的原因。

**间接验证方法**：

1. 问`Claude Code`跳转定义

随便找个项目里的函数，问它：

"`getUserById`这个函数在哪定义的？"

如果它秒回精确位置（文件名+行号），说明`LSP`在工作。

如果它还在`Grep`搜一堆结果然后判断，说明没配好。

2. 用`cclsp`测试

装了`cclsp`的话，它有测试命令可以直接验证。

3. 看`Token`消耗

配好`LSP`后，同样的代码搜索任务，`Token`消耗应该明显下降。

### 方法二：`cclsp`（MCP服务器方案）

如果你不用VS Code，或者官方`LSP`报错，试试这个：

```
# 一键安装配置
npx cclsp@latest setup
```

`cclsp`是社区做的`MCP`服务器。

最牛的是它能**自动修正行列号**。

Github地址：https://github.com/ktnyt/cclsp

为什么这个重要？

因为AI生成的代码位置信息经常不准。

行号差个一两行，直接调`LSP`就找错。

`cclsp`会尝试多种位置组合，智能匹配正确符号。

### 方法三：手动配置`.lsp.json`（折腾党）

项目根目录建 `.lsp.json`文件：

```json
{
  "typescript": {
    "command": "typescript-language-server",
    "args": ["--stdio"],
    "extensionToLanguage": {
      ".ts": "typescript",
      ".tsx": "typescriptreact"
    }
  },
  "python": {
    "command": "pylsp"
  }
}
```

**注意**：配置文件只告诉Claude Code怎么连语言服务器。

语言服务器本身得自己装：

```
# TypeScript
npm install -g typescript-language-server

# Python
pip install python-lsp-server
```

### 老金的Critical Thinking

说完好处，老金我得泼点冷水。

**不是所有人都需要配`LSP`。**

**什么情况值得折腾？**

1. 项目够大（1万行以上）

小项目`Grep`也不慢。

2. 经常做重构

改函数名、移动文件这些操作需要精确找引用。

3. `Token`预算紧张

40%省下来很可观。

**什么情况别折腾？**

1. 只写新代码

从零写功能，`LSP`帮助不大。

2. 官方还不稳定

GitHub上一堆报错，Anthropic还在打磨。

3. 不想折腾

建议等几个版本。

### 目前的坑（2025年12月实测）

老金我踩过的坑，提前告诉你们：

1. `No LSP server available`报错

最常见。通常是语言服务器没装好。

先检查能不能直接运行 `typescript-language-server`。

2. 没有状态反馈

`LSP`服务器启动没、有没有报错，界面里看不到。

出问题只能翻日志。

3. 某些操作偶尔失败

跨文件引用有时候找不全。

官方还在修。

### 老金建议

虽然`LSP`目前还不完美，但方向绝对对。

以前Claude Code写代码像"盲人"，只能靠搜和猜。

有了`LSP`，它终于能"看懂"代码结构了。

代码理解深了，生成质量自然上去。

**顺便说一嘴`Augment`**：

有人问`LSP`是不是和`Augment`一样？

目标相似——都是让AI理解代码结构。

但技术路线不同：`LSP`是标准协议按需查询，`Augment`是自研向量索引持续追踪。

`LSP`轻量免费，`Augment`功能更强但要额外付费。

用`TypeScript`或`Python`的，建议现在就试试VS Code集成。

遇到问题翻GitHub Issues，社区方案不少。

嫌折腾就等几个版本。

不过有一点老金我敢确定：

未来AI编程工具，`LSP`是标配。

谁能更好理解代码结构，谁就能生成更准的代码。

### 参考来源

**`2.0.74`其他更新**：新增`Kitty`/`Alacritty`/`Zed`/`Warp`终端支持、`ctrl+t`切换语法高亮。

完整更新日志看GitHub。

1. `cclsp` - Claude Code LSP MCP服务器

2. Claude Code官方LSP Issue讨论

3. `LSP`插件配置问题Issue #14803

4. `Enable VSCode LSP APIs`性能提升讨论

5. `Milvus Blog` - 为什么反对`Grep-only`检索

6. Claude Code官方插件文档

**老金说**：`LSP`这事儿，现在配好的人等于在大家还用卡带机的时候就买了CD机。技术成熟只是时间问题，先吃螃蟹的人先享受便利。收藏这篇，配置遇到问题了回来看！

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
