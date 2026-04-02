# Claude 学习手册

> 来源：[https://yunyinghui.feishu.cn/docx/KAh0dJMbRoMsSixKbbvctSTZnTf](https://yunyinghui.feishu.cn/docx/KAh0dJMbRoMsSixKbbvctSTZnTf)

> **适用人群：****互联网关注 AI 对话发展人群**

> 首发时间：2023 年 4 月 20 日

> 更新时间：2024 年 3 月 6 日

> **内容出品人：**

> **使用建议：**如果需要快速定位到精确内容，可以使用快捷键 Ctrl +F/Command +F 的形式，搜索关键字/词，查找你想要的内容，PC 端查看学习效果更好。

> **持续更新 Claude 模型****相关资讯动态，使用****教程，研究报告，实用场景等板块**内容，永久免费在线查看，欢迎收藏转发支持，文末有惊喜，大国出品，必属精品，和 50W+读者共学。

# 更新日志

# 一、Claude 简介

## 企业介绍

因不满老东家成为微软附庸，11 名 OpenAI 前员工怒而出走，其中就包括前研究副总裁 Dario Amodei，**2021 年 1 月创立了 Anthropic **公司，总部位于美国加利福尼亚州旧金山，打造了 ChatGPT 最强竞品：聊天机器人 Claude，最新轮融资对 Anthropic 的估值高达 184 亿美元。

**官方网站：**https://claude.ai/

## 团队介绍

Anthropic 的创始团队成员，大多为 OpenAI 的重要员工或关联成员，这些人曾是 OpenAI 的中坚力量，参与了 OpenAI 的多项研究。Anthropic 有着很高的招聘标准，目前他们只为 2% 的候选人发放了 offer，其中接受率达到 83%。

![](img/c63a730a02d16670905795a1fd811526.png)

![](img/6184305236149f903a4d52d7e12e3eec.png)

左：Dario Amodei（达里奥·阿莫代），Anthropic CEO；

右：Daniela Amodei（丹妮拉·阿莫迪），Anthropic 总裁

## 融资历史

Anthropic 与 OpenAI 有着微妙关系，不仅如此，谷歌在与微软进行智能搜索引擎大战之际，还斥资 3 亿美元投资了 Anthropic。其支持者还包括谷歌、Salesforce 和亚马逊，融资总额约为 73 亿美元。

+   2021 年 5 月：获得 1.24 亿美元融资，Skype 联合创始人 Jaan Tallinn 领投（A 轮）

+   2022 年 4 月：获得 5.8 亿美元融资，FTX 首席执行官 Sam Bankman-Fried 领投（B 轮）

+   2023 年 2 月：获得 Google 投资 3 亿美元，Google 持股 10%

+   2023 年 5 月：获得融资 4.5 亿美元，由 Spark Capital 领（C 轮）

+   2023 年 8 月：获得韩国最大电信公司 SK 投资 1 亿美元

![](img/f74aa3947b0b78069157a9c38f1098c7.png)

Anthropic 的融资历史记录

## 产线上线时间

2023 年 3 月 15 日，Anthropic** 发布了一款类**似 ChatGPT 的产品 Claude。

**2023 年 7 **月，Anthropic 宣布，已发布最新大语言模型 Claude 2。

2023 年 11 月 22 日，An**thropic 发布其**聊天机器人最新版本 Claude 2.1。

2024 年 **3 月 4 日，Claude 宣布其最新力作——Clau**de 3 模型家族正式亮相。这一**系列包括 Claude 3 Haiku、Claude 3 Sonne**t 和 Claude 3 Opus 三款模型，各具特色，旨在为用户提供更智能、更快速、更高效的选择，并称其是迄今为止速度最快、性能最强大的 AI 模型，Claude 3 单次可处理大约 15 万个单词，而 ChatGPT 每次只能处理大约 3000 个单词。

![](img/3792a4ab416e3f3f5ad125cceb2816a5.png)

+   **Claude3 家族官方：**https://www.anthropic.com/news/claude-3-family

+   Claude 3 包括三款模型，分别是 Opus（著作）、Sonnet（十四行诗）和 Haiku（俳句）。

+   **现已上线**：Opus 和 Sonnet 模型，在 claude.ai 及 Claude API 上对全球 159 个国家开放。

+   **即将推出**：Haiku 模型，敬请期待。

![](img/85b21081dd3974f4823d8ce5bf898160.png)

![](img/2f53a5803d63e87f5f376eb257571dbc.png)

[7a674a59dcd2a37e87cd667b572f264b.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Nz2UbvvRAoPu3yx4GYYckFAunec)

## Claude3 突破性能力

+   **速度更快**：Claude 3 系列模型，尤其是 Haiku，展现出令人印象深刻的速度，这对于实时应用至关重要。Claude 3 系列模型能够支持实时聊天、自动完成和数据提取等任务，这些任务需要即时响应。Haiku 是同等智能类别中最快速、最具性价比的模型。它可以在不到 3 秒的时间内阅读包含图表和图形的 arXiv 研究论文 （大约 1 万个标记）。Sonnet 在大多数工作负载中比克劳德 2 和 2.1 快 2 倍，同时还拥有更高的智能水平。

![](img/eed1967854f0dfa14aad1b2f5c09bb88.png)

+   **视觉能力更强**：这些模型能够识别并处理多种视觉格式，如照片、图表和技术图纸。

+   **减少错误拒绝**：Claude 3 模型在理解边缘性提示方面取得显著进步，减少不必要的拒绝回答。

![](img/62e05535b0ad9f6573f9a42af0d3305b.png)

+   **准确率提升**：在处理复杂问题时，这些模型表现出更高的准确性，与前几代模型相比，Claude 3 减少了提供错误信息现象，并且在复杂问题上提供了更高的准确性。对于各行各业的企业来说，模型的输出准确性至关重要。Claude 3 采用了全新的评估方法，能够更准确地回答复杂的事实性问题，并且会更加诚实地承认自己不知道答案，而不是提供错误的信息。。

![](img/1065feb0c66de34fe39119dcde420495.png)

+   **长期记忆**：最初提供 200K token 的上下文窗口，能够有效处理长篇输入，Opus 模型在信息回忆方面表现尤为出色。

![](img/aa45e92233c16644d1218d386e502042.png)

## Claude3 官方测评图

Opus 是我们最智能的模型，在人工智能系统的大多数常见评估基准上都优于同行，包括本科水平专家知识 （MMLU）、研究生水平专家推理 （GPQA）、基础数学 （GSM8K） 等。它在复杂任务上表现出接近人类水平的理解力和流畅性，引领通用智能的前沿。

所有 Claude 3 模型都显示出在分析和预测、细致内容创建、代码生成以及西班牙语、日语和法语等非英语语言对话方面的增强能力。

以下是 Claude 3 模型与我们的同行模型在多个性能基准上的比较：

![](img/6d23adb09eb48edcf3b2e169fd3bf93a.png)

## Claude3 技术文档

![](img/22ba2d03ca1d761b83dda99811f85cc1.png)

[Claude3模型技术论文(中文版）.pdf](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/YUeYbExXeo8RGdxUe9ncVziFnGc)

## Claude3 提示词库

Anthropic 还推出了一个包含多样化创意 prompt 的 prompt 库。这非常适合那些想要深入了解和充分利用 Claude 3 新功能的朋友们。

![](img/e5aaa853b240eb39565f6a6c5550e441.png)

https://docs.anthropic.com/claude/prompt-library

# 二、Claude 注册步骤

> **注意：**该步骤为 Claude2 之前的注册步骤，此处仅供留存，克劳德3直接跳后面教程。

## **1、注册 Slack**

Claude 是无需注册的，但是 Claude 是“嵌套”部署在 Slack 中的（就像使用 Midjourney 必须注册 Discord 一样），因此，我们得先有一个 Slack 账号。

Slack 是一款企业内部通信 Collaboration 工具，它可以用于团队内部的频繁交流与协作。

Slack 网站地址：https://slack.com/intl/zh-cn/

网站注册很简单，可以使用电子邮件注册，更方便地也可直接使用谷歌账号进行登陆。如下图所示：

![](img/e36cd424a933fa67ec731b3fa9f67189.png)

注册完成之后，我们来到这个界面，下一步需要在 Slack 中创建一个工作区：

![](img/9f902b6e9d6335d3d36b6d919dcc8284.png)

点击“创建工作区”，总共有 5 步操作：填写团队名称、填写姓名、邀请同事（可忽略）、团队工作、准备就绪。

![](img/b0686a7fd56ca6184fda14bc3a6fd315.png)

![](img/480376550ddf598b3eba3ef018d68add.png)

![](img/c1ef1fda589e7f2ff89876a0cb510a25.png)

![](img/7536b986cf8971d42754caf1b0d006a4.png)

![](img/6f64a6ec7d4e96dd193ceea501bec3e6.png)

简单 5 步，创建工作区完成！

![](img/630c5473f04ace726483827777c8d2ba.png)

## **2、添加 Claude**

打开 Slack 的应用网站，找到 Claude AI 在 Slack 上的应用位置：

https://www.anthropic.com/index/claude-now-in-slack

点击“添加到 Slack”。

![](img/c5d54b5701fd9f2a484ae765b3537f1a.png)

![](img/bd0c83365dd2c09f20a65361f5c32482.png)

然后，进入 Claude 与 Slack 的关联项选择，点击“允许”。

![](img/56759f5da382fdc95ad12548a68af7a6.png)

如果弹出下面的页面，则表示 Claude 被成功添加到 Slack 中了！

![](img/067c4d0f583f8f5b88868804f46a5665.png)

关联成功之后，我们再回到 Slack 界面，点击“启动 SLACK”。

![](img/bb7b4e7d9522b6845cac75db7af509e2.png)

就能看到 Claude 就会出现在 Apps 一栏中了。

![](img/5e02a73d4e4c3f855b4a8876ffbb572c.png)

## **3、如何使用 Claude**

使用 Claude 与 ChatGPT 类似，点击左侧菜单栏的 Claude，就可以输入文字与 Claude 进行对话了！值得注意的是，第一次对话时，Claude 会询问你是否授权，直接点击“Agree”就可以了！

Claude 的注册网站：https://www.anthropic.com/claude-in-slack

Claude 的登录网站：https://slack.com/get-started#/createnew

登录网站记得加为收藏哦，方便下次使用。

如果想申请 API 进行开发，直接去官网申请

https://www.anthropic.com/product

点击“request access”之后填表就可以。

## 4.Claude3 注册及使用步骤

点击以下链接，选择试用 Claude3：

https://www.anthropic.com/news/claude-3-family

![](img/3df0a5a1029b44a2c0b4e92763346fc7.png)

继续点击试用：

![](img/7c1e357fa564c25b920a2f751e7f9463.png)

点击进去，用谷歌授权登录，来到下面的界面，Claude 要求进行手机号验证，这里选择填入国外号码验证即可。具体支持地区：https://www.anthropic.com/claude-ai-locations

![](img/638660587fc38caa5eb53f915937448b.png)

此处需要一个国外的号码需要接验证码，可以去 SMS 网站（https://sms-activate.org/）上面下单一个临时号码，选择合适的，之后把号码复制过来，发验证码提交就好了。

![](img/ebbb683dd0d74c3211d7b305d47249e7.png)

直接输入号码后再回来看验证码即可来到该页面：

![](img/70c0d424aa82cfc14090e46a88797469.png)

一直往下滑，可以来到对话界面：

![](img/29d2c131ebe5f555fb8ed6cf97387c2d.png)

**点击对话框可以来到对话界面：**

![](img/38fb9b0458115382902cd150c2be8da4.png)

Claude Pro 是 Anthropic 最新发布的付费产品每月 20 美元，主要对标 OpenAI 的 ChatGPT Plus。Claude Pro 的对话文本输入是 Claude 的 5 倍，并在高负载峰值时享受优先使用权，支持论文总结、数据查询、代码生成等功能。

Anthropic 为了保证 Claude Pro 的性能和稳定性，每 8 小时只能发送 20 次，如果用户发送的内容相对较短，大约 200 个英语句子，每个句子约 15-20 个单词，那么每 8 小时可发送 100 次。

## 5.使用 POE

在 POE 上也集成了最新的** **Claude-3-Opus-200k 和 Claude-3-Sonnet，其中 Opus 需要付费使用，Sonnet 可以免费使用。

![](img/82539c6b6c4e4a0e98f5d0242136623f.png)

POE 网址：https://poe.com/

在 POE 上，不仅仅是 Claude，可以同时使用各种知名的国外语言大模型。

而 POE 的注册方法通过 GOOGLE 账户就可以，并且 POE 不容易被封号，给大家一个好的推荐

![](img/12c67a9e3e1faaf3e94b71a4d6a49d8f.png)

# 三、Claude 使用实测

以下为 Claude3 实测过程：

## 测试文档

![](img/726a06c5e684061560f71162d2f4574e.png)

## 测试图片

![](img/d24ba92f8de6be70028666d032892ea9.png)

![](img/4f84c5881a86ba97e304c1ff34c3478a.png)

## 群友测试

![](img/f87613a3e6b051446480dc8e642c5e54.png)

## 常见 Q&A 问题

**官方地址：**https://support.anthropic.com/en/collections/4078531-claude-ai

# 四、产品对比

## 1.Claude VS ChatGPT 对比

在以下任务上，ChatGPT 的表现更好：

![](img/74f22ebb9746a9c2408bfc99ce26a437.png)

在以下任务上，Claude 的表现更好：

![](img/5b2c5f3e0f3fd04a85c9616a44674f19.png)

在以下任务上，两者表现的差不多：

![](img/ff0bf583ac7c48e741741e6260284d56.png)

结合这些对比，可以发现 Claude 完全不弱于 ChatGPT：

**• Claude 优点：**更擅长拒绝有害提示词、更有趣、写作更长更自然、更能遵守指令；

**• Claude 缺点：**对于代码生成和推理包含较多错误；

• Claude 和 ChatGPT 相似点：逻辑问题的计算或推理，两者表现差不多。

你还可以在 https://nat.dev/compare 上对比 Claude 和其他模型的推理速度和生成效果：

![](img/101c2bc4e0a29e49dafa56a76f3726ef.png)

## 2.Claude3 Opus PK GTP4 Turbo

在 YouTube 上博主 Matthew Berman 简单介绍了一下 Claude3 家族，通过一些问题实际对比了 Claude3 和 GPT4 的效果。

Matthew Berman 的评测视频🔗：https://youtu.be/sX8Ri3w2MeM?si=70n41mO0gVMWu0ub

![](img/e8702574f339dcd8233fbd15d4b3ea94.png)

表格总结了一下他测试的问题和两者回答结果的对比，总结以下几点：

1.Claude3 的语义理解和逻辑推理等能力确实能和 GPT4 扳手腕了，甚至在某些方面更胜一筹，比如在贪吃蛇游戏的编程上，Claude3 生成的代码不仅能运行，并且完美达到了可玩的要求，而 gpt4 生成的代码却无法正常运行

2.Claude3 目前并没有文生图能力，也就是说，暂时还不支持像 gpt4 那样的多模态

3.Claude3 和 Gimini 一样，为确保 zz 正确，在问题处理的过程中，道德和安全过滤能力更加强，而 gpt4 在这方面则更加放松，这在一定程度上会影响用户体验

4.相比之下，gpt4 依然还有优势，一方面是因为 Claude 的用户注册审查力度比 gpt 严格，大陆用户非常容易被封号。另一方面 gpt4 turbo（1106-preview）相比 Claude3 OPUs 的成本相对更低，并且拥有多模态能力，性价比更高

# 五、相关报道

## 量子位

全球最强大模型一夜易主，GPT-4 被全面超越

## 新智元

[**全**](https://mp.weixin.qq.com/s/WqQWS-hiQ1i1Ve6IPH3djw)球最强大模型一夜易主，GPT-4 时代终结！Claude 3 提前狙击 GPT-5，3 秒读懂万字论文理解力接近人类

[**G**](https://mp.weixin.qq.com/s/n2vRkQT44N4OwhjvXHvjCQ)PT-4 最大竞争对手 Claude 2 震撼发布！一次 10 万 token 免费用，代码、数学、推理史诗级提升

## 腾讯科技

[**单词处理能力是 ChatGPT 近 50 倍**](https://mp.weixin.qq.com/s/29EEQRxvCCyq_Ch-9UBGYg)！OpenAI“死敌”推出最强聊天机器人

对标 ChatGPT，新聊天机器人 Claude 来了

## CSDN

[**ChatGPT 最强竞争对手 Claude 2 推出**](https://mp.weixin.qq.com/s/kJpv1FsACCzZqkxZMErzNg)公开 Beta 版，我们上手实测了一把！

## 极客公园

融资拿到手软，估值超 40 亿美元，这家公司成 OpenAI 最大对手

---

# 关联阅读

[玩赚 AI 实验室](https://yunyinghui.feishu.cn/wiki/WhelwZnxQi55mpkOzatc3Xran5b?from=from_copylink)-一站式AI学习社区

---

#  学习&合作交流

1.持续关注并更新 AI 的**相关资讯动态，研究报告，赚钱案**例，常见问题，永久免费在线查看，无收费内容，目标是成为最大的免费 AI 学习社群。

2.已建立社群聊聊 **AI** 发展及应用，**没进群的可以扫码加入，群满可加 AI 研究爱好者大国微信 ID：zyg90s（备**注：AI），已经在社群内的无需重复加入，开源知识库及社群学习全部为公开免费分享。

3.**关于文档负责人大国**：高帆文化CEO，曾就职于教培大厂，目前创业，主攻研究AI落地应用、企业流量增长及达人IP孵化。互联网知识科普博主，互联网流量观察家，曾就职于互联网大厂，现研究 AI 实战应用，努力成为最懂 AI 的项目操盘手。

**联合作者德里克文：**知乎 AI 领域大 V，同济大学毕业。

**5000人飞书学习交流群**

![](img/dc3ea8a874a57a25f6c75a95f405c9ad.png)

**飞书学习交流群**

![](img/3f80ee590d29f2ac3afe98442ceed183.png)

好友（ID：zhangyuguo94s）

**扫码加微信入微信群**

![](img/ba057e38c2d6dc03993d2adb301ff18c.png)

微信扫码加好友（微信 ID：derekdevin）

欢迎扫码了解更多 AI 学习案例

**觉得有用欢迎留个赞**