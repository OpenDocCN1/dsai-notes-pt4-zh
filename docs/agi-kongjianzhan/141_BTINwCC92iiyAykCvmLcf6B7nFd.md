# OpenAI开发者大会全记录：GPT-4 Turbo亮相，GPTs + Assistant API = Agent 新篇章！

> 来源：[https://agispace.feishu.cn/docx/CoU5d9C44obBykxOWP2cuxL1nGh](https://agispace.feishu.cn/docx/CoU5d9C44obBykxOWP2cuxL1nGh)

> 作者：   Carl

> 👸号： 卡尔的AI沃茨

> 原文链接：https://mp.weixin.qq.com/s/xrFvntV-Oms-JPoANd5BlA

## **视频纯享版**

👏完整双语+实时讲解可以看👸号置顶直播👏

##

## **图文收藏版**

![](img/e1bec7b5877e9d1fc8db1f91641b1aa5.png)

### **GPT Enterprise**

+   提供企业级安全性和隐私，更高速的 gp4 访问，更长的上下文

+   大约有 200 万开发人员在我们的 API 上构建了各种用例

+   超过 92% 的财富 500 强公司基于openAI服务上构建

+   并且现在在 chat GPT 上拥有大约 1 亿的每周活跃用户

### GPT4 turbor 六大功能

![](img/79551360586bb5b8cbb6e9a680268618.png)

1.  context length：增加为 128k tokens! 大概是300页书的概念8k的16倍，而且对于长文本更加的准确

![](img/68cdd8e82de2708ee965fd6301155c68.png)

1.  More control

![](img/61b6699a6ecefbe71061b26f4100a575.png)

+   满足开发者的需求，使开发者更加容易的获取model的response 和outputs

![](img/70902518837be52e74fc7321eba556ae.png)

+   New feature：JSON mode函数调用更强了，可以一次性调用多个函数，准确性也更高

![](img/8a0ae815fae5b2e09521dcca3e49953e.png)

+   New feature：reproducible outputs

![](img/85dbc1370020ca75cca87b77be2ba9fa.png)

+   可以传种子参数，使得模型输出更具有连贯性的输出， 使得模型的输出更可控，今天在beta版推出

在未来几周，会放出在 api 中查看日志问题的feature

1.  Better Knowledge

![](img/3b73622568c12cd53c1c09d322cdd103.png)

+   推出检索功能，可以检索外部的文档或者数据库，你想放什么都可以

![](img/f0f1247a3a503dda729bc3d8eaeb4c19.png)

+   更新了chatGPT的知识时间范围，GPT4 turbo 知识已经截止到April 2023

![](img/eff8a61b422f5828250ac12a7c6aab88.png)

1.  New Modalities

![](img/ec106495eb01f39d0bfac4edce0755a7.png)

+   DALL E 3，GPT-4 turbo with vision， 新的TTS model 都将进入到API，GPT 4 Turbo 可以通过API 接受图片输入。- 使用 DALLE 3 进行编程以编程方式生成图像，而且openAI安全系统可以帮助开发人员保护他们的应用程序免遭滥用。

+   GPT 4 Turbo 可以通过API 接受图片输入。可以生成标题，分类，分析。

+   Be my eyes 可以帮助盲人和视力障碍人群完成日常任务，例如识别他们面前的产品。

![](img/912ca6233c1d9357655548fcd0e7e1b2.png)

+   新的语音模型有六种不同的预设声音，还解锁了许多用例，例如语言学习和语音辅助。

![](img/07cdea0ce364409c43cc01cca98b27a8.png)

+   发布了开源语音识别模型 Whisper V3 的下一个版本，它将很快推出 API，它具有改进的性能 跨多种语言。

![](img/25c7ea00175fe2eace0d7aab531019ae.png)

1.  Customerzation：

![](img/411162dbfa8b03ab02804cc7359270ff.png)

+   自定义微调扩展到GPT-4的16k 模型的版本。

![](img/6f6695e97dcfc37bf2bf16c48a2b37f1.png)

+   开放了一个叫做Custom models(自定义模型）的功能，让选定的公司能跟 OpenAI 合作，针对他们的特定领域训练定制 GPT-4。支持修改模型训练过程的每个步骤，并且训练好后的模型是公司专属。

![](img/4374502d6eae1702adb8948d09845204.png)

1.  Higher rate Limits：

![](img/50355de70287e5f361f1e22bff2f5089.png)

+   GPT-4 客户每分钟2倍token，以便更容易做更多事情

+   除了这些速率限制之外，还能够直接在您的 API 帐户设置中请求更改进一步的速率限制和配额

+   引入了版权保护版权保护，如果您面临有关版权侵权的法律索赔，我们将介入并为我们的客户辩护并支付所产生的费用，适用于ChatGPT Enterprise 和API。

+   GPT4的API价格更便宜了，GPT-4 Turbo 输入代币比 GPT-4 便宜 3 倍，为 0.01 美元，输出代币便宜 2 倍，为 0.03 美元。顺带手把GPT-3.5 Turbo也给升级了，新版本GPT-3.5 Turbo默认支持16K了，版本的的价格也给砍了。GPT-3.5 Turbo 4K 模型输入token减少 4 倍至 0.003 美元，输出token便宜 2.7 倍，至 0.006 美元。GPT-3.5 Turbo 16K版本输入便宜 3 倍，为 0.001 美元，输出便宜 2 倍，为 0.002 美元。

+   并且OpenAI承诺不会拿API过来的数据进行训练，以保护开发者，避免版权纠纷。

![](img/56f605b69eb6df89aade6a1c3e59bbf9.png)

![](img/eff91ad8828b4d10910556568c9499b7.png)

![](img/629660abc8dd86e011539e135d2aa81c.png)

## GPT-4 tools & GPTs 如期上线

如今 GPT 能够准确识别何时以及使用何种插件。只需向 GPT 提出需求，它将为您完成所有这些任务。这些功能经常定义为Agent。

![](img/f7525e9232de155f0b34cf9576b56bf2.png)

借此 Openai 更新了GPTS，您现可打造几乎能够适用于任何场景的定制版 GPT，配备有指令集、知识库和执行动作。

![](img/4f12cf6f584f27c5f4be0547dfd90685.png)

此外，您还可以将其分享给他人使用。

![](img/d97d1de9bcc8aabf4d666472b8817675.png)

在直播中展示了用于课程规划的 GPT ，它帮助教师为中学生打造更具吸引力的学习体验，还有集成 Canva 的 GPT，能直接生成海报，以及通过 Zapier 集成，实现日历同步和直接发送信息的功能。

![](img/65670d48e2795f03fd10a4b6c13bbe19.png)

![](img/8f51adb3347c0a0384dfa1bd5c7befee.png)

![](img/0855f618411c81f824da2eb8237ebd8b.png)

奥特曼通过简单对话就完成了搭建

![](img/128db6377817fe3cda8acee152eb9981.png)

制作好的 GPTs 可以发布至 GPT Store，用户能够从中获得收益分享。

![](img/ecee996ee43ce49020996040dc38a745.png)

## Agents 的支持同样扩展至了 API 领域！

Assistance API 现支持持久对话线程，解决了对话历史过长的问题，并集成了检索功能、代码解释器和沙盒环境（All-in-One）。

![](img/fddb5bcc242e5798aaee5349e7bc7522.png)

现场演示了如何通过向 Assistance API 添加代码解释器和网络检索功能，使其能够选择现场观众，并为其账户各自添加了 500.00 美元的 API 积分🤯。

![](img/66aa9d850540119e8e8a7d5197d9f7a1.png)

## **个人感悟**

GPT 和 Assistance API 是支撑 Agent 功能发挥到极致的基础设施。

![](img/09cae62245c9cfc52fd85612fdc177e4.png)

这次更新又将迎来一次大洗牌！我们将持续追踪最新的技术更新～

结尾引用一句直播中我很喜欢的话🎉：

> We'll be able to do more, to create more and to have more.

**OpenAI科技春晚**

快来加入我们的直播讨论群吧！openai科技春晚将会是一个围绕openai为核心，通过openai对一些初创企业的支持，对技术的更新以及追赶它的closed LLM和open LLM将会是我们主要更新的内容～