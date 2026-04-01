# Scaling Law 的困局、Self-play RL 的希望，以及200美金一斤的草莓

> 来源：[https://v11enp9ok1h.feishu.cn/docx/QgY7dAPQdolmn7xcRzvcl8JWnoh](https://v11enp9ok1h.feishu.cn/docx/QgY7dAPQdolmn7xcRzvcl8JWnoh)

**就在昨天，发生了三件事：**

+   听完了小珺和广密的 AI 行业 Q3 总结性的播客，讲到了预训练的 Scaling Law 瓶颈，以及 Self-Play RL 的重要性

+   The Information 爆料，OpenAI即将在2周内发布新的草莓🍓模型

+   ChatGPT Pro 订阅计划开始灰度，售价200美金/月，上去用了一下却发现什么都没有

这三件事关联在一起，拼凑出了一些一些即将成为共识的信息

## **Scaling Law 的困局**

整个大语言模型行业已经很久没有大的进步了。

这是所有人共同的感受。

在模型达到达数千亿参数之后，训练成本和难度上升了很多，但似乎上万亿也并不能带来质变的提升。

Ilya 更是直言，大家已经不知道谈论 Scaling Law 的时候在 Scaling 什么了，而他对 Scaling 有了一些新的思路。

广密说到硅谷逐渐形成的共识就是RL强化学习是接下来的突破点。

而预训练方面，80%的公司会放弃预训练。

## Self-play RL 全村的希望

预训练玩不起，强化学习就成了全村的希望。

第一个超出预期的是 Claude Sonnet 3.5，代码能力超越了 GPT-4o，赋能 Cursor ，让 AI Coding 成了最热的话题。在代码能力方面的突破，使用的方法可能就是 RL。

第二个超出预期的是 DeepSeek，起步很晚，但是专注提升模型的推理、代码、数学能力，最近把 Coder 和 Chat 模型合并，代码水平接近 GPT-4o，在国内是断档第一的存在。

这两家公司的共性就是：单点突破。

如果说大语言模型的特点是通用智能的提升， RL 的特点就是单点突破。

**而推理、代码、数学、Agent 就是目前**价值最高的生产力领域，最适合做单点突破。

## **草莓模型，两周驾到**

这部分信息昨天来自 The Information 的报道：

+   OpenAI is planning to release a text-only version of "Strawberry" within the next two weeks, according to two testers involved with the model.据两名测试者透露，OpenAI 计划在未来两周内发布“Strawberry”的纯文本版本。

>

+   Early impressions indicate it’s somewhat underwhelming, primarily using chain-of-thought prompting. Responses take 10-20 seconds, making it slower than expected.初步印象表明，它有些不及预期，主要是使用链式思维提示。响应时间为 10 到 20 秒，比预期的慢。

>

+   While testers found its performance slightly better than GPT-4o, Strawberry struggles with short, simple queries and has issues with memory integration.虽然测试人员发现它的能力略优于 GPT-4o，但 Strawberry 在处理简短 Query 时表现不佳，并且在记忆方面存在问题。

>

+   The model lacks image integration, making it exclusively text-based for now.该模型目前不支持图像集成，因此只能处理文本。

>

+   It is expected that Strawberry will have rate limits and might introduce a higher-priced ti**er for users seeking faste**r response times, diverging from the current pricing structure of ChatGPT.预计 Strawberry 会设定速率限制，并可能为需要更快响应时间的用户推出更高价位的套餐，这与目前的 ChatGPT 定价结构有所不同。

## ChatGPT Pro 上线，200美金/月

前几天有报道 OpenAI 考虑上一个 2000美金/月的订阅，简直太疯狂了。结果今天 OpenAI 终于公布了实际的订阅价格是：200美金/月。。。

不知道是不是因为有2000的铺垫了，感觉200的话，还行？

这个200美金的订阅买完，用了一下发现除了可以不限量使用 4o，并没有什么新的东西。

所以猜测唯一的可能就是即将发布的草莓的价格。

卡兹克对草莓的一句话总结：**基**于新范式Self-play RL所做的，在数学、代码能力上强到爆炸、且具备自主为用户执行浏览器/系统操作级别的新模型。更智能、更慢、更贵。

为什么这么贵？简单说这就是更高智能的代价。：

+   从效用角度来说，草莓模型的特点是：代码、数学、推理能力、Agent 能力超强，这几项能力都是高价值的，但说代码能力，如果能比现在的 Claude 3.5 再明显好一截，200美金一个月也是能接受的。

+   从成本角度来说，草莓模型每次回答，会进行大量的内部「思考」，会长达10-20秒，其算力消耗成本应该在GPT4的10倍以上。

+   从原理上来说，草莓的方法 Self-play RL 本身需要巨大的推理成本进行训练，并且由于非实时性，这代模型的价值可能是合成数据，而

既然定价已出，就等 OpenAI 11月的 dev day 公布模型了，今年 AI 行业的重头戏，会是新的里程碑还是平淡如苹果发布会，让我们拭目以待！