
# 「圆桌」`MindSearch` 遇见 `Coze` 会擦出什么火花

> 来源：[https://aicarrier.feishu.cn/docx/FuQPdcvpqoAhHsxUp0ncmBHmnKe](https://aicarrier.feishu.cn/docx/FuQPdcvpqoAhHsxUp0ncmBHmnKe)

![](img/2f3950cd45ccca3316b5d0bf8b863858.png)

## `coze`是什么

字节的一个“智能体开放平台”

类似`dify`，它提供工作流画布/插件/豆包等大模型，你可以用单`agent`/工作流/多`agent`的模式搭建你的智能体应用

![](img/f3c0e070cdedf3032648c5bec82cf64d.png)

## 为什么要部署到`coze`

1. 因为最近工作原因在耍`coze`

2. 因为`mindsearch`源码用的免费`duckduckgo`搜索成功率低 延迟高，而`coze`提供了包括`bing`/头条/抖音好几个搜索接口，免费！很快！

3. 因为部署在`coze`后，`mindsearch` 可以作为`coze bot/workflow api`使用，而且-again-免费！

4. `coze`可以通过插件形式调用硅基流动，好了，这样`llm`部分也可以 免费了！

5. 因为`mindsearch`代码配合`lagent`确实有点复杂，工作中也想用，所以希望简化，并且适配其他`llm`和`function call`的格式！

## 难点和工作量

诶？我在写工作`prd`吗 为什么要写难点工作量

1. `coze`支持的是工作流，是画布，代码只能作为一个插件使用，整个`mindsearch`流程需要改造包括`planner loop`和`browseragent`，改成画布上的一个或多个工作流

2. 调用大模型和工具的方式，`planner`和`browseragent`都是和`internlm2.5`支持的`plugin`语法强绑定，比如`<`plugin_end`>`这种，改到`coze`就需要使用通用的`llm`方式，比如`function call`

3. `mindsearch`中异步方式和图节点的增加需要对应的`coze`也支持并发的搜索，总结，节点更新，不然串行的太慢

## 实现方式

主要由两个工作流组成

1. `browseragent`工作流，支持输入多个`node`并发

2. `planner loop`工作流，包括`plan` 代码执行 调用1. 更新图节点

### `browseragent`工作流

这个相对简单 `coze`上也有类似的开放工作流参考，我们做一些定制化改造就可以了

1. 搜索前先理解一下有个`thought`和提取多个搜索词

2. 支持`node`和`query`双重并发

3. 支持输入`parent node`结果和`root query`

4. 类似`browseragent`，支持多轮`loop`（这部分我先省略了，试了很多词`lagent.browseragent`全部是一轮，多轮以后看情况加，

5. 支持`select`并且爬取的部分（`bing`提供的`snipet`很全，而且`coze`的`browser`有点慢，这部分我先省略了，

如图

`node`并发工作流套`query`并发工作流:

![](img/7416ff44e120cba754b99c3d37206707.png)

子工作流:

![](img/3b29f784650532e08a494c00923ebc75.png)

### `planner loop`工作流

这部分是关键，我们要把`mindsearch`涉及的状态信息保留在工作流里，把`planner` `llm`和代码执行和`node`/`edge`信息维护做成无状态的插件（状态也作为查遍输入输出

状态信息有：

1. `history`包括`system query` 和多轮的`thought`+`code`+`node result`

2. `nodes/edges`

3. `last node result`

工作流就一个循环组件，其余组件全部在循环组件内搭建，循环条件：

+ 最多10轮迭代

+ 如果是添加的`response`节点则不用搜索，再次通过`llm`输出最终答案

+ 如果没有新节点，包括`response`节点也没新增，直接返回和`llm`历史对话最后一条(类似源码中的`inner_steps`)

训练工作流里面的过程：

1. `Planner core` (输入包括`history`/`nodes`/`edges`的所有状态，执行`llm plan`，并执行`code`)

2. `Planner post process` (主要是判断以上循环条件，构造查询数据结构）

3. 调用`browseragent`工作流

4. `Browseragent post process`(处理查询结果，更新循环状态)

5. 中间结果输出小卡片（第`i`轮的 思考，图，结果）

![](img/d2de9cab4ea5ad4cfb5b4d80f4eed5ba.png)

### `BOT`配置

直接使用了工作流模式（和工作流对话）所以所有的消息都会触发`mindsearch`流程

主要是现在用`doubao`做工具调用不太理想，群友经常问题没有触发流程（也可能是我`prompt`问题

![](img/5a246032192a5aef2266f1522af8f8da.png)

## 效果示意图

`https://www.coze.cn/store/bot/7414377126654197787?panel=1&bid=6dpair9v89g0j`

![](img/bda49fb562142ea586e2e99242a10c47.png)

![](img/240de8ea1994720de83f87545854ccea.png)

![](img/abf5f924c80a8618288cbe46705bfac9.png)
