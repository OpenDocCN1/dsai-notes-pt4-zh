# 脑网络中的因果涌现理论应用：来自信息分解的启示

> 来源：[https://agijuejin.feishu.cn/docx/IJ8sdKsi1o1OKwxgbWNcU3ZjnCh](https://agijuejin.feishu.cn/docx/IJ8sdKsi1o1OKwxgbWNcU3ZjnCh)

> 集智俱乐部｜[阅读原文](https://mp.weixin.qq.com/s/eNhZIzXWmSvSyIbt-qDXcQ)

> 转载请联系原作者取得授权

![](img/607203dcd7c3acd1f0d1731bacd85862.png)

> **导语**

> **跨尺度**[**、跨层次的涌现是复杂**](http://mp.weixin.qq.com/s?__biz=MzIzMjQyNzQ5MA==&mid=2247687960&idx=1&sn=97d99347590e621c2fbe1f9f647e929f&chksm=e8994595dfeecc83856435bc9906db11db930f6397fa75dbf031970fc9daa4696f1211059ef2&scene=21#wechat_redirect)**系统研究的关键问题，大脑是一个典型的多尺度复杂系统，而意识作为一种涌现现象更是被广泛提及。因此，包括整合信息论、因果涌现理论**、整合信息分解等各种量化涌现的理论都将大脑作为重要研究对象。然而，不同的因果涌现框架在应用到脑领域时都存在问题。在集智俱乐部 「因果涌现」读书会 ，约翰·霍普金斯大学研究生荣英淇梳理了在脑科学领域与因果涌现强相关的学者谱系，探讨了因果涌现理论在应用于脑网络时取得的一系列最新研究成果和面临的诸多挑战。虽然意识的大统一理论可能非常遥远，但希望和对这个方向感兴趣的朋友们深入交流，一起迎接挑战！

>

> **研究领域：因果涌现，信息分解，信息动力**学，脑科学，意识理论，向下因果，因果解耦

![](img/d48acaa752453a69db399b5d494d62d5.png)

荣英淇 **| 作者 **

王志鹏，杨明哲 **| 审校**

梁金 ** | 编辑**

在因果涌现领域，其中比较具有代表性的两个方向分别是：Erik Hoel 及其**同事结合干预主****义因果 （in**terventionist causality） 和有效信息提出的 因果涌现框架 [1-2]，和 Fernando Rosas 及其同事结合转移熵和 整合信息分解 对因果涌现进行了再定义[3-4]。关于具体入门，因果涌现社区也有非常好的介绍，比如张江老师撰写的因果涌现入门路径：https://pattern.swarma.org/article/296。

但依旧可能有许多对因果涌现乃至涌现这一概念保持质疑的朋友会发问，其应用点在哪里？如果涌现这一大问题能够解决，那么我们应该期待许多子问题或小问题也能被阐述得更清楚。本文的目的并非试图去打消质疑，而是作为一名 同样 感到困惑挣扎想探索方向的学生，和大家分享两位因果涌现方向的年轻学者使用信息分解工具在大脑这一应用场景中的相关工作。同样也有其他整理珠玉在前，参看路径“脑神经系统中的因果涌现”：https://pattern.swarma.org/article/253。

其中 Luppi 及其同事主要是希望借助信息分解工具进一步认识意识的机理，目前研究较为问题导向[5-6]；而Varley及其同事的研究集中于信息分解框架本身的扩展，目前研究较为工具盒导向[7-9]。所以，前者近期发表的综述可能会在大的应用场景上有更清晰的阐述，而后者会在具体的信息论指标上有所发展。

这里我们希望通过对大的科学故事和具体的信息论指标的讨论，来一同探寻是否能从中获得启发 （为什么脑领域和因果涌现关联大？可见下图或《 脑与涌现的联系：从协同核到因果涌现 》） ，推敲因果涌现理论应用于脑领域的可能问题，比如：

（1）对于Hoel的框架，确定性和简并性在脑网络里意味着什么？ （《 量化因果涌现表明：宏观可以战胜微观 》）

（2）对于Rosas的框架，如何解决或回避信息分解本身的计算复杂度？哪些更为简要的信息指标能对脑网络进行衡量？ （《 量化涌现：信息论方法识别多变量数据中的因果涌现 》）

（3）对[于NIS及其扩展框架，通过结合](http://mp.weixin.qq.com/s?__biz=MzIzMjQyNzQ5MA==&mid=2247673437&idx=1&sn=9abb63e99ad526e24d3b52487a3910d1&chksm=e8997cd0dfeef5c686ba16dd3395440a15e77515a72b10d453e0cf94e77f4c2b425354e1a099&scene=21#wechat_redirect)神经网络科学的知识，能否进一步改善对脑网络粗粒化和动力学拟合的策略？ （《 什么是涌现？人工智能给你答案 》）

![](img/d73da2a390146a7d633969d977530daa.png)

图1. 脑领域里与因果涌现强相关的部分学者

### **脑领域与因果涌现强相关的学者**

相比较于其他复杂系统，因果涌现理论在其发展过程中自然地与脑科学紧密相关。

**Erik Hoel **及其同事这批来自于整合信息论 （Integrated Information Theory, 一种意识理论） 团队的研究者结合干预主义因果和有效信息，定义了最开始的因果涌现框架 （《 全网最全总结！因果涌现核心指标“有效信息” 》） 。当宏观上的动力学相对微观上的动力学展现出更强的因果效应 （确定性减去简并性） ，我们认为因果涌现发生了。随着 Hoel 淡出学术界，现在整合信息论组的研究者如 Larrisa Albantakis 会更喜欢讨论因果结构 （causal structure） 而非因果涌现的概念，表达更内敛，但思想上的一致性还是比较高的[10]。

**Fernando Rosas **及其同事结合转移熵和整合信息分解对因果涌现进行了再定义，将其分为了因果解耦和向下因果两部分，这方面更贴近哲学上的强涌现[11]。因果解耦意味着作为整体的系统对未来作为整体的系统进行影响，向下因果意味着作为整体的系统对未来系统内部的子系统进行影响。Rosas 及其同事大部分同样是参与脑意识领域的研究，且受到了整合信息论的影响。

Thomas F. Varley 及其同事结合局部信息 （local information） 拓展了因果涌现框架[12]，且结合信息分解框架去剖析涌现现象是什么[13] （《 从微观到宏观：涌现的实质是信息转换？ 》） 。在有意地控制系统从微观到宏观，其在不同尺度下整体的互信息没有损失，但是如果对互信息本身进行信息分解的话，会发现在宏观尺度上有更多的协同偏好 （协同：系统两个及以上变量作为整体所产生的影响） 。其对因果涌现的猜想是，从微观到宏观，因果涌现的实质是因果无关的信息被转换成因果相关信息，但有待验证和确定的是，什么是因果相关信息？什么又不是？

Andrea I. Luppi 及其同事则是会和因果涌现距离相对远一点，其本人只作为合著者参与过相关工作。但他作为研究意识的学者，通过整合信息分解这一因果涌现强相关工具在脑数据中得到研究成果[14]，并能够很好地整理信息分解如何应用在脑领域并和意识问题关联起来[15]。

其他在因果涌现相关领域做出较大贡献的脑科学工作者包括但不限于：

+   Giulio Tononi 和 Olaf Sporns 提出有效信息 （effective information） [2]；

+   Dani Bassettt 讨论脑的复杂性 （complexity） 和涌现[16]，近期和其同事还有一篇工作通过机器学习做信息分解[17]；

+   Anil Seth 提出的通过格兰杰因果去量化涌现[18]；

+   Pedro A.M. Mediano 和其同事提出的整合信息分解框架 （《 量化涌现：信息论方法识别多变量数据中的因果涌现 》） ；

+   具体技术细节可以详见因果涌现社区产出的综述[19] 《 集智科学研究中心重磅综述：复杂系统中的因果和涌现 》 ，不局限于脑科学领域对因果涌现理论的阐述，并且加入了因果涌现如何与机器学习关联起来的论述。

考虑到 Hoel 本人计划出走学界成为科普作家，Rosas 研究兴趣广泛，在本文中，我们选取Varley及其同事以及Luppi及其同事的相关工作进行讨论。这两位都是脑科学背景出身，前者会使用信息分解和一些扩展的指标去探索大脑的信息整合，工具盒较为丰富，后者比较青睐整合信息分解这个单一工具，但是在具体的意识问题上有更多不同的方向性尝试，而不是将信息本身作为最主要的工作。

### **因果涌现理论在脑网络中的应用**

在正式进入具体论文前，让我们先考虑问题：现有因果涌现理论在脑网络中有哪些应用？存在哪些问题？

##### **NIS 及 NIS+ 框架 **

对 Hoel 等人的框架来言，在脑领域里，除却整合信息、因果结构的相关工作，其余探索有待更深入的文献调研。此处介绍的是基于 Hoel 的因果涌现哲学，引入机器学习技术所拓展出来的 NIS 及 NIS+ 框架的结果 （见图2） [20-21]。其框架输入是两组不同的数据，分别是人在看无意义的电影 （图片剪接） 或者人没有执行具体任务的核磁共振数据 （通过 Schaefer Atlas） 降维到100所得到的微观数据 （在神经动力学领域肯定是属于宏观动力学那一类，但计算条件所限，NIS+目前还没有能力处理维度特别高的数据，所以各个实验情况，微观数据的维度一般不会高于三位数） 。以100维的微观数据为基准，可以搜索比100维更小的尺度，在论文中搜索的是52，27，14，7，3，1这几个尺度作为宏观尺度，看在什么尺度下因果涌现的发生最为显著。

当我们将两组不同的数据输入NIS+训练完成对粗粒化策略的选取和动力学的拟合，对于NIS+来说，此处的核心目的是平衡最小化对微观数据的重构误差 （避免学习到无意义的宏观动力学） 和最大化每个尺度的有效信息 （通过反向动力学的引入） 。最后发现， 对于人看电影的核磁共振数据，在宏观尺度为1的时候因果涌现效应最大，且通过归因方法分析会发现这个尺度和视觉系统高度相关。对于人的静息态核磁共振数据，会发现在宏观尺度为7的时候因果效应最大，虽然根据数据本身的异质性有不一样的结果 （但比较可惜的是无法和Yeo Atlas的7个子系统一一对应起来） 。

如果去追踪NIS+的论文，会发现脑实验的结果相较于其他仿真实验结果是比较初步的 （SIR，鸟群以及生命游戏） ，比如尺度的可解释性，实验者该如何解释确定性和简并性这两大有效信息的基本组件对脑功能认知的意义？和协同核的联系该如何进一步阐述？[22]比如动力学的拟合问题，这方面已经有很多相关工作和先验知识，能否将其有效地嵌入机器学习框架？

![](img/15fb6abdfc445783cf5d600ec93794fb.png)

图2. NIS+基本框架

不同于 Hoel 等人需要对数据进行do操作来实现严格的干预主义因果，Rosas 等人对因果的定义是通过格兰杰因果（转移熵）来量化涌现。 在《 量化涌现：信息论方法识别多变量数据中的因果涌现 》和《 脑与涌现的联系：从协同核到因果涌现 》里已经有了相对细节的描述，这里主要指出其在脑实验中的应用与Hoel 等人框架上的主要不同 （见图3） 。

**（1）Ro**sas 等人认为因果涌**现的发生分**为两类， 因果解耦 意味着系统多个元素作为整体会影响系统多个元素作为整体 （协同影响协同） ， 向下因果 意味系统多个元素作为整体会影响系统的任一单一元素。而 Hoel 等人的框架里，更多是考虑不同尺度下对动力学本身的因果效应衡量，对搜寻最佳的粗粒化策略来讲，可能会考虑到系统以怎么样的形式交互，但更多的联系需要进一步讨论 ；

（2）Rosas 在脑实验中的宏观变量是跟行为数据直接相关的，比如，猕猴的腕部移动。而对于Hoel或NIS以及NIS+的扩展框架，宏观变量都是通过微观变量粗粒化得来的；

（3）如果将NIS+实验中所使用的微观数据以及粗粒化所得到的宏观数据输入 Rosas 等人的框架，会发现结构会显示出比较强的宏观偏好，意味着宏观尺度如果越小，其框架更容易判定因果涌现发生了，且其框架无法区分看电影的核磁共振功能数据以及静息态核磁功能数据的区别。目前不成熟的推测可能还是因为信息分解本身因为计算复杂度过高 （超指数增长） ，Rosas及其同事设计了许多替代指标，但这些替代指标本身没有消除尺度的影响。

接下来的文章，会带着这些目前因果涌现应用中存在的问题进行解读和思考。

![](img/c188e2eecf07c8f20abd980595b9682b.png)

图3. Rosas 等人的实验结果

### Andrea I. Luppi及其同事对信息分解及其架构的讨 论[5]

在Luppi的文章里 （《 大脑如何分解信息？信息分解帮助理解生物和人工认知系统 》） ，信息动力学 （信息流） 和信息分解都可以帮我们更好地理解大脑如何处理信息。前者可以解释信息在大脑里是如何传递和流动，而后者则和鲁棒性以及大脑如何整合信息有关。如，鲁棒性让一只眼睛受伤的情况下依然看到下图蓝色的部分，而两只眼睛信息的整合可以帮我们形成3D视觉 （下图红色的部分） 。关于这两部分内容更深入的讨论，可见推文分享：

> 悉尼大学 Joseph Lizier：信息动力学如何帮助理解大脑？

> 新信息论：从分解到整合｜因果涌现读书会第四季启动

![](img/39f4ecb2ccaad204ea9b933193a1a685.png)

图4. 信息流与信息分解

鲁棒性和信息分解[框架中的冗余息息相关](http://mp.weixin.qq.com/s?__biz=MzIzMjQyNzQ5MA==&mid=2247679970&idx=1&sn=073059daf85fc50394fee33935a932b6&chksm=e899656fdfeeec792019d7665f01a5862a1d8bf2a29522006691ed03152a0410c7022d1993a1&scene=21#wechat_redirect)，信息整合的需求则和协同相关。信息分解的基础框架在集智的其他推文里以及有很细节的阐述 （见图5，《 互信息的“微积分” 》）

![](img/916b355b5ed040189496a6d3f6efe7eb.png)

图5. 部分信息分解（PID）

如果我们将信息分解的框架嵌入进信息动力学中，可以发现对一些传统的信息流指标能够进行有趣的拆解 （见图6） 。

![](img/fbed7c77dfe48cd88f1cbdac377286b7.png)

图6. 信息分解对传统信息指标的拆分

在此处对于帮助定义 Rosas 等人因果涌现的整合信息分解框架 （见图7，《 从分解到整合：整合信息分解框架 》） ，我们得意识到我们需要同时讨论信息动力学以及信息分解。结合信息动力学的基本概念以及信息分解来拆解，得到了向下因果和因果解耦 （Syn->Syn） 那个点。

> 参看因果涌现读书会第四季的分享

> [整合信息分解框架 ](http://mp.weixin.qq.com/s?__biz=MzIzMjQyNzQ5MA==&mid=2247681427&idx=1&sn=7c274a4e72f4d1decf9ee64cce17f810&chksm=e8995f1edfeed60818c515ab9c48351915d5ec389745c167dad4aab8b5450f8e3bf72ae08a9f&scene=21#wechat_redirect)：https://pattern.swarma.org/study_group_issue/606

![](img/3a171714cb3919377a13f594e017c35e.png)

图7. 整合信息分解

信息分解也可**以有效地嵌入对大脑信息整合能力的讨论中 （见图8） ，在**信息分解的视角下，信息整合可以被分为两个部分： 冗余意味着系统的各个元素以类似的方式进行响应，协同意味着系统的各个元素之间彼此交互得到新的信息 ，而特有信息是元素本身独立的那部分，不涉及整合 （此处存在一个公开话题，协同如果对应信息整合，冗余对应鲁棒性，那为什么 需要 特有信息？它对大脑认知功能的影响是什么？） 。

关于支撑信息的具体元素能否被识别 （differentiation） ，也可以发现谁在提供冗**余信息是相对难识别的，因为当你破坏了提供冗余信息的其中一个元**素时，其他的元素也在提供同种信息，但协同信息和特有信息在单一元素的损失中也一同损失掉了。 从信息本身所存在的位置来讲，特有信息和冗余信息都是存在于元素本身的，而协同信息必须在交互中才能产生 。

![](img/2dba822e2ae9cf287616d9724114901f.png)

图8. 信息整合的三个维度

信息分解 （对Luppi 等人来说，主要是整合信息分解） 在脑领域已经有所应用。比如，与感知动作功能相关的脑区是高冗余的，与高级认知功能相关的脑区是高协同的；处于网络模块内部的节点是高冗余的，具有较高的全局链接度的脑区是高协同的；脑的冗余结构和其本身的结构链接是类似的，而高协同的区域往往有更高的突触密度。

在进化尺度下，从猴进化到人，会发现**更**大的协同结构进化出来了，因此其猜想， 对于任何高认知系统，首先需要强冗余的系统确保对感知动作的输入输出是准确的，然后高协同的区域在这些冗余架构  （scaffold）  之间进化出来去支撑更复杂的功能 。

![](img/aff7836fda25c7d248f04304d6f671a8.png)

图9. 信息分解对脑研究的启示

一个对这猜想可能的验证来自于Luppi与Proca等人合作的使用信息分解框架分析 人工神经网络 的工作[24]。在这篇文章中，作者们猜想如果人工神经网络需要学习更多的任务，就应该期待神经元之间有更高的协同 （这里是逐层分析） 。在这里简要介绍其中一个最简单的实验 （见图10） 。

存在两个任务，输入是一个二维的两元数组，一个任务不需要两个维度进行交互 （COPY） ，只是单纯的把第一个维度上的0或1值输出，而另一个任务是个高协同任务 （XOR） ，需要两个维度彼此交互进行输出 （这两个任务是两个很简单的极端案例，可以解释我们为什么需要信息分解框架。在因果结构的讨论中也有所涉及） 。

如果在训练过程中将dropout率提高，两个任务中神经网络神经元之间的冗余都得到了显著提高，因为协同和特有信息都会被dropout这个操作给打乱，所以需要将重要的信息存储到更多的地方。如果在训练过程完成后随机破坏（lesion）神经元，会发现如果我们破坏的是那些和其他神经元有更高协同的神经元，其高协同任务的表现相较于破坏低协同的神经元会出现显著下降。

![](img/55f645f86961fc04f0135dd395c4f617.png)

图10. 信息分解在人工神经网络的实验

信息**动力学及信息分**解虽然能作为很好的工具，让我们对大脑作为信息处理系统有更多的理解，但有一个关键的问题目前被回避掉了：我们知道信息传播的结果，但是不知道信息是通过哪种物理途径进行传播的。这就是 网络沟通模型 （network communication models） 可能会启发下一步有关信息分解在脑领域应用的原因。比如，是否因为不同信号在对撞节点的相遇导致了协同效应的发生，对于重要信息需要通过多途径广播 （broadcast）， 所以会存在高冗余。

### Andrea I. Luppi及其同事对意识问题的综述[6]

围绕意识这个 问题， 在Luppi 等人的综述里提出了我们需要针对时间尺度、空间尺度以及信息尺度进行分解 （decompose） 来细致拆分。这里分解的概念涵盖可能有两类：一是对数据的降维，直接通过降维手段例如 PCA 保留关键维度，二是对数据的再表征，拆分成更细致的组成结构进行分析。

![](img/9192fc748b83a535c7d39b59910a5455.png)

图11. 三种层面上的分解

+   **时间尺度分解**

提供一个 **时间尺度分解 **的简单案例，将一个长时间轴的脑数据分成很多个短的脑数据，然后根据每个短的时间轴计算功能链接 （见图12） 。我们会发现，在无意识活动时，那些与结构链接更类似的功能链接会更频繁的出现，而与结构链接不相像的功能链接会变得更低频。

![](img/6333f57807708651068c9f74c83bc5e2.png)

图12. 案例：时间上的分解

+   **空间尺度分解 **

空间尺度的分解则更为复杂，可以在功能上进行分解，也可以在空间 （结构） 上进行分解。可以发现当意识状态被不同的的方法改变时 （比如意识紊乱（ Disorder of Consciousness ）和服用致幻剂（ Psilocybine ）） ，从功能分解去理解，会发现两者都是 awareness 受到影响[25]，但是从结构功能耦合 （structure-function coupling） 的角度来看，前者可以被大脑本身结构更好的预测，而后者依赖一些与大脑结构不类似的部分[26]。

![](img/39677f421bf2304d3f8b5afe2109c682.png)

图13. 案例：空间上的分解

+   **信息尺度分解**

**信息尺度分解 **则和信息分解这一框架紧密相关。信息处理中，冗余信息提供系统的鲁棒性，而协同信息可以增进系统的效率。具体在意识这领域的应用，在计算整合信息中，会发现之前的Φ WMS 多减去了一份冗余，而causal density多增加了一份协同 （见图14，具体定义可见[27]） 。所以在[15]中，新的指标 Φ R 便被设计出来去发现过去指标所不能区分的有无意识状态。目前还存在的一个公开话题是，对信息在一个特定的时间或空间尺度进行分解会不会有意义。

![](img/485a5f4b6abdc70064fab6b4e46159d8.png)

图14. 案例：信息上的分解

### Thomas F. Varley及其同事对信息论指标的讨论

因为信息分解的计算复杂度，所以**在实际应用中研究者可能会采取**一些更简要的指标，比如Varley及其同事常会讨论的 total** correlation 去衡**量信息多大程度被整合， O-information 去衡量系统是冗余主导 （大于0） 还是协同主导 （小于0） ， TSE Complexity 去衡量系统的整合与隔离平衡。在[8]中，主要是对 O-information 的使用。在[9]中，对于 O-Information 有类似于将 Φ WMS 转化成 Φ R 的操作，通过信息分解解读公式是否合理地衡量了我们关心的系统性质，然后进行操作改变。

信息分解的框架也会有所不同，比如PID是需要我们去定义什么是目标才能做信息分解，但PED框架是不需要的[28]，GI[D框架是Varley近期对信息分解框](http://mp.weixin.qq.com/s?__biz=MzIzMjQyNzQ5MA==&mid=2247685665&idx=1&sn=bbc5099136ec68c57a9d30bdb1edc66d&chksm=e8994cacdfeec5bab112b4254372a06634e732edf8384c43e97c125d716498876ed1fe86cffd&scene=21#wechat_redirect)架的 拓展，针对KL散度进行信息分解[7]。关于具体这些指标的使用结果，可以回顾《 复杂系统研究者 Thomas Varley 做客集智读书会：信息、脑与网络 》。另外，在「因果涌现读书会」第四季吕奥博等人在信息分解的计算框架下也有进一步地拓展。

![](img/a185b5cba391da59bc7224c12e4fc821.png)

图15. Varley常讨论的指标

### **展望**

脑作为多尺度的复杂系统[29]，且因为意识问题的存在，是因果涌现理论的天然适用场景 （见图16） 。但不同的因果涌现框架在应用到脑领域都 存在 问题。整合信息论、因果结构或者因果涌现 （Hoel） 是否真的能解释意识问题也面临许多理论或实验上的挑战，详见：

> 《 吵翻了：意识是如何产生的？科学家能否达成一致？ 》

> 《 万字长文：意识的大一统理论要来了吗？ 》

虽然 信息分解乃至整合信息分解 （Rosas） 在脑领域已经有很丰富的产出，但不可回避的是，目前应用忽略了底层的数学性质，并通过技巧回避了计算复杂度的问题 （多次取样少量元素而不是去计算多数元素的信息原子） 。引入机器学习的操作让人兴奋，但神经动力学本身的拟合是个大问题，并且也对可解释性带来了更大的挑战。但依旧不可否认的是，理论可能是错的，其想探索的问题却是真的，而这已经足够让人兴奋。

![](img/81eb0ae3a25b0e39e0eca9d44a756ec5.png)

图16. 总结

**参考文献**

1. Hoel EP, Albantakis L, Tononi G. Quantifying causal emergence shows that macro can beat micro. Proc Natl Acad Sci U S A. 2013 Dec 3;110(49):19790-5. doi: 10.1073/pnas.1314922110. Epub 2013 Nov 18. PMID: 24248356; PMCID: PMC3856819.

2. Tononi, G., Sporns, O. Measuring information integration. BMC Neurosci 4, 31 (2003). https://doi.org/10.1186/1471-2202-4-31

3. Rosas, Fernando E., et al. "Reconciling emergence: An information-theoretic approach to identify causal emergence in multivariate data." PLoS computational biology 16.12 (2020): e1008289.

4. Mediano PAM, Rosas FE, Luppi AI, Jensen HJ, Seth AK, Barrett AB, Carhart-Harris RL, Bor D. 2022 Greater than the parts: a review of the information decomposition approach to causal emergence. Phil. Trans. R. Soc. A 380: 20210246. https://doi.org/10.1098/rsta.2021.024

5. Luppi, A. I., Rosas, F. E., Mediano, P. A. M., Menon, D. K., & Stamatakis, E. A. (2024). Information decomposition and the informational architecture of the brain. Trends in Cognitive Sciences, 28(4), 352–368. https://doi.org/10.1016/j.tics.2023.11.005

6. Luppi, A. I., Rosas, F. E., Mediano, P. A. M., Demertzi, A., Menon, D. K., & Stamatakis, E. A. (2024). Unravelling consciousness and brain function through the lens of time, space, and information. Trends in Neurosciences, 47(7), 551-568. https://doi.org/10.1016/j.tins.2024.05.007

7. Varley, T.F., Pope, M., Faskowitz, J. et al. Multivariate information theory uncovers synergistic subsystems of the human cerebral cortex. Commun Biol 6, 451 (2023). https://doi.org/10.1038/s42003-023-04843-w

8. Varley, T. F., Pope, M., Puxeddu, M. G., Faskowitz, J., & Sporns, O. (2023). Partial entropy decomposition reveals higher-order information structures in human brain activity. Proceedings of the National Academy of Sciences, 120(30), e2300888120. https://doi.org/10.1073/pnas.2300888120

9.. Varley, T. F. (2024). Generalized decomposition of multivariate information. PLOS ONE, 19(2), e0297128. https://doi.org/10.1371/journal.pone.0297128

10. Grasso, M., Albantakis, L., Lang, J.P. et al. Causal reductionism and causal structures. Nat Neurosci 24, 1348–1355 (2021). https://doi.org/10.1038/s41593-021-00911-8

11. Bedau, M. A. (1997). Weak Emergence. Philosophical Perspectives, 11, 375–399. http://www.jstor.org/stable/2216138

12. Varley, T.F. Flickering Emergences: The Question of Locality in Information-Theoretic Approaches to Emergence. Entropy 2023, 25, 54. https://doi.org/10.3390/e25010054

13. Varley, Thomas F., and Erik Hoel. "Emergence as the conversion of information: a unifying theory." Philosophical Transactions of the Royal Society A 380.2227 (2022): 20210150.

14. . P.A.M. Mediano, et al. Towards an extended taxonomy of information dynamics via integrated information decomposition. arXiv (2021). http://doi.org/10.48550/arxiv.2109.13186

15. Luppi, A. I., Mediano, P. A. M., Rosas, F. E., Allanson, J., Pickard, J. D., Carhart-Harris, R., Williams, G. B., Craig, M. M., Finoia, P., Owen, A. M., Naci, L., Menon, D. K., Bor, D., & Stamatakis, E. A. (2024). A synergistic workspace for human consciousness revealed by integrated information decomposition. *eLife, 88173. https://doi.org/10.7554/eLife.88173

16. Bassett DS, Gazzaniga MS. Understanding complexity in the human brain. Trends Cogn Sci. 2011 May;15(5):200-9. doi: 10.1016/j.tics.2011.03.006. Epub 2011 Apr 14. PMID: 21497128; PMCID: PMC3170818.

17. Murphy, K. A., & Bassett, D. S. (2024). Information decomposition in complex systems via machine learning. Proceedings of the National Academy of Sciences, 121(13).https://doi.org/10.1073/pnas.2312988121

18. Seth, A. K. (2008, August). Measuring emergence via nonlinear Granger causality. In alife (Vol. 2008, pp. 545-552).

19. Yuan, B., Zhang, J., Lyu, A., Wu, J., Wang, Z., Yang, M., Liu, K., Mou, M., & Cui, P. (2024). Emergence and Causality in Complex Systems: A Survey of Causal Emergence and Related Quantitative Studies. Entropy, 26(1), 108. https://doi.org/10.3390/e26020108

20. Zhang, Jiang & Liu, Kaiwei. (2022). Neural Information Squeezer for Causal Emergence. Entropy. 2023. 10.3390/e25010026.

21. Yang, Mingzhe, et al. "Finding emergence in data: causal emergence inspired dynamics learning." arXiv preprint arXiv:2308.09952 (2023).

22. Luppi AI, Mediano PAM, Rosas FE, Holland N, Fryer TD, O'Brien JT, Rowe JB, Menon DK, Bor D, Stamatakis EA. A synergistic core for human brain evolution and cognition. Nat Neurosci. 2022 Jun;25(6):771-782. doi: 10.1038/s41593-022-01070-0. Epub 2022 May 26. PMID: 35618951; PMCID: PMC7614771.

23. Paul L. Williams,Randall D. Beer: Nonnegative Decomposition of Multivariate Information ,arXiv:1004.2515,2010

24. Proca, A. M., Rosas, F. E., Luppi, A. I., Bor, D., Crosby, M., & Mediano, P. A. M. (2024). Synergistic information supports modality integration and flexible learning in neural networks solving multiple tasks. PLoS Computational Biology, 20(6), e1012178.https://doi.org/10.1371/journal.pcbi.1012178

25. Huang, Z., Mashour, G. A., & Hudetz, A. G. (2023). Functional geometry of the cortex encodes dimensions of consciousness. Nature Communications, 14, 72.https://doi.org/10.1038/s41467-022-35764-7

26. Luppi, A. I., Vohryzek, J., Kringelbach, M. L., Mediano, P. A. M., Craig, M. M., Adapa, R., ... & Stamatakis, E. A. (2023). Distributed harmonic patterns of structure-function dependence orchestrate human consciousness. Communications Biology, 6, 117.https://doi.org/10.1038/s42003-023-04474-1

27. Seth, A. K., Barrett, A. B., & Barnett, L. (2011). Causal density and integrated information as measures of conscious level. Philosophical Transactions of the Royal Society A: Mathematical, Physical and Engineering Sciences, 369(1942), 3748–3767.https://doi.org/10.1098/rsta.2011.0079

28. Ince, R.A.A. (2017) The partial entropy decomposition: decomposing multivariate entropy and mutual information via pointwise common surprisal. arXiv, Published online February 20, 2017. https://doi.org/10.48550/arXiv.1702.01591

29. Betzel, R. F., & Bassett, D. S. (2017). Multi-scale brain networks. NeuroImage, 160, 73–83.https://doi.org/10.1016/j.neuroimage.2016.11.006