
# Claude Code终于有仪表盘了：3条命令装个HUD，上下文用了多少一眼就知道

> 来源：https://my.feishu.cn/docx/HpzudjuOCo8NNGxyHmYc5tpln8b

老金我最近用Claude Code，遇到一个特别烦的事。

写着写着，突然蹦出来一句`context window is getting full`。

然后AI就开始犯傻了——回答变短、逻辑变乱、之前说好的方案全忘了。

每次遇到这种情况，老金我都想骂人。

## 问题出在哪？

Claude Code的终端界面，压根看不到上下文用了多少。

你只能输入`/context`手动查，但谁写代码的时候会记得去查这个？

等你发现的时候，已经晚了。

## 先说结论：这个插件解决了什么问题

老金我前两天在GitHub上发现了一个叫`claude-hud`的插件，作者是Jarrod Watts。

装上之后，终端底部会多出一个实时状态栏，长这样：

![](img/3a949dd5649628e2b1119f412649ca4b.png)

一眼就能看到：

+   当前用的什么模型（Opus/Sonnet）

+   上下文窗口用了多少（进度条，绿→黄→红）

+   你的订阅额度还剩多少（Pro/Max用户）

+   当前Git分支和状态

+   AI正在调用什么工具、跑什么Agent

老金我用了两天，感觉就像开车从没有仪表盘变成有仪表盘了。

以前是盲开，现在是心里有数。

## 安装：真的只要3条命令

老金我最怕那种安装步骤写了两页纸的工具。

这个不一样，3条命令搞定，老金我实测不到1分钟。

### 第1步：添加插件市场

```
```/plugin marketplace add jarrodwatts/claude-hud
```
```

### 第2步：安装插件

```
```/plugin install claude-hud
```
```

### 第3步：配置状态栏

```
```/claude-hud:setup
```
```

装完立刻生效，不用重启。

**Linux用户注意**：如果安装报错`EXDEV: cross-device link not permitted`，先跑这个：

```
```mkdir -p ~/.cache/tmp && TMPDIR=~/.cache/tmp claude
```
```

然后在这个会话里重新安装就行。这是Claude Code平台的一个已知问题，不是插件的锅。

## 核心功能拆解：它到底能看到什么

老金我把这个插件的功能一个个拆给你看。

### 1、上下文健康度（最有用的功能）

```
```Context █████░░░░░ 45%
```
```

这个进度条会变色：

+   绿色：安全，随便用

+   黄色：注意了，开始控制输出

+   红色：快爆了，赶紧`compact`或者开新会话

老金我觉得这是整个插件最值钱的功能。

以前上下文爆掉都是事后才知道，现在随时能看到，心里踏实多了。

到了85%以上，它还会自动显示token明细，告诉你哪部分占用最多。

### 2、订阅额度监控（Pro/Max用户福音）

```
```Usage ██░░░░░░░░ 25% (1h 30m / 5h)
```
```

如果你是Claude Pro或Max订阅用户，这个功能太实用了。

它会实时显示你的5小时滑动窗口用了多少，还剩多少时间重置。

7天额度快到的时候也会提醒（默认80%阈值）。

老金我之前经常用着用着突然被限速，完全不知道额度花到哪去了。

现在一看进度条，心里有数，该省省该花花。

### 3、工具活动追踪

```
```◐ Edit: auth.ts | ✓ Read ×3 | ✓ Grep ×2
```
```

这行显示AI当前正在干什么——在读文件、在编辑代码、还是在搜索。

老金我一开始觉得这个功能可有可无。

但用了之后发现，当AI卡住不动的时候，看一眼这行就知道它是在思考还是在跑工具，不用干等着瞎猜了。

### 4、Agent状态追踪

```
```◐ explore [haiku]: Finding auth code (2m 15s)
```
```

如果你用了子Agent（比如`Task`工具派出去的探索Agent），这行会显示：

+   Agent名称和用的模型

+   正在干什么

+   已经跑了多久

老金我经常派Agent去搜代码，以前完全不知道它跑到哪了。

现在一看，"哦，已经跑了2分钟了，还在找"，心里有底。

### 5、Todo进度追踪

```
```▸ Fix authentication bug (2/5)
```
```

如果AI在用`TodoWrite`跟踪任务，这行会显示当前进度。

比如5个任务完成了2个，一目了然。

## 配置：3种预设，随你选

装完之后，运行`/claude-hud:configure`可以自定义。

它会引导你选一个预设：

![](img/474dd5c7006ae084fcb49bef95d8d927.png)

选完预设之后，还能单独开关每个功能。

老金我用的是`Essential`，再手动开了工具追踪。

`Full`太满了，`Minimal`又太少，`Essential`刚刚好。

配置文件在`~/.claude/plugins/claude-hud/config.json`，也可以直接改JSON。

## 技术实现：为什么它能做到

老金我扒了一下源码，给你说说它是怎么实现的。

### 核心原理：用的是Claude Code原生的statusline API

```
```Claude Code → stdin JSON → claude-hud → stdout → 终端显示
           ↘ transcript JSONL（工具、Agent、Todo数据）
```
```

简单说就是：

+   Claude Code会通过`stdin`把当前状态（模型、token用量等）传给插件

+   插件同时解析`transcript`文件，获取工具和Agent的活动信息

+   处理完之后输出到终端底部的状态栏

### 几个技术亮点

+   Token数据是Claude Code原生提供的，不是估算的，准确度很高

+   每300毫秒刷新一次，基本是实时的

+   纯`TypeScript`实现，零运行时依赖（只有`devDependencies`）

+   代码结构很清晰，13个源文件，每个职责单一

老金我看了一下`src/index.ts`，入口函数很简洁：读`stdin` → 解析`transcript` → 读配置 → 获取Git状态 → 获取用量 → 渲染输出。

标准的管道式处理，没有花里胡哨的东西。

这种设计老金我很欣赏：不搞额外窗口，不依赖`tmux`，就用原生API，简单直接。

## 有没有坑？老金我说几个

用了两天，说说遇到的问题：

### 1、API用户看不到Usage

如果你是用`API Key`而不是订阅账号登录的，`Usage`那行不会显示。

因为`API`是按量计费，没有速率限制的概念。

### 2、工具/Agent/Todo默认是关的

装完之后你可能觉得"就这？只有两行？"

别急，工具追踪、`Agent`追踪、`Todo`追踪默认都是关闭的，需要手动开启。

跑一下`/claude-hud:configure`就能开。

### 3、Windows路径问题

老金我在Windows上用，偶尔Git状态显示不出来。

重新跑一下`setup`就好了，不是大问题。

## 老金建议：谁该装这个

### 强烈推荐装的

+   Claude Code重度用户（每天用超过2小时的）

+   Pro/Max订阅用户（需要监控额度的）

+   经常用子Agent的（需要追踪Agent状态的）

### 可以不装的

+   偶尔用一下Claude Code的轻度用户

+   对终端界面有洁癖、一行多余信息都不想看的

老金我觉得，只要你每天用Claude Code超过1小时，这个插件就值得装。

光是上下文监控这一个功能，就能帮你避免很多"上下文爆掉重来"的痛苦。

## 总结

`claude-hud`这个插件，解决的是一个很小但很痛的问题：让你看到Claude Code的运行状态。

3条命令安装，零配置就能用，想定制也很方便。

老金我用了两天，最大的感受就是——以前用Claude Code像开没有仪表盘的车，现在终于知道油还剩多少了。

项目地址：https://github.com/jarrodwatts/claude-hud

有问题随时问老金我，你们用了之后有什么发现，评论区聊聊。

https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc
