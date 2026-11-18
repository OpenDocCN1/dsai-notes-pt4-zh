
# Cursor 0.47更新及免费续杯说明，老金手把手教你玩Cursor！

> 来源：[https://my.feishu.cn/docx/ZiZOdOJq7ocXz9xegG2cDT1Jnuc](https://my.feishu.cn/docx/ZiZOdOJq7ocXz9xegG2cDT1Jnuc)

## 引言

Cursor 0.47.1已经开放下载，新版本的能力老金给你们整理好了！

咱来看看这次它是否还一如既往的牛X！

官网没有的话，你可以在这下载。

一行小字…… 文末当然有你关注的是否还能白嫖~！

下载地址：`https://github.com/oslook/cursor-ai-downloads?tab=readme-ov-file`

![](img/7733321cbd498c4b32ac28b30c065256.png)

## 功能更新

0.47版本正式宣告它的 “AI 进化” 新里程碑。

咱们先别着急，我先卖个关子，文末我再跟大伙儿说如何一键整合 MCP、顺带给你们看看我自己的实测案例，保证让你看完就直呼“真香”！

### 一、Cursor 0.47 大更新，惊喜在此！

老金先来汇总一下 Cursor 0.47 带来的“开挂”功能，以及它是如何一步步升级，顺便帮大伙儿对比下老版本 0.46 有哪些 bug 被彻底干掉。

1. 自动选择模型

+ 现在 Cursor 不仅能一键切换，还能按照任务需要自动选择合适的模型。

+ 不过老金喜欢手动指定，因为我常用的是 Claude 3.7 Thinking。据说这个模式修复了之前常见的 error core，让对话更稳定，就算我跟它唠了几十句，也几乎没遇到崩溃。

![](img/aadacb2f964a5db709ae45c21f67958f.png)

1. 全局 MCP 服务器 & 自定义 agent 模式

+ 0.47 引入了可在 `cursor-mcp.json` 或 `.env` 里自定义全局 MCP 服务器的功能。

+ 这意味着你可以像老金一样，在同一个项目下搞定所有 MCP 的配置，还能为每个 MCP 分配不同的权限。你想让它只能读数据库？行！想让它还会改 CSS？那也没问题。

+ “自定义 agent 模式” 也进一步开花，Cursor 让你自由设置工具访问权限、控制对话语言，甚至可以像老金这么皮——让它只用德语回复，也不是做不到。

1. UI 改进 & 更新频率可自定义

+ 0.47 优化了主题样式，还能拖拽 UI，简直像给编辑器换了层高端皮肤。

+ 在设置里把 Update 频率改成 “Early Access” 模式，就能抢先体验更多实验性功能，比如 Beta 功能、快捷键绑定等。

+ 之前在 0.46 版本里常见的 “只能上传一张图片” 的坑，现在也填了，多图不再是问题。

![](img/b6109332dfb431ee768e116aef7b4e8a.png)

1. `.cursorignore` 隐私保护

+ 早先 `.cursorignore` 文件中的内容还是会被偷偷发到 LLM，容易暴露隐私。

+ 但在 0.47 中，这些本地内容已不会再上传到模型端，保护性更佳。

1. 主题更新

新版本会带来多个新的和更新的主题，包括Cursor Dark、Cursor Midnight和Cursor Dark (High contrast)，满足不同用户的视觉偏好

1. 自定义模式

![](img/173290f8722f0ba7edf7438a132439d1.png)

这样你就可以设置自己的专属模式。

![](img/2ece1113023d78736b93447aadee0067.png)

1. 快捷键之类的其他功能

抱歉，还没看到在哪……估计还没更新。

![](img/181950ab1b064dcf15b012fd89ac2737.png)

### 二、Claude 3.7 Thinking：更贵，但更猛

有人问，为什么 Cursor 0.47 默认倾向调用 Claude 3.7 Thinking？

老金告诉你，因为它现在多任务、多轮对话时更稳定，还能深度思考。

以前 0.46 就容易报错，如今问题不大了，只不过一回合要花费两个请求，价格要高些。

好消息是，性能和输出内容质量都明显上升，你省下的时间肯定也抵得上那点额外花费。

![](img/f00549fc29d142d8aefb256b1bba4566.png)

---

### 三、更多功能：自定义指令、`CursorRuls` 模式

首先更多指令，没啥好讲的，看名字就能看得懂：

![](img/b6a10612982df753b961a133b2e78f10.png)

0.47 还强化了 `CursorRuls`，现在有四种模式：

+ `Always`：任何命令都触发它

+ `Manual`：需要明确提及才触发

+ `Agent Requested`：Agent代理请求

+ `Auto Attached`：MCP自动连接

![](img/fc44b79d52bec515aabd4d01c0bbf126.png)

---

### 四、MCP新方式&推仨MCP

之前总有小伙伴问我有什么好的MCP，这次顺带给大家讲一下。

在0.47版本中，MCP也变成了json可配置，方便多了。

![](img/26494e6a8e06cf5fa48cb4aee659e90f.png)

![](img/7b2324c4679807746652a9df7e9041ad.png)

正好也推仨好用的：

之前的请看这 [Win下的Cursor连MCP都是坑？老金手把手教你！](https://tffyvtlai4.feishu.cn/wiki/RUTPwFlrqihT8YkWPXZcZfWPnCe)

+ Neon MCP：数据库管理，有免费十个项目上限，比 Supabase 慷慨些。

安装说明：`https://github.com/neondatabase-labs/mcp-server-neon`

+ Magic MCP：帮忙生成好看的 UI，按钮、菜单全自动搞定，可惜免费版次数限制略严。

安装说明：`https://github.com/21st-dev/magic-mcp`

+ Browser Tools：装个浏览器插件，就能在管理后台做些自动化脚本。

安装说明：`https://github.com/AgentDeskAI/browser-tools-mcp`

---

## 结语

你们最想知道啥老金我知道。

[Cursor 0.45之后没法免费续杯了？老金教你怎么搞！](https://tffyvtlai4.feishu.cn/wiki/KpTawyKWoier5lk95M7ch863n13)过实测0.45的方法依旧可用，请参考

0.47的到来，距离0.46更新也就半个来月，更新速度越来越快了。

真的，AI一天，人间十年。

自从Deepseek爆火后，来找老金咨询和求教学的人变得越来越多了。

说明AI真的是在方方面面快速的渗透到了普通人的生活。

就像手机的诞生干掉了照相机一样，工具的诞生是没错的。

它会让生产力变得更多，也许在我有生之年，能看到世界翻天覆地的变化。

但反过来看，历史淘汰的，都是那些跟不上时代，固守在原地的人。

回想Chatgpt3.5和Midjourney刚诞生时候，抵制AI绘画频频上热搜，甚至好莱坞都爆发过游行来抵触AI编剧。

结果呢？

现在已经听不见声音了，因为，你不会，别人会，活该，你被淘汰！

不要拿自己的懒惰当借口。

AI它会让“懂”的人更快，“不懂”的人，给喂到嘴边了，依旧不会。

底层逻辑从来没变过，你的认知决定了你的高度。

<https://my.feishu.cn/sync/HDlcdCPBksnJpBbRk0EcW364nmc>
