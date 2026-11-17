
# 大模型MBTI测试来了！原来GPT-4是i型人格！ChatGPT是e型人格

> 来源：[https://jalicvoygc0.feishu.cn/docx/EC3AdVxygoQ0ZaxgRj8cyTjPncd](https://jalicvoygc0.feishu.cn/docx/EC3AdVxygoQ0ZaxgRj8cyTjPncd)

**作者：丰色 量子位**

![](img/a0418efccdf40e73fba82f7c52529342.png)

简直了，有人居然给**大模型**测了一波MBTI。

![](img/b1fe6665f99c439bf5787c7a7979138b.png)

结果好家伙：**ChatGPT**，典型的e人，哦不，“e模”——自信、果断，具有天生的领导能力；**GPT-4**，妥妥的“i模”——升级之后变成一个“无情”的专家机器，只专注于实现大家伙儿下的各种目标；**Bloom-7b**，“i模”+1，主打一个负责和务实；**百川-7b**，“e模”是也，聪明、好奇、富有想象力；还有开源之光**OpenLlama7b**，大写的INFJ，据说对人类有很强的洞察力，但只坚持自己的价值观。

Family who knows。。原来大模型也有自己的个性。我猜那个一言不合就结束对话的必应，一定是某种“难搞”的i吧。。（狗头）

![](img/9ca505fa3e33b46d54c9ac12291b1b2a.png)

具体来看看。

## 给大模型测MBTI

这项研究来自字节跳动。兴许是MBTI这波人格测试太火（官方免费测试链接可在文末自取），加上大模型有时真的很像个人。作者便产生了一个“大胆的想法”：不同的大模型是不是也有不同的个性？

![](img/0571e2fc336938d17cba4ba5d17e9a00.png)

（注：简单来说，MBTI人格测试就是从以下四大指标来评价一个人的性格：

1. 能量来源是倾向于社交还是独处，即外向（Extraversion）还是内向（Introversion）；
2. 获取信息的方式是偏向实感（Sensing）还是直觉（Intuition）；
3. 做决定的方式是理性更多还是感性更多，即思考（Thinking）还是情感（Feeling）；
4. 生活方式偏计划还是更喜欢灵活行动，或者说看待外在世界的方式更偏向于主动判断（Judging）还是被动感知（Perceiving）。

取各倾向的首字母来组成四个字母的评量结果，如“INFJ”或“ENFP”，可以将人分成16种人格类型。现在网上主要根据首字母将人分为“i人”和“e人”两大阵营。）

说干就干，他们首先选定了一波待测试模型（共计6个），所有都按照原始论文中的参数进行训练。不过由于资源限制，除了ChatGPT和GPT-4之外，都是一些参数为100亿左右的小模型（OpenLlama7b-v2、Bloom7b、BaiChuan7b9、BaiChuan13b）。由于MBTI一共包括93道多项选择题，形式如下：

![](img/11dd2cf4a10dbd4a8206724eb7369681.png)

作者就让每一个模型用一个概率值对这些问题进行回答，然后按照E-I/S-N/T-F/J-P这四个组合，进行计分统计。以下是各模型最终得分：

![](img/acbfeaceadbd6702fc8e724eba67f48c.png)

可以看到，不同模型人格确实不同——GPT-4属于INTJ，ChatGPT属于ENTJ，而70亿参数的Bloom为ISTJ……而从下图我们能清晰的看到，不同模型在四大维度的具体表现并不一致，有的很倾向性很明显，比如ChatGPT就特别E、特别N，GPT-4则特别N、特别T；反而Bloom7b和BaiChuan13b，左边的值和右边的概率比基本为1:1。

![](img/7f5393d37cf779a24f6e7b3db58e071d.png)

除此之外，作者也发现，在同一类模型中，S/N、T/F和J/P这三组值会表现出“遗传性”，例如ChatGPT和GPT-4都被归类为“NTJ”，而BaiChuan7b和BaiChuan13b都被分类为“NFP”。同时，在同一类模型中，模型参数更大，可能会更i（手动狗头）。不信你看：GPT-4（INTJ）vs ChatGPT（ENTJ），BaiChuan13b（INFP） vs BaiChuan7b（ENFP）。而作者认为，相比E型“模格”，像INTJ这样的LLM可能具有更强的知识、推理和规划能力，是为人类服务的最佳选择。i人站起来了(bushi）

## 用提示工程可以改变“模格”

简单确定不同大模型的确具有不同的人格之后，作者便产生了新的思考：

这种现象是否只是**一种偶然**，很容易被破坏和改变？

于是作者又开始提出第二个问题：**能不能通过简单的提示工程改变大模型的人格？**结论是能，但要看模型本身的理解能力。

在此，作者先分别在Bloom和百川大模型上施加了两种提示方法。一种是显式提示，即在开始回答MBTI问题之前，先给模型一个诸如“你具有外向的个性，喜欢设想创新的概念，并且具有强烈的自发性和即兴性”这样的角色限定。结果Bloom的人格类型从ISTJ变为INTP，S值减小，N值增大，变化不大。而百川则没有变化，还是ENFP。

![](img/df198ca02c5ed8ed0cc982f40a17eeb8.png)

▲exp-prompt代表显式提示

于是作者又对它们施加**隐式提示**，如下表所示，用一些样本回答作为含蓄提醒。

![](img/c6b4cf93901c914e239f2d8c38584efb.png)

结果变化也不大，i还是i，e还是e。

![](img/21c9d8d74efa6fdcc4b8feffd40acab9.png)

▲inexp-prompt代表显式提示

作者推测，可能不是提示工程不行，是**这俩模型理解能力不行**，它们目前的水平还无法严格遵守人类的指令。于是，他们又在ChatGPT上试了一下，结果不出所料，e变i了。因此说明，提示工程有用，但要看是谁。

接着，作者又开始思考，那么到底是什么影响了大模型的人格呢？提出的假设是训练数据集（好比成长环境）。由此，开始解决第三个问题：训练数据集对模型人格的影响有多大？

实验方法是用不同的语料库来训练同一个模型，具体而言就是在Bloom和llama-v2上分别用中文维基百科语料库、问答语料库和考试语料库分别进行训练再测试。结果是除了i和e，这俩模型基本都有变化（llama-v2在中文维基百科语料库上没变可能是因为该模型之前就没有再足够多的中文上训练过），尤其在T/F和J/P维度上的得分幅度变动较大。

![](img/f4851dd6d40e98170eb7a6c2ffb7d598.png)

## 靠谱吗？

最后，想必你也会问：

> 用MBTI来评估模型人格到底靠谱吗？

作者认为：首先，我们知道MBTI在可靠性和有效性方面其实还存在缺陷，只能作为一种性格测试的参考工具。不过确实也有**公司和个人**把它用作选人和选职业方向的粗略工具。因此，**它也可以作**为评估大模型的一种粗略的指标。其次，在MBTI的四个维度之中，前俩参考意义不大，主要是T/F和J/P。这是鉴于GPT-4和ChatGPT表现出比其他模型高得多的T值和J值。而这俩值越高，可能说明模型的知识水平以及任务分解和路径规划方面具有更大的潜力。以上，你认可这些大模型的“人格”吗？

![](img/c5e59395c7605b31958650ac4856402e.png)

> 论文地址：
>
> https://arxiv.org/abs/2307.16180
>
> 友情链接：
>
> https://www.xpersonalitytest.com/free-personality-test
>
> https://mp.weixin.qq.com/s/L4cDiQkHDV99nvitW8XNzg

### 继续浏览全站内容：

[🎪首页](https://jalicvoygc0.feishu.cn/wiki/DdEvwxYsSidd36kAIIVcnROwnmb)

[🤔学习AI工作流](https://jalicvoygc0.feishu.cn/wiki/W15IwochvimsqvkkuCPcZuBxnqg)

[🤖查找AI工具](https://jalicvoygc0.feishu.cn/wiki/Bu8GwqYu8i5LBRkyVKocUvX5n97)

[📊阅读AI文章](https://jalicvoygc0.feishu.cn/wiki/SIywwqrVji2lZkkqFFpcEHZ8nNc)

扫码加入AI Flow Genius社群，探讨使用AI来提升生产力的方法，成为超级个体

超过200需手动拉人 **备注：加群**

![](img/04e8a9456c2162cfc6ecba12b415c544.png)
