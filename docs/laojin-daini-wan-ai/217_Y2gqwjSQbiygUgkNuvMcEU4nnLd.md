
# Claude Code 30k+ star官方插件，小白也能写专业级代码

> 来源：[https://my.feishu.cn/docx/N45HdDdhPo7cUexqbhLcRmSAnGc](https://my.feishu.cn/docx/N45HdDdhPo7cUexqbhLcRmSAnGc)

1月18日，Jesse（obra）在GitHub上更新了`Superpowers`插件，star数刚突破3万。

老金我点进去一看，这玩意儿去年10月才发出来，3个月就3万多star，涨得有点夸张。

**更关键的**是，这还是`Claude Code`的官方插件。

## 什么是`Superpowers`

老金我先把话说清楚：`Superpowers`不是独立工具，是`Claude Code`的插件系统。

装上之后，`Claude Code`会自动多出一套"技能树"：

`brainstorming`：帮你理清需求再动手，不是瞎写

`test-driven-development`：强制TDD流程，先写测试

`systematic-debugging`：4步调试法，找根因

`writing-plans`：把需求拆成2-5分钟的小任务

`subagent-driven-development`：子代理流水线干活

还有十几个技能，老金我就不一一列了。

核心就一句话：让AI不要瞎搞，按照专业开发流程来。

## 为什么火得这么快

老金我分析了下，原因大概有三个。

**第一，官方背书**。

作者是Jesse Vincent，`Claude Code`的核心贡献者。这不是野生插件，是亲儿子级别的。

**第二，解决真实痛点**。

你用`Claude Code`写代码，是不是经常遇到这些问题：

AI直接开写，没问你需求细节

写出来的代码没测试

遇到bug就开始乱改

`Superpowers`就是来解决这些的。它让AI在写代码前先`brainstorming`，写代码时强制TDD，debugging时`systematic`。

**第三，开源免费**。

MIT协议，3万star，社区活跃。有问题去GitHub提`issue`，回复挺快。

## 老金实测体验

上周老金我拿一个真实项目测了。

之前的流程：跟`Claude Code`说需求 → AI直接开写 → 发现理解不对 → 重写 → 改bug

装了`Superpowers`之后：

1、`brainstorming`技能触发，问了老金我七八个问题

2、确认需求后生成plan，拆成15个小任务

3、每个任务先写测试，再写代码

4、遇到bug走`systematic-debugging`流程

说实话，第一次被AI问那么多问题，老金我有点懵。

但结果是：**一次性跑通，没有返工**。

老金我又试了第二次，流程顺畅多了。确实比之前高效。

## 怎么装

老金我手把手教一遍。

### 第0步：检查并卸载旧版本（重要）

如果你之前装过`superpowers`，先卸载掉，否则会冲突。

在`Claude Code`里输入：

```
/plugin remove superpowers
```

如果提示找不到插件，说明你没装过，直接跳到下一步。

### 第一步：注册`marketplace`

打开`Claude Code`，输入：

```
/plugin marketplace add obra/superpowers-marketplace
```

![](img/66322aec283a688ccf5483b63e20baf0.png)

### 第二步：安装插件

```
/plugin install superpowers@superpowers-marketplace
```

![](img/2a4c7f533d5c7d48f222635b72031840.png)

### 第三步：验证

```
/help
```

看到这三个命令就成功了：

```
/superpowers:brainstorm
/superpowers:write-plan
/superpowers:execute-plan
```

**这三个命令是干什么的？**

`brainstorm`（头脑风暴）：写代码前，AI会问你一堆问题，把需求搞清楚，避免理解偏差

`write-plan`（写计划）：把需求拆成具体小任务，每个任务2-5分钟能完成

`execute-plan`（执行计划）：按照任务清单逐个完成，边做边验证

**典型流程：**

```
你说："我要做个用户登录"
  ↓
/superpowers:brainstorm → AI问：JWT还是Session?要不要记住登录?
  ↓
你回答完
  ↓
/superpowers:write-plan → 生成任务清单：1.建User表 2.写登录API 3.写前端...
  ↓
/superpowers:execute-plan → AI逐个完成任务
```

就这么简单。

## 老金建议

**如果你是小白**：装。

`Superpowers`会让AI按专业流程带你，避免走弯路。虽然前期问答有点多，但能帮你养成好习惯。

**如果你是老手**：看情况。

如果你已经有一套成熟的工作流，可能觉得它有点啰嗦。但老金我建议至少试试`brainstorming`和`systematic-debugging`，这两个技能对谁都有效。

**老金我的结论**：

`Superpowers`不是魔法，不会让你一夜变大神。

但它能让AI助手更专业、更规范。长期用下来，你自己也会被影响，形成更好的开发习惯。

30k+ star不是白拿的，确实有点东西。

你们觉得呢？有用过的可以跟老金我聊聊体验。

## 参考来源

+ `Superpowers` GitHub 仓库：https://github.com/obra/superpowers

+ 作者博客：https://blog.fsck.com/2025/10/09/superpowers/

+ `Claude Code` 官方文档：https://docs.anthropic.com/en/docs/claude-code

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
