
# Claude Code 接入 Qwen3-Coder-Plus 之后……

> 来源：[https://huchegames.feishu.cn/docx/LZbSdMQuwoXTp7xdWCEcQPFYn1e](https://huchegames.feishu.cn/docx/LZbSdMQuwoXTp7xdWCEcQPFYn1e)

> 🔗 原文链接：https://mp.weixin.qq.com/s/BY81sjhQlbN_rHIwbHZYcQ

> ⏰ 原创 曾嵘 曾嵘胡扯的地方 2025 年 07 月 24 日 09:45 湖北

[昨天，通义千问Qwen ](https://mp.weixin.qq.com/s?__biz=Mzk0ODg4NDI5NA==&mid=2247485224&idx=1&sn=8f6a7ad6633f797942b5dc4c7611322d&scene=21#wechat_redirect)正式发布了 Qwen3-Coder，曾老师在第一时间接入了 Claude Code 并进行了测试。

一直测试到阿里云通知我欠费停服……

![](img/8850ffde8f3168bfc01302854c45acfe.png)

其实这个停服通知是延迟的，后面我又干了半个多小时的活儿就健身去了，回来才发现欠费干不动了。

阿里云允许实例欠费，幸亏我没有在健身之前跑后台任务，不然欠费可能更多。

这中间发生了什么？听曾老师娓娓道来……

+ 退出 Claude Code 登录
+ Claude Code 接入阿里云百炼
+ 设置 `qwen3-coder-plus` 模型
+ 解决 `ERR_BAD_REQUEST` 问题
+ 编码效果如何？
+ 花了多少 token(money)？

## 退出 Claude Code 登录

如果你的 Claude 账号还没有被封，而且之前使用过，那么你的 Claude Code 可能处于登录已鉴权态。

在配置了新的 `ANTHROPIC_APK_KEY` 之后，Claude Code 可能会进行这样的提示：

```
Auth conflict: Using ANTHROPIC API_KEY instead of Anthropic Console key. Either unset ANTHROPIC_API_KEY, or run 'claude /logout'.
```

此时退出 Claude Code 交互界面，在命令行中执行 `claude /logout` 就可以清除掉 Console 中的鉴权信息。

（这里给各位看官埋了个雷，请继续往下看）

## Claude Code 接入阿里云百炼

设置环境变量：

```
export ANTHROPIC_BASE_URL="https://dashscope.aliyuncs.com/api/v2/apps/claude-code-proxy"
export ANTHROPIC_AUTH_TOKEN="sk-******"
```

由于有接入 `kimi-k2` 模型的路径依赖，我一开始使用了 `AUTHROPIC_API_KEY` 这个变量来保存 token，导致请求 401 报错。

所以，一定注意要使用 `ANTHROPIC_AUTH_TOKEN` 这个环境变量。

## 设置 qwen3-coder-plus 模型

访问阿里云百炼平台，可以查询到 `qwen3-coder-plus` 提供了一百万免费 token：

https://bailian.console.aliyun.com/?tab=model#/model-market/detail/group-qwen3-coder-plus?modelGroup=group-qwen3-coder-plus

使用 `ANTHROPIC_MODEL` 环境变量，可以为 Claude Code 设定自定义模型。

```
export ANTHROPIC_MODEL=qwen3-coder-plus-2025-07-22
```

也可以进入 Claude Code 之后，使用 `/model` 命令来设定模型，例如下面这样：

![](img/140448543d4bdf8c17d2c9628c086b9e.png)

诡异的是，这个设定可能没啥用，因为我明确设定了 `qwen3-coder-plus-2025-07-22` 之后，消耗的依然是 `qwen3-coder-plus` 的 token（如下图）。

![](img/00671fc5f72531219cc275806de08231.png)

顺便说一句，免费赠送的一百万 token 在使用 Vibe Coding 的时候，随便几个问题就消耗完了。

## 解决 ERR_BAD_REQUEST 问题

如果你听劝在上面的步骤使用 `claude /logout` 退出了登录，或者你刚刚安装好 Claude Code，只进行了上面的环境变量设置，那么……

在启动 Claude Code 的时候，你可能会碰到下面的错误：

```
Unable to connect to Anthropic services

Failed to connect to console.anthropic.com: ERR_BAD_REQUEST

Please check your internet connection and network settings.

Note: Claude Code might not be available in your country. Check supported countries at https://anthropic.com/supported-countries
```

你当然可以用魔法重新登录。不过既然我们要使用中国的大模型，可以让 Claude Code 跳过登录步骤：

```
# 修改 `.claude.json` 配置文件
vim ~/.claude.json

# 修改其中的 `hasComplateOnboarding` 为 true
hasCompletedOnboarding: true
```

这个配置在 Claude Code 文档中并没有出现，应该是个内部配置，先用着再说吧。它的作用应该是告诉 Claude Code：跳过新手引导，直接开干！

曾老师不得不吐槽一下 Claude Code 的混乱配置：

```
~/.claude.json
~/.claude/settings.json
/claude config
```

## 编码效果如何？

我基于一个正在用 Cursor 开发的项目进行了测试，让 Qwen3-Coder 替代 Cursor 进行开发，主要的工作是写单元测试和重构代码。

这个项目的源码在这里：

https://github.com/zrong/fabik

基于已经完成的代码自动编写单元测试：

![](img/91e79dc45ead94b3b61f284c6fa5464f.png)

### 打断命令并重新布置任务

![](img/2c8619e3b2a51af1e0111dc04cf0c5c0.png)

实际测试已经完成的命令，并自动修复错误：

![](img/b5a756c57d47eca2365e355ccf38d8ff.png)

平心而论，如果没有下面一个主题中提到的问题（穷），我很愿意切换到 Qwen3-Coder。

## 花了多少 token(money)？

使用 `/cost` 命令可以看到当前的花费：

```
> /cost 
  ⎿  Total cost:            $15.69 (costs may be inaccurate due to usage of unknown models)
     Total duration (API):  15m 7.0s
     Total duration (wall): 18h 0m 6.9s
     Total code changes:    483lines added, 471lines removed
     Usage by model:
         claude-3-5-haiku:  5.8k input, 345output, 0 cache read, 0 cache write
     qwen3-coder-plus-2025-07-22:  5.0m input, 19.8k output, 1.8m cache read, 0 cache write
```

阿里云这边的账单是 31.3 人民币：

![](img/a77406d964371f49eca156cf88500c1b.png)

按照阿里云百炼出的优惠政策，如果我在昨天 20:00 以后开始测试的话，15 元人民币就可以搞定上面的任务了。

15 元人民币对 15 美元，真正实现了油电同价。

`Qwen3-Coder` 的价格优惠政策：

![](img/77573a7036ebcd38407af6c2c9ba40a9.png)

https://help.aliyun.com/zh/model-studio/models?spm=a2ty02.30268951.d_model-market.1.fc2974a1IkTV4T&scm=20140722.S_help%40%40#d698550551bob

开心国产大模型崛起，庆幸我在经历了计算机、互联网浪潮之后，还能在 AI 浪潮到来时写代码。

#胡扯AI #AI

---

<https://huchegames.feishu.cn/sync/Hzv6dRSGJsmHgOb4eSHcN1BHntg>

## 这些可以读读

[从30亿收购到被肢解：Winsurf事件撕开AI行业最残酷的生存法则 ｜ 胡扯AI](https://mp.weixin.qq.com/s?__biz=MzA5NzQ3NDM3MA==&mid=2656294269&idx=1&sn=3e723eb2cd98ede5c53b156773af1434&scene=21#wechat_redirect)

[一键生成中文语音播客！NotebookLM支持50种语言（附草台班子立项经验音频）](https://huchegames.feishu.cn/wiki/S1Kdwd15qiIHSYkaCXQcuMR0nIh)

[小团队AI 生存策略：将AI嵌入游戏研发工作流——GGS2025 ｜ 胡扯会议](https://mp.weixin.qq.com/s?__biz=MzA5NzQ3NDM3MA==&mid=2656293864&idx=1&sn=286a6e1d3bfeaa21fffb7bc33bd1f825&scene=21#wechat_redirect)

都刷到这里了，不来个 「点赞」「分享」「在看」 一键三连吗？
