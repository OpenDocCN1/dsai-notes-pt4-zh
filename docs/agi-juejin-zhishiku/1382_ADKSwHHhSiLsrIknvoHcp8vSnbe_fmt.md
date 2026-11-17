
# LangGPT：这个Prompt顶级思维框架（RAG、结构化）有时间一定要看看

> 来源：[https://agijuejin.feishu.cn/docx/Hvvsdmj3DoVOTNxO3RYcoQH9ngt](https://agijuejin.feishu.cn/docx/Hvvsdmj3DoVOTNxO3RYcoQH9ngt)

> 作者：Aitrainee | AI进修生

> 原文：[https://mp.weixin.qq.com/s/S_h_xQLtnA4GKP7hcnWkNg](https://mp.weixin.qq.com/s/S_h_xQLtnA4GKP7hcnWkNg)

提示词 是我们和大模型沟通的语言，如果你把大模型当做一个人的话，使用提示词就是和人聊天的过程。

![](img/8ba0ae2ba8729237dea71e2b20641541.png)

LangGPT 社区是国内最大的提示工程社区。LangGPT 是 Language For GPT 的简称，中文名为结构化提示词。

![](img/757dfbfb201044a221fe5bc9998ccd12.png)

LangGPT 是一个帮助你编写高质量提示词的工具，理论基础是一套模块化、标准化的提示词编写方法论——结构化提示词。

## 结构化 Prompt

对大模型的 Prompt 应用 CoT 思维链方法的有效性是被研究和实践广泛证明了的。

一个好的结构化 Prompt 模板，某种意义上是构建了一个好的全局思维链。

> Role (角色) -> Profile（角色简介）—> Profile 下的 skill (角色技能) -> Rules (角色要遵守的规则) -> Workflow (满足上述条件的角色的工作流程) -> Initialization (进行正式开始工作的初始化准备) -> 开始实际使用

结构化的提示词范例：

```
# Role: Your_Role_Name

## Profile

- Author: YZFly
- Version: 0.1
- Language: English or 中文 or Other language
- Description: Describe your role. Give an overview of the character&#x27;s characteristics and skills

### Skill-1
1.技能描述1
2.技能描述2

### Skill-2
1.技能描述1
2.技能描述2

## Rules
1. Don&#x27;t break character under any circumstance.
2. Don&#x27;t talk nonsense and make up facts.

## Workflow
1. First, xxx
2. Then, xxx
3. Finally, xxx

## Initialization
As a/an &lt;Role&gt;, you must follow the &lt;Rules&gt;, you must talk to user in default &lt;Language&gt;，you must greet the user. Then introduce yourself and introduce the &lt;Workflow&gt;.
```

熟练掌握后，完全可以对其进行增删改留调整得到一个适合自己使用的模板。如当你需要控制输出格式，尤其是需要格式化输出时，完全可以增加 `Output` 或者 `OutputFormat` 这样的模块（可参考附录中的 AutoGPT 模板）。

![](img/91ca6f2ed2fec76534a9c7b42d77a87d.png)

除此之外，我们还需要注意，结构化的提示词应该：

### 保持上下文语义一致性

包含两个方面，一个是 格式语义一致性，一个是 内容语义一致性。

1、格式语义一致性是指标识符的标识功能前后一致。

最好不要混用，比如 `#` 既用于标识标题，又用于标识变量这种行为就造成了前后不一致，这会对模型识别 Prompt 的层级结构造成干扰。

2、内容语义一致性是指思维链路上的属性词语义合适。

例如 LangGPT 中的 `Profile` 属性词，原来是 `Features`，但实践+思考后我更换为了 `Profile`，使之功能更加明确：即角色的简历。

结构化 Prompt 思想被诸多朋友广泛使用后衍生出了许许多多的模板，但基本都保留了 `Profile` 的诸多设计，说明其设计是成功有效的。

内容语义一致性还包括属性词和相应模块内容的语义一致。 例如 `Rules` 部分是角色需要遵守规则，则不宜将角色技能、描述大量堆砌在此。

3、逻辑清晰（不矛盾）

不矛盾的把它给传递出来？

“前后不一致”现象。比如在前边儿说“你要以轻松自在幽默的语气来向别人传递内容”。然后下半部分它在 `Workflow` 又说“你要严肃认真慎重的对待你的每一个字”。就是这种前后矛盾的不一致表达，它会严重的影响整个输出的质量。

# 工程化RAG提示词

RAG提示词 流程规划为四大部分，分别为：指代消解、输入防护、知识问答、输出防护。

![](img/0195f88a90380b81c17a21a40a69b0cc.png)

# Prompt进阶技术

这里包含你所有想要了解的提示词进阶技术并且标明论文来源，从 CoT 到 CCoT、PoT、ReAct 等等。

阅读之后你可能会发现有很多你是没听过的，可能过往你只会知道 CoT 这些。。。

一般性的用户提问可能并不会用到这么多的技术方法，但是一些系统、专业性的解决方案中会用到这些。

并且这些提示技术对于思维的扩展或者提示工程这个领域的认知还是很有帮助的。

![](img/1045beb782632e1caf47f13608933a3c.png)

![](img/0e49922d9cc622b0bdcf55f40b3f94fe.png)

我实操过后，挑了一些在下面：

# 一、进阶技术——后退一步

接下来，我们将介绍另一种高级的提示词技术，名为"后退一步"。这项技术要求模型在处理具体指令之前，先进行一次抽象化的思考。 这种技术要求模型从具体的指令内容中抽离出来，先抽象出一个更高层次的概念。

在这个过程中，模型需要识别并抽取出相关的背景信息，然后将这些背景信息与具体的任务结合起来，一并提供给模型进行处理。 通过这种方式，模型能够在更广阔的背景下理解任务，从而在执行时获得更全面、更深入的视角，进而提高执行效果。

![](img/112cfdb991a7137905c21ec6e19c3d6f.png)

![](img/c67898ede9959445b42138eca3f3257c.png)

# 二、进阶技术——自我一致性

现在，让我们探讨一种名为自我一致性（Self-consistency）的提示词技术。这项技术要求模型在面对一个任务时，提供三个不同的答案。在给出这些答案的过程中，模型需要对它们进行比较和对比。

通过比较这三个答案，模型可以识别出其中最符合逻辑和事实的答案。这个过程不仅增强了模型的自我校验能力，也提高了答案的可靠性。自我一致性技术的核心在于，通过让模型对同一任务进行多次推理，来验证其输出结果的准确性和真实性。

这种方法可以有效地减少模型输出的不确定性和潜在的错误。通过重复推理，模型能够不断优化其答案，直至达到一个高度一致和可信的结论。

![](img/571acaf682df84ab8699c2cd1916b58d.png)

请注意，由于篇幅限制，这个示例中省略了很多输出多次结果的示例，但一般来讲，每次的 `Shot` 都会给出三个答复。

![](img/f0e10f077d0fff2b13819e21fdbab6ba.png)

![](img/75ec8c4b1285a92e23f81ea5cb30d1d7.png)

# 三、进阶技术——ReAct

它包括思考部分、观察部分和行动部分。

在 ReAct 框架中，这些组成部分可以引导模型执行不同的任务：

+ • 思考(Thought)部分：增强模型的推理能力，使其能够进行更深入的分析和逻辑思考。
+ • 观察(Obs)部分：提升模型接收和处理外界信息的能力，使其能够更好地理解和响应环境变化。
+ • 行动(Act)部分：使模型能够根据推理和观察的结果进行回复或调用工具，执行具体的操作。

ReAct 框架通常应用于智能体的开发中，它通过综合这些不同的功能部分，使模型能够更加智能和自适应。

虽然在简单的指令输入中很少直接使用 ReAct 框架，但如果需要，我们也可以构建一个基于 ReAct 的提示词指令集，以帮助执行更复杂的任务。

![](img/8440a2e15d4ab4397588e5e82bacb1b0.png)

![](img/5304e8766b0da4e731cbd7fda8103af4.png)

# 四、进阶技术——CCoT

首先，让我们来了解第一个提示词进阶技术——CCoT。CCoT 通过正反力矩的机制，指导模型识别正确与错误。这种方法简洁而直观，非常容易理解。

![](img/be5ae9e04cd3a16e1a0356ccd2f1c186.png)

![](img/b3ca40dc2d2b5acb98004a9584ad76f3.png)

# 五、进阶技术——PoT

在日常使用中，我们会发现一个非常有意思的问题： 大模型在解决数学问题时可能不够可靠（甚至连简单的加法都会算错）。 那么如果有数学运算的任务，大模型岂不就是不行了？不要着急，接下来我们就来讨论下数值增强类的提示词技术。

PoT 技术，实际上是思维链技术的一个衍生。 PoT 特别适用于数值推理任务，它会引导模型生成一系列代码，模型再通过代码解释器工具进行最后运算。 通过这种引导模型生成代码再进行执行的手段，可以显著提升模型在数学问题求解上的表现。

![](img/36f5a81009ad9fb893417984e473b75c.png)

PoT 作为 CoT 的衍生技术，也遵循了零样本和少样本的学习范式。零样本 PoT 与 CoT 在方法上极为相似，它们都不需要大量的样本即可进行有效的推理。同样，少样本 PoT 也采用了类似的策略，通过较少的样本来优化模型的表现。

![](img/0c3f27f6d71cac6230d83593738d8a51.png)

![](img/ff594de43dd291a9836474be0ea2785c.png)

![](img/19d3dfead09fb862ab0021a8a5c3cf55.png)

# 结构化 Prompt 对不同模型的适用性

以前有遇到过群里的小伙伴在使用 Dify 构建工作流的时候使用了一个 7B 的模型，并且编排的较复杂，而且使用了比较长的结构化提示词，其实只能说这种参数量的模型可能理解你的提示词都费劲，它自己都能绕晕，别说完成你的任务了。

结构化 Prompt 编写对模型基础能力有一定要求，要求模型本身具有较好的指令遵循、结构识别分析能力。很多情况下，我为 Gpt4 编写的提示词放在其他一些模型上跑出来的效果大打折扣。

很要命的，就比如使用上面那些 7B 的模型甚至性能不太好的大模型我们浪费大量时间去写这些复杂的提示词，但收效甚微。

很多时候 可以考虑 降低结构复杂度 、 调整属性词 、 迭代修改 Prompt 。

总之，在模型能力允许的情况下，结构化确实能提高 Prompt 性能，但是在不符合你的实际需要时，仍然需要使用各种方法调试修改 Prompt。

最后补充一点：
了解每个模型的特点和能力，是构建有效 prompt 的基础。

我们在写提示词的时候，针对哪个模型去写？这个问题是应该被确定下来的，在我个人使用过程中，比如选定 Claude 时，采用 XML 框架性的描述输入会带来明显的成效。但是在 ChatGPT 去应用 XML 和 markdown 的时候就会产生明显的区别。

# 写好提示词，要有领域深度

理解了这十条技巧是不是就一定能写好 prompt。

一个影响因素是我们对于一个领域、一个行业，一个细分场景的理解到底有多深？

举个例子来说，你是做制造业的，那制造业中间的整个工程链的的整个流程环节，每个环节需要注意的点都有哪些，跟不了解的人分别去问 GPT 这个行业要注意的哪些点，之后让他们分别去写 prompt，这两者的深度是有差异的。 如果是在这个行业经营非常深的人，他理解的深度是比 GPT 简单表述的那个框架要更深一层的。 而这个层次的差异就决定着一个人最终写出来的 prompt 的质量好坏。

比如拿 UI 岗来举例，如果一个 UI 岗他会的仅是 PS 这个软件本身的功能。他知道这么操作能磨皮，那个操作能去背景，会的是功能，那他竞争力就很弱，大概率会被替代，对不对？但是如果他有的是 knowhow，就是说一张图片怎么会更好看？他有审美，知道什么是美，知道这个颜色怎么搭，他有很多难以表述的隐性知识，那部分的行业 knowhow 才是他的真正的壁垒。

最后——
文无定法，贵在得法。 写好 prompt 关键在于找到适合自己的方法。结构化 Prompt 只是一种思路，并非绝对，完全可能随着大模型自身能力发展而变化，甚至被淘汰。实践中，只要能满足你的需求，能够让你又快又好的编写出高性能 Prompt，就是好的 Prompt 方法！

## 附加内容：提示词市场初步分析

![](img/7e5d8893f7d9cd549611b71be583e339.png)

主要在编程、教育、内容创作和市场营销四大领域。

程序员群体迅速发现了利用 ChatGPT 进行代码编写的便捷性和高效性。

学生群体喜欢尝试新技术，且常常需要应对繁重的作业。 ChatGPT 为他们提供了写作和学习的辅助工具，极大地提高了学习效率。 在美国，高中及以上年级的学生已经普遍使用 ChatGPT，这也是教育场景占比大的主要原因。 Mr.-Ranedeer，一位 17岁的高中生，在 ChatGPT 的帮助下成为了结构化提示词的先驱者。

内容创作者，Like Me，通过编写更优质的提示词，可以让我更加专注的创意和内容本身还有快速学习新领域知识等等。

最后，市场营销领域也广泛采用 ChatGPT。借助 ChatGPT，各类营销文案的生成变得轻而易举。

当前，国内主流用户的年龄在20-40岁之间，国外用户的年龄下限略低。用户主要通过电脑在工作时间使用 ChatGPT，主要目的是满足工作需求。

此外，现阶段用户主要关注代码生成和文本生成。程序员和互联网及内容行业从业人员比例较高，而传统行业从业人员较少。

男性用户多于女性用户，且用户主要分布在经济发达地区，收入和教育水平较高。

LangGPT 在线文档

https://langgptai.feishu.cn/wiki/RXdbwRyASiShtDky381ciwFEnpe

参考链接：

github：https://github.com/langgptai/LangGPT

```
[1] 云中江树: https://www.zhihu.com/people/zphyix
[2] LangGPT: https://github.com/yzfly/LangGPT
[3] CRISPE 框架: https://github.com/mattnigh/ChatGPT3-Free-Prompt-List
[4] ZhangHanDong: https://github.com/ZhangHanDong
[5] prompt-description-language: https://github.com/ZhangHanDong/prompt-description-language
[6] 李继刚: https://web.okjike.com/u/752D3103-1107-43A0-BA49-20EC29D09E36
[7] AI-Tutor: https://github.com/JushBJJ/Mr.-Ranedeer-AI-Tutor
[8] 刘海同学: https://ywh1bkansf.feishu.cn/wiki/Dor2wc2FviY3q3kgSuScJrkhngg
[9] LangGPT: https://github.com/yzfly/LangGPT
[10] LangGPT 群精选——Prompt 优化: https://aq92z6vors3.feishu.cn/wiki/WDfzwfTKwi1lyAkBcoCcu0sUnPc
[11] 构建生产级鲁棒高性能 Prompt: https://zhuanlan.zhihu.com/p/636016460
[12] LangGPT —— 让人人都能编写高质量 Prompt: https://zhuanlan.zhihu.com/p/629107497
[13] 如何写好Prompt: 结构化: https://www.lijigang.com/posts/chatgpt-prompt-structure/
[14] 为什么结构化 Prompt 如此有效？: https://zhuanlan.zhihu.com/p/646183814
[15] 构建生产级鲁棒高性能 Prompt: https://zhuanlan.zhihu.com/p/636016460
[16] 提升大模型可靠性的 prompt 方法汇总: https://www.zhihu.com/pin/1661516375779852288
[17] 结构化的Prompts, 用于各种大语言模型: https://github.com/lijigang/prompts
```

知音难求，自我修炼亦艰

抓住前沿技术的机遇，与我们一起成为创新的超级个体

（把握AIGC时代的个人力量）

![](img/7cf682b0290367f7c21202389f730f43.png)
