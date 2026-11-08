# 九天Hector：2024大模型技术年度总结

> 来源：[https://kq4b3vgg5b.feishu.cn/docx/SipiddJPhoBB3lxGZaXcHM8Gnmf](https://kq4b3vgg5b.feishu.cn/docx/SipiddJPhoBB3lxGZaXcHM8Gnmf)

---

2024年可以说是大模型应用爆发元年，都发生了什么呢？我们并不会单纯总结那个模型好或者那个模型不好，从大模型技术发展角度来说，哪怕是同一个模型，一年内也会发生多次迭代，单纯的进行性能对比可能意义不大。

相比之下，对技术人来说有两个主题是必须要关注的。

+   **其一是“别人家是怎么**用大模型的”，也就是想要知道大模型有哪些应用场景，大家都是如何实现的；

+   其二则是未来哪些技术和哪些场景会更有价值，方便规划自己的学习、求职方向，甚至是公司未来技术迭代方向。

这也是次总结要给大家解答的疑问

+   参考资料：

> IBM 2025 大模型技术展望：https://www.youtube.com/watch?v=5zuF4Ys1eAw

> 2025德勤预测：Agentic AI自主生成式AI智能体即将爆发：https://mp.weixin.qq.com/s/IVX17vsOf2T84m7XAVGArg

> 微软研报：2025 AI 六大发展趋势：https://mp.weixin.qq.com/s/RMsGhTh78gw_7Q_wcSV0Jg

> LangChain 2024年人工智能发展报告：https://mp.weixin.qq.com/s/YbWkFutPdMYw7CU1lhrBmw

> Amy Webb重磅发布，2025年十大科技趋势！：https://mp.weixin.qq.com/s/nG7KEokPwbBFaInslvRpaA

> Anthropic最新：AI Agents 2024年度总结！https://mp.weixin.qq.com/s/onLByvapFe-rzaeryf5vUw

[完整版视频解读，点我跳转](https://www.bilibili.com/video/BV1Xn6nYnEnr/?)

---

#### Part 1. 大模型应用爆发元年

20**24是**大模型技术狂飙**式发展的一年，我们无**意去“神话”大模型技术，这项技术的发展历程本身就是一个奇迹。从早期的不被看好，到公认的通往AGI的最佳技术道路，只用了不到2年的时间。如果说23年是大模型技术爆发元年，那24年，毫无疑问，则是大模型应用爆发元年。

这一年，我们见证了大模型Agent智能体的大爆发，从AI科研助手到AI办公提效工具，从AI创意创作到AI自动编程，一年时间不到，AI工具已经渗透到我们生活的方方面面。

这一年，我们见证了多模态大模型的异军突起，图、音、视频、甚至是手机端大模型纷纷亮相，具身智能指日可待。

甚至在2024年，我们还破天荒的见证了诺贝尔物理学奖和化学奖颁发给了AI技术大佬，为表彰AI技术对相应科学领域做出的突出共享。由此也足见AI技术势不可挡的技术影响力。

> 谷歌多模态大模型开参考资料：https://deepmind.google/technologies/gemini/

而就在刚刚结束的蚂蚁集团20周年庆上，马云更是感慨：“AI 时代的机遇，比互联网要大 100 倍，未来的20年，AI所带来的巨大的变革，会超出所有人的想象。”

![](img/55a046d491c906e901b08d8bd752f543.png)

而伴随着国内外各大科技巨头纷纷重磅押注AI，大模型也即将进入技术发展周期的“黄金十年”，

作为国内最早一批大模型知识服务创业团队，24年我们的大模型课程已有超过2000名小伙伴报名学习，也有超过5000名小伙伴加入到我们的大模型技术社区，我们的交互式AI编程Agent也在Github上斩获超过1100个star，此外，24年我们还帮助了30多家企业完成定制化智能体开发、也进行了数十场企业内训。

> 交互式AI编程Agent：MateGen GitHub地址—>https://github.com/fufankeji/MateGen

![](img/598cb6ef83e026bb76e22875a88d8f59.png)

接下来，我将以一个大模型技术创业者的角度，带大家回顾2024年大模型技术发展的大事件，并结合IBM、微软、德勤、LangChain和Anthropic等公司发布的《2025大模型技术展望》报告，为大家梳理2025年大模型技术发展的底层逻辑。观前提醒，本期内容非常丰富，大家记得先点赞收藏一波！好了，话不多说，就让我们正式开始吧！

> 发展报告包括：

> IBM 2025 大模型技术展望：https://www.youtube.com/watch?v=5zuF4Ys1eAw

> 2025德勤预测：Agentic AI自主生成式AI智能体即将爆发：https://mp.weixin.qq.com/s/IVX17vsOf2T84m7XAVGArg

> 微软研报：2025 AI 六大发展趋势：https://mp.weixin.qq.com/s/RMsGhTh78gw_7Q_wcSV0Jg

> LangChain 2024年人工智能发展报告：https://mp.weixin.qq.com/s/YbWkFutPdMYw7CU1lhrBmw

> Amy Webb重磅发布，2025年十大科技趋势！：https://mp.weixin.qq.com/s/nG7KEokPwbBFaInslvRpaA

> Anthropic最新：AI Agents 2024年度总结！https://mp.weixin.qq.com/s/onLByvapFe-rzaeryf5vUw

#### Part 2.文生视频-Sora诞生

2024年2月，当我们还沉浸在春节的喜悦中时，OpenAI再次发布核弹级大模型应用：Sora。此前我们从未想过，大模型居然还能自动生成视频！如果说大模型生成文字，是基于对人类语言表达习惯的理解，那么大模型自动创建视频，则代表着大模型或许具备“理解世界物理规律”的可能性。

> Sora：https://openai.com/sora/

**一石激起**千层浪，此后的一年内，各大厂商纷纷下场，争抢文生视频赛道的市场红利。并且短短10个月内，就基本确定了这条赛道的竞争格局。国内可灵、海螺、即梦三款应用横空出世，并完成市场垄断，而国外，则由Voe、Flux、Pika、Sora等产品坐镇。

> Flux：https://flux1.ai/create

> Pika：https://pika.art/

> Voe：https://deepmind.google/technologies/veo/veo-2/

此外，由Sora引发的、关于“大模型能否理解真实世界的物理规律”这一议题，也是成为24年大模型学术界争论的热点。持反方观点的如图灵奖得主杨立昆(Yann LeCun)，坚决认为大模型是无法理解真实世界的物理规律的，而持正方观点的如被誉为AI教母的李飞飞，则躬身入局、发起了空间智能创业项目，认定多模态大模型具备物理空间理解能力，并且未来的潜力无限。

大模型领域就是这样，看各路大佬相互争辩，既感到有趣，同时也非常荣幸，荣幸生长在大模型技术快速发展和激烈碰撞的时代。

> 杨立昆百度百科：https://baike.baidu.com/item/%E6%9D%A8%E7%AB%8B%E6%98%86/51137221?fr=ge_ala

> 李飞飞百度百科：https://baike.baidu.com/item/%E6%9D%8E%E9%A3%9E%E9%A3%9E/7448630?fr=ge_ala

#### Part 3.基座模型性能大爆发

好，看完了24年的开年大戏，我们**再来看24年**另一个竞争异常激烈的战场——基座模型训练。

所谓基座模型，指的是用于去构建AI应用的底层大模型，我们可以把这些基座模型比作是土壤，而我们现在看到的AI应用实际上是生长在这片土壤上的作物，基座模型性能越强，土壤就越肥沃，就会催生更多的具有生产力价值的AI应用。因此，谁拥有更强的基座模型，谁就能笼络更多的开发者，谁拥有更大的市场话语权。从这个角度来说，基座模型的开发就是各路AI大厂的必争之地。

但是基座模型训练却是一件非常困难的事情，不仅技术难度高，而且训练过程消耗的算力成本极为昂贵，截止目前，要训练一个能正常说话的大模型，目前公认的算力成本价是3000万人民币起步，而要如果训练一个能达到ChatGPT性能的大模型，成本则最低需要10个亿，这还只是算力成本。因此，现在放眼全球，能够在基座大模型领域“卷”起来的，也就只有中美两国。

> 大模型竞技场排名表，蓝色为美国大模型，红色为中国大模型

> 大模型竞技场地址：https://huggingface.co/spaces/lmarena-ai/chatbot-arena-leaderboard

![](img/ad1fcb726e1b78a14f42e38451c3c5d6.png)

**在过去的2024**年，根据权威的大模型竞技场统计，全球范围内总共**发布**了196款基**座模型，**相比上一年度，最强基座模型性能提升约30%。而从技术人角度来看，24年基座模型领域有3个最值得关注的技术突破，分别是：

1.  原生多模态大模型成为主流模型；

1.  基于强化学习的大模型训练道路探索成功，可用于训练得到高智力的大模型，如OpenAI o系列模型；

1.  基于合成数据的大模型训练实验成功，未来大模型训练成本有望下降80%；

**这些听起来很专业，接下来**我将用尽量简洁的语言跟大家介绍这些突破将带来的哪些影响。

首先，2024年5月13号，OpenAI召开春季发布会，介绍了全新版ChatGPT：新版ChatGPT不再是简单的文字聊天机器人，而是能听会看、同时还能实时语音聊天的AI应用。用更简洁的语言来描述就是，ChatGPT俨然进化成了一个“人”，能看你所看、也能和你顺畅聊天。

![](img/473f9d9c669972367672d585eae941d5.png)

> OpenAI GPT4o介绍页面：https://openai.com/index/hello-gpt-4o/

是不是很酷，而这背后其实就归功于GPT4o这个基座模型，该模型也是GPT4模型的多模态版大模型，支持文字、音频和视频输入，以及文字和音频的输出。所谓多模态模型，指的是不仅有支持文字输入和输出，还支持音频、视频等形式的输入和输出。新版ChatGPT在24年12月底已正式上线，现在你的ChatGPT将正式升级为你的私人贾维斯。

> OpenAI 12场次发布会说明：https://openai.com/12-days/

> 本部分内容可以参考OpenAI发布会Day 6，ChatGPT现场教学煮咖啡的演示：

![](img/439407e4c3e731f55682d2b857ce1720.png)

除了gpt4o外，谷歌的也发布了原生多模态大模型gemini 2.0，并且谷歌CEO放出狠话，25年将和OpenAI在这一领域展开生死之战。

> 谷歌多模态大模型开参考资料：https://deepmind.google/technologies/gemini/

> 谷歌誓夺回第一：https://mp.weixin.qq.com/s/mq82CnxZ_Ple_PKjS5WrWg

**24年，在多模态大模型还**有**两个值得关注的重大**突破，那就是Computer Use和Phone Use，它能让大模型直接接管你的电脑或手机，你只需要发送一条指令，大模型就能自动帮你操作电脑或者手机来完成相应的工作，小到点个外卖，大到自动整理表格发送邮件，大模型都能帮你完成。哇，这简直是一键开启打工人的AI分身！而要做到这点，需要大模型了解电脑或者手机的操作逻辑，而且还要能时时捕捉屏幕信息，很明显，这就需要多模态大模型的支持。这两个应用中，Computer Use由Anthropic Claude与24年10月23号发布，而Phone Use则是由国内智谱AI重磅发布。

> Anthropic Computer Use：https://docs.anthropic.com/en/docs/build-with-claude/computer-use

> 智谱AI Phone Use：https://mp.weixin.qq.com/s/wDZkuPsjer3pN82f7-V1Hg

好了，聊完多模态大模型，我们再看今年大模型技术圈的年度热词“强化学习”。作为行业带头人，OpenIA在年末压轴发布了o3大模型。该模型被认为是GPT系列模型的接班人，o3模型性能强劲，在数学、编程、科研方面性能是GPT-4o模型的5-10倍，超越了95%的人类专家，能轻松在奥林匹克数学竞赛中夺金，网传还能淘汰99.9%的程序员，可以说是一只脚已经踏进了AGI的大门。而o3模型背后的功臣，就是强化学习，这种基于强化学习的大模型训练方法，可以使得大模型的智力水平呈现出跨维度增长。而一个高智力的大模型，不仅能解决数学、编程和科研的问题，同时还能极大程度提高智能体的智能程度，可以说是价值无限。

![](img/c2c25b7ab9f377e3126b5108a25ba5f6.png)

> o3模型说明：https://openai.com/index/deliberative-alignment/

相信在2025年，我们必然将要见证一波类似o3模型的基座模型大爆发，而强化学习也即将成为各位大模型技术人的必修课。

除了强化学习外，24年由微软发布的合成数据大模型训练法，也堪称梦幻，怎么说呢，就是微软按照合成数据训练法，训练了一个名为Phi-4的模型，一个140亿参数的模型，性能上就能超越其他4000亿参数的顶尖开源大模型。

![](img/a74095c837839fab5c8b20e5329f0c70.png)

> 微软Phi-4模型介绍：https://techcommunity.microsoft.com/blog/aiplatformblog/introducing-phi-4-microsoft%E2%80%99s-newest-small-language-model-specializing-in-comple/4357090

我当时首次听到这个消息是完全不相信的，140亿参数的模型怎么可能打得过4000亿参数的模型呢，直到微软开源了这个模型权重，我实际运行了一下才发现，效果真的非常恐怖，甚至很多人在实际评测后觉得这个模型是迄今为止最接近GPT-4的开源模型，而且140亿参数并不大，最低16G显存即可本地运行。要知道，此前号称和GPT4性能相当的开源大模型，还是Llama-3-400B大模型，本地运行至少需要700G显存。

![](img/3505081afc38b12bc64555a2b45a436d.png)

![](img/19559aaa55f1ca1f3f951cedbca6a76d.png)

![](img/00f31b99e6df61b31ba54f9864ee5135.png)

![](img/343d0b06eec0ed462d38050af1f0e260.png)

![](img/5004c957d8c2899da545afdd0515d95d.png)

而Phi-4模型之所以能够以140亿参数打败4000亿参数，核心就在于其训练数据中包含290B的合成数据、占总训练数据的40%，这些高质量的合成数据并不是互联网上的公开数据，而是由类似OpenAI o系列模型创建的高质量问答数据。

![](img/ddf3cce69975389d6515b215491a3ce7.png)

可以说Phi-4模型的训练成功，在24年的年末又再次引发大模型训练领域的巨震，一方面，一旦合成数据被验证是可以用于大模型训练的，那业内关于大模型训练是否存在数据墙的观点不攻自破，大模型训练哪怕是耗尽了全网的数据，还有取之不尽用之不竭的合成数据可以使用；

![](img/710b841d362da358de9d86529a1d365b.png)

而另一方面，优质大模型可生产优质合成数据，而优质的合成数据可以训练得到更优质的大模型，进而产生更优质的合成数据。如此往复，大模型训练领域的飞轮效应就此产生，而这必然在2025年引爆新一轮的基座模型能力增长。

#### Part 4.大模型技术年度关键词-Agent

好了**，聊完了2**024年基座模型的重大突破，大家是否已经感受到了大模型技术汹涌澎湃的发展势头了呢？不过相比基座模型，大模型在应用层的突破更加令人瞩目。如果要评选一个大模型2024年度技术热词的话，那必然就是Agent，也被称作智能体。所谓Agent智能体，指的是基于大模型开发的AI智能应用，可以帮我们全自动的完成一些工作，比如一些自动做会议总结的应用、自动编写PPT的应用，都是智能体。

不知道大家有没有这样的感觉，2024年一年，忽然之间这些Agent智能体应用就进入到了我们生活的方方面面。其实早在23年底，我就大胆预测24年将是Agent爆发的一年，并很早就给我们的学员朋友提供了Agent开发教程。

![](img/282b8cd871b787a9c3ffa7ee7bfa7c4d.png)

![](img/820080f46d98bcfdb0bbc602a3e054ab.png)

![](img/b9c9fac21db2e4a9bca1cbf0b7ed512e.png)

而24年也确实迎来了Agent智能体的大爆发，大模型像水电一样被引入到了各行各业的各个应用场景，掀起了新一轮的效率革命。根据LangChain发布的《2024 AI Agents》，2024年美国100人以上、2000人以下的中型公司，约63%的公司已经采用智能体进行工作。

![](img/69fe27ce60b1605f8e9b84f7ca81ffcf.png)

> LangChain报告：https://blog.langchain.dev/langchain-state-of-ai-2024/

而在具体应用场景方面，2024年，智能体在在学习、科研、办公、创意创作、编程、运营和数据分析这七大领域集中式爆发，彻底颠覆了传统的工作流程。

![](img/49d038f390a74e1c51632e69aa98e37b.png)

![](img/ac35b506a2b540a677d89e5a36cba8c8.png)

![](img/ad6ffc2862524d5de015198c3b3e0baf.png)

![](img/30aadeae4d0d998962223008a5701bc7.png)

![](img/ffbbd27fc457dc455cb48da8c5c0196e.png)

![](img/303bbbdfb5f74734b0df099db4cef1b9.png)

![](img/4bd00adb637004171c1a80ec04d44599.png)

![](img/18637cb736d140a2e5e1f16be807b15f.png)

![](img/3e61c23ba4872f5d4c83a297dd56d65a.png)

![](img/e12076c8af77403c398a7653d80ad58d.png)

![](img/d1d5eb17d3275485a8f02cc455d0876b.png)

![](img/746a1504f1201acf5e1f49af97c1e9b9.png)

![](img/7dfb43e06b521db5fa6d1ab05e8f0a03.png)

![](img/8bd1ae589ff3cd5484f2eb35b229ce40.png)

![](img/3cfeab9b58bb36dd110d04fdb4fa69d7.png)

![](img/cad55f9bf06964adeb4645abb939020c.png)

比如学习方面，谷歌的问答式学习神器NoteBookLM横空出世，能帮你快速整合互联网上文本和视频信息，也允许上传本地文件来构建知识库，然后NoteBookLM通过引导提问的方式引发你的思考，并通过一次次问答来帮助你完成学习，并且NoteBookLM还能自动汇总和整理你的问答内容，帮你生成学习笔记和测试问卷等。

![](img/ff9fe54f2c601b83b75200bdb5d7da97.png)

![](img/88515a4b4c9cd851d4a5c4119da2d9d0.png)

该产品一经推出便火遍全网，是极为罕见的几乎没有人差评的AI应用，被誉为谷歌最成功的AIGC产品。上个月我也测试使用了一下，简直是一眼入坑！问答式学习流程比填鸭式学习效率高太多了，NoteBookLM现在也是我在学习时候的必不可少的AI工具。

![](img/e0370271e0156fb5c7fa9da92be45fe0.png)

![](img/c15b9b1610df75291f776c10bb4c34f3.png)

![](img/85f98b1cca171a15578eb8d86dd38cbc.png)

![](img/404201ca786916060f5c51726df6c110.png)

而在科研和论文编写方面，ChatGPT的地位无人可以撼动。最新版ChatGPT集成了博士级推理模型o1，科研效率Max！而且新版ChatGPT还集成了Custom mode、Canvas和Project等功能，在模型指令跟随、长文本书写以及知识库信息管理方面能力，又有巨大提升！

![](img/4938964909ed1d62636b90c6e3f67126.png)

![](img/02f26763b6baefe5b98c184a9ec294c0.png)

而在高效办公方面，无论是自动会议总结、公文编写，还是企业专业知识库查询、简历识别与筛选，都诞生了许多生产力级别的智能体。至于创意创作，现在自动创建PPT、自动网页设计、AI绘画、AI生成视频等技术都趋于成熟，甚至掌握这些AI工具已经成为绝大多数设计岗位的必修技能。

> AI自动设计网页demo，使用Gamma.app完成:

![](img/94cbc14cb8b6832983001bf7ceba85e9.png)

[2024-12-28 18-35-20.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/WGwjbvghgoPttUxwGYMcFTWOnOh)

而在AI自动运营方面，智能体可以作用于拉新、接待和转化的各个环节，自动赚钱链路仿佛就在眼前啊。实际上，24年一年，光是AI📕自动图文编写和爆款仿写的Agent，就有数十种之多。这一刻，借助AI工具进行掘金，是不是就无比的具象化了呢。

> 部分📕Agent官网：

> XHBPlus：https://xhsplus.io/

> 135📕排版：https://www.135editor.com/ai_editor/red-book/home/

> 红薯智语：https://space.chinaz.com

> 多种草AI：https://duozhongcao.com

> 快写红薯通AI：https://www.hongshutong.com/#/aiIndex

此外，AI数据分析也是智能体的热门应用场景，智能体可以实现自动数据提取、数据清洗，自动多维分析和自动报表编写，提数工程师们的岗位岌岌可危。

而AI编程，24年更是卷出了天际。从年初爆火的cursor，到年中的Windsurf，再到年末的Bolt.new，每个AI编程产品的推出，都将自动编程推上新高度，如今一句话自动编写一个应用已成常态。程序员的就业生态面临剧变。

> AI自动编程的demo，使用bolt.new完成：

![](img/c5503122245de20eda19b39c874bbed9.png)

[2024-12-28 18-29-08.mp4](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/all/Vm0NbEwiEoI21KxgyAzcAWf2nTh)

#### Part 5.展望未来

其实无论是哪个领域的智能体，短期内迅速爆发的核心原因只有一个，那就是能大幅提高工作效率。试想一下，传统的一些岗位，比如初级数据分析师，其核心工作就是编写SQL在数据库中提取数据，而如今一些SQL Agent，典型的如vanna.ai，不仅能够自动阅读公司数据字典了解公司业务情况，而且还能全自动编写复杂SQL，在通用任务的准确率上超过了80%，可以说这个准确率已经超过绝大多数的数据分析师了。

> vanna.ai：https://vanna.ai

> vanna.ai研究报告：https://vanna.ai/blog/ai-sql-accuracy.html?utm_source=chatgpt.com

甚至，当很多工作都可以借助智能体完成的时候，AI员工的概念就呼之欲出。尽管以现在智能体的智能程度，肯定还不能完全替代掉某个岗位，但智能体的大爆发确实对很多岗位已经造成了巨大的冲击。早在2023年，OpenAI就和宾夕法尼亚大学共同发表了一篇论文，名叫《大模型对北美劳动力市场影响的早期观察》，该论文表明，美国约80%的岗位会受到大模型技术影响，这些岗位中约10%以上工作将可以由AI直接代替完成，而其中19%的岗位或将直接面临淘汰，这些面临重大冲击的岗位包括程序员、数学家、会计师、翻译员、作家等。

![](img/44c4a7515ebbc944ed44217df9a5ddbc.png)

> 《An Early Look at the Labor Market Impact Potential of Large LanguageModels》论文地址：https://arxiv.org/pdf/2303.10130

而更关键的是，智能体的技术浪潮才刚刚开始，据微软24年研报《Generative AI in Real-World Workplaces》的预测，2025年智能体的能力将进一步增长，智能体将在科学、法律、医学等领域迎来新一轮的爆发；而Claude母公司Anthropic，在24年研报《Building effective agents》中也有类似的观点，认为2025年将是更高效率的智能体系统爆发的一年。

![](img/8f01b199924990cdadb37d14dfe4ea6b.png)

而这一观点，在德勤的研报《Autonomous generative AI agents: Under development》中也得到了验证，德勤认为，更加智能的生成式AI智能体将在2025年大爆发。

![](img/6bb5fae079f225f2662944daba8669df.png)

而IBM则在《2025AI趋势预测中》明确表示，学会和AI Agent协作，将会是未来职场人的必修课。

![](img/fdfc46c0368976ce2daefcbda2df12a7.png)

> 微软研报：https://www.microsoft.com/en-us/research/uploads/prodnew/2024/07/Generative-AI-in-Real-World-Workplaces.pdf

> Anthropic报告：https://github.com/anthropics/anthropic-cookbook/tree/main/patterns/agentshttps://www.anthropic.com/research/building-effective-agents

> 德勤研报：https://www2.deloitte.com/us/en/insights/industry/technology/technology-media-and-telecom-predictions/2025/autonomous-generative-ai-agents-still-under-development.html#promising-use

> IBM 2025年AI趋势分析：https://www.youtube.com/watch?v=5zuF4Ys1eAw

而谷歌的态度也是如此，笃定**未来将会是一个Agent的时代**，将会是Agentic Era。这里有个有趣的小彩蛋，谷歌在2023年年末发布大模型Gemini 1.0的时候，口号还是“Welcome to the Gemini era”，欢迎来到Gemini的世界。而在2024年年末发布Gemini 2.0的时候，口号就变成了“Built for the agentic era”，创建一个Agent时代。由此也能看出，Agent智能体发展趋势不可阻挡。

![](img/a84c6e089c4a566f9979175c418305dd.png)

![](img/f6f8b8e414edf2a52061b61b3216099b.png)

那对于普通职场人来说，身处智能体爆发的时间点，我们应该如何自处呢？目前智能体距离真正大规模代替岗位还有一段距离，对于我们职场人来说，不必过度恐慌，但是，需要抓紧时间用上AI。借用刘润在2024年跨年演讲《进化的力量》中的观点，“10年后替代我们的或许是AI，但明天替代我们的是更会用AI的人”。迅速掌握一些AI工具，不仅有利于帮助现在的工作提高效率，同时也有利于了解AI底层逻辑，为未来做准备。

![](img/6d2ac07c7198f64f537002f375cc1442.png)

而如果是想要从事大模型技术的技术工作者，那么恭喜你，大模型技术方向上，无论是大模型训练、还是大模型智能体开发，目前都处于蓝海，机会属于每一位努力奋进的技术人！

#### Part 6.总结

我知道，面对新兴技术的爆发，我们会听到各类褒贬不一的声音，那么这期视频的最后，跟大家分享一个有趣的小故事，那就是AI将有助于提高我们这代人的平均寿命。

2024年10月9号，诺贝尔化学奖正式公布，本次化学奖获奖者包括美国科学家大卫·贝克（David Baker），以及英国科学家德米斯·哈萨比斯（Demis Hassabis）和约翰·江珀（John M. Jumper），以表彰他们在计算机蛋白质设计和蛋白质结构预测方面的突出贡献。 其中，哈萨比斯和江珀都来自谷歌的Deepmind团队，他们获奖的原因是开发了人工智能模型AlphaFold，并成功解决了蛋白质结构预测这一长期难题，传统上，实验测定蛋白质三维结构可能需要数月甚至数年的时间，而 AlphaFold 能够在数小时内完成这一任务。哈萨比斯在11月接受了《金融时报》的采访，采访中哈萨比斯说到，借助AlphaFold，将显著缩短新药研制周期，在一些特定领域，新蛋白质结构发现时间将缩短90%，也就是说，以往10年才能研制的新药，现在或许只需要1年左右的时间。而伴随着新药研制周期的大幅缩短，人类也将有望在更短的时间内攻克一些疑难杂症，进而提高全人类的平均寿命。我相信在未来，大模型技术肯定会普惠到更多的人。

> 哈萨比斯在时报上的采访：https://www.ft.com/content/72d2c2b1-493b-4520-ae10-41c1a7f3b7e4?utm_source=chatgpt.com

好了，以上就是本期视频的全部内容。

内容来自**B站账号****@****九天****Hector**，点击下方链接即可观看。如果觉得视频有用，记得三连支持哦~