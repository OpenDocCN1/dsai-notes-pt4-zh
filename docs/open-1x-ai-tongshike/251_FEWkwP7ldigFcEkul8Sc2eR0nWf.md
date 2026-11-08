# 组队学习记忆

> 来源：[https://datawhaler.feishu.cn/docx/A2QqdrQlRowJ2HxPhf9cSvsLnAb](https://datawhaler.feishu.cn/docx/A2QqdrQlRowJ2HxPhf9cSvsLnAb)

<https://datawhaler.feishu.cn/sync/FsXmd9Oifs2Jx4bq3CCcPXE8npg>

> 欢迎来到**《从零开始学多智能体应用开发》**课程 共创笔记板块！

> **在这个文档中，我们将**共同整理和总结学习过程中的重点内容、问题和解答。

> 欢迎随时分享你的笔记和心得，我们一起完善这份笔记！

> 通过共同创作，我们将建立起一份全面、实用的实战笔记，

> 帮助到更多的学习者进行相关学习和实践，以及迭代优化开源项目！

> 欢迎大家共同编写此文档，你的任何一小段的尝试和思考都有其参考价值和独特的意义！

> 

> 此文档的内容完全开源，可基于此文档进行二创

>

> **感谢答疑的讲师——** @李子韬、@邝炜瑞、@高大伟、@潘叙辰

> 感谢积极学习答疑的优秀助教/学习者—— @无缺、 、 、 、 、 、

<https://datawhaler.feishu.cn/sync/WfPJdTkREs3I0XbeTSrcOM3CnPh>

**分享 & 讨论 可参考如下话题**：

1.  ⚙ 技术概念解析

1.  🎥 直播笔记

1.  📒 优秀笔记

1.  💓 学习感悟

1.  ❓ 学习过程中的QA

# [组队学习攻略](https://datawhaler.feishu.cn/wiki/YWN9wvCkriEZZFkhFcccwCMMnEd)

<https://datawhaler.feishu.cn/sync/FoMhdsTAzsnHyPbfoMlckpltnmb>

# 贡献者清单

| **角色** | **姓名** |
| --- | --- |
| 课程设计者 | 通义实验室算法团队：李子韬、高大伟、潘叙辰、邝炜瑞 讲师天团 |
| 运营助教 | 杨飞凡、Amy、水琼 |
| 专业助教 | 高立业、刘伟鸿、王泽宇、赵文恺、魏宇昕、许亚军 |
| 优秀学习者 | 王熠明、黄定华、刘威、 |

# 推荐阅读已有教程/经验文档

# AgentScope简介

<https://datawhaler.feishu.cn/sync/Oo4nd6BMNsyxZPbDPb4cY8bSn1d>

# ❓ QA板块

> 此处收集直播以及学习群交流的相关问题的解答，鼓励大家共创QA板块，并基于QA板块输出自己的学习笔记等，Ctrl + F 一下，会发现很多惊喜哟~

## AgentScope框架功能

1.  **Multi-Agent如何实现ag**ent之间的交流？能否通过特征向量交流？

> 在AgentScope中，Message是Agent之间的桥梁，它是一个python字典（dict），包含两个必要字段name和content，以及一个可选字段url用于本地文件（图片、视频或音频）或网络链接。

> 换言之，agent间特征向量你可以放在content或者url中，只要通信的Agent能理解，应该就是可以的。

1.  **Ag****e**ntScope框架支持哪些模型？

+   框架是独立于模型的。

+   只要有一个模型，跑在一些能够接收POST API的一个端口上面，通过POST API发送消息，然后让端口后面的模型来进行处理。

+   现在除了OpenAI之外，也会兼容像通义和阿里的这边的DashScope。

1.  **AgentScope****如何****使用通义千问模型？**如何使用其它国内外的大语言模型的API？

![](img/8ba395152ce5fcd8495986780b357191.png)

> **AgentScope**支持 **openai** 和 post resquest api 两种模式，只要写好对应的配置文件，就可以加载使用，详见 README，支持的 API 列表在文档中也有提到。

> 更多使用详情，可见由  写作的经验文章：[《AgentScope初探》](https://datawhaler.feishu.cn/wiki/KoF9wN0ksiUw8okHeAWczrWFnH5#YHv5dAO9qonR0TxuGhdc98gFnqb)

1.  目前源码编译版本已经支持千问的API版本

![](img/190d13c759e10030b6789cdc9e383f6b.png)

![](img/a4052d68d4f2654e50777abf5c967f78.png)

> 可以直接使用千问API版本，但要使用源码编译，pip下载的发布版本还没有这个功能。

> 更多详见：https://modelscope.github.io/agentscope/zh_CN/tutorial_zh/102-installation.html

```
**#**** model_name可以****根据https:/****/dashscope.console.a****liyun.com/billing选择免费的模****型
# api_key需要****申请

agent****scope****.**init(
    model_configs=[
        {
            &quot;model_type&quot;: &quot;tongyi_chat&quot;,
            &quot;config_name&quot;: &quot;qwen&quot;,
            &quot;model_name&quot;: &quot;qwen-72b-chat&quot;, 
            &quot;api_key&quot;: &quot;xxx&quot;,
            &quot;generate_args&quot;: {
                &quot;temperature&quot;: 0.5,
            }
        }
    ]
)
```

1.  **另外其他大模型API均可使用pos**t request API的模式进行调用

1.  **AgentScope目前支持哪些工具？和**单Agent框架支持的工具有什么区别呢？

> Agent Scope目前支持的工具有：数据库查询处理、网络搜索、一般性质的文件操作、retrieval和summarizing，详见 https://modelscope.github.io/agentscope/zh_CN/agentscope.service.html，或直接在源码的src\services目录查看最新支持情况。

>

+   function调用还是在规划当中。通过大模型来实现暂时还没有特别好的支持。但是已经在短期规划之中。

+   短期有一个规划，就是会把一个叫做react的一个Mutli-Agent的一个框架来接入进来。react：让每个agent可以先根据他的observation（观察或输入）来进行一个思考。然后根据他的思考，再产生一个输出，然后再根据他的输出去观察到新的一个环境的变化，然后再进行思考输出。

>

> 大部分单Agent框架(ModelScope-Agent)支持的工具都已支持，工具数量会持续增加，用户也可以自己开发service function，嵌入到Agent Scope中使用。

![](img/ab70aacd5d1548e15effaf2bde5069c2.png)

![](img/248dda8ef9705c566e4f31fe10aeb340.png)

![](img/a8090e092d08fab9bd5cb0d60497d01d.png)

1.  **使**用模型API有每分钟token数限制（TPM），如何修改配置使得agent在运行时调用api token速率控制在我想要的范围内？

> 可以修改constant.py的_DEFAULT_RETRY_INTERVAL参数，这个参数是用于控制openai限速报错的重试间隔秒数(interval * failed_time)的。

1.  **mu****l****ti-agent提示词****应该如何去设计呢****？**

> 不同的模型其实有不同的提示词的策略。

> 自己在实验的时候也可以看到，

+   并不是所有的模型其实都是针对muti-agent这样一个场景设计的，

+   尤其是有一些模型可能还需要我们自己去组织prompt的形式，

+   有些模型可能只支持user跟agent的对话，并不支持让agent或者是让user 去扮演不同的一个角色，多角色进行对话。
在这种情况下，我们要对模型的 prompt做一些特殊的处理。

>

> 在agentscope里有两个方面的支持：

+   一方面我们尝试引进一个关于prompt engineering的功能，帮助用户把不同的message、字符串，还有system prompt等信息，组合在一起。
但坦白来说， 因为模型非常多，模型适配的prompt的格式非常多，要求也不完全一样，所以这方面的话我们更多是把自由度留给agentscope的用户。
也就是说用户可以完全自己设计、掌控Prompt处理这一块儿。

+   同时我们也支持开发者引入第三方的库，比如langchain以及别的一些工具的库来完成自己的prompt engineering。

1.  **在msghub中，其实是按照我们要求的顺序让那些agent进****行发****言，后****续****有些什**么样的方式能够让他们更主动、自由地发言?

> agentscope现**在呈现在**example这个目录下的非常简单的本地化的 conversation只是开源的案例的一部分，开发团队 也将在后续加入其他的例子:

+   **比如说现在已经在在PR** 中审阅的**一个例子，可以理解成**agent是主动发言的，或者说是带有一定的"@"功能，
就像我们在🛰️的聊天群组或者钉钉的聊天群组里面，
我们往往每个人的回话都是自己去決定我要不要在这个时刻进行回话，
以及还可以"@"某些人，让他们来回答我的这个回话。

>

> **开发团队**也在尝试，利用agentscope分布式的功能去主动实现Agent的发言决策。

> 其实最自然的形式还是每一个agent都作为一个独立的个体去思考。

> 此时此刻基于现在的这样一个环境情况，应不应该发言，应该进行怎样的一个对话。

> **开发团队**正在做这样的一个example，也希望很快就把它能够上线到github 上。

## AgentScope框架对比

1.  Multi-Agent与Agent相比的具体优势是什么？

1.  **Mutli-Agent和transforme**r的agent在实现上的主要区别是什么？

> Mutli-Agent更多讲究是agent之间的互动。像 AgentScope 框架，更多的会关注agent之间的互动编排，以及比如说非常天然的适合分布式的场景。这是我们框架一个比较大的亮点。

1.  多领域agents很像混合专家，有什么不同吗？

+   混合专家模型的话，像传统的机器学习里面有多个模型来进行一个综合预测。

+   Agent层面来说的话，在增强模型可靠性有一点混合专家的意思。但是它可以是一个多轮的，不停的多个Agent之间互相去辩论，更加拟人。

+   比混合专家模型是更高一层次的一个互动。

+   着眼点也是不太一样。

1.  Agent Scope和Long Graph有什么区别？

+   Long Graph是基于long ten的可视化编排界面，类似于可视化编程。

+   Agent Scope更侧重于Martin agent交流的角度。

1.  **AgentScope****框架****有哪些****优势****？**

+   框架易用性主要体现在引入一些语法糖。会帮助开发者能以更简洁的代码更快速地去编排整个Multi-Agent的应用。

+   框架的编程风格是procedure oriented（程序导向）的编排风格。可以在主逻辑里面看出不同agent之间的执行逻辑。应用的可读性和代码的维护性上会比较有优势。

+   有一些原生的关于多模态和鲁棒性方面的内容。

+   一个大亮点就是分布式的支持。使得开发者可以非常简单的从一个中心化的应用，拓展到一个分布式的应用，来进行不同场景的一个优化。

## 容错处理

1.  **容错****机制设计方面的一些****经验****分享**

+   关于容错，还有⼀些尝试并不适合融⼊到整个框架中。

+   ⽐如说我们尝试过⼀个简单例⼦，在agent scope的agent的那个reply⽅式⾥，在⼀个agent在它产⽣最后的返回信息之前进⾏逐步的检查。

这个逐步的检查其实包含了很多层⾯。

+   第⼀层检查是最基础的不应该包含⼀些攻击性的话语。

+   第⼆层的话就是⼀些语义的必须符合你规定返回的东⻄：符合现在的⼀个对话历史以及上下⽂。

+   第三层是去检查⼀些关键字，你的⼀些特殊要求：

+   ⽐如说要求返回的是⼀个Json的格式或者是某种格式。

+   或是⼀些策略情形的。⽐如狼⼈杀的样例，在⾥⾯试过，返回每个信息前检查⼀次这个agent到底有没有暴露⾃⼰的身份信息。

+   但是后来我们发现虽然可以通过加这些东⻄避免⼀些错误，但是整个agent的调⽤成本上升了，还有就是它的返回速度会⾮常慢。因为原本可能在⼀个简单的对话agent⾥⾯只需要调⽤你⼀次模型。但是这样加入大模型进行容错检查的话，模型的调⽤成本会升的⾮常⾼。

1.  **容错处理的能力边界在哪里？**

> 容错处理的能力依赖于底层使用的模型。

> 对于模型能力较强的模型，很多问题可以在中间过程中解决。

> 开发过程中遇到的问题包括使用大模型判断语义时结果随机性，以及并发处理多个错误时的效率问题。

>

> 能解决与不能解决，很⼤程度上依赖于底层的模型。在于使⽤使⽤⼤模型去判断语义的时候，结果可能会随机⼀点，多个处理时，往往还是会漏到若⼲个。一部分原因可能由于上下⽂太⻓了。

1.  **容错机制的耗时情况一般如何？**

> 基于规则的容错执行速度快，但基于大模型的容错耗时较长。

> 大模型在纠错时耗时增加，token消耗也增加。

> 期待小模型能完成单一的纠错任务，但目前尚未有专门为此设计的大模型。

## 应用设计

1.  **什么才算是多模态应用？**

> Agent Scope提供了一些多模态的样例，如通过文字生成图片，通过图片和文字配合生成视频。

> [谜撰](https://modelscope.cn/studios/AgentScope/find_the_taste/summary)中，有在游戏中输出图片，是多模态应用的一个案例，

> 通过上传图片和语音使图片动起来的应用

> 创空间⾥⾯其实提供了⼀些多模态的样例，在已build好的这个案例⾥⾯，其实是包含了⽐如说通过⽂字⽣成图⽚，以及通过⽂字再去⽣成，通过图⽚和⽂字配⼀起配合去产⽣对应的视频这样的⼀个多模态的功能。比如：谜传的游戏，它其实就可以输出到图⽚，其实应该也算是多模态的⼀种展现形式。

1.  目前multi agent的应用是在哪里？是用在游戏里吗？

> 复杂工程、社会模拟等，更多的等你探索！

1.  **可以制作🐧机器人吗？**

+   用我们这个框架的话，可以通过三十行代码，就可以非常简单的去搭一个比较简单的聊天机器人。

+   但如果牵扯到对个人信息做一个归纳总结，安排行程等，要等到RAG功能上线之后，才会更简单的去支持这样的应用。

## 参赛相关

1.  参赛一个人可以加入多个组吗？可以提交多个agent参赛吗？

> 可以，赛事较为开放，只要参赛者有时间精力即可

> 大家可以多多报名，报名链接：

1.  **参赛有免费的token吗？**

<https://datawhaler.feishu.cn/sync/KBlMdwfKEszVjnb🐧YycsinunVe>

## 开发环境

1.  魔搭的notebook被拉黑怎么解决？

![](img/025696d86bf75761eceb4836f3b4921c.png)

![](img/9ccaceb61a526c7965680f6ea2e35047.png)

> 去魔塔钉钉售后群，@技术人员 你的用户名 解除就可以了

1.  **笔记本3070可以跑吗？**

> 大模型推理直接使用大模型厂商的 API 即可，不要求去本地跑任何的大模型，我们可以通过POST API的端口去调用。比如说像DashScope，它的大模型其实是跑在阿里云的服务器上面的。

>

> 如果想自己本地跑开源模型的话，可以自己尝试去检索哪些可以跑在3070上。

1.  [**Mac 卸载python**](https://www.sysgeek.cn/macos-uninstall-python/)

## 大模型基座相关

1.  阿里云的灵积平台和百炼平台是什么关系？

> 两款不同的阿里云产品，各有侧重，灵积更侧重模型，百炼更侧重工具链

1.  **通义模型的中文能力如何？英文会更**好用吗？现在最强的文本的开源模型是什么？

> 通义千问中文英文的能力都还不错。特别推荐最近DashScope的一个免费开放的通义max接口。

>

> 大家可以自己多去使用，或者看目前比较主流的榜单或者博主的测评；

> 只有亲身体验才更有发言权，了解各种大模型的优势和是否合适你的使用场景

> 不仅限于开源模型的话，目前claude3-opus的最新智力测试达到了101分，属于中等智商水平，其余大模型分数大多在其之下；

## AgentScope未来开发计划

1.  讲的功能在代码里都实现了，还是未来的规划？

+   刚刚在代码里面讲的功能是都已经实现的。

+   最近会支持除了通过POST API端口，直接在本地去调用一些本地跑的大模型。只要大模型有一个本地的接口就可以。

+   近期内会去推出像是最近比较火的一个RAG（Retrieve Augmented Generation）的一些功能，去更多的丰富我们这个框架，也争取给开发者带来更多的便利。

1.  Agent Scope后续的更新有什么规划？

+   Agent Scope最近的更新速度较快，例如快速支持了千问的API。

+   Agent Scope支持long chain的编排，允许用户引入第三方库。

+   Agent Scope在GitHub的PR中提供了与long chain结合的样例。

## 其他

1.  不会代码可以开发agent吗？有低代码开发途径吗？

> [AgentScope框架有剧本](https://datawhaler.feishu.cn/wiki/N2IowK6YkigGoJkydS8cNWBZnGc#Xg36d1RIqolMkdx9iWGcmJjrnAf)工具的开发者版本，可以 🎮 零代码制作自己的游戏应用

> 另外可以参考[《动手学Agent应用开发》学习手册](https://datawhaler.feishu.cn/wiki/GhOLwvAPkiSWmokjUgqc1eGonDf)，单Agent开发框架AgentFabric可无[代码构建Ag](https://datawhaler.feishu.cn/wiki/GhOLwvAPkiSWmokjUgqc1eGonDf#CO9odIm9VoNKfCxQsvScOTOvnEZ)ent，具体操作可见其中的 核心必知必会

>

+   零代码的话，通过游戏框架去提供比较好玩的剧情，去设计一些这个角色，然后生剧剧情线索和任务，也可以生成一个好玩的小游戏。比如说前段时间比较流行的哄哄女友，包括是糟糕被美女包围。

+   有代码基础的话，可以基于这个agent框架去魔改代码，然后去做其他的，比如说工具的一些应用。

# 待整理

1.  这个分布式设计和ray有什么区别？

1.  python适合做分布式嘛

1.  那在创空间上能用到分布式能力吗

1.  placeholder那个部分属于异步编程嘛？--

1.  消息会持久化存储嘛，agent挂了会有重新拉起的机制嘛

1.  请问mutiaent代码及函数和说明书哪里有

1.  那在创空间上能用到分布式能力吗

1.  怎么证明只是跑了一个核

【容错机制与构建多模态应用】

1.  内置工具调用会和单agent一样多吗?

1.  容错处理机制的能力和边界在哪里？

1.  多模态能力在前端如何展示呢

1.  关于容错的更多经验分享

1.  agentscope支持langchain的chain吗？支持编排吗？

1.  现在agent的编排逻辑都需要代码的定义，agent scope有自动编排的功能吗？类似autogt的[Shocked]

1.  Agent 如何支持多租户的应用？

1.  AgentScope和langgraph有什么区别？

1.  多模态输入可以训练本地大模型吗

1.  agentscope要求模型支持function calling吗？

1.  **多模态的能⼒在前端如何展示？它可能有哪些类型，**你有制作过⼀些多模态的应⽤的⼀些经验吗？

+

1.  ⽬前这种容错机制⼤致会跟没有容错机制⽐起来，它的耗时差距⼤概是多少？

+   因为⾸先第⼀点是基于规则的。⼀般情况下基于规则的这样的⼀个容错其实并不是耗时很⻓的。代码执⾏层⾯的速度还⽐较快。

+   主要的耗费还是在于基于⼤模型的容错上。耗时会增加，消耗的tokens数也是成倍的在增⻓。尤其是如果是基于⼤模型再去纠错。如果有⼩的模型能够完成这种单⼀的纠错任务更好，但目前还没有合适的模块。

1.  agent scope⽀持langchain⽀持编排吗？

+   是⽀持的，是这样agent scope跟其他的库结合其实是可以分多种⽅式的。agent scope⾥⾯的agent，它内部的实现逻辑可以完全⽤别的库的。⽐如说reply function⾥⾯，关于记忆管理，关于prompt的处理，关于最后message的⽣成。其实整个过程不⼀定完全要⽤agent scope⾥⾯的东⻄。允许⽤户去引进第三⽅库。正在构建⼀个langchain跟agentscope结合的⼀个样例。

1.  现在agent的编排逻辑都需要代码的定义。有⾃动编排的功能吗？

+   是的。比如⻆⾊的⾃动编排。有⼀个agent builder的例⼦。在这个例⼦⾥⾯，其实每⼀个agent扮演的⻆⾊以及他的职责，其实都是由另⼀个agent来指定的。⽐如说我们叫中⼼化的agent叫host agent。其他agent的⻆⾊，还有他们的⽬标，其实都由其他host agent来决定。

+   第⼆种就是刚才说的编排的这个问题。其实我理解这⾥的编排⼀个是执⾏计划的plan，另⼀个就是交流的顺序。关于交流的顺序，其实我们现在正在审核⼀个PR，这个PR⾥⾯包含了⼀个样例。这个样例就是说我们在⼀个多⼈对话的情况下，每⼀个agent都是由⾃⼰来决定要不要进⾏发⾔。这⾥的agent就是具备两个功能。⼀个功能是它会⾃⼰去检查⾃⼰当前时刻是否应该进⾏发⾔。第⼆个在🛰️或者在钉钉⾥⾯聊天的at的功能。也就是说谁艾特另⼀个⼈，可能另⼀个⼈才会出来回话，就同时⽀持了这两个feature。

1.  **⽬前agent cope框架要求使⽤**的模型⽀持function call吗？

+   是这样，⽐如说像openAI、google提供的原⽣API⾥⾯就本身有tools接⼝。我们只要把符合它规定格式的⼀个⽅式，输⼊的时候还是⼀个Json schema格式的⼀个函数说明。主要把这个函数说明放到tools这样的关键字下⾯。对OpenAI这样的⼀个接⼝来说，他们那边的⼤模型就可以调⽤这个函数。对，这个是⾸先是原⽣的API的⼀个⽀持⻆度。

+   agent scope，提供了switch factory这样的⼀个功能，⾃动的把函数转换成json schema的格式。

+   对于⾃⼰的⼤模型，可能并没有原⽣的这样的接⼝⽀持。就需要prompt engineering。⽐如说我们给他⼀个⼯具的描述，然后让他再约定⼀下。⽐如说你如果要调⽤⼯具的话，应该怎么样调⽤，返回的格式应该是什么样⼦？对这部分⼯作可能就需要开发者⾃⼰去负责这部分对。

1.  agent scope和longgraph有什么区别？

+   Longgraph是⼀个可视化编排的界⾯。

+   agent scope这边更侧重于multi-agent交流。

1.  这个分布式设计和ray有什么区别？

+   ray是机器学习中一个常用的分布式框架。主要是面向模型训练，或者是涉及数据处理相关的工作。并没有先天向这个场景提供支持。

+   agent scope主要是面向的agents级别的交互和编排。提供一种尽可能简单的方式，把分布式做的跟单机模式一样。虽然号称是分布式，但是编码上实际上是完全可以用单机的思路来写。

1.  在设计上会有一些什么样的差异呢？

+   分布式正常情况下，首先是需要考虑通信，任务的调度，以及中间可能涉及到竞争，冲突这些问题。这些问题agent scope都尽可能封装掉了。尽可能把分布式这部分对用户来透明化。让用户不需要去了解分布式的细节，也可以做到分布式。

+   当然分布式的优点其尽可能提升速度，比如说单机跑，python是问题比较严重的。Python因为有一个叫全局锁的东西，严格来说只能利用单核的资源，导致只能跑满一个CPU。

+   我们这个分布式点严格来说叫多进程，能尽可能利用更多的计算资源，甚至单机分布式的情况，单机的核就全跑满了。觉得单机的核还不够用，可以多机继续跑。

1.  要学习分布式的开发的话，究竟有多麻烦。

+   多进程应该是计算机专业的一个是所谓的并发编程，这是一门基础课。它有各种冲突，什么顺序？两个操作稍微倒一下顺序就会出现错误。涉及到数据库里面就会有事务，再往上还会有一些分布式的共识协议，比较常见的像zookeeper ECD。又有很深的这些各种协议。

+   但是对于我们agent scope来说的话，尽可能想给提供一套，用户完全不需要了解这些，假设只写过python，正常的有C语言的入门基础就能拿到，就能写agent scope，并且把它扩展到分布式的场景上。

1.  **python适合做分布式吗？**

+   严格来说不适合，Python本身的一个全局锁，限制了非常多的功能。导致单独起的一个python进程，完全不去调用其它库的话，严格来说只能跑满一个CPU。虽然能起很多线程，但实际上它也只能跑在一个CPU上。

+   像python的异步编程，严格来说也是基于多线程的，所以也只能跑满一个核。这也是导致现在的大部分的agent的框架，不管是用单一agent的还是多 agent，他们虽然用了异步，但是他们本身的代码是只能跑在一个CPU上的。比如说现在大部分用户CPU可能都十六核，也只能跑到一个核。

1.  agent scope这个框架的话可以做到分布式么？

+   是的。那个多进程的search。同时开了很多的去爬网页，但是如果相当于起一个进程去跑的话，虽然可以通过异步的方式来避免爬取网页的延迟。但是对网页的清洗，提取桌面信息这部分还是需要用CPU去计算的。所以这个不能加速，还是可以通过多进程合并的情况下拿到。

1.  在创空间上面可以用到分布式的能力吗？

+   可以的，创空间上可以使用单机多多进程的模式，例如例子中search的模式，但辩论会那个模式不可以，因为辩论会需要单独去在不同的机子上起这个server，创空间只有一个端口。但是单机多进程的加速还是可以达到加速效果。

+   因为现在的分布式版本主要提供了对reply和observe这两个关键方法来提供支持。其他方法需要自己编写，后续会支持。

1.  placeholder那个部分属于异步编程吗？

+   对，严格来说placeholder后边那部分的实现，实际上就是异步的一种实现方式。虽然没有用异步编程，但是实际上是用python写了一套类似于异步编程的效果。

1.  异步效果是怎么样去定义的？为什么它能够达到这个效果呢？

+   异步编程就是，你提出了一个任务，然后你去做别的事儿了。然后你相当于过一段时间来看这个任务完成了没有，不会阻塞掉了。而placeholder其中其实就是提供了这么一层抽象。

1.  消息会持久化存储吗？Agent挂了之后会有重新拉起的机制吗？

+   这部分目前还没有实现。目前主要的场景还是单机多进程这种，快速起一堆agent的同时去处理一批任务这种场景。后续会引入数据库的持久化。

1.  请问multi agent代码及函数和说明书哪里有？

+   可以看一下仓库的example，example部分的distributed文件夹下提供了两个例子，

+   一个是分布式的对话，一对一的对话。user和assistant可以运行在两台不同的机器上。

+   第二个例子其实就是分布式的辩论会的例子。这个辩论会有三个参与方，然后正方和反方都可以选择，是以用户本身去参与，或者是让一个大模型来参与。

1.  **python只能跑在只能用一个核。这个的话有什**么实验是可以去可视化的看到这种效果的吗？

+   python的GIL。是python广受诟病的一个功能，但是它也带来了很多好处，之所以python现在能这么简单，写起来这么简单，也是因为它这是个双刃剑。当然python也可以通过调一些库的方式。比如说python的本身是只能跑满一个核的，可以调一个C++库，把整个CPU的资源全部吃完，那我也可以做到很好的速度。这个其实是现在的各种训练框架，虽然它顶层是python，但底层掉用的肯定也不是python。

1.  **agent scope这个框架，跟其**他的多智能体框架比起来有什么样的优势呢？

+   易用性。

+   从使用方式上，是一个类似python的面向过程的编程方式。

+   不同模块组合的灵活性上。市面上别的一种框架，可能要写很多代码，在这边拿出我们的一个example，稍微魔改一下，或者是简单的换一下模型，或者是换一些prompt，它也可以运行起来，然后达到目标。

1.  **开源框架debug的经验。**

+   agent scope是一个处于不断开发过程当中的这么一个框架。

+   推荐大家游戏赛道不要使用开发版来进行。这一个比较确定的一个逻辑。开发版可能有一些导致一些接口上的不兼容。推荐使用预发版本，不会再改变的版本。

+   创空间上的一些谜钻这种游戏就是一个比较固定的，大家可能只会改一些类似confifg的操作。这样大家还是使用我们的发版，就是一个比较固定比较稳定的一个版本。

1.  **配置生成较慢**

+   有可能是因为用的人太多了，导致那个线路有些拥堵。可能定时去重重启这个应用，然后可能对就是晚上可能用的人少一些。或者本地运行。

1.  agent scope如何实现数据的持久化？

+   只要把agent的 memory设置成一个数据库，就比如说是一个SQL lite或者是其他数据库。memory是不断的往里做添加的。

+   in memory，是很快会有的一个新功能。就是一个teacher的agent，也就是一个不断学习的agent。就是相当于你每次启动这个agent，它都会去不断的去去记住你在和他的交流过程中的一些事情。然后在这个passage结束后，他会自动的去对整个过程中学到的一些知识，或者是你教给他的一些有用的东西，做一次summary，然后再存成一个文件。如果下次再去开启这个python进程来启用它，他就会从这个文件中读取这个memory，进他的那个内存当中，然后再次调用。

1.  如何实现这个日程安排助手或者是爬虫程序。

+   Agent可能就是大家更想要和现实之间联系起来。那么一个程序是如何和现实联系起来的？就像大家可能家中也有一些智能小家电，都是通过一些API。只要把现实生活当中的这些API接口，放在那个service tool里，agent就知道如何去调用它的。

1.  前端形态是只能用Gradio吗？

+   创空间也支持streamlit和 HTML。

+   建议游戏赛道复用gradio，只要写一个配置，就能够轻代码量的去实现这个功能，可以用一套已经预制好的gradio，这个是创空间支持的。也可以去用HTML或者是streamlit来做。

+   工具赛道比较自由。我们也提供了这个gradio界面，可以发挥自己的想象力去搭一些别的前端，

1.  会期待什么样的游戏来产出出来呢？

+   更期待的是一种悬疑类，就能够吊足胃口，然后最后中间很多的转折来发生的。

1.  agentscope这边有商用计划吗？

+   暂时定位为一个开源框架。如果想商用的话，比如游戏赛道来说，用Apache协议，只要遵守了这个协议，就不用为付费。

1.  通义千问的智能体有没有用上agent scope呢

+   通义千问的智能体更专注于单个agent的使用，

+   agent scope更偏向于一个多智能体协作，是从他们怎么样更好的交互，更方便的交互以及分布式的一些运行这方面去考虑的。

1.  能不能把通义千问的智能体用在agent scope框架当中？

+   可以的。agent scope中的agent实际上是调用了一个reply function来进行接收消息以及回复消息的。可以在 reply 函数下去自定义一些函数。把model scope的agent融进来，让它来调用单个agent干的事。然后通过agent协作，或者说通过pipe line去达到一个能够做出三个臭皮匠顶一个诸葛亮的一个效果。

1.  **ModelScope agent那边所支持的工具之后**，我们这边也会支持到吗，比如代码解释器？

+   是的，比如代码解释器agents scope是有的。可以用service的那个factory来调用execution python函数，来做python代码解释器。

1.  **agent scope后续有没有说兼容一些langch**ain，或者是别的工具这么一个计划对吧？

+   会的，包括GPT4，谷歌的gemini, lang chain等的兼容，会根据大家反馈不断迭代。

1.  本地大模型能否支持agent scope的使用？

+   可以把本地大模型设置成一个flask服务，或者是通过fast chat这些框架来，只要把它搭建成一个HTTP的服务，然后就能够通过agent scope来做调用的。

1.  **人类和agent可以协同吗？**

+   可以的，比如狼人杀。且库里有人类agent的代理模块，叫user agent。用户也可以参与到整个协同过程当中，和其他的智能体一起来完成一些事情。

1.  **目前的大模型它的推理能力好像也没有我们想象中的那么**强，去做agent它有些什么样的局限性？

+   agent scope框架它能够兼容各种不同的模型，它其实是有一些局限性的。就比如说咱们写的狼人杀的例子，如果你可用一个lamma7B你可能是运行不了的对。

1.  用一些推理能力不强的模型，如何来支撑写一个应用程序？

+   未来的一个发展计划是增加一个SFT微调的过程。把使用agent scope当中的一些历史记录，或者是一些agent的一些运行习惯。比如说一些bad case保存下来，然后做一个模型的微调。能够让本来不能运行的模型，能够让它运行起来，就做一个比较微量的微调。

1.  老师对工具赛道有什么期待或者建议吗？

+   多模态的利用，就像OpenAI最近的sara，它能够生成一段很好的视频，整个画面也很协调能不能够利用model scope，就是咱们魔搭这个社区当中的一些模型，能够去达到和他类似的能力？能不能通过魔塔开源社区中比较弱的模型，去搭建出一个很强大的这么一个多模态的一个能力。建议希望大家去多多使用魔塔社区，以及多在github或者是开发者群里交流讨论。

1.  **如何调用工具？比如模拟审计团队成员Agent1、Agent2、**Agent3，最终输出表格形式的成果报告

1.  agent的数据集是怎么构建的？

1.  **有哪些容错方案的经验？**

1.  那之前老师说让agent在🛰️群里自动判断如何发言是怎么实现的呢？

1.  **调用工具是怎么做到的？比如调用Exce**l，将成果几个Agent的成果以表格呈现

1.  请问一下实例中，提示词比较少，问什么他们已经开始自我介绍了？

1.  agentscope和类似metagpt等框架的关系是啥

1.  **请问llm agent和rl agen**t的比较优势在哪里，适用场景有哪些不同呢

1.  开源模型部署成api形式就可以使用吗？数据集需要怎么构建呀？

1.  **模**型调用使用post_api如果返回格式需要自己处理，是否需要新建一个类，配置一个自己的model_type?

1.  仅靠角色扮演，真的就可以定义不同的agent吗？

1.  **想**问一下调用工具使用的是toolformer的方式还是function call的方式？哪个会更方便一些

1.  Multi agent 实际业务上已经有什么应用案例了嘛？

1.  实现Multi-Agent具体的难点是什么？

1.  **multi-agent中多agent在开发过程**中如何调试，优化，将内部处理流程可视化?

1.  AgentScope框架的具体优势？

1.  **开源框架debug的经验**

1.  agentscope如何实现数据持久化？

1.  如果要实现日程安排助手/爬虫程序，数据应该如何存储？

1.  **agent具身智能，比如打开一个**开关或者执行一个动作，有没有已有的案例？

1.  有一些细节没看到，前端形态是只能用gradio吗

1.  通义千问的智能体有没有用上agentscope？

1.  可以和modelscope的单智能体框架结合吗

1.  目前大模型支持多智能体群协同吗? 人类和agent协同?

1.  **Agent 关键是planning.需要强大的**推理能力，目前大模型的局限性是怎么样的？

1.  老师对工具赛道有什么期待或者建议?

# 使用案例

## 调用阿里云的API使用AgentScope框架

## 在Mac上使用虚拟环境，源安装最新agentscope

> 排坑记录人：

![](img/d6a02f00dca75a1bae103ae22c9f5741.png)

> 这是由于Mac已经使用 Homebrew 进行了安装包管理，无法使用pip；

> 但brew似乎无法使用类似pip install -e .、pip install -e .[distribute]的命令，

> 因此需要使用虚拟环境

```
**#****如果在克隆的agentscope****目录下，可以在其外面创建一个虚****拟**空间目录，Datawhale_env可以随便写
python3 -m venv ../Datawhale_env
source ../Datawhale_env/bin/activate
#如果不想使用虚拟环境，还可以取消brew管理权限
brew unlink python
brew unlink python3
#即可直接使用pip进行安装

#然后就可以愉快地进行pip了

git clone https://github.com/modelscope/agentscope.git
cd agentscope

pip install --upgrade pip

pip install -e .

pip install -e &#x27;.[distribute]&#x27;
```

![](img/8685bb0f2b13890a0e32b5c3b14b6bf6.png)

## 在Mac上本地运行谜撰

> 找到一个命令行，敲击以下代码

```
#创建并进入虚拟环境
python3 -m venv Datawhale_env
source Datawhale_env/bin/activate

#克隆谜撰代码并进入该目录
git clone https://www.modelscope.cn/studios/AgentScope/game_qwen.git
cd game_qwen

#配置所需环境：安装相关包
pip install -r requirements.txt
pip install google protobuf PyYAML gradio colorist pypinyin inquirer oss2 modelscope_studio

#设置环境变量
export GAME_NAME=test
export DASHSCOPE_API_KEY=sk-xxxxxx

#运行谜撰应用
python app.py


=======使用魔搭Notebook需修改game.py里的demo.launch======
    port = 7860
    name = os.environ[&#x27;JUPYTER_NAME&#x27;]
    root_path =f&#x27;/{name}/proxy/{port}&#x27;
    demo.launch(root_path=root_path, server_port=port, max_threads=400)
```

## 在本地跑Agentscope的 werewolf 案例

1.  安装 **agentscope** 开发版

```
**#**通过源方式安装agentscope
git clone https://github.com/modelscope/agentscope.git
cd agentscope
pip install -e .
pip install -e &#x27;.[distribute]&#x27;
```

1.  修改 **werewolf** 案例中的配置信息

> ./example/werewolf/configs 文件夹下的内容

```
#agent_configs.json
#将所有&quot;model_config_name&quot;: &quot;gpt-4&quot; 修改成 &quot;model_config_name&quot;: &quot;qwen&quot;,


#model_configs.json 改写为
[
    {
        &quot;model_type&quot;: &quot;openai&quot;,
        &quot;config_name&quot;: &quot;gpt-4&quot;,
        &quot;model_name&quot;: &quot;gpt-4&quot;,
        &quot;api_key&quot;: &quot;xxx&quot;,
        &quot;organization&quot;: &quot;xxx&quot;,
        &quot;generate_args&quot;: {
            &quot;temperature&quot;: 0.5
        }
    },
    #添加这段内容
    {
        &quot;model_type&quot;: &quot;tongyi_chat&quot;,
        &quot;config_name&quot;: &quot;qwen&quot;,
        &quot;model_name&quot;: &quot;qwen-72b-chat&quot;, 
        &quot;api_key&quot;: &quot;sk-7ee446af66b44f3bbd4d5cd3a0ecfc35&quot;,
        &quot;generate_args&quot;: {
            &quot;temperature&quot;: 0.5
        }
    },
    {
        &quot;model_type&quot;: &quot;post_api_chat&quot;,
        &quot;config_name&quot;: &quot;my_post_api&quot;,
        &quot;api_url&quot;: &quot;https://xxx&quot;,
        &quot;headers&quot;: {},
        &quot;json_args&quot;: {}
    }
]
```

1.  注意：可能存在一些报错是由于库import失败，需要单独下载那些库

+   报错信息

ImportError: Could not find module spec for werewolf at /Users/mac/Documents/Working/1_Datawhale/1_学习内容设计/4学习手册写作/魔搭MultiAgent/Learn/agentscope/examples/werewolf/werewolf

+   解决方案：

```
pip install gradio modelscope_studio
```

1.  运行 **werewolf** 案例

执行命令 as_studio werewolf.py (可以看见gradio前端) 或 python werewolf.py（只能在命令行查看）

执行效果大致为：

![](img/521a6736fea7be42b53ed2efe1d1333e.png)

+   注意事项：大模型回复可能出现错误的情况，容错机制会使其重试最多三次

![](img/ac069c568c04a25c24434da11f4a4d84.png)

1.  修改为中文版本，可查看[AgentScope 初探](https://datawhaler.feishu.cn/wiki/KoF9wN0ksiUw8okHeAWczrWFnH5#M9xudjmDDoYdtbxTnOscbPoGnjd)中的压缩包

# 📚 推荐资料

# 🎗 继续往前冲吧！~

### 欢迎随时共建~或是基于本文档的内容二创文章，更加欢迎投稿~

>

> 在Datawhale开源学习社区，我们都是学习者，我们一起成长！

>

> 虽然只是一次短期学习的结束，但更是我们更多结伴学习、成长旅程的开端！

>

> Datawhale 陪你一路前行，在AI学习的路上，我们并不孤单！相互鼓励，走好每一步！

>

> 冲鸭！心怀梦想的学习者们！！！成为战士，勇敢无畏地披荆斩棘、乘风破浪！战胜一个个学习的难关！

>

> 就继续勇敢往前冲吧！！！学习群随时还可以进行沟通交流~ 我们这次连接并不是终点~

#