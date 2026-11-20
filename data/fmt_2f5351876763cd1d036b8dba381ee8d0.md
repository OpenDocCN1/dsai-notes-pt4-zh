
# Claude Code 接入 Kimi-K2 模型

> 来源：[https://huchegames.feishu.cn/docx/NcqkdlEoDoL6N2xZwzdcnFwHnIc](https://huchegames.feishu.cn/docx/NcqkdlEoDoL6N2xZwzdcnFwHnIc)

> 🔗 原文链接：h[ttps://mp.weixin.q](https://mp.weixin.qq.com/s/kwnCOahLJEdIIU_QLKb7OQ?scene=1&click_id=7)q.com/s/kwnCOahL...

> ⏰ 原创 曾嵘 曾嵘胡扯的地方2025年07月26日 10:12  湖北

周四的文章 [Claude Code 接入 Qwen3-Coder-Plus 之后……](https://huchegames.feishu.cn/wiki/LX6Hwg0JhioJyzkUxmIcPlqxnvb)中，曾老师花了 31.3 元巨款测试了 Qwen3-Coder-Plus 模型，效果挺好的。

今天周末水一篇短文，说说 Claude-Code 如何使用 kimi-k2 模型。

## 退出登录，解决 `ERR_BAD_REQUEST` 问题

如[Claude Code 接入 Qwen3-Coder-Plus 之后……](https://huchegames.feishu.cn/wiki/LX6Hwg0JhioJyzkUxmIcPlqxnvb)需要退出 Claude Code 登录，或者在退出登录后碰到了 `ERR_BAD_REQUEST` 问题，可以看周四的文章 参照解决。

![](img/d331e76be8d715572494ca8f14764334.png)

## Claude Code 接入月之暗面API

设置环境变量：

```
export ANTHROPIC_BASE_URL="https://api.moonshot.cn/anthropic/"
export ANTHROPIC_API_KEY="sk-***"
export ANTHROPIC_MODEL=kimi-k2-0711-preview
```

启动 Claude Code 后确认使用新的 Api Key 就可以开始干活儿了：

![](img/dc712ea98290e093122e8edb8b7923ea.png)

对于模型的设置，也可以进入 Claude Code 之后使用 `/model` 命令来设置。

![](img/0a1d13cf6a676b42a3445a0f153bc59f.png)

## 花了多少 token(money)？

使用 `/cost` 命令可以看到当前的花费，kimi-k2 在 fabik 项目中，修了十几个 bug，并完成了所有单元测试。token 消耗大约是周四 Qwen3-Coder 消耗的 9 倍。

> 项目源码地址：https://github.com/zrong/fabik

Claude Code 按照标准给出的费用预估： 122.39 美元，4300万 token。

```
/cost
⎿ Total cost: $122.39 (costs may be inaccurate due to usage of unknown models)
Total duration (API): 58m 59.2s
Total duration (wall): 18h 18m 1.6s
Total code changes: 1105 lines added, 261 lines removed
Usage by model:
kimi-k2-0711-preview: 40.3m input, 88.3k output, 0 cache read, 0 cache write
```

但实际上，月之暗面开发者后台显示的是 人民币 14.02元 。

![](img/eab7bbf0cd594c25c702c9df8a797a57.png)

如果说周四测试 Qwen3-Coder-Plus 的时候，还是 人民币对美刀油电同价 的感受，今天 Kimi-k2 就是 人民币对哈萨克斯坦坚戈 了。

> 2025年7月25日，1人民币 = 64.403834哈萨克斯坦坚戈。

接下来试试腾讯 Codebuddy，免费用 Claude-4-Sonnect。

#胡扯AI #AI #AI游戏研发工作流

---

<https://huchegames.feishu.cn/sync/Hzv6dRSGJsmHgOb4eSHcN1BHntg>

## 胡扯 AI 群，匹配游戏小团队创业，关注 AI 嵌入游戏工作流

[利用 Riffusion AI 生成游戏背景音乐](https://huchegames.feishu.cn/wiki/Ll4mwULJgiLiLzkwsxbcLsaEnzL)

[10 秒生成游戏音乐音效，快手、字节、谷歌的 AI 工具谁更靠谱？（附实测音频）](https://huchegames.feishu.cn/wiki/EIlUwGsAKiTWEQk1GYgcrweBnOh)

[利用 OptimizerAI 生成游戏音效](https://huchegames.feishu.cn/wiki/U20owS6YtiL842kLJRWcEeuIn0b)

都刷到这里了，不来个 「点赞」「分享」「在看」 一键三连吗？
